Copy the code from function.php and place it in the end of your function.php

To remove all existing comments use following sql queries:

TRUNCATE wp_commentmeta; 

TRUNCATE wp_comments;

NOTE: Use these queries carefully with woocommerce sites. You will loose order related comments as well. (Receipt Number, Payment Status etc)