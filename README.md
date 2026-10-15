# sara-02.github.io

Personal website of Sarah Masud, built with the [al-folio](https://github.com/alshedivat/al-folio) Jekyll template (v1.2).

## Where things live

- `_pages/`: about, papers, background (incl. honors), coverage, service, talks, writings
- `_bibliography/papers.bib`: publications (shown on the papers page, grouped by year)
- `_data/venues.yml`: colours of the venue badges (Conference, Journal, …)
- `_data/socials.yml`: social icons on the about page
- `_data/talks.yml`: talks, tutorials, posters and open mics shown on the talks page (one entry per talk)
- `assets/img/`: profile picture and favicon
- `_config.yml`: site settings

## Writing posts

Posts live in `_posts/`, named `YYYY-MM-DD-short-title.md` (the date is the publish date). The writings page (`/writings/`) has four types:

- **Essays** (long-form): front matter `categories: essays`.
- **Notes** (short): front matter `categories: notes`.
- **Invited** (published elsewhere): front matter `categories: invited`, `publication: "Where it appeared"` and
  `redirect: <article URL>`. It is listed with the publication and year, and opens the original article.

- **Poems**: front matter `categories: poems`; for a poem hosted on WordPress add `publication: "The Messier (WordPress)"` and
  `redirect: <poem URL>`, and it opens the original.

The writings page lists all posts newest first, each tagged Essay, Note, Invited or Poem, with buttons to filter by type.
An optional `description` is shown under the title in the list.

Copy `_drafts/example-essay.md` or `_drafts/example-note.md` to start. Files in `_drafts/` are not published;
preview them with `bundle exec jekyll serve --drafts`.

## Preview locally

Requires Ruby 3.3 and ImageMagick.

```bash
bundle config set --local path vendor/bundle   # once
bundle install                                 # once, and after Gemfile changes
bundle exec jekyll serve --livereload
```

Open http://localhost:4000. Pages reload on save; restart the server after editing `_config.yml`.

Or with Docker: `docker compose up`, then open http://localhost:8080.

## Deploying

Pushing to `master` runs `.github/workflows/deploy.yml`, which builds the site and publishes it to the `gh-pages` branch.
GitHub Pages must be set to _Deploy from a branch → `gh-pages` / root_ (Settings → Pages).

For template documentation see the [al-folio docs](https://github.com/alshedivat/al-folio/tree/main/docs).
