# How to Edit Default Queries

When manipulating queries in WordPress, it is important to know when to write new custom queries and when to just edit the default query in suttle ways. If there is already a default query in use just edit it to work how you want it to instead of creating a new custom query. This will ensure that your pagination is working correctly.  

To edit a default query, navigate over to your `functions.php` file and add an action (`add_action();`). The name of the WordPress event that we want to hook onto is `pre_get_posts`.

```php
<?php
  function university_adjust_queries($query) {
    if (!is_admin() AND is_post_type_archive('event') AND $query->is_main_query()) {
      $query->set('posts_per_page', '1'); //=> This statement should only affect the event-archive query
    }
  }

  add_action('pre_get_posts', 'university_adjust_queries');
```

If you don't use an if statement that targets the post category you want to edit then the `$query->set('post_per_page', '1')` code will affect your WordPress site globally including `wp-admin`.

### Here is another example with code taken from a custom query.  

#### You can use a custom query as a guide also

```php
<?php
  $today = date('Ymd');
  $homepageEvents =  new WP_Query(array(
    'posts_per_page' => -1,
    'post_type' => 'event',
    'meta_key' => 'event_date',
    'orderby' => 'meta_value_num',
    'order' => 'ASC',
    'meta_query' => array(
      array(
        'key' => 'event_date',
        'compare' => '>=',
        'value' => $today,
        'type' => 'numeric'
      )
    )
  ));
```

#### edit functions.php

```php
<?php
  function university_adjust_queries($query) {
    if (!is_admin() AND is_post_type_archive('event') AND $query->is_main_query()) {
      $today = date('Ymd');
      $query->set('meta_key', 'event_date');
      $query->set('orderby', 'meta_value_num');
      $query->set('order', 'ASC');
      $query->set('meta_query', array(
        array(
          'key' => 'event_date',
          'compare' => '>=',
          'value' => $today,
          'type' => 'numeric'
        )
      ));
    }
  }

  add_action('pre_get_posts', 'university_adjust_queries');
```
