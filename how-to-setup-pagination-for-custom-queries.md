# How to Setup Pagination for Custom Queries

Write out your custom query an add the `'paged' => get_query_var('paged', 1)` parameter to your query. Or you can include it in an already existing query.

```php
$pastEvents =  new WP_Query(array(
  'paged' => get_query_var('paged', 1), // Gets number from the paged url and fixes pagination issues
));
```

Next add `'total' => $pastEvents->max_num_pages` as a parameter to the `paginate_links()` function. This function should be at the bottom of your WordPress loop like so:

```php
// Bottom of WordPress loop
<?php }
    echo paginate_links(array(
      'total' => $pastEvents->max_num_pages
    ));
  ?>
```
