# GitHub Actions Build Options

> **Added in 1.1.1**

These optional settings apply to the reusable build workflow at `@v1.1.1` or a
later compatible reference. Add them under `with:` in the `call-build-site` job
of your repository's `.github/workflows/main.yaml`.

## Skip unused source directories during checkout

If your repository contains large directories that the build never reads, list
them in `CHECKOUT_EXCLUDE_PATHS`. Give exact directory paths relative to the
repository root, one per line:

```yaml
jobs:
  call-build-site:
    uses: comic-git/comic_git_engine/.github/workflows/build_site.yaml@v1.1
    with:
      CHECKOUT_EXCLUDE_PATHS: |
        large-source-archive
```

Replace `large-source-archive` with a directory that exists in your repository.
The workflow checks each path before building. It rejects missing directories,
wildcards, absolute paths, and paths outside the repository. It also keeps
`.github` and `.git` available. The setting applies only to the caller
repository; the engine is checked out separately.

Check that your comic config, images, transcripts, themes, Code Hooks, and any
other build inputs remain available. An excluded directory is absent from the
build runner even if a hook tries to read it. Leave this setting empty unless
checkout transfer is a meaningful part of your build time.

## Keep deployment artifacts longer

`ARTIFACT_RETENTION_DAYS` defaults to `1`. You can set it to an integer from
`1` through `90`, subject to your repository or organization's maximum:

```yaml
    with:
      ARTIFACT_RETENTION_DAYS: "7"
```

Choose a longer retention period if an environment approval may be delayed or
you expect to retry a deployment job. Once its artifact expires, rerun the
complete workflow to rebuild the site.

## Output directory in GitHub Actions

> **Changed in 1.1.1**

The reusable workflow builds into `build` by default. An empty `OUTPUT_DIR`
also means `build`; `OUTPUT_DIR: .` and other paths resolving to the checkout
root are rejected. Set `OUTPUT_DIR` to a separate directory if you need a
different location. This rule applies to GitHub Actions builds. The local
Python build script still supports an explicitly empty `OUTPUT_DIR` for
advanced in-place builds.

The build uploads the generated directory for the chosen deployment target.
GitHub Pages and Neocities omit dot-prefixed files from their site artifacts.
SFTP uses its own [hidden-path settings](hosting-comic_git-elsewhere.md#hidden-files-and-site_root).
