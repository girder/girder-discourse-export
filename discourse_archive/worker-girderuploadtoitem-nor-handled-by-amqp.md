# Worker: GirderUploadToItem nor handled by amqp

## Edern_Haumont on 2019-06-27T14:06:20.549Z

Hi everyone,


I have issues trying to use an upload transform for my girder worker task.  

I do not understand the internals of this but apparently the hook is wrongly “registered” in amqp during the task publication.


I tried to downgrade the celery version just in case but without new results.


Does anyone have pointers or had recently similar problems ?


Thanks.


Guilty code:



```
# file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/helpers/anonymization.py
async_result = anonymization_task.delay(
    inputDirectoryPath=GirderItemId,
    patientIdentifier=patient['identifier'],
    girder_result_hooks=[
        GirderUploadToItem(
            str(newItem['_id']),
            delete_file=True,
            upload_kwargs={
                'user': user
            }
        ),
    ]
)

```

Error stack:



```
{
    "message": "FrameSyntaxError: FrameSyntaxError(None, \"    Table type <class 'girder_worker_utils.transforms.girder_io.GirderUploadToItem'> not handled by amqp. [value: <girder_worker_utils.transforms.girder_io.GirderUploadToItem object at 0x7f8548514a58>]\\n\", None, '')",
    "trace": [
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/girder/api/rest.py, line 628 in endpointDecorator>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/girder/api/rest.py, line 1188 in GET>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/girder/api/rest.py, line 944 in handleRoute>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/girder/api/describe.py, line 677 in wrapped>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/api/download.py, line 185 in getDownload>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/decorators/access.py, line 66 in wrapped>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/decorators/access.py, line 194 in wrapped>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/api/download.py, line 302 in download>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ImagePlatform/ViewerPlugin/ofsep_viewer/server/helpers/anonymization.py, line 51 in anonymizeItem>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/celery/app/task.py, line 427 in delay>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/girder_worker/task.py, line 111 in apply_async>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/celery/app/task.py, line 570 in apply_async>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/celery/app/base.py, line 756 in send_task>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/celery/app/amqp.py, line 552 in send_task_message>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/kombu/messaging.py, line 181 in publish>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/kombu/connection.py, line 510 in _ensured>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/kombu/messaging.py, line 203 in _publish>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/channel.py, line 1771 in _basic_publish>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/abstract_channel.py, line 51 in send_method>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/method_framing.py, line 111 in write_frame>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/serialization.py, line 554 in _serialize_properties>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/serialization.py, line 317 in dumps>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/serialization.py, line 337 in _write_table>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/serialization.py, line 395 in _write_item>",
        "<FrameSummary file /home/edern/Projects/Girder3-OFSEP/ofsep_env/lib/python3.6/site-packages/amqp/serialization.py, line 354 in _write_array>"
    ],
    "type": "internal",
    "uid": "b790b609-a903-43a8-a932-6781bdef826d"
}

```

---

## Zach_Mullen on 2019-06-27T14:42:12.460Z

Is it possible that your version of `girder_worker_utils` is not the same on your worker as it is on the Girder server side?


---

## Edern_Haumont on 2019-06-28T07:23:33.926Z

Hello !  

I use one virtualenv for each side. I just checked and both virtualenvs use the same version of `girder_worker_utils`, that is to say the last master \+ two new transforms (you can see I added `GirderItemId()` for instance)



---

## Edern_Haumont on 2019-07-03T07:13:14.566Z

Has someone some other ideas ? I can send other details if needed.


---

## danlamanna on 2019-07-03T18:33:16.039Z

Could you post more of your internal code? At least the definition of `GirderItemId` and the function signature of `anonymization_task`?


I’m willing to bet if you remove `girder_result_hooks` the error still occurs, since it looks like Celery is having a problem serializing your function call before putting it on a queue to be worked on.


---

## Edern_Haumont on 2019-07-04T12:38:55.184Z

Hello [@danlamanna](/u/danlamanna), thanks for your answer.


I could solve the problem on my side: As you can see, there is a mistake in the code I first sent: I forgot to give an argument to my transform `GirderItemId` !


It took me quite a long time to find it because the error messages did not help me a lot ![:smiley:](https://discourse.girder.org/images/emoji/twitter/smiley.png?v=9 ":smiley:")


Anyways, problem solved ! I will push the GirderItemId to the girder\_worker\_utils repo sometime.


---

