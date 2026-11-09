# Hosting comic\_git Elsewhere

## Hosting comic\_git Outside of GitHub Pages

You can publish comic\_git outside GitHub Pages using [SFTP](#sftp) or
[Neocities](#neocities), while continuing to edit and push your comic through
GitHub. This is useful if you already have hosting or have
[exceeded GitHub Pages' usage limits](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#usage-limits).
You can also build and upload your website manually, as described below.

For workflow settings shared by these deployment targets, including checkout
exclusions, artifact retention, and the output directory, see
[GitHub Actions Build Options](github-actions-build-options.md).

Moving GitHub Pages to another website is as simple as running the [website building script locally on your own PC](building-your-website-on-your-own-pc.md) and then uploading those files to your web host via whatever method they prefer. The most common method will be SFTP, which your web host's documentation will hopefully help you get set up.

Without selective copying, the staged build copies all of `your_content` into
`build`. Future-dated pages have no generated HTML, but their source images can
still be in the copied content unless the legacy deletion flag removes them first.

With engine 1.1.1 or later, use selective copying for a manual deployment build:

1. Run `python comic_git_engine\src\build\build_site.py --selective-copy` from your comic repository.
2. Check the finished `build` folder, including any required hidden-file exceptions.
3. Upload the contents of `build` to your web server.

Future page folders remain in source; generated thumbnails may be added or updated
there for reuse. See [Selective copying](building-your-website-on-your-own-pc.md#selective-copying)
for configuration, eligibility, hidden files, and the distinction from legacy copying.

{% hint style="danger" %}
In legacy mode, `--delete-scheduled-posts` deletes future-dated comic page folders
from the checkout where it runs. Use a disposable checkout for that older deployment
process. Selective mode does not delete those folders, even if the flag is also supplied.
{% endhint %}

After comic\_git's scripts are run, your built website is nothing more than HTML, CSS, and a little JavaScript, so any basic web server will be able to host it.

## SFTP

> **Added in 1.1.1**

SFTP lets GitHub upload your finished comic to a hosting account such as DreamHost.
GitHub can build and upload your site after each push. You can also
[build and deploy from your own computer](#deploy-from-your-computer).

### Back up the target directory

{% hint style="warning" %}
**Before your first SFTP deployment, download a backup of the entire target
directory, including hidden files.** By default, comic_git overwrites existing
files at matching paths and deletes files in the target directory that are absent
from the new build. This applies to GitHub Actions and local deployments.
Use a dedicated website directory. Setting `SFTP_CLEAN=false` keeps files
absent from the build, while files at matching paths are still overwritten.
{% endhint %}

### 1. Prepare your hosting account

Create a hosted website or subdomain with your provider and enable HTTPS. You
need these details from the hosting control panel:

| Setting | Example | Meaning |
| --- | --- | --- |
| SFTP host | `server.example.com` | The server you connect to; it can differ from your website's domain. |
| Port | `22` | Use the port supplied by your host. |
| Username | `comicartist` | Your website's SFTP user, which may differ from your control-panel login. |
| Password | Your SFTP password | Keep it in GitHub Secrets. |
| Website directory | `/home/comicartist/comic.example.com` | An existing directory whose contents are served as your website. |

On DreamHost, use the website's SFTP user and its assigned server. A typical
website directory is `/home/username/example.com`. Upload into that website
directory, **not** `/home/username`. SFTP-only access is sufficient: comic_git
does not need a remote shell. See DreamHost's [connection details](https://help.dreamhost.com/hc/en-us/articles/115000675027-FTP-overview-and-credentials)
and [website directory guide](https://help.dreamhost.com/hc/en-us/articles/214920778-Where-do-I-upload-my-files-once-connected-via-FTP).

SFTP is different from FTP and FTPS. Your host must support SFTP. The destination
must already exist and the SFTP user must be able to write and delete files there.
comic_git will not create a missing destination directory.

### 2. Set your website URL

Edit the existing `[Comic Settings]` section of `your_content/comic_info.ini`:

```ini
[Comic Settings]
Engine version = 1.1
Comic domain = comic.example.com
Comic subdirectory =
```

An empty subdirectory means `https://comic.example.com/`. For a site at
`https://example.com/comic/`, use `example.com` for the domain and `comic` for the
subdirectory, and deploy into the server directory that serves `/comic/`.
The URL subdirectory is not added to your upload path automatically.

Update DNS and HTTPS through your hosting provider separately. comic_git does
not do that for you, and it generates HTTPS absolute URLs. Do not use the
GitHub Pages DNS records when your site is hosted elsewhere.

Both selectors must support SFTP: the workflow reference in step 4 and the engine
version here. Starting with 1.1.1, `v1.1` and `1.1` receive 1.1 patch updates.
To pin exactly this release instead, use workflow `v1.1.1` and engine `1.1.1`.

### 3. Add GitHub Secrets

In your GitHub repository, open **Settings → Secrets and variables → Actions →
New repository secret**. Add:

| Secret name | Value |
| --- | --- |
| `SFTP_USERNAME` | Your SFTP username |
| `SFTP_PASSWORD` | Your SFTP password |
| `SFTP_KNOWN_HOSTS` | A verified server host-key entry, explained below |

For screenshots, see [Adding GitHub Secrets](code-hooks.md#adding-github-secrets).
Do not paste passwords into the workflow YAML or your comic configuration.

#### Verify the server identity

The host-key entry lets GitHub verify it is connecting to your hosting server.
It is a public server identifier, not an SSH login key. You still sign in with
your password; you do not need to create a personal SSH key.

Ask your host for an OpenSSH `known_hosts` entry or its SSH host-key fingerprint.
If the host gives you a verified entry, paste the complete line into
`SFTP_KNOWN_HOSTS`. It looks like:

```text
server.example.com ssh-ed25519 AAAA...the-server-public-key...
```

If you only have a fingerprint, someone comfortable with a terminal can collect
the entry and compare it with your provider's independently obtained fingerprint:

```sh
ssh-keyscan -p 22 -t ed25519 server.example.com > known_hosts
ssh-keygen -lf known_hosts
```

Only save the entry if the fingerprints match. `ssh-keyscan` alone does not verify
the server. Use the actual SFTP host, port and key type supplied by your provider;
for a nonstandard port an entry starts with `[server.example.com]:2222`. If your
provider replaces the server key later, verify the replacement before updating
the secret. comic_git deliberately refuses unverified or changed keys.

### 4. Enable automatic SFTP deployment

Edit `.github/workflows/main.yaml`. Keep your existing `on:` triggers and replace
the `call-build-site` job with this, substituting your host and directory:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      DEPLOY_TARGET: sftp
      OUTPUT_DIR: build
      SFTP_HOST: server.example.com
      SFTP_REMOTE_DIR: /home/comicartist/comic.example.com
    secrets:
      SFTP_USERNAME: ${{ secrets.SFTP_USERNAME }}
      SFTP_PASSWORD: ${{ secrets.SFTP_PASSWORD }}
      SFTP_KNOWN_HOSTS: ${{ secrets.SFTP_KNOWN_HOSTS }}
```

Keep the separate `build` output folder. The SFTP build removes unapproved hidden
files from generated output, so it cannot use your source checkout as its output.

Commit and push these changes to the branch your workflow builds. Open the
repository's **Actions** tab, select the run, and check the build and
`deploy-sftp` jobs. Once both succeed, visit your website and a comic page. For
scheduled comics, keep your existing daily schedule trigger enabled.

### Optional settings

Add these under the job's `with:` section as needed:

| Setting | Default | Purpose |
| --- | --- | --- |
| `SFTP_PORT` | `22` | Your provider's SFTP port. |
| `SFTP_CLEAN` | `true` | Remove destination files absent from the build. Set `false` to keep obsolete files; current files are still uploaded and updated. |
| `SFTP_HIDDEN_PATHS` | Empty | Hidden output files or directories you explicitly want published. |
| `SFTP_EXCLUDE_PATHS` | Empty | Destination files or directories that deployment must neither overwrite nor delete. |

#### Hidden files and site_root

A hidden file or directory has a name beginning with a dot, such as `.htaccess`
or `.well-known`. Hidden paths are omitted from SFTP output unless you explicitly
include them. Ordinary files in `your_content/site_root/` are copied to the
website root as usual.

For example, put Apache rules in `your_content/site_root/.htaccess`, then allow
that output path:

```yaml
      SFTP_HIDDEN_PATHS: |
        .htaccess
```

Each line is an exact path relative to the built website root. An included
directory includes its contents. For a file copied to the root from `site_root`,
use `.htaccess`, not `your_content/site_root/.htaccess`; those are separate output
locations. Paths elsewhere in the built site use their full output-relative
location. Do not use wildcards or `..`. Include only files your host needs; `.nojekyll` is generally
unnecessary outside GitHub Pages. Entries must exist in the completed output.

#### Preserve files managed by the host

If your host manages `.well-known` for certificates, preserve it instead of
uploading your own copy:

```yaml
      SFTP_EXCLUDE_PATHS: |
        .well-known
```

Exclusions are exact destination-relative paths, one per line, with no wildcards.
They preserve both files and directory contents. An excluded path is never
uploaded, even if also included by `SFTP_HIDDEN_PATHS`. A hidden file omitted from
the build can still be removed by cleanup unless it is explicitly excluded.

### Optional: sign in with an SSH key

Passwords are the normal setup above. If you prefer key authentication, create a
dedicated SSH keypair and install its public key for the SFTP user using your
host's instructions. Store the complete **unencrypted OpenSSH private key** in a
GitHub Secret named `SFTP_PRIVATE_KEY`, including its BEGIN/END lines.

Replace the password mapping in the workflow's `secrets:` section:

```yaml
    secrets:
      SFTP_USERNAME: ${{ secrets.SFTP_USERNAME }}
      SFTP_PRIVATE_KEY: ${{ secrets.SFTP_PRIVATE_KEY }}
      SFTP_KNOWN_HOSTS: ${{ secrets.SFTP_KNOWN_HOSTS }}
```

Pass exactly one of `SFTP_PASSWORD` or `SFTP_PRIVATE_KEY`. The deploy runner
accepts LF and Windows CRLF line endings and adds a final newline if needed.
Never commit the private key.

### Deploy from your computer

[Back up the target directory](#back-up-the-target-directory) before the first run.

Use this command when you want to build and publish the contents of your local
checkout in one step. Follow the [local build setup](building-your-website-on-your-own-pc.md)
and install the optional SFTP dependencies in your Python environment. This
works in a native Windows, macOS, or Linux Python environment. Run these
commands from the comic repository with your local build environment active.

```sh
python -m pip install -r comic_git_engine/requirements_sftp.txt
```

From your comic repository, set your connection details in the terminal and run
the deployment command. Replace the example hostname, directory, and username
with the values from your host. Save the full [verified OpenSSH host-key line](#verify-the-server-identity)
for that hostname as `dreamhost-known-hosts` in your home directory.

**Windows PowerShell**

```powershell
$env:SFTP_HOST = 'server.example.com'
$env:SFTP_REMOTE_DIR = '/home/comicartist/comic.example.com'
$env:SFTP_USERNAME = 'comicartist'
$env:SFTP_KNOWN_HOSTS = Get-Content -Raw -LiteralPath (Join-Path $HOME 'dreamhost-known-hosts')
$env:SFTP_CLEAN = 'false'
python comic_git_engine/src/scripts/deploy_sftp.py
```

**macOS and Linux**

```sh
export SFTP_HOST=server.example.com
export SFTP_REMOTE_DIR=/home/comicartist/comic.example.com
export SFTP_USERNAME=comicartist
export SFTP_KNOWN_HOSTS="$(cat "$HOME/dreamhost-known-hosts")"
export SFTP_CLEAN=false
python comic_git_engine/src/scripts/deploy_sftp.py
```

In an interactive terminal, the command prompts for your password when no
credentials are set. For automation, supply `SFTP_PASSWORD` or `SFTP_PRIVATE_KEY`
through the environment. For key authentication, supply the complete unencrypted
OpenSSH private key. If your server uses a port other than 22, set `SFTP_PORT`
and use the `[hostname]:port` form in the host-key line.

The command enables selective copying and builds from your actual repository into
a temporary output directory. Your preview build stays in place. Scheduled source
folders remain intact; thumbnail generation can create or update source cache
files. Scheduled pages and their assets are omitted from the upload unless you
explicitly pass `--publish-all-comics`, which uploads future pages too. Source
symlinks/junctions are unsupported. Theme hooks run from your real repository and
can read other host-owned inputs.

The local command uses the same hidden-path allowlist and remote exclusions as
GitHub Actions. Set `SFTP_HIDDEN_PATHS` and `SFTP_EXCLUDE_PATHS` as newline-separated
environment variables when needed. Cleanup is on by default. Keep
`SFTP_CLEAN=false` to retain server files that are absent from the build.

SFTP's explicit hidden-path allowlist takes precedence over the selective mode's
`Include hidden files` setting. List intentional exceptions in `SFTP_HIDDEN_PATHS`.

The local command always enables selective copying, even if the main config
sets it to false. Hosted SFTP builds keep the existing copying default unless
you set `Selective copy = true` in the main `comic_info.ini` file's `[Build]` section. Both apply
the explicit SFTP hidden-path allowlist after copying and postprocess hooks.

### Troubleshooting

For local deployment, pass `--verbose` to `deploy_sftp.py` to see
SSH negotiation, the authentication method attempted, and any methods the
server advertises. The log includes the server and login name. If the same
password works in an interactive SFTP client, check the `SFTP_PASSWORD` value
your terminal or IDE actually passes to the script. Update any saved IDE run
configuration that still supplies the old password after a reset.

| Symptom | What to check |
| --- | --- |
| Permission denied during login | SFTP username/password, or installed public key and private-key formatting. Your hosting-panel password may be different. |
| Host key verification fails | Verify the host and port, then compare the key with your provider. Do not disable the check. |
| Cannot change to the destination | Confirm the website directory exists and the user can access it. |
| Upload or deletion permission denied | The SFTP user needs write/delete access to the website directory. |
| Hidden file missing | Check `SFTP_HIDDEN_PATHS` and any exclusion for the same path. |
| Images or navigation lead to GitHub Pages | Set both the comic domain and subdirectory to your new website's URL. |
| Unknown SFTP input or command-line option | Update both the reusable workflow reference and the engine selector to 1.1.1 or later. |

Deployments update files in place. A failed transfer can leave a partly updated
site; correct the error and rerun the workflow. Publication is not atomic and
does not provide automatic rollback. The workflow uses `lftp`, a command-line
file-transfer client installed automatically on GitHub's runner.

## Neocities

> **Added in 1.1.0**

If you want to host your site on Neocities, comic\_git can also deploy there through GitHub Actions. This is not a different publishing workflow from the user's perspective. You will still upload your changes to GitHub normally so that the Action can run. The difference is that the finished site will be deployed to Neocities instead of GitHub Pages.

### What you need before starting

Before setting this up, you should have:

* a working comic\_git repository
* a Neocities account
* a Neocities API token

### Set your website URL

Set the comic domain to your published Neocities domain, such as
`mycomic.neocities.org`, and leave the comic subdirectory empty when publishing
at the site root. Follow [Hosting outside GitHub Pages](changing-your-website-url.md#hosting-outside-github-pages)
for configuration examples. The deployment target does not change these URL
settings automatically.

### Minimal Neocities workflow

To deploy to Neocities, update the `call-build-site` job in your `.github/workflows/main.yaml` file so it looks like this:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      DEPLOY_TARGET: neocities
    secrets:
      NEOCITIES_API_TOKEN: ${{ secrets.NEOCITIES_API_TOKEN }}
```

### Adding your Neocities API token

Your Neocities API token should be saved in GitHub Secrets as `NEOCITIES_API_TOKEN`.

If you have not added a GitHub Secret before, see the [Adding GitHub Secrets](code-hooks.md#adding-github-secrets) section of the Code Hooks page. That section already includes screenshots of the GitHub interface to help you through the process.

### Neocities workflow parameters

The `build_site` workflow supports these Neocities-related parameters:

| Parameter                   | Required? | What it does                                                                                                                             |
|-----------------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------|
| `DEPLOY_TARGET`             | Yes       | Set this to `neocities` to deploy to Neocities instead of GitHub Pages.                                                                  |
| `NEOCITIES_API_TOKEN`       | Yes       | Your Neocities API token, passed in through GitHub Secrets.                                                                              |
| `NEOCITIES_CLEAN`           | No        | If `true`, files on Neocities that are not present in the built output may be deleted. Default: `true`.                                  |
| `NEOCITIES_SUPPORTER`       | No        | Set this to `true` if you have a paid Neocities supporter account and want to bypass the unsupported file type filter. Default: `false`. |
| `NEOCITIES_PROTECTED_FILES` | No        | A glob pattern for remote Neocities files that should never be deleted by cleanup. Protected files can still be updated.                 |


### Example configurations

Here is an example that turns cleanup off while you are testing the deployment:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      DEPLOY_TARGET: neocities
      NEOCITIES_CLEAN: false
    secrets:
      NEOCITIES_API_TOKEN: ${{ secrets.NEOCITIES_API_TOKEN }}
```

Here is an example that protects specific remote files from cleanup:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      DEPLOY_TARGET: neocities
      NEOCITIES_PROTECTED_FILES: "assets/uploads/**"
    secrets:
      NEOCITIES_API_TOKEN: ${{ secrets.NEOCITIES_API_TOKEN }}
```

Here is an example for a paid Neocities supporter account:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      DEPLOY_TARGET: neocities
      NEOCITIES_SUPPORTER: true
    secrets:
      NEOCITIES_API_TOKEN: ${{ secrets.NEOCITIES_API_TOKEN }}
```

{% hint style="warning" %}
**Be careful with cleanup**

`NEOCITIES_CLEAN` is useful, but it can also remove files from your Neocities site that are not present in your latest build output. It is a good idea to test with `NEOCITIES_CLEAN: false` first, and to use `NEOCITIES_PROTECTED_FILES` for any remote files you need to keep.
{% endhint %}

### Official GitHub Action documentation

comic\_git uses the [`bcomnes/deploy-to-neocities`](https://github.com/bcomnes/deploy-to-neocities) GitHub Action to upload files to Neocities. If you want more detail on the underlying deployment action, that repository's README is the official reference.
