# How to Setup Dynamic WordPress Menus

You can control the navigation menu by either editing your `header.php` file, or by setting it up using the WordPress API. To set it up using WordPress' API follow these steps:

1. Navigate to `functions.php`
2. If you haven't already, add an action and put `after_setup_theme` for the first argument. When you want to add new menu locations to a theme, `after_setup_theme` is the hook you want to use. The second argument will be the name of the function you create:

```php
function menu_feature() {
  // add WordPress function here
}

add_action('after_setup_theme', 'menu_feature');
```
3. Next, add `register_nav_menu(arg1, arg2)` to your function. The first argument can be a made up name. **The second argument will show up in the WordPress admin screen**. You can make up the name for this argument as well:

```php
function menu_feature() {
  register_nav_menu('headerMenuLocation', 'Header Menu Location');
}

add_action('after_setup_theme', 'menu_feature');
```
4. Comment the nav links out in the `header.php` file. Enter php mode and insert the WordPress function `wp_nave_menu()` like so. Remember, this function takes an associative array as an argument:

```php
<nav class="main-navigation">
  <?php
    wp_nav_menu(array(
      'them_location' => 'headerMenuLocation'
    ));
  ?>

  <!--
  <ul>
    <li><a href="<?php echo site_url('/about-us'); ?>">About Us</a></li>
    <li><a href="#">Programs</a></li>
    <li><a href="#">Events</a></li>
    <li><a href="#">Campuses</a></li>
    <li><a href="#">Blog</a></li>
  </ul>
  -->
</nav>
```

You can set up as many menu locations as you like within the same function:
```php
function university_features() {
  register_nav_menu('headerMenuLocation', 'Header Menu Location');
  register_nav_menu('footerLocationOne', 'Footer Location One');
  register_nav_menu('footerLocationTwo', 'Footer Location Two');
  add_theme_support('title-tag');
}

add_action('after_setup_theme', 'university_features');
```

5. After adding `wp_nav_menu()` to your `header.php` or `footer.php` file, you will be able to access the `Menus` link in your WordPress Admin dashboard in the `Appearance` section. You can now customize menus from the admin dashboard.

## Tip

You can changed the color of active links by modifying the `.current-menu-item` class in your css.

## How to Highlight the Selected Nav Link while Viewing the Selected Page

To highlight the selected nav link while viewing that page you have to use an `if` statement. This is where the `is_page()` WordPress function comes in handy. Make sure you check the slug for the page and insert that as an argument. Follow the code below like so:

```php
<li <?php if(is_page('about-us')) echo 'class="current-menu-item"' ?>><a></a></li>
```

The `current-menu-item` class can be any class just make sure you have the appropriate css rules to change the color.  

This code also works.

```php
<li <?php if (get_post_type() == 'post') echo 'class="current-menu-item"'; ?>><a href="<?php echo site_url('/blog'); ?>">Blog</a></li>
```

## Children Pages

Use this `if` statement to include child pages. When the child page is being viewed. the parent page link will still be highlighted. The numbers 0 and 14 represent the post ID's. The argument 0 can be static but 14 is the post id for the parent page you would like to remain highlighted.

```php
<li <?php if (is_page('about-us') or wp_get_post_parent_id(0) == 14) echo 'class="current-menu-item"'; ?>><a></a></li>
```
