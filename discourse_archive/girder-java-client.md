# Girder Java Client?

## sfines on 2018-03-20T17:56:11.487Z

Does a Java Girder Client exist? I need to stream data in from spark processing and would like to avoid writing my own if possible.


---

## Zach_Mullen on 2018-03-20T18:24:52.814Z

Hi sfines, at this time we do not have a Java client, though depending on what you want to do it may be straightforward to just use an HTTP library.


---

## Zach_Mullen on 2018-03-20T19:10:41.605Z

The Girder REST API uses the OpenAPI (formerly swagger) standard, so it’s possible to auto\-generate clients for a variety of languages. You could use this project to generate a Java client for your Girder instance:




![](https://github.githubassets.com/favicons/favicon.svg)
[GitHub](https://github.com/swagger-api/swagger-codegen)


![](https://discourse.girder.org/uploads/default/original/1X/ee5dd3ba8a387dbfbd0a0c2f93c274b74e652941.png)
### [swagger\-api/swagger\-codegen](https://github.com/swagger-api/swagger-codegen)


swagger\-codegen contains a template\-driven engine to generate documentation, API clients and server stubs in different languages by parsing your OpenAPI / Swagger definition. \- swagger\-api/swagger\-...







Hope this helps,


\-Zach


---

## sfines on 2018-03-20T21:01:41.415Z

It does, though I have to figure the correct swagger endpoint… using /api/v1 doesn’t seem to work.


---

## Zach_Mullen on 2018-03-20T21:07:25.801Z

It’s at `/api/v1/describe`


---

## sfines on 2018-03-21T20:49:26.150Z

Well, that works for generating everything except the models… it seems that something in the description doesn’t parse so I’m generating the case classes by hand, but that’s a much easier task.


---

