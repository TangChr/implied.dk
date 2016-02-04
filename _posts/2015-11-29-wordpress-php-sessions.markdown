---
layout: post
date:   2015-11-29 14:13:00
title:  "Enable PHP sessions in WordPress"
---
I have been using WordPress for several years now. I primarily use it as a CMS-system instead of a weblog. I have a tendancy for ending up writing plugins or just tweaking the website instad of using it as an actual blog.

<div class="seperator"></div>
I recently had to write a theme which included a lot of functionality writtin directly in the theme (language specific content and menus). This however turned out to be quite a challenge, because WordPress doesn't support "native" PHP sessions (ie. $_SESSION). I'm not sure if WordPress has some kind of alternative, but being a PHP developer, I just couldn't let this one go.

### The solution
In order to solve this issue, I thought to myself, what if I wrote a plugin?
And in case you are wondering, I did write a plugin!

#### Install the plugin
To install the plugin just create a PHP-file (e.g. *session-activator.php*) in the plugins-folder (usually /wp-content/plugins) and paste the following code into that file. Log in to the Control Panel and activate the plugin.

```php
<?php
/*
Plugin Name: Session Activator
Plugin URI: http://willitcompile.net
Description: Enables the use of PHP sessions ($_SESSION) in themes and plugins.
Version: 1.0.0
Author: Christian Tang
Author URI: http://seaweed.dk
*/
add_action('init', 'session_activator_start', 1);
add_action('wp_logout', 'session_activator_end');
add_action('wp_login', 'session_activator_end');

function session_activator_start() {
    if(!session_id()) {
        session_start();
    }
}

function session_activator_end() {
    session_destroy();
}
?>
```