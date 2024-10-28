# CORS policy with Girder

## finetjul on 2019-02-09T19:38:12.047Z

Hi,


I have a Girder server running on [www.my\-awesome\-server.com](http://www.my-awesome-server.com) .


I have a front page running locally on my laptop (localhost using an apache serveur) and I make it access some custom girder API on [my\-awesome\-server.com](http://my-awesome-server.com).


Problem is, Chrome complains about CORS policy:  

*Access to XMLHttpRequest at ‘[http://my\-awesome\-server/api/v1/viewer/configuration?token\=](http://my-awesome-server/api/v1/viewer/configuration?token=)…’ from origin ‘<http://localhost:8080>’ has been blocked by CORS policy: Response to preflight request doesn’t pass access control check: Redirect is not allowed for a preflight request.*


On the Girder server, I configured the Advanced setting “CORS Allowed Origins” with \* and I restarted the server. When I check the network request headers, I don’t see `Access-Control-Allow-Origin`  

Is there anything else I must do ?


It works fine if I start Google Chrome without web security checks.


Thanks,  

Julien.


---

## Zach_Mullen on 2019-02-10T15:29:43.201Z

You should definitely see `Access-Control-Allow-Origin` headers in responses from Girder REST requests if you’ve configured the server to allow it. What version of Girder are you running?


---

## Jonathan_Beezley on 2019-02-11T13:37:12.079Z

I suspect the problem might be related to the server sending back a redirect. As stated in the error message, redirects are not allowed in the preflight OPTIONS request. The browser might just be dropping the CORS header because of that.


You might check the request in the terminal using curl (right click on the request and select “Copy as cURL”). Getting the verbose output from that might help illuminate what is actually going on.


---

## Zach_Mullen on 2019-02-11T14:59:32.902Z

Could the domain mismatch be causing the redirect? (`www.my-awesome-server.com` vs `my-awesome-server.com`)


---

## finetjul on 2019-02-11T15:08:45.501Z

Thanks Jonathan for the curl copy hint. With it, I realized that the URL I was using was http but the server redirects http to https. Therefore the OPTIONS request was failing with error “301 Moved Permanently”.


Now that I changed the URL from http to https, I have an error “401 Unauthorized”. While my URL works with GET, it fails with OPTIONS. I guess it means I need to change the server to accept OPTIONS requests…


Keeping you informed…


Thanks for your help,  

Julien.


---

## Jonathan_Beezley on 2019-02-11T15:20:41.751Z

That is probably the CORS preflight request, which is required for any HTTP verb that can mutate the server state (PUT, POST, DELETE, etc.). The browser automatically sends it when necessary.


I believe that girder will reply to the OPTIONS request without checking auth, so I’m not sure where the 401 is coming from.


---

## Zach_Mullen on 2019-02-11T15:22:02.086Z

Yes, if the request gets through to Girder it will give you a 200 OPTIONS response, my guess is something else is giving that response. Do you have the full response output including headers?


---

