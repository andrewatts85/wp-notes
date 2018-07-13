# How to Create Custom Fields

There are two ways you can create custome fields. You can do it by editing your `mu-plugins` file or you can use a WordPress plugin. To do it with code just add a `supports` keyword to your associative array and the `custom-fields` item to its array list.

```php
<?php

function university_post_types() {
  register_post_type('event', array(
    'supports' => array('title', 'editor', 'excerpt', 'custom-fields'), // => will enable custom fields also
    'rewrite' => array('slug' => 'events'),
    'has_archive' => true,
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

## Industry Standard (Custom Field) Plugins

- Advanced Custom Fields (ACF)
- CMB2 (Custom Metaboxes 2)
