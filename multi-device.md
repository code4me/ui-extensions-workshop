# Multi-device

Duration: 25 minutes

Best practices when creating a UI extension that works correctly on specialist,
self-service and mobile views.


The 4me application can be viewed on a variety of devices. Mobile devices like a
smartphone or a tablet, laptops with low or high resolution screens and desktops
with large external monitors.

This presents some challenges when creating a UI extension.


## Stick to the defaults

When you stick to the snippets without extra styling, you are almost certain to
create a UI extensions that works on all devices and also looks good.

Let's create a UI extension with only snippets:

* Create a new UI extension called 'Defaults'
* Add a text snippet with label "Name"
* Add a number snippet with label "Quantity". Specify minimum, maximum and
  number of decimal places.
* Add a section help explaining the next field.
* Add a checkbox snippet with label "Premium"

Now demo the UI extension:
* Show in SD console.
* Show in Self Service.
* Show in Mobile.
* Show in responsive.

Next to the fact that it works / looks OK, this approach has the following
benefits:

- low maintenance (less code => less bugs/less things to fail)                                                                                                                [11/292]
- support from 4me:
  - if you use a snippet and it does not work correct, we will fix it
  - if there is a problem with a certain browser, we will fix it
  => again low maintenance


Examples where defaults work better / simpler.

> use a section-help element
> https://4me.4me.com/ui_extensions/3000

> also section-help?
> https://4me.4me.com/ui_extensions/3020

> enclose elements in section
> https://4me.4me.com/ui_extensions/3030

> use section-help / less use of sections
> https://4me.4me.com/ui_extensions/3040


## Mobile first

When developing a UI extension, first make sure it works on a mobile device.
On larger devices/screens it will usually work. If you start on a large screen
and come up with some big/complex form, it is almost impossible to make it work
on a mobile device after that.
