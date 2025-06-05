# Adding An RSS Feed

Many users prefer to read their web comics through an RSS feed. Building an RSS feed as part of publishing your comic is simple.

### Edit your comic\_info.ini file

Look for the \[RSS Feed] section in your `comic_info.ini` file. It should look something like below:

```
[RSS Feed]
Build RSS feed = False
Description = Explore a free webcomic-based static site generator delivered through GitHub!
Language = en-us
Image = your_content/images/banner.png
Image width = 100
Image height = 36
```

Set `Build RSS Feed` to `True`. Fill out all the other fields as needed:

* **Description:** A short description, no more than a few sentences. This will appear underneath the name of your comic in RSS feed readers.
* **Language:** The language your comic is written in, usually the two-letter code for your language. For example, "English" would be "en", or "United States English" for "en-US". Select from [this list of acceptable language codes](http://backend.userland.com/stories/storyReader$16).
* **Image:** The relative path to an image you want to represent your website. A banner can do, but something small around 100x100 px is best.
* **Image width:** The width of the image defined in the Image option above.
* **Image height:** The height of the image defined in the Image option above.

### Add a way for users to subscribe to your feed

The most common way is to add a link in your Links Bar to the feed.xml file. The text can be simply `RSS` or a [RSS feed symbol](https://www.google.com/search?q=rss+feed+symbol).

### Publish your comic

And you're done! Your RSS feed file will be created the next time you update your comic, and it will be kept automatically updated alongside your comic.
