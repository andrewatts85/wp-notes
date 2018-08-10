# How to Create Custom Queries

Custom queries allow us to load whatever we want, wherever we want. For example, say you want to include the most recent two or three blog posts in a side menu on your Homepage, that would require a custom querie.  

## $myQuery = new WP_Query();  
The trick to loading a custom query involves creating a new instance of `WP_Query()`. You can tell WordPress what you want it to load by providing an associative array as a parameter. The following code below will load the two most recent blog posts.

```php
<?php
  $homepagePosts = new WP_Query(array(
    'posts_per_page' =>  2,
    'category_name' => 'awards'
  ));

  while($homepagePosts->have_posts()) {
    $homepagePosts->the_post(); ?>
    <li><?php the_title(); ?></li>
  <?php }
?>
```

Note: `->` means to look inside. Now, `posts_per_page` is just one of many parameters you can use. `'post_type' => 'page'` will query a list of your pages. Remember, queries have to be used along with the WordPress loop.

```php
// This is a custom query
$homepagePosts = new WP_Query(array(
  'posts_type' =>  'page'
));
```

## Tip

You can use the WordPress function `wp_trim_words()` to trim the content. It takes two arguments. The first argument is the content you want to limit. The second argument is the number of words you want to limit it to.

```php
<p><?php echo wp_trim_words(get_the_content(), 18); ?></p>
```

## wp_reset_postdata()

After you've created your custom query and used it in a while loop, right after that while loop ends, you must get in the habit of calling `wp_reset_postdata()`. This will reset WordPress data and global variables back to normal. It's not 100% necessary but should be done anyway.

```php
<?php
  $homepagePosts = new WP_Query(array(
    'posts_per_page' =>  2
  ));

  while($homepagePosts->have_posts()) {
    $homepagePosts->the_post(); ?>
    <li><?php the_title(); ?></li>
  <?php } wp_reset_postdata();
?>
```
