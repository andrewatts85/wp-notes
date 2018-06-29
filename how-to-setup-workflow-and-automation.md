# Workflow and Automation w/ Gulp

The main goal of automating your workflow is to save time and that's it. `Gulp` seems to be the industry standard for automating task so it would be a good idea to learn the basics of `Gulp`. Answering the following questions will help determine if you might want to automate your workflow or not.  

1. Would you like your browser to auto-refresh anytime yous ave a change to a theme file?
2. Would you like your css (Sass, Less, PostCSS) to be pre/post-processed automatically?
3. Would you like your JS to be compiled and bundled automatically?

If you answered yes to these questions, you should definitely automate your workflow.  

## Node.js

Almost all automation in web development is achieved with `Node.js`. Go to [nodejs.org](https://nodejs.org/en/) and install `Node.js` on your machine. You can check what version of node you have by typing `node -v` in the `terminal`.  

Next use `npm` to install `Gulp`. Reference the example code below for syntax. Depending on if you use a Mac or PC execute this command in your terminal:

```
// Mac
sudo npm install gulp-cli -g

// Windows
npm install gulp-cli -g
```

You can check to see if it's installed by typing this command:

```
gulp -v
```

# Apply These Tools to WordPress

Now that we have `Node.js` and `Gulp`, we just need to apply these tools to our WordPress project so we can benefit from automation. To do this:  

1. Navigate to your WordPress theme folder
2. In the app folder that houses `wp-admin`, `wp-content`, `wp-includes`, and a bunch of other files, you will need to drop in four files
    - `gulpfile.js`
    - `package.json`
    - `settings.js`
    - `webpack.config.js`
3. Now navigate to that same folder using the `terminal`
4. **Note:** The `package.json` file is basically our grocery list. It's a list of all of the different tools and packages that we need.
5. Go to the `terminal` and type:
    ```
    sudo npm install
    ```
6. Open the `settings.js` file in your text editor
7. Set up your exports. Pay special attention to the url and paths:
    ```javascript
    exports.themeLocation = './public/wp-content/themes/exyavier-university-theme/'; // relative to where the settings.js folder lives
    exports.urlToPreview = 'http://exyavier-university.local/';
    ```
8. To begin the automation, run the command:
    ```
    gulp watch
    ```
9. This will open up a new tab in your browser pointing towards `localhost:3000`

Now your browser will automatically refresh and update everytime a change is saved.

## Tip

If your phone and computer are on the same wifi, you can preview your wordpress site on your phone by using a special url from the terminal provided by the `gulp watch` command. Just enter the external url inside the red box into your phones browser to pull up the site.

[![Screen_Shot_2018-06-28_at_5.49.54_PM.png](https://s15.postimg.cc/llmnrfqqj/Screen_Shot_2018-06-28_at_5.49.54_PM.png)](https://postimg.cc/image/gzqjj357b/)

**Note:** To end `gulp watch` type <kbd>ctrl</kbd> + <kbd>c</kbd> in the `terminal`.
