# Building Your Website On Your Own PC

## Building your Website on your own PC

While it's very convenient that GitHub can build and deploy your website itself with no further required input from you, trying to make changes to the design or layout of your website can be a slow process. You have to make a change, then commit the change, then push, then wait for GitHub Pages to rebuild... then do that over and over until your website looks how you want. It can be VERY tedious and unpleasant.

Fortunately, it's possible to build your website locally without pushing to GitHub! This is very useful for testing changes to your website quickly and efficiently. This section will walk you through that.

### Install Python

The main brain of the comic\_git workflow is a Python script that is run whenever you push a change to GitHub, so to build your website locally, you will need to run that script.

First, [download the most recent version of Python](https://www.python.org/downloads/) from the Python website.&#x20;

{% hint style="warning" %}
comic\_git requires Python 3.12 or greater.
{% endhint %}

Install Python on your computer, and follow the instructions. If there are options to add Python to your PATH, enable it.

Next, verify that Python is installed and working on your machine.

#### Windows

Open the Command Prompt. Type in the following command, and you should get back the version of Python currently installed on your PC.

```
C:\Users\JohnSmith> python --version
Python 3.13.4
```

If you're on Windows 7 or lower and the install didn't work properly, you'll see something like below. If you are on Windows 10, Windows may automatically open up the Windows Store to download Python. If it does, close it and move on.

```
C:\Users\JohnSmith> python --version
'python' is not recognized as an internal or external command,
operable program or batch file.
```

In this case, check that Python was installed by finding the `python.exe` file, which will usually be found at `C:\Users\<username>\AppData\Local\Programs\Python\Python38\python.exe`. If you cannot find this file, try reinstalling Python.

Otherwise, Python was not added to your PATH properly. You can do so yourself by following [these instructions](https://geek-university.com/python/add-python-to-the-windows-path/). You may need to restart your PC afterwards.

#### Mac

[Python has full documentation on installing Python on Mac](https://docs.python.org/3/using/mac.html).

#### Linux

[Python has full documentation on installing Python on different flavors of Linux](https://docs.python.org/3/using/unix.html).

### Add comic\_git\_engine to your repo

To work on GitHub, comic\_git is split into two repositories: your personal content repo and comic\_git\_engine, which hosts all the necessary scripts to build the site. When running locally, you won't be able to access the engine repo. These instructions will install the engine into your local repository so you can make use of it.

In a terminal window, navigate to your base repo directory. This is the directory which contains `your_content`, `favicon.ico`, and so on. We'll assume it's `D:\GitHub\comic_git` for these instructions.

Type the following command:&#x20;

```
D:\GitHub\comic_git> git submodule add -b "[engine version]" -f https://github.com/ryanvilbrandt/comic_git_engine
```

&#x20;In that command line, replace `[engine version]` with the same value as the engine version listed in `comic_info.ini`.

This installs comic\_git\_engine as a **submodule** of your personal repo.

{% hint style="info" %}
When building your site locally, you'll use the local version of comic\_git\_engine, which may not update when the live repo updates. You can manually update it to the newest version with the command `git submodule update --remote`.
{% endhint %}

### Install the required libraries

Your next step is to install the libraries needed for comic\_git to build your website. Fortunately, Python comes with a tool that can automatically do this for you called _pip_.

Open up your Command Prompt/Terminal in your base repo directory and type the following command:

```
D:\GitHub\comic_git> python -m pip install -r comic_git_engine\scripts\requirements.txt
```

pip will then install a number of Python packages on your computer. Once it's done, you'll see something like below:

```
Successfully installed Jinja2-2.11.2 MarkupSafe-1.1.1 Pillow-7.2.0 markdown2-2.3.9 pytz-2020.1
```

### Update your comic\_info.ini

There is some necessary information GitHub provides to comic\_git that is not available on your local machine. You will need to provide this info in the `comic_info.ini` file.

Edit your `comic_info.ini` file to add two new options at the bottom of the \[Comic Settings] section.

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
D:\Github\comic_git> python comic_git_engine\scripts\build_site.py
```

comic\_git will then take over and build your site for you.

```
Local time is 2020-07-05 13:27:40.954881-07:00
Running on GitHub: False
...
Write HTML files: 143.65 ms
Total time: 1444.15 ms
```

And you're done! The script will create the necessary HTML files, which you can then view on your computer.

Any time you make a change to `comic_info.ini`, any of the `info.ini` files, any of the `.tpl` files, any of the Python scripts, or if you add or delete a comic page folder, you'll need to run the `build_site` script again.

You do NOT need to run the script again if you update any of the CSS or Javascript files. These can be reloaded just by refreshing your web browser.

{% hint style="danger" %}
**Do NOT edit any of the HTML files that are generated**

If you do, any of your changes will be overwritten the next time the Python script is run. If you wish to edit the HTML of any of the pages, you will want to edit your existing [Theme](../advanced-editing/themes.md).
{% endhint %}

{% hint style="info" %}
**Deleting auto-generated files**

If you test your website locally, you may notice that when you go to make a commit to GitHub, there are a LOT more files than usual, particularly a bunch of index.html files. These are the files that are generated by GitHub whenever you push an update, and basically run your website.

Pushing these files to GitHub isn't a problem, as they'll be deleted and regenerated anyways. However, if you want to keep your commits small and clean (a good idea if you ever need to go back and see what changes you've made), then you can run `python comic_git_engine\scripts\delete_autogenerated_files.py` to clear these out beforehand.
{% endhint %}

## Viewing your Website on your own PC

You've learned how to build your website on your local PC. However, you also need to be able to view your website. While you can open the individual HTML files, this doesn't replicate a full server environment. These instructions walk you through how to start a simple web server on your PC, so you can view your site as if you were looking at it on GitHub Pages.

Navigate to your base repo directory and run the following command:

```
D:\GitHub\comic_git> python -m http.server 8000
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```

Once this appears, open your web browser and go to either [http://localhost:8000](http://localhost:8000/) or [http://127.0.0.1:8000](http://127.0.0.1:8000/). You should see your site pop up exactly as if it's been published to the web!

To stop the web server, go into the command prompt window where you ran the command above and hit Ctrl+C.
