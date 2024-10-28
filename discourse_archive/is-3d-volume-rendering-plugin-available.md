# Is 3D volume rendering plugin available?

## Jingdan on 2018-04-07T04:11:42.626Z

Hi,I am a new user of Girder and I am impressed by the Girder’s simplicity and flexibility.


I need a plugin for visualizing 3D volume together with mesh using javascript \+ webGL. Is such plugin already available? Thanks.


---

## Zach_Mullen on 2018-04-09T12:41:43.365Z

Hi Jingdan,


We don’t have an out\-of\-the\-box plugin that can do everything you mentioned, but we have built a variety of custom plugins for Girder’s front end for both volume rendering and mesh visualization, including visualizing both data types in the same scene.


The tool we use to create these custom plugins is also one that we develop called vtk.js, its web site is here\[1]. There are some examples of integrating it with Girder if you are interested in pursuing that.


\[1] [https://kitware.github.io/vtk\-js/](https://kitware.github.io/vtk-js/)


Thanks,


\-Zach


---

## Zach_Mullen on 2018-04-16T19:12:00.577Z

For one example of a vtk.js \+ Girder integration, check out this page: [https://algorithms.kitware.com/\#item/5a71d5704f2ae56dd1629e01/pv\_glance](https://algorithms.kitware.com/#item/5a71d5704f2ae56dd1629e01/pv_glance)


That shows vtk.js performing volume rendering as part of a Girder plugin.


---

## Jingdan on 2018-04-17T01:22:29.332Z

Zach,


Thank you very much. This is a great example.


Best,  

Jingdan


---

## tudor33sud on 2018-10-11T07:38:15.654Z

Hi Zach! I’m also looking for an option to visualize data used in girder with your paraview project. Does the Girder plugin which allows opening girder data in Paraview opensource or publicly available somewhere?


One other thing that’s worth noting, if I do have a data platform of my own, is there any possiblity of loading that data to Paraview similar to how you load it from Girder? Any thoughts would be highly appreciated.


Thank you very much for your support.


Best Regards,  

Tudor


---

## Zach_Mullen on 2018-10-11T12:01:20.545Z

Hi Tudor,


If you’re interested in opening datasets in ParaView, it will require changes in ParaView itself to handle URL schemes. To my knowledge, that doesn’t exist yet, it’s just a concept we came up with that would be nice, and if they did it, it could be extended to support other URI schemes besides just Girder.


If that’s a feature you’d like to see from ParaView, the best thing to do would be to post on the ParaView Discourse site: <https://discourse.paraview.org/>


Thanks!


\-Zach


---

## tudor33sud on 2018-10-12T07:33:55.322Z

Thanks for your reply.


So, if I understand correctly, basically what you’re saying is that for girder integration, you needed both a girder plugin \+ branching some changes into the paraview frontend itself right?


I was thinking if this is straightforward possible with the paraview frontend, and just needing to write a plugin or to share data in some form with frontend ( presigned urls for example ).


Best,  

Tudor


---

