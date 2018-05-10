## Answer Exercise 4

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
  "road": ["#tire_size", "#tape_color"],
  "mountain": ["#chain_size", "#front_shocks"],
  "recumbent": ["#flag"]
};

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

First, we specify the given combinations of bicyle types and required fields in
a JavaScript object that we can use in the rest of the code:

``` js
var requiredFieldsByType = {
  "road": ["#tire_size", "#tape_color"],
  "mountain": ["#chain_size", "#front_shocks"],
  "recumbent": ["#flag"]
};
```

Calling `requiredFieldsByType['road']` will return an Array containing the ids
of the fields that should be required when the "Road" bicycle type has been
selected.

---

Similar to hiding all fields before applying the `with-..-selected` class, we
first remove the requiredness from all fields:

``` js
$('input', $bicycleFields).required(false);
```

The `$('input', $bicycleFields)` part will select all `input` elements within
the scope of `$bicycleFields`. If you have `select` element in there, you
can expand the selector to `'input, select'`.

---

The last new lines are:

``` js
var selector = requiredFieldsByType[this.value].join(',');
$(selector, $bicycleFields).required();
```

The `this.value` (e.g. "road" or "mountain") is used to retrieve the ids of the
required fields from the JavaScript object as shown before.

Calling `join(',')` on a Array will join the elements into a String using the
comma-character as a separator. For example:

``` js
['#tire_size', '#tape_color'].join(',')
// => '#tire_size,#tape_color'

['#tire_size'].join(',')
// => '#tire_size'

['#tire_size', '#tape_color'].join(' and ')
// => '#tire_size and #tape_color'
```

The joined elements are used to select the fields within the `$bicycleFields`
scope and make them required.

---

Notice that, we can implement the hiding/showing of fields this way. The object
then also stores what fields are visible:

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

Those last lines become something like this:

``` js
var visibleFields = fieldsByType[this.value]['visible'].join(',');
$(visibleFields, $bicycleFields).show();

var requiredFields = fieldsByType[this.value]['required'].join(',');
$(requiredFields, $bicycleFields).required();
```

You can get rid of the CSS in that case.

Be careful not to make the configuration object too complex though, as it might
be difficult to understand in a couple of months.

[Continue to the next exercise.](05-using-dynamic-options.md)
