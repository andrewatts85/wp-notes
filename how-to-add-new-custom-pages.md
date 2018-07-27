# How to Add New Custom Pages

There are two ways to add a new page to your WordPress admin that you can custom code from the ground up. You can either use the slug from the new page's permalink in your WordPress admin or you can reference the Page Attributes and set the new page's template.

## Slug way

1. Create a new page in your WordPress admin. Enter a title and publish the page.
2. In your text editor, add a new file to the custom theme folder. Reference the permalink from the page and name it `page-enter-slug-from-page-here.php`
3. Now anything you code in that file will show up on the new page you just made.
4. View the page permalink to see the results.

## Template way

1.Create a new page in your WordPress admin. Enter a title and publish the page.  
2. In your text editor, add a new file to the custom theme folder. Name it `enter-page-title-here.php`.  
3. Next, add a comment at the top of the file. WordPress will parse this comment and enable the `Template` options in the   `Page Attributes` section.
```php
<?php
 /*
  * Template Name: name-of-page
  */
  get_header(); ?>
```
4. Go to your WordPress admin and set the Template to the file name of the new page you just created.  
5. Now anything you code in that file will show up on the new page you just made.  
6. View the page permalink to see the results.
