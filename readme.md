WP cheats
=========

## Invoke `short codes` from template
`<?php echo do_shortcode("[shortcode]"); ?>`

## get the `URL` of...

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

