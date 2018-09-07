# How to Setup Live Search

Live search means to have the results pop up instantly instead of a traditional search that takes the user to a whole different web page with the search result displayed. 

## Test to Make Sure JavaScript is Working

Just execute a simple `alert` function and see if it pops up on your website. If it does you can confirm that JavaScript it working.

## JavaScript's `class`, `import`, and `export` Keywords

If you know ES6 syntax you will be fine with these keywords.

## Search Overlay

A good place to put the html for the overlay would be at he bottom of the `footer.php` file before the closing body and php tags.

```php
    <footer class="site-footer"></footer>

    <!-- html markup for search -->
    <div class="search-overlay search-overlay--active">
      hello 123
    </div>

    <?php wp_footer(); ?>
  </body>
</html>
```
## Search Overlay Logic
#### OOP with JavaScript Classes

```javascript
import $ from 'jquery';

class Search {
  // 1. describe and create/initiate our object
  // set up your variables and assign the elements you need to add events to here *****
  constructor() {
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
  // declare your events here *****
  events() {
    this.openButton.on('click', this.openOverlay.bind(this));
    this.closeButton.on('click', this.closeOverlay.bind(this));
    $(document).on('keydown', this.keyPressDispatcher.bind(this));
    this.searchField.on('keyup', this.typingLogic.bind(this));
  }

  // 3. methods (funtion, action...)
  // write out your methods here, add and remove classes from the desired elements when an event occurs *****
  typingLogic() {
    if (this.searchField.val() != this.previousValue) {
      clearTimeout(this.typingTimer);

      // if search feild is emty
      if (this.searchField.val()) {
        if (!this.isSpinnerVisible) {
          this.resultsDiv.html('<div class="spinner-loader"></div>');
          this.isSpinnerVisible = true;
        }
        this.typingTimer = setTimeout(this.getResults.bind(this), 800);
      } else {
        this.resultsDiv.html('');
        this.isSpinnerVisible = false;
      }
    }

    this.previousValue = this.searchField.val();
  }
  
  // jQuery is used to work with WP REST API here
  getResults() {
    $.getJSON('http://localhost:3000/wp-json/wp/v2/posts?search=' + this.searchField.val(), posts => {
      this.resultsDiv.html(`
        <h2 class="search-overlay__section-title">General Information</h2>
        <ul class="link-list min-list">
          ${posts.map(item => `<li><a href="${item.link}">${item.title.rendered}</a></li>`).join('')}
        </ul>
      `);
    });
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
    this.isOverlayOpen = true;
  }

  closeOverlay() {
    this.searchOverlay.removeClass('search-overlay--active');
    $("body").removeClass("body-no-scroll");
    this.isOverlayOpen = false;
  }
}

export default Search;
```

## Get JSON Data with `/wp-json`

By using ajax or javascript's fetch api you can pull in json data with http request.

* `root url` + `/wp-json/wp/v2/posts` will pull in json data for the 10 most recent posts
* `root url` + `/wp-json/wp/v2/pages` will pull in json data for the 10 most recent pages

```javascript
// jQuery is used to work with WP REST API here
getResults() {
  $.getJSON('http://localhost:3000/wp-json/wp/v2/posts?search=' + this.searchField.val(), posts => {
    this.resultsDiv.html(`
      <h2 class="search-overlay__section-title">General Information</h2>
      <ul class="link-list min-list">
        ${posts.map(item => `<li><a href="${item.link}">${item.title.rendered}</a></li>`).join('')}
      </ul>
    `);
  });
}
```

## How to Make the HTTP Request URL Dynamic
When making an http request we need to make sure that the root url is dynamic. `http://localhost:3000/` will not work in a production environment. To fix this problem we need to edit our `functions.php` file and use the `wp_localize_script()` method. This method accepts 3 arguments and attaches to the WordPress `wp_engueue_scripts` hook.

`wp_localize_script(1,2,3)`
1. The first parameter needs the name of the script that it will be outputted to
2. The second argument just needs a variable name that you can make up
3. The third argument is for an associative array that you want to make available

Next add the property `'root_url' => get_site_url()` to the associative array.

```php
// This function controls `wp_head()` in `header.php`
function university_files() {
  wp_enqueue_script('main_university_js', get_theme_file_uri('/js/scripts-bundled.js'), NULL, microtime(), true);
  wp_localize_script('main_university_js', 'universityData', array(
    'root_url' => get_site_url()
  ));
}

add_action('wp_enqueue_scripts', 'university_files');
```

If you right click on the front page of your WordPress site and then click `view page source` and scroll to the bottom, you will see this bit of code generated by WordPress indicating that the root url is what we want.

```javascript
/* <![CDATA[ */
var universityData = {"root_url":"http:\/\/localhost:3000"};
/* ]]> */
```

Now we can replace `http://localhost:3000/` with `universityData.root_url` like so to make our url relative that will work on a live server. Remember that `universityData` is the variable name we made up when we initialized the `wp_localize_script()` method.

```javascript
getResults() {
$.getJSON(universityData.root_url + '/wp-json/wp/v2/posts?search=' + this.searchField.val(), posts => {
  this.resultsDiv.html(`
    <h2 class="search-overlay__section-title">General Information</h2>
    <ul class="link-list min-list">
      ${posts.map(item => `<li><a href="${item.link}">${item.title.rendered}</a></li>`).join('')}
    </ul>
  `);
})}
```

## How to Include HTML Markup in Your `Search.js` File

Just add a function to your `search.js` file like so. Don't forget to call it in your `constructor()` at the top or your other variables in the `constructor()` won't work.

```javascript
class Search {
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
```

## How to Add Search Functionality to both Pages and Posts

To include all pages as well as posts in the search results you can nest the api calls within each, although it is not ideal because it takes a lot of time and could harm the user experience.

```javascript
getResults() {
  $.getJSON(universityData.root_url + '/wp-json/wp/v2/posts?search=' + this.searchField.val(), posts => {
    $.getJSON(universityData.root_url + '/wp-json/wp/v2/pages?search=' + this.searchField.val(), pages => {
      // combine the results by concatinating both arrays together  
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
  });
}
```
