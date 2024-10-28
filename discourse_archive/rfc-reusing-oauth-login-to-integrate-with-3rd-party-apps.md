# [RFC] Reusing oauth_login to integrate with 3rd party apps

## xarthisius on 2019-04-09T14:46:25.647Z

### Rationale


I’d like to integrate Girder with external apps through the OAuth flow. Possible use cases:


* Girder user would like to create a job that accesses a private repo from GitHub using their credentials.
* Girder user would like to access/import data from a Globus endpoint


### Approach


I’d like to avoid rewriting `oauth_login` plugin and reuse as much code as possible. I think that in order to achieve my goals it would be sufficient to short\-circuit the login process based on events. As an example an event handler for `oauth.auth_callback.before` could store the provider’s access token in the User model and stop further code execution in `GET /oauth/<provider>/callback`:



```
diff --git a/plugins/oauth/girder_oauth/rest.py b/plugins/oauth/girder_oauth/rest.py
index 5a1b92624..63ca4e74b 100644
--- a/plugins/oauth/girder_oauth/rest.py
+++ b/plugins/oauth/girder_oauth/rest.py
@@ -133,18 +133,23 @@ class OAuth(Resource):
         providerObj = provider(cherrypy.url())
         token = providerObj.getToken(code)
 
-        events.trigger('oauth.auth_callback.before', {
+        event = events.trigger('oauth.auth_callback.before', {
             'provider': provider,
             'token': token
         })
 
+        if event.defaultPrevented:
+            raise cherrypy.HTTPRedirect(redirect)
+
         user = providerObj.getUser(token)
 
-        events.trigger('oauth.auth_callback.after', {
+        event = events.trigger('oauth.auth_callback.after', {
             'provider': provider,
             'token': token,
             'user': user
         })
+        if event.defaultPrevented:
+            raise cherrypy.HTTPRedirect(redirect)
 
         girderToken = self.sendAuthTokenCookie(user)
         try:

```

Please let me know if that’s a reasonable approach.


---

## Zach_Mullen on 2019-04-10T17:01:26.268Z

That change looks sensible enough to me to make a PR.


For what it’s worth, here’s an example of a Globus plugin that stores a user’s Globus token on the user document by hooking into the `after` event: <https://github.com/girder/globus_endpoints/blob/master/globus_endpoints/>**init**.py\#L269


---

