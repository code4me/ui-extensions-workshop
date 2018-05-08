# Answer Exercise 2

### HTML

``` html
<div class="row vertical">
  <label for="type_of_bicycle" title="Type of bicycle">Type of bicycle</label>
  <select id="type_of_bicycle">
    <option value=""></option>
    <option value="road">Road</option>
    <option value="mountain">Mountain</option>
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
</div>
```

The two new fields, "Size" and "Rear shocks", were added to the HTML.

Each row now has a `id` attribute that can be used in a CSS selector to, for
example, hide the row.

All rows are now wrapped by a `div` element with id `#bicycle-fields`. We'll see
how that is used in a bit.

### JavaScript

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
  }
}).change();
```

We got rid of some code, hurray! Less code, less bugs :)

The lines calling `show` and `hide` on each row were replaced by:

``` js
$bicycleFields.addClass('with-road-selected');
```

That will set the CSS class `with-road-selected` on the `#bicycle-fields`
element when the "Road" option is selected.

When the empty option is selected, the class attribute is removed. This is only
possible when there are no other CSS classes already set on the
`#bicycle-fields` element. Otherwise, you should explicitly remove the classes
to avoid removing the existing classes as well:

``` html
<div id="bicycle-fields" class="blink">
  ...
</div>
```

``` js
$bicycleFields.removeClass('with-road-selected with-mountain-selected');
```

Now, let's look at the CSS to see how those classes are used.

### CSS

``` scss
#bicycle-fields {
  .row { display: none; }
}

#bicycle-fields.with-road-selected {
  #size-row,
  #chain-size-row,
  #tire-size-row,
  #tape-color-row {
    display: block;
  }
}

#bicycle-fields.with-mountain-selected {
  #size-row,
  #chain-size-row,
  #tire-size-row,
  #front-shocks-row,
  #rear-shocks-row {
    display: block;
  }
}
```

Let's look at the CSS selectors to see how they help us to hide and show the
correct rows depending on which type of bicycle is selected.

When no type of bicycle has been selected (e.g. the empty option has been
selected) all rows are hidden. The following selector achieves this:

``` scss
#bicycle-fields {
  .row { display: none; }
}
```

The JavaScript will add either `with-road-selected` or `with-mountain-selected`
to the `#bicycle-fields` wrapper element depending on which type of bicycle was
selected. Two CSS selectors are used for this:

``` scss
#bicycle-fields.with-road-selected { ... }
#bicycle-fields.with-mountain-selected { ... }
```

These selectors are more specific than the first selector. This means that every
CSS rule added to these blocks overrides (or is more !important) than similar
rules defined in the first selector.

We can use this to override the `display: none` rule on the `.row` elements by
`display: block` for each row that needs to be shown for the selected type of
bicycle:

``` scss
#size-row,
#chain-size-row,
#tire-size-row,
#tape-color-row {
  display: block;
}
```

---

You might have noticed that it is not actually CSS. It is actually
[SCSS](https://sass-lang.com/guide). It is converted to CSS on the server before
rendering it in the HTML page.

SCSS is an extension of the syntax of CSS. This means that every valid CSS
stylesheet is a valid SCSS file with the same meaning.

SCSS allows you (among other things) to nest your CSS selectors. This makes it
possible to write simpler, more readable CSS. As comparison example, let's take
a look what the above SCSS selectors looks like in CSS:

``` css
#bicycle-fields .row {
  display: none;
}

#bicycle-fields.with-road-selected #size-row,
#bicycle-fields.with-road-selected #chain-size-row,
#bicycle-fields.with-road-selected #tire-size-row,
#bicycle-fields.with-road-selected #tape-color-row {
  display: block;
}

#bicycle-fields.with-mountain-selected #size-row,
#bicycle-fields.with-mountain-selected #chain-size-row,
#bicycle-fields.with-mountain-selected #tire-size-row,
#bicycle-fields.with-mountain-selected #front-shocks-row,
#bicycle-fields.with-mountain-selected #rear-shocks-row {
  display: block;
}
```

[Continue to the next exercise.](03-using-dynamic-classes.md)
