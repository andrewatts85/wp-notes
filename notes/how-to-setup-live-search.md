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

## OOP with JavaScript Classes

```javascript
import $ from 'jquery';

class Search {
  // 1. describe and create/initiate our object
  constructor() {
    this.openButton = $('.js-search-trigger');
    this.closeButton = $('.search-overlay__close');
    this.searchOverlay = $('.search-overlay');
    this.events();
  }

  // 2. events
  events() {
    this.openButton.on('click', this.openOverlay.bind(this));
    this.closeButton.on('click', this.closeOverlay.bind(this));
  }

  // 3. methods (funtion, action...)
  openOverlay() {
    this.searchOverlay.addClass('search-overlay--active');
  }

  closeOverlay() {
    this.searchOverlay.removeClass('search-overlay--active');
  }
}

export default Search;
```

