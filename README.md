# ~/tblog

Personal blog built with Jekyll and the [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) theme. The site is published with GitHub Pages at <https://shivanshu-semwal.github.io/blog/>.

## Build and preview

Install Ruby 3.3 and Bundler, then run:

```sh
bundle install
JEKYLL_ENV=production bundle exec jekyll build
```

The generated site is written to `_site/`. For a local preview at the server root, run:

```sh
bundle exec jekyll serve --baseurl ""
```

Then open <http://127.0.0.1:4000/>. The local preview omits the production `/blog` prefix.

## Posts and media

- Blog posts live in `_posts/` and use the `YYYY-MM-DD-title.md` filename format.
- Navigation pages live in `_tabs/`.
- Post-specific images and covers belong in `assets/img/posts/<post-slug>/`. Reference them from Markdown with a root-relative path, for example `![Diagram](/assets/img/posts/my-post/diagram.png)`. Chirpy adds the configured `/blog` base path when rendering the image.
- Site-wide images live directly under `assets/img/`; the sidebar avatar is `assets/img/avatar.png`.
- Other downloadable files can be stored in `assets/files/<post-slug>/` and linked with Jekyll’s `relative_url` filter, for example `[Download PDF]({{ '/assets/files/my-post/example.pdf' | relative_url }})`.

The configured post permalink format is `/:year/:month/:day/:title.html`; keep existing filenames and front matter consistent when editing older posts.

## Deployment

`.github/workflows/pages-deploy.yml` builds the site and deploys it to GitHub Pages on pushes to `master`; it can also be started manually with `workflow_dispatch`. Set the repository’s Pages source to **GitHub Actions**. The production URL is `https://shivanshu-semwal.github.io` with `baseurl: /blog`, since the account-root Pages site hosts a different site.
