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

