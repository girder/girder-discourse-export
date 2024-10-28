# Multiple girder instances

## finetjul on 2019-09-06T05:14:49.122Z

Hi,


I have 2 Girders running on the same machine (sharing the same database but having different configuration and behavior).  

Girder workers can be triggered by any Girder instance. I can not have a unique celery queue because I get 401 HTTP errors (authentification on Girder `#2 is different from Girder`\#1) How can I have 2 instances of celery running and making sure they consume their own “jobs” ? Should I create 2 queues, if so, how ?


Thanks,  

Julien.


---

## Jonathan_Beezley on 2019-09-06T12:26:31.816Z

I don’t think what you want to do is supported “out of the box”. It will likely involve writing a custom plugin to solve the problem.


I can think of a few solutions:


* Inject some configuration to the celery app to [route tasks](https://docs.celeryproject.org/en/latest/userguide/routing.html) to different queues per instance.
* A custom girder plugin that overrides the [worker.api\_url](https://github.com/girder/girder_worker/blob/28e63de5d5a4f3272f5c18fcc7577ba6e35ed486/girder_worker/girder_plugin/utils.py#L50) setting per instance so they worker’s communicate back with the correct instance.
* A custom girder plugin that overrides the [broker setting](https://github.com/girder/girder_worker/blob/master/girder_worker/girder_plugin/constants.py#L29) per instance and maintain two independent brokers.

---

