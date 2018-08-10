# Quick Edits and Improvements (header.php)

These quick edits will give you a few improvements that you can apply to your WordPress site.  

![header.php](https://s15.postimg.cc/47eqr0twb/Screen_Shot_2018-06-19_at_2.26.24_PM.png)

Navigate to `header.php` and insert this code between the head tag:

```html
  <meta name=“viewport” content=“width=device-width, initial-scale=1”>
```

Modify the `html` tag to include the WP function `language_attributes()`. This will dynamically add the language we are working with:

```php
  <html <?php language_attributes(); ?>>
```

This will let the browser know what type of characters, punctuation, and numbers we’ll be using:

```php
  <meta charset="<?php bloginfo('charset'); ?>">
```

This will provide class names for the body that can be used for css and js:

```php
  <body <?php body_class(); ?>>
```
