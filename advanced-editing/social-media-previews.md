# Social Media Previews

When someone shares one of your comic pages on a social platform, chat app, or forum, that site may generate a preview card with a title, description, and image. comic_git supports this automatically by generating [Open Graph](https://ogp.me/) metadata when your site is built.

You do **not** need to configure everything by hand to get started. comic_git already provides sensible defaults, and you can customize them when you need more control.

## What happens by default

comic_git has built-in Social Media Preview behavior for different kinds of pages.

| Page group | What it covers                            | Default behavior                                                                                           |
|------------|-------------------------------------------|------------------------------------------------------------------------------------------------------------|
| `base`     | General site pages such as your home page | Uses your comic name, comic description, page title, page URL, and `your_content/images/preview_image.png` |
| `comic`    | Individual comic pages                    | Uses your comic name, page title, page URL, post text, thumbnail image, and alt text                       |
| `latest`   | The Latest page                           | Uses the same default behavior as `comic`                                                                  |

This means your home page and your comic pages do **not** necessarily use the same preview image or description by default.

{% hint style="info" %}
If you do nothing, comic_git will still generate Social Media Preview metadata for your pages.
{% endhint %}

## The simplest setup

The easiest way to improve your previews is to add a default preview image.

Create a file named `preview_image.png` and place it here:

```text
your_content/images/preview_image.png
```

This image is used by the default `base` preview behavior, which usually means your home page and other non-comic pages.

A `200` by `200` pixel image is the recommended default size for Social Media Preview images.

You can change that default image in either of these ways:

- replace `your_content/images/preview_image.png`
- set `og:image` in `your_content/social_media.json` to a full image URL

{% hint style="info" %}
**Screenshot placeholder**

Capture `social-media-previews-01-preview-image-location.png`: show the repository tree with `your_content/images/preview_image.png` visible.
{% endhint %}

## How to customize Social Media Previews

If you want more control, create this file:

```text
your_content/social_media.json
```

comic_git will load that file and use it to customize the Social Media Preview metadata for that comic.

{% hint style="info" %}
**Screenshot placeholder**

Capture `social-media-previews-02-social-media-json-location.png`: show the repository tree with `your_content/social_media.json` visible.
{% endhint %}

The file must be valid JSON.

## How top-level sections work

The top-level keys in `social_media.json` are **not** a fixed list.

Each top-level key is the name of the template whose Social Media Preview metadata you want to control. comic_git looks for a section whose name matches the template it is currently rendering.

So the rule is:

- if a top-level key matches the template name, comic_git uses that section
- if there is no matching section, comic_git falls back to `base`

This means you can use any template name here, including custom templates you created yourself.

For example, if your site builds a page from a template called `about`, you can add an `about` section. If your site builds a page from a template called `cast`, you can add a `cast` section.

`base` is the important special case, because it acts as the fallback section when there is no exact template-name match.

Inside any section, `_inherits` is also special. It lets one section start from another section's values before applying overrides.

The built-in defaults use `base`, `comic`, and `latest`, so those are the most common keys you will see. Here is that default behavior written out explicitly:

```json
{
  "base": {
    "og:type": "website",
    "og:site_name": "_comic_name",
    "og:title": "_title",
    "og:description": "_comic_description",
    "og:url": "_url",
    "og:image": "_preview_image"
  },
  "comic": {
    "og:type": "article",
    "og:site_name": "_comic_name",
    "og:title": "_title",
    "og:description": "_post_text",
    "og:url": "_url",
    "og:image": "_thumbnail",
    "og:image:alt": "_alt_text"
  },
  "latest": {
    "_inherits": "comic"
  }
}
```

{% hint style="info" %}
**Screenshot placeholder**

Capture `social-media-previews-03-social-media-json-example.png`: show an editor open to a simple `your_content/social_media.json` example.
{% endhint %}

## Special values comic_git understands

Inside `social_media.json`, comic_git recognizes the following placeholder values:

| Placeholder          | Meaning                                  |
|----------------------|------------------------------------------|
| `_comic_name`        | The `Comic name` from `[Comic Info]`     |
| `_comic_description` | The `Description` from `[Comic Info]`    |
| `_url`               | The final URL of the page being built    |
| `_title`             | The page title                           |
| `_preview_image`     | `your_content/images/preview_image.png`  |
| `_thumbnail`         | The thumbnail for the current comic page |
| `_post_text`         | The current page's post text             |
| `_alt_text`          | The current page's alt text              |

You can also use ordinary text or URLs directly. A value does **not** need to be a placeholder.

## Common customization examples

### Use a custom image URL

If you want to point previews at a specific image URL instead of `preview_image.png`, you can do this:

```json
{
  "base": {
    "og:image": "https://example.com/images/social-card.png"
  }
}
```

### Set image dimensions

If you want to give platforms a hint as to the best size to render your preview image, add `og:image:width` and `og:image:height`:

```json
{
  "base": {
    "og:image": "_preview_image",
    "og:image:width": "1200",
    "og:image:height": "630"
  }
}
```

Note that I said "hint" above. Depending on the platform a user is viewing the preview image on, these settings may not have a noticeable effect. It really depends on what the platform decides to do with these values.


### Use `_inherits` to avoid duplication

`_inherits` lets one section start with another section's values, then override only what you want to change.

That means:

- `latest` can inherit from `comic`
- you only need to specify the values that are different

For example, if you want to add a title to the Latest page that will show up in previews, you can do so by adding a value for `og:title`:

```json
  ...
  "latest": {
    "_inherits": "comic",
    "og:title": "Read the latest page"
  }
  ...
```

## How preview images work

Preview images work differently depending on the page type.

- General pages use `preview_image.png` by default through `_preview_image`
- Comic pages use the page thumbnail by default through `_thumbnail`
- You can override either behavior with `og:image`
- You can declare image size metadata with `og:image:width` and `og:image:height`

This means comic pages usually look best if you enable thumbnail generation or provide your own thumbnails.

If a comic page has no thumbnail, and you do not override `og:image`, the metadata may still exist, but the preview image may be missing or not look the way you expect.

## Extra Comics

Extra Comics follow a specific fallback order.

When comic_git looks for Social Media Preview settings for an Extra Comic, it checks:

1. that Extra Comic's own `your_content/social_media.json`
2. the main comic's `your_content/social_media.json`
3. the built-in engine defaults

So an Extra Comic can:

- define its own Social Media Preview behavior
- inherit the main comic's Social Media Preview behavior if it does not define its own file
- fall back to engine defaults if neither file exists

This makes it easy to share one main Social Media Preview config across your site while still overriding individual Extra Comics when needed.

## Per-comic-page overrides

You also have the option of providing a `social_media.json` file to override the behavior of specific comic pages. Just place a `social_media.json` file, using the same format as always, in the comic folder itself.

For example, if you want the Social Media Preview of comic page 005 to show the special preview image you prepared that's stored in `your_content/images/chocolate_starfish.tiff`, create the file `your_content/comics/005/social_media.json` and give it the contents:

```json
{
  "comic": {
    "og:type": "article",
    "og:site_name": "_comic_name",
    "og:title": "_title",
    "og:description": "_post_text",
    "og:url": "_url",
    "og:image": "your_content/images/chocolate_starfish.tiff",
    "og:image:alt": "_alt_text"
  }
}
```

## How to check your results

After your site is built and published, test one of your pages with a preview debugger.

A good option is [Facebook's Sharing Debugger](https://developers.facebook.com/tools/debug/sharing/). Even if your audience does not use Facebook, that tool is useful for checking the Open Graph metadata your page exposes.

{% hint style="info" %}
**Screenshot placeholder**

Capture `social-media-previews-04-facebook-debugger-example.png`: show a validator or debugger result for a built comic page with preview metadata visible.
{% endhint %}

Preview cards are often cached by third-party platforms. If you change your metadata and the preview does not update right away, the platform cache may be the reason.

## Troubleshooting

### My preview image is not showing

Check these first:

- does `og:image` point at the image you expect
- does that image URL actually load in a browser
- if you are relying on `_thumbnail`, does the comic page have a thumbnail
- if you are relying on `_preview_image`, does `your_content/images/preview_image.png` exist

### The title or description is wrong

Check whether the page is using:

- built-in defaults
- your main `social_media.json`
- an Extra Comic `social_media.json`

If you used placeholders, make sure they refer to the values you expect.

### The preview is using the wrong URL

Check `Comic domain` and `Comic subdirectory`, especially if you build locally.

### My Extra Comic is not using the preview settings I expected

Remember the fallback order:

1. Extra Comic `social_media.json`
2. main comic `social_media.json`
3. engine defaults

### My changes are not appearing

The page may be published correctly, but the platform showing the preview may still be using cached data. Try a preview debugger and force a refresh there if possible.

## Summary of the main files involved

| File                                         | What it controls                             |
|----------------------------------------------|----------------------------------------------|
| `your_content/images/preview_image.png`      | Default preview image for general pages      |
| `your_content/social_media.json`             | Main Social Media Preview customization file |
| Extra Comic `your_content/social_media.json` | Extra Comic-specific override file           |
| Comic thumbnails                             | Default preview images for comic pages       |

If you want to change how metadata is inserted into the page templates themselves, see [Themes](themes.md) and [Other Expert Tips](../expert-editing/other-expert-tips.md#list-of-values-available-to-jinja2-templates).
