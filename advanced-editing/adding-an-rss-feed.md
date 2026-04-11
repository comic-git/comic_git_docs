# Adding An RSS Feed

Many readers prefer to follow webcomics through an RSS feed. comic_git can build that feed for you automatically whenever your site updates.

## RSS Feed options

The `[RSS Feed]` section supports the following options:

| Option                       | Data type | Default                          | Notes                                                                                                                                                                                                                                          |
|------------------------------|-----------|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Build RSS feed`             | `boolean` | `False`                          | Turns RSS feed generation on or off for that comic. Extra Comics inherit this setting from the main comic unless they override it in their own `comic_info.ini`.                                                                               |
| `Description`                | `string`  | Comic's description              | A short description of your comic. This appears underneath the name of your comic in many feed readers. If you leave this out, comic_git will use the `Description` from that comic's `[Comic Info]` section.                                  |
| `Language`                   | `string`  | `en-us`                          | The language your comic is written in. For example, `en` for English or `en-US` for United States English. You can choose from [this list of language codes](http://backend.userland.com/stories/storyReader$16).                              |
| `Image`                      | `string`  | `your_content/images/banner.png` | The relative path to an image to represent your website in the feed. This is the feed's channel image, which many RSS readers show near the feed title or feed header. A small square image usually works best, but a banner can also be used. |
| `Image width`                | `integer` | `100`                            | The width of the image from the `Image` option above, in pixels. This helps define the displayed size of the feed's channel image in RSS readers that use it.                                                                                  |
| `Image height`               | `integer` | `36`                             | The height of the image from the `Image` option above, in pixels. This helps define the displayed size of the feed's channel image in RSS readers that use it.                                                                                 |
| `Newest first`               | `boolean` | `False`                          | Reverses the order of the RSS items so the newest comic pages appear first in the feed output.                                                                                                                                                 |
| `Combine with Main RSS Feed` | `boolean` | `False`                          | Advanced option. If `True`, that Extra Comic's posts appear in the main comic's root `feed.xml` instead of getting their own separate feed file. If set on the main comic, it becomes the default for Extra Comics.                            |
| `RSS title format`           | `string`  | none                             | Advanced option. Changes how page titles appear in the RSS feed. Supports `{comic_title}` and `{page_title}`.                                                                                                                                  |

### Edit your comic_info.ini file

Look for the `[RSS Feed]` section in your `comic_info.ini` file. It should look something like this:

```ini
[RSS Feed]
Build RSS feed = False
```

Set `Build RSS feed` to `True`.

The sample `comic_info.ini` keeps this section short on purpose, so some RSS settings use built-in defaults unless you add them yourself. You can use the table above as a reference for the other available options.

### Add a way for readers to subscribe

The most common way is to add a link in your Links Bar to the feed file, such as `RSS = /feed.xml`.

Another good option is to use an [RSS feed symbol](https://www.google.com/search?q=rss+feed+symbol) instead of the text `RSS`.

### Publish your comic

Once RSS is enabled, the feed file will be created the next time you build or update your site, and comic_git will keep it updated automatically alongside your comic.

If you use [Extra Comics](extra-comics.md), the rest of this page covers the additional RSS behavior available for them.

## Advanced: RSS feeds for Extra Comics

By default, every Extra Comic that has RSS enabled gets its own feed file.

Extra Comics also inherit the main comic's RSS settings unless you override them in that Extra Comic's own `comic_info.ini` file.

{% hint style="info" %}
These advanced RSS options may not yet appear in your default `comic_info.ini` file. That is normal. You can add them manually if you need them.
{% endhint %}

### Turn RSS on or off for each Extra Comic

If an Extra Comic has its own `comic_info.ini` file, you can override whether it builds an RSS feed by adding:

```ini
[RSS Feed]
Build RSS feed = True
```

or:

```ini
[RSS Feed]
Build RSS feed = False
```

If you do not add that line, the Extra Comic will inherit the main comic's `Build RSS feed` setting.

### Include an Extra Comic in the main feed

If you want Extra Comics to also appear in the main comic's root `feed.xml` by default, add this to the main comic's `comic_info.ini` file:

```ini
[RSS Feed]
Combine with Main RSS Feed = True
```

Extra Comics will inherit that setting unless they override it in their own `comic_info.ini` file.

If you want to control one Extra Comic individually, add this to that Extra Comic's `comic_info.ini` file:

```ini
[RSS Feed]
Combine with Main RSS Feed = True
```

or:

```ini
[RSS Feed]
Combine with Main RSS Feed = False
```

If an Extra Comic has `Build RSS feed = False`, then it will not appear in any RSS feed, even if `Combine with Main RSS Feed = True`.

If an Extra Comic has `Combine with Main RSS Feed = True`, it is included in the main comic's root feed instead of getting its own separate feed file.

### Where feed files are created

The feed file locations work like this:

* If the main comic has RSS enabled, its feed is built at `feed.xml`
* Every Extra Comic with RSS enabled gets its own feed at `<extra comic folder>/feed.xml`, unless it is combined into the main feed
* If an Extra Comic has `Combine with Main RSS Feed = True`, its posts appear in the main comic's root `feed.xml` instead of getting their own separate feed file

For example, if you have an Extra Comic in `stories/bonus`, its own feed will be built at:

```text
stories/bonus/feed.xml
```

If you want to add links to those extra feed files manually so readers can subscribe to each comic separately, see [Changing the Links Bar for your Extra Comic](extra-comics.md#changing-the-links-bar-for-your-extra-comic).

### Titles inside the main feed

Posts in the main comic's root feed keep their normal page titles by default.

Many creators will want to change the title format so readers can immediately tell which comic an update came from.

To do that, add an `RSS title format` to the main comic's `[RSS Feed]` section:

```ini
RSS title format = {comic_title}: {page_title}
```

With that setting, titles in the main comic's root feed might look like this:

```text
Main Comic: Page 12
Bonus Story: Page 12
```

You can customize the format however you like:

```ini
RSS title format = [{comic_title}] {page_title}
```

The available format values are:

* `{comic_title}`
* `{page_title}`

{% hint style="warning" %}
This title formatting only affects the main comic's root feed. On an Extra Comic's own feed, each feed already belongs to a single comic.
{% endhint %}

If you are still setting up your Extra Comic itself, including its own `comic_info.ini` and Links Bar, see [Extra Comics](extra-comics.md).
