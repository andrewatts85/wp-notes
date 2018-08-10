# Avoid Caching CSS and JS

During WordPress development you may run into a problem when trying to view css updates in your browser. Sometimes you may not be able to see the updates because your machine is using the cached version of your css instead of the new updated version. To fix this there a few things to understand.

# functions.php file

In your `functions.php` file, refer to the function that controls `wp_head()`.

```php
function university_files() {
    wp_enqueue_script('main_university_js', get_theme_file_uri('/js/scripts-bundled.js'), NULL, microtime(), true);
    wp_enqueue_style('google_fonts', '//fonts.googleapis.com/css?family=Roboto+Condensed:300,300i,400,400i,700,700i|Roboto:100,300,400,400i,700,700i');
    wp_enqueue_style('font_awesome', '//maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css');
    wp_enqueue_style('university_main_styles', get_stylesheet_uri(), NULL, microtime());
  }

  add_action('wp_enqueue_scripts', 'university_files');
```

As you can see this function is loading three stylesheets along with one JavaScript file. The `wp_enqueue_style()` and `wp_enqueue_script()` methods take up to five arguments. Specifically, JS files take all five arguments.

```php
  wp_enqueue_script('Make up a description name', 'Enter the file uri', 'Does it have any dependencies?', 'Make up a version number', 'Do you want to load this file right before the closing body tag?');
```

- **First Arg** - Make up a suitible name describing your script or css file
- **Second Arg** - You can use the `get_theme_file_uri()` function to provide the path for the file
- **Third Arg** - Does the file have any dependencies? If no put `NULL`
- **Fourth Arg** - This is where you will avoid caching your css and js files. Put the `microtime()` function here.
- **Fifth Arg** - Put `true` to load the script before the closing body tag

The `microtime()` function will return a unique timestamp in microseconds. **So when you refresh your page to view the new css or JS updates, this will give WordPress a different version number, forcing your machine to NOT use the cached version.**

Note: Don't do this for production sites. You can also use Chrome Dev Tools and disable caching but this will disable caching for EVERYTHING. Not individual files.

# WP Files

- `functions.php`

# WP Fucntions/Code

- `wp_enqueue_script()`
- `wp_enqueue_style()`
- `microtime()`
