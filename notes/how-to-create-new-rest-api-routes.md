## How to Create New Rest API Routes (URL)

By default a custom post type will not be included in the WordPress API. To fix that Navigate to your `mu-plugins` directory.

1. Pick whatever custom post-type you want to modify. Then all we have to do is edit the property of the associative array.
2. Just add `'show_in_rest' => true` to the associative array and this will give the custom post-type rest api capability.

```php
// Professor Post Type
register_post_type('professor', array(

  'show_in_rest' => true, // add new property here
  
  'supports' => array('title', 'editor', 'thumbnail'),
  // 'rewrite' => array('slug' => 'professors'), // We don't need an archive slug
  // 'has_archive' => true, // We don't need an archive either
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

add_action('init', 'university_post_types');
```

3. Now you can use something like `http://localhost:3000/wp-json/wp/v2/professor` to send a request for the professor custom post-type

### 4 Reasons Why You Might Create Your Own New Rest API URL

1. Custom search logic
2. Respond with less JSON data (load faster for visitors)
3. Send only 1 getJSON request instead of 6 in our JS
4. Perfect exercise for sharpening PHP skills.

## How to Create your own Rest API URL

1. Open the `functions.php` file
2. It is best to create seperate files then include or require them in `functions.php`
3. Create a new folder called `includes` in your WP Theme folder. Then add a file called `search-route.php`, or whatever you'd like to name it, to that directory
4. Next add `require get_theme_file_path('/includes/search-route.php');` to `functions.php` at the top. This will add that modular code into `functions.php`

```php
<?php
  require get_theme_file_path('/includes/search-route.php');
```

5. Now navigate back to `search-route.php` or the module you just created and refer to the following code to create your new custom route

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

// Ex: http://localhost:3000/wp-json/wp/v2/professor
// Note: The `wp` in the url above is the default WordPress core namespace it should be custom
// Note: We dont want to use the `wp` namespace in our api request url
// Note: The `v2` or version number is just part of the namespace
// Note: `professor` in the url above, would be the route since it is the ending part of the url

function universityRegisterSearch() {
  // first arg is the namespace you want to use. Choose a name that is unique so it doesn't conflict with plugins. Add a `/v1` as a good practice
  // second arg is the route or ending part of the url
  // third arg is an associative array that describes what should happen when someone visits the url
  register_rest_route('university/v1', 'search', array(
    // 'methods' => 'GET', // loads and reads data. It will almost always work
    'methods' => WP_REST_SERVER::READABLE, // use this instead if you want to go the extra mile. Will make sure that it works on any web host out there
    'callback' => 'universitySearchResults' // Make up a usefull function name then create a function with that name below
  ));
}

function universitySearchResults() {
  // this is where WP will determine what kind of JSON data will be served
  return 'Congratulations, you created a route.';
}

// Now save the file and test out your new url
// http://localhost:3000/wp-json/university/v1/search

?>

```

## How to Create Your Own Raw JSON Data

1. Look in your `includes` folder and open `search-route.php`
2. Note: WordPress will automatically convert php data to JSON data by itself
3. Refer to the following code and comments

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

function universityRegisterSearch() {
  register_rest_route('university/v1', 'search', array(
    'methods' => WP_REST_SERVER::READABLE, // use this instead if you want to go the extra mile. Will make sure that it works on any web host out there
    'callback' => 'universitySearchResults' // Make up a usefull function name then create a function with that name below
  ));
}

function universitySearchResults() {
  // custome queries are useful for getting data
  $professors = new WP_Query(array(
    'post_type' => 'professor'
  ));

  // return $professors->posts; // will return the 10 most recent posts for professors. You can stop right here or use the code below instead to display the exact data that you want

  // put the data you want into this array for access later as a custom JSON data
  $professorResults = array();

  // now run the famous wordpress loop
  while($professors->have_posts()) {
    $professors->the_post();

    // first arg is the array you want to add on too
    // second arg is what you want to add onto the array
    array_push($professorResults, array(
      'title' => get_the_title(),
      'permalink' => get_the_permalink()
    ));
  }

  return $professorResults;
}

// Now save the file and test out your new url
// http://localhost:3000/wp-json/university/v1/search
```

## How to Setup Keyword Searches (WP_Query)

1. To add keyword searches navigate to your `incudes` folder and open `search-route.php`
2. Where going to add a `data` parameter to `universitySearchResults()` along with another array property `'s' => sanitize_text_field($data['term'])` to the `$professors` query
3. Refer to the code and comments below 

