# docker_run task - Permission error

## lucie.macron on 2019-10-11T16:57:22.704Z

Hi,


I’m trying to run a `docker_run` task and I have this permission error:


`[2019-10-11 17:59:58,135: ERROR/ForkPoolWorker-8] Task girder_worker.docker.tasks.docker_run[e6f0d055-f358-4f8b-b9a8-b3b2c812f8c8] raised unexpected: DockerException("Error while fetching server API version: ('Connection aborted.', PermissionError(13, 'Permission denied'))",)`


I tried to run celery as root, with admin rights … nothing works.


Has someone already experienced this type of error ?


---

## Zach_Mullen on 2019-10-11T17:24:29.146Z

Hi,


You need to run the girder\_worker process as a system user who is authorized to run docker containers. The way to do this is to add them to the `docker` system group.


\-Zach


---

## lucie.macron on 2019-10-31T18:23:02.910Z

Thanks [@Zach\_Mullen](/u/zach_mullen) it worked !


---

