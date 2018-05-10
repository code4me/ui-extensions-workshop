## Exercise 3: Using dynamic classes

Another requirements comes in: a new type of bike is available, the recumbent
bike.

![Recumbent bike](../images/recumbentbike.png)

The type of bike has the following fields:

* Chain size
* Tire size
* Flag

The new 'Flag' field needs to be added and you must specify the fields that are
visible when the 'Recumbent' bike is selected. This is pretty straightforward
and the code still looks simple enough. A possible solution looks like this
(HTML and full CSS omitted):

``` js
var $ = ITRP.$;            // jQuery
var $extension = $(this);  // The UI extension container with custom HTML

var $bicycleType   = $("#type_of_bicycle", $extension);
var $bicycleFields = $("#bicycle-fields", $extension);

$bicycleType.change(function() {
  $bicycleFields.removeAttr('class');

  if (this.value === "road") {
    $bicycleFields.addClass('with-road-selected');

  } else if (this.value === "mountain") {
    $bicycleFields.addClass('with-mountain-selected');

  } else if (this.value === "recumbent") {
    $bicycleFields.addClass('with-recumbent-selected');
  }
}).change();
```

``` scss
...
#bicycle-fields.with-recumbent-selected {
  #chain-size-row,
  #tire-size-row,
  #flag-row {
    display: block;
  }
}
...
```

However, we are repeating the same code for each type that is selected. Let's
not do that, as it will lead to maintainability problems down the road.

### Exercise

Implement requirement without increasing the number of JavaScript lines. You can
dynamically build the CSS class name to achieve this.

Good luck!

[Continue to answer.](answer-03-using-dynamic-classes.md)
