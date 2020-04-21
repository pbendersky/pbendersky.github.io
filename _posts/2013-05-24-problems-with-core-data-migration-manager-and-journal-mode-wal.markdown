---
title: "Problems with Core Data Migration Manager and journal_mode WAL"
date: 2013-05-24 10:29
comments: true
categories: [Cocoa, CoreData]
---
Recently for a major update of [an app][Shopster] I'm working on, I had to use a Core Data Migration Manager
for the first time.

I run into a weird problem with some `pragma` statements set in the underlying SQLite database, so I'd like
to offer a workaround in case someone else runs into this issue in the future.

<!-- more -->

## The setup
Here's the setup of the Core Data stack in our app:

{% highlight objective_c %}
- (NSPersistentStoreCoordinator *)persistentStoreCoordinator
{
    if (_persistentStoreCoordinator != nil) {
        return _persistentStoreCoordinator;
    }
    
    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"CDMigration.sqlite"];
    
    NSError *error = nil;
    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:[self managedObjectModel]];
    
    NSDictionary *options = @{NSMigratePersistentStoresAutomaticallyOption:@YES,
                              NSInferMappingModelAutomaticallyOption:@YES,
                              NSSQLitePragmasOption: @{@"journal_mode": @"WAL"}
                              };
    
    if (![_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:options error:&error]) {
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
    }    
    
    return _persistentStoreCoordinator;
}
{% endhighlight %}

It's nothing fancy: the stock Core Data initialization you get with the Xcode template, plus the two options
that are commented out (`NSMigratePersistentStoresAutomaticallyOption` and
 `NSInferMappingModelAutomaticallyOption`) to enable migrations, and a setting to put SQLite in
 `journal_mode = WAL`.

I'm not really sure I need this `journal_mode` set, but since our app started using [MagicalRecord],
that's the way it was setup. Here's the relevant [SQLite documentation][SQLiteJournalModes] on journal
modes, so you can make an informed decision.

## The problem
When you use a [Migration Manager][MigrationManagersDocs], Core Data will create a new database for you,
and start copying the entities one by one from the old DB to the new one.

As we are using `journal_mode = WAL`, there's an additional file besides `DB.sqlite` called `DB.sqlite-wal`.

From what I can tell, the problem seems to be that Core Data creates a temporary DB, inserts everything
there, and when it renames it to the original name, the `-wal` file is kept as a leftover from the old
version. The problem is that you end up with an inconsistent DB.

## Workaround
As I'm not sure I want WAL enabled, I decided to keep the setting and implement a workaround. It's simple:

1. Detect if you need a migration.
2. If you do, set the `journal_mode` to `DELETE`.
3. Initialize the `NSPersistentStoreCoordinator`, so it triggers the migrations.
4. After a successful initialization, recreate the `NSPersistentStoreCoordinator` with `journal_mode` set to
`WAL`, as it was.

Here's how the code looks:

{% highlight objective_c %}
- (NSPersistentStoreCoordinator *)persistentStoreCoordinator
{
    if (_persistentStoreCoordinator != nil) {
        return _persistentStoreCoordinator;
    }
    
    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"CDMigration.sqlite"];
    
    NSError *error = nil;
    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel:[self managedObjectModel]];
    
    NSDictionary *options = @{NSMigratePersistentStoresAutomaticallyOption:@YES,
                              NSInferMappingModelAutomaticallyOption:@YES,
                              NSSQLitePragmasOption: @{@"journal_mode": @"WAL"}
                              };
    
    // Check if we need a migration
    NSDictionary *sourceMetadata = [NSPersistentStoreCoordinator metadataForPersistentStoreOfType:NSSQLiteStoreType URL:storeURL error:&error];
    NSManagedObjectModel *destinationModel = [_persistentStoreCoordinator managedObjectModel];
    BOOL isModelCompatible = (sourceMetadata == nil) || [destinationModel isConfiguration:nil compatibleWithStoreMetadata:sourceMetadata];
    if (! isModelCompatible) {
        // We need a migration, so we set the journal_mode to DELETE
        options = @{NSMigratePersistentStoresAutomaticallyOption:@YES,
                    NSInferMappingModelAutomaticallyOption:@YES,
                    NSSQLitePragmasOption: @{@"journal_mode": @"DELETE"}
                    };
    }

    NSPersistentStore *persistentStore = [_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:options error:&error];
    if (! persistentStore) {
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
    }    
    
    // Reinstate the WAL journal_mode
    if (! isModelCompatible) {
        [_persistentStoreCoordinator removePersistentStore:persistentStore error:NULL];
        options = @{NSMigratePersistentStoresAutomaticallyOption:@YES,
                    NSInferMappingModelAutomaticallyOption:@YES,
                    NSSQLitePragmasOption: @{@"journal_mode": @"WAL"}
                    };
        [_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:options error:&error];
    }

    
    return _persistentStoreCoordinator;
}
{% endhighlight %}

## Radar or GTFO[^RadarOrGTFO]
I've filed a [bug report to Apple on this issue][OpenRadarLink]. If you run into this as well, feel free
to duplicate it.

[Shopster]: http://click.linksynergy.com/fs-bin/stat?id=ekjqZxweDbw&offerid=146261&type=3&subid=0&tmpid=1826&RD_PARM1=https%253A%252F%252Fitunes.apple.com%252Fus%252Fapp%252Fshopster-geo-learning-groceries%252Fid613223118%253Fmt%253D8%2526uo%253D4%2526partnerId%253D30
[MagicalRecord]: https://github.com/magicalpanda/MagicalRecord
[SQLiteJournalModes]: http://www.sqlite.org/pragma.html#pragma_journal_mode
[MigrationManagersDocs]: http://developer.apple.com/library/ios/#documentation/cocoa/conceptual/CoreDataVersioning/Articles/vmLightweightMigration.html#//apple_ref/doc/uid/TP40004399-CH4-SW3
[OpenRadarLink]: http://openradar.appspot.com/radar?id=3031401
[^RadarOrGTFO]: http://blackpixel.com/blog/2012/02/radar-or-gtfo.html