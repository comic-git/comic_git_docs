# Building Your Website On Your Own PC

## Building your Website on your own PC

While it's very convenient that GitHub can build and deploy your website itself with no further required input from you, trying to make changes to the design or layout of your website can be a slow process. You have to make a change, then commit the change, then push, then wait for GitHub Pages to rebuild... then do that over and over until your website looks how you want. It can be VERY tedious and unpleasant.

Fortunately, it's possible to build your website locally without pushing to GitHub! This is very useful for testing changes to your website quickly and efficiently. This section will walk you through that.

### Install Python

The main brain of the comic\_git workflow is a Python script that is run whenever you push a change to GitHub, so to build your website locally, you will need to run that script.

First, [download the most recent version of Python](https://www.python.org/downloads/) from the Python website.

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

In a terminal window, navigate to your base repo directory. This is the directory which contains `your_content`, `.github`, and so on. We'll assume it's `D:\GitHub\comic_git` for these instructions.

Type the following command:

```
git submodule add -b "[engine version]" -f https://github.com/comic-git/comic_git_engine
```

In that command line, replace `[engine version]` with the same value as the [Engine version](https://comic-git.gitbook.io/documentation/basic-editing/editing-your-comic-info#engine-version) listed in `comic_info.ini`.

This installs comic\_git\_engine as a **submodule** of your personal repo.

{% hint style="info" %}
When building your site locally, you'll use the local version of comic\_git\_engine, which may not update when the live repo updates. You can manually update it to the newest version with the command `git submodule update --remote`.

If you ever want to use a different version of the engine, you can switch it by going into your `comic_git_engine` directory and running `git checkout <version>`, where `<version>` is a branch or release tag such as `latest`.
{% endhint %}

### Install the required libraries

Your next step is to install the libraries needed for comic\_git to build your website. Fortunately, Python comes with a tool that can automatically do this for you called _pip_.

Open up your Command Prompt/Terminal in your base repo directory and type the following command:

```
python -m pip install -r comic_git_engine\requirements.txt
```

pip will then install a number of Python packages on your computer. Once it's done, you'll see something like below:

```
Successfully installed Jinja2-2.11.2 MarkupSafe-1.1.1 Pillow-7.2.0 markdown2-2.3.9 pytz-2020.1
```

If you are using [Code Hooks](code-hooks.md) and any active theme for your main comic or Extra Comics includes a `scripts/requirements.txt` file for extra Python libraries, install the libraries from each of those files directly as well.

```
python -m pip install -r your_content\themes\<theme>\scripts\requirements.txt
```

If you use different themes for your main comic and any Extra Comics, repeat this for each theme that has its own `scripts/requirements.txt` file.

### Provide your website address locally

There is some necessary information GitHub provides to comic\_git that is not available automatically on your local machine. The cleanest option is to set `GITHUB_REPOSITORY` in the terminal before building:

```powershell
$env:GITHUB_REPOSITORY='username/repo_name'
```

You can instead provide the domain and subdirectory in `comic_info.ini`:

Edit your `comic_info.ini` file to add two new options at the bottom of the \[Comic Settings] section.

```
[Comic Settings]
...
Comic domain = https://username.github.io
Comic subdirectory = repo_name
```

`username` should be your GitHub username, and `repo_name` should be the name of the GitHub repo you created. However, if you set up your GitHub Pages to serve from a custom domain, replace everything in `Comic domain` with that and leave `Comic subdirectory` blank.

For `comic_info.toml`, the equivalent values are `site.comic_domain` and `site.comic_subdirectory`. See [TOML Configuration](../advanced-editing/toml-configuration.md).

### Run the build\_site.py script

Once your local setup is ready, you can build your site directly on your own PC. Before you do, make sure your repo contains a `your_content/site_root/` folder. comic\_git expects that folder to exist even if it is empty.

```
python comic_git_engine\src\build\build_site.py
```

comic\_git will then build your website using your current templates, settings, and comic files. By default, the finished site is written into a `build` folder inside your repo.

```
Base URL: https://ryanvilbrandt.github.io/comic_git_dev, base subdirectory: /comic_git_dev
Local time is 2026-04-18 09:50:19.588168-07:00
...
Copy extra files to output directory: 38.74 ms
Postprocessing hook: 0.03 ms
Total time: 246.05 ms
```

When the script finishes, look in `build` for the generated site files. In most local workflows, this is the folder you will:

* preview in a browser
* inspect while testing template changes
* upload if you are hosting somewhere other than GitHub Pages

{% hint style="info" %}
**Using a different output folder**

If you want comic\_git to build somewhere other than `build`, use the `--output-dir` argument.

For example, if you want the generated site to go into a folder called `preview_site`:

```
python comic_git_engine\src\build\build_site.py --output-dir preview_site
```

This is especially useful when you want the output folder name to match your `Comic subdirectory` for local browser testing.

If you specifically want comic\_git to write the generated HTML files directly into the repo root instead of a separate folder, you can still set `OUTPUT_DIR` to an empty value before you run the build script. That older environment-variable approach is mainly useful for advanced cases.

```powershell
$env:OUTPUT_DIR=''
python comic_git_engine\src\build\build_site.py
```
{% endhint %}

You should run the build script again any time you change:

* `comic_info.ini`
* `comic_info.toml`
* any page `info.ini` file
* any page `info.toml` file
* any `.tpl` template
* any Python script used by your build
* your comic page folders or page files

If you want to convert existing INI and page text files to TOML, use the dry-run-first process in [TOML Configuration](../advanced-editing/toml-configuration.md#migrating-existing-files).

If you only change CSS or JavaScript files, you usually do not need to rebuild. In most cases, refreshing your browser is enough.

### Auto-rebuilding preview server

For theme and content work, comic_git includes a preview server that rebuilds when supported source files change. Install its optional watcher once:

```powershell
python -m pip install watchdog
```

Then run this from your comic_git repository root:

```powershell
python comic_git_engine\src\scripts\dev_server.py
```

The command prints the local URL to open and watches changes to TOML, INI, text, Markdown, HTML, and template files. Press Ctrl+C to stop it.

To include future-dated pages in a local preview without deleting their source folders, use:

```powershell
python comic_git_engine\src\scripts\dev_server.py --publish-all-comics
```

Do **not** use `--delete-scheduled-posts` for an ordinary local preview.

{% hint style="warning" %}
**Generated HTML files are temporary build output**

With the default `build` workflow, editing generated HTML files is less risky than it used to be because those files live in their own folder. But they are still temporary build output, and your changes will be overwritten the next time you rebuild.

If you want to change how a page is generated, edit your existing [Theme](../advanced-editing/themes.md) instead.
{% endhint %}

{% hint style="info" %}
**Deleting auto-generated files**

With the default setup, all of your generated local site files live in `build`. If you want to clear them out, you can simply delete that folder and rebuild it later.

If you are using a custom output folder, delete that folder instead.

If you are using the old in-place build behavior described above, the helper script below is useful because it removes the generated HTML files from your repo root for you.

```
python comic_git_engine\src\scripts\delete_autogenerated_files.py
```
{% endhint %}

## Viewing your Website on your own PC

You've learned how to build your website on your local PC. However, you also need to be able to view your website. While you can open the individual HTML files, this doesn't replicate a full server environment.

By default, comic\_git builds your generated site into the `build` folder. That means:

* If you just want to inspect the generated files or upload them somewhere else, use the contents of `build`.
* If you want to run a simple local web server, the exact instructions depend on whether your `Comic subdirectory` is blank.

### If `Comic subdirectory` is blank

If your site is hosted directly at the root of your domain, and `Comic subdirectory` is blank in `comic_info.ini`, you can preview the default `build` folder directly.

Navigate to your repo directory and run:

```
D:\GitHub\comic_git> cd build
D:\GitHub\comic_git\build> python -m http.server 8000
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```

Then open [http://localhost:8000](http://localhost:8000/) or [http://127.0.0.1:8000](http://127.0.0.1:8000/) in your browser.

### If `Comic subdirectory` is set

If your site uses a non-empty `Comic subdirectory` (the usual GitHub Pages setup), the generated links expect your site to live at a URL like `https://username.github.io/repo_name/`. Because of that, serving the `build` folder directly from `http://localhost:8000/` will not match the URLs comic\_git generated.

For a local browser preview in this case, set `--output-dir` to the same value as `Comic subdirectory`, then rebuild your site. For example, if `Comic subdirectory = comic_git`, run:

```
python comic_git_engine\src\build\build_site.py --output-dir comic_git
```

This will create a folder in your repo with the same name as your configured subdirectory, which makes the local URL structure match what comic\_git generated.

Then start the web server from your repo root:

```
D:\GitHub\comic_git> python -m http.server 8000
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
```

Once this appears, open your web browser and go to either [http://localhost:8000/comic\_git](http://localhost:8000/comic_git) or [http://127.0.0.1:8000/comic\_git](http://127.0.0.1:8000/comic_git) (Replace `comic_git` in the URL with the name of the folder your repository is in). You should see your site pop up exactly as if it's been published to the web!

To stop the web server, go into the command prompt window where you ran the command above and hit Ctrl+C.

{% hint style="info" %}
**Why the URL needs to include your repo's folder name**

Putting `comic_git` at the end of the URL above is necessary because the web server will use the directory that you launch it from as the URL root. Meaning, it will serve all the folders and files it finds in that directory when you go to `http://localhost:8000`. To have it display your website correctly, the generated site files need to be inside a folder whose name matches your `Comic subdirectory` setting.

The default setup for GitHub Pages is to serve your website from `https://<username>.github.io/<repo_name>`, rather than from `https://<username>.github.io/` . Because of that, comic\_git builds all its links in your website (including the ones to CSS and JavaScript files necessary to run the site) by prepending whatever value you put in your "Comic subdirectory" config option above to all links. If it didn't, then your website wouldn't load any CSS properly, and all links to pages within the site would be broken!

If you're hosting directly from the root of your domain (e.g., `https://www.tamberlanecomic.com`), and your "Comic subdirectory" config option is blank, then you do not need this in-place preview workflow at all. You can simply serve the default `build` folder directly as described above.
{% endhint %}
