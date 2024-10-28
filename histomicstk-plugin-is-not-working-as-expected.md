# histomicstk plugin is not working as expected

## rukamesh.kumar on 2018-08-17T11:42:49.607Z

Hi,  

I have installed histomicstk successfully with dependency large\_image, slicer\_cli\_web as girder plugin but not able to see full functionality on UI. I am able to do zoom in and zoom out but not able see analyser part on UI


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-17T12:03:08.000Z

The analysis menu only shows up when you are logged in as a user AND and at least one analysis docker image has been installed. For instance, you can install an analysis CLI via the PUT /HistomicsTK/HistomicsTK/docker\_image API endpoint. If you pass “dsarchive/histomicstk:latest” (including the quotes) to the endpoint, it will install the latest set of analyses from the HistomicsTK project.


* David

---

## rukamesh.kumar on 2018-08-20T06:09:15.941Z

Hi,  

Now i have installed Histomicstk as plugin and i got analyser part on UI but when I select a algorithm for analysis and click on submit it creates a job but job mode is always shown as Queued. So could you please explain how can i run job successfully after submission for image analysis?


If i have to expose some more rest api then how can i do this using existing histomicstk plugin?


Thanks  

Rukamesh kumar


---

## David_Manthey on 2018-08-20T12:05:39.911Z

Did you set up Girder Worker? The analysis jobs are run through Girder Worker, which is a distributed task runner (it doesn’t have to be on the same computer as Girder). If you go to the Girder job’s page, there is a “Task Information” button that can show if Girder is communicating with Girder Worker. If it is isn’t, the jobs will remain queued.


* David

---

## rukamesh.kumar on 2018-08-20T12:58:54.740Z

Hi,  

Thanks for reply I have one more question


If i have to add some more feature in existing histomicstk plugin and i want to access that feature from analyser UI then how can i do this?


Thanks  

Rukamesh kumar


---

## Jonathan_Beezley on 2018-08-20T13:33:43.397Z

What kind of feature are you wanting to add?


---

## rukamesh.kumar on 2018-08-20T13:39:58.007Z

Like if want to convert a image into grayscale and i want to access this from analyser UI so how can do this? Just to understand the flow of histomicstk plugin


---

## rukamesh.kumar on 2018-08-20T14:03:26.876Z

Hi,  

After Girder Worker setup i am getting below mentioned error:


\[2018\-08\-20 19:15:50,448: ERROR/ForkPoolWorker\-2] Task girder\_worker.run\[74850af5\-fd9e\-458e\-a2b7\-c8ec7fd67c1a] raised unexpected: Exception(u’Invalid mode: docker’,)  

Traceback (most recent call last):  

File “/usr/local/lib/python2\.7/dist\-packages/celery/app/trace.py”, line 375, in trace\_task  

R \= retval \= fun(*args, \*\*kwargs)  

File “girder\_worker/girder\_worker/task.py”, line 148, in **call**  

results \= super(Task, self).**call**(*\_t\_args, \*\*\_t\_kwargs)  

File “/usr/local/lib/python2\.7/dist\-packages/celery/app/trace.py”, line 632, in **protected\_call**  

return self.run(\*args, \*\*kwargs)  

File “girder\_worker/girder\_worker/tasks.py”, line 19, in run  

return core.run(\*pargs, \*\*kwargs)  

File “girder\_worker/girder\_worker/core/utils.py”, line 123, in wrapped  

return fn(\*args, \*\*kwargs)  

File “girder\_worker/girder\_worker/core/**init**.py”, line 132, in run  

raise Exception(‘Invalid mode: %s’ % mode)  

Exception: Invalid mode: docker


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-20T14:34:21.644Z

When you installed Girder Worker, did you enable any plugins? You probably need to run `girder-worker-config set girder_worker plugins_enabled 'girder_io,docker'` and then restart Girder Worker.


– David


---

## rukamesh.kumar on 2018-08-20T14:52:36.129Z

Hi,  

Could you please explain how to add some more feature in existing histomicstk plugin so that i can access that feature from analyser UI?


Thanks  

Rukamesh Kumar


---

## rukamesh.kumar on 2018-08-20T15:35:05.942Z

Hi,  

Could you please give me some suggestion on this?


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-20T16:55:43.006Z

One way to add new analyses is to make a docker that exposes a slicer\-cli style interface. The docker file in the root of the HistomicsTK repository does this. You’ll see that it after installing all prerequisites, it calls a bash script which passes parameters to cli\_list\_entrypoint.py, which either lists available analyses or runs a particular algorithm. There are a number of analyses in the HistomicsTK repository; I recommend looking at them, picking the one that most closely matches what you want to do, and see if you can alter it to perform your new task.


– David


---

## rukamesh.kumar on 2018-08-21T05:41:05.769Z

Hi,  

Is there any way to run without docker locally?


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-21T12:08:15.183Z

Yes. For the whole HistomicsTK and Girder deployment, see the ansible/deploy\_local.sh script for how it can be installed locally. For developing analysis CLIs, there are some examples (in the docs/examples directory) showing use of them via Jupyter notebooks – those are running locally.


---

## rukamesh.kumar on 2018-08-21T16:26:29.487Z

Hi,  

How can i get girder id of a input file to call rest API?


---

## David_Manthey on 2018-08-21T19:46:00.851Z

If you are browsing in the Girder web client, when you examine an item, there is a list of files at the bottom of the  

item page. The file name has a link to download the file that includes an ID.


From the Web API, you can get a list of file ids associated with a Girder item. You can also look up the file ID via the GET resource/lookup endpoint.


---

## rukamesh.kumar on 2018-08-23T13:14:51.610Z

Hi,  

I have installed histomicstk as plugin on remote machine but i am getting two issues:


1. girder\-worker is not working i am getting connection refused while connecting at amqp://guest:\*\*@localhost:5672//
2. I am getting all rest API but not Analyser on UI after calling Docker image put rest API


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-23T13:24:00.000Z

Is Girder Worker on the same machine as Girder? Which machine is RabbitMQ running on? The broker address can be set separately on Girder Worker and Girder so that they both refer to the location of RabbitMQ (in your example it is trying to go to localhost, but can’t find RabbitMQ there).


Is this your own API or the sample one from HistomicsTK? When you say you are getting all rest API, are you seeing the endpoints like POST /HistomicsTK/dsarchive\_histomicstk\_latest/NucleiDetection/run ?


---

## rukamesh.kumar on 2018-08-23T13:29:43.939Z

Hi,  

This is sample one from HistomicsTK only and i am seeing the endpoints like POST /HistomicsTK/dsarchive\_histomicstk\_latest/NucleiDetection/run.


Thanks  

Rukamesh Kumar


---

## David_Manthey on 2018-08-23T13:41:06.000Z

Are you logged in? Does your user have access to analyses? The users that can access analyses are settable in the HistomicsTK plugin settings, and are never exposed to anonymous access. Otherwise, are there any javascript console errors?


---

