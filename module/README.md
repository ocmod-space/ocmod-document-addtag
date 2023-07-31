# Document addTag

## Description
**Document addTag** is an OpenCart 3.x extension to extend internal class `Document` by adding two new methods `addTag` and `getTags` to allow store and embed any html tags with custom attributes and content.

1. To add a tag use `$this->document->addTag($tag, $id, $group)`, where:
* `$tag` is an array with tag data, where:
 - `name` (string): tag name, e.g, 'script', 'meta', 'link', etc.;
 - `content` (string) - any data;
 - `attributes` (array) - tag attributes or properties, e.g. array('type' => 'text/javascript);
 - `closing` (bool) - a special flag to make closing tags, e.g., <link/>, <meta/>, or open tags, e.g., <script></script>.
* `$id` is an unique id to identify this entry.
* `$group` is a group where the tag will be placed (e.g, 'header' or 'footer')

Example below allows to store javascript script
```
$content = '
    var button_checkout = "Checkout";
    var link_checkout = "https://www.example.com/index.php?route=checkout/checkout";
';
$tag = array(
    'name'    => 'script',
    'content' => $content,
    'attrs'   => array('type' => 'text/javascript'),
    'closing' => true
);
$id = 'unique_id';
$group = 'header';

$this->document->addTag($tag, $id, $group)
```


2. To get tags use something like the next:
```
$tags = '';

if ($this->document->getTags('header')) {
    foreach ($this->document->getTags('header') as $tag) {
        if ($tag['id'] == $route) {
            $tags .= '<' . $tag['tag']['name'];

            $attrs = '';

            foreach ($tag['tag']['attrs'] as $attr => $value) {
                $attrs .= $attr . '="' . $value . '" ';
            }

            $tags .= ' ' . $attrs;

            if (isset($tag['tag']['closing']) && $tag['tag']['closing']) {
                $tags .= '>';
            } else {
                $tags .= ' />';
            }

            $tags .= $tag['tag']['content'];

            if (isset($tag['tag']['closing']) && $tag['tag']['closing']) {
                $tags .= '</' . $tag['tag']['name'] . '>';
            }
        }
    }
}
```

## License
Licensed under the [MIT License](https://raw.githubusercontent.com/ocmod-space/ocmod-document-addtag/main/LICENSE.txt)
