# Adding Comic Pages

Uploading comic pages to your website is as simple as copying the comic image file to a folder and editing a text file!

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/uploading_your_comic/your_content_dir.png" alt=""><figcaption></figcaption></figure>

Open the `comics` directory. When you created your own comic\_git repository in [Getting Started](../getting-started/getting-started.md), you created a copy of the original repository. This includes some example comic pages to help you get started.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/adding_comic_pages/comic_dir.png" alt=""><figcaption></figcaption></figure>

The easiest way to upload a new comic page is to first make a copy of an existing directory in `your_content/comics`. After you've done that, you can delete the remaining example directories.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/adding_comic_pages/new_dir.png" alt=""><figcaption></figcaption></figure>

You can name the directory whatever you want, but be aware that it will show up in the URL for that page. E.g. **https://\[username].github.io/\[repo name]/comic/Page 1/**

Open the new directory you've created, and you should see a few files: `info.ini`, `post.txt`, and an image file.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/adding_comic_pages/comic_files.png" alt=""><figcaption></figcaption></figure>

## Comic File

First things first, delete the existing image file and copy whatever image file you want to use for your comic in its place. The name of your comic image file can be anything you want, even the same name as other comic image files in other folders.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/adding_comic_pages/new_comic_file.png" alt=""><figcaption></figcaption></figure>

When building your website, comic\_git will search through your comic folders for any image files in the same folder and add them to the web page for that folder. The images will be shown vertically in alphabetical order, according to their filenames.

Files with any of the following extensions are considered image files: `jpg`, `jpeg`, `png`, `tif`, `tiff`, `gif`, `bmp`, `webp`, `webv`, `svg`, `eps`

You can override this behavior by adding a `Filenames` option to your `info.ini` file, as described in the next section.

## Page Info

Next, open `info.ini`. The file will look something like this:

```
Title = Page 197
Post date = November 27, 2019
Alt text = Tamberlane, can you sign "ongoing trauma"?
Storyline = Chapter 4a
Characters = Avery, Belfry, Cur, Piper, Tamberlane
Tags = Tag 1, Tag 2, Tag 3
```

Edit the values in this file to match the comic you are uploading.

<details>

<summary>Title</summary>

* Required
* Value: `string`: page title
* Example: `Page 197`

The title of this particular comic page.  The page title shows up in the tab every time a page from your website is loaded along with the comic name (for example, **Page 197** - comic\_git Example). It also appears in the info box below the comic on the page itself.

</details>

<details>

<summary>Post date</summary>

* Required
* Value: `string`: date comic is posted, matching date format
* Example: `November 27, 2019`

The date and/or time your comic is posted. This should match the format defined in your `comic_info.ini` file, as described in [Editing your Comic Info](editing-your-comic-info.md#date-format). If you have not changed that option in your `comic_info.ini`, just use the same format already in the file.

{% hint style="warning" %}
If you're using the default date format, don't forget the comma after the day!
{% endhint %}

{% hint style="info" %}
**Scheduled Posts**

Any comic with a Post Date set in the future (according to the Timezone you have set in your comic\_info.ini file) will be "scheduled" for later, meaning it will not be published at that point in time. By default, comic\_git automatically reruns every morning at 8am UTC to publish any scheduled posts that might need to be created. See the [Scheduled Posts](../other-expert-tips.md#scheduled-posts) section for more information, including how to change when comic\_git rechecks the scheduled posts.
{% endhint %}

</details>

<details>

<summary>Filenames</summary>

* Optional
* Value: `string`: list of filenames for the comic images separated by commas
* Example: `Page 197a.png, Page_197b.png`

If this option is present in the info.ini file, comic\_git will not auto-collect images from the folder but will instead use the files defined here. This is useful if you want the images displayed not in alphabetical order, or you want to display only some of the images in this folder.

{% hint style="warning" %}
The filenames are case sensitive, so be sure to write them in exactly as the files are named!
{% endhint %}

</details>

<details>

<summary>Alt text</summary>

* Optional (but recommended)
* Value: `string`: alt text
* Example: `Tamberlane, can you sign "ongoing trauma"?`

The text that should show up when the user hovers their mouse over the comic image. This is generally recommended for accessibility purposes, but is not required.

</details>

<details>

<summary>Storyline</summary>

* Optional
* Value: `string`: storyline to attach this page to
* Example: `Chapter 4a`

The name of the current chapter, book, section, or whatever else you use to separate out different parts of your webcomic. This is used when building the Archive page and Infinite Scroll page. If this option is blank, this page will count as not having a storyline and won't show up on the Archive page.

</details>

<details>

<summary>Characters</summary>

* Optional
* Value: `string`: list of comic characters separated by commas
* Example: `Avery, Belfry, Cur, Piper, Tamberlane`

A comma-separated list of characters on this page. Any character names here will turn into a hyperlink which links to a list of pages with that character in them.

</details>

<details>

<summary>Tags</summary>

* Optional
* Value: `string`: list of tags separated by commas
* Example: `Tag 1, Tag 2, Tag 3`

A comma-separated list of non-character tags. Any tags here will turn into a hyperlink which links to a list of pages with that tag attached to them.

{% hint style="danger" %}
**Invalid Tag Names**

Due to the way comic\_git generates pages for tags and characters, there are some limitations on what characters you can use in your tags and character lists in the info.ini file. Please avoid using any of the following: `\ / : * ? " < > |`

As of comic\_git 0.3.6, you can however include unicode in your info.ini files. If you look around, you can find some good unicode options to take the place of those characters if you need them.
{% endhint %}

</details>

When you're done, save and close `info.ini`.

## News Post

Open `post.txt` in a text editor like Notepad. The file will look something like this:

```
WHOOPS! Caught red-pawed! ...and poor Tamberlane is having a heck of a time today, isn’t she? 🙁

Thanks again to Chaon for letting me use Cur in his Patreon cameo!
```

This is the file where you put any text that accompanies your comic upload, such as a news post update or a few snarky comments. This file supports both [Markdown](https://daringfireball.net/projects/markdown/syntax) and HTML formatting, including CSS or JavaScript.

{% hint style="info" %}
All paragraphs must have two line breaks (i.e. a blank line) between them. Single line breaks will be ignored and converted into spaces.
{% endhint %}

You can even embed images in this page, like so:

```
Sorry, guys, I'm sick so no comic today. Please enjoy a sad panda instead.

<img src="../../your_content/images/sad_panda.png">
```

And you're done! You've now created your first comic page! If you like, you can upload your changes now and see them on the web. Or if you prefer, you can first spend some time changing the colors, images, and other layout of your website.

Adding more pages in the future is as easy as copying the comic folder, renaming it, and following the same steps as above. If you wish to make changes to the comic after it's been posted, simply edit the image file in the folder. If you wish to edit the post or page info of any comic, just edit `info.ini` or `post.txt`. If you wish to delete a comic, just delete the whole folder.
