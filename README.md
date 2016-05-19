# MoMEMta website

## How to use it

The website is generated with [mkdocs](http://www.mkdocs.org). Main configuration file is `mkdocs.yml`. In order to generate the documentation, you first need to install mkdocs. Follow the official instructions for that: http://www.mkdocs.org/#installation

You also need to checkout some submodules (like the theme we use):

```
git submodule init
git submodule update
```

## Write documentation

Simply add some files in the `docs` folder, in Markdown and add the page in the `mkdocs.yml` file. That's all!

## Preview the website

Run `mkdocs serve` and access the website on http://127.0.0.1:8000. The page is automatically refreshed when you add or modify any file!

## Deploy the website

You can then deploy the website on github pages when you are happy with your changes.

**Note**: this will detroy the old version of the website, so be extremely careful on what you execute.

 1. If you forked and clone the main repository, you need to define the `upstream` remote: `git remote add upstream git@github.com:MoMEMta/momemta.github.io.git`. If you did not, then you'll need to use `origin` instead of `upstream` in the next commands
 2. Deploy the website: `mkdocs gh-deploy -c -r upstream -b master`
 3. Enjoy: http://momemta.github.io/