```php
<?php

add_action('rest_api_init', 'universityRegisterSearch');

function universityRegisterSearch() {
  register_rest_route('university/v1', 'search', array(
    'methods' => WP_REST_SERVER::READABLE, // use this instead if you want to go the extra mile. Will make sure that it works on any web host out there
    'callback' => 'universitySearchResults' // Make up a usefull function name then create a function with that name below
  ));
}

function universitySearchResults($data) {
  // custome queries are useful for getting data
  $professors = new WP_Query(array(
    'post_type' => 'professor',
    // sanatize-text_field prevents hacking through user inputs, that is why we wrap the $data parameter with it
    's' => sanitize_text_field($data['term']) // we can access any parameter added to the url with this, s stand for search
  ));

  // return $professors->posts; // will return the 10 most recent posts for professors. You can stop right here or use the code below instead to display the exact data that you want

  // put the data you want into this array for access later as a custom JSON data
  $professorResults = array();

  // now run the famous wordpress loop
  while($professors->have_posts()) {
    $professors->the_post();

    // first arg is the array you want to add on too
    // second arg is what you want to add onto the array
    array_push($professorResults, array(
      'title' => get_the_title(),
      'permalink' => get_the_permalink()
    ));
  }

  return $professorResults;
}

// Now save the file and test out your new url
// http://localhost:3000/wp-json/university/v1/search
```

## How to Setup Keyword Searches for Multiple Post-Types

To query for multiple post-types, edit the `$professors` query inside the `universitySearchResults($data)` function to use an array and list the post-types that you want to include. Refer to the code and comments below.

```php
$professors = new WP_Query(array(
  'post_type' => array('post', 'page', 'professor'), // this will setup keyword searches for multiple posts
  // sanatize_text_field prevents hacking through user inputs, that is why we wrap the $data parameter with it
  's' => sanitize_text_field($data['term']) // we can access any parameter added to the url with this, s stands for search
));
```

You can check your JSON DATA with this type of url `http://localhost:3000/wp-json/university/v1/search?term=about` the term is equal to whatever is in the search field when executed. You should be able to search all your data in WP with this.

