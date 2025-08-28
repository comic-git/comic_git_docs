# Webring

{% hint style="success" %}
**Coming Soon!**
{% endhint %}

Do you remember [webrings](https://fanlore.org/wiki/Webring)? It seemed like every Geocities page had one, back in the day. Do you want some of that nostalgia back? Well, good news, comic\_git supports them!

To set up a webring, you'll be working with a JSON file. Don't worry if that sounds intimidating, JSON files are just text files with a specific format, like the INI files you've already been working with.

The first thing you'll need is to create a JSON file that's accessible on the public internet somewhere. This file will contain the information for all the sites on the webring, and each site using it will need to access it. See the [JSON File](webring.md#json-file) section below for how to set that up.

Then, you'll need to add a `[Webring]`  section to your `comic_info.ini` file, and add some config options to it. See the [Config Options](webring.md#config-options) section below for more info on that.

After that, just build your website and you should see your webring showing up at the bottom of each page on your website!

_Example image demonstrating the webring goes here_

## JSON File

The JSON file will contain all the information needed for a site to build its own webring layout. Its structure is important, so make sure to follow the schema defined below.

**Important Note:** If someone else has already created this file and you're just trying to enable the webring on your own comic\_git site, you can skip this section and go right to [Config Options](webring.md#config-options) below.

<details>

<summary>version</summary>

* Required
* Value: `number`: 1

</details>

<details>

<summary>name</summary>

* Optional
* Value: `string`&#x20;
* Default: empty string

The name of the header above the webring on the user's site.&#x20;

</details>

<details>

<summary>home</summary>

* Optional
* Value: `dictionary`&#x20;
* Default: none

If defined, a link to the webring homepage will be included in the webring layout. Must follow the schema of the [Comic dictionary](webring.md#comic-dictionary) below.

</details>

<details>

<summary>members</summary>

* Required
* Value: `list` of dictionaries

A list of all the comics in the webring. Each item in the list must be a dictionary matching the [Comic dictionary](webring.md#comic-dictionary) schema below. The order of the dictionaries in the list defines what comic is the "previous" and "next" links for a given comic.

The list wraps around, so if we take the [Example Payload](webring.md#example-payload) below, for Clara's Cliffside, the "Previous" comic would be Bertrand's Barn and the "Next" comic would be Albert's Atrium.

</details>

### Comic dictionary

<details>

<summary>id</summary>

* Required
* Value: `string`&#x20;

Any string uniquely identifying the given comic. It does not have to match the name of the comic. It will be used by a user when setting his [Config Options](webring.md#config-options), so that comic\_git can identify what the "Previous" and "Next" comics will be.

This value is **not** required when defining the dictionary for the `home` entry.

</details>

<details>

<summary>name</summary>

* Optional
* Value: `string`&#x20;
* Default: empty string

The name of the comic as displayed on the website. May not be displayed if `image` is defined.

</details>

<details>

<summary>url</summary>

* Required
* Value: `string` : URL

The URL linking to the homepage of the given comic.

</details>

<details>

<summary>image</summary>

* Optional:
* Value: `string` : URL of an image file
* Default: none

If defined, the given image will be displayed as a link to the given comic.

</details>

### Example Payload

```json
{
    "version": 1,
    "name": "Our Comics Webring!",
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
* Default: `False`&#x20;

If True, enables the webring feature. If False, all other options in this section are ignored.

</details>

<details>

<summary>Endpoint</summary>

* Required if "Enable webring" is True
* Value: `string`&#x20;

The URL to the JSON file containing the webring data. See [JSON File](webring.md#json-file) below.

This can be a URL to a file on another website (e.g. `https://my.webring.com/webring.json`) or to a file on your own site (e.g. `/your_content/webring.json`).

</details>

<details>

<summary>Webring ID</summary>

* Required if "Enable webring" is True

- Value: `string`&#x20;

A unique ID for your comic, as defined in `id` field of the webring data returned by the Endpoint. This is needed to know where your comic is in the list of comics in the webring.

</details>

<details>

<summary>Show all members</summary>

* Optional
* Value: `boolean`: `True` or `False`&#x20;
* Default: `False`

If True, instead of the webring displaying a Previous and Next link, it will display all webring members at once in a list.

</details>

### Example

```
[Webring]
Enable webring = True
Endpoint = https://my.webring.com/webring.json
Webring ID = comic_b
Show all members = False
```

