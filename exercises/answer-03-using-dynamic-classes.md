## Answer Exercise 3

### HTML

``` html
<div class="row vertical">
  <label for="type_of_bicycle" title="Type of bicycle">Type of bicycle</label>
  <select id="type_of_bicycle">
    <option value=""></option>
    <option value="road">Road</option>
    <option value="mountain">Mountain</option>
    <option value="recumbent">Recumbent</option>
  </select>
</div>

<div id="bicycle-fields">
  <div id="size-row" class="row vertical">
    <label for="size" title="Size">Size</label>
    <input id="size" type="text" autocomplete="off">
  </div>

  <div id="chain-size-row" class="row vertical">
    <label for="chain_size" title="Chain size">Chain size</label>
    <input id="chain_size" type="text" autocomplete="off">
  </div>

  <div id="tire-size-row" class="row vertical">
    <label for="tire_size" title="Tire size">Tire size</label>
    <input id="tire_size" type="text" autocomplete="off">
  </div>

  <div id="front-shocks-row" class="row vertical">
    <label for="front_shocks" title="Front shocks">Front shocks</label>
    <input id="front_shocks" type="text" autocomplete="off">
  </div>

  <div id="rear-shocks-row" class="row vertical">
    <label for="rear_shocks" title="Rear shocks">Rear shocks</label>
    <input id="rear_shocks" type="text" autocomplete="off">
  </div>

  <div id="tape-color-row" class="row vertical">
    <label for="tape_color" title="Tape color">Tape color</label>
    <input id="tape_color" type="text" autocomplete="off">
  </div>

  <div id="flag-row" class="row vertical">
    <label for="flag" title="Flag">Flag</label>
    <input id="flag" type="text" autocomplete="off">
  </div>
</div>
```

### CSS

``` scss
#bicycle-fields {
  .row { display: none; }
}

#bicycle-fields.with-road-selected {
  #size-row,
  #chain-size-row,
  #tire-size-row {
    display: block;
  }
}

#bicycle-fields.with-mountain-selected {
  #size-row,
  #chain-size-row,
  #tire-size-row,
  #tape-size-row,
  #front-shocks-row,
  #rear-shocks-row {
    display: block;
  }
}

#bicycle-fields.with-recumbent-selected {
  #size-row,
  #chain-size-row,
  #tire-size-row,
  #flag-row {
    display: block;
  }
}
```

### JavaScript

``` js
var $ = ITRP.$;            // jQuery
var $extension = $(this);  // The UI extension container with custom HTML

var $bicycleType   = $("#type_of_bicycle", $extension);
var $bicycleFields = $("#bicycle-fields", $extension);

$bicycleType.change(function() {
  $bicycleFields.removeAttr('class');

  if (this.value !== '') {
    $bicycleFields.addClass('with-' + this.value + '-selected');
  }
}).change();
```

---

In the previous example, each selectable option had it's own branch in the
if/else-statement. Instead of specifying each option, there is only a single if-
statement present:

``` js
if (this.value !== '') {
  ...
}
```

Nothing has to be done when the empty option was selected. The fields are
already hidden, because of the line that removes the `class` attribute. When a
non-empty option is selected (`this.value !== ''`), the CSS class for that
specific option can be added to the `#bicycle-fields` elements:

``` js
$bicycleFields.addClass('with-' + this.value + '-selected');
```

This evaluates to `$bicycleFields.addClass('with-road-selected')`. This triggers
the CSS rules as explained in the previous exercise, showing the fields for that
option.

[Continue to the next exercise.](04-using-configuration-object.md)
