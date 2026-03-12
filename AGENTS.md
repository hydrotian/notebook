# Repository Guidelines

## Project Structure & Module Organization
This repository is a Jekyll site for `notebook.simhydro.com`.

- `_posts/`: published posts (`YYYY-MM-DD-title.md`)
- `_layouts/`, `_includes/`, `_data/`: theme/layout building blocks and site data
- `_sass/` and `assets/css/`: Sass sources and compiled stylesheet entrypoint
- `assets/js/`: site JavaScript and search/lightbox scripts
- `images/`: static image assets used in posts/pages
- `interactive/`: standalone interactive pages (prebuilt static bundles)
- `_config.yml`: global site configuration

## Build, Test, and Development Commands
Run commands from the repository root.

- `bundle install`: install Ruby/Jekyll dependencies
- `bundle exec jekyll serve`: local dev server with live reload
- `bundle exec jekyll serve --drafts`: preview content from `_drafts/`
- `bundle exec jekyll build`: production-style build into `_site/`

Use `bundle exec` to ensure the project gem versions are used.

## Coding Style & Naming Conventions
- Markdown posts: include YAML front matter (`layout`, `title`, `date`, `categories`, `tags`).
- Post filenames must follow `YYYY-MM-DD-title.md`.
- Use 2-space indentation in YAML, HTML, JS, and Sass files.
- Keep keys/paths consistent with existing conventions (e.g., `_data/navigation.yml` entries use `title` + `url`).
- Prefer editing source files (`_sass/`, `assets/js/main.js`) rather than minified artifacts unless intentionally updating built output.

## Testing Guidelines
There is no formal automated test suite in this repo. Treat the following as required checks before opening a PR:

- `bundle exec jekyll build` must complete without errors.
- Run `bundle exec jekyll serve` and manually verify changed pages/posts.
- Confirm links, images, categories/tags, and search behavior for modified content.

## Commit & Pull Request Guidelines
Recent history favors short, imperative commit messages (e.g., `add a post`, `fix missing links`, `url updates`).

- Keep commits focused and descriptive.
- In PRs, include: purpose, changed paths, and validation steps run.
- Attach screenshots for visual/layout updates.
- Link related issues when applicable and note any config/domain changes (`_config.yml`, `CNAME`).

## Security & Configuration Tips
- Never commit secrets, API keys, or admin passwords.
- Review `_config.yml` and comment-system related includes carefully when changing site-wide behavior.
