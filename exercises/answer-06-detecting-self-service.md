## Answer Exercise 6

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
    <select id="size"></select>
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

<div id="order-id-row" class="row vertical">
  <label for="order_id" title="Order ID">Order ID</label>
  <input id="order_id" type="text" autocomplete="off">
</div>
```

### CSS

Same as CSS in [exercise 5](answer-05-using-dynamic-options.md).

### JavaScript

``` js

var $ = ITRP.$;            // jQuery
var $extension = $(this);  // The UI extension container with custom HTML

var $bicycleType   = $("#type_of_bicycle", $extension);
var $bicycleFields = $("#bicycle-fields", $extension);
var $sizeField     = $("#size", $extension);
var $orderIdRow    = $("#order-id-row", $extension);

var configurationByType = {
  "road": {
    "required": ["#tire_size", "#tape_color"],
    "sizes": [
      "",
      44, 45, 46, 47, 48, 49,
      50, 51, 52, 53, 54, 55, 56, 57, 58, 59,
      60, 61, 62, 63, 64, 65, 66
    ],
    "visible": ["#size", "#chain_size", "#tire_size", "#tape_color"]
  },
  "mountain": {
    "required": ["#chain_size", "#front_shocks"],
    "sizes": ["", 13, 14, 15, 16, 17, 18, 19, 20, 21, 22],
    "visible": ["#size", "#chain_size", "#tire_size", "#front_shocks", "#rear_shocks"]
  },
  "recumbent": {
    "required": ["#flag"],
    "sizes": ["", 42],
    "visible": ["#size", "#chain_size", "#tire_size", "#flag"]
  },
};

// Generates a String contain an <option> element for each value in the
// `options` Array.
function generateOptions(options) {
  return options.map(function(option) {
    return "<option value='" + option + "'>" + option + "</option>";
  }).join();
}

$bicycleType.change(function() {
  // Hide and remove requiredness from all fields.
  $('.row', $bicycleFields).hide();
  $('input', $bicycleFields).required(false);

  if (this.value !== "") {
    // Show fields.
    var visible = configurationByType[this.value].visible.join(",");
    $(visible, $bicycleFields).closest(".row").show();

    // Mark fields required.
    var required = configurationByType[this.value].required.join(",");
    $(required, $bicycleFields).required();

    // Load the frame sizes and reset the selected value.
    var sizes = configurationByType[this.value].sizes;
    var optionsHtml = generateOptions(sizes);
    $sizeField.html(optionsHtml).val("");
  }
}).change();

// Hide the Order ID field for end users
if (ITRP.context === 'self_service') {
  $orderIdRow.hide();
}
```

---

That was easy enough, only a few lines of code were added:

``` js
var $orderIdRow    = $("#order-id-row", $extension);

...

// Hide the Order ID field for end users
if (ITRP.context === 'self_service') {
  $orderIdRow.hide();
}
```

Most of this code has been discussed before. The `ITRP.context` variable is new
and it returns the current context of the UI extension. This can be either one
of the following values:

* `full_ui`: the specialists view, and
* `self_service`: the end users view.

Using this value you can hide the "Order ID" element from end users.

The `ITRP` object contains the `$` variable (that was discussed earlier) and the
"UI extension API". This API consists of a number of methods, objects and values
that can be used to work with a UI extension. The `context` value is part of
that API.

We will talk more about it in the next exercise.

---

## New functionality!

The above way of hiding the field when in Self Service has a number of problems:

* the value is still present in the HTML,
* a client can edit the page to make the field visible and editable again,
* the value is visible when viewing the saved record.

That is why we have been working on a better to achieve this requirement.

You will be able to add the `data-internal` attribute to an element. When this
attribute is present, all of the fields that are enclosed in that element will
be hidden for end-users.

Instead of writing the JS or CSS in above answer, we could've satisfied this
requirement by writing the following HTML:

``` html
<div class="row vertical" data-internal>
  <label for="order_id" title="Order ID">Order ID</label>
  <input id="order_id" type="text" autocomplete="off">
</div>
```

Using this attribute, the field will no longer be present in the HTML for end
users (both in edit and view mode).

[Continue to the next exercise.](07-searching.md)
