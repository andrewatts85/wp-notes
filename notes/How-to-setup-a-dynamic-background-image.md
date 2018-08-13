# How to Setup a Dynamic Background Image

In WordPress, post can only have one single featured image. So if you use that on something like a profile pic or background photo, you have to use the *custom field* technique instead.

1. Navigate to the `Custom Field` link in your WordPress admin and create a new field. The plugin is called **Advanced Custom Fields**
2. Name the field group 'Page Banner'
3. Name the field label 'Page Banner Subtitle'
4. Scroll down and click `+ Add Field` to add another field
5. Name it 'Page Banner Background Image'
6. Set the field type to `image`
7. Scroll down to the Location rules and click the `add rule group` button and set it to `Post Type` `is not equal to` `post`. This will allow the field group to be display on all post whether they are custom or generic
8. Now publish the new field group

## How to Display a Dynamic Background Image on the Frontend

9. In your `functions.php` file add the image size for the page banner to display on the frontend

```php
// This functions enables featured images on WordPress post
function university_features() {
  add_theme_support('post-thumbnails');
  add_image_size('professorLandscape', 400, 260, true);
  add_image_size('professorPortrait', 480, 650, true);
  add_image_size('pageBanner', 1500, 350, true); // Add the image size for the page banner
}

  add_action('after_setup_theme', 'university_features');
```

10. Refer to the code below closely to output the field results to the frontend. Use `get_field()` and `the_field()`. This code should be written in your `single-*.php` files. This goes inside the WordPress loop. Ideally you would want to put this everywhere you want to be able to edit the background image. Maybe a function should be used instead?

```php
<div class="banner">
  <div class="banner__bg-image" style="background-image: url(<?php $pageBannerImage = get_field('page_banner_background_image'); echo $pageBannerImage['sizes']['pageBanner'] ?>);"></div>
  <div class="banner__subtitle" >
    <p><?php the_field('page_banner_subtitle'); ?></p>
  </div>
</div>
```
