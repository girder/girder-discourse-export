# Compress request answers

## finetjul on 2019-07-23T07:10:54.801Z

Hi,


Within a girder plugin, I created a new route that generates a large json output (betwen 1MB and 10MB).  

Is there some nice way to get my output gzipped ?


The only way I saw would be to teach [\_createResponse](https://github.com/girder/girder/blob/master/girder/api/rest.py#L493-L527) to support the “Accept\-Encoding” request header ? But I’m not sure if it is a good idea to compress all requests as soon as the browser supports “Accept\-Encoding”.


What do you think ?


Thanks,  

Julien.


---

## Zach_Mullen on 2019-07-23T11:22:47.045Z

Hi J2,


I would recommend doing this with your reverse proxy for production. Nginx example: <http://nginx.org/en/docs/http/ngx_http_gzip_module.html>


In that module there are a number of configuration options to control how \& when compression is applied.


---

## finetjul on 2019-07-23T12:20:27.037Z

Thanks Zach, I’ll give it a try.


---

