# Extra Comics

One frequently requested feature is the ability to support multiple comics on one website. comic\_git supports that with the "Extra Comics" feature! This can be used for hosting your main comic and side comics, or even setting up things like a fanart galley. An "extra comic" will function exactly like the main comic, with all the same features available for you to make use of. It can be themed, designed, and managed completely independently from your main comic, while still hosted in the same repository.

### Adding Extra Comic Pages

To create an extra comic, create the folder for your extra comic in the `your_content` directory. You can name this folder whatever you want, but keep in mind that the name will define what the URL for your extra comic will be. For this example we'll use `sidestory` as the folder name.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/add_sidestory.png" alt=""><figcaption></figcaption></figure>

Create a `comics` folder within that directory.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/add_sidestory_comics_folder.png" alt=""><figcaption></figcaption></figure>

Create the individual comics folders within `comics` as described in [Adding Comic Pages](../basic-editing/adding-comic-pages.md).

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/add_sidestory_individual_comics.png" alt=""><figcaption></figcaption></figure>

Once the comic is set up, open up the `comic_info.ini` file. Under [\[Comic Settings\]](../basic-editing/editing-your-comic-info.md#comic-settings), add the line `Extra comics =`  followed by the folder name for your extra comic. This is case-sensitive, so be sure not to add random capital letters. The below image shows the example `Extra comics = sidestory`.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/add_sidestory_to_comic_info.png" alt=""><figcaption></figcaption></figure>

You may also want to add a link to your extra comic from the links bar. Under \[Links Bar], add a line `<Name of comic> = <name of comic folder>`. The below image shows the example `My Side Story = sidestory`.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/add_sidestory_link.png" alt=""><figcaption></figcaption></figure>

And you're done! Push your changes to GitHub, and your extra comic will be available to browse!

To start with, the only pages published for your extra comic are the comic pages themselves. No home page, no archive page, etc. That means when you link to your extra comic, you'll want to link directly to the comic pages. So, for example, linking to the first page in the extra comic in the images above would be `/sidestory/comic/001` or `http://<your username>.github.io/<your repo name>/sidestory/comic/001/#comic-page`.

### Adding More Pages to your Extra Comic

One of the first things you'll probably want to do is add a home page to your extra comic, and possibly an archive page as well, so people can browse it just like your main comic. To do that, you'll need to add a `comic_info.ini` file to your extra comics folder. If you like, you can copy the `comic_info.ini` file from your main comic into this folder to start.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_comic_info.png" alt=""><figcaption></figcaption></figure>

Next, open the `comic_info.ini` file in a text editor and add a [\[Pages\]](../basic-editing/editing-your-comic-info.md#pages) section, along with the pages you want your extra comic to support.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_comic_info_pages.png" alt=""><figcaption></figcaption></figure>

If you're adding an index page, as in the above example, and you haven't set up a custom `index.tpl` template file, you'll need to put a `home page.txt` file in your extra comic folder as well, just like your main comic.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_home_page.png" alt=""><figcaption></figcaption></figure>

### Changing the Links Bar for your Extra Comic

You may have noticed at this point that the Links Bar above your comic but below the comic banner is the same as on your main website. You may want a links bar that's unique for your extra comic. If you do, setting that up is easy. Just edit the `comic_info.ini` file for your extra comic to have its own Links Bar section.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_comic_info_links_bar.png" alt=""><figcaption></figcaption></figure>

All links are relative to your `your_content folder`, so to link to pages in your extra comic, you will need to start the link with the extra comic's directory name. Using the `sidestory` example, your main comic's archive would be `/archive/` while your extra comic's archive would be `/sidestory/archive/`.

### Changing your Extra Comic's Settings

By default, your extra comics inherit all the settings from your main comic. This includes all the settings found in the `comic_info.ini` file, like whether or not to use thumbnails for your archive page, the date formats, and your Google Analytics ID.

If you want to change any of these values for your extra comic, you just need to add that line to your extra comic's `comic_info.ini` file.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_comic_info_archive_setting.png" alt=""><figcaption></figcaption></figure>

### Changing your Extra Comic's Theme

In addition to changing the comic\_git specific settings of your extra comic, you may want to change things like the colors or font of your extra comic, to differentiate it from your main comic. To do so, you'll want to create a new theme per the [Themes](themes.md#creating-your-own-themes) page, then assign that theme to your extra comic in the `comic_info.ini` file.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/extra_features/sidestory_comic_info_theme.png" alt=""><figcaption></figcaption></figure>

### Multiple Extra Comics

comic\_git supports multiple extra comics! Just create multiple folders, one for each extra comic, as described in this section. Then add all the folder names to the "Extra comics" option in a comma separated list, like so: `Extra comics = sidestory, backstory, fanart_gallery`
