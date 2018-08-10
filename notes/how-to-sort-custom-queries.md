# How to Sort Custom Queries

In order to sort custom queries, you will need to set the array parameter in your customer query to `'orderby' => 'post_date'`. `post_date` is the default action by WordPress but you have the option to change it. `rand` will randomize the results.

```php
<?php
  $homepageEvents =  new WP_Query(array(
    'posts_per_page' =>  -1, // => -1 is our way of telling WordPress to give us all post that meet the conditions of the query
    'post_type' => 'event',
    'orderby' => 'title', // => `title` will order all post alphabetically by title
    'order' => 'ASC'
  ));

```

You can use parameters `meta_key`, `meta_value`, and `meta_value_num` to set the order by using a custom field.

```php
`meta_key` => `name_of_the_custom_field_we_are_interested_in` // => must be used in conjunction with `meta_value`
`order_by` => `meta_value` //=> set the order up by using a custom field
```

## Meta_query

You can chain together instructions for sorting using the `meta_query` parameter.

```php
$today = date('Ymd');
$homepageEvents =  new WP_Query(array(
  'posts_per_page' => -1,
  'post_type' => 'event',
  'meta_key' => 'event_date',
  'orderby' => 'meta_value_num',
  'order' => 'ASC',
  'meta_query' => array( // => 'meta_query' is used to sort custom queries with the comparison operators.
    array(
      'key' => 'event_date',
      'compare' => '>=',
      'value' => $today,
      'type' => 'numeric'
    )
  )
));
```
