# Customizing Your Website

## Editing Your Comic's Homepage

By default, comic\_git comes with a landing page that is more useful for people visiting comic\_git itself rather than your personal webcomic. One of the first things you'll want to do is edit this page to your preferred message for new visitors.

To do so, open up `your_content/home page.txt` in any text editor. This is an HTML file, but is labeled .txt for ease of use; comic\_git will properly convert it to `index.html` when building the site. Feel free to change the text and add or delete any HTML you want.&#x20;

{% hint style="info" %}
If you're substantially changing the front page, we recommend at least providing links to both the first and last pages of your comic.
{% endhint %}

## Editing the Colors and Layout of your Website

The default color scheme of comic\_git is designed to be as appealing to as many people as possible, but you will most likely want to change the color scheme to suit your own tastes and better reflect the art of the comic you're creating.

To change the colors of your website, edit the file `your_content/themes/default/css/stylesheet.css`. This is a [CSS file](https://www.codecademy.com/learn/learn-css), and it is where things like text color, background color, font type, font style, and general layout of a website are defined. You can even define the width/height of your comic images here (by default, the comic images grow to fit the site width automatically while keeping their aspect ratio).

Basic (and hopefully easy to understand) settings are pre-defined here, with comments describing what each one does. It is very difficult to actually break your website by changing this file, so feel free to change these values to your heart's content. In the worst case, deleting this file will cause your browser to use its default colors for everything, which won't look too horrible.

If you wish to change more complex things like the spacing between different elements in your website, you can look at [advanced\_stylesheet.css](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/css/advanced_stylesheet.css) in the main comic\_git\_engine repository for guidance. This contains all the style definitions that are less clear-cut or changing them and making them look good takes a lot of trial and error. If you wish to change any of these values, copy them into `stylesheet.css` first, then make changes there. Any values defined in your `stylesheet.css` take precedence over the values in `advanced_stylesheet.css`.&#x20;

If you are comfortable with CSS, you do not need to limit yourself to only what's in `stylesheet.css` or `advanced_stylesheet.css`. Feel free to add whatever CSS settings you need!

## Adding a New Font

In addition to editing the colors of your website to create the look you want, you may also want to change the font. There are many ways to add a new font to a website, but the easiest way in comic\_git is to import the font into your CSS file.

First, get access to the new font you want to add. For this example, we'll use [Google Fonts](https://fonts.google.com/) and the Roboto font. If you wish to add this font to your website, you want to go that font family on the Google Fonts website, [open up the sidebar](https://fonts.google.com/specimen/Roboto?sidebar.open=true\&selection.family=Roboto), and select the Embed tab. This will show you something like the following:

```
<link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
```

What you want from this is the URL that will let you access the font: `https://fonts.googleapis.com/css2?family=Roboto&display=swap`

Next, open `your_content/themes/default/css/fonts.css` and add the following line:

```
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
```

After that, that font is accessible by any of the CSS styles in `stylesheet.css`! You can apply it to all the text in your `<body>` tag, for example (that's the main text on your website):

```
body {
    font-family: 'Roboto', sans-serif;  /* The font of all the text on all pages */
    ...
```

You can also use font files that you have downloaded or created. For example, if you manage to download Roboto as the file `Roboto.ttf` and made the directory `your_content/fonts/` to put it in, you could import that font in your CSS file like so:

```
@import url('../../../fonts/Roboto.ttf');
```

## Creating a Banner Image

The first thing your users see when they come to your website is the banner at the top of the screen. You should probably make one of your own. Once you do, save it as a PNG, name it `banner.png`, and copy it into the `your_content/images/` folder. The filename is case-sensitive, so make sure you name it exactly as written here.

{% hint style="info" %}
The `images` folder is for any images that you want to use on your website that are not comic pages, like a cast page or your text posts.
{% endhint %}

If you want to use a different file for your banner, for example as a JPG instead of a PNG, you can set your banner image filepath to whatever you like using the [Banner image](editing-your-comic-info.md#banner-image) option in your `comic_info.ini` file.

## Adding Text Before or After Each Comic Post

Each comic page allows for its own text post to accompany the comic below the image. However, you may prefer if every comic page was accompanied by some common text, like a request to your readers to support you on Patreon. Fortunately, comic\_git supports this out of the box.

To add some text that will show up **before** every comic post, add a `before post text.txt` file to the `your_content` directory. To add some text that will show up **after** every comic post, add an `after post text.txt` file to the `your_content` directory. Both files support both [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML formatting.

The filenames `before post text.html` and `after post text.html` are also accepted.

## Adding a Favicon to Your Website

Look at the tab for this webpage in your browser right now. You see that star with a comet tail? That's called a favicon. It helps users distinguish between different browser tabs at a glance.

comic\_git comes with a favicon already created and configured. It's recommended that you replace this with your own favicon. To create a favicon, you can use an online favicon generator, like [this one](https://www.favicon-generator.org/). You will want to create or pick a square image. You can resize this image yourself down to 16x16, or let the website do it for you.

Once the favicon is generated, make sure it's named `favicon.ico` and replace the file of the same name with that one in the root directory of your website. Then, you're done!

{% hint style="info" %}
The favicon is the only piece of customizable content found outside the `your_content` folder.
{% endhint %}

## Changing the Navigation Icons

By default, comic\_git uses icons for the First, Previous, Next, and Latest comic links on every comic page. You can disable these icons in your `comic_info.ini` and just use text links instead, or you can replace these icons with your own images.

Just replace any of the images in `your_content/images/navigation_icons/` with ones you want, and they will be used when building the comic pages.

* Make sure to provide both "enabled" and "disabled" images. The "enabled" images will be used when the links can be clicked, and the "disabled" images will be used when they cannot, like when you're at the first or last comic in the list.
* The new images must be named the same as the default images, including uppercase letters. Those filenames are:
  * `Icon_First.png`
  * `Icon_First_Disabled.png`
  * `Icon_Latest.png`
  * `Icon_Latest_Disabled.png`
  * `Icon_Next.png`
  * `Icon_Next_Disabled.png`
  * `Icon_Previous.png`
  * `Icon_Previous_Disabled.png`

## Other Changes

To make further changes to your website, like adding your own pages or changing the HTML of existing pages, see [Themes](../additional-information/extra-features.md#themes).
