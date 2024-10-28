# HistomicsTK official release

## sfarag on 2020-01-27T16:44:21.820Z

Hi,


I just want to know, if there would be an official release for HistomicsTK anytime soon?


Best,


Sherif


---

## David_Manthey on 2020-01-27T17:11:25.279Z

We don’t issue official releases of HistomicsTK very often. The master branch gets updates which are tested and should always work. The default deployment (using the deploy\_docker.py script) installs the latest master.


HistomicsTK has two main components: the image processing tool kit and the user interface. There is an update of the user interface that switches to Girder 3 that is currently a PR (see <https://github.com/DigitalSlideArchive/digital_slide_archive/pull/41>). As part of this, the UI code is moving to the HistomicsUI repo. We expect this to be released in the next week or three.


* David

---

## David_Manthey on 2020-01-27T17:29:35.000Z

I should also add that the HistomicsTK image processing docker module gets published whenever we merge to master.


---

## sfarag on 2020-01-27T17:47:00.818Z

Thank you David, that was indeed helpful!


---

