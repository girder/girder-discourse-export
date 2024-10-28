# How to Get Worker Config Values

## ThomasThelen on 2018-08-14T23:35:07.611Z

I’m trying to get the value of some items in my girder\-worker\-config. I’ve tried


`girder-worker-config get 'girder_io', allow_direct_path`  

`girder-worker-config get [girder_io], allow_direct_path`  

`girder-worker-config get girder_io, allow_direct_path`  

`girder-worker-config get '[girder_io]', allow_direct_path`


And haven’t had any luck. I get `No section: 'girder_io'`


`girder-worker-config list`  

gives


\[girder\_io]  

diskcache\_enabled \= 0  

diskcache\_directory \= girder\_file\_cache  

diskcache\_eviction\_policy \= least\-recently\-used  

diskcache\_size\_limit \= 1073741824  

diskcache\_cull\_limit \= 10  

diskcache\_large\_value\_threshold \= 1024  

allow\_direct\_path \= False


---

