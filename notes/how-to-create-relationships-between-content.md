# How to Create Relationships Between Content

1. Navigate to `mu-plugins` and create a new post-type. You will use this post-type as a makeshift (category)
2. Add post (categories) to the new post type
3. Update the permalinks
4. Create a dedicated `single-nameOfCustomPostType.php` page. Just copy what you have from `single.php` and customize it
5. Create a dedicated `archive-nameOfCustomPostType.php` page. Just copy what you have from `archive.php` and customize it
6. Now, what you want to do here is provide a way to include or link related post from other post-types to the custom post-type you just created. To do that you will have to create a custom field where you can select which custom post-type you want to relate it to
7. Click on `Custom Fields` in your WordPress admin and create a new field group
8. Edit `Field Label`, `Field Name`, `Field Type`, `Post Type`, and the `Location` rules
9. Make sure you choose `Relationship` when editing your `Field Type`
10. When editing the `Post Type` field select your custom post-type that was just created. This will create the relationship between your content
11. Now edit your `Location` rules to only show this new custom field on the specific post of your choice
