# Customizing Your Website

## Editing Your Comic's Homepage

By default, comic\_git comes with a landing page that is more useful for people visiting comic\_git itself rather than your personal webcomic. One of the first things you'll want to do is edit this page to your preferred message for new visitors.

To do so, open up `your_content/home page.txt` in any text editor. You can include any HTML here that you want, though it's recommended you provide links to both your first and last pages of your comic at least.

## Editing the Colors and Layout of your Website

I did my best at making the default color scheme of comic\_git as appealing to as many people as possible, but you will most likely want to change the color scheme to suit your own tastes and better reflect the art of the comic you're creating.

To change the colors of your website, you will want to edit the file `your_content/themes/default/css/stylesheet.css`. This is a [CSS file](https://www.codecademy.com/learn/learn-css), and it is where things like text color, background color, font type, font style, and general layout of a website are defined. You can even define the width/height of your comic images here (by default, the comic images grow to fit the site width automatically while keeping their aspect ratio).

I've put the most basic and (hopefully) easy to understand settings in this file, and described what each does with a comment. Feel free to change these values to your heart's content, as it will be very hard to break your website by changing this file. In the worst case, deleting this file will cause your browser to use its default colors for everything, which won't look too horrible.

If you wish to change more complex things like the spacing between different elements in your website, you can look at `/src/css/advanced_stylesheet.css` for guidance. This contains all the style definitions that are less clear-cut or changing them and making them look good takes a lot of trial and error. If you wish to change any of these values, I recommend moving them over to `stylesheet.css` and changing them there, where they will take priority over the values in `advanced_stylesheet.css`. This will allow you to more easily apply future updates of comic\_git later.

## Adding a New Font

In addition to editing the colors of your website to create the look you want, you will probably also want to change the font. There are many ways to add a new font to a website, but the easiest way in comic\_git is to import the font into your CSS file.

First, get access to the new font you want to add. For this example, I'll reference [Google Fonts](https://fonts.google.com/) and the Roboto font. If you wish to add this font to your website, you want to go that font family on the Google Fonts website, [open up the sidebar](https://fonts.google.com/specimen/Roboto?sidebar.open=true\&selection.family=Roboto), and select the Embed tab. This will show you something like the following:

```
<link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
```

What you want from this is the URL that will let you access the font: `https://fonts.googleapis.com/css2?family=Roboto&display=swap`

Next, open up `your_content/themes/default/css/stylesheet.css` and put the following line somewhere near the top of the file, above any place in the file where you might want to apply your font to a CSS style.

```
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
```

After that, that font is accessible by any of the CSS styles in `stylesheet.css`! You can apply it to all the text in your \<body> tag, for example (that's the main text on your website):

```
body {
    font-family: 'Roboto', sans-serif;  /* The font of all the text on all pages */
    ...
```

You can also use font files that you have downloaded or created. For example, if you managed to download Roboto as the file `Roboto.ttf` and made the directory `your_content/fonts/` to put it in, you could import that font in your CSS file like so:

```
@import url('../../../fonts/Roboto.ttf');
```

## Creating a Banner Image

The first thing your users see when they come to your website is the banner at the top of the screen. You should probably make one of your own. When you do, save it as a PNG, name it "banner.png", and copy it into the `your_content/images/` folder. That filename is case-sensitive, so make sure you name it exactly that.

This `images` folder is for your own personal use. It's recommended that you put any images here that you want to use in other parts of your website, like a cast page or your text posts.

If you wish to provide a different file for your banner, for example as a JPG instead of a PNG, you can set your banner image filepath to whatever you like using the Banner image option in your comic\_info.ini file.

## Adding Text Before or After Each Comic Post

Each comic page allows for its own text post to accompany the comic below the image. However, you may prefer if every comic page was accompanied by some common text, like a plea to your readers to support you on Patreon. Fortunately, comic\_git supports this out of the box.

To add some text that will show up **before** every comic post, add a `before post text.txt` file to the `your_content` directory. To add some text that will show up **after** every comic post, add an `after post text.txt` file to the `your_content` directory. Both files support both [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML formatting.

The filenames `before post text.html` and `after post text.html` are also accepted.

## Adding a Favicon to Your Website

Look at the tab for this webpage in your browser right now. You see that black circle with a cat silhouette in the middle? That's called a favicon. It helps users distinguish between different browser tabs at a glance.

comic\_git comes with a favicon already created and configured. It's recommended that you replace this with your own favicon. To create a favicon, you can use an online favicon generator, like [this one](https://www.favicon-generator.org/). You will want to create or pick a square image. You can resize this image yourself down to 16x16, or let the website do it for you.

Once the favicon is generated, make sure it's named `favicon.ico` and replace the file of the same name with that one in the root directory of your website. Then, you're done!

## Changing the Navigation Icons

By default, comic\_git uses icons for the First, Previous, Next, and Latest comic links on every comic page. You can disable these icons in your comic\_info.ini and just use text links instead, or you can replace these icons with any images you want.

Just replace any of the images in `your_content/images/navigation_icons/` with ones you want, and they will be used when building the comic pages. make sure to provide both "enabled" and "disabled" images. The "enabled" images will be used when the links can be clicked, and the "disabled" images will be used when they cannot, like when you're at the first or last comic in the list.

## Other Changes

To make further changes to your website, like adding your own pages or changing the HTML of existing pages, see Themes.
