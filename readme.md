WP cheats
=========

## Invoke `short codes` from template
`<?php echo do_shortcode("[shortcode]"); ?>`

## get the `URL` of...

| function        | usage                                   | output                                                     |
| --------------- | --------------------------------------- | -----------------------------------------------------------|
| home_url()      | Home URL                                | http://www.example.com                                     |
| site_url()      | Site directory URL                      | http://www.example.com or http://www.example.com/wordpress |
| admin_url()     | Admin directory URL                     | http://www.example.com/wp-admin                            |
| includes_url()  | Includes directory URL                  | http://www.example.com/wp-includes                         |
| content_url()   | Content directory URL                   | http://www.example.com/wp-content                          |
| plugins_url()   | Plugins directory URL                   | http://www.example.com/wp-content/plugins                  |
| theme_url()     | Themes directory URL (#18302)           | http://www.example.com/wp-content/themes                   |
| wp_upload_dir() | Upload directory URL (returns an array) | http://www.example.com/wp-content/uploads                  |



### the *content* dir
`<?php content_url() ?>`

### uploades directory
```
<?php
$content_url = wp_upload_dir();
$prod_url = $content_url['baseurl']."/products/";
?>
```
### base `URL` of site

`<?php get_home_url()`

## Shortcodes
```
//============[helloworld]==============================
function helloworld_shortcode($atts, $content, $tag){
    
    //use this to for defaults in $atts
    $a = shortcode_atts( array(
            'att_1' => "value_1",
            'att_2' => 420
        ), $atts);
        
    //return NOT echo
    return "<div class='column small-12 panel'><p class='label alert'>Hello World!</p></div>";
    
}
//add shortcode
add_shortcode('helloworld', 'helloworld_shortcode');

//===========[hello]content[/hello]==========================
//$content = null so content is encapsulated with closing tag
function hello_shortcode($atts, $content = null, $tag){
    //put $atts in $a..
    $a = shortcode_atts(array(
            'class' => 'no_class'
        ), $atts);
        
    return "<div class='small=12 panel rounded " . $a['class'] . "'>" . $content . "</div>";
}
//add shortcode
add_shortcode('hello', 'hello_shortcode');
```
## Remove the base Open sans from wordpress
*(to many fonts loaded!)* goes into a `functions.php` file in root of theme
```
<?php
if(!function_exists('remove_wp_open_sans')) :
    function remove_wp_open_sans(){
        wp_deregister_style('open-sans');
        wp_register_style('open-sans', false);
    }
    add_action('wp_enqueue_scripts', 'remove_wp_open_sans');
    add_action('admin_enqueue_scripts', 'remove_wp_open_sans');
    endif;
?>
```

## stop wp from messing with content
WP likes to mess with your `HTML` and add `<p>` tags and other crap. This takes care of that and goes into a `functions.php` file in root of theme.
```
<?php
remove_filter('the_content', 'wpautop');
remove_filter('the_excerpt', 'wpautop');
?>
```
