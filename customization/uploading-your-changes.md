# Uploading your Changes

So you've made some changes to some files in your comic. Maybe you changed the comic name, or uploaded some comic files. You are ready to publish your changes to the web! This page will walk you through uploading any changes you've made to GitHub.

{% hint style="info" %}
**Note**

The examples on this page assume all you've done is change the name of your comic in your `comic_info.ini`. The pictures will differ slightly depending on what changes you've done, and how many files you've changed.
{% endhint %}

Open GitHub Desktop. You should notice that the main screen looks a little different now that you've made changes to one of the files in your repository. The left column on GitHub Desktop displays which files have been added, changed, or deleted. When you select a file (in this case, there's only one), it will show you what changes were made to that file.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/uploading\_your\_comic/ready\_to\_commit.png)\


The next step is to "commit" your changes. This converts your file changes into a little packet of data called a "commit" that will be uploaded to the server.

Make sure the checkboxes next to the files you want to commit are selected. You can select some or all of the files for each commit, so it's easy to split up files between different commits.

{% hint style="info" %}
**Tip**

You can choose to discard any changes you didn't intend by right-clicking the changed file in the left column and clicking **Discard changes...** This will revert that particular file to what its state was before you made your changes.
{% endhint %}

Type the name of the commit you're about to make in the lower left. You may name your commit anything you like. It is not used in the code anywhere, but making your commit names descriptive means you can easily look back in your history and see what you've done in the past at a glance.

Once named, you may also add a description if you like (but it is not required).

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/uploading\_your\_comic/named\_commit.png)\


When you are satisfied, click **Commit to working**. You can go to the History tab to see all your past commits, including the one you just created. The up arrow next to it means it hasn't been uploaded to GitHub yet.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/uploading\_your\_comic/committed.png)\


Click **Push origin**. This is the action that actually uploads your changes to GitHub.

Once this is complete, your website should now be updated! Go to **https://\[username].github.io/\[repo-name]/** to view your updated comic, and see that the title has been updated!

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/uploading\_your\_comic/checking\_website.png)\


Sometimes it takes a few minutes for the changes to appear, so just be patient.

You can check the progress of your deployment to GitHub Pages. Though even after that finishes, you may need to clear your browser cache to see the change. The easiest way to do this is with a force refresh (Ctrl+F5), or you may need to clear the cache in your browser's settings, or even just wait a minute for the cache to expire and your browser decides to pull the new changes down.

{% hint style="info" %}
**Publishing Changes**

Every time comic\_git runs, it regenerates your website. It runs automatically whenever you upload changes to GitHub, but it also runs every morning at 8am UTC. See Scheduled Posts for more information.
{% endhint %}
