## How to Setup Search Logic Thats Aware of Relationships

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

function universityRegisterSearch() {
  register_rest_route('university/v1', 'search', array(
    'methods' => WP_REST_SERVER::READABLE,
    'callback' => 'universitySearchResults'
  ));
}

function universitySearchResults($data) {
  $mainQuery = new WP_Query(array(
    'post_type' => array('post', 'page', 'professor', 'program', 'campus', 'event'),
    's' => sanitize_text_field($data['term'])
  ));

  $results = array(
    'generalInfo' => array(),
    'professors' => array(),
    'programs' => array(),
    'events' => array(),
    'campuses' => array()
  );

  while($mainQuery->have_posts()) {
    $mainQuery->the_post();

    if (get_post_type() == 'post' OR get_post_type() == 'page') {
      array_push($results['generalInfo'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'postType' => get_post_type(),
        'authorName' => get_the_author()
      ));
    }

    if (get_post_type() == 'professor') {
      array_push($results['professors'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'image' => get_the_post_thumbnail_url(0, professorLandscape)
      ));
    }

    if (get_post_type() == 'program') {
      array_push($results['programs'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'id' => get_the_id()
      ));
    }

    if (get_post_type() == 'campus') {
      array_push($results['campuses'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'event') {
      $eventDate = new DateTime(get_field('event_date'));
      $description = null;
      if (has_excerpt()) {
        $description = get_the_excerpt();
      } else {
        $description = wp_trim_words(get_the_content(), 18);
      }
      array_push($results['events'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'month' => $eventDate->format('M'),
        'day' => $eventDate->format('d'),
        'description' => $description
      ));
    }

  }
  
  // Search logic aware of relationships
  $programsMetaQuery = array('relation' => 'OR');

  foreach($results['programs'] as item) {
    array_push($programsMetaQuery, array(
      'key' => 'related_programs',
      'compare' => 'LIKE',
      'value' => '""' . $item['id'] . '""'
    ));
  }

  $programRelationshipQuery = new WP_Query(array(
    'post_type' => 'professor',
    // you can search based on a value of a custom field with meta_query
    // you can string together multiple filters with each nested array which will be nested in the outer array
    'meta_query' => array(
      'relation' => 'OR',
      array(
        'key' => 'related_programs',
        'compare' => 'LIKE',
        'value' => '""' . $results['programs'][0]['id'] . '""'
      )
    )
  ));

  while($programRelationshipQuery->have_posts()) {
    $programRelationshipQuery->the_post();

    if (get_post_type() == 'professor') {
      array_push($results['professors'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'image' => get_the_post_thumbnail_url(0, professorLandscape)
      ));
    }
  }

  // remove duplicates with array_unique('your target array', 'a func that tells it to play nice with associat arrays')
  // to get rid of the added array keys (numbers) wrap the array in array_values()
  $results['professors'] = array_values(array_unique($results['professors'], SORT_REGULAR));

  return $results;
}

```

## Modified w/ if statement and loop

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

function universityRegisterSearch() {
  register_rest_route('university/v1', 'search', array(
    'methods' => WP_REST_SERVER::READABLE,
    'callback' => 'universitySearchResults'
  ));
}

function universitySearchResults($data) {
  $mainQuery = new WP_Query(array(
    'post_type' => array('post', 'page', 'professor', 'program', 'campus', 'event'),
    's' => sanitize_text_field($data['term'])
  ));

  $results = array(
    'generalInfo' => array(),
    'professors' => array(),
    'programs' => array(),
    'events' => array(),
    'campuses' => array()
  );

  while($mainQuery->have_posts()) {
    $mainQuery->the_post();

    if (get_post_type() == 'post' OR get_post_type() == 'page') {
      array_push($results['generalInfo'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'postType' => get_post_type(),
        'authorName' => get_the_author()
      ));
    }

    if (get_post_type() == 'professor') {
      array_push($results['professors'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'image' => get_the_post_thumbnail_url(0, professorLandscape)
      ));
    }

    if (get_post_type() == 'program') {
      array_push($results['programs'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'id' => get_the_id()
      ));
    }

    if (get_post_type() == 'campus') {
      array_push($results['campuses'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'event') {
      $eventDate = new DateTime(get_field('event_date'));
      $description = null;
      if (has_excerpt()) {
        $description = get_the_excerpt();
      } else {
        $description = wp_trim_words(get_the_content(), 18);
      }
      array_push($results['events'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink(),
        'month' => $eventDate->format('M'),
        'day' => $eventDate->format('d'),
        'description' => $description
      ));
    }

  }

  // Search logic thats aware of relationships modified with an if statement and loop
  if($results['programs']) {
    $programsMetaQuery = array('relation' => 'OR');

    foreach($results['programs'] as $item) {
      array_push($programsMetaQuery, array(
        'key' => 'related_programs',
        'compare' => 'LIKE',
        'value' => '' . $item['id'] . ''
      ));
    }

    $programRelationshipQuery = new WP_Query(array(
      // use 'post_type' => array('professor', 'event') for multiple post-types
      'post_type' => 'professor',
      // you can search based on a value of a custom field with meta_query
      // you can string together multiple filters with each nested array which will be nested in the outer array
      'meta_query' => $programsMetaQuery
    ));

    while($programRelationshipQuery->have_posts()) {
      $programRelationshipQuery->the_post();

      if (get_post_type() == 'professor') {
        array_push($results['professors'], array(
          'title' => get_the_title(),
          'permalink' => get_the_permalink(),
          'image' => get_the_post_thumbnail_url(0, professorLandscape)
        ));
      }
    }

    // remove duplicates with array_unique('your target array', 'a func that tells it to play nice with associat arrays')
    // to get rid of the added array keys (numbers) wrap the array in array_values()
    $results['professors'] = array_values(array_unique($results['professors'], SORT_REGULAR));
  }

  return $results;
}
```
