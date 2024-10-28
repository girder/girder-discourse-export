# Girder javascript client

## finetjul on 2019-03-11T12:59:21.382Z

Hi,


I have some confusion between different options for girder web clients and servers. Here is my findings and questions:


* [external\-web\-clients documentation](https://girder.readthedocs.io/en/stable/external-web-clients.html)
	+ is it the entry point for girder web client solutions ?
	+ is it up to date ?
* [girder](https://www.npmjs.com/package/girder) :
	+ no npm documentation
	+ no UMD module
	+ Works with girder 2\.5\. Does it work with girder 3 ?
	+ no need for window ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:")
* [@girder/core](https://www.npmjs.com/package/@girder/core) :
	+ no npm documentation
	+ no UMD module
	+ works with girder 3\. Does it work with girder 2 ?
	+ requires a defined window
* [@girder/components](https://www.npmjs.com/package/@girder/components):
	+ no npm documentation
	+ with UMD module ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:")
	+ Works with girder 3\. Does it work with girder 2 ?
	+ requires a defined window
* [js\-girder\-client](https://github.com/Kitware/js-girder-client):
	+ obsolete ?
	+ with UMD module ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:")
	+ Works with girder 2\.5 (does it work with girder 3?)
	+ no need for “window” ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:")
* [paraviewweb](https://github.com/Kitware/paraviewweb/tree/master/src/IO/Girder):
	+ no UMD module
	+ I did not tested
	+ no need for “window” ?
	+ Works with girder 2\.5 (does it work with girder 3?)


Did I miss other clients ?


Indeed, I have a nodejs server (paraviewweb related) and I would like to query a girder server.  

What is your recommended way to fix the “window is not defined” error to get @girder/core and @girder/components working ?


As of now, the simplest solution working out of the box is for me:



> ```
> npm install js-girder-client
> node .\node_modules\js-girder-client\bin\girder-cli.js --command login my-login my-password
> ...
> 
> ```


I would love some getting started examples…


Thanks,  

Julien.  

p.s. please bear with my poor JS skills ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:")


---

## Jonathan_Beezley on 2019-03-11T13:56:41.647Z

Your confusion is understandable. The answer really depends on what you are looking to do with a javascript client. Here is a gentle introduction to what some of these packages are.


* girder: This is the main Girder application source code for pre 3\.0\. Modern Girder includes all the JS sources inside the python package. This isn’t really designed to be used in downstream projects.
* @girder/core: This contains the components that make up the Girder application without the build system and testing infrastructure. If you are just looking to include a widget from Girder into a custom application this would be what you want.
* @girder/components: This is a Vue replacement of the old Backbone client. You could understand it as the new version of @girder/core. It is being developed parallel to Girder 3, but will work just fine with a Girder 2\.x server.


I don’t know much about the other packages.


If what you are looking for is just a way to interact with a Girder API server through javascript, you can try looking at [rest.js](https://github.com/girder/girder_web_components/blob/master/src/rest.js) from girder\_web\_components. It is just a thin authentication layer on top of [axios](https://github.com/axios/axios). With an axios instance, you can perform REST requests directly to Girder’s API just as you would with python’s “requests” library.


Hopefully this helps. Maybe if you can describe what you want to do, we can offer some more targeted advice.


---

## finetjul on 2019-03-12T07:59:50.444Z

Thanks Jonathan for your answer.


Is girder\_web\_components the same as @girder/components ? (\-\> there is no github link on [npmjs.com](http://npmjs.com) ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=6 ":slight_smile:") )


If so, I can’t use rest.js because it requires a “window”, but my nodejs server does not have one.


My goal: I have a paraviewweb JS server that instantiates a paraview instance when a client requires a session. My js server also needs to interrogate and pull data from girder in order for them to be processed by paraview and the result sent to the client.


Thanks,  

Julien.


---

## Jonathan_Beezley on 2019-03-12T13:02:47.047Z




![](https://discourse.girder.org/user_avatar/discourse.girder.org/finetjul/48/19_2.png) finetjul:

> Is girder\_web\_components the same as @girder/components ? (\-\> there is no github link on [npmjs.com](http://npmjs.com) ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=12 ":slight_smile:") )



Yes, I’ll make a PR to add the link.





![](https://discourse.girder.org/user_avatar/discourse.girder.org/finetjul/48/19_2.png) finetjul:

> My goal: I have a paraviewweb JS server that instantiates a paraview instance when a client requires a session. My js server also needs to interrogate and pull data from girder in order for them to be processed by paraview and the result sent to the client.



I see. Nothing in the `@girder` org was design for server side use.


If js\-girder\-client does what you need, it should be safe for you to use. Only some very trivial changes were made to the REST API going from Girder 2 to 3, so there should not be any problems there.


What I would probably do is just instantiate an Axios instance myself like this:



```
const axios = require('axios');
const rest = axios.create({baseURL: 'https://data.kitware.com/api/v1'});

```

You can then hit any [REST endpoint](https://data.kitware.com/api/v1) using this object. For example,



```
rest.get(
  'folder', {params: {parentId: '579787098d777f1268277a27', parentType: 'collection'}}
).then((resp) => console.log(resp.data))

```

---

## finetjul on 2019-03-15T09:49:17.049Z

Thanks Jonathan


---

