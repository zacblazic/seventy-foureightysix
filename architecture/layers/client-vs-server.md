
# Client-Side versus Server-Side Processing

There is a case for both client and server side processing of whatever data you're working with, but considerations need to be made taking into account the limitations of each.

## Server-Side Processing

You don't want to be sending massive amounts of data down to the client's browser and have it perform some intensive calculations. That's why we have powerful servers to which we can offload these tasks.

Once you've completed a task you can then simply return a small lightweight response to the client that represents the result of his or her task.

## Client-Side Processing

Just because servers are powerful doesn't mean we have to make them do everything. Nowadays, the client-side code is much more prevalent, with frameworks such as Angular, Backbone & Ember.

These frameworks represent a shift in the residency of application layers, moving the model, view, and controller to the client. Of course there is still server processing involved, and this is usually done through the use of AJAX requests.
