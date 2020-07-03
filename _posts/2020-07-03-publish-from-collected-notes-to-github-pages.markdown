---
title: Publish from Collected Notes to GitHub Pages
date: 2020-07-03T15:13:38+00:00
---
# Publish from Collected Notes to GitHub Pages

My blog is a statically generated Jekyll site hosted on GitHub pages.

While I do use Collected Notes to post some short things, I haven’t migrated my blog there since it will break all my past permalinks. However, I do love the simplicity to both write and publish quickly from any device that Collected Notes gives me.

In order to keep the best of both worlds, I created a small GitHub Action, that I can trigger from my phone using Shortcuts, that will quickly import a note in Collected to a post in GitHub Pages.

You can check the code [here](https://github.com/pbendersky/pbendersky.github.io/blob/master/.github/workflows/import-from-cn.yml)

The important part is this shell script:

```bash

curl --location ${{ github.event.client_payload.url }}.json > note.json
date_prefix=`date +%Y-%m-%d`
markdown_name=${date_prefix}-`jq -r '.path' note.json`.markdown
title=`jq -r '.title' note.json`
post_path=$GITHUB_WORKSPACE/_posts/${markdown_name}
cat <<EOF > ${post_path}  
---
title: ${title}
date: `date --iso-8601=seconds`
---
`jq -r '.body' note.json`
EOF
```

In order to run it, you have to:

1. Create the GitHub Action.
2. Create a Personal Token in GitHub.
3. Import [this Shortcut](https://www.icloud.com/shortcuts/ddd6c18c623f4bda8fcb4ef59f184fe7) (or create your own!)
4. From your iOS device, share a note in CN and choose the Shortcut.
5. Check your new post in your blog!
