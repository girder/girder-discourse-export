# Google OAuth not working

## oleks_korshak on 2019-10-29T17:59:45.387Z

After I set up the oauth plugin and get filled in the appropriate client id and client secret on the girder oauth plugins page, I get this error when trying to register an account using google. It looks like the plugin is trying to use a deprecated API from google.



```
\"error\": {
    \"code\": 403,
    \"message\": \"Legacy People API has not been used in project 149423623306 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/legacypeople.googleapis.com/overview?project=149423623306 then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry.\",
    \"status\": \"PERMISSION_DENIED\"
} 

```

I found that it is not possible for me to activate the Legacy People API for a project at this time according to google’s documentation:



```
Can I enable the Legacy People API for a new developer project?
No, the Legacy People API cannot be enabled for new developer projects. Use recommended alternatives such as Google Sign-in or Google People API.

```



![](https://discourse.girder.org/uploads/default/original/1X/e95f249bbbc14b840d6c8032e500c1f83496ef06.png)
[Google Developers](https://developers.google.com/people/legacy)


![](https://discourse.girder.org/uploads/default/original/1X/b8a60136526877b21331497a740714b9350d1e4b.png)
### [Legacy People API  \|  Google Developers](https://developers.google.com/people/legacy)


("Minimally implements the legacy Google\+ \`people.get\` and \`people.getOpenIdConnect\` " "APIs maintained for backward compatibility.")







Any advice on how I should proceed?


Thanks,  

Oleks


---

## brianhelba on 2019-11-14T23:56:51.190Z

Hi [@oleks\_korshak](/u/oleks_korshak)


Thanks for bringing this up. I’ve just fixed the underlying bug here: <https://github.com/girder/girder/pull/3147>


A new release of Girder’s OAuth plugin has also been made, which includes this fix: `v3.0.5`: [https://pypi.org/project/girder\-oauth/3\.0\.5/](https://pypi.org/project/girder-oauth/3.0.5/) If you install that version or later, it should resolve your issue.


If you continue to have any problems, please file a bug on Girder’s issue tracker: <https://github.com/girder/girder/issues> . It should get a more rapid response there.


---

