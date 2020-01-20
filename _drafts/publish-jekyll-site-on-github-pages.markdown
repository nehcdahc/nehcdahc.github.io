# publish Jekyll site on Github Pages

## Create repository

1. Create a repository named `githubaccountname.github.io`.
1. Initialize this repository with a README and Type a description.
1. Go to your repository’s home page, and click the branch drop-down menu.
1. Create a new branch called gh-pages.
1. Click Settings and change the default branch to gh-pages.

## Config Jekyll site

1. Open the `_config.yml` file and add the following

   ```yml
   url: githubaccountname.github.io
   baseurl: /githubaccountname.github.io
   ```

1. Open `gemfile`, add the github pages gem

   ```yml
   source 'https://rubygems.org'
   gem 'github-pages'
   ```

   ```bash
   bundle install
   ```
