# Answer Exercise 4

### HTML

Same as HTML in [exercise 3](answer-03-using-dynamic-classes.md).

### CSS

Same as CSS in [exercise 3](answer-03-using-dynamic-classes.md).

### JavaScript

``` js
var $ = ITRP.$;            // jQuery
var $extension = $(this);  // The UI extension container with custom HTML

var $bicycleType   = $("#type_of_bicycle", $extension);
var $bicycleFields = $("#bicycle-fields", $extension);

var requiredFieldsByType = {
  "road": ["#tire-size", "#tape-color"],
  "mountain": ["#chain-size", "#front-shocks"],
  "recumbent": ["#flag"]
}

$bicycleType.change(function() {
  $bicycleFields.removeAttr('class');
  $('input', $bicycleFields).required(false);

  if (this.value !== '') {
    $bicycleFields.addClass('with-' + this.value + '-selected');

    var selector = requiredFieldsByType[this.value].join(',');
    $(selector, $bicycleFields).required();
  }
}).change();
```

---

Notice that, we could've also implemented the hiding/showing of fields this way.
Something like this could work:

``` js
{
  "road": {
    "visible": [..., ...],
    "required": [..., ..., ...]
  },
  "mountain": {
    "visible": [...],
    "required": [..., ..., ...]
  },
  ...
}
```

Be careful not to make the configuration object too complex though, as it might
be difficult to understand in a couple of months.

[Continue to the next exercise.](05-using-dynamic-options.md)
