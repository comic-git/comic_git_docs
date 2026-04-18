# Extra Features

## Google Analytics

comic\_git supports Google Analytics right out of the box! After you've signed up for [Google Analytics](https://analytics.google.com/) and generated the tracking ID for your webpage (looks like this: UA-123456789-0), you just need to add it to the `Tracking ID` option in the `[Google Analytics]` section of your `comic_info.ini` file.

Don't know what Google Analytics is or how to use it? [Google Analytics for Beginners](https://analytics.google.com/analytics/academy/course/6) has you covered!

## Transcripts

comic\_git includes support for transcripts on every comic page! Just drop your transcript in the form of a .txt file in your comic folder (except `post.txt`), and comic\_git will load the transcripts element on your comic page beneath your text post. Multiple languages are supported, with the filename of each .txt file showing up as a separate item in the transcripts list. The transcripts list on the comic page is sorted alphabetically.

For example, if you want to add an English transcript to your comic, you simply need to make a file named `English.txt` in the comic folder that contains the transcript text, and it will show up on the comic page when your website is built.

Transcript files support [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML syntax.

For more options, like disabling transcripts, changing the transcripts folder, and the default language, see the [Editing your Comic Info](../basic-editing/editing-your-comic-info.md#transcripts) page.
