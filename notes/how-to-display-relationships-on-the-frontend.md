# How to Display Relationships on the Frontend

1. After setting up your custom field, pay special attention to the `Post Type` and `Location` rules you set up for it.  
2. The `Location` you set up for the custom field to show up at will be the `single-*.php` file you want to edit. Ex: `single-event.php`  
3. Selecting a `Post Type` for that rule will give the custom field you created a list for accessing all the post from the selected `Post Type` rule. These Post can be used as categories for your relationships  

## Display the Custom Field w/ `getField()` (Location)
4. Navigate to `single-*.php` in your text editor. Go to the line you want to display the info and enter php mode.
5. Create a new variable to hold `get_field('enter_Field_Name')`. You can look up the field name of your custom field in your WordPress admin. If you want to see what lives inside a variable, php has a function called `print_r($variable)` that will give you information on that variable

```php
<?php
  $relatedPrograms = getField('related_programs'); //=> Array
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
9. Make sure you only run the code if it actually does have related programs or WordPress will throw an error. You can do so with an `if` statement to avoid this

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

## Display the Custom Field (Post Type)

The post that the related links link to on the other end are not yet aware of its related post. So lets fix that. We can create a Custom Field for the other end to point back to the `Location` but that would require double the work. The solution is to use a `custom querie` to reference the relationship.  

10. Navigate to the `single-*.php` file in your text editor. Subsitute the `*` for the `Post Type` you selected when you set up your custom field. Ex: `single-program.php` Begin coding your `custom querie` on the line you want it to show up on

```php
<?php
  // Custom query
  $today = date('Ymd');
  $homepageEvents =  new WP_Query(array(
    'posts_per_page' => 2,
    'post_type' => 'event',
    'meta_key' => 'event_date',
    'orderby' => 'meta_value_num',
    'order' => 'ASC',
    
    // Filter that only returns upcoming post (events)
    'meta_query' => array(
      array(
        'key' => 'event_date',
        'compare' => '>=',
        'value' => $today,
        'type' => 'numeric'
      ),
      
      // Another filter that only returns post that mention a relationship to the current program
      array(
        'key' => 'related_programs',
        'compare' => 'LIKE',
        'value' => '"' . get_the_ID() . '"'
      )
    )
  ));
  
  // WordPres loop 
  while($homepageEvents->have_posts()) {
    $homepageEvents->the_post(); ?>
    <div class="event-summary">
      <a class="event-summary__date t-center" href="<?php the_permalink(); ?>">
        <span class="event-summary__month"><?php
          $eventDate = new DateTime(get_field('event_date'));
          echo $eventDate->format('M');
        ?></span>
        <span class="event-summary__day"><?php echo $eventDate->format('d'); ?></span>
      </a>
      <div class="event-summary__content">
        <h5 class="event-summary__title headline headline--tiny"><a href="<?php the_permalink(); ?>"><?php the_title(); ?></a></h5>
        <p><?php if (has_excerpt()) {
          echo get_the_excerpt();
        } else {
          echo wp_trim_words(get_the_content(), 18);
        } ?> <a href="<?php the_permalink(); ?>" class="nu gray">Learn more</a></p>
      </div>
    </div>

  <?php } wp_reset_postdata();
?>
```
