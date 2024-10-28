# HTTPS, Authorization and Girder-Authorization

## finetjul on 2019-09-14T08:23:54.961Z

Hi,


I am having challenges to login with Girder available under HTTPS.


Related to the following change:  



[github.com/girder/girder](https://github.com/girder/girder/commit/339da9ee3ad56d3b8aa872b35e4c8325a2a605d3)


[![brianhelba](https://discourse.girder.org/uploads/default/original/1X/db12671e58ac7f9355a174737b2213a77cdacefd.png)](https://github.com/brianhelba)
#### [Use "Authorization" as the default header for Basic Auth](https://github.com/girder/girder/commit/339da9ee3ad56d3b8aa872b35e4c8325a2a605d3)



```
"Authorization" is a standard RFC7235 header, which is automatically supported
by nearly any third-party HTTP client. Using "Authorization" as the preferred
header makes...
```


 by [brianhelba](https://github.com/brianhelba)
 on [11:48PM \- 06 May 19 UTC](https://github.com/girder/girder/commit/339da9ee3ad56d3b8aa872b35e4c8325a2a605d3)


 changed **6 files**
 with **11 additions**
 and **12 deletions**.









I don’t understand how I can give both the HTTPS `Authorization` header and the Girder `Authorization` headers at the same time. With Girder 2, I believe I had the following headers in my `api/v1/user/authentication` request and it worked well: `Authorization=Basic myHTTPSBase64LoginPwd; Girder-Authorization=Basic myGirderBase64LoginPwd`


If I do the same with Girder 3, Girder considers only `myHTTPSBase64LoginPwd` and it ignores `Girder-Authorization`


I guess that should be an easy fix server side (use ‘Girder\-Authorization’ if available in request headers). However, on the client side, should “Girder\-Authorization” be sent if URL starts with HTTPS ?


What do you think ?


---

## Zach_Mullen on 2019-09-14T23:21:00.913Z

Hi Julien,


Just so I understand, the issue here is that you have some front\-end layer that requires you to log in via the Authorization header, but you also need a separate login header for Girder, so you’re passing both on the same request? (FWIW, this has nothing to do with HTTPS, all of this is inside HTTP\-land.)


I’d be fine to change Girder to look for `Girder-Authorization` prior to `Authorization`, but a proper workaround, assuming you have control over the front\-end service that needs the `Authorization` header, would be to configure it to not forward that header to Girder.


---

