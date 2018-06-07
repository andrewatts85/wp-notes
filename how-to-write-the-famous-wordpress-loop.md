# The Famous WordPress Loop

```php
<?php
  while(have_posts()) {
    the_post(); ?> /* Close the php tag here to exit out of PHP mode so we can enter HTML mode */
    
    <h1>This is a page, not a post.</h1>
    <h2><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h2>
    <?php the_content(); ?>
    
  <?php }
?>
```

**Note:** The WordPress API has built in functions to make life easier. We've used a few in the snippet above.

- `have_post()`
- `the_post()`
- `the_title()`
- `the_content()`
- `the_permalink()`

---

# `single.php` vs `page.php`
[![Screen_Shot_2018-06-06_at_11.04.05_PM.png](https://s15.postimg.cc/80x0ykpqj/Screen_Shot_2018-06-06_at_11.04.05_PM.png)](https://postimg.cc/image/czkjd3tjb/)

WordPress is always on the lookout for different files names in the theme folder. Files named `single.php` and `page.php` are linked to certain permalinks. For example, clicking on a blog post from `index.php` will navigate you to `single.php`. Clicking on the permalink in the pages section of the wordpress admin with take you to `single.php`.
