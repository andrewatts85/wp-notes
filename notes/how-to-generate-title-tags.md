# How to Generate Title Tags for Each Screen

Navigate to `functions.php`. To make WordPress do something you have to add an action, then create a specific function for that action. The name of the WordPress event that we want to hook onto would be `after_setup_theme`. When you want to enable a feature for your theme, the function you must call is `add_theme_support()`, then tell it what feature you're interested in which would be `title-tag`.

```php
function university_features() {
  add_theme_support('title-tag');
}

add_action('after_setup_theme', 'university_features');
```

## Tip

### site_url()

The `sit_url()` function will automatically give you the root url of your current WordPress site. Anything included as an argument will get added on to it. **If no arguments are provided then the root or home url will be returned.**

```php
<li><a href="<?php echo sit_url('/about-us'); ?>"></a></li>
```
