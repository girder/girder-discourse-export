# Use Plugins in the App

## vickyapril02 on 2021-07-23T15:33:21.496Z

Hi ,  

We are brand new to Girder data management .  

We just followed the Girder documentation and deployed the app using Docker (was working fine).  

But when we go to Plugins page its empty, but in the documentation its mentioned that you can view the plugins ,  

We also executed into the girder docker container and tried to install manually via pip.  

Then it shows up in the webpage but we are facing challenge how to use it because when we click the plugin it redirects us to the documentation page.  

[![Screenshot from 2021-07-23 17-18-37](https://discourse.girder.org/uploads/default/optimized/1X/191983fe54b067139d39bf69e4c59ed039c7c84f_2_690x241.png)
Screenshot from 2021\-07\-23 17\-18\-371545×541 20\.8 KB](https://discourse.girder.org/uploads/default/original/1X/191983fe54b067139d39bf69e4c59ed039c7c84f.png "Screenshot from 2021-07-23 17-18-37")


Any leads would be a great help for our team


Thanks and Regards  

Vicky


---

## David_Manthey on 2021-07-27T16:24:54.952Z

Hi Vicky,


Generally, to add plugins to Girder, you pip install the plugin, then run `girder build` to build the web client for that plugin, then restart the girder server. You may have to reload the website to see the changes.


I suspect you didn’t run `girder build`, which is why you aren’t seeing the plugins.


– David


---

## vickyapril02 on 2021-07-28T07:41:42.624Z

Hi David,


Thanks for the info, You are right we are not aware of girder build.  

Yes after girder build its working fine .


Regards  

Vicky


---

