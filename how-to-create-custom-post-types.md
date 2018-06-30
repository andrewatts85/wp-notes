# How to Create Custom Post-Types

By default out of the box WordPress websites only come with two post-types, `post` and `pages`. A page is actually just a post with a special post type of page. This means that all items or entries of content in WordPress are really just posts with different post-type labels. If that doesn't make sense don't worry, by the end of this article you will be creating custom post types in your sleep. You'll be able to create new category post-types in the `wp-admin` menu to the left.  

## Register a New Post-Type

1. Head over to `functions.php` in your theme files. The goal is to register or create a new post-type.
2. Next add an action with the hook `init` for the first argument. Create a function for the second argument (can be named anything). 
3. Inside your function call the `register_post_type()` function. The first argument is the name of the custom post-type that we want to create (can be named anything). The second argument is an associative array of different options that describe your custom post-type.
    - `'public' => true` will make the post-type visible to editors and viewers of the website
    - `'menu_icon' => 'dashicons-calendar-alt'` will set your dashicon, get it [here](https://developer.wordpress.org/resource/dashicons/#editor-paste-text)
    - `'labels' => array('name' => 'Events', ...etc)` will provide the name of your custom post-type
4. After that, navigate to your `wp-admin` dashboard and click refresh. Your custom post-type will show up in the `wp-admin` menu to the left.
    
```php
function university_post_types() {
  register_post_type('event', array(
    'public' => true,
    'menu_icon' => 'dashicons-calendar-alt',
    'labels' => array(
      'name' => 'Events',
      'add_new_item' => 'Add New Event',
      'edit_item' => 'Edit Event',
      'all_items' => 'All Events',
      'singular_name' => 'Event'
    )
  ));
}

add_action('init', 'university_post_types');
```

**Note:** `functions.php` is **not** the absolute best place to keep this custom post-type code. Reason being is if someone switches themes, you will essentially be locked out from editing the post-type associated with that code since it was being kept in the theme folder. We don't want access to our data to be reliant on a certain theme being activated.

## Must Use Plugins

The best way of handling custom post-types is by leveraging something in WordPress called `Must Use Plugins`. These plugins live in their own special dedicated folder and you cannot deactivate them as long as the `php` file exists in the `mu-plugins` folder.

1. Navigate to your local site `wp-content` folder.
2. Create a new folder called `mu-plugins`. Any `php` files that we include within this `mu-plugins` folder will be automatically and always loaded and activated by WordPress.
3. Create a new `php` file in the `mu-plugins` folder and give it a meaningful name such as `university-post-types.php`.
4. Remove your custom post-type function including the `action` from `functions.php` and put it in the new `university-post-types.php` file located in the `mu-plugins` folder we just created.

Now we don't have to worry about accidentally being locked out of editing custom content.
