## Answer Exercise 8

### HTML

Same as CSS in [exercise 6](answer-06-detecting-self-service.md).

### CSS

Same as CSS in [exercise 5](answer-05-using-dynamic-options.md).

### JavaScript

``` js

var $ = ITRP.$;            // jQuery
var $extension = $(this);  // The UI extension container with custom HTML

var $bicycleType    = $("#type_of_bicycle", $extension);
var $bicycleFields  = $("#bicycle-fields", $extension);
var $sizeField      = $("#size", $extension);
var $orderIdRow     = $("#order-id-row", $extension);
var $reqStatusField = $("#req_status");

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
    $(required, $bicycleFields).required(true);

    // Load the frame sizes and reset the selected value.
    var sizes = configurationByType[this.value].sizes;
    var optionsHtml = generateOptions(sizes);
    $sizeField.html(optionsHtml).val("");
  }
}).change();

// Mark the attachment field required when setting the status to 'Completed'
$reqStatusField.change(function() {
  var isRequired = this.value === 'completed';
  ITRP.field('attachment').required(isRequired);
});

// Hide the Order ID field for end users
if (ITRP.context === 'self_service') {
  $orderIdRow.hide();
}

// Mark the Type of Bicycle field read-only when the record is no longer new.
if (!ITRP.record.new) {
  $bicycleType.readonly(true);
}
```

---

In the first exercise we talked about scoping the jQuery selectors to the HTML
of the UI extension. In this exercise, we actually want to search the entire
page for the "Status" element:

``` js
var $reqStatusField = $("#req_status");
```

This looks up the "Status" field on the Request form and stores a jQuery
object in the `$reqStatusField`. The variable is used to detect when the
"Status" field is changed and trigger an event handler function:

``` js
$reqStatusField.change(function() {
  var isRequired = this.value === 'completed';
  ITRP.field('attachment').required(isRequired);
});
```

The above code checks if the current status was changed to "Completed" and marks
the attachment field as required. Using the `ITRP.field` function, you do not
have to worry about differences in HTML between Self Service and the specialists
view.

The `ITRP.field` function exposes a couple of fields and functions on those
fields. In Self Service, when editing:

* in requests
    * Attachment: `required`, `show`, `hide`, `toggle`
    * Note: `required`, `show`, `hide`, `toggle`, `val`
    * Subject: `required`, `show`, `hide`, `toggle`, `val`
    * Asset: `required`, `show`, `hide`, `toggle`, `val`
* in tasks
    * Attachment: `required`, `show`, `hide`, `toggle`

In the specialists view, when editing:

* requests:
    * Attachment: `required`, `show`, `hide`, `toggle`
    * Note: `val`
    * Internal Attachment: `required`, `show`, `hide`, `toggle`
    * Internal Note: `val`
    * Subject: `val`
* tasks:
    * Attachment: `required`, `show`, `hide`, `toggle`

Important to note is that we do not append a call to `.change()` after
registering the onChange event handler function. Doing this would otherwise make
the attachment required when you edit a completed request that is linked to this
UI extension.

---

The last lines that were added mark the type bicycle field readonly after the
request has been created:

``` js
if (!ITRP.record.new) {
  $bicycleType.readonly(true);
}
```

The `readonly()` function works the same way as the `required()` function.

---

TODO: add a bonus exercise explaining how this example works:
https://developer.4me.com/v1/ui_extensions/advanced_examples/#add-another-set-of-fields

