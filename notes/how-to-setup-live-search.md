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
    this.searchField = $('#search-term');
    this.events();
    this.isOverlayOpen = false;
    this.typingTimer;
  }

  // 2. events
  events() {
    this.openButton.on('click', this.openOverlay.bind(this));
    this.closeButton.on('click', this.closeOverlay.bind(this));
    $(document).on('keydown', this.keyPressDispatcher.bind(this));
    this.searchField.on('keydown', this.typingLogic.bind(this));
  }

  // 3. methods (funtion, action...)
  typingLogic() {
    clearTimeout(this.typingTimer);
    this.typingTimer = setTimeout(function() {
      console.log('it works');
    }, 800);
  }

  keyPressDispatcher(e) {
    if (e.keyCode === 83 && !this.isOverlayOpen) {
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

