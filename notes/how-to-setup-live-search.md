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

  getResults() {
    $.getJSON('http://localhost:3000/wp-json/wp/v2/posts?search=' + this.searchField.val(), function(posts) {
      alert(posts[0].title.rendered);
    });

    // this.resultsDiv.html('Nothing found...');
    // this.isSpinnerVisible = false;
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

* `site url` + `/wp-json/wp/v2/posts` will pull in json data for the 10 most recent posts
* `site url` + `/wp-json/wp/v2/pages` will pull in json data for the 10 most recent pages
