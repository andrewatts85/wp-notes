# How to Setup Blog Archives (archive.php)

Utilizing the `archive.php` file in your theme folder will allow you to setup dynamic archives for blog posts. This can be accomplished by using `if` statements with the `is_category()` and `is_author()` functions.  

1. Copy the code from `index.php` to `archive.php`.
2. Edit `archive.php` to dynamically update the heading for `archive.php`
3. To get the dynamic name of the category that you're currently viewing call `single_cat_title()`.  
4. To get the dynamic name of the user call `the_author()` function.

```php
<h1 class="page-banner__title"><?php
  if(is_category()) {
    single_cat_title();
  }
  if (is_author()) {
    echo 'Posts by '; the_author();
  }
  ?></h1>
```

# the_archive_title()

A much easier way to setup dynamic archive titles would be to use the `the_archive_title()` function. It will do everything that the previous `if` statement does except you lose a little control over what the title output is. If you want fine grain control over the title then use the previous `if` statement. If not, `the_archive_title()` will do the job just fine.

```php
<h1 class="page-banner__title"><?php the_archive_title(); ?></h1>
```

# the_archive_description()

This function is the brother to `the_archive_title()`, but instead of pulling in the title, it will pull in the biography text for the current archive author.  

```php
<h1 class="page-banner__title"><?php the_archive_title() ?></h1>
<div class="page-banner__intro">
  <p><?php the_archive_description(); ?></p>
</div>
```
## Edit Biography Text

1. Navigate to your WordPress admin dashboard and click on `Users` then `Your Profile`.
2. Edit the `Biographical Info` box then click `Update Profile`.

## Edit Category Description Text

1. Navigate to your WordPress admin dashboard and click `Posts` then click `Categories`.
2. Select a categorgy then edit the `Description`. Click `Update`.
