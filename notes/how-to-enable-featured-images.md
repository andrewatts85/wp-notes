# How to Enable Featured Images

By default, featured images are not supported by themes. Follow these steps to enable featured images.

1. Navigate to `functions.php` in your text editor
2. To add support for featured images use the `add_theme_support()` method with the `post-thumbnails` argument

```php
function university_features() {
  add_theme_support('title-tag');
  add_theme_support('post-thumbnails'); // This method will add theme support for images 
}

add_action('after_setup_theme', 'university_features');
```

**Note:** This alone will **only** enable featured images for blog post but for our *custom post-types* we have to do some extra steps.

3. Navigate to your `mu-plugins` folder
4. In your custom post-types file add on `thumbnail` to the `supports` array of your target `register_post_type`

```php
function university_post_types() {
  // Professor Post Type
  register_post_type('professor', array(
    'supports' => array('title', 'editor', 'thumbnail'), // add the thumbnail parameter here
    'public' => true,
    'menu_icon' => 'dashicons-welcome-learn-more',
    'labels' => array(
      'name' => 'Professors',
      'add_new_item' => 'Add New Professor',
      'edit_item' => 'Edit Professor',
      'all_items' => 'All Professors',
      'singular_name' => 'Professor'
    )
  ));
}

add_action('init', 'university_post_types');

```

## How to Output Featured Images on the Frontend

5. Navigate to the `single-*.php` file of your custom post-type
6. Use the WordPress function `the_post_thumbnail()` to access the featured image set by the post

```php
<div class="row">
  <div class="col">
    <?php the_post_thumbnail(); ?>
  </div>
  <div class="col">
    <?php the_content(); ?>
  </div>
</div>
```

7. You can also use `the_post_thumbnail_url()` as the source for an `<img>` tag and pull in the image that way

```php
<img src="<?php the_post_thumbnail_url(); ?>">
```

## How to Make Image Size Adjustable

8. You can adjust featured image sizes by editing your `functions.php` file. Use the `add_image_size()` method to do so.  
**Reference:** `add_image_size('customName', 'pxWide', 'pxTall', 'crop')`

```php
// This function provides the title tag to be displayed on browser tabs
// This function also enables featured images on WordPress post
// This function also enables adjustable image sizes
function university_features() {
  add_theme_support('title-tag');
  add_theme_support('post-thumbnails');
  add_image_size('professorLandscape', 400, 260, true); 
  add_image_size('professorPortrait', 480, 650, true);
}

add_action('after_setup_theme', 'university_features');
```

## Retro Actively Create New Image Sizes w/ Regenerate Thumbnails Plugin

9. Download and activate `Regenerate Thumbnails`
10. Hover over the `Tools` option and select `Regen. Thumbnails`
11. Click `Regenerate All Thumbnails` to create new sizes for all images

## How to Use Your Custom Image Files on the Frontend
12. To use your custom image files that are defined in the `functions.php` file by `add_image_size()`, use the custom name from the `add_image_size()` function as an argument for `the_post_thumbnail()` and `the_post_thumbnail_url()` functions

```php
<div>
  <?php the_post_thumbnail('professorPortrait'); ?>
  <img class="professor-card__image" src="<?php the_post_thumbnail_url('professorLandscape'); ?>">
</div>
```

## How to Crop and Position Featured Images w/ Manual Image Crop Plugin

13. Download and activate Manual Image Crop Plugin
14. Now you can navigate to a post with a Featured Image and click `Crop featured image` in the side widget to crop the image
