## Exercise 7: Searching fields

Of course, specialists should be able to search for the 'Order ID' and find the
correct request.

### Exercise

Unfortunately, implementing this requirement is currently not possible yet. But
together with the ability to mark fields internal, we have been working on a way
to mark fields that should be available in the search engine.

Adding the `data-searchable` attribute will make a value searchable:

``` html
<div class="row vertical" data-searchable>
  <label for="order_id" title="Order ID">Order ID</label>
  <input id="order_id" type="text" autocomplete="off">
</div>
```

The above HTML will make it possible to find the request by searching on the
"Order ID".

We hope to have this and the `data-internal` attribute available over the next
couple of weeks.

[Continue to the next exercise.](08-using-extensions-api.md)
