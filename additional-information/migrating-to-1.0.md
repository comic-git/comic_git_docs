# Migrating to 1.0

If you've already installed and set up comic\_git, you **are not** required to update to 1.0 if you're happy with your site as is. Not doing so means you won't get access to further bugfixes or enhancements, but if you're okay with that, you don't need to do anything.

If you do want to migrate your site to 1.0, there are three methods to do so:

## Make a new repository

This is the quick and dirty way to upgrade, and may be most useful if you haven't made substantial customizations to comic\_git or built a readership that goes to the existing URL.

1. Follow the instructions in [Creating your own Repository](../getting-started/creating-your-own-repository.md) to make a new repo from comic\_git 1.0.
2. Copy your `your_content` and `favicon.ico` into the new repo.

You're done! This will create a new GitHub Pages URL while keeping your old one intact. You'll need to reset any repository settings or customizations.

## Remake your repository

This takes a little more effort, but is useful if you want to keep your previous repo name so your readership isn't disrupted by the migration.

1. Back up or move your `your_content` and `favicon.ico` files out of the repo directory.
2. On GitHub, go to your repo **Settings**, then scroll down to the **Danger Zone** and follow the instructions for deleting your repo.
3. Follow the instructions in [Creating your own Repository](../getting-started/creating-your-own-repository.md) to make a new repo from comic\_git 1.0 using the old name.
4. Move your `your_content` and `favicon.ico` back into the new repo.

You're done! This will recreate the GitHub Pages site with minimal effort. However, any customizations such as a custom domain or edited template files in `/src` will be lost and need to be redone.

## Surgical update (recommended)

This method takes a bit more finesse, but allows you to update your repo to 1.0 without losing any customizations.

1. Move any edited templates in `/src/templates/` into `/your_content/themes/default/templates/`.
2. Copy the text in the `.github/workflows/main.yaml` [file from the comic\_git repo](https://github.com/ryanvilbrandt/comic_git/blob/working/.github/workflows/main.yaml).
3. Open the `.github/workflows/main.yaml` file in your own repo and replace all the text in that with the text you copied from comic\_git. Save and close the file.
4. Delete `/src`.

You're done! This is the least destructive method, as it does not delete any repo settings, but does require that you keep track of customized files outside of `your_content`.
