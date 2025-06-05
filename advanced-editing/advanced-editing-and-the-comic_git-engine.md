# Advanced Editing and the comic\_git Engine

comic\_git is designed to be as easy as possible for webcomic creators to set up their own free comic site. However, to do this, many of the advanced features are hidden so as not to overwhelm those who just need a place to post their comic.

If you're comfortable digging deeper into the guts of comic\_git, it is possible to open up these features and adjust them for your site. Some of these features just require adding additional folders, files, and config options, like adding [extra comics](extra-comics.md) and [RSS feeds](adding-an-rss-feed.md), but some (especially in [Expert Editing](broken-reference)) require you to copy over files from the comic\_git\_engine repository.

## What is comic\_git\_engine?

[comic\_git\_engine](https://github.com/ryanvilbrandt/comic_git_engine) is where the actual work gets done that turns the files in your\_content into a fully-fledged website. The setup you did in [Getting Started](../getting-started/getting-started.md) allows your comic repo to call comic\_git\_engine, and allows comic\_git\_engine in turn to make the necessary changes to your repo.

Because comic\_git\_engine needs to be able to work for all comic\_git users, you can't modify it directly. We also don't recommend trying to clone it or add files from it to your own repo unnecessarily. If you run your own version of comic\_git\_engine, then as it gets updated with new features and bugfixes, your copy will not receive those.

However! Copying **select** files from comic\_git\_engine and modifying them as directed in some of the following pages will allow you greater control over the customization of your website. Any files placed in your repo will override those in comic\_git\_engine, so long as they are placed in the right folder and named appropriately. When this is needed, the documentation will guide you through the proper steps.
