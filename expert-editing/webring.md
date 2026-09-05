# Webring

Do you remember [webrings](https://fanlore.org/wiki/Webring)? It seemed like every Geocities page had one, back in the day. If you want that same kind of shared navigation between comics, comic_git supports it.

A comic_git webring is driven by a JSON file that lists all the comics in the ring. Each comic_git site fetches that same JSON file, figures out where its own comic is in the list, and then renders either:

- Previous / Home / Next navigation
- a full list of all webring members

<div><figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Default layout with Home</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>"Show all members" with Home</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Default layout without Home</p></figcaption></figure> <figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>"Show all members" without Home</p></figcaption></figure></div>

## How it works

Setting up your webring in comic_git is really simple, and only requires two steps.

### Upload webring.json

{% hint style="info" %}
If someone else already created the webring JSON file, and you only need to connect your own comic_git site to it, you can skip ahead to [updating your comic_info.ini](webring.md#update-your-comic_infoini).
{% endhint %}

Upload a `webring.json` file to somewhere out on the publicly accessible internet. It could be Dropbox link, a file on another website, or even hosted from your own comic_git site. But it should be an absolute URL, meaning it should start with `https://`.

This gives not only you but every website on that webring access to the same webring data.

The format for this file is in the [JSON File](webring.md#json-file) section below.

### Update your comic_info.ini

Update your `comic_info.ini` file per the [Config Options](webring.md#config-options) section below.

## JSON File

The JSON file contains the information needed for each site on the webring to build its own webring layout, all using the same data. Its structure matters, so follow the schema below. Also, all URLs in the JSON file should be absolute URLs, meaning they should start with `https://`.

### Top-level fields

<details>

<summary>version</summary>

* Required
* Value: `number`: `1`

</details>

<details>

<summary>label</summary>

* Optional
* Value: `string`
* Default: empty string

The heading shown above the webring on the site.

</details>

<details>

<summary>home</summary>

* Optional
* Value: `dictionary`
* Default: none

If defined, a link to the webring homepage will appear in the webring layout. This uses the same structure as the [Comic dictionary](webring.md#comic-dictionary) below, except that it does not need an `id`.

</details>

<details>

<summary>members</summary>

* Required
* Value: `list` of dictionaries

A list of all comics in the webring. Each item must match the [Comic dictionary](webring.md#comic-dictionary) schema below.

The order of this list defines the Previous and Next links for each comic. The list wraps around, so the first and last members connect to each other.

</details>

### Comic dictionary

<details>

<summary>id</summary>

* Required
* Value: `string`

A unique ID for the comic. This is the value comic_git uses to identify your comic inside the webring member list.

This value is **not** required for the `home` entry.

</details>

<details>

<summary>name</summary>

* Optional
* Value: `string`
* Default: empty string

The name of the comic as displayed on the website. If an `image` is defined, the image may be shown instead of the text name.

</details>

<details>

<summary>url</summary>

* Required
* Value: `string`: absolute URL

The homepage URL for the comic.

</details>

<details>

<summary>image</summary>

* Optional
* Value: `string`: absolute URL of an image file
* Default: none

If defined, the image is displayed as a link to the comic.

</details>

### Example Payload

```json
{
    "version": 1,
    "label": "Our Comics Webring!",
    "home": {
        "name": "Home",
        "url": "https://my.webring.com/",
        "image": "https://my.webring.com/icon.png"
    },
    "members": [
        {
            "id": "comic_a",
            "name": "Albert's Atrium",
            "url": "https://comic.albert.net/",
            "image": "https://comic.albert.net/icon.png"
        },
        {
            "id": "comic_b",
            "name": "Bertrand's Barn",
            "url": "https://bertrand.github.io/my_barn",
            "image": "https://bertrand.github.io/my_barn/your_content/images/webring.jpg"
        },
        {
            "id": "comic_c",
            "name": "Clara's Cliffside",
            "url": "https://clara-is-cool.neocities.org/",
            "image": "https://images.ctfassets.net/hrltx12pl8hq/7JnR6tVVwDyUM8Cbci3GtJ/bf74366cff2ba271471725d0b0ef418c/shutterstock_376532611-og.jpg"
        }
    ]
}
```

## Config Options

<details>

<summary>Enable webring</summary>

* Optional
* Value: `boolean`: `True` or `False`
* Default: `False`

If `True`, enables the webring feature. If `False`, all other options in this section are ignored.

</details>

<details>

<summary>Endpoint</summary>

* Required if `Enable webring` is `True`
* Value: `string`

The location of the webring JSON data.

In normal use, this should be an absolute URL to a remotely hosted `webring.json` file, such as:

```text
https://my.webring.com/webring.json
```

For local development, see [Local Development](webring.md#local-development).

</details>

<details>

<summary>Webring ID</summary>

* Required if `Enable webring` is `True`
* Value: `string`

A unique ID for your comic, matching the `id` field of your comic's entry in the webring JSON.

comic_git uses this value to:

- determine your Previous and Next neighbors in the webring
- identify your own comic when `Exclude own comic from members = True`

</details>

<details>

<summary>Show all members</summary>

* Optional
* Value: `boolean`: `True` or `False`
* Default: `False`

If `True`, the webring displays all members at once instead of rendering Previous and Next links.

</details>

<details>

<summary>Exclude own comic from members</summary>

* Optional
* Value: `boolean`: `True` or `False`
* Default: `False`

If `True`, your own comic will be removed from the member list shown when `Show all members = True`.

This relies on `Webring ID`, so make sure your `Webring ID` exactly matches your member entry's `id`.

</details>

### Example

```ini
[Webring]
Enable webring = True
Endpoint = https://my.webring.com/webring.json
Webring ID = comic_b
Show all members = True
Exclude own comic from members = True
```

## Local Development

If you want to test a webring locally before you have a real hosted endpoint ready, comic_git supports a special local-development mode.

Set:

```ini
[Webring]
Endpoint = local
```

When `Endpoint = local`, comic_git reads this file directly from your host repo at:

```text
your_content/webring.json
```

This mode exists only to make local development easier. Even in local mode, the `url` and `image` values inside `webring.json` should still be absolute URLs.

## Customizing the look with CSS

If you want to change how your webring looks without changing its HTML layout, you can target the CSS classes used by the default `webring.tpl` template.

The webring uses one of two layouts:

* a Previous / Home / Next layout
* a full members list layout when `Show all members = True`

The HTML structure looks like this:

```text
nav.webring
├── h2.webring-header          (only if `label` is defined)
│
├── when `Show all members = True`
│   ├── div.webring-members
│   │   └── div.webring-member  (one per member)
│   │       └── a.webring-link
│   │           └── img.webring-img
│   │
│   └── if `home` is defined
│       ├── div.webring-home
│       │   └── a.webring-link
│       │       └── img.webring-img
│       └── div.webring-label
│           └── a.webring-link
│
└── when `Show all members = False`
    └── div.webring-links
        ├── div.webring-prev
        │   └── a.webring-link
        │       └── img.webring-img
        ├── div.webring-home     (if `home` is defined)
        │   └── a.webring-link
        │       └── img.webring-img
        ├── div.webring-next
        │   └── a.webring-link
        │       └── img.webring-img
        ├── div.webring-label
        │   └── a.webring-link   ("Previous")
        ├── div.webring-label    (for `home`, if defined)
        │   └── a.webring-link
        └── div.webring-label
            └── a.webring-link   ("Next")
```

{% hint style="info" %}
The default template also inserts a plain blank `<div>` in the Previous / Home / Next layout when `home` is not defined, so the spacing still works. That blank element does not have a CSS class.
{% endhint %}

### CSS classes used by the default webring

| CSS class          | Purpose                                                                                                                                                   |
|--------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `.webring`         | The outer `<nav>` element for the entire webring. This is the easiest place to set margins, borders, padding, background colors, or overall layout rules. |
| `.webring-header`  | The `<h2>` element used for the optional webring heading when the JSON payload defines `label`.                                                           |
| `.webring-members` | The container used when `Show all members = True`.                                                                                                        |
| `.webring-member`  | The wrapper for each member inside the full members list.                                                                                                 |
| `.webring-links`   | The container used for the Previous / Home / Next layout.                                                                                                 |
| `.webring-prev`    | The visual area for the previous comic link.                                                                                                              |
| `.webring-home`    | The visual area for the optional webring home link. This class appears in both layouts.                                                                   |
| `.webring-next`    | The visual area for the next comic link.                                                                                                                  |
| `.webring-label`   | The text-label rows underneath the link images in the Previous / Home / Next layout, and the home label in the full members layout.                       |
| `.webring-link`    | The `<a>` element used for all webring links. Use this to control link color, hover styles, underlines, and other anchor styling.                         |
| `.webring-img`     | The `<img>` element used when a webring entry provides an image. Use this to control image size, borders, spacing, or hover effects.                      |

## Customizing the layout with a Theme

If CSS is not enough, you can replace the default webring template completely.

To do that, create your own Theme and add a custom `webring.tpl` file in that Theme's `templates` folder. comic_git will use your themed template instead of the default one in `comic_git_engine`.

See [Themes](../advanced-editing/themes.md) for the full process for overriding template files.

If you want to edit `webring.tpl`, the most important Jinja variables are:

* `enable_webring`
* `webring_label`
* `webring_home`
* `show_all_members`
* `webring_members`
* `webring_prev`
* `webring_next`

For the current Jinja values available to templates, see [Template and Hook Data](template-and-hook-data.md).

When changing the template, keep in mind:

* `webring_members` is only used when `Show all members = True`
* `webring_prev` and `webring_next` are only used when `Show all members = False`
* `webring_home` may be missing entirely
* a member may have an `image`, or may only have a text `name`

## Troubleshooting

### The webring does not appear at all

Check these first:

- is `Enable webring = True`
- is `Endpoint` defined
- does the build log show a webring-related error

### The build says it could not load the webring data

Check that:

- the endpoint URL is correct and publicly accessible
- if you are using `Endpoint = local`, `your_content/webring.json` exists in the host repo
- the JSON file is valid JSON

### The build says `Webring ID` is missing

`Webring ID` is required whenever the webring feature is enabled.

### The build says it could not find your `Webring ID`

Make sure your configured `Webring ID` exactly matches one member `id` in the JSON file.

Matching is exact, including capitalization and punctuation.

### `Exclude own comic from members` does not seem to work

That option relies on `Webring ID`. If your `Webring ID` does not exactly match your member entry's `id`, comic_git will not know which member is your own comic.

### Previous and Next are not the sites you expected

Previous and Next follow the order of entries in the `members` list.

The list wraps around from first to last and from last to first.
