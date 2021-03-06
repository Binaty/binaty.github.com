---
layout: products
thumbnail-path: "/img/get-facebook-likes.jpg"
title: "Get Facebook Likes"
link: "http://binaty.org/plugins/get-facebook-likes"
description: "Get Facebook Likes interacts with Facebook Graph API and saves likes, shares, and comments count to post meta right after users hit Like/Share buttons or leaves a Facebook comment. Therefore you can do some awesome stuff that you think it's impossible before like sort posts, order search results, vote, deeper analytics... by Facebook likes (and shares, comments too)."
---

As we're both developers and end users, we're obviously love fast & ease of use plugins. **Get Facebook Likes** uses native Facebook JS SDK and works right after activated, no cronjob setup is required. Just a few options to tweak your script to the highest speed.

## Features
* Basic Mode which compatibility with all WP Sites 
* Advanced Mode which have better performance and more accurate
* Use native Facebook API to listening user event and get facebook likes/shares/comments
* Save likes/shares/comments to post meta for further use
* Built-in shortcodes `[likes]`, `[shares]`, and `[comments]`
* Likes custom column and sorting
* Most favourite content on the Dashboard area which display top 10 posts which have most likes+shares+comments
* Display likes/shares/comments in Post Editing screen if [Meta Box](https://wordpress.org/plugins/meta-box) plugin is installed

### Use Cases
* Display most liked post from your website
* Create a contest on your website which use Facebook Like (or Share) to vote
* Restrict content, require at least X likes to view.
* Discount your product if it has at least X likes
* Track Facebook likes, shares, comments per day
* ... 

## Installation

1. Upload the plugin files to the `/wp-content/plugins/get-facebook-likes` directory, or install the plugin through the WordPress plugins screen directly.
1. Activate the plugin through the 'Plugins' screen in WordPress
1. Use the `Settings\Get Facebook Likes` screen to configure the plugin

## Advanced Usage
Likes, Shares and Comments are saved in `wp_postmeta` table as `fb_like_count`, `fb_share_count`, `fb_comment_count` and a special field called `fb_total_count` so basically, you can use `get_post_meta()` function to retrieve these values.

We're created some helper functions and examples to make this process easier. Let's try it:

**Get num of likes, shares, comments, or total likes+shares+comments**

Likes, Shares, Comments and Total Likes+Shares+Comments can be retrived by using `gfl_count()` function

```
gfl_count( $custom_field_name, $post_id );
```

Where

`$custom_field_name` (Required) name of custom field. Default: `fb_like_count`

`$post_id` (Optional) Post ID. Default: Current Post ID

**Get Num of Likes**

```
gfl_likes( $post_id );
```

When

`$post_id`: *Optional* Post ID to get likes, default: current post id

*This function is a shortcut of `gfl_count();`*


**Shortcodes to display likes, shares, or comments**

`[likes]`, `[shares]`, and `[comments]` are shortcodes to display related action. You can also pass post `id` in case you want to display value from another post or outside the post. Like so:

`[likes id="35"]`

**Order Posts By Likes**

The most favourited part is here. You can order post by Facebook Likes/Shares/Comments or total of them. This is the example:

```
$args = [
	'meta_key' 	=> 'fb_like_count', 
	'orderby' 	=> 'meta_value_num', 
	'order' 	=> 'DESC'
];

$query = new WP_Query( $args );
```

Or simply pass variables to URL like so:
```
http://example.com/?fsortby=likes&forderby=desc
```

Please note that
<strong>fsortby</strong> supports `likes`, `shares`, `comments`, and `all`
<strong>forderby</strong> supports `asc`, and `desc`

To learn more about WP_Query. See [WP Codex](https://codex.wordpress.org/Class_Reference/WP_Query).


### Javascript Events
Version `1.1` comes with Javascript Events, this allows you listening Facebook Events, Updating Events so you can extend it to create your own addons. Awesome right?

To subscribe an event, you must run `GFL.subscribe()` method after page load. The syntax like so:

```js
GFL.subscribe(event, callback);
```

To fire an event, you can run `GFL.fire()` method after pageload, like so:

```js
GFL.fire(event [,arguments]);
```

#### The Events List
Name | Arguments | Description
---|---|---
`edge.create` | url | Run when users hit like button
`edge.remove` | url | Run when users hit unlike button
`comment.create` | url | Run when users post a comment
`comment.remove` | url | Run when users remove a comment
`update.before` | null | Run when system try to update likes/shares/comments count
`update.response` | response | Run when system return a response after update likes/shares/comments count
`update.after` | null | Run when system updated likes/shares/comments count

Example:

Run an anonymous function to log url when user hit like button

```js
jQuery(document).ready(function ($) {
	GFL.subscribe('edge.create', function(url) {
		console.log(url);
	});
});

```

Run  `my_custom_function()` after update likes/shares/comments count

```js

jQuery(function($){

	function my_custom_function() {
		alert('Update Successful');
	}

	GFL.subscribe('update.after', my_custom_function)
});

```


## Frequently Asked Questions

#### Does it requires Facebook App ID?
No, **Get Facebook Likes** works without App ID. But it's recommended to use more advanced feature of Facebook like App Tracking, Moderate Comments, etc...

#### I have already installed Facebook JS SDK, what option I should use?
If you've already installed Facebook JS SDK by placing their script right after open `body` tag or by another plugin. Just uncheck `Auto add Facebook JS SDK to wp_head` option and place `GFL_Main.init();` after `FB.init();` in `window.fbAsyncInit` method like **Setup Guide** in `Settings\Get Facebook Likes`

#### Does the plugin count the likes from the beginning of time?
Yes, each time the update like event was fired (also applied to share, comment), it makes a `GET` request to Facebook to retrieve total count of them. This because some users may like or share your post outside of your website.

## Roadmap

#### 1.2
* **New** Most Liked Widget
* **New** Auto add `like` and `share` buttons so users don't have to edit theme file or use other plugins

## Changelog

#### 1.1.2 (March 08, 2016)
* **New** Add sorting query args so user can sort posts via url
* **New** Convert to K, M, B if total likes, shares, comments is more than 10k
* **Fix** [comments] shortcode doesn't works

#### 1.1.1 (Feb 25, 2016)
* **Improvement** Add Likes, Shares, Comments columns in Most Favourite Content meta box
* **Improvement** Add settings page hooks and default settings filter
* **Improvement** Most Favourite Content widget now showing Likes and Shares
* **Improvement** Add minified script <code>gfl-main.min.js</code>
* **Fix** Javascript Event doesn't works properly
* **Fix** Use WordPress recommended text domain

#### 1.1 (Feb 01, 2016)
* **New** Introduction to JS Events
* **New** Add I18N support
* **New** Allows users set JS SDK Locale
* **Improvement** Use `gfl_count()` instead of `gfl_facebook_count()`;

#### 1.0 (Jan 24, 2016)
* Initial Release