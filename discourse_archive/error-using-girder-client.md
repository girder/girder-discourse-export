# Error Using girder-client

## oleks_korshak on 2020-02-18T23:26:27.447Z

Hello,  

I ran `pip install girder-client` on my machine and tried to authenticate with my girder server with the following python script but I get an error about wrong version number. Any suggestions on what I might be doing wrong?  

Girder API version: `[ base url: /api/v1 , api version: 3.0.4.dev4+gda5c16e5e.d20191103 ]`


Python script:



```
import girder_client
gc = girder_client.GirderClient(apiUrl=apiUrl)
gc.authenticate(username, password)

```

Error Message:



```
Traceback (most recent call last):
  File "D:\ProgramFiles\Anaconda3\envs\shapeworks\lib\site-packages\urllib3\contrib\pyopenssl.py", line 485, in wrap_socket
    cnx.do_handshake()
  File "D:\ProgramFiles\Anaconda3\envs\shapeworks\lib\site-packages\OpenSSL\SSL.py", line 1934, in do_handshake
    self._raise_ssl_error(self._ssl, result)
  File "D:\ProgramFiles\Anaconda3\envs\shapeworks\lib\site-packages\OpenSSL\SSL.py", line 1671, in _raise_ssl_error
    _raise_current_error()
  File "D:\ProgramFiles\Anaconda3\envs\shapeworks\lib\site-packages\OpenSSL\_util.py", line 54, in exception_from_error_queue
    raise exception_type(errors)
OpenSSL.SSL.Error: [('SSL routines', 'ssl3_get_record', 'wrong version number')]

```

–  

Oleks


---

## Zach_Mullen on 2020-02-19T18:54:03.216Z

By chance are you trying to speak HTTPS to a server that is not using TLS?


---

## oleks_korshak on 2020-02-19T21:40:38.980Z

I have no idea. How do I go about checking this?


---

