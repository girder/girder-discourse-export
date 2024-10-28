# Girder worker on remote machine

## Edern_Haumont on 2019-05-09T14:08:48.393Z

Hello,


I have no problem using girder workers on my machine (girder, rabbitmq and worker on localhost), both using normal jobs and docker tasks.  

I am now trying to run the worker on another machine. I could make it connect to the queue and receive tasks. However, in the girder side of the code, an exception occurs when trying to get the result :



```
{
  "message": "ConnectionError: ConnectionError('MaxRetryError(\"HTTPConnectionPool(host=\\'localhost\\', port=8080): Max retries exceeded with url: /api/v1/file/5cb589201bece03a628d6eab (Caused by NewConnectionError(\\'<urllib3.connection.HTTPConnection object at 0x7f8a4a5cc630>: Failed to establish a new connection: [Errno 111] Connection refused\\',))\",)')",
  "trace": [
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/girder/api/rest.py, line 633 in endpointDecorator>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/girder/api/rest.py, line 1195 in GET>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/girder/api/rest.py, line 952 in handleRoute>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/girder/api/describe.py, line 700 in wrapped>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/simple_plugin/__init__.py, line 27 in processFile>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/celery/result.py, line 226 in get>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/celery/backends/base.py, line 500 in wait_for_pending>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/celery/result.py, line 331 in maybe_throw>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/celery/result.py, line 324 in throw>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/vine/promises.py, line 244 in throw>",
    "<FrameSummary file /home/edern/Projects/MAIA/maia_env/lib/python3.7/site-packages/vine/five.py, line 195 in reraise>"
  ],
  "type": "internal",
  "uid": "fc0d2aae-4996-488b-a38b-e6b766bcdfdb"
}

```

Note the reference to localhost that I do not really understand.  

Here is the server code involved:



```
# imports...

@access.public(scope=TokenScope.DATA_READ)
@autoDescribeRoute(
    Description('Simple Job')
    .modelParam('id', model=File, level=AccessType.READ)
    .param('message', 'Message to make the worker print',
           default='Hello', enum=('Hello', 'Goodbye'))
)
def processFile(file, message):
    result = simple_task.delay(GirderFileId(str(file['_id'])), message)
    return result.get()



class SimplePlugin(GirderPlugin):
    DISPLAY_NAME = 'Simple plugin'

    def load(self, info):

        info['apiRoot'].file.route('GET', (':id', 'simple_job'), processFile)

```

The task:



```
# imports...

# TODO: Fill in the function with the correct argument signature
# and code that performs the task.
@girder_job(title='Example Task')
@app.task(bind=True)
def simple_task(self, inputFile, message):
    print(message)
    return 'I said {}'.format(message)

```

Any idea on the cause of this problem ?  

Thanks.


---

## David_Manthey on 2019-05-09T14:12:45.240Z

Unless told otherwise, Girder tells Girder Worker to connect to it the same way that whatever triggered the job connected to Girder. For instance, from your browser, you are connected to Girder on localhost:8080, so when you trigger a job, Girder tells Girder Worker to respond on localhost:8080\. If it is on the same machine, this works. But since it is on a different machine it doesn’t.


In the Remote Worker plugin settings, there is a setting to change this behavior: “Alternative Girder API URL”. Set this to however Girder Worker need to reference the Girder api (e.g/, “<http://girdermachine:8080/api/v1>”). Then, Girder will tell Girder Worker to use this instead of inspecting the request that triggered the job.


---

