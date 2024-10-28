# Status of plugins bundled in Girder Docker Image

## John_Roberts on 2019-08-26T23:47:30.562Z

What is the status of the plugins shipped with the Docker girder image?


Should they work out of the box? I managed to launch a working container with a mongo back end using the latest image ( commit source e97b1f7\). However, none of the bundled plugins are visible within the Admin Console Plugin page.


There are some complicating factors:


* I am running behind a proxy, so maybe I haven’t got all the settings right there? By that I mean the tools.proxy.base, api\_root, static\_public\_path, etc settings needed to account for the proxy.
* I changed the owner:group of the installation within the image and run as the same non\-root user. So, perhaps there are root vs non\-root issues here?


I am assuming that if the plugin source is bundled in the plugins directory, then they are compatible with the bundled girder version. Would that be a bad assumption? Is it possible the docker images are created with incompatible bundled plugins that haven’t caught up yet to the bundled girder version?


John.


---

