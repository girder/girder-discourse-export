# Temporarily block all requests to plugin

## Edern_Haumont on 2019-03-05T10:12:39.970Z

Good morning everyone,


We developed several linked plugins which open a lot of routes to access from a client.


We have a use case where if we loose the connection to an external service, we want to block (return an error) all incoming requests from a client to these routes. When we can access the external service again, access to these routes should be reopened.


I could not find in the documentation a way to disable/enable a set or all user defined routes on demand. [This discussion](https://stackoverflow.com/questions/32642655/cherrypy-how-to-stop-and-buffer-incoming-request-while-data-is-updated) seems to relate to the topic but we are not sure this is what we need.


Does someone have any pointer on how to close/reopen routes on demand ?


Thanks for your help.


---

## Zach_Mullen on 2019-03-05T12:50:34.980Z

I would just implement a helper function that tests the gateway connection and if it fails, raises a RestException with 502, or whatever error you want to use in this case. Then, just add that into each route handler that you want this behavior on.


The test method could either actually attempt a connection on demand, or lookup some setting in Girder’s `Setting` collection that acts as an indicator of the connectivity state.


---

## Edern_Haumont on 2019-03-07T09:33:30.509Z

Good idea ; actually we already use custom decorators on nearly all of our exposed functions so we could just add the check to them.  

Thank you !


---

