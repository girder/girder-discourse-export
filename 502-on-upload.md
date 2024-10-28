# 502 on upload

## finetjul on 2019-11-14T20:35:40.593Z

Hi,


When I upload into Girder a lot (\>1000\) of small files from a Web browser (e.g. chrome), I eventually get 502 Bad Gateway errors. Here are the logs on the server:



```
root@ofsep:~/Projects/logs# tail -n0 -f error.log
[2019-11-14 15:18:03,374] ERROR: 500 Error
Traceback (most recent call last):
  File "/root/Projects/girder/girder/api/rest.py", line 630, in endpointDecorator
    val = fun(self, path, params)
  File "/root/Projects/girder/girder/api/rest.py", line 1205, in POST
    return self.handleRoute(method, path, params)
  File "/root/Projects/girder/girder/api/rest.py", line 947, in handleRoute
    val = handler(**kwargs)
  File "/root/Projects/girder/girder/api/describe.py", line 679, in wrapped
    return fun(*args, **kwargs)
  File "/root/Projects/girder/girder/api/v1/file.py", line 236, in readChunk
    return UploadModel().handleChunk(upload, chunk, filter=True, user=user)
  File "/root/Projects/girder/girder/models/upload.py", line 137, in handleChunk
    upload = adapter.uploadChunk(upload, chunk)
  File "/root/Projects/girder/girder/utility/filesystem_assetstore_adapter.py", line 167, in uploadChunk
    data = chunk.read(BUF_SIZE)
  File "/root/Projects/girder/girder/utility/__init__.py", line 144, in read
    return self.stream.read(*args, **kwargs)
  File "/root/Projects/env/lib/python3.5/site-packages/cherrypy/_cpreqbody.py", line 480, in read
    return self.fp.read(size, fp_out)
  File "/root/Projects/env/lib/python3.5/site-packages/cherrypy/_cpreqbody.py", line 817, in read
    data = self.fp.read(chunksize)
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 381, in read
    data = self.rfile.read(size)
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/makefile.py", line 410, in read
    val = super().read(*args, **kwargs)
  File "/usr/lib/python3.5/_pyio.py", line 1000, in read
    return self._read_unlocked(size)
  File "/usr/lib/python3.5/_pyio.py", line 1040, in _read_unlocked
    chunk = self.raw.read(wanted)
  File "/usr/lib/python3.5/socket.py", line 575, in readinto
    return self._sock.recv_into(b)
socket.timeout: timed out
Additional info:
  Request URL: POST http://ofsep.kitware.fr/api/v1/viewer/upload/chunk
  Query string: offset=0&uploadId=5dcdb650e4970173fb36100b&token=eyJ...iTASwA
  Remote IP: 90.63.248.229
  Request UID: 49fd4326-2db4-488a-ab7a-2340768bbf0d
[14/Nov/2019:15:18:03] ENGINE socket.error 'cannot read from timed out object'
Traceback (most recent call last):
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 1273, in communicate
    req.respond()
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 1077, in respond
    self.server.gateway(self).respond()
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/wsgi.py", line 148, in respond
    self.write(chunk)
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/wsgi.py", line 232, in write
    self.req.ensure_headers_sent()
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 1124, in ensure_headers_sent
    self.send_headers()
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 1190, in send_headers
    self.rfile.read(remaining)
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/server.py", line 381, in read
    data = self.rfile.read(size)
  File "/root/Projects/env/lib/python3.5/site-packages/cheroot/makefile.py", line 410, in read
    val = super().read(*args, **kwargs)
  File "/usr/lib/python3.5/_pyio.py", line 1000, in read
    return self._read_unlocked(size)
  File "/usr/lib/python3.5/_pyio.py", line 1040, in _read_unlocked
    chunk = self.raw.read(wanted)
  File "/usr/lib/python3.5/socket.py", line 572, in readinto
    raise OSError("cannot read from timed out object")
OSError: cannot read from timed out object

```

Here is some information that may be relevant:


* multiple files are uploaded in parallel using the Girder Upload JS component
* upload requests get stalled for longer and longer until the 502 error rise


Does it mean I need to increase some timeout ? Can it be done via Girder ?


Thanks,  

Julien.


---

## Zach_Mullen on 2019-12-04T16:13:13.167Z

This is happening below the Girder application level, in the guts of cherrypy’s HTTP server. It looks like a result of network oversaturation, how many parallel uploads are we talking about? You may need to scale your deployment to accommodate more traffic.


---

## Jonathan_Beezley on 2019-12-04T16:17:41.381Z

Yes, this looks like the kind of thing that happens when a server is overloaded. Unlikely increasing a timeout is going to solve anything. I would look at monitoring the server resources when this is happening. What is the bottleneck? file I/O? low memory? cpu? etc.


---

