## How to Create New Rest API Routes (URL)

By default a custom post type will not be included in the WordPress API. To fix that Navigate to your `mu-plugins` directory.

1. Pick whatever custom post-type you want to modify. Then all we have to do is edit the property of the associative array.
2. Just add `'show_in_rest' => true` to the associative array and this will give the custom post-type rest api capability.

```php
// Professor Post Type
register_post_type('professor', array(

  'show_in_rest' => true, // add new property here
  
  'supports' => array('title', 'editor', 'thumbnail'),
  // 'rewrite' => array('slug' => 'professors'), // We don't need an archive slug
  // 'has_archive' => true, // We don't need an archive either
  'public' => true,
  'menu_icon' => 'dashicons-welcome-learn-more',
  'labels' => array(
    'name' => 'Professors',
    'add_new_item' => 'Add New Professor',
    'edit_item' => 'Edit Professor',
    'all_items' => 'All Professors',
    'singular_name' => 'Professor'
  )
));

add_action('init', 'university_post_types');
```

3. Now you can use something like `http://localhost:3000/wp-json/wp/v2/professor` to send a request for the professor custom post-type

### 4 Reasons Why You Might Create Your Own New Rest API URL

1. Custom search logic
2. Respond with less JSON data (load faster for visitors)
3. Send only 1 getJSON request instead of 6 in our JS
4. Perfect exercise for sharpening PHP skills.

## How to Create your own Rest API URL

1. Open the `functions.php` file
2. It is best to create seperate files then include or require them in `functions.php`
3. Create a new folder called `includes` in your WP Theme folder. Then add a file called `search-route.php`, or whatever you'd like to name it, to that directory
4. Next add `require get_theme_file_path('/includes/search-route.php');` to `functions.php` at the top. This will add that modular code into `functions.php`

```php
<?php
  require get_theme_file_path('/includes/search-route.php');
```

5. Now navigate back to `search-route.php` or the module you just created and refer to the following code to create your new custom route

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

// Ex: http://localhost:3000/wp-json/wp/v2/professor
// Note: The `wp` in the url above is the default WordPress core namespace it should be custom
// Note: We dont want to use the `wp` namespace in our api request url
// Note: The `v2` or version number is just part of the namespace
// Note: `professor` in the url above, would be the route since it is the ending part of the url

function universityRegisterSearch() {
  // first arg is the namespace you want to use. Choose a name that is unique so it doesn't conflict with plugins. Add a `/v1` as a good practice
  // second arg is the route or ending part of the url
  // third arg is an associative array that describes what should happen when someone visits the url
  register_rest_route('university/v1', 'search', array(
    // 'methods' => 'GET', // loads and reads data. It will almost always work
    'methods' => WP_REST_SERVER::READABLE, // use this instead if you want to go the extra mile. Will make sure that it works on any web host out there
    'callback' => 'universitySearchResults' // Make up a usefull function name then create a function with that name below
  ));
}

function universitySearchResults() {
  return 'Congratulations, you created a route.';
}

// Now save file and test out your new url
// http://localhost:3000/wp-json/university/v1/search

?>

```
