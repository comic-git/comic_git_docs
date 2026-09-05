# TOML Configuration

comic_git supports TOML files as an alternative to the traditional INI and text files. TOML is optional: existing `comic_info.ini`, page `info.ini`, `post.txt`, transcript, and `social_media.json` files continue to work.

TOML is useful when you want structured lists, per-image settings, or one self-contained file per comic or page. It is also the content format that future CMS work will build on.

{% hint style="warning" %}
If both formats exist for the same comic or page, the TOML file is the complete source of truth. comic_git does not merge missing values from the matching INI or page-local text files.
{% endhint %}

## Comic settings

Use `your_content/comic_info.toml` instead of `your_content/comic_info.ini`. Extra Comics may likewise use `your_content/<extra-comic>/comic_info.toml`.

A small main-comic configuration can look like this:

```toml
[engine]
version = "1.1"

[comic]
name = "My Comic"
author = "Comic Creator"
description = "A webcomic about..."

[site]
theme = "default"
date_format = "%B %d, %Y"
timezone = "America/Los_Angeles"

[archive]
use_thumbnails = true
entry_mode = "pages"

[[links]]
name = "Archive"
url = "/archive/"

[[pages]]
template_name = "archive"
title = "Archive"
```

The supported tables correspond to the sections documented in [Editing your Comic Info](../basic-editing/editing-your-comic-info.md):

| TOML table           | Supported keys                                                                                                                                |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| `[engine]`           | `version`                                                                                                                                     |
| `[comic]`            | `name`, `author`, `description`                                                                                                               |
| `[site]`             | `theme`, `banner_image`, `date_format`, `timezone`, `comic_domain`, `comic_subdirectory`, `extra_comics`, `on_comic_click`, `markdown_extras` |
| `[archive]`          | `date_format`, `use_thumbnails`, `show_uncategorized_comics`, `show_text_only_posts`, `entry_mode`, `image_title_fallback`                    |
| `[navigation]`       | `use_images`, `above_comic`, `below_comic`, `below_blurb`                                                                                     |
| `[transcripts]`      | `enabled`, `load_from_comic_folder`, `folder`, `default_language`                                                                             |
| `[image_processing]` | `create_thumbnails`, `overwrite_existing_images`, `thumbnail_size`                                                                            |
| `[analytics]`        | `google_analytics_id`                                                                                                                         |
| `[rss]`              | `build`, `newest_first`, `language`, `image`, `image_width`, `image_height`, `title_format`, `channel_description`, `combine_with_main`       |
| `[webring]`          | `enabled`, `endpoint`, `id`, `show_all_members`, `exclude_own_comic_from_members`                                                             |
| `[[links]]`          | `name` or `image_url`, `url`, `open_in_new_tab`                                                                                               |
| `[[pages]]`          | `template_name`, `title`                                                                                                                      |

TOML uses lowercase `snake_case` names. Lists such as `site.extra_comics`, `site.markdown_extras`, page `characters`, and page `tags` are written as arrays, for example `tags = ["funny", "chapter one"]`.

Unknown keys are rejected so that spelling mistakes do not silently change your site. Optional settings can be omitted when you want comic_git's default behavior.

The migration tool preserves uncommon legacy settings that do not have a first-class TOML key under `[legacy.<section>]`. This is a compatibility area for migrated data, not the preferred place for new settings.

{% hint style="info" %}
Site-wide social-media settings remain in `your_content/social_media.json`. The `[social_media]` example below is supported inside a page's `info.toml`; it is not currently accepted in `comic_info.toml`.
{% endhint %}

## Comic pages

Use `info.toml` inside a comic page folder instead of that page's `info.ini`. A TOML page can contain its post, transcripts, social-media overrides, and ordered images in one file:

```toml
post_date = "2026-09-04"
title = "Chapter One"
alt_text = "Page-level alt text used as an image fallback."
storyline = "Book 1"
characters = ["Alice", "Bob"]
tags = ["mystery", "rain"]

post_text = """
The news post for this page supports Markdown.
"""

[transcripts]
English = """
Panel one: Alice opens the door.
"""

[social_media]
"og:title" = "A custom sharing title"

[[images]]
filename = "page-1.png"
title = "Opening panel"
alt_text = "Alice opens a blue door."
thumbnail = "page-1-thumbnail.jpg"

[[images]]
filename = "page-2.png"
```

Important differences from `info.ini` pages:

* `post_date` must use `YYYY-MM-DD`.
* Every image must be listed in an ordered `[[images]]` table. TOML pages do not auto-discover images.
* An image's optional `title`, `alt_text`, and `thumbnail` override page-level fallbacks.
* Inline `post_text`, `[transcripts]`, and `[social_media]` replace that page's `post.txt`, transcript files, and `social_media.json`.
* Custom template or hook values belong in `[extra]`. Keys beginning with `!` are intentionally not migrated into public page data.

See [Adding Comic Pages](../basic-editing/adding-comic-pages.md) for how page and image fallbacks work.

## Migrating existing files

The migration tool starts in dry-run mode and does not change your files unless you pass `--write`.

Run these commands from the root of your comic_git repository:

```powershell
python -m pip install -r comic_git_engine\requirements_migration.txt
python comic_git_engine\src\build\migrate_to_toml.py
```

Review the list of files it would create. Then write the TOML files:

```powershell
python comic_git_engine\src\build\migrate_to_toml.py --write
```

The migration includes configured Extra Comics by default. Add `--main-only` if you only want the main comic.

After writing the files:

1. Review the generated TOML.
2. Build and inspect your site.
3. Commit the working migration so you have an easy recovery point.
4. Keep the legacy files as a backup, or remove them only after you are satisfied with the TOML build.

If you choose to remove the replaced legacy files, the migration tool can do that explicitly:

```powershell
python comic_git_engine\src\build\migrate_to_toml.py --delete-legacy
```

{% hint style="danger" %}
`--delete-legacy` deletes replaced INI, post, transcript, and page social-media files. Commit or back up your work first. Before deleting anything, comic_git validates every replacement TOML file in scope and cancels the deletion if one is invalid.
{% endhint %}

You do not need to migrate to TOML to continue using comic_git for the foreseeable future.
