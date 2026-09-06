# Hosting comic\_git Elsewhere

## Hosting comic\_git Outside of GitHub Pages

It is possible, and in fact quite easy, to use comic\_git outside of GitHub. You may prefer this if you have your own hosting you're already paying for, or if you've [exceeded GitHub Pages' usage limits](https://docs.github.com/en/github/working-with-github-pages/about-github-pages#usage-limits).

Moving GitHub Pages to another website is as simple as running the [website building script locally on your own PC](building-your-website-on-your-own-pc.md) and then uploading those files to your web host via whatever method they prefer. The most common method will be SFTP, which your web host's documentation will hopefully help you get set up.

The default staged build copies `your_content` into `build`. Future-dated pages do not get HTML pages, but their source images can still be present in that copied folder unless deployment mode removes them first.

The safest manual deployment process is:

1. Commit and push everything you want to preserve.
2. Make a fresh, disposable clone or copy of the repository for the deployment build.
3. In that disposable copy, run `python comic_git_engine\src\build\build_site.py --delete-scheduled-posts`.
4. Upload the contents of its `build` folder to your web server.
5. Delete the disposable copy when you are finished.

{% hint style="danger" %}
`--delete-scheduled-posts` deletes future-dated comic page folders from the checkout where it runs. Do **not** run it in your normal working copy. The GitHub-hosted deployment workflow is safe because its checkout is temporary.
{% endhint %}

After comic\_git's scripts are run, your built website is nothing more than HTML, CSS, and a little JavaScript, so any basic web server will be able to host it.

## Uploading to Neocities

If you want to host your site on Neocities, comic\_git can also deploy there through GitHub Actions. This is not a different publishing workflow from the user's perspective. You will still upload your changes to GitHub normally so that the Action can run. The difference is that the finished site will be deployed to Neocities instead of GitHub Pages.

### What you need before starting

Before setting this up, you should have:

* a working comic\_git repository
* a Neocities account
* a Neocities API token

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
