# Wordpress Tips, tricks and Code Snippets


## Table of Contents

- [Disable wordpress comments](#disable-wordpress-comments)
- [Yoast SEO - Allow empty categories in Sitemap](#yoast-seo-allow-empty-categories-in-sitemap)

## Disable wordpress comments

Copy the code from function.php and place it in the end of your `function.php`

```
add_action('admin_init', function () {
    // Redirect any user trying to access comments page
    global $pagenow;
    
    if ($pagenow === 'edit-comments.php') {
        wp_redirect(admin_url());
        exit;
    }

    // Remove comments metabox from dashboard
    remove_meta_box('dashboard_recent_comments', 'dashboard', 'normal');

    // Disable support for comments and trackbacks in post types
    foreach (get_post_types() as $post_type) {
        if (post_type_supports($post_type, 'comments')) {
            remove_post_type_support($post_type, 'comments');
            remove_post_type_support($post_type, 'trackbacks');
        }
    }
});

// Close comments on the front-end
add_filter('comments_open', '__return_false', 20, 2);
add_filter('pings_open', '__return_false', 20, 2);

// Hide existing comments
add_filter('comments_array', '__return_empty_array', 10, 2);

// Remove comments page in menu
add_action('admin_menu', function () {
    remove_menu_page('edit-comments.php');
});

// Remove comments links from admin bar
add_action('init', function () {
    if (is_admin_bar_showing()) {
        remove_action('admin_bar_menu', 'wp_admin_bar_comments_menu', 60);
    }
});
```

To remove all existing comments use following sql queries:

```
TRUNCATE wp_commentmeta;

TRUNCATE wp_comments;
``` 

>NOTE: Use these queries carefully with woocommerce sites. You will loose order related comments as well. (Receipt Number, Payment Status etc)


## Yoast SEO - Allow empty categories in Sitemap

By default, [Yoast SEO plugin](https://en-au.wordpress.org/plugins/wordpress-seo/) exclude empty taxonomy from sitemap. If you want to include them in sitemap put following code in your `function.php`


```
add_filter( 'wpseo_sitemap_exclude_empty_terms', function() { return false; } ); 
```

