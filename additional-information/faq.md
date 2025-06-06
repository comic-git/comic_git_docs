---
description: Frequently Asked Questions
---

# FAQ

<details>

<summary>I've installed GitHub Desktop. How do I use it to organize my comic files?</summary>

GitHub Desktop is only used to publish your changes to GitHub, and thus to your website. All organization of your files will happen in the file browser on your PC (for Windows, that's File Explorer). However, comic\_git makes this organization easy! Every comic page has its own folder, and every folder only needs a few files. See [Adding Comic Pages](../basic-editing/adding-comic-pages.md) for more information.

</details>

<details>

<summary>I pushed my comic update to GitHub, but it's not showing up on my GitHub Page.</summary>

Your first step should be to check for errors when building your website. Then, check if your changes have been deployed to GitHub Pages yet. Finally, you may just need to force refresh (Ctrl+F5) your browser to see the changes.

</details>

<details>

<summary>How do I test my website before publishing it on GitHub Pages?</summary>

The easiest way is to [build your website on your own computer](../expert-editing/building-your-website-on-your-own-pc.md). You can then view that website on your own computer as if it were a full webpage.

</details>

<details>

<summary>How do I change my comic URL/my GitHub username/the name of my webcomic repository?</summary>

[Changing your Website URL](../expert-editing/changing-your-website-url.md) has you covered.

</details>

<details>

<summary>I'm reading through comic_git's template files trying to understand how it works, but these files are too confusing!</summary>

The templates in [comic\_git\_engine's](https://github.com/ryanvilbrandt/comic_git_engine/tree/master/templates) `/templates/` directory make use of a somewhat advanced feature of Jinja2 that lets them "extend" the `base.tpl` template. This makes it easier for me to manage and update these files for core comic\_git, but can make them difficult for Jinja2 novices to understand and edit if they want to make more complicated changes to their website than the base comic\_git functions allow.

I've included a simplified version of the `comic.tpl` file at [comic\_git\_engine's](https://github.com/ryanvilbrandt/comic_git_engine/blob/master/extras/comic_minimal.tpl) `/extras/comic_minimal.tpl` for anyone who wants to edit their comic page and start from a cleaner slate. This same file can also be used to create other custom pages by replacing everything in the `container` div and writing either plain HTML or your own Jinja2 template. Though if you do that, I request that you please keep the `powered-by` div at the bottom so people know how to make their own cool web comics just like you did. 😃

</details>

<details>

<summary>I want my webcomic's home page to point to the latest comic. How do I change that?</summary>

I've created a theme for this, so it is very easy to switch! Just change the Theme setting in your `comic_info.ini` file to `homepage_is_latest_comic` and you're done! If you've edited the `default` or have created your own theme, just copy the file at `templates/index.tpl` in the `homepage_is_latest_comic` theme folder to a similar place in the theme you're using, e.g. `default/templates/index.tpl`.

</details>

<details>

<summary>Are there any size/traffic limits on comic_git?</summary>

comic\_git itself is highly performant. For a small webcomic, it will build the whole site in less than a second. For a major webcomic with over 200 pages, its build time has been known to range from 1 second to 10 seconds, depending on computer load at the time. In terms of serving the website, that is entirely reliant on the abilities of the web server comic\_git is hosted on.

However, GitHub Pages has [limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits) on how big and how popular your website can get. It's recommended you read their terms of use, but in short your website should stay under 1 GB in size, and shouldn't serve more than 100 GB of data per month. If you go over those limits, my experience shows that GitHub will not bring down the hammer on you right away, but they may eventually reach out to you to discuss the situation. In the worst case, they will reserve the right to close down your repository. If that happens, fear not, because there are other options for how to host comic\_git.

</details>

<details>

<summary>Can I host smut/classy erotica/get-rich-quick schemes/something illegal with comic_git?</summary>

I cannot stop you from publishing any content you want with comic\_git. Whatever you do with my software is your own choice, and I promise no liability or responsibility on my part for your actions while using it.

However, if you host your site on GitHub Pages, you must abide by [GitHub's Acceptable Use](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies) policy, which prohibits porn, commercial websites, and illegal content, among other things. If your website falls under any of those categories, you can still use comic\_git with another hosting service.

</details>

<details>

<summary>I just realized that people can see pages before they are posted to my website by going to my GitHub repository! Can I make my GitHub repository private and still publish my website to GitHub Pages?</summary>

You absolutely can! However, publishing a private GitHub repo is not an option available on a free GitHub account. To do this, you will need to update your account to a [GitHub Pro](https://github.com/account/upgrade) account. This only costs $4 per month, and if you compare this to equivalent services you'd be using otherwise, like Squarespace at $12 per month, it's very affordable.

[Follow these instructions to switch your repository from Public to Private. ](../other-expert-tips.md#switching-from-a-public-to-a-private-repo)

[You can also use a hosting service other than GitHub Pages if you prefer](../hosting-comic_git-elsewhere.md), to avoid having to upgrade to GitHub Pro.

</details>
