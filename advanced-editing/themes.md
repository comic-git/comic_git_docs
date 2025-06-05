# Themes

The default layout and features of comic\_git are intended to appeal to as many people as possible, but if you really get into customizing your website to fit your needs, you'll probably want to modify the pages themselves, or even add extra pages. This is where themes come in.

comic\_git has a default theme, which can be found at `/your_content/themes/default/`. Initially, this folder contains just the `stylesheet.css` and `fonts.css` files in the `css` directory, which defines things like the colors and fonts of your website. However, this folder can be used to make even bigger changes to your website.

### Editing Existing Pages

Let's say you wanted to edit the layout of your comic pages. Perhaps to do something like... I don't know... put a Hypno Toad gif between the Previous and Next links. Whatever, man, it's your web comic. Do what you want.

To do this, first create a templates folder in `/your_content/themes/default/`.

Go to [comic\_git\_engine's templates folder](https://github.com/ryanvilbrandt/comic_git_engine/tree/master/templates) and find the file you want to change. For comic pages it's `comic.tpl`, for the archive page it's `archive.tpl`, and so on. Open the dropdown for the full list of templates if you aren't sure which one you need.

<details>

<summary>Available templates</summary>

* [404.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/404.tpl) is the layout of the "404 Not Found" page that appears if someone tries to access a page on your site that doesn't exist.
* [archive.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/archive.tpl) is the layout of the Archives page.
* [base.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/base.tpl) is the base template which all the other templates use. If something you want to change is present across the entire site, it's probably in here.
* [comic.tp](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/comic.tpl)l is the layout of the individual comic pages.
* [index.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/index.tpl) is the layout of the home page. If you want to change the layout of this page, use the `home page.txt` file in `your_content` instead.&#x20;
* [infinite\_scroll.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/infinite_scroll.tpl) is the layout for the infinite scroll comics page.
* [latest.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/latest.tpl) is the layout for the latest comic page. This differs slightly from the individual comic pages in that comic\_git will always update this one to show the most recent (by post date) comic.
* [md\_page.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/md_page.tpl) is the layout for any .md (Markdown) files present on the site.
* [tagged.tpl](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/templates/tagged.tpl) is the layout of the page that displays all comic pages a clicked character or tag appears in.

</details>

Copy the file(s) you want to change into `/your_content/themes/default/templates/`. You can then freely edit the template file, and it will override the file in comic\_git\_engine. You can use any text editor to edit templates.

### Jinja Template Format

Template files are written in a format called [Jinja](https://jinja.palletsprojects.com/en/2.11.x/templates/). Jinja is an incredibly powerful tool that lets you create files that can be converted into HTML files using code like for loops, if/else blocks, and variables acting as placeholders for values passed in by the comic\_git script. I've done my best to comment the existing template files in the comic\_git\_engine templates directory to help clarify how to use Jinja for anyone curious enough to go poking around.

I recommend using Jinja templates whenever you're making pages for your website, as they provide a lot of flexibility and power that you won't get from normal HTML pages. For more information on the details of Jinja and the variables available to your templates when they're built, see Advanced Tips.

### Alternative HTML Format

If you prefer to not mess with Jinja templates for your first attempt at editing your website, that's fine, too! comic\_git also allows plain HTML files. Instead of creating a \*.tpl file in the `/your_content/themes/default/templates/` directory, make a .html file instead. The easiest way to start is to go to the page you want to edit in your web browser, right click on the page, go to **View Source**, and just copy all that text into the \*.html file.

Creating HTML files is **NOT** recommended for most pages that have content that will change as you update the rest of your website, like comic pages, the archive page, and tagged pages. Jinja template files are required to keep these pages up to date whenever the website is built. However, using HTML files will usually be just fine when adding new pages to your website that don't come pre=packaged by default in comic\_git.

### Adding New Pages

Adding new pages to your website beyond what's already provided by comic\_git is very similar to editing existing pages, with a few small changes. You'll usually want to start with copying an existing template or HTML file to use as a starting point, but give it its own unique name. You will also want to put it in the `/your_content/themes/default/templates/` directory. Then, you will need to add the file name to the \[Pages] section of your [comic\_info.ini](../basic-editing/editing-your-comic-info.md#pages) file. For example, if you're creating a `cast.html` file, you should add the line `cast = Cast Page`.

If you need to create a large number of pages that can change every time the website is built, like a cast page for each character, you may want to make use of the `build_other_pages` code hook. See Code Hooks in Expert Editing for more information.

### Creating Your Own Themes

You may eventually want to create your own theme from scratch, rather than editing the default one. comic\_git is designed so that you can easily switch between themes without having to delete or edit existing files.

To do so, make a copy of the `default` theme directory, change the name to whatever you like, and edit the contents in there as described in the previous two sections.

In comic\_info.ini, add a line under [\[Comic Settings\]](../basic-editing/editing-your-comic-info.md#comic-settings) that says `Theme =` followed by the folder name for your desired theme. An example `geocities` theme has already been created, and you can check it out by adding the line `Theme = geocities` to \[Comic Settings] and rebuilding your webpage.

Themes can also be shared between different comic\_git powered websites! As long as all the files required for a theme to work are somewhere in the theme directory, you can just send the whole theme directory to a friend who's also using comic\_git, and they can try out your theme on their own site!
