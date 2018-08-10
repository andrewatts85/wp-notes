# Rebuild WordPress Permalink Structure

When you create a new custom post-type, WordPress will need to update it's permalink structure if you want post created from that custom post-type to work properly. WordPress doesn't update everything on every save so it would have no idea that you created a custom post-type unless you manually rebuild the WordPress permalink structure. It's pretty easy to do just follow these 2-step instructions:

1. Navigate to your `wp-admin`, go to `Settings` and click `Permalinks`
2. Scroll down and click `Save Changes`

Now that the permalinks are updated you should be able to navigate to post with no issues when using WordPress functions like `the_permalink()`.  

## single-[insert custom post-type here].php

You can power custom post-types with a custom template by creating a file called `single-[post-type].php`. So for instance say you created a new custom post-type called `Events`. To make a new `single.php` template for that, you would create a new file in your custom theme folder called `single-event.php`. Start off by including the header and footer then style as you please.

## rewrite, has_archive, & supports

Navigate to `mu-plugins`. You can edit your custom post-type and add `has_archive`, `rewrite`, and `supports` to setup the archive properly.


```php
<?php

function university_post_types() {
  register_post_type('event', array(
    'supports' => array('title', 'editor', 'excerpt'), => will enable support for `excerpts`.
    'rewrite' => array('slug' => 'events'), // => with this you can create a custom slug whether it be plural or singular
    'has_archive' => true,  // => will provide the archive url 
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

## archive-[insert custom post-type here].php

Create a custom archive for your post-type by creating a new file in your theme call `archive-events.php` or whatever the name of your post-type is. Then copy over the code from the regular `archive.php` file and start from there.

## echo get_post_type_archive_link('event')

After setting up the archive you can use `get_post_type_archive_link('name of custom post-type')` instead for urls in `single-[insert custom post-type].php`
