# How to Reduce Duplicate Code

You can define a function in your `functions.php` file and use it throughout all your WordPress page files.

1. Navigate to `functions.php` and create a function that includes the markup you want to avoid duplicating

```php
<?php

  function pageBanner() {
    // php logic will live here
    ?>
    <div class="page-banner">
      <div class="page-banner__bg-image" style="background-image: url(<?php $pageBannerImage = get_field('page_banner_background_image'); echo $pageBannerImage['sizes']['pageBanner'] ?>);"></div>
      <div class="page-banner__content container container--narrow">
        <h1 class="page-banner__title"><?php the_title(); ?></h1>
        <div class="page-banner__intro">
          <p><?php the_field('page_banner_subtitle'); ?></p>
        </div>
      </div>
    </div>
  <?php }
```

2. To display on the frontend, reference the below code. Ideally this code will be placed inside the WordPress loop
```php
<?php pageBanner(); ?>
```

## Create Functions w/ Arguments

3. Reference the below code. Utilize the `$args` parameter (can be named anything). `echo` out the `$args['param']` to display in your html. **Note:** Setting `$args` to `NULL` will make the argument optional instead of required

```php
// functions.php
<?php

  function pageBanner($args = NULL) {
    // php logic will live here
    // you can setup fallbacks with if statements
    if (!$args['title']) {
      $args['title'] = get_the_title();
    }

    if (!$args['subtitle']) {
      $args['subtitle'] = get_field('page_banner_subtitle');
    }

    if (!$args['photo']) {
      if (get_field('page_banner_background_image')) {
        $args['photo'] = get_field('page_banner_background_image')['sizes']['pageBanner'];
      } else {
        $args['photo'] = get_theme_file_uri('/images/ocean.jpg');
      }
    }

    ?>

    <!-- html markup will live here -->
    <div class="page-banner">
      <div class="page-banner__bg-image" style="background-image: url(<?php echo $args['photo']; ?>);"></div>
      <div class="page-banner__content container container--narrow">
        <h1 class="page-banner__title"><?php echo $args['title'] ?></h1>
        <div class="page-banner__intro">
          <p><?php echo $args['subtitle']; ?></p>
        </div>
      </div>
    </div>
  <?php }
```

```php
// page.php
<?php

  get_header();

  // WordPress loop
  while(have_posts()) {
    the_post();
    
    pageBanner(array(
      'title' => '',
      'subtitle' => '',
      'photo' => 'https://png.pngtree.com/thumb_back/fw800/back_pic/00/06/31/695628f664c006c.jpg'
    ));
    ?>
```