[Section 15, Lecture 64](https://www.udemy.com/become-a-wordpress-developer-php-javascript/learn/v4/t/lecture/7909630?start=0)

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
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'professor') {
      array_push($results['professors'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'program') {
      array_push($results['programs'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'campus') {
      array_push($results['campuses'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

    if (get_post_type() == 'event') {
      array_push($results['events'], array(
        'title' => get_the_title(),
        'permalink' => get_the_permalink()
      ));
    }

  }

  return $results;
}

```

## How to Display Keyword Searches for Multiple Post-Types

In your `search.js` file in your JS modules folder, you can use custom html markup in the `results` function using template strings.

```javascript
// search.js
import $ from 'jquery';

class Search {
  // 1. describe and create/initiate our object
  constructor() {
    this.addSearchHTML();
    this.resultsDiv = $('#search-overlay__results');
    this.openButton = $('.js-search-trigger');
    this.closeButton = $('.search-overlay__close');
    this.searchOverlay = $('.search-overlay');
    this.searchField = $('#search-term');
    this.events();
    this.isOverlayOpen = false;
    this.isSpinnerVisible = false;
    this.previousValue;
    this.typingTimer;
  }

  // 2. events
  events() {
    this.openButton.on('click', this.openOverlay.bind(this));
    this.closeButton.on('click', this.closeOverlay.bind(this));
    $(document).on('keydown', this.keyPressDispatcher.bind(this));
    this.searchField.on('keyup', this.typingLogic.bind(this));
  }

  // 3. methods (funtion, action...)
  typingLogic() {
    if (this.searchField.val() != this.previousValue) {
      clearTimeout(this.typingTimer);

      // if search feild is emty
      if (this.searchField.val()) {
        if (!this.isSpinnerVisible) {
          this.resultsDiv.html('<div class="spinner-loader"></div>');
          this.isSpinnerVisible = true;
        }
        this.typingTimer = setTimeout(this.getResults.bind(this), 400);
      } else {
        this.resultsDiv.html('');
        this.isSpinnerVisible = false;
      }
    }

    this.previousValue = this.searchField.val();
  }

  // Display results using template strings and html here
  getResults() {
    // Uses our new custom api url to return data for all custom post-types
    // No need to worry about asynchronous vs synchronous because we no longer need to make multiple json request
    $.getJSON(universityData.root_url + '/wp-json/university/v1/search?term=' + this.searchField.val(), (results) => {
      this.resultsDiv.html(`
        <div class="row">
          <div class="one-third">
            <h2 class="search-overlay__section-title">General Information</h2>
            ${results.generalInfo.length ? '<ul class="link-list min-list">' : '<p>No general information matches that search.</p>'}
              ${results.generalInfo.map(item => `<li><a href="${item.permalink}">${item.title}</a> ${item.postType == 'post' ? `by ${item.authorName}` : ''}</li>`).join('')}
            ${results.generalInfo.length ? '</ul>' : ''}
          </div>
          <div class="one-third">
            <h2 class="search-overlay__section-title">Programs</h2>
            ${results.programs.length ? '<ul class="link-list min-list">' : `<p>No programs information matches that search. <a href="${universityData.root_url}/programs">View all programs</a></p>`}
              ${results.programs.map(item => `<li><a href="${item.permalink}">${item.title}</a></li>`).join('')}
            ${results.programs.length ? '</ul>' : ''}
            <h2 class="search-overlay__section-title">Professors</h2>

          </div>
          <div class="one-third">
            <h2 class="search-overlay__section-title">Campuses</h2>
            ${results.campuses.length ? '<ul class="link-list min-list">' : `<p>No campuses match that search. <a href="${universityData.root_url}/campuses">View all programs</a></p>`}
              ${results.campuses.map(item => `<li><a href="${item.permalink}">${item.title}</a></li>`).join('')}
            ${results.campuses.length ? '</ul>' : ''}
            <h2 class="search-overlay__section-title">Events</h2>

          </div>
        </div>
      `);
      this.isSpinnerVisible = false;
    });

    /* Asynchronous
    $.when(
      $.getJSON(universityData.root_url + '/wp-json/wp/v2/posts?search=' + this.searchField.val()),
      $.getJSON(universityData.root_url + '/wp-json/wp/v2/pages?search=' + this.searchField.val())
    ).then((posts, pages) => {
      var combinedResults = posts[0].concat(pages[0]);
      this.resultsDiv.html(`
        <h2 class="search-overlay__section-title">General Information</h2>
        ${combinedResults.length ? '<ul class="link-list min-list">' : '<p>No general information matches that search.</p>'}
          ${combinedResults.map(item => `<li><a href="${item.link}">${item.title.rendered}</a> ${item.type == 'post' ? `by ${item.authorName}` : ''}</li>`).join('')}
        ${combinedResults.length ? '</ul>' : ''}
        </ul>
      `);
      this.isSpinnerVisible = false;
    }, () => {
      this.resultsDiv.html('<p>Unexpected error; please try again.</p>');
    }); */

    /* Synchronous
    $.getJSON(universityData.root_url + '/wp-json/wp/v2/posts?search=' + this.searchField.val(), posts => {
      $.getJSON(universityData.root_url + '/wp-json/wp/v2/pages?search=' + this.searchField.val(), pages => {
        var combinedResults = posts.concat(pages);
        this.resultsDiv.html(`
          <h2 class="search-overlay__section-title">General Information</h2>
          ${combinedResults.length ? '<ul class="link-list min-list">' : '<p>No general information matches that search.</p>'}
            ${combinedResults.map(item => `<li><a href="${item.link}">${item.title.rendered}</a></li>`).join('')}
          ${combinedResults.length ? '</ul>' : ''}
          </ul>
        `);
        this.isSpinnerVisible = false;
      });
    }); */
  }

  keyPressDispatcher(e) {
    if (e.keyCode === 83 && !this.isOverlayOpen && !$('input, textarea').is(':focus')) {
      this.openOverlay();
    }

    if (e.keyCode === 27 && this.isOverlayOpen) {
      this.closeOverlay();
    }
  }

  openOverlay() {
    this.searchOverlay.addClass('search-overlay--active');
    $("body").addClass("body-no-scroll");
    this.searchField.val('');
    setTimeout(() => this.searchField.focus(), 301);
    this.isOverlayOpen = true;
  }

  closeOverlay() {
    this.searchOverlay.removeClass('search-overlay--active');
    $("body").removeClass("body-no-scroll");
    this.isOverlayOpen = false;
  }

  // add html to bottom of page w/ javascript, call in the constructor
  addSearchHTML() {
    $('body').append(`
      <div class="search-overlay">
        <div class="search-overlay__top">
          <div class="container">
            <i class="fa fa-search search-overlay__icon" aria-hidden="true"></i>
            <input type="text" class="search-term" placeholder="What are you looking for?" id="search-term" />
            <i class="fa fa-window-close search-overlay__close" aria-hidden="true"></i>
          </div>
        </div>

        <div class="container">
          <div id="search-overlay__results"></div>
        </div>
      </div>
    `);
  }
}

export default Search;

```

## 
