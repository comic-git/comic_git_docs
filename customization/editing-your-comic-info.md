# Editing your Comic Info

{% hint style="info" %}
**Quickstart Tip**

If you're setting up comic\_git for the first time, and you just want to get started quickly without delving into all comic\_git's features, you will only need to edit the \[Comic Info] section.
{% endhint %}

When setting up your web comic, there are some important things you need to do to identify your comic, like give it a name or define what links go in the Links Bar. comic\_git also supports a few extra features that can be enabled for your project, like automatically generating thumbnails for your comic pages. All these settings can be adjusted in the `your_content/comic_info.ini` file.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/editing\_your\_website/comic\_info.png)\


This file is a standard [INI file](https://en.wikipedia.org/wiki/INI\_file) that you may have seen elsewhere, if you've ever edited config files for other programs. Its format is very simple: it consists of pairs of options and their values (e.g. `Title = Sample Comic`), and these options are separated into sections (e.g. `[Comic Info]`).

Each of the sections and their options are described in more detail below.

## **Quick Links**

* [Comic Info](editing-your-comic-info.md#comic-info)
  * [Comic name](editing-your-comic-info.md#comic-name)
  * [Author](editing-your-comic-info.md#author)
  * [Description](editing-your-comic-info.md#description)
* [Comic Settings](editing-your-comic-info.md#comic-settings)
  * [Date format](editing-your-comic-info.md#date-format)
  * [Timezone](editing-your-comic-info.md#timezone)
  * [Use images in navigation bar](editing-your-comic-info.md#use-images-in-navigation-bar)
  * [Comic domain (optional)](editing-your-comic-info.md#comic-domain-optional)
  * [Comic subdirectory (optional)](editing-your-comic-info.md#comic-subdirectory-optional)
  * [Use https when building comic URL (optional)](editing-your-comic-info.md#use-https-when-building-comic-url-optional)
  * [Theme (optional)](editing-your-comic-info.md#theme-optional)
  * [Extra Comics (optional)](editing-your-comic-info.md#extra-comics-optional)
  * [Banner image (optional)](editing-your-comic-info.md#banner-image-optional)
  * [Auto-detect comic images (optional)](editing-your-comic-info.md#auto-detect-comic-images-optional)
  * [Show Uncategorized comics (optional)](editing-your-comic-info.md#show-uncategorized-comics-optional)
* [Pages](editing-your-comic-info.md#pages)
* [Links Bar](editing-your-comic-info.md#links-bar)
* [Archive](editing-your-comic-info.md#archive)
  * [Use thumbnails](editing-your-comic-info.md#use-thumbnails)
  * [Date format](editing-your-comic-info.md#date-format-1)
* [Image Reprocessing](editing-your-comic-info.md#image-reprocessing)
  * [Create thumbnails](editing-your-comic-info.md#create-thumbnails)
  * [Thumbnail size](editing-your-comic-info.md#thumbnail-size)
  * [Overwrite existing images](editing-your-comic-info.md#overwrite-existing-images)
* [RSS Feed](editing-your-comic-info.md#rss-feed)
* [Transcripts](editing-your-comic-info.md#transcripts)
  * [Enable transcripts](editing-your-comic-info.md#enable-transcripts)
  * [Default language](editing-your-comic-info.md#default-language)
  * [Load transcripts from comic folder (optional)](editing-your-comic-info.md#load-transcripts-from-comic-folder-optional)
  * [Transcripts folder (optional)](editing-your-comic-info.md#transcripts-folder-optional)
* [Google Analytics](editing-your-comic-info.md#google-analytics)
  * [Tracking ID](editing-your-comic-info.md#tracking-id)

### Comic Info

#### Comic name

This is the name of your comic. The comic name shows up in the tab every time a page from your website is loaded. (e.g. Page 202 - Sample Comic). It does not have to match the name you gave your repository.

#### Author

Whatever name or credit you wish to give for the creation of your comic. It can be a single name, a list of names, a sentence, whatever you want. It's currently only used when generating your RSS Feed.

#### Description

A short, one-sentence description of your web comic. This will show up in your RSS feed and social media previews.

### Comic Settings

#### Date format

This is the date format that all your comic Post dates will be in. By default, they're similar to `July 20, 1969`. It's necessary that all Post dates use the same format, so that comic\_git knows how to organize your posts chronologically.

The following table is a list of common date format strings. You can copy/paste any of these into the **Date format** option to change what date format to use in your comic files. (See Adding Comic Pages for more info on setting the Post dates for your comics)

| Format string         | Example                   |
| --------------------- | ------------------------- |
| %B %d, %Y             | July 16, 1969             |
| %Y-%m-%d              | 1969-07-16                |
| %a, %d %b %Y %H:%M:%S | Wed, 16 Jul 1969 04:20:00 |

You can also build your own format strings. See https://docs.python.org/3/library/time.html#time.strftime for the list of %-substitutions you can use.

#### Timezone

The timezone for all the dates in your comic. This is important for when comic\_git is determining when scheduled posts should be published. For example, if you push out an update at 9pm your time just before the midnight deadline, you don't want comic\_git to publish the page right away just because it's past midnight in some other timezone!

All timezones found in the "TZ database name" column on [this page](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones) are allowed.

#### Use images in navigation bar

When set to `true`, this will replace the First, Previous, Next, and Latest navigation links on the comic pages with the icons found at `your_content/images/navigation_icons/`. You can change which icons are used by replacing these files with your own, or change this setting to `false` to just use text links.

#### Comic domain (optional)

If you are building your website locally, and you haven't configured a custom domain, you must set this so that comic\_git knows what domain to use to build the URL to link to your comic, for the purposes of things like your RSS feed and your social media preview links. For more details, see Building your Website on your own PC.

This option should include your website's entire domain, subdomain, and top-level domain. Do not include the slash at the end. You may include the "http://" or "https://" if you wish.

Examples: `http://ryanvilbrandt.github.io`, `www.tamberlanecomic.com`

#### Comic subdirectory (optional)

If you are building your website locally, and you have not set a custom domain, you must set this to the name of your GitHub repository. This allows most of the links on your website to function properly. For more details, see Building your Website on your own PC.

This option should not include leading or trailing slashes.

Examples: `comic_git`, `tamberlane`

#### Use https when building comic URL (optional)

If you are building your website locally, or you've set up a custom domain, setting this to `True` will force any URLs pointing to your website to use `https://` instead of `http://`.

#### Theme (optional)

The name of the theme to use for your site. If not included or left blank, comic\_git will use the default theme.

For more information, see Themes.

#### Extra Comics (optional)

A comma-separated list of any extra comics hosted on your site. For more information, see Extra Comics.

#### Banner image (optional)

This option tells comic\_git where to go to find the banner image for the comic, that big image that goes at the top of every page in the default comic\_git website layout. The default value for this option is `/your_content/images/banner.png`.

#### Auto-detect comic images (optional)

By default, comic\_git relies on the `Filename` option provided in each comic's info.ini file to know what image file to use for the comic page (see Adding Comic Pages). If the `Filename` is not provided in this case, comic\_git will raise an error and fail to build your website.

If this option is set to True, and `Filename` is missing from the comic's info.ini file, comic\_git will look in the comic page's directory for any image files. If it finds only one image file (aside from `thumbnail.jpg`), it will assume that is the image file it should use for the comic page. If there are no image files in the directory, or more than one image file in the directory, comic\_git will raise an error.

If you never store more than one image in a comic page's directory, this is a useful option to set, because it will save you the trouble of updating the `Filename` of your info.ini file every time you upload a new comic page.

If you wish to store multiple image files in a comic page's directory, you can define `Filename` in just that comic's info.ini file, and comic\_git will use that value instead of attempting to auto-detect the image.

Image files are any files with the following extensions: jpg, jpeg, png, tif, tiff, gif, bmp, webp, webv, svg, eps

#### Show Uncategorized comics (optional)

By default, if you don't give a comic page a `Storyline` value in its info.ini file, it will be placed in an "Uncategorized" section in your Archive page below all your other comic pages. If you wish for these Uncategorized pages to just not show up on your Archive page, set this value to `False`.

### Pages

This is a special section without pre-defined options. This section tells comic\_git what extra web pages beyond the standard comic pages to build. To the left of the equals sign is the template file name to use, to the right is the title of the page once it's built. If you wish to remove a default web page like tagged pages or the Latest page, delete that line from this section.

For more info on adding pages to your website, see Themes.

### \[Links Bar]

This is another special section without pre-defined options. This section tells comic\_git how to build out the Links Bar. To the left of the equals sign is the name of the link as it should show up on the website, to the right is the URL it links to. Any values that start with `/` will link to a page on your website. All other links will be treated as external links to other websites.

### Archive

#### Use thumbnails

When this value is False, the Archive page will display all the comics in your archive in an [unordered list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul), broken up by storyline. When this value is True, the Archive page will display all the comics in your archive in a grid of comic thumbnails. When looking for thumbnails, the Archive page look in each comic directory for a thumbnail image by the name of `thumbnail.jpg`

You can either create your own thumbnails, or use comic\_git's built-in thumbnail generation, described in the \[Image Reprocessing] section below.

#### Date format

This is the format that post dates will be displayed in when `Use thumbnails` is set to True. This is defined separately because longer post dates can screw up the spacing of the thumbnails in the grid. It accepts the same inputs as the `Date format` in the \[Comic Settings] section.

### Image Reprocessing

#### Create thumbnails

If set to True, thumbnails will be generated for each comic page, named `thumbnail.jpg`.

#### Thumbnail size

The size of the thumbnail to be generated. This can be a width/height pair in pixels like `100, 36`, a percentage of the size of the original image like `10%`, a set height in pixels (`100h`), or a set width in pixels (`100w`). For the latter two options, comic\_git will keep the aspect ratio of the original image the same, adjusting to fit just your defined height or width.

#### Overwrite existing images

If this is set to False, and a thumbnail already exists in the comic page's folder, comic\_git will not attempt to recreate the thumbnail. If set to True, comic\_git will always attempt to generate a thumbnail, assuming creating these files is enabled via one of the options above.

### RSS Feed

See Adding an RSS Feed

### Transcripts

#### Enable transcripts

If set to True, comic\_git will attempt to create a Transcripts section below every comic that has transcripts files provided for it. A transcript file is a text file with the name of the language as its filename, e.g. `English.txt`. The transcript file can contain plain text, unicode (for those fancy accents and non-roman alphabets), HTML tags, and Markdown.

#### Default language

If defined, any transcript \*.txt file will be moved to the top of the list of transcripts, and thus will automatically be loaded whenever the comic page is loaded. The default value for this is "English".

#### Load transcripts from comic folder (optional)

If this setting is `True`, comic\_git will search in each comic page's folder for any \*.txt files (except for post.txt) and any it finds it will add to the list of available transcripts for that comic page. The default value for this is `True`.

#### Transcripts folder (optional)

If you wish to move the transcripts to their own folder, you can define that folder here. Each page must have a separate folder that matches the comic folder name. For example, if you set the transcripts folder to be `your_content/transcripts`, then the transcript files for `Page 197` should be found at `your_content/transcripts/Page 197/`. This path is always relative to the root of the repository.

Both this setting and "Load transcripts from comic folder" can be set, so you can have transcripts in both places. In that case, transcript files in your custom transcripts folder will be used instead of transcript files in your comic folder, if they share the same name.

### Google Analytics

#### Tracking ID

If you have set up a [Google Analytics](https://analytics.google.com) for your comic, you can put the Tracking ID here (e.g. UA-123456789-0) and comic\_git will automatically insert the analytics tracking code on all pages of your website.
