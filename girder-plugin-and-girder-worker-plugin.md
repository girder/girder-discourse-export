# Girder plugin and Girder Worker plugin

## finetjul on 2019-01-13T14:22:44.507Z

Hi,


What really happens when I activate a plugin from the Admin Console ? Is it being pip installed ?  

If I do not fork Girder but I pip install Girder, how Girder will find my plugins ?


Can a Girder Worker plugin be a Girder plugin ? or are those totally separated ?  

If I modify a Girder Worker, should I pip upgrade it ? anytime or only if I add a new function ?


How can I see the web UI of the Remote Worker plugin ?


I’m using Girder 2\.5, does Girder \>\=3 handles Girder Workers differently ?


Thanks for your help,  

Julien.


---

## Zach_Mullen on 2019-01-13T19:47:02.338Z

Hi Julien,


No, it’s not being pip installed, it’s just marking it as enabled, meaning the next time you start the server its `load` method will be invoked.


Girder 3 does change the way plugins are installed, i.e. as pip packages rather than directories, however the process of activating them via the admin console and restarting is still roughly the same.


Girder worker underwent a refactor of its own fairly recently, it might be good to touch base about what you’re trying to do with the worker to see what version of everything makes the most sense to use.


Thanks,


\-Zach


---

