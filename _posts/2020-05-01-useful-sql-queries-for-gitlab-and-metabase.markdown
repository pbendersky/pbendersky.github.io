---
title: "Useful SQL queries for integrating GitLab in Metabase"
date: 2020-05-01T18:25:00-03:00
---
Recently I started playing with [Metabase] to create interesting visualizations and have the teams quickly check how we are working.

One of the [main tools we use](https://blog.quadiontech.com/tools-we-use-2016-edition-d91a7ccf95a) is GitLab, so I needed the ability to query it. To be on the safe side, I configured the backup job to create a read-only mirror of the DB, and have Metabase access it.

However, as soon as I started trying to query it, I found that the table structure used in GitLab was difficult to use for quick visualizations: while the unit our users know is their project, projects reside in a namespace, and namespaces can be nested. Also, since GitLab supports forks, you may end up with several projects that you want to aggregate stats of in order to reflect accurate numbers.

To solve this, I created a few helper queries using [PostgreSQL CTEs](https://www.postgresql.org/docs/9.1/queries-with.html)[^CTE].

Here's a dump of the queries I found useful, and some brief explanation of what they do.

{% highlight SQL %}

-- flat_namespaces - for a leaf_id, obtain the full name of the namespace where it is
WITH RECURSIVE flat_namespaces AS (
    SELECT CONCAT(name) AS names, id, parent_id, id AS leaf_id
    FROM namespaces
    WHERE parent_id IS NULL
UNION ALL
    SELECT CONCAT(flat_namespaces.names, ' / ', namespaces.name) AS names, namespaces.id, namespaces.parent_id, flat_namespaces.leaf_id
    FROM namespaces
    INNER JOIN flat_namespaces ON namespaces.id = flat_namespaces.parent_id
)
-- root_projects - searches for the root project based on a candidate fork id
, root_projects(root_project_id, fork_project_id) AS (
    ( -- project with no forks
        SELECT id AS root_project_id, id AS fork_project_id
        FROM projects
        WHERE NOT EXISTS (
            SELECT 1
            FROM fork_network_members
            WHERE (forked_from_project_id = projects.id OR project_id = projects.id)
        )
    )
    UNION
    ( -- project that has forks (parent)
        SELECT DISTINCT forked_from_project_id as root_project_id, forked_from_project_id as fork_project_id
        FROM fork_network_members
    )
    UNION
    ( -- project that is a fork
        SELECT DISTINCT forked_from_project_id AS root_project_id, project_id AS fork_project_id
        FROM fork_network_members
        WHERE forked_from_project_id IS NOT NULL
    )
),
project_with_full_namespace(full_name, project_id) AS (
    SELECT CONCAT(flat_namespaces.names, ' / ', projects.name), root_projects.fork_project_id AS project_id
    FROM root_projects
    INNER JOIN projects ON projects.id = root_projects.root_project_id
    INNER JOIN flat_namespaces ON projects.namespace_id = flat_namespaces.leaf_id
)

SELECT DISTINCT *
FROM project_with_full_namespace

{% endhighlight %}

Let's start from the top:

`flat_namespaces` will return a table containing the concatenated names, the parent namespace id, and the leaf namespace id.

So, if you have a namespace at `/a/b/c`, where the IDs are 1, 2, and 3 respectively, and you query it with a `WHERE` clause of `leaf_id = 3`, you'll get
`a/b/c` as the `names` value.

`root_projects` will return a pair of `root_project_id` and `fork_project_id`. This takes into account the three different possibilities that might exists:

- Projects with no forks.
- Projects with forks.
- Forks of other projects.

Basically, with a `fork_project_id` you can get the corresponding `root_project_id`. If you couple this with `flat_namespaces`, for any given project ID on your GitLab installation, you can grab the `root_project_id` with the "full path" of namespaces as a string.

Finally, `project_with_full_namespace` uses the previously defined CTEs to create the "full name" of the project and all its parent namespaces, similar to what you see in GitLab's URLs. It's important to note that this query will ignore forks, as it uses `root_projects` to navigate the fork tree, but this is what we needed for our dashboards.

[Metabase]: https://www.metabase.com
[^CTE]: Common Table Structure