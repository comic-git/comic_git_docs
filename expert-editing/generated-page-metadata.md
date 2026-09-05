# Generated Page Metadata

comic_git publishes structured page information for client-side features and external tools. The main comic writes:

```text
/comic/page_info_list.json
```

Each Extra Comic writes a similar file under its own URL, such as `/sidestory/comic/page_info_list.json`.

This JSON file is public site output. Do not put secrets or private notes in fields that you expect it to publish.

## Version 1 structure

The top level contains:

| Field                      | Meaning                                              |
|----------------------------|------------------------------------------------------|
| `schema_version`           | Metadata format version; currently `1`               |
| `comic_git_engine_version` | Engine release that generated the file               |
| `comic`                    | The current comic's ID and name                      |
| `scheduled_post_count`     | Number of future-dated pages omitted from this build |
| `pages`                    | Published pages in chronological order               |

Each page includes its stable comic/page ID, page-folder name, URL, resolved title, ISO post date, optional thumbnail URL, storyline, characters, tags, transcript languages, ordered images, and public custom `extra` values.

Each image includes:

* `filename`
* `url`
* resolved `title`
* resolved `alt_text`
* optional `thumbnail_url`

Image array order is the public ordering contract. Internal image IDs and HTML anchors are not published. If an integration needs a standalone-page image fragment, derive `#comic-image-1`, `#comic-image-2`, and so on from the one-based image position.

## Private page values

Legacy page-info keys that begin with `!` are omitted from public metadata. For example, `!Editor note = redraw later` can remain in the source file without being written to `page_info_list.json`.

This prefix is a lightweight publication filter, not encryption. Keep real secrets outside page source files and pass sensitive Code Hook values through GitHub Secrets.

## Schema

Version 1 output is defined by the [JSON Schema in comic_git_engine](https://github.com/comic-git/comic_git_engine/blob/latest/schemas/page_info_list.schema.json). Integrations should check `schema_version` and ignore engine implementation details that are not part of that schema.

For the corresponding Jinja and Code Hook objects, see [Template and Hook Data](template-and-hook-data.md).
