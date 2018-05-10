## Exercise 4: Using configuration object

After using the UI extension in production for a couple of weeks you receive
complaints that the users are not always entering all information. The new
requirement is to make the following fields required:

* When type 'Road' is selected:
    * Tire size
    * Tape color
* When type 'Mountain' is selected:
    * Chain size
    * Front shocks
* When type 'Recumbent' is selected:
    * Flag

This is usually where the lines of code of an UI extensions explode. You have to
specify for each combination what fields are required and you have to do it in
JavaScript.

Users will ususally go for a similar pattern as we saw in the first exercise:

``` js
if (this.value === "road") {
  $("#tire_size").required();
  ...
} else if (this.value === "mountain") {
  $("#chain_size").required();
  ...
}
...
```

If the UI extensions is not too complex, go right ahead. But it is important to
be able to refactor this into something more manageable when more options or
fields are added.

### Exercise

Implement requirement without adding an extra if/else branch.

Storing the different combinations of bicycle types and their required fields in
a JavaScript object is good starting point.

Good luck!

[Continue to answer.](answer-04-using-configuration-object.md)
