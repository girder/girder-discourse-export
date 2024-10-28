# Authorization issue when downloading a resource

## nicholsn on 2018-09-07T13:33:58.435Z

Hello,


I have a folder that I have set to be private. When I try to download an item in this directory in the UI, I get the following message:


`{ "message": "Read access denied for folder 5b6af63271d638e57307eb89 (user None).", "type": "access" }`


However, if I use the girder\-cli there is no issue and I can download the file.


Here are a few details about my configuration that may help figuring out what is up:


* This instance of Girder was built from Git version [7c5cd54](https://github.com/girder/girder/tree/7c5cd5448ccd5df05545eb2f190e6d309cf75a4d). API version 2\.4\.0\.
* For *Routing* I have *core\_girder* set to `/girder/`, *core\_static\_root* set to `/girder/static/` and *api\_route* set to `/girder/api/v1` in my girder config.
* Girder is running behind an apache reverse proxy


Any thoughts on why Girder doesn’t recognize that I am logged when I try to download something in the UI?


Thanks!


---

## nicholsn on 2018-09-19T17:53:38.884Z

any thoughts on this?


---

## nicholsn on 2018-09-27T19:01:42.379Z

Revisiting this, it looks like when I login I get a 401 when girder try to setup a connection over the notification stream:



> girder/api/v1/notification/stream?:1 Failed to load resource: the server responded with a status of 401 (Unauthorized)


The notification stream is enabled under the advanced settings, but the stream is still getting blocked and not sure why it is throwing a 401, which is not a code documented in the rest api.


---

## Zach_Mullen on 2018-09-27T19:27:13.344Z

Hello,


This could be a result of using scoped tokens created from an API key in the swagger page. If you did this, it would have overwritten your normal login token.


We fixed this issue on the server, but it was very recent, so your instance might not have the fix. As a solution, you can clear your cookies for the Girder server’s domain and log in again through the web client, and that should clear it up.


---

## nicholsn on 2018-09-29T15:18:05.731Z

Hi Zach,


Thanks for the suggestion! I had already tried clearing cookies and was still seeing the 401 error and cookies were not being set.


On a hunch, I switched from running in production mode to running in development mode and for whatever reason cookies started being set again, even after switching back to production and clearing cookies. Not sure what that triggered, but its working!


Was this issue fixed on the master branch or on a branch that is consistent with the 2\.5\.0 documentation? If the latter, can you point me to it?


---

