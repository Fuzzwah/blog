## Why

The blog tracks an evolving agentic coding stack — tools and workflows change over time, and newer posts often supersede older ones. Readers landing on an older post (e.g., "Adopting GitHub Spec-Kit") have no way of knowing that a newer, more relevant post exists (e.g., "Switching from Spec-Kit to OpenSpec"). They need a clear signal at the top of the page pointing them forward.

## What Changes

- Add a frontmatter-driven update notice system for blog posts
- Create a reusable Jekyll include that renders a notification banner when `update_notice` frontmatter is present
- Add the include to the post layout so it appears between the post header and content
- Style the notice as a subtle info box with left accent bar, supporting both light and dark themes
- Add update notices to two existing posts that have been superseded by newer content

## Capabilities

### New Capabilities
- `update-notice`: A frontmatter-driven notification banner system for blog posts. Covers the frontmatter schema (`message`, `url`, `link_text`), the Jekyll include, post layout integration, and CSS styling with dark mode support.

### Modified Capabilities
None — this is purely additive. Posts without `update_notice` frontmatter are completely unaffected.

## Impact

- **New file**: `_includes/update_notice.html`
- **Modified**: `_layouts/post.html` (one include line added)
- **Modified**: `assets/css/style.scss` (update-notice styles added, plus dark mode variant)
- **Modified**: `_posts/2026-01-07-my-agentic-coding-stack.md` (frontmatter addition)
- **Modified**: `_posts/2026-01-08-adopting-github-spec-kit.md` (frontmatter addition)
- No dependencies added. No breaking changes.
