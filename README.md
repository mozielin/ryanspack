# Private Composer Package Repository

This repository publishes a Satis Composer registry to GitHub Pages. It lets Laravel applications install packages from your GitHub repositories without publishing them on Packagist.

## One-time GitHub setup

1. Push this repository to GitHub, using `main` as the default branch.
2. In **Settings > Pages > Build and deployment**, select **GitHub Actions** as the source.
3. Add each package repository as described below, commit, and push to `main`.

The `Build and deploy package repository` workflow builds Satis and publishes it at:

```text
https://mozielin.github.io/ryanspack
```

Pull requests and non-`main` pushes run the same validation and build without deploying.

## Add a package

Each package needs a valid `composer.json`, a unique `name` such as `acme/laravel-tools`, and Git tags for the versions consumers may install.

Add its Git repository to the `repositories` array in `satis.json`:

```json
{
    "type": "vcs",
            "url": "https://github.com/mozielin/laravel-tools.git"
}
```

For example, with two package sources:

```json
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/mozielin/laravel-tools.git"
    },
    {
        "type": "vcs",
        "url": "https://github.com/mozielin/payment-kit.git"
    }
]
```

`require-all` is enabled, so every tagged version discovered in these repositories is published automatically. Push changes to `main` whenever you add a package or want Satis to refresh package metadata.

## Use from Laravel

Add the published repository before Packagist in the consuming application's `composer.json`:

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://mozielin.github.io/ryanspack"
        }
    ]
}
```

Then install a tagged package normally:

```bash
composer require acme/laravel-tools:^1.0
```

## Private source repositories

GitHub Pages can serve the package metadata publicly, but Composer must still be able to clone each package source. For private GitHub package repositories, configure the consuming machine or CI with a GitHub token that has read access:

```bash
composer config --global --auth github-oauth.github.com YOUR_GITHUB_TOKEN
```

For entirely private package metadata, use a private registry host rather than public GitHub Pages.
