# Rebuild WordPress Permalink Structure

When you create a new custom post-type, WordPress will need to update it's permalink structure if you want post created from that custom post-type to work properly. WordPress doesn't update everything on every save so it would have no idea that you created a custom post-type unless you manually rebuild the WordPress permalink structure. It's pretty easy to do just follow these 2-step instructions:

1. Navigate to your `wp-admin`, go to `Settings` and click `Permalinks`
2. Scroll down and click `Save Changes`

Now that the permalinks are updated you should be able to navigate to post with no issues when using WordPress functions like `the_permalink()`.
