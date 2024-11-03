# Advanced Tips

1. [Building your Website on your own PC](advanced-tips.md#building-your-website-on-your-own-pc)
2. [Viewing your Website on your own PC](advanced-tips.md#viewing-your-website-on-your-own-pc)
3. [Changing your Website URL](advanced-tips.md#changing-your-website-url)
4. [Moving to a Custom Domain](advanced-tips.md#moving-to-a-custom-domain)
5. [Hosting comic\_git Outside of GitHub Pages](advanced-tips.md#hosting-comic\_git-outside-of-github-pages)
6. [Switching from a Public to a Private Repo](advanced-tips.md#switching-from-a-public-to-a-private-repo)
7. [Changing Archive Headers to Banner Images](advanced-tips.md#changing-archive-headers-to-banner-images)
8. [Scheduled Posts](advanced-tips.md#scheduled-posts)
9. [The Power of Jinja2](advanced-tips.md#the-power-of-jinja2)
10. [Code Hooks](advanced-tips.md#code-hooks)

The tips and instructions given below are very useful for doing work on your website, but require more than the basic level of technical knowledge that's been required up to this point. You will need to be comfortable using unfamiliar programs or running Python scripts. However, I've worked to make the instructions below as friendly, clear, and painless as possible, and you will hopefully get great benefit from them if you put in the effort.

## Building your Website on your own PC

While it's very convenient that GitHub can build and deploy your website itself with no further required input from you, trying to make changes to the design or layout of your website can be a slow process. You have to make a change, then commit the change, then push, then wait for GitHub Pages to rebuild... then do that over and over until your website looks how you want. It can be VERY tedious and unpleasant.

Fortunately, it's possible to build your website locally without pushing to GitHub! This is very useful for testing changes to your website quickly and efficiently. This section will walk you through that.

### Install Python 3.8

The main brain of the comic\_git workflow is a Python script that is run whenever you push a change to GitHub, so to build your website locally, you will need to run that script.

First, [download the most recent version of Python](https://www.python.org/downloads/) from the Python website. comic\_git requires Python 3.8 or greater.

Install Python on your computer, and follow the instructions. If there are options to add Python to your PATH, enable it.

Next, verify that Python is installed and working on your machine.

#### Windows

Open the Command Prompt. Type in the following command, and you should get back the version of Python currently installed on your PC.

```
C:\Users\JohnSmith>python --version
Python 3.8.2
```

If you're on Windows 7 or lower and the install didn't work properly, you'll see something like below. If you are on Windows 10, Windows may automatically open up the Windows Store to download Python. If it does, close it and move on.

```
C:\Users\JohnSmith>python --version
'python' is not recognized as an internal or external command,
operable program or batch file.
```

In this case, check that Python was installed by finding the python.exe file, which will usually be found at `C:\Users\<username>\AppData\Local\Programs\Python\Python38\python.exe`. If you cannot find this file, try reinstalling Python.

Otherwise, Python was not added to your PATH properly. You can do so yourself by following [these instructions](https://geek-university.com/python/add-python-to-the-windows-path/). You may need to restart your PC afterwards.

#### Mac

TBC

#### Linux

If you're using Linux, you probably know what to do here. You don't need my help.

### Install the required libraries

Your next step is to install the libraries needed for comic\_git to build your website. Fortunately, Python comes with a tool that can automatically do this for you called _pip_.

Open up your Command Prompt/Terminal in your `comic_git` directory and type the following command.

```
D:\Github\comic_git>python -m pip install -r src\scripts\requirements.txt
```

pip will then install a number of Python packages on your computer. Once it's done, you'll see something like below:

```
Successfully installed Jinja2-2.11.2 MarkupSafe-1.1.1 Pillow-7.2.0 markdown2-2.3.9 pytz-2020.1
```

### Update your comic\_info.ini

There is some information that GitHub provides to comic\_git that it needs to run that is not available on your local machine. You will need to provide this info in the comic\_info.ini file.

Edit your comic\_info.ini file to add two new options at the bottom of the \[Comic Info] section.

```
[Comic Settings]
...
Comic domain = https://username.github.io
Comic subdirectory = repo_name
```

`username` should be your GitHub username, and `repo_name` should be the name of the GitHub repo you created. However, if you set up your GitHub Pages to serve from a custom domain, replace everything in `Comic domain` with that and leave `Comic subdirectory` blank.

### Run the build\_site.py script

Once this is done, you can call the Python script that builds your website directly on your own PC using the following command.

```
D:\Github\comic_git>python src\scripts\build_site.py
```

comic\_git will then take over and build your site for you.

```
Local time is 2020-07-05 13:27:40.954881-07:00
Running on GitHub: False
...
Write HTML files: 143.65 ms
Total time: 1444.15 ms
```

And now you're ready to build your website locally! Any time you make a change to comic\_info.ini, any of the info.ini files, any of the \*.tpl files, any of the Python scripts, or if you add or delete a comic page folder, you will need to run Python script again.

You do NOT need to run the script again if you update any of the CSS or Javascript files. These can be reloaded just by refreshing your web browser.

{% hint style="danger" %}
**Do NOT edit any of the HTML files that are generated**

If you do, any of your changes will be overwritten the next time the Python script is run. If you wish to edit the HTML of any of the pages, edit the appropriate \*.TPL file in src\views.
{% endhint %}

{% hint style="info" %}
**Deleting auto-generated files**

If you test your website locally, you may notice that when you go to make a commit to GitHub, there are a LOT more files than usual, particularly a bunch of index.html files. These are the files that are generated by GitHub whenever you push an update, and basically run your website.

Pushing these files to GitHub isn't a problem, as they'll be deleted and regenerated anyways. However, if you want to keep your commits small and clean (a good idea if you ever need to go back and see what changes you've made), then you can run `python src\scripts\delete_autogenerated_files.py` to clear these out beforehand.
{% endhint %}

## Viewing your Website on your own PC

You've learned how to build your website on your local PC. However, you also need to be able to view your website, and that also takes a little bit of work.

Unfortunately, modern web browsers have mechanisms in place to prevent what's called [Cross-site Scripting attacks](https://www.owasp.org/index.php/Cross-site\_Scripting\_\(XSS\)), so you can't just double-click on your index.html file for example and have it work in your web browser. You will need to run an HTTP server on your local machine to serve your website for it to work properly.

Fortunately, there are many ways to do this. One of the easiest ways is to install the [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb/related?hl=en) extension.

Once it's installed, click the `Launch App` button, and a dialog window should come up.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/web\_server\_dialog.png)

Take note of the **CHOOSE FOLDER** button, the switch underneath it, and the hyperlink under **Web Server URL(s)**. You do not need to worry about any of the other settings.

When the app launches, the switch is blue, meaning that the app is currently serving the default folder as a web page. This is not going to be the folder you want, so click the blue switch so it turns gray. This will disable the web server.

Click the **CHOOSE FOLDER** button, and navigate to where you have your comic repository saved. Select the directory your comic repository folder is in (NOT the folder of the comic repository itself). For example, if you have your comic repository at `C:\Users\username\Documents\GitHub\sample-comic`, you should select the folder `C:\Users\username\Documents\GitHub`. This is because on github.io, your comic repository is not at the "root directory" of your server space, but rather in its own directory. This will give your local web server a similar setup, and keeping your development space as similar to your production space as possible is good practice.

Once you've selected your folder, click the switch to turn it blue again. Then click the hyperlink under **Web Server URL(s)**. This will open up a web browser pointing to http://127.0.0.1:8887/. Add your comic name on the end (e.g. http://127.0.0.1:8887/sample-comic/) and you should see your web comic appear.

Voila! Now as soon as you save changes to any of your website files, you should be able to refresh that web page and see them without having to upload to GitHub. If you make changes to any CSS or Javascript files, you may need to **force refresh** the page to force your browser to load those files again. This is usually done with Ctrl-F5.

## Changing your Website URL

When you create your GitHub Pages website for the first time, its URL is decided by your GitHub username, and the name of the repo you've created. For example, if your username is `JacobWG`, and you named your repo `sample-comic`, then the URL for your website will be http://JacobWG.github.io/sample-comic/.

There are two ways to change the URL for your web page: Change your username/repo name, or assign a custom domain to your website.

### Changing your Username

Changing your username will change the part of the URL right before `.github.io`. It will also change the username you will have to use to login to GitHub, and you will have to relogin to GitHub Desktop. To change it, click your icon in the top right corner of any GitHub page, and then click **Settings**.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change-username-01.png)

In the list of options on the left, click **Account**. On the right, there will be a "Change Username" button. Click this and follow the steps GitHub gives you.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change-username-02.png)

### Changing your Repo Name

Changing the name of your webcomic's repository will change the last part of your URL. To change it, go to the main page of your repository and click the Settings link near the top.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change-reponame-01.png)

The very first option on the right will show your repo name. Click in the text box, make your desired changes, then click **Rename**.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change-reponame-02.png)

## Moving to a Custom Domain

At some point, you will likely want to host your comic from a custom domain, so your readers don't have to keep going to your github.io URL. Fortunately, GitHub Pages supports this!

### Change your GitHub Pages Settings

First, go to the Settings page of your repository.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/settings.png)

Scroll down to the GitHub Pages section on this page, and find the Custom Domain option.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/custom\_domain.png)

Fill out the text field for the Custom Domain to match whatever custom domain you want to assign to your website, then click Save.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/set\_custom\_domain.png)

The page will reload with a blue banner that says `Custom domain "<domain name>" saved.`

Scroll down to the GitHub Pages section. You may see a yellow banner with an informational message about your domain like below:

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/custom\_domain\_is\_set.png)

Do whatever you need to do with your registrar to resolve any issues listed here. GitHub provides support articles for this process here: https://docs.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site

Any further issues you have with this process is beyond the extent of this document. Good luck and godspeed.

### Create a CNAME file in your Repository

In your repository, create a new file called `CNAME` with no extension. In the file, add only a single line with your new domain, e.g. `www.epilepsystories.com`

### Edit Your comic\_info.ini

Open `comic_info.ini` and add the `Comic domain` option to the `[Comic Settings]` section with your new domain as its value. Add `Comic subdirectory` as well but leave the value blank. Example below:

```
[Comic Settings]
Comic domain = www.epilepsystories.com
Comic subdirectory = 
...
```

## Hosting comic\_git Outside of GitHub Pages

It is possible, and in fact quite easy, to use comic\_git outside of GitHub. You may prefer this if you have your own hosting you're already paying for, or if you've [exceeded GitHub Pages' usage limits](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#usage-limits).

Moving GitHub Pages to another website is as simple as running the website building script locally on your own PC and then uploading those files to your web host via whatever method they prefer. The most common method will be SFTP, which your web host's documentation will hopefully help you get set up.

The recommended steps for building and deploying comic\_git to another web service are as follows. This process will not upload any content for unpublished comic pages, while also not losing any of your work on your local machine.

1. Commit any changes you want to keep
2. Run `build_site.py --delete-scheduled-posts`
3. Upload your whole comic\_git directory to your web server
4. Discard all uncommitted changes with `git checkout -f`. (i.e., delete autogenerated files and undelete scheduled post content)

After comic\_git's scripts are run, your website is nothing more than HTML, CSS, and a little Javascript, so any basic web server will be able to host it.

## Switching from a Public to a Private Repo

The main reason someone would want to make their comic\_git repository private would be to be able to schedule posts while still publishing their website to GitHub Pages. If you wish to do so, you'll want to first upgrade your account to a [GitHub Pro](https://github.com/account/upgrade) account for $4 per month. (Honestly, I think you're more than getting your money's worth in this case.)

After that, go to your **Settings** page, and scroll all the way to the bottom to the section marked **Danger Zone**, and click the **Change Visibility** button.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change\_visibility.png)

This will pop up a dialog asking you to select Public or Private.

![](https://raw.githubusercontent.com/ryanvilbrandt/comic\_git/docs/docs/img/advanced\_tips/change\_visibility\_dialog.png)

When you select Private, many scary warnings will pop up underneath, the most disturbing of which is "You will permanently lose all pages published from this repository." This just means that the copy of your website that's currently uploaded to GitHub Pages will be deleted, and you'll just need to reenable Publishing to GitHub Pages afterwards. However, this means you should go through this process when you're unlikely to have many people trying to read your comic.

To finish converting your repo to a private one, type your username and repo name into the text box at the bottom of the dialog and click the "I understand, change repository visibility." button to confirm. Then [reenable GitHub Pages on your repository](https://github.com/ryanvilbrandt/comic\_git/wiki/Publishing-to-GitHub-Pages), and you're done!

## Changing Archive Headers to Banner Images

One option creators frequently request is providing header images for their archive pages rather than the default text-only headers that come with comic\_git.

This is fortunately very easy to do! You can use CSS to replace the `<h2>` tags that represent the headers with images! For example, if you create a `Ch3.png` file in the `/your_content/images/` directory, you can replace your "Chapter 3" header with the following CSS in `your_stylesheet.css`:

```
#archive-section-Chapter-3 {
    display: inline-block;
    color: transparent;
    background: url("../../your_content/images/Ch3.png") no-repeat;
    background-size: contain;
    width: 100px;
    height: 100px;
}
```

You can do the same thing for the chapter link on the infinite scroll page using `#infinite-scroll-Chapter-3` instead.

For both archive sections and infinite scroll links, be sure to set `width` and `height` to match the dimensions of the image you've provided.

## Scheduled Posts

If you set the Post Date for a comic page for a future date or time (according to the Timezone you have set in your comic\_info.ini file), comic\_git will not create that page when you push your changes to GitHub. However, the comic data (image file, info.ini file, etc.) are not gone, and the next time comic\_git runs, that comic page may be created if it's no longer set for a future date.

By default, comic\_git reruns every morning at 8:00 AM UTC to publish any comic posts that may have been scheduled previously. That is either 12am or 1am Pacific/West Coast (depending on Daylight Savings Time). If you wish to change this update schedule, you can do so by updating the following line in your `.github/workflows/main.yaml` file:

```
    - cron: '0 8 * * *'  # Runs at 8:00 AM UTC every day
```

The text `0 8 * * *` is what's called a "cron string" and it is a common way to tell computers when to perform automated tasks. This is a very powerful expression, but can be a little opaque. Fortunately, https://crontab.guru/ is an excellent resource for generating the correct cron string to represent whatever update schedule you want.

Note that the cron string is processed by GitHub assuming it's in UTC time. This means the update time changes with respect to most American timezones whenever Daylight Savings Time begins or ends. Unfortunately, there's no way to change this, so it's recommended that you either pick an update time in the middle of the night where your readers won't notice if the comic uploads one hour earlier for half of the year, or change the cron string whenever Daylight Savings Time changes.

{% hint style="info" %}
**Hiding Your Scheduled Posts**

Despite the scheduled post protection above, any scheduled posts you want to hide from the public are still available to anyone who visits your GitHub page, if they know where to look. If you wish to make your scheduled posts completely hidden from the general public, you will need to set your repository to Private.
{% endhint %}

## The Power of Jinja2

One of the main components of the architecture of comic\_git that allows it to work as well as it does are [Jinja2 Templates](https://jinja.palletsprojects.com/en/2.11.x/templates/). Put very simply, Jinja2 templates are HTML files with extra syntax in them that act as placeholders for data that can be passed to the templates later to create a fully-fledged webpage. For example, there is a single template file that is used to create every comic page that's generated by comic\_git. The following is an excerpt from the part where the page title and post date parts of the web page are created:

```
    <div id="blurb">
        <h1 id="page-title">{{ page_title }}</h1>
        <h3 id="post-date">Posted on: {{ post_date }}</h3>
```

When the Python script that builds all the web pages runs, it passes a variable called `page_title` to the template, which gets added where `{{ page_title }}` is in that template. Same with `post_date`.

There are many other features of Jinja2 templates that make them an incredibly powerful tool for automatically building a website that I won't go into here, but if you're interested in learning about them, I highly recommend reading through the [Jinja2 documentation](https://jinja.palletsprojects.com/en/2.11.x/templates/). I have also put a lot of effort into properly commenting and describing the existing template files in the `src/templates/` directory to hopefully help anyone who wants look through them to figure out how comic\_git works.

### List of Values Available to Jinja2 Templates

Assuming you want to create your own Jinja2 templates (see Customizing Your Website for guidance on how to do that), it will be helpful for you to know what data the comic\_git Python script makes available to the template whenever it builds a web page. The following is a list of all variables passed to the templates as they're being built, as well as a short description of each variable. The descriptions below assume you have basic knowledge of HTML, Jinja2, and Python data structures.

* **autogenerate\_warning**: A bit of text added to the top of every file when it's created, to warn comic creators against trying to edit the HTML files directly. Not very useful for any other purpose.
* **version**: The comic\_git version, e.g. 0.2.1
* **comic\_title**: The comic name, as defined in the `[Comic Info]` section of the `comic_info.ini` file.
* **comic\_author**: The comic name, as defined in the `[Comic Info]` section of the `comic_info.ini` file.
* **comic\_description**: The comic description, as defined in the `[Comic Info]` section of the `comic_info.ini` file.
* **banner\_image**: The web path of the banner image at the top of your website, as defined in the `[Comic Settings]` section of the `comic_info.ini` file.
* **theme**: The name of the current theme that's in use.
* **comic\_url**: The URL of the homepage of your website, e.g. https://ryanvilbrandt.github.io/comic\_git/. See Changing You Website URL for information about changing this value, or you can host your webcomic from a custom domain.
* **base\_dir**: The base directory for your comic, aka your repository name while you're hosting from a GitHub Pages URL. It is important that all URLs that reference the root directory of your website use this variable, or have the correct root directory hardcoded in the template file.
* **comic\_base\_dir**: The web path for the current comic being built. For the main comic, this will always be the same as `base_dir`, but for extra comics, the extra comic name will be added. e.g. `/base_dir/extra_comic`
* **content\_base\_dir**: The web path where the `your_content` files are stored for the current comic being built. For the main comic, this will always be the `/{base_dir}/your_content`, but for extra comics, the extra comic name will be added. e.g. `/base_dir/your_content/extra_comic`
* **links**: The list of links that make up the Links Bar on your website, partially defined by the `[Links Bar]` section of the `comic_info.ini` file. This is a list of dictionaries, each with the format `{"name": "<link name>", "url": "<link url>"}`
* **use\_images\_in\_navigation\_bar**: If True, comic pages will use images for the navigation icons. If False, text will be used. Defined in the `[Comic Settings]` section of the `comic_info.ini` file.
* **use\_thumbnails**: The boolean value of the `Use thumbnails` option, and defined in the `[Archive]` section of the `comic_info.ini` file.
* **storylines**: A dictionary of all the comics in the archive, grouped by their `Storyline` value as defined in their `info.ini` files. Any comics without a Storyline value are put into the "Uncategorized" storyline. The dictionary has the format `{"<storyline>": [<comic_dict>, <comic_dict>, <comic_dict>, ...]}`, and each comic is a dictionary of all the values comic\_git needs to build a comic page, and then some. See the next bulleted list for a description of that.
* **home\_page\_text**: The text contained in the `/your_content/home_page.txt` file. It is used by the default index.tpl file provided with comic\_git.
* **google\_analytics\_id**: Your Google Analytics Tracking ID, as defined in the `[Google Analytics]` section of the `comic_info.ini` file. If it's not defined, this value will be an empty string.
* **scheduled\_post\_count**: The number of comic pages you have uploaded that have Post Dates set in the future, so web pages for those comics have not been built yet. Useful for teasing your audience with extra pages. This number will always be `0` if you have the `publish_all_comics` command line argument set to `True`.
* **page\_title**: The title of the page, as defined in the `[Pages]` section of the `comic_info.ini` file. See this page for more information.

In addition to the above information, the information for the most recent comic page is also passed to every template as it's built. The values below are specific to that comic page, and can be used for things like creating a landing page that shows the first comic image or even just the title and post date. This is also the same data provided in each `comic_dict` object in the `storylines` variable, described above.

* **page\_name**: The unique identifier for the given comic page. This matches the name of the folder the comic is in, and the value in the URL for that comic page, e.g. `001` in `https://ryanvilbrandt.github.io/comic_git/comic/001/`
* **filename**: The filename for the comic image file, as defined in the comic's `info.ini` file.
* **comic\_path**: The path of the comic image file, of the format `your_content/comics/<page_name>/<filename>`.
* **thumbnail\_path**: The path to find the thumbnail of the comic image, if it exists. See [this page](https://github.com/ryanvilbrandt/comic\_git/wiki/Editing-your-Comic-Info#use-thumbnails) for more information.
* **alt\_text**: The alt text of the comic, as defined in the comic's `info.ini` file.
* **first\_id**: The page\_name of the first comic on your site, sorted chronologically.
* **previous\_id**: The page\_name of the previous comic.
* **current\_id**: The page\_name of the current comic.
* **next\_id**: The page\_name of the next comic. Will usually be the same as `current_id`.
* **last\_id**: The page\_name of the latest comic. Will usually be the same as `current_id`.
* **page\_title**: The title of the comic, as defined in the comic's `info.ini` file.
* **post\_date**: The post date of the comic, as defined in the comic's `info.ini` file.
* **archive\_post\_date**: The post date of the comic, but formatted to match the "Date format" in the `[Archive]` section of the `comic_info.ini` file.
* **storyline**: The storyline of the comic, as defined in the comic's `info.ini` file. If storyline is not defined, this will be `None`.
* **characters**: A list of the comic's Characters tags, as defined in the comic's `info.ini` file.
* **tags**: A list of the comic's Tags, as defined in the comic's `info.ini` file.
* **post\_html**: The contents of the `post.txt` file for the current comic, after being run through a Markdown parser to make it pure HTML.
* **transcripts**: A dictionary of the transcripts for this comic, of the format `{"<language>": "<transcript text>"}`. See this page for more info.

If you want to add your own custom variables to the list of global variables, you can! See Code Hooks

## Code Hooks

Some parts of comic\_git can be customized simply by editing config files or creating new templates. For complex or broader customization options, comic\_git provides Code Hooks.

In short, Code Hooks are Python functions that you can write yourself that, if present when comic\_git builds your site, will be run in the middle of site building logic. They can affect how that script runs, what values are provided to templates when they build, and more.

To add Code Hooks to your build, simply create a `scripts` folder inside your theme directory (if you're not using a theme, put it in the `default` theme directory). Then, copy the example file from `src/extras/hooks.py` into that folder. You can edit the functions in this file to do anything you want, including calling other Python scripts elsewhere in your repository.

{% hint style="danger" %}
**Do Not Change The Working Directory**

In comic\_git, the working directory is the repository root. Please don't change this, or you risk breaking other things.
{% endhint %}

### List of available hooks

A small list of Code Hooks are supported. All of them are present in the example `hooks.py` file as well as listed below:

#### preprocess

Runs immediately after the main comic's `comic_info.ini` file is loaded. Can be used to do any setup before the comic starts to build.

#### extra\_page\_info\_processing

Runs on every info.ini file that's processed, allowing you to do further custom processing to the page info. If you return any non-null value, it will use that in place of the page info that was passed into this hook.

#### extra\_comic\_dict\_processing

Runs on every comic data dict that's processed, allowing you to do further custom processing to the dict. If you return any non-null value, it will use that in place of the comic data dict that was passed into this hook.

#### extra\_global\_values

Returns a dictionary that will be added to the global values sent to all templates when they're built. For example, if you've created a new template which programmatically displays a list of your patrons, you'll need to hook in a new variable. This is where you'd add that variable in.

#### build\_other\_pages

This function is called after all other HTML files are built. You can use this function to build whatever additional HTML files you may want, using the `utils.write_to_template()` function. Generally it's recommended that you use the Pages section of your `comic_info.ini` file to add new pages to your site. However, if you're building pages dynamically, such as separate cast pages for each character, this is where you will call it.

#### postprocess

Runs at the very end of the comic\_git build process. Can be used to do any miscellaneous cleanup you might need.

{% hint style="info" %}
**Additional hooks**

Do you have ideas for other code hooks you'd like to see added to comic\_git? Please let me know by leaving your suggestion in the [comic\_git issues page](https://github.com/ryanvilbrandt/comic\_git/issues)!
{% endhint %}

### Third-Party library support

If you're writing additional code for comic\_git, you will likely want to make use of Python's extensive third-party library options. And you can do that! But you will need to do a little more setup.

For any `hooks.py` script that makes use of additional third-party libraries above and beyond what comic\_git already uses, you'll need to create a `requirements.txt` file in the same folder. The package name for each third-party library you need should be added on a separate line in this file. For example, you can see the packages included in the base requirements file at `src/scripts/requirements.txt`:

```
Jinja2
Pillow
markdown2
pytz
```

Any requirements you provide in any themes used by your main comic or extra comics will be auto-magically loaded and installed when the GitHub action is run, before the site is built. If you are not using a theme that contains a `requirements.txt` file, that file will not be loaded and the packages within it will not be installed.
