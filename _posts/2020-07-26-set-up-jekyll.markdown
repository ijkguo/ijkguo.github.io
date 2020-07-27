---
layout: post
title:  "Set up Jekyll"
date:   2020-07-26 17:17:41 -0700
categories: jekyll
---

After creating the repo, [doc](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll):

1. Navigate to https://ijkguo.github.io immediately to view the default README.md served as a webpage.

1. (repo index) Settings -> Options -> GitHub Pages. Confirm that "Your site is published". Note that Source cannot be changed, because the demo project is a "User Pages" (e.g. https://ijkguo.github.io). See this [stackoverflow](https://stackoverflow.com/questions/25559292/github-page-shows-master-branch-not-gh-pages) on other options like "gh-pages branch", "master branch /docs folder" for project pages (e.g. https://ijkguo.github.io/nginx).

1. (repo index) Code -> Environments -> "github-pages" will link to "Deployments / Activity log" where we can see continuous build and roll out status. The documentation described changes to be effective within 20 minutes, and failures are handled by emails. But this is an up-to-date deployment status.

Setting up Jekyll correctly, [doc](https://jekyllrb.com/docs/):

1. Set up gem no-sudo [here](https://jekyllrb.com/docs/troubleshooting/#no-sudo):
```bash
export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH
```

1. Install Jekyll and bundler by `gem install jekyll bundler`.

1. Initialize a new Jekyll site `jekyll new . --force`.
* Do not use `docs` folder. User Pages requires us to use the top level folder.
* Do not use `bundle exec jekyll` because `Gemfile` will be generated automatically by `jekyll`.
* Delete the autogenated `README.md` so that the Jekyll website will be rendered.

1. View local rendered website: `bundle exec jekyll serve`.

Commit changes, [doc](https://docs.github.com/en/github/using-git/caching-your-github-credentials-in-git):

1. Set up credential caching by `git config --global credential.helper osxkeychain`.

1. Enter credentials for the first time. No need to use Keychain Access to manually enter the credential. Crendential can be viewed at the `local`, not `iCloud` keychain.
