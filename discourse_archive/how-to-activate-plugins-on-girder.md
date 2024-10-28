# How to activate plugins on girder

## sthlmj on 2019-11-11T10:18:54.547Z

Hi! I’m new to girder, using it in a docker container. I’ve read some documentations regarding plugins. There are a couple of plugins that are included within the girder/plugins directory like google analytics, gravatar, ldap, etc. When logged in within my local instance, I do not see these plugins on the plugins administrator page within the web browser. My question here is how do I activate these plugins and make them show up on the plugins configuration page on my browser? Girder version I’m using is the latest 3\.0\.5\.dev2\+ga3d359c89 and also tried 2\.5\.0\.


---

## David_Manthey on 2019-11-11T16:36:26.000Z

You need to pip install any girder plugins you want to use. If you are doing this from pip, then this will be something like `pip install girder-ldap` (or whichever plugins you want to have)… If you are doing this from the source in development mode, then `pip install -e plugins/ldap` (see the requirements\-dev.txt file for an example of this being done for testing).


* David

---

## sthlmj on 2019-11-12T08:57:58.463Z

I just found on the document that I also need to do `girder build` for it to turn up on the web gui.


---

