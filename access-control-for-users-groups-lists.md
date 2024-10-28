# Access Control for Users/Groups Lists

## John_Roberts on 2018-09-10T21:06:01.163Z

Update: I see now that it’s only the User’s page that is exposed without requiring a login. At least on my instance, I can’t see Groups or Collections without first logging in. I can see Users without logging in.


I’m running an older version of Girder:  

This instance of Girder was built on 9/25/2017 . API version 2\.3\.0\.


I’m wondering if there’s an option to limit the view of the Users/Groups pages to authenticated users only, essentially requiring login.


I looked in the online documentation, but I didn’t see any mention of such capability. I’d consider upgrading if this were possible, perhaps either by setting permissions on the User/Groups pages themselves, or a flag in each User Profile that indicates whether their account should be “published” to the Users page.


For security reasons, I would prefer if only the authenticated users could see the full list of members.


Thanks,  

John.


---

## John_Roberts on 2018-09-10T21:23:00.634Z

More details.


I’m running an Apache proxy out in front of the Girder (with Girder running within a Docker environment).


At this point, the Apache proxy acts as a passive pass through.


I suppose it might be possible to determine at the proxy stage whether the user was authenticated and then permit or deny access to the Users URL.


For that matter, I could block all access with a simple filter in the proxy, but presumably the admin would need access to the User list at some point.


John.


---

## David_Manthey on 2018-09-12T13:56:05.649Z

Hi John,


Girder doesn’t currently have a way to do this. We have some issues requesting such behavior (see issue \#2252 and \#659).


One way to just restrict the user access list would be to have a Girder plugin alter just that route. For example, you could make a plugin with the file `server/__init__.py`:



```
from girder.api import access


def load(info):
    # The first route in the user route table is *probably* the GET /user route
    route, handler = info['apiRoot'].user._routes['get'][0][0]
    # Only adjust this if this is the route we expect
    if route == () and getattr(handler, 'accessLevel', None) == 'public':
        # Update the route tuple by wrapping the handler so it requires user access
        info['apiRoot'].user._routes['get'][0][0] = (route, access.user(handler))

```

I haven’t tested the ramifications of this fully, so use with caution. For instance, this doesn’t give true security – an anonymous client could poll the `/user/{id}` endpoint to discover users or learn about users through other endpoints.


* David

---

