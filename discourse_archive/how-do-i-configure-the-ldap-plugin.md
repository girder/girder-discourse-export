# How do I configure the ldap plugin?

## anteveoneer on 2019-05-28T14:07:40.975Z

Hi I have installed the ldap plugin, and when I go to the admin console plugin page I can see it there, but how do I configure it?


The docs mentions “on the plugin configuration page” but I have no such thing. As I deploy girder in a container, it would be nice to just copy in a config file and restart the container.


---

## danlamanna on 2019-07-03T19:00:18.232Z

The plugin configuration page is the cog wheel located next to the LDAP plugin on the plugins page. If you don’t see a cog wheel you likely need to re\-run `girder build` (or `girder-install web` for Girder 2\).


---

