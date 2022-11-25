# algolia-jekyll-action

![latest-version](https://img.shields.io/github/v/release/dieghernan/algolia-jekyll-action)
![gh-pages](https://img.shields.io/badge/gh—pages-ready-green)
![jekyll](https://img.shields.io/badge/jekyll-%3E%3D%203.6.0-blue)
![ruby](https://img.shields.io/badge/ruby-%3E%3D%202.3.0-blue)

This is a wrapper of the [jekyll-algolia](https://community.algolia.com/jekyll-algolia/) plugin.

### 🎉 GitHub Pages ready!


This action pushes all your content in an Algolia index.

## Repo setup 

Full reference on the [`jekyll-algolia` documentation](https://community.algolia.com/jekyll-algolia/getting-started.html).

### A. Install `jekyll-algolia`

1. On your [Algolia](https://www.algolia.com/) account (free Community plan available), get your credentials from your dashboard.

2. On your `_config.yml`, write:

  ```yaml
  
  algolia:
     application_id: YOUR_APPLICATION_ID
     index_name: YOUR_INDEX_NAME # You can replace that with whatever name you want
     search_only_api_key: YOUR_SEARCH_ONLY_API_KEY
     
  ```
  
3. Add `jekyll-algolia` to your `Gemfile`, as part of the `:jekyll-plugins` group.

  ```ruby
  source 'https://rubygems.org'
  gem 'jekyll', '~> 3.6'
  
  group :jekyll_plugins do
    gem 'jekyll-algolia'
  end
  ```

4. **Important**: On your GitHub repo, go to *Settings > Secrets* and set a new secret:

  | Name  | Value |
  | --- | --- |
  | `ALGOLIA_API_KEY` | `YOUR_ADMIN_API_KEY` |

### B. Create a GitHub Action

Create a workflow file (e.g. `algolia-search.yml`) in `your-repo/.github/workflows/` directory, similar to:

``` yaml
on:
  push:
    branches:
      - master
      - main

name: algolia-search
jobs:
  algolia-search:
    runs-on: ubuntu-latest
    env:
      JEKYLL_GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    steps:
      - uses: actions/checkout@v3
      - name: Algolia Jekyll Action
        uses: dieghernan/algolia-jekyll-action@main
        with:
          APIKEY: '${{ secrets.ALGOLIA_API_KEY }}'

```

This action would run on every commit on the `main/master`. For more trigger events see [this link](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#configuring-workflow-events).

## Inputs available

-   `APIKEY`: Required. It is better to use GitHub Secrets (see above) instead the actual value on the action, for security reasons.
-   `ruby-version`: Version of Ruby to use on build. Default is `'2.6'`.
-   `cache-version`: default `1`. If you need to invalidate the existing cache pass any other number and a new cache will be used.
-   `jekyll_src`: default `""`. Jekyll website source directory. Use it if the source of your site is in a subfolder (e.g `docs`, etc.).


## Disclaimer

This software is in no way officially related to or endorsed by Algolia.



