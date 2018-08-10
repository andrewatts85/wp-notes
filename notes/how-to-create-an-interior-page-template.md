# How To Create an Interior Page Template  

The "Interior Page Template" is the page that will be used over and over to display different content. It will look different from the home or index page but should still have styling that is consistant with the home page. This page is ideal for nav links and such. You can customize this page by editing `page.php` in your custom theme.  

```php
<?php // page.php

  get_header();
  
  while(have_posts()) {
    the_post(); ?>
  
    <!-- Put custom HTML markup here. -->
  
  <?php }
  
  get_footer();
  
?>
```

## Tips  

### get_theme_file_uri()

Use `get_theme_file_uri()` to help define the path for an image asset. This function provides the beginning part of our url by looking into wp_content, themes, and then our custom theme name.

```php
 <div class="page-banner__bg-image" style="background-image: url(<?php echo get_theme_file_uri('/images/ocean.jpg') ?>)"></div>
```

