# Extra Features

## Adding Social Media Previews

Have you ever shared a link on Facebook, Twitter, Discord, etc. and a preview of the page appeared below the link? You can do the same thing with your comic!

The method of doing this is defined by the [Open Graph Protocol](https://ogp.me/). comic\_git provides some OGP data for every one of your pages by default. However, the one thing it can't automatically create for you is a preview image.

A preview image is the small image that shows up alongside the title and description of a link that shows up as part of the link preview, usually 200px wide and 200px high. To create your own preview image, create a file named `preview_image.png` and put it in the `your_content/images/` directory. If your preview image's dimensions are different than 200px by 200px, you must edit the following lines in the `base.tpl` file (follow [the instructions in the Themes section](themes.md#editing-existing-pages) on putting `base.tpl` in your theme directory):

```
    <meta property="og:image:width" content="200" />
    <meta property="og:image:height" content="200" />
```

You can see an example of how the preview of your comic will be presented by plugging your URL into [Facebook's Sharing Debugger](https://developers.facebook.com/tools/debug/sharing/).

## Google Analytics

comic\_git supports Google Analytics right out of the box! After you've signed up for [Google Analytics](https://analytics.google.com/) and generated the tracking ID for your webpage (looks like this: UA-123456789-0), you just need to add it to the `Tracking ID` option in the `[Google Analytics]` section of your `comic_info.ini` file.

Don't know what Google Analytics is or how to use it? [Google Analytics for Beginners](https://analytics.google.com/analytics/academy/course/6) has you covered!

## Transcripts

comic\_git includes support for transcripts on every comic page! Just drop your transcript in the form of a .txt file in your comic folder (except `post.txt`), and comic\_git will load the transcripts element on your comic page beneath your text post. Multiple languages are supported, with the filename of each .txt file showing up as a separate item in the transcripts list. The transcripts list on the comic page is sorted alphabetically.

For example, if you want to add an English transcript to your comic, you simply need to make a file named `English.txt` in the comic folder that contains the transcript text, and it will show up on the comic page when your website is built.

Transcript files support [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML syntax.

For more options, like disabling transcripts, changing the transcripts folder, and the default language, see the [Editing your Comic Info](../basic-editing/editing-your-comic-info.md#transcripts) page.
