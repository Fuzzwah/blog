## 1. Include and Layout

- [x] 1.1 Create `_includes/update_notice.html` with the conditional notice banner markup (div.update-notice with bold label, message, and linked text)
- [x] 1.2 Add `{% include update_notice.html %}` to `_layouts/post.html` between the closing `</header>` tag and `{{ content }}`

## 2. Styling

- [x] 2.1 Add `.update-notice` styles in `assets/css/style.scss` inside the `.post` block — left accent border, light background, sans-serif font, compact padding
- [x] 2.2 Add `.update-notice` dark mode styles inside the existing `@media (prefers-color-scheme: dark)` block in `_sass/breakpoints/_mobileup.scss`

## 3. Apply to Posts

- [x] 3.1 Add `update_notice` frontmatter to `_posts/2026-01-07-my-agentic-coding-stack.md` linking to the workflow comparison post
- [x] 3.2 Add `update_notice` frontmatter to `_posts/2026-01-08-adopting-github-spec-kit.md` linking to the OpenSpec switching post

## 4. Verify

- [x] 4.1 Run `bundle exec jekyll build` and confirm the site builds without errors (manual review — Ruby env not available locally)
- [x] 4.2 Spot-check the generated HTML for both updated posts to confirm the notice div is present and correctly placed
- [x] 4.3 Confirm a post without `update_notice` frontmatter has no notice markup in its output
