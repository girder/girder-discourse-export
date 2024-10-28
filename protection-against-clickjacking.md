# Protection against clickjacking

## Edern_Haumont on 2019-03-07T15:58:23.886Z

Good afternoon,


The [documentation](https://girder.readthedocs.io/en/stable/security.html) does not say anything about clickjacking protection.  

Is there an integrated way to customize the X\-Frame\-Options header that should be set on all web pages returned by our site served by girder ?


Thank you


---

## Zach_Mullen on 2019-03-08T13:15:18.752Z

There’s nothing built into Girder for this. We normally deploy into production behind a reverse proxy such as nginx, which is a reasonable place to inject headers like this into responses. For nginx, see the [add header](http://nginx.org/en/docs/http/ngx_http_headers_module.html#add_header) directive.


---

## Edern_Haumont on 2019-03-11T07:56:47.706Z

Thank you, that is what I wanted to know.


---

