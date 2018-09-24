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
11. 
