# What is comic\_git?

## You Drew It, You Own It

There are many options on the internet these days for hosting webcomics. They range anywhere from old school solutions like Smack Jeeves and ComicPress to more new-fangled services like Tapas and WebToons.&#x20;

One major fault of all these solutions, however, is that you are not in control of the content you publish on them. The worst offenders have language in their Terms of Service that allow them to use your creative work to make money for themselves. Even for self-hosted solutions like ComicPress, which give the user full legal ownership of the content, you are still relying on the hosting service to take good care of your comic. If the service shuts down or is hit by hackers and you didn't make a backup of your comic, it's completely gone. All your artwork, your design work, everything is kaput.

comic\_git is different from these other services because it is designed to give you full ownership at all times of not just your comic, but your entire website.&#x20;

## Your Computer, Your Comic, Your Way

Every other webcomic solution has a web-based UI. That is, to manage your comic in any way, whether that's uploading new pages, changing the layout of your website, or even just tweaking text post somewhere, you need to log in to their website and make your changes there.&#x20;

comic\_git was written from the ground up so that every change you want to make to your comic is done on your own computer. No logging in required, no buggy UIs to deal with, no risk of losing your work because you had to refresh your browser or your computer bluescreened before you could hit upload. You can do all your designing and updating completely on your own machine, and only when you're ready to update the live version of your webpage do you need to even be connected to the internet. And even then, if you follow our setup steps properly, uploading your changes will be as simple as clicking a button.

## 100% Freedom of Design

The idea for comic\_git came around from watching my wife struggle with ComicPress, one of the self-hosting options for webcomics. ComicPress is not an independent piece of software, but rather a plugin for WordPress which is intended for self-hosting blog posts. As such, ComicPress relies on a bunch of hacking of WordPress' functionality to get it to support webcomics in any meaningful way. So doing anything beyond the most basic of web design for her website was an absolute nightmare. Trying to make any changes to the layout of her website involved hours of trial and error, and sometimes coming across long-standing bugs that made it impossible for her to do what she wanted.

comic\_git's web design layout is driven entirely by the most basic building blocks of web technologies: HTML, CSS, and Javascript. There are no custom plugins that obfuscate the final results of your webpage, that might break and make what you want to do impossible. If it can be done on any other website, you can do it in comic\_git.

This also has the benefit that you don't need someone who's an expert in comic\_git to be able to design a website. Do you have a friend who's built his own website? You can use his knowledge! Anyone with any experience in building websites can use comic\_git!

## 100% Free of Cost

One of the biggest strengths of comic\_git, and likely the major selling point (pun intended) is that it is 100% free, out of the box. You can run through the instructions to get started in less than an hour, and end up with a fully-functioning webcomic that will never cost you a dime.&#x20;

The key to this is that comic\_git, by default, uses GitHub Pages. This is a hosting service built into GitHub that will host any content in any repository you own, free of charge. All you have to do it upload your comic to GitHub, change some settings, and GitHub will automatically run comic\_git's scripts to build your website, and upload it to GitHub Pages.

There are some limitations to the free, out-of-the-box version of comic\_git, however. For one, it's hosted from GitHub Pages' domain, e.g. [https://ryanvilbrandt.github.io/comic\_git\_showcase/](https://ryanvilbrandt.github.io/comic_git_showcase/). However, if you want to host your comic from a domain you already own, GitHub Pages supports that. You will also be required to abide by GitHub's Terms of Service and usage limits, however these limits are so generous that [Tamberlane](https://www.tamberlanecomic.com/), the heaviest user of comic\_git, hasn't come close to hitting those limits after over 400 pages published and years of use.

Don't like GitHub Pages and want to use a different hosting service? comic\_git supports that as well! Any hosting service that lets you upload files directly from your computer is easy to use with comic\_git.

## Lightning-Fast Performance

The reason why GitHub Pages can provide this service for free with such generous limits is that it only supports [static websites](https://blog.hubspot.com/website/static-vs-dynamic-website). That is, it will only serve up pre-built HTML files. The biggest benefit of this, aside from allowing for incredibly cheap hosting, is that your website will always be blazing fast. As soon as your reader's browser downloads the file, they'll be able to see it. In a world of overloaded servers and loading screens, this is something your readers will love you for.

The major downside of running a static website is that a lot of the features that users take for granted isn't available to you. Like user accounts, databases, server-side processing... but I can guarantee that it's possible to make a beautiful, full-featured webcomic without all those things, and you'll find that your website will be better because of it.

## Okay, but... what IS comic\_git really?

comic\_git isn't a service like Tapas or WebToons. It's a software solution for hosting webcomics. It's a collection of HTML pages (more accurately, template files that can be turned into HTML pages), CSS files, and Javascript files. All you have to do is copy your comic pages into a particular folder, edit some text files, and upload everything to GitHub (which takes just three clicks). After you've done that, GitHub will run its Python script, the real "brains" of comic\_git, which will turn your comic pages into a set of files that will be uploaded automatically to GitHub Pages to create your website.

And that's it! You can delve more deeply into the details of how comic\_git works if you like, but the most basic use of comic\_git won't rely on you doing anything more than copying files between folders on your computer, renaming them, and editing simple text files. For basic changes to your website, the most you'll need to do is open up a CSS file and change "blue" to "red", or something like that.

## Sounds great! How do I get started?

If you move onto our [Getting Started](broken-reference) section, it will guide you through creating and setting up your own comic\_git repository on GitHub, and uploading your first comic pages. You'll be hosting your comic in no time! And if you run into any issues or get stuck, our [Discord server](https://discord.gg/zmdHGXB) is full of people who will be happy to help unstick you.
