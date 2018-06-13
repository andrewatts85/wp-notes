# Create the Header

Creating a header and footer with Wordpress is like chopping the HTML document in half.  
1. Open a new file in your theme and name it `header.php`
2. Include the following code:
```php
<!DOCTYPE html>
<html>
  <head>
    <?php wp_head(); ?>
  </head>
  <body>
    <h1>This is the Header</h1>
```
3. In the `<head>` be sure to enter php mode and include the `wp_head()` function. This function will pull in your css and js. Keep in mind that `wp_head()` will be set up in `functions.php` in your theme.

# Create the Footer

To create the footer we are going to finish the adding the rest of the closing tags to the `footer.php` file.  
1. Open a new file in your theme and name it `footer.php`
2. Include the following code:
```php
    <h1>This is the Footer</h1>
    <?php wp_footer(); ?>
  </body>
</html>
```
3. Be sure to enter php mode and include the `wp_footer()` function right before the closing `</body>` tag. This will load your JS files last. More importantly it will add the `Black Admin Bar` to the top of your website.

# Load your CSS file

1. Create a new `functions.php` file in your theme. No need to close the `<php` tag since this file is always in php mode.
```php
  <?php
  
  /* You can load as many CSS or JS files as you like in this function */
  function university_files() {
    
    /* The first param name does not matter it just needs to make sense. The second param needs the stylesheet url */
    wp_enqueue_style('university_main_styles', get_stylesheet_uri());
  }
  
  /* This function-method instructs WordPress on what it should do */
  /* The first param is a special WordPress `hook name`, the second param is a function that you create (you can name it whatever) */
  add_action('wp_enqueue_scripts', 'university_files')
```
2. Use the WP function `get_stylesheet_uri()` to get the url for your stylesheet.
