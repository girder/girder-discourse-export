# Deploying Girder on AWS Lightsail doesn't serve

## rbaustin on 2019-12-03T20:33:24.939Z

I’m trying to deploy a brand new Girder 3 instance on an AWS Lightsail VPS, and I can’t firgure out why it’s not working.


Steps to reproduce (based on [https://girder.readthedocs.io/en/stable/installation\-quickstart.html](https://girder.readthedocs.io/en/stable/installation-quickstart.html)):


* Created a new VPS running Ubuntu 18
* Opened up the network to allow access to port 8080
* Installed python, mongo, and node (as per readthedocs)
* Built the project with `girder build`
* Ran the server with `girder serve`


There was no response from the server at the root IP, nor IP:8080


Next, I tested a very minimal Node.js app running at port 8080, and that served fine.


I also ran the server with `girder serve --port 0.0.0.0`, and in this case, the root IP responds with `Not Found`, but still nothing at IP:8080


I’m a bit at a loss on what to try next, so any ideas would be greatly appreciated.


---

## brianhelba on 2019-12-05T04:48:00.152Z

Hi [@rbaustin](/u/rbaustin),


Can you do the following:


1. SSH into your server
2. Run `girder serve`
3. Paste the full output here
4. SSH into your server again from a separate SSH session (while the first is still running)
5. Run `curl -v 127.0.0.1:8080`
6. Paste the full output here

---

## rbaustin on 2019-12-05T16:51:22.083Z

Sure thing [@brianhelba](/u/brianhelba)


Output from `girder serve`:



```
Running in mode: production
Connecting to MongoDB: mongodb://localhost:27017/girder
[05/Dec/2019:16:47:59] ENGINE Bus STARTING
[05/Dec/2019:16:47:59] ENGINE Started monitor thread '_TimeoutMonitor'.
Started asynchronous event manager thread.
[05/Dec/2019:16:47:59] ENGINE Serving on http://127.0.0.1:8080
[05/Dec/2019:16:47:59] ENGINE Bus STARTED

```

Output from `curl`:



```
* Rebuilt URL to: 127.0.0.1:8080/
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
> GET / HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.58.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Date: Thu, 05 Dec 2019 16:49:28 GMT
< Content-Length: 1188
< Content-Type: text/html;charset=utf-8
< Allow: DELETE, GET, HEAD, PATCH, POST, PUT
< Server: Girder 3.0.5
< 
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Girder</title>
    <link rel="stylesheet" href="/static/built/girder_lib.min.css">
    <link rel="icon" type="image/png" href="/static/built/Girder_Favicon.png">
  </head>
  <body>
    <div id="g-global-info-apiroot" class="hide">/girder/api/v1</div>
    <script src="/static/built/girder_lib.min.js"></script>
    <script src="/static/built/girder_app.min.js"></script>
    <script type="text/javascript">
        $(function () {
            girder.events.trigger('g:appload.before');
            girder.app = new girder.views.App({
                el: 'body',
                parentView: null,
                contactEmail: 'kitware\u0040kitware\u002Ecom',
                privacyNoticeHref: 'https\u003A\u002F\u002Fwww\u002Ekitware\u002Ecom\u002Fprivacy',
                brandName: 'Girder',
                bannerColor: '\u00233F3B3B',
                registrationPolicy: 'open',
                enablePasswordLogin: true
            }).render();
            girder.events.trigger('g:appload.after', girder.app);
        });
    </script>
  </body>
</html>
* Connection #0 to host 127.0.0.1 left intact

```

---

## rbaustin on 2019-12-05T17:50:00.345Z

Alright, I’m an idiot. I think the server IP got changed on me between running `girder serve` and `girder serve --host 0.0.0.0`. It kind of works now, although I do have some errors. But I am in the right place to continue. Sorry about that. Not sure how to close this issue.


---

## brianhelba on 2019-12-05T20:26:47.667Z

I’m glad you’ve resolved the issue, [@rbaustin](/u/rbaustin). I’m happy to help as a [rubber duck](https://en.wikipedia.org/wiki/Rubber_duck_debugging).


---

