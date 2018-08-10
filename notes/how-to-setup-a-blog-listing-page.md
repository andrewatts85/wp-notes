# How to Setup a Blog Listing Page 
(index.php vs front-page.php)

By default WordPress makes your homepage a generic blog listing but we don't want our root domain to be a blog listing. What we want instead is something like this `mywebsite.com/blog` to be our blog. So let's set this up.

## Configure Homepage and Post page
1. Start by accessing your wp-admin dashboard. Create two new pages called `Home` and `Blog`.
2. Now navigate to `Settings` and click the `Reading` option. This is where you tell WordPress where your Homepage or front-page should display. By default it is set to `Your latest posts`. Instead click the `static page` option so that we can manually set the front-page and post-page.
3. Set the `Homepage` to the `Home` page you just created. Then set the `Posts page` to the `Blog` page you also just created and save your changes.  

[![Screen_Shot_2018-06-22_at_5.09.12_PM.png](https://s15.postimg.cc/8x8eeeozf/Screen_Shot_2018-06-22_at_5.09.12_PM.png)](https://postimg.cc/image/m1dyr3h13/) . 

##  index.php === Post page: [ Blog ]
Now the code from `index.php` in your theme files is responsible for the blog `Post page`.

## front-page.php === Homepage: [ Home ]
Now the code from `front-page.php` in your theme files is responsible for being the `Homepage` now.

## Tip

### the_excerpt()

When setting up blog links you'll probably be using `the_content()` to display the whole body text. If you want to display only a portion of the text use `the_excerpt()` instead.  

### the_author_posts_link()

Use this function to return the author resposible for a post. You can update the way the user name is displayed by navigating to `Users` in `wp-admin`, then update the `Nickname` and `Display name publicly as` fields.  

### the_time()

Use the `the_time()` function to dynamically output the date or time of each post.

### get_the_category_list(', ')

This function will dynamically (`echo`) output the post category.

### WordPress Functions that Start with `get`

Wordpress function names that start with `get` will return the result instead of `echo`ing it out to the page. Make sure you use `echo` when dropping into `php` mode to output the function result.
