# How to Display Relationships on the Frontend

1. After setting up your custom field, pay special attention to the `Post Type` and `Location` rules you set up for it.  
2. Selecting a `Post Type` for that rule will give the custom field you created a list for accessing all the post from the selected `Post Type` rule. These Post can be used as categories for your relationships  
3. The `Location` you set up for the custom field to show up at will be the `single-*.php` file you want to edit. Ex: `single-event.php`  

## Display the Custom Field w/ `getField()`
4. Navigate to `single-*.php` in your text editor. Go to the line you want to display the info and enter php mode.
5. Create a new variable to hold `get_field('enter_Field_Name')`. You can look up the field name of your custom field in your WordPress admin. If you want to see what lives inside a variable, php has a function called `print_r($variable)` that will give you information on that variable

```php
<?php
  $relatedPrograms = getField('related_programs'); // Array
  // print_r($relatedPrograms);
?>
```

6. Now that we know its an array, we know we can loop through it with a `foreach()` loop
7. Use `get_the_title()` and pass a parameter to access the title. Note that only certain WP function workin the main WP loop and won't work in a `foreach()` loop. Also functions that begin with "get" won't handle the `echo` for you so be sure to `echo` it out

```php
<?php
  $relatedPrograms = getField('related_programs');
  foreach($relatedPrograms as $program) {
    echo get_the_title($program);
  }
?>
```

8. From here you are able to style and format the output by flipping back from php to html mode and so forth

```php
<?php
  if ($relatedPrograms) {
    echo '<hr class="section-break">';
    echo '<h2 class="headline headline--medium">Related Program(s)</h2>';
    echo '<ul class="link-list min-list">';
    
    foreach($relatedPrograms as $program) { ?>
      <li><a href="<?php echo get_the_permalink($program); ?>"><?php echo get_the_title($program); ?></a></li>
    <?php }
    
    echo '</ul>';
  }
?>
```
