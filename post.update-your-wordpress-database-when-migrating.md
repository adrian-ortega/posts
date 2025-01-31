---
title: Update your WordPress database when migrating for local development with SQL.
description: Whenever I have to develop a WordPress site, plugin or theme I end up migrating the existing website (be it a staging or development site) to my machine and working on it locally. This is usually a tedious process (if you've ever done it before you know what I mean).
tags: [mysql, code, php, wordpress]
created_at: '2018-02-21T02:00:00.000Z'
---

Whenever I have to develop a WordPress site, plugin or theme I end up migrating the existing website (be it a staging or development site) to my machine and working on it locally. This is usually a tedious process (if you've ever done it before you know what I mean).

Now, before I continue, I want to say that there are great plugins out there that will do this for you. An honorable mention is [WP Migrate DB](https://wordpress.org/plugins/wp-migrate-db/). I've used this countless times and it's definitely helped me a ton. I recommend it to anyone that wants to the following automatically. But for those of you that want to know the manual way of updating your tables to work with your local environment, continue on.

So I'm going to skip a ton of steps here because I assume you're here for a the following script:

```sql
SET @oldurl = '//wp.aortegadesign.com';
SET @newurl = '//wp.aortegadesign.dev';

UPDATE wp_options SET option_value = replace(option_value, @oldurl, @newurl) WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, @oldurl, @newurl);
UPDATE wp_posts SET post_content = replace(post_content,@oldurl, @newurl);
UPDATE wp_postmeta SET meta_value = replace(meta_value, @oldurl,@newurl); 
```

## Let's break it down a bit

The real only issue when migrating a WordPress database from one server to another (or in this case, locally) is updating all of the urls within each table to point to the new server's location. So right away you see that we set a couple of variables that will help us find and replace them throughout the database.

### Variables to the rescue

```sql
SET @oldurl = '//wp.aortegadesign.com';
SET @newurl = '//wp.aortegadesign.dev';
```

Notice how in the above the protocol is omitted. I do this so I don't have to worry about http vs https within the database. This is usually a concern for the server setup. However, if you're working on a SSL website but are developing on a non-SSL server, do yourself a favor and update the above to replace https to http.

### The only one you really need

```sql
UPDATE wp_options SET option_value = replace(option_value, @oldurl, @newurl) WHERE option_name = 'home' OR option_name = 'siteurl'; 
```

Really, this is the only update you need to do to get your website running. WordPress stores both main URL and admin panel URL within the database.

### Content and Meta Data

```sql
UPDATE wp_posts SET guid = replace(guid, @oldurl, @newurl); UPDATE wp_posts SET post_content = replace(post_content,@oldurl, @newurl); 
```

When posts and pages are edited using the [WYSIWYG](http://www.mediawiki.org/wiki/WYSIWYG_editor), images and references to links within the site have the site's URL pre-pended to them. This will replace all of them with the new url.

```sql
UPDATE wp_postmeta SET meta_value = replace(meta_value, @oldurl,@newurl);
```

WordPress and Plugins that store data within the `wp_postmeta` also need to be updated just in case they store the database.