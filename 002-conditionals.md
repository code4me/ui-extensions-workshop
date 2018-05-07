## Conditionals

Duration: 50 minutes

Best practices when working with conditional fields / logic.

A common scenario in UI extensions is the need to conditionally hide or show
certain fields and/or values. Depending on the business rules behind the form,
this can become very complex, very quickly.

Let's start with a simple example, see this JsFiddle:

* [https://jsfiddle.net/robinroestenburg/akuua8dx/](https://jsfiddle.net/robinroestenburg/akuua8dx/)

The code in this example was taken from an actual UI extension in production but
the fields and values were adapted a bit for this example. It is representative
of a lot of the JavaScript in UI extensions and it makes for a good example to
refactor to display a few techniques.

Assume your organization allows new employees to select and customize a bike.
You are asked to create a UI extesions that allows the new employee to select
the type of bike and the customizations they want.

The example has a select element with 2 bike types and depending on the selected
bikes some or all of the following fields are shown.

* Road bike
    * Chain size
    * Tire size
    * Tape color
* Mountain bike
    * Chain size
    * Tire size
    * Front shocks

The JavaScript performs the following actions:

* **Lines 1 - 4**: hide all fields initially
* **Lines 6 - 9**: remove the class attribute the input fields (e.g. remove
  requiredness)
* **Line 11**: register an onChange-handler on the 'Bicycle type' field
* **Lines 12 - 16**: hide/show fields when option 'Road' was selected
* **Lines 17 - 21**: mark/unmark fields required when option 'Road' was selected
* **Lines 22 - 26**: hide/show fields when option 'Mountain' was selected
* **Lines 28 - 31**: mark/unmark fields required when option 'Mountain' was selected

The code is pretty verbose, but it works and might do for now.

### Exercise 1: using CSS

A new requirement comes in! Employees have been asking for extra customizations,
they want to be able to specify:

* the size of the bike, and
* the type of rear shocks of a mountain bike.

**IMPLEMENT REQUIREMENT**: [https://jsfiddle.net/robinroestenburg/akuua8dx/](https://jsfiddle.net/robinroestenburg/akuua8dx/)

This relatively simple requirement forced me to add a couple of lines of
JavaScript. In bigger, complex forms this will be a lot more. More code, more
bugs.

[https://jsfiddle.net/robinroestenburg/fnpu9xkm/](https://jsfiddle.net/robinroestenburg/fnpu9xkm/)

Let's remove the need for these extra lines of code, by refactoring the UI
extension. First, load the previous JsFiddle again.

**HINTS**:

* Create CSS classes that specify which fields are shown when/where.
* Add a wrapper element around the fields whose visibility is dependent on the
  selected role.

**RESULT**: [https://jsfiddle.net/robinroestenburg/nsxeqts6/](https://jsfiddle.net/robinroestenburg/nsxeqts6/)

If you now redo the above requirement starting from the JsFiddle below, it costs
you 5 lines CSS:

**RESULT**: [https://jsfiddle.net/robinroestenburg/nsxeqts6/5/](https://jsfiddle.net/robinroestenburg/nsxeqts6/5/)


### Exercise 2: using dynamic classes

Another requirements comes in: another type of bike if available. It is a
recumbent bike, which has the following fields:

* Chain size
* Tire size
* Flag

**IMPLEMENT REQUIREMENT**: [https://jsfiddle.net/robinroestenburg/bu9fhc01/](https://jsfiddle.net/robinroestenburg/bu9fhc01/)

The new 'Flag' field needs to be added and you must specify the fields that are
visible when the 'Recumbent' bike is selected. This is straightforward and the
code still looks simple enough.

**RESULT**: [https://jsfiddle.net/robinroestenburg/bu9fhc01/1/](https://jsfiddle.net/robinroestenburg/bu9fhc01/1/)

However, we are repeating the same code for each type that is selected. Let's do
something about that. You can remove the `if`- conditions by dynamically
building the class name.

**HINTS**:

* ...
* ...

**RESULT**: [https://jsfiddle.net/robinroestenburg/bu9fhc01/2/](https://jsfiddle.net/robinroestenburg/bu9fhc01/2/)


### Exercise 3: using configuration object

After using the UI extension in production for a couple of weeks you receive
complaints that the users are not always entering all information. The new
requirement is to make a number of fields required:

* Road
    * Tire
    * Tape color
* Mountain
    * Chain
    * Front shocks
* Recumbent
    * Flag

**IMPLEMENT REQUIREMENT**: [https://jsfiddle.net/robinroestenburg/xjpy6n0a/](https://jsfiddle.net/robinroestenburg/xjpy6n0a/)

This is usually where the lines of code of an UI extensions explode. You have to
specify for each combination what fields are required and you have to do it in
JavaScript.

Users will ususally go for a similar pattern as we saw in the beginning:

    if (this.value === "road") {
      $("#tire-field").required();
      ...
    } else if (this.value === "mountain") {
      $("#chain-field").required();
      ...
    }
    ...

Again, if the UI extensions is not too complex, go right ahead. But it is
important to refactor into something more manageable when more options or fields
are added.

For this specific requirement we can create a JavaScript object containing the
configuration of the required fields.

**HINTS**:

* You can scope the jQuery selector using `$(selector, scope)`, e.g. `$('input',
  '#bicycle-fields')`.
* You can specify multiple selectors in a single jQuery selector string, e.g.
  `$('#tire-field, #chain-field')`.

**RESULT**: [https://jsfiddle.net/robinroestenburg/xjpy6n0a/1/](https://jsfiddle.net/robinroestenburg/xjpy6n0a/1/)

Notice that, we could've also implemented the hiding/showing of fields this way.
Something like this could work:

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

Be careful not to make the configuration object too complex though, as it might
be difficult to understand in a couple of months.

### Exercise 4: using dynamic select options

TODO dynamically create all options of a select list


### Exercise 5: using data-internal attribute

Another requirement!

The specialists handling the new employee requests order the bike, receive an
order identifier and have to wait two weeks until the bike is delivered. They
want us to add a 'Order ID' field that is only visible for specialists.

**NEW FEATURE**

When adding the `data-internal` attribute to an element, all of the fields that
are enclosed in that tag will be hidden for end-users.

    <div id="order-id-field" class="row vertical" data-internal>
      <label for="order-id" title="Order ID">Order ID</label>
      <input id="order-id" type="text" autocomplete="off">
    </div>

As this is not available in production yet, I will demo it.


### Exercise 6: using data-searchable attribute

Of course, specialists should be able to search for the 'Order ID' and find the
correct request.

    <div id="order-id-field" class="row vertical" data-internal data-searchable>
      <label for="order-id" title="Order ID">Order ID</label>
      <input id="order-id" type="text" autocomplete="off">
    </div>

TODO
