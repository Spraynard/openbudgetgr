# Open Budget: Oakland

## Developing Locally

### Harp

This site is built on Harp using Node.js That means you can run it locally with minimal setup!

What you'll need:

-  [Node](http://nodejs.org/download/)
-  [npm](https://www.npmjs.org/)
-  [Harp](http://harpjs.com/)


### Install & Run Harp

Once you have npm installed, you can install Harp

```
# to install harp for the first time
npm install harp -g
```

```
# To start the Harp server, cd to the _src directory
cd [repo-location]/_src
harp server
```

## Making Changes

This project is coded with:

- [jade](http://jade-lang.com/)
- [Sass](http://sass-lang.com/)
- [Bootstrap](http://getbootstrap.com/)


## Creating & Editing Pages

- Page content is inserted into the layout.jade file (which includes basic header and footer snippets)
- Create your .jade file
- Add a link to the main nav in the appropriate place
- Add relevant metadata in _data.json (page title, page slug (url), ...)
- If your page uses custom page-specific css, add it to a new .scss partial and import it into the main stylesheet. (Make sure to namespace it the same way the others are.)


### Additional instructions for "flow" diagram pages

1. Flow pages are built off a template; copy one of the `*-budget-flow.jade` pages and update the content blocks as necessary.
1. Data files must be placed in the `data/flow` directory. Follow the naming convention seen there or your files won't load properly. You also will need to point your page at the appropriate files as seen in the `get_datafiles` content block.
1. the following columns are required in your datafile and their names should be normalized as seen here. Other columns should be removed to minimize the data download.
    - budget_year
    - department
    - fund_code
    - account_type (this should be the Expense/Revenue column, if there are duplicate names)
    - account_category
    - amount

### Additional instructions for treemap diagram pages

1. Treemap pages are built off a template; copy one of the `*-budget-tree.jade` pages and update the content blocks as necessary.
1. Instructions for generating the necessary data files can be found [here](_treemap/README.md). Add them to the `data/tree/` directory following the naming convention seen in the existing files.
1. Update the `datafiles` content block with the appropriate metadata and file path for the data files you generated.

## Publishing Changes
Make changes on your personal fork or branch. If you have repo access, and your changes are ready for review, you can merge them into the development branch and publish to the staging site for review. You can also publish changes to your own server and merge to development afterwards.

### Publishing to Staging
If you have access to the openoakland repo, you can easily publish a preview of your changes to [staging.openbudgetoakland.org](http://staging.openbudgetoakland.org) with the script below.

```
# Run shell script to publish changes from your current branch to the staging 
# Because of path referencing, you'll need to run this script from inside the _src directory for now
bash ../_publish-preview.sh
```

### Publishing to Production

Even though Harp runs locally, static files need to be compiled for the live site (hosted on Github pages).
Once you have made all your changes, you'll need to compile everything in order for it to run on gh-pages. Because of how Harp compiles (that it clears the target directory), this workflow gets a bit wonky. We'll try to make it a little less fragile if people begin publishing changes more often.


```
# make sure your repo is up to date and you are on the master branch
git fetch
git checkout master

# merge your changes from your branch or development into master
git merge origin/development

# here's where it gets hacky - open to suggestions for an improved workflow
# delete the gh-pages branch and then recreate it as an orphan (untracked) branch
git branch -D gh-pages
git checkout --orphan gh-pages

# move into the _src directory and compile source files to the root
cd _src
harp compile ./ ../

# move back to the root, and add and commit files
cd ../
git add -A
git commit -m "deploy"
  
# push changes to remote gh-pages branch using *gasp* --force! 
# !!! Never push --force on any public branch besides gh-pages!
git push --set-upstream origin gh-pages --force  

# make sure your changes are showing up and you didn't break anything
```

# merge your changes from your branch or development into master
git merge origin/development

If you are on a forked branch, create a pull request to have your changes reviewed for merge!

If you are on a forked branch, create a pull request to have your changes reviewed for merge!

##Dynamic Server (UNDER DEVELOPMENT)

If you want to run the site as a dynamic server:

    npm install
    node express-server

So far only the index page is hooked up and a lot is broken.
