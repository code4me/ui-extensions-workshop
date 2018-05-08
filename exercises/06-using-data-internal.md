## Using data-internal attribute

Another requirement!

The specialists handling the new employee requests order the bike, receive an
order identifier and have to wait two weeks until the bike is delivered. They
want us to add a 'Order ID' field that is only visible for specialists.

When adding the `data-internal` attribute to an element, all of the fields that
are enclosed in that tag will be hidden for end-users.

``` html
<div id="order-id-field" class="row vertical" data-internal>
  <label for="order-id" title="Order ID">Order ID</label>
  <input id="order-id" type="text" autocomplete="off">
</div>
```

As this is not available in production yet, I will demo it.

[Continue to the next exercise.](07-using-data-searchable.md)
