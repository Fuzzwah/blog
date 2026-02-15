## Context

The blog is a Jekyll site with a simple post layout (`_layouts/post.html`) that renders a title, date/reading-time meta, content, and prev/next navigation. Posts use YAML frontmatter for metadata (`title`, `date`, `author`, `description`, optional `image`). There is no existing mechanism for cross-referencing or linking related posts beyond manual inline markdown links within content.

The blog covers an evolving tooling stack. Several older posts have been directly superseded by newer ones (Spec-Kit → OpenSpec, original stack post → workflow comparison). Readers need a clear signal before they invest time reading potentially outdated material.

## Goals / Non-Goals

**Goals:**
- Provide a simple, maintainable way to flag older posts with a link to newer content
- Keep the mechanism entirely frontmatter-driven — no data files, no plugins, no build-time logic
- Support both light and dark themes
- Make the notice visually distinct from post content but not disruptive

**Non-Goals:**
- Automatic detection of superseded posts (manual frontmatter is deliberate and gives control over messaging)
- Support for multiple notices per post (keep it simple — one notice max)
- Series/collection grouping system
- Any changes to the homepage or post listing pages

## Decisions

**Frontmatter schema over data files**: The `update_notice` frontmatter object lives directly in the post that needs the notice. This is simpler than a separate `_data/` mapping file because the notice context lives right next to the content it annotates. Tradeoff: if a target URL changes, you update individual posts rather than one central file — acceptable given the small post count and infrequent changes.

**Separate include file over inline layout code**: Using `_includes/update_notice.html` rather than embedding the conditional directly in `post.html`. This keeps the layout clean and makes the notice component reusable if other layouts ever need it.

**Left-accent-bar info box style**: Using `$info-color` (blue) with a 4px left border and subtle background. This follows a widely recognised "info callout" pattern without being alarming (not red/warning). The `$info-color` variable already exists in `_config.scss`.

**Placement between header and content**: The notice sits after the post title and meta line but before the post body. This ensures readers see it before they start reading, which is the whole point.

## Risks / Trade-offs

**Manual maintenance** → Acceptable for a small blog. If the blog grows significantly, could revisit with a data-driven approach, but that's a problem for future-Fuzzwah.

**Hardcoded URL paths in frontmatter** → If permalink structure changes, notice links break. Mitigated by Jekyll's stable default permalink format and the small number of posts involved.
