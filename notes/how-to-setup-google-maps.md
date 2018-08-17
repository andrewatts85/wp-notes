# How to Setup Google Maps

1. Navigate to `Custom Fields` in your WordPress admin. You must have the `Advanced Custom Fields` plugin installed to do so.

2. Set up the name, set the field type to google maps, and set up the location.

3. Get an API_KEY with this link [https://developers.google.com/maps/documentation/javascript/get-api-key](https://developers.google.com/maps/documentation/javascript/get-api-key).  

4. Manage your API_KEY with this linke [https://console.cloud.google.com/](https://console.cloud.google.com/). Be sure to enable all the maps, all the places, geolaction, and geocoding api's or you may have problems with your Map widget.  

5. Navigate to `functions.php` in your text editor and refer to the code below to provide WordPress your API_KEY. Make note of the   `acf/fields/google_map/api` hook to utilize the KEY since this is being set up with custom fields using the `Advanced Custom Fields` plugin.

```php
// This function provides the api key for google maps through your custom field
function universityMapKey($api) {
  $api['key'] = 'AIzaSyAi4HJI-1B_oIRoVWeqdQS4nZYrrXchCrw';
  return $api;
}

add_filter('acf/fields/google_map/api', 'universityMapKey');
```

## How to Display Google Maps on the Frontend

6. Remeber, you can access custom fields with the `get_field()` function. `print_r()` in PHP is similar to `console.log` in JavaScript. `print_r($mapLocation)` will display array values associate with it. You will find latitude and longitude info we can use to display the map.

```php
<?php 
  $mapLocation = get_field('map_location');
  print_r($mapLocation); ?> // print_r() in PHP is similar to console.log in JavaScript
```

7. You can use the HTML `data` attribute to set up the latitude and longitude so that JavaScript can be allowed to utilize the data values.

```php
<div class="acf-map">
  <?php
    while(have_posts()) {
      the_post();
        $mapLocation = get_field('map_location');
      ?>
      <div class="marker" data-lat="<?php echo $mapLocation['lat']; ?>" data-lng="<?php echo $mapLocation['lat']; ?>"></div>
    <?php }
    echo paginate_links();
  ?>
</div>
```

## How to Load a JavaScript File on the Frontend in WordPress

8. Navigate to `functions.php` and find the functions that controls `wp_head()` in `header.php`. The action hook will be `wp_enqueue_scripts' which is used to load JavaScript files.  

9. Refer to the code below to add a JavaScript file. You can add as many JavaScript files you want. 

10. The second argument for `wp_enqueue_script()` will be the google url for the api call which is  `//maps.googleapis.com/maps/api/js?key=` + `INSERT YOUR API_KEY HERE`. This is where you will also enter your API_KEY.

```php
// This function controls `wp_head()` in `header.php`
function university_files() {
  // JavaScript files
  wp_enqueue_script('googleMap', '//maps.googleapis.com/maps/api/js?key=AIzaSyAi4HJI-1B_oIRoVWeqdQS4nZYrrXchCrw', NULL, microtime(), true);
  wp_enqueue_script('main_university_js', get_theme_file_uri('/js/scripts-bundled.js'), NULL, microtime(), true);
  
  // CSS styles
  wp_enqueue_style('google_fonts', '//fonts.googleapis.com/css?family=Roboto+Condensed:300,300i,400,400i,700,700i|Roboto:100,300,400,400i,700,700i');
  wp_enqueue_style('font_awesome', '//maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css');
  wp_enqueue_style('university_main_styles', get_stylesheet_uri(), NULL, microtime());
}

add_action('wp_enqueue_scripts', 'university_files');
```

## How to Feed Our Lat and Lng Values Into the Google Maps Script

11. Since we are using the `Advanced Custom Fields` plugin, we need to refer to their [documentation](https://www.advancedcustomfields.com/resources/google-map/) so we can create the JavaScript file from their JS template and put it in our theme. Keep in mind some minor adjustments will need to be made to the code to get it to work but nothing major.

12. Once the JavaScript file is created, import it to your `scripts.js` file so WordPress can access it.

```javascript
// 3rd party packages from NPM
import $ from 'jquery';
import slick from 'slick-carousel';

// Our modules / classes
import MobileMenu from './modules/MobileMenu';
import HeroSlider from './modules/HeroSlider';
import GoogleMap from './modules/GoogleMap'; // import GoogleMap.js here

// Instantiate a new object using our modules/classes
var mobileMenu = new MobileMenu();
var heroSlider = new HeroSlider();
var googleMap = new GoogleMap(); // initiate a new class of GoogleMap
```

13. Now we need to use the command line to rebundle all the JavaScript files.

14. `cd` to the correct path that holds `gulpfile.js`, `package.json`, `settings.js`, `webpack.config.js`. If using local-by-flywheel the path will probably look like this `local sites => custom-theme-name => app => public`

15. Then just run `gulp watch` again and save your changes or run `gulp scripts` to rebundle your JS files.
