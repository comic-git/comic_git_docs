# Template and Hook Data

comic_git uses structured page and image objects when rendering Jinja templates and calling page-related Code Hooks. Default templates and simple CSS-only themes do not need to work with these objects directly.

## Page data

Comic templates receive a `page` object and an `images` list. Common values are:

| Value                                                                | Meaning                                                |
|----------------------------------------------------------------------|--------------------------------------------------------|
| `page.page_name`                                                     | The comic page folder name and URL identifier          |
| `page.url`                                                           | The published URL path for the page                    |
| `page.title`                                                         | The resolved page title                                |
| `page.post_date`                                                     | The normalized ISO date or datetime                    |
| `page.display_post_date`                                             | The date formatted for the comic page                  |
| `page.archive_post_date`                                             | The date formatted for the archive                     |
| `page.images` or `images`                                            | The ordered list of image objects                      |
| `page.thumbnail_path`                                                | The resolved page thumbnail, or `None`                 |
| `page.storyline`                                                     | The page's storyline, or an empty string               |
| `page.characters`                                                    | Ordered character names                                |
| `page.tags`                                                          | Ordered tags                                           |
| `page.post_md`                                                       | Combined source post text                              |
| `page.post_html`                                                     | Rendered post HTML                                     |
| `page.transcripts`                                                   | Transcript text keyed by language                      |
| `page.extra`                                                         | Custom page values added by source files or Code Hooks |
| `page.first_id`, `previous_id`, `next_id`, `last_id`                 | Navigation page names                                  |
| `page.first_anchor`, `previous_anchor`, `next_anchor`, `last_anchor` | Navigation destinations within those pages             |

For compatibility and convenience, the default comic-page context also provides aliases including `page_name`, `page_title`, `images`, `first_id`, `previous_id`, `current_id`, `next_id`, `last_id`, `archive_post_date`, `post_md`, `post_html`, `transcripts`, `_title`, `_post_date`, `_storyline`, `_characters`, `_tags`, and `_on_comic_click`.

Code Hooks can add custom values through `page.extra`. Public keys are emitted under the page's `extra` object in `page_info_list.json`; keys beginning with `!` remain private.

## Image data

Each item in `images` provides:

| Value                  | Meaning                                                      |
|------------------------|--------------------------------------------------------------|
| `image.filename`       | Filename relative to the page folder                         |
| `image.web_path`       | Image path within the site; prepend `base_dir` in a template |
| `image.title`          | Resolved image title                                         |
| `image.alt_text`       | Resolved image alt text                                      |
| `image.thumbnail_path` | Resolved image thumbnail, or `None`                          |

Image order is authoritative. Standalone comic pages use `#comic-image-1`, `#comic-image-2`, and so on. Infinite Scroll uses `#<page-name>_01`, `#<page-name>_02`, and so on.

A simple image loop looks like this:

```jinja2
{% for image in images %}
<div class="comic-image-container" id="comic-image-{{ loop.index }}">
    <img class="comic-image" src="{{ base_dir }}/{{ image.web_path | e }}" alt="{{ image.alt_text | e }}">
</div>
{% endfor %}
```

## Archive data

`storylines` is grouped by storyline name, with each item represented by an archive-entry object. Archive entries provide `page_name`, `page_url`, `post_date`, `title`, `thumbnail_path`, optional `image`, and optional `image_index`.

When `list_images_separately` is false, there is one Archive entry per page. When it is true, there is one entry per image, while an included text-only page still has one entry without an image.

Link each entry to the content it represents. Image entries use their one-based image position, while text-only entries use `post-body`:

```jinja2
{% for entry in entries %}
<a href="{{ entry.page_url | e }}#{{ "comic-image-" ~ entry.image_index if entry.image_index else "post-body" }}">
    {{ entry.title }}
</a>
{% endfor %}
```

`entry.thumbnail_path` can be `None`. Check it before rendering an image, and prepend `base_dir` when it exists.

## Global template values

The global context includes values such as `comic_title`, `comic_author`, `comic_description`, `banner_image`, `theme`, `comic_url`, `base_dir`, `comic_base_dir`, `content_base_dir`, `links`, `storylines`, `home_page_text`, `scheduled_post_count`, navigation settings, and Webring values.

It also includes:

* `comic_folder`: the current Extra Comic folder, or an empty string for the main comic
* `infinite_scroll_chapters`: the first image-bearing page in each storyline
* `extra_comics`: page-object lists keyed by Extra Comic name
* `tagged_pages_enabled` on comic-page rendering: whether the current comic has a `tagged` page configured

For Python hook inputs, see [Code Hooks](code-hooks.md).
