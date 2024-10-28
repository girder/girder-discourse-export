# Accesing Job object from within girder-worker task

## xarthisius on 2018-09-26T17:19:22.917Z

Hi!  

Is there a way to access a Job associated with a girder\-worker task from within that task? Specifically, I’d like to update Job’s progress message.  

Cheers,  

Kacper


---

## danlamanna on 2018-09-26T18:03:52.673Z

I think if your task is a [bound task](http://docs.celeryproject.org/en/latest/userguide/tasks.html#bound-tasks), then it should have access to a couple of things:


1. An instance of Girder Client, using the `girder_client` attribute of your bound task (`self.girder_client`). This may or may not be authenticated already.
2. An instance of a [JobManager](https://github.com/girder/girder_worker/blob/c17b1fa0fc352366c089a5a76a6a653c77acfcf9/girder_worker/utils.py#L172), which has methods for updating progress (`self.job_manager`).


I’m a bit fuzzy on exactly what qualifies tasks for receiving both of those things, they’re attached in the Celery task\-prerun signal [here](https://github.com/girder/girder_worker/blob/c17b1fa0fc352366c089a5a76a6a653c77acfcf9/girder_worker/app.py#L112). [@Chris\_Kotfila](/u/chris_kotfila) knows a bit more about the details.


---

## Zach_Mullen on 2018-09-26T18:26:08.315Z

A quick code example of what [@danlamanna](/u/danlamanna) was referring to:



```
from girder_worker.app import app

@app.task(bind=True)
def my_task(self, *args, **kwargs):
    self.job_manager.updateProgress(message='starting', total=100, current=0)
    # do stuff

```

---

## xarthisius on 2018-09-26T20:46:56.852Z

Thanks a lot!


---

