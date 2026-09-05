# Code Hooks

Some parts of comic\_git can be customized simply by editing config files or creating new templates. For complex or broader customization options, comic\_git provides Code Hooks.

In short, Code Hooks are Python functions that you can write yourself that, if present when comic\_git builds your site, will be run in the middle of site building logic. They can affect how that script runs, what values are provided to templates when they build, and more.

{% hint style="warning" %}
Page-related hooks receive structured page and image objects. See [Template and Hook Data](template-and-hook-data.md) for the available fields.
{% endhint %}

To add Code Hooks to your build, create a `scripts` folder inside your theme directory (if you're not using a theme, put it in the `default` theme directory), then copy [comic_git_engine's example `hooks.py`](https://github.com/comic-git/comic_git_engine/blob/latest/extras/hooks.py) into it. The example includes every supported hook; keep only the functions you need, using the signatures below. Your functions can also call other Python scripts elsewhere in your repository.

{% hint style="danger" %}
**Do Not Change The Working Directory**

In comic\_git, the working directory is the repository root. Please don't change this, or you risk breaking other things.
{% endhint %}

### List of available hooks

| Hook                            | Description |
|---------------------------------|-------------|
| preprocess                      | Receives the resolved main comic configuration immediately after it is loaded. TOML configuration is normalized to the same `RawConfigParser` interface used for INI. |
| extra_page_info_processing      | Receives `comic_folder`, `comic_info`, `page_path`, and one normalized `ComicPage`. Return a replacement page or `None` to keep it. |
| extra_comic_dict_processing     | Receives `comic_folder`, `comic_info`, and one enriched `ComicPage` after navigation and post content are resolved. The historical hook name is unchanged. |
| extra_get_storylines_processing | Receives `comic_info`, the full `list[ComicPage]`, and the grouped Archive entries. Return replacement storyline groups or `None`. This hook does not receive `comic_folder`. |
| extra_global_values             | Receives `comic_folder`, `comic_info`, and `list[ComicPage]`. Return a dictionary of extra global template values. Engine-owned values such as `tagged_pages_enabled` may be calculated afterward. |
| build_other_pages               | Receives `comic_folder`, `comic_info`, and `list[ComicPage]` after standard HTML is built. Use it for dynamic pages that cannot be expressed through the Pages configuration. |
| postprocess                     | Receives the main comic configuration, the main `list[ComicPage]`, and final global values at the end of the build. |

The corresponding function signatures are:

```python
def preprocess(comic_info):
    pass

def extra_page_info_processing(comic_folder, comic_info, page_path, page):
    return page

def extra_comic_dict_processing(comic_folder, comic_info, page):
    return page

def extra_get_storylines_processing(comic_info, pages, storylines):
    return storylines

def extra_global_values(comic_folder, comic_info, pages):
    return {}

def build_other_pages(comic_folder, comic_info, pages):
    pass

def postprocess(comic_info, pages, global_values):
    pass
```

{% hint style="info" %}
**Additional hooks**

Do you have ideas for other code hooks you'd like to see added to comic\_git? Please let me know by leaving your suggestion in the [comic\_git issues page](https://github.com/comic-git/comic_git/issues)!
{% endhint %}

### Third-Party library support

If you're writing additional code for comic\_git, you will likely want to make use of Python's extensive third-party library options. And you can do that! But you will need to do a little more setup.

For any `hooks.py` script that makes use of additional third-party libraries above and beyond what comic\_git already uses, you'll need to create a `requirements.txt` file in the same folder. The package name for each third-party library you need should be added on a separate line in this file. For example, see [comic\_git\_engine's base requirements file](https://github.com/comic-git/comic_git_engine/blob/latest/requirements.txt):

```
Jinja2
markdown2
Pillow
pytz
```

Any requirements you provide in any themes used by your main comic or extra comics will be auto-magically loaded and installed when the GitHub action is run, before the site is built. If you are not using a theme that contains a `requirements.txt` file, that file will not be loaded and the packages within it will not be installed.

### Passing Input Data to Code Hooks

You will sometimes have configuration data that you want to pass to your code hooks that doesn't make sense to include in the code itself. The most common use case is sensitive data that shouldn't be present in code in plain text, like API keys or Discord webhooks. comic\_git provides a way to pass this kind of data into your code hook code easily and securely.

The comic\_git `build_site` GitHub Action recognizes two input parameters: `INPUTS` and `SECRETS`. `INPUTS` is for any data that can be stored in plaintext but makes more sense to pass in as an input to your code. `SECRETS` is for sensitive information that can't be added safely in plaintext, and is the real purpose for this feature. Both these inputs let you provide data to be turned into "environment variables", which are data that can be used easily from anywhere within your code hook.

To use this feature, you will want to update your `.github/workflows/main.yaml` file so the `call-build-site` job section at the bottom looks something like this:

```
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      INPUTS: |
        TZ: PST8PDT
        GOOGLE_SPREADSHEET: abcde12345
    secrets:
      SECRETS: |
        PATREON_API_KEY: ${{ secrets.PATREON_API_KEY }}
        AIRTABLE_API_KEY: ${{ secrets.AIRTABLE_API_KEY }}
```

When the `build_site.py` Python script runs, this will cause four new environment variables to be created: `TZ`, `GOOGLE_SPREADSHEET`, `PATREON_API_KEY`, and `AIRTABLE_API_KEY`. `TZ` and `GOOGLE_SPREADSHEET` will both have the values shown after the `:` in the YAML file. `PATREON_API_KEY` and `AIRTABLE_API_KEY` will have the values defined in the GitHub Secrets of the same name that you have defined in your repository. (See the next section for how to do that.)

Each variable must be on its own line, and the "key" (e.g. `GOOGLE_SPREADSHEET`) must be separated from the "value" (e.g., `abcde12345`) by a colon (`:`). Any leading or trailing whitespace will be stripped from both the key and the value when they're parsed into environment variables.

This same `INPUTS` section can also be used for comic\_git build settings that are controlled through environment variables. For example, if you're troubleshooting a build, you can set `COMIC_GIT_LOG_LEVEL` here to change how much detail appears in the GitHub Actions logs. See [Troubleshooting](../additional-information/troubleshooting.md#changing-how-much-build-logging-you-see) for the available log levels.

To reference these environment variables in your code, you just need to use the `os.getenv()` function, like so:

```
api_key = os.getenv("PATREON_API_KEY")
... (use the API key for something)
```

{% hint style="warning" %}
**Do NOT put sensitive data in plain text! Use Secrets!**

If you have any data that gives you access to a service or personal space, like an API key or a Discord webhook, it is **HIGHLY RECOMMENDED** that you use secrets to save this data. While it may be less convenient to use than saving that data in plain text, it is MUCH more secure and will protect you against people that want to steal your information, impersonate you, blackmail you for access to your data, or worse.
{% endhint %}

#### Adding GitHub Secrets

To add data to GitHub Secrets so that it can be used by comic\_git, go to Settings in your repository.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

In the sidebar, click Secrets and Variables, and then click Actions.

<figure><img src="../.gitbook/assets/image (38).png" alt="" width="402"><figcaption></figcaption></figure>

In the new window that opens up, you'll see a list of all Secrets that have been created for your repository. This list starts out empty. To add a secret, click the green New Repository Secret button.

<figure><img src="../.gitbook/assets/image (40).png" alt="" width="563"><figcaption></figcaption></figure>

This will take you to a new page where you will add your secret. Give it a name (the standard practice is to give it a name IN\_THIS\_FORMAT), then put the actual secret value in the "Secret" section. If you're saving an API key, this is where that key goes.

<figure><img src="../.gitbook/assets/image (41).png" alt="" width="563"><figcaption></figcaption></figure>

When you're done, click the "Add secret" button, and the secret will be added to your list of Repository Secrets, and will be available to your repository to pass on to the `build_site` GitHub Action via the `SECRETS` input as described above.

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

You can now add more secrets if you want. From this page, you can also edit or delete existing secrets. Note that you can **not** view secrets that you've saved here. If you need those secrets for anything else, make sure to save a copy separately.

This page also lets you set up Environment Secrets or Variables that can be used similarly. How to use those features is beyond the scope of this document.
