# MoMEMta website

## How to use it

The website is generated with [mkdocs](http://www.mkdocs.org). Main configuration file is `mkdocs.yml`. In order to generate the documentation, you first need to install mkdocs (follow special instructions for ingrid first if you need to)

### Cloning the documentation

```
git clone -o upstream -b website git@github.com:MoMEMta/momemta.github.io.git
```

### ingrid special instructions

If you want to deploy from ingrid instead of your own laptop (why?), you need to perform some extra actions:

 1. Load python: `module load python/python27_sl6_gcc49`
 2. Install virtualenv: `pip install --user virtualenv`. You will also need to add `$HOME/.local/bin` to your `$PATH` variable, and `$HOME/.local/lib/python2.6/site-packages` to `$PYTHONPATH`.
 3. Setup the virtual environment: `virtualenv -p /cvmfs/cp3.uclouvain.be/python/python-2.7.9-sl6_amd64_gcc49/bin/python env`
 4. Activate the virtual env: `source env/bin/activate`. You are now isolated from the rest of ingrid python env, and can install all the software you want.
 5. Follow the standard instruction to install mkdocs and the dependencies (see below)

You can exit the virtual environment by executing `deactivate`.

**Note**: you'll need to reload the virtual environment everytime you want to use `mkdocs` by doing `source env/bin/activate`!

### Installing mkdocs and all the dependencies

The dependencies needed for the website are listed in the `requirements.txt` file. Let's install everything with `pip`:

```
pip install -r requirements.txt
```

This will install `mkdocs`, `mkdocs-material` (the theme) as well as some extensions we use.


## Write documentation

Simply add some files in the `docs` folder, in Markdown and add the page in the `mkdocs.yml` file. That's all!

## Preview the website

Run `mkdocs serve` and access the website on http://127.0.0.1:8000. The page is automatically refreshed when you add or modify any file!

**Note**: on ingrid, you'll need to use the command `mkdocs serve -a 0.0.0.0:8000` and use the following url to access the website: http://ingrid-ui1.cism.ucl.ac.be:8000/

## Deploy the website

You can then deploy the website on github pages when you are happy with your changes.

**Note**: this will detroy the old version of the website, so be extremely careful on what you execute.

 1. If you forked and clone the main repository, you need to define the `upstream` remote: `git remote add upstream git@github.com:MoMEMta/momemta.github.io.git`. If you did not, then you'll need to use `origin` instead of `upstream` in the next commands
 2. Deploy the website: `mkdocs gh-deploy -c -r upstream -b master`
 3. Enjoy: http://momemta.github.io/
