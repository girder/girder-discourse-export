# User listing is possible without valid login

## Florian_Link on 2019-04-04T13:02:22.859Z

The current girder implementation has a public endpoint to list all users (and their login), see for example:


<https://challenge.kitware.com/girder/api/v1/user>


I think this is a big security problem, since it opens doors for password brute\-forcing.


The user list should be only available to users which have a valid login.


I was told that it is possible to use the extension API to change such a default behavior and would be happy to get help on how to disable the user listing for non\-logged\-in request.


---

## xarthisius on 2019-04-04T16:29:56.098Z

Before every endpoint is actually called Girder triggers an event `rest.<verb>.<path>.before`. You can use it to restrict access, e.g.:



```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from girder.api import access, rest
from girder import events

@access.user
@rest.boundHandler
def _restrictUsers(self, event):
    pass

def load(info):
    events.bind('rest.get.user.before', 'my_plugin', _restrictUsers)

```

would only let authenticated users access `GET /user`


---

