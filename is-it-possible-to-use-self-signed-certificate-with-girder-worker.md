# Is it possible to use self-signed certificate with girder_worker ?

## paul.choisel on 2022-01-12T09:02:23.311Z

Hi all,


I need to enable HTTPS with a self\-signed certificate on my Girder server, but this apparently make it incompatible with girder\_worker.  

In multiple places, girder\_worker uses the Python library *requests* to perform HTTP(S) requests on Girder, but there is no way of specifying that the certificate should not be verified (with the kwarg *verify*)  

The *requests* library fails when trying to contact a server through HTTPS without a signed certificate.


Do you see a way of making this work ?  

CherryPy does not seem to support both HTTP and HTTPS at the same time and I don’t think that deploying another Girder server with HTTP enabled would be a great idea.


The requests library has a *session* mechanism that could solve this problem, it is implemented in girder\_client. Do you think it would be valuable to use it in girder\_worker too ?


Thanks a lot for your answers


---

