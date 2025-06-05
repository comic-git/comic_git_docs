# Building Your Website On Your Own PC

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

[Python has full documentation on installing Python on Mac](https://docs.python.org/3/using/mac.html).

#### Linux

[Python has full documentation on installing Python on different flavors of Linux](https://docs.python.org/3/using/unix.html).

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

Unfortunately, modern web browsers have mechanisms in place to prevent what's called [Cross-site Scripting attacks](https://www.owasp.org/index.php/Cross-site_Scripting_\(XSS\)), so you can't just double-click on your index.html file for example and have it work in your web browser. You will need to run an HTTP server on your local machine to serve your website for it to work properly.

Fortunately, there are many ways to do this. One of the easiest ways is to install the [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb/related?hl=en) extension.

Once it's installed, click the `Launch App` button, and a dialog window should come up.

<figure><img src="https://raw.githubusercontent.com/ryanvilbrandt/comic_git/docs/docs/img/advanced_tips/web_server_dialog.png" alt=""><figcaption></figcaption></figure>

Take note of the **CHOOSE FOLDER** button, the switch underneath it, and the hyperlink under **Web Server URL(s)**. You do not need to worry about any of the other settings.

When the app launches, the switch is blue, meaning that the app is currently serving the default folder as a web page. This is not going to be the folder you want, so click the blue switch so it turns gray. This will disable the web server.

Click the **CHOOSE FOLDER** button, and navigate to where you have your comic repository saved. Select the directory your comic repository folder is in (NOT the folder of the comic repository itself). For example, if you have your comic repository at `C:\Users\username\Documents\GitHub\sample-comic`, you should select the folder `C:\Users\username\Documents\GitHub`. This is because on github.io, your comic repository is not at the "root directory" of your server space, but rather in its own directory. This will give your local web server a similar setup, and keeping your development space as similar to your production space as possible is good practice.

Once you've selected your folder, click the switch to turn it blue again. Then click the hyperlink under **Web Server URL(s)**. This will open up a web browser pointing to http://127.0.0.1:8887/. Add your comic name on the end (e.g. http://127.0.0.1:8887/sample-comic/) and you should see your web comic appear.

Voila! Now as soon as you save changes to any of your website files, you should be able to refresh that web page and see them without having to upload to GitHub. If you make changes to any CSS or Javascript files, you may need to **force refresh** the page to force your browser to load those files again. This is usually done with Ctrl-F5.
