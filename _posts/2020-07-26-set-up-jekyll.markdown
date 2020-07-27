---
layout: post
title:  "Set up Jekyll"
date:   2020-07-26 17:17:41 -0700
categories: jekyll
---

## Create the repo

Source: [Creating a GitHub Pages site with Jekyll](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll)

1. (github) Create new repo `ijkguo.github.io` with `README`, `.gitignore=Jekyll` and `LICENSE=APACHE 2.0`.

1. Navigate to https://ijkguo.github.io immediately to view the default README.md served as a webpage.

1. (repo index) Settings -> Options -> GitHub Pages. Confirm that "Your site is published". Note that Source cannot be changed, because the demo project is a "User Pages" (e.g. https://ijkguo.github.io). See this [stackoverflow](https://stackoverflow.com/questions/25559292/github-page-shows-master-branch-not-gh-pages) on other options like "gh-pages branch", "master branch /docs folder" for project pages (e.g. https://ijkguo.github.io/nginx).

1. (repo index) Code -> Environments -> "github-pages" will link to "Deployments / Activity log" where we can see continuous build and roll out status. The documentation described changes to be effective within 20 minutes, and failures are handled by emails. But this is an up-to-date deployment status.

## Setting up Jekyll correctly

Source: [Quickstart Jekyll](https://jekyllrb.com/docs/)

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

1. Update `Gemfile` to use `github-pages` gem. Run `bundle update` to take effect.

1. View local rendered website: `bundle exec jekyll serve`.

## Commit changes

Source: [Caching your GitHub credentials in Git](https://docs.github.com/en/github/using-git/caching-your-github-credentials-in-git)

1. Set up credential caching by `git config --global credential.helper osxkeychain`.

1. Enter credentials for the first time. No need to use Keychain Access to manually enter the credential. Crendential can be viewed at the `local`, not `iCloud` keychain.

## Set up custom domain

Source: [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)

1. Purchase a domain from a registrar, e.g. Google Domains. Use `whois ijkguo.com` to verify registration.

1. (repo index) Settings -> Options -> GitHub Pages. Change Custom domain to `www.ijkguo.com`.

1. From domain registrar, add a CNAME record from `www` subdomain to `ijkguo.github.io` with TTL 1d. Use `dig www.ijkguo.com +nostats +nocomments +nocmd` to verify the CNAME record and the A records automatically populated. Visit http://www.ijkguo.com/ to verify.

1. Set up synthetic record forwarding `ijkguo.com` to `www.ijkguo.com` with 302 temporary direct, forward path, and SSL disabled as described in [stackoverflow](https://webmasters.stackexchange.com/questions/85519/can-i-configure-google-domains-to-redirect-a-bare-domain-to-a-subdomain-over-htt). Use `dig ijkguo.com +nostats +nocomments +nocmd` to verify the A records automatically populated. Apex (root) domain cannot have CNAME, so A records should be generated. Visit http://ijkguo.com/ to verify the redirect.

## Enable TLS on custom domain.

Source: [Securing your GitHub Pages site with HTTPS](https://docs.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https)

Modern browser sometimes reject HTTP-only traffic.

1. (repo index) Settings -> Options -> GitHub Pages. Enable Enforce HTTPS and wait for TLS certificate generation. Visit https://www.ijkguo.com/ to verify.

1. Update synthetic record forward to enable SSL. Wait for TLS certificate generation. Visit https://ijkguo.com/ to verify the redirect.
