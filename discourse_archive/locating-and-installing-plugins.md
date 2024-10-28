# Locating and installing plugins

## rbaustin on 2019-12-09T18:44:06.285Z

I’m migrating from Girder 2 to 3, and found this guide: [https://girder.readthedocs.io/en/stable/migration\-guide.html](https://girder.readthedocs.io/en/stable/migration-guide.html)


The issue I’m running into is around finding, installing, and configuring plugins. In V2, I think there were several plugins available by default. It looks like in V3, we have to manually find and import the plugins with `pip`.


The Girder docs has a plugins page that lists some of the plugins: <https://girder.readthedocs.io/en/stable/plugins.html>. But many of the plugins we have installed in our V2 instance are not mentioned on that page. And the ones that are mentioned do not have instructions for how to install them.


I was able to install the homepage plugin by guessing the command `pip install girder-homepage` and it worked. I was not able to do the same for other plugins. I also noticed that in V2, there was a toggle switch to enable/disable the plugin, and a gear icon to configure the plugin. In V3, I see the Homepage plugin in the list, but there is not toggle (which kind of makes sense) and no configuration option.


I feel like I’m missing some key information around plugins that I’m not finding in the docs.


---

## brianhelba on 2019-12-09T19:04:59.707Z

Hi [@rbaustin](/u/rbaustin),


You may not be seeing a configuration option for the Homepage plugin if you never rebuilt Girder’s front\-end (with `girder build`) after installing the new plugin. Does that fix your issue?


I think you also bring up a very good point: the Girder documentation should provide a comprehensive list of the “upstream” plugins which can be installed. I agree that the [Plugins page of the docs](https://girder.readthedocs.io/en/stable/plugins.html) is the best place for this, but that page needs to be improved to at least list the Python package names of all those plugins.


You mentioned that some of the plugins from your V2 instance were not mentioned on the [Plugins page of the docs](https://girder.readthedocs.io/en/stable/plugins.html); which ones specifically are missing?


---

## Jonathan_Beezley on 2019-12-09T19:10:21.673Z

I think what you are looking for is at the end of the migration guide  

[https://girder.readthedocs.io/en/stable/migration\-guide.html\#removed\-or\-moved\-plugins](https://girder.readthedocs.io/en/stable/migration-guide.html#removed-or-moved-plugins)


Some of the plugins were moved to external repositories, but were migrated and should still work. Others were abandoned and never converted to Girder 3 due to a lack of interest or funding.


---

## rbaustin on 2019-12-09T19:26:11.779Z

[@brianhelba](/u/brianhelba) Ok, wow, thanks. That helps a lot. I did rebuild the site and it looks like the plugin configuration options are there now.


[@Jonathan\_Beezley](/u/jonathan_beezley) your comment was also very helpful. I tried installing the “Authorized Uploads” plugin with `pip install girder-authorized-uploads`, `pip install girder-authorized_uploads`, `pip install girder-authorizeduploads`, and `pip install girder-authorizedUploads` but none of them worked. My guess is that maybe it got deleted? But then why would it be the first plugin in the list, I’m not sure.


I gave up after not being able to find that plugin and feeling like I wasn’t installing the homepage one correclty. Now I’ve tested adding a few of the others, and I am able to find some, but not all. The previous instance looks like it had ALL the plugins, but I’m not sure we were using all of them.


The main one I need to figure out is HistomicsTK. I found the guide for installing it as a Girder plugin here [https://digitalslidearchive.github.io/HistomicsTK/installation.html\#installing\-histomicstk\-as\-a\-server\-side\-girder\-plugin](https://digitalslidearchive.github.io/HistomicsTK/installation.html#installing-histomicstk-as-a-server-side-girder-plugin), and I got the dependencies installed. However, there isn’t any direction on how to actually install it and `pip install girder-histomicstk` did not do anything.


Thanks again for the help, both of you. It’s already helped me make some progress after several hours of head banging.


---

## David_Manthey on 2019-12-09T19:39:41.345Z

The authorized upload plugin is called `girder-authorized-upload` (you can search pypi for girder plugins).


HistomicsTK hasn’t *yet* been converted to Girder 3\. It is actively being converted, though, and *might* be available in a Girder 3 form sometime this week. We’ve had some naming indecision between HistomicsTK and the Digital Slide Archive. Once the Girder 3 version is published, HistomicsTK will be the analysis toolkit part of it (which doesn’t depend on Girder), and Digital Slide Archive will be the Girder plugin and UI. Therefore, this will probably be published as digital\-slide\-archive when it is available.


---

## rbaustin on 2019-12-09T22:01:03.968Z

Got it. I guess I tried every different naming convention for the Authorized Uploads plugin except removing the `s` from the end.


Regarding the HistomicsTK plugin, that makes sense. I’ll keep an eye out for it.


Thanks for the help everyone. This was very useful.


---

## rbaustin on 2020-01-03T17:29:57.009Z

[@David\_Manthey](/u/david_manthey)  

Hope you had a great holiday and are enjoying the New Year so far. I wanted to check in here on the HistomicsTK/DSA plugin. The last update I saw was that it was maybe going to be out the first or second week of December, but I know how the holidays can be. Is there any news?


---

## David_Manthey on 2020-01-03T17:51:15.323Z

There is a version of HistomicsTK has been updated to Girder 3\. There is a new repository at <https://github.com/DigitalSlideArchive/HistomicsUI>. It works locally and passes CI. As a caveat, I don’t think anyone has deployed it yet besides myself.


The deployment scripts have not yet been converted to Girder 3 (they are actively being worked on).


– David


---

## rbaustin on 2020-01-03T18:30:27.208Z

Alright. Thanks for the update. Just tested it out running `pip install histomicsui --find-links https://girder.github.io/large_image_wheels` and got the message:



```
ERROR: Could not find a version that satisfies the requirement histomicsui (from versions: none)
ERROR: No matching distribution found for histomicsui

```

Which I’m assuming has to do with the missing deployment scripts (sorry if that is a naive idea).


But this looks like progress from the last time I tried installing it on Girder 3 ![:slight_smile:](https://discourse.girder.org/images/emoji/twitter/slight_smile.png?v=9 ":slight_smile:")


---

