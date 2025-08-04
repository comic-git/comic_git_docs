# Migrating to 1.0

If you've already installed and set up comic\_git, you **are not** required to update to 1.0 if you're happy with your site as is. Not doing so means you won't get access to further bugfixes or enhancements, but if you're okay with that, you don't need to do anything.

If you do want to migrate your site to 1.0, there are three methods to do so:

### Make a new repository (recommended for basic users)

This is the quick and dirty way to upgrade, and may be most useful if you haven't made substantial customizations to comic\_git or built a readership that goes to the existing URL.

1. Follow the instructions in [Creating your own Repository](../getting-started/creating-your-own-repository.md) to make a new repo from comic\_git 1.0.
2. Copy your `your_content` and `favicon.ico` into the new repo.

You're done! This will create a new GitHub Pages URL while keeping your old one intact. You'll need to reset any repository settings or customizations.

### Remake your repository (recommended for intermediate users)

This takes a little more effort, but is useful if you want to keep your previous repo name so your readership isn't disrupted by the migration.

1. Back up or move your `your_content` and `favicon.ico` files out of the repo directory.
2. On GitHub, go to your repo **Settings**, then scroll down to the **Danger Zone** and follow the instructions for deleting your repo.
3. Follow the instructions in [Creating your own Repository](../getting-started/creating-your-own-repository.md) to make a new repo from comic\_git 1.0 using the old name.
4. Move your `your_content` and `favicon.ico` back into the new repo.

You're done! This will recreate the GitHub Pages site with minimal effort. However, any customizations such as a custom domain or edited template files in `/src` will need to be redone.

### Surgical update (recommended for advanced users)

This method takes a bit more finesse, but allows you to update your repo to 1.0 without losing any customizations.

1. Move any edited files in `/src` into `/your_content/themes/default` .
   1. For example, move any edited templates from `/src/templates` to `/your_content/themes/default` .
2. Copy the text in the `.github/workflows/main.yaml` [file from the comic\_git repo](https://raw.githubusercontent.com/ryanvilbrandt/comic_git/refs/heads/working/.github/workflows/main.yaml).
3. Open the `.github/workflows/main.yaml` file in your own repo and replace all the text in that with the text you copied from comic\_git. Save and close the file.
4. Delete your `/src` folder.
5. Add a `.nojekyll` file to the root of your repo (i.e., alongside `favicon.ico`).
6. Go to your GitHub Pages settings and change your "Build and deployment" source to "GitHub Actions".

You're done! This is the least destructive method, as it does not delete any repo settings, but does require that you keep track of customized files outside of `your_content`.

However, for any customized files you may have, you will need to update per the instructions in the next section.

## What Has Changed in 1.0?

While the above options describe different ways to upgrade to 1.0, but if you have heavily customized your website (e.g., by making a [Theme](../advanced-editing/themes.md) with custom templates), you will need to be aware of the changes to some of the under-the-hood behavior of comic\_git to make sure your templates all work with the new version of the engine.

### Template File Changes

This is the list of changes to the names of HTML elements and Jinja variables. Update these in any custom templates you may have.

1. `#page-title` is now `#post-title`&#x20;
2. The following attributes for each `page` object have changed. These attributes are used by default in the comic and archive templates.
   1. `page_title` -> `_title`&#x20;
   2. `post_date` -> `_post_date`&#x20;
   3. `storyline` -> `_storyline`
   4. `characters` -> `_characters`&#x20;
   5. `alt_text` -> `escaped_alt_text`
      1. This one is not strictly required, but this way nonstandard characters in your alt text will render properly.
3. Comic pages now support a list of comic images, arranged in alphabetical order. `comic_path`  as a single filepath has been replaced by `comic_paths` as a list of filepaths. If you plan to only ever have one comic image per page, you can replace any use of `comic_path` in your templates with `comic_path[0]`. Otherwise, you should use a for loop to iterate through all the images properly. See the [comic.tpl file in comic\_git\_engine](https://github.com/ryanvilbrandt/comic_git_engine/blob/1.0/templates/comic.tpl#L19) for an example of how to do that.
4. `navigation-bar` is now a class rather than an ID.
5. All references to files in your `src` directory should now either point to files in `comic_git_engine` or you should move those files to your [Theme](../advanced-editing/themes.md) directory.
   1. For example, script elements that load the module `/src/js/infinite_scroll.js` should instead load `/comic_git_engine/js/infinite_scroll.js`&#x20;

### Comic Info Changes

This is a list of changes to your [comic\_info.ini file](../basic-editing/editing-your-comic-info.md) that are required for the 1.0 version of comic\_git to work properly.

1. [Engine Version](https://comic-git.gitbook.io/documentation/basic-editing/editing-your-comic-info#engine-version) is a required value.
2. "Use images in navigation bar" has been moved to "Use images" in the [Navigation Bar](https://comic-git.gitbook.io/documentation/basic-editing/editing-your-comic-info#navigation-bar) section.

### Code Hooks

Some [code hooks](https://comic-git.gitbook.io/documentation/other-expert-tips#code-hooks) will require an update if you have them defined in your hooks.py file.

1.  `postprocess` now takes three arguments:&#x20;

    ```
    comic_info, comic_data_dicts, global_values
    ```

### Other Changes

This section describes changes that don't fall into any of the other sections describes above.

1. Your social media preview image is defined by default to be 200px by 200px. If you've changed these values in your own custom templates, you can ignore this.
2. Various changes and fixes have been made to the Javascript and CSS files that were in `src`, and are now in `comic_git_engine`. If you have made changes to any of these files, I recommend checking out their [current version in comic\_git\_engine](https://github.com/ryanvilbrandt/comic_git_engine/tree/1.0) and recreate those files with your changes in your [Theme](../advanced-editing/themes.md) directory.
