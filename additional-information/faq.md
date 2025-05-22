---
description: Frequently Asked Questions
---

# FAQ

## Table of Contents

1. [I've installed GitHub Desktop. How do I use it to organize my comic files?](faq.md#ive-installed-github-desktop.-how-do-i-use-it-to-organize-my-comic-files)
2. [I pushed my comic update to GitHub, but it's not showing up on my GitHub Page.](faq.md#i-pushed-my-comic-update-to-github-but-its-not-showing-up-on-my-github-page.)
3. [How do I test my website before publishing it on GitHub Pages?](faq.md#how-do-i-test-my-website-before-publishing-it-on-github-pages)
4. [How do I change my comic URL/my GitHub username/the name of my webcomic repository?](faq.md#how-do-i-change-my-comic-url-my-github-username-the-name-of-my-webcomic-repository)
5. [I'm reading through comic\_git's template files trying to understand how it works, but these files are too confusing!](faq.md#im-reading-through-comic_gits-template-files-trying-to-understand-how-it-works-but-these-files-are-t)
6. [I want my webcomic's home page to point to the latest comic. How do I change that?](faq.md#i-want-my-webcomics-home-page-to-point-to-the-latest-comic.-how-do-i-change-that)
7. [Are there any size/traffic limits on comic\_git?](faq.md#are-there-any-size-traffic-limits-on-comic_git)
8. [Can I host smut/classy erotica/get-rich-quick scheme/something illegal with comic\_git?](faq.md#can-i-host-smut-classy-erotica-get-rich-quick-schemes-something-illegal-with-comic_git)
9. [I just realized that people can see pages before they are posted to my website by going to my GitHub repository! Can I make my GitHub repository private and still publish my website to GitHub Pages?](faq.md#i-just-realized-that-people-can-see-pages-before-they-are-posted-to-my-website-by-going-to-my-github)

## I've installed GitHub Desktop. How do I use it to organize my comic files?

GitHub Desktop is only used to publish your changes to GitHub, and thus to your website. All organization of your files will happen in the file browser on your PC (for Windows, that's File Explorer). However, comic\_git makes this organization easy! Every comic page has its own folder, and every folder only needs a few files. See Adding Comic Pages for more information.

## I pushed my comic update to GitHub, but it's not showing up on my GitHub Page.

Your first step should be to check for errors when building your website. Then, check if your changes have been deployed to GitHub Pages yet. Finally, you may just need to force refresh your browser to see the changes.

## How do I test my website before publishing it on GitHub Pages?

The easiest way is to build your website on your own computer. You can then view that website on your own computer as if it were a full webpage.

## How do I change my comic URL/my GitHub username/the name of my webcomic repository?

Changing your Website URL has you covered.

## I'm reading through comic\_git's template files trying to understand how it works, but these files are too confusing!

The templates in `src/templates/` make use of a somewhat advanced feature of Jinja2 that lets them "extend" the base.tpl template. This makes it easier for me to manage and update these files for core comic\_git, but can make them difficult for Jinja2 novices to understand and edit if they want to make more complicated changes to their website than the base comic\_git functions allow.

I've included a simplified version of the comic.tpl file at `src/extras/comic_minimal.tpl` for anyone who wants to edit their comic page and start from a cleaner slate. This same file can also be used to create other custom pages by replacing everything in the `container` div and writing either plain HTML or your own Jinja2 template. Though if you do that, I request that you please keep the `powered-by` div at the bottom so people know how to make their own cool web comics just like you did. 😃

## I want my webcomic's home page to point to the latest comic. How do I change that?

I've created a theme for this, so it is very easy to switch! Just change the Theme setting in your `comic_info.ini` file to `homepage_is_latest_comic` and you're done! If you've edited the `default` or have created your own theme, just copy the file at `templates/index.tpl` in the `homepage_is_latest_comic` theme folder to a similar place in the theme you're using, e.g. `default/templates/index.tpl`.

## Are there any size/traffic limits on comic\_git?

comic\_git itself is highly performant. For a small webcomic, it will build the whole site in less than a second. For a major webcomic with over 200 pages, its build time has been known to range from 1 second to 10 seconds, depending on computer load at the time. In terms of serving the website, that is entirely reliant on the abilities of the web server comic\_git is hosted on.

However, GitHub Pages has [limits](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#usage-limits) on how big and how popular your website can get. It's recommended you read their terms of use, but in short your website should stay under 1 GB in size, and shouldn't serve more than 100 GB of data per month. If you go over those limits, my experience shows that GitHub will not bring down the hammer on you right away, but they may eventually reach out to you to discuss the situation. In the worst case, they will reserve the right to close down your repository. If that happens, fear not, because there are other options for how to host comic\_git.

## Can I host smut/classy erotica/get-rich-quick schemes/something illegal with comic\_git?

I cannot stop you from publishing any content you want with comic\_git. Whatever you do with my software is your own choice, and I promise no liability or responsibility on my part for your actions while using it.

GitHub Pages' terms of use are a little more restrictive, however. [Their documentation goes into more detail here](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#prohibited-uses), but porn, commercial websites, and illegal content are prohibited. If your website falls under any of those categories, you can still use comic\_git with another hosting service.

## I just realized that people can see pages before they are posted to my website by going to my GitHub repository! Can I make my GitHub repository private and still publish my website to GitHub Pages?

You absolutely can! However, publishing a private GitHub repo is not an option available on a free GitHub account. To do this, you will need to update your account to a [GitHub Pro](https://github.com/account/upgrade) account. This only costs $4 per month, and if you compare this to equivalent services you'd be using otherwise, like Squarespace at $12 per month, it's very affordable.

Follow these instructions to switch your repository from Public to Private. You can even use a hosting service other than GitHub Pages if you prefer, to avoid having to upgrade to GitHub Pro.

← Advanced Tips | Troubleshooting →
