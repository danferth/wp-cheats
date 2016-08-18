WP cheats
=========

## Invoke `short codes` from template
`<?php echo do_shortcode("[shortcode]"); ?>`

## get the `URL` of...

| function              | usage                                     | output                                    |
| ------------------    | ----------------------------------------- | ------------------------------------------|
| home_url('/test/')    | Home URL (site address in settings)       | http://www.example.com/test               |
| includes_url()        | Includes directory URL                    | http://www.example.com/wp-includes        |
| content_url('/hello') | Content directory URL                     | http://www.example.com/wp-content/hello   |
| plugins_url()         | Plugins directory URL                     | http://www.example.com/wp-content/plugins |
| wp_upload_dir()       | Upload directory URL *(returns an array)* | http://www.example.com/wp-content/uploads |

`home_url()` and `content_url()` do NOT return a trailing slash *i.e. http://www.example.com instead of http://www.example.com/*


### uploades directory
```
<?php
$upload_array = wp_upload_dir();
$upload_url = $upload_array['baseurl'];
?>
```

## Shortcodes

- `$atts` = an associative aray of attributes, or an empty string if no attirbutes are given;
- `$content` = the enclosed content (if the shorcode is used in its enclosing form)
- `$tag` = the shortcode tag, usefull for shared callback functions

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

## custom fields
```
/*
here we are looking for a custom field called *ng_app* and using it in the template
The plan is to set a custome variable called *ng_app* and have it set an attribute
on a div for Angular injection

//empty variable for custom field value
$ng_app = "";

//check to see if it's set
if(get_post_meta($post->ID, "ng-app")){
//grab custom field value and put into $ngApp variable
$ngApp = get_post_meta($post->ID, "ng-app", true);
//this example puts an attribute into a variable for easy placement into template
$ng_app = "ng-app='" . $ngApp . "'";
}
```

## to enqueue a script
```
//wp_enq..('fileName', 'location of script', array('dependancy'), version, boolean for if requires dependancy loaded first)


wp_enqueue_script('site', get_template_directory_uri() . '/assets/javascript/site.js', array('jquery'), '1.0.0', true);


//so the above looks for site.js in assets/javascript, it needs jQuery, is version 1.0.0 and to load jQuery FIRST
```