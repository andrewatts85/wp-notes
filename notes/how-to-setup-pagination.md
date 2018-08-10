## How to Setup Pagination

Go to `index.php` in your theme folder. Choose where you want to put the pagination then enter `php mode` and put `echo paginate_links();`. This function will magically generate pagination on the page.  

```php
<?php
  echo paginate_links();
?>
```

## Tip

* single.php => individual posts
* page.php => individual pages
