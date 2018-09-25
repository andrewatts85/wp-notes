## How To Setup Non-JS Fallback Traditional Search

### search.php


### Fallback Search URL

You can search a wordpress site by adding `/?s=` plus the search term to the end of the root url.

```php
$rootURL = 'https://www.exyavier-university.com';
$searchTerm = 'biology';
'$rootURL' . '/?s=' . '$searchTerm'
// https://www.exyavier-university.com/?s=biology
```

1. Create a new page called `Search`. Leave the body blank
2. create a new theme file that is only used for the `Search` page
3. Name is `page-search.php`. WordPress will use it for just that one page
4. Copy and paste the contents from the generic `page.php` file then modify it to your liking
5. Be sure to include a form that the user can submit for a search

```php
<form method="get" action="<?php echo esc_url(site_url('/')); ?>">
  <input type="search" name="s"/>
  <input type="submit" value="search" />
</form>
```

6. Adding the attribute type `search` and the attribute name `s` to the input tag will set the `/?s=$searchTerm` portion in the url. Whatever you name the input field, it will show up in the url like so: `/?[$inputName]=[$searchTerm]`
7. The action attribute will submit it to the desired url or it will submit the user input to the url of itself by default
8. `echo site_url('/')` will generate the homepage url for our WordPress installation
9. **WordPress Security Tip:** Whenever you manually echo a url coming from your database you should wrap it in `esc_url()`
10. Using a `method="get"` instead of post will make sure the contents of the form end up in the url
11. Now wherever your search button is in your WordPress theme, you can link it to the `search` page you just created

```php
<a href="<?php echo esc_url(site_url('/search')); ?>">Search</a>
```

12. Now open your `search.js` file. We need to make sure to use this search if JavaScript is disabled.
13. Go down to your `openOverlay()` function and add `return false;` at the bottom. This will prevent the default behavior of `<a>` or link elements

```javascript
openOverlay() {
  this.searchOverlay.addClass('search-overlay--active');
  $("body").addClass("body-no-scroll");
  this.searchField.val('');
  setTimeout(() => this.searchField.focus(), 301);
  this.isOverlayOpen = true;
  return false;
}
```

## Power Your Search Results Page with `search.php`

This traditional search method is powered by `index.php`. If you have a file called `search.php` in your WordPress theme, WordPress will you that instead of `index.php` to power your search page.

14. Create a file called `search.php` in your WordPress theme
15. Copy the contents of `index.php` into `search.php` then customize the page to your liking
16. The method `get_search_query()` will return the search term that was inputed by the user

```php
<?php
  get_header();
  pageBanner(array(
    'title' => 'Search Results',
    'subtitle' => 'You searched for &ldquo;' . get_search_query() . '&rdquo;'
  ));
?>
```

17. Use `esc_html(get_search_query(false))` instead when outputting as html - [Traditional WordPress Searching (Part 2) section 17 lectur 71](https://www.udemy.com/become-a-wordpress-developer-php-javascript/learn/v4/t/lecture/8007378?start=15)

18. `searchform.php`
19. `get_search_form()`
