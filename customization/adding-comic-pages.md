# Adding Comic Pages

Uploading comic pages to your website is as simple as copying the comic image file to a folder and editing a text file!

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/uploading\_your\_comic/your\_content\_dir.png)\


Open the `comics` directory. When you created your own comic\_git repository in Getting Started, you created a copy of the original repository. This includes some example comic pages to help you get started.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/adding\_comic\_pages/comic\_dir.png)\


The easiest way to upload a new comic page is to first make a copy of an existing directory in `your_content/comics`. After you've done that, you can delete the remaining example directories.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/adding\_comic\_pages/new\_dir.png)\


You can name the directory whatever you want, but be aware that it will show up in the URL for that page. E.g. **https://\[username].github.io/\[repo name]/comic/Page 1/**

Open the new directory you've created, and you should see a few files: `info.ini`, `post.txt`, and an image file.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/adding\_comic\_pages/comic\_files.png)\


## Comic File

First things first, delete the existing image file and copy whatever image file you want to use for your comic in its place. The name of your comic image file can be anything you want, even the same name as other comic image files in other folders.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/adding\_comic\_pages/new\_comic\_file.png)\


## Page Info

Next, open `info.ini`. The file will look something like this:

```
Title = Page 197
Post date = November 27, 2019
Filename = Page_197.png
Alt text = Tamberlane, can you sign "ongoing trauma"?
Storyline = Chapter 4a
Characters = Avery, Belfry, Cur, Piper, Tamberlane
Tags = Tag 1, Tag 2, Tag 3
```

Edit the values in this file to match the comic you are uploading.

* Set `Title` to be the title of this particular comic page, as it will show up on your website page itself.
* Set `Post date` to be the date and/or time your comic is posted. This should match the format defined in your `comic_info.ini` file, as described in Editing your Comic Info. If you have not changed that option in your `comic_info.ini`, just use the same format already in the file. Note that if you set the Post date to sometime in the future, comic\_git will not publish the page until that day comes around.
* Set `Filename` to be the filename of the comic image you just copied into this folder, without any of the directory path, but with the file extension.
* Set `Alt text` to be the text that should show up when the user hovers their mouse over the comic image. If you don't want alt text, you can leave this value blank.
* _Optional:_ Set `Storyline` to the name of the current chapter, book, section, or whatever else you use to separate out different parts of your webcomic. This is used when building the Archive page and Infinite Scroll page. If you delete this option or leave it blank, this page will just count as not having a storyline and won't show up on the Archive page.
* _Optional:_ Set `Characters` to be a comma-separated list of characters on this page. Any character names here will turn into a hyperlink which will link to a list of pages with this character in them. You can delete or leave this option blank if you prefer.
* _Optional:_ `Tags` works exactly like `Characters` but shows up in a different list on the comic page, so you may keep lists of characters on a page separate from miscellaneous tags, if you prefer.

When you're done, save and close `info.ini`.

{% hint style="info" %}
**Scheduled Posts**

Any comic with a Post Date set in the future (according to the Timezone you have set in your comic\_info.ini file) will be "scheduled" for later, meaning it will not be published at that point in time. By default, comic\_git automatically reruns every morning at 8am UTC to publish any scheduled posts that might need to be created. See the Scheduled Posts section for more information, including how to change when comic\_git rechecks the scheduled posts.
{% endhint %}

{% hint style="danger" %}
**Invalid Tag Names**

Due to the way comic\_git generates pages for tags and characters, there are some limitations on what characters you can use in your tags and character lists in the info.ini file. Please avoid using any of the following: `\ / : * ? " < > |`

As of comic\_git 0.3.6, you can however include unicode in your info.ini files. If you look around, you can find some good unicode options to take the place of those characters if you need them.
{% endhint %}

## News Post

Open `post.txt` in a text editor like Notepad. The file will look something like this:

```
WHOOPS! Caught red-pawed! ...and poor Tamberlane is having a heck of a time today, isn’t she? 🙁

Thanks again to Chaon for letting me use Cur in his Patreon cameo!
```

This is the file where you put any text that accompanies your comic upload, like a few snarky comments, or perhaps a news post update. This file supports both [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML formatting including any CSS or Javascript you may want to add.

Note that all paragraphs must have two line breaks (i.e. a blank line) between them. Single line breaks will be ignored and converted into spaces.

You can even embed images in this page, like so:

```
Sorry, guys, I'm sick so no comic today. Please enjoy a sad panda instead.

<img src="../../your_content/images/sad_panda.png">
```

And you're done! You've now created your first comic page! If you like, you can upload your changes now and see them on the web. Or if you prefer, you can first spend some time changing the colors, images, and other layout of your website.

Adding more pages in the future is just as easy as copying the comic folder, renaming it, and following the same steps as above. If you wish to make changes to the comic after it's been posted, simply edit the image file in the folder. If you wish to edit the post or page info of any comic, just edit `info.ini` or `post.txt`. If you wish to delete a comic, just delete the whole folder.
