# Guidance migrating from flask/eve

## Shotgunosine on 2017-10-03T18:47:21.657Z

I’ve been developing the backend for a website to crowdsource manual image segmentation tasks with a focus on neuroscience, [meduilna.org](http://medulina.org), code at [github.com/medulina/mindref](https://github.com/medulina/mindref). The backend is currently written in flask/eve, but Satra Ghosh has suggested that we use girder instead of flask/eve so that we don’t have to reimplement things like user and file management that girder already does well.


I’m trying to understand what I need to do to get the various endpoints I’ve already got in flask/eve running on girder. For example, I’ve got an image endpoint for jpegs representing individual images for segmentation (see image\_schema below). I’ve got validation defined for each of the fields in that endpoint. I’ve also got some logic that fires when a GET request comes in for an image that selects which image to show to the user based on their performance and the images they’ve seen before (see get\_seen\_images and pre\_image\_get\_callback functions below).


I just want to double check that I understand what needs to be done to create an image plugin recreating this endpoints behavior:


For validation, I create an image model where i expose each of those fields, and then write a validation method that goes through and checks all of the fields and returns a ValidationError if any of the required fields are missing or any of the fields have the wrong type, length, etc… Is there anything else I need to have in the model?


Then I need to create an image resource that handles the custom GET behavior for images and wraps all of the other default resource routes so that I can write documentation more specific than what’s in [girder/api/v1/resource.py](https://github.com/girder/girder/blob/master/girder/api/v1/resource.py), as well as handling different levels of access.


Is there anything else I need to take care of, or does that pretty much sum up what’s required?



```
image_schema = {
    'slice_direction': {
        'type': 'string',
        'allowed': ['ax', 'cor', 'sag']
    },
    'mode': {
        'type': 'string',
        'allowed': ['test', 'train'],
        'required': True
    },
    'task': {
        'type': 'string',
        'minlength': 1,
        'maxlength': 50,
        'required': True
    },
    'subject': {
        'type': 'string',
        'minlength': 1,
        'maxlength': 50,
    },
    'session': {
        'type': 'string',
        'minlength': 1,
        'maxlength': 50,
    },
    'slice': {
        'type': 'integer',
        'min': -10000,
        'max': 10000,
    },
    'pic': {
        'type': 'media',
    },
    'image_hash': {
        'type': 'string',
        'unique': True
    },
    'context': {
        'type': 'media'
    },
    'shape': {
        'type': 'list',
        'schema': {
            'type': 'float',
        }
    }
}



def get_seen_images(user_id, mode, task):
    masks = app.data.driver.db['mask']
    pipeline = [{'$match': {'user_id': ObjectId(user_id),
                            'mode': mode,
                            'task': task}},
                {'$group': {'_id': '$image_id', 'count': {'$sum': 1}}}]
    seen_images = pd.DataFrame([r for r in masks.aggregate(pipeline)], columns=['_id', 'count'])
    seen_ids = list(seen_images['_id'].values)
    return seen_images, seen_ids



def pre_image_get_callback(request, lookup):
    """Decide if the user will get a train or test image
    if train, decide if user will get a repeated image,
    if not repeated, try to give the user a novel training image"""

    try:
        user_id = request.args['user_id']
        token = request.args['token']
        try:
            task = re.findall(app.config['TASK_RE'], request.args['where'])[0]
        except IndexError as e:
            raise type(e)(str(e)+request.args['where'])
    except KeyError:
        # raise type(e)(str(e)+request.args['where'])
        return None

    users = app.data.driver.db['user']
    images = app.data.driver.db['image']
    #a = users.find_one({'_id': ObjectId(user_id), 'token': token})
    seen_test_images, seen_test_ids = get_seen_images(user_id, 'test', task)

    task_test_images = images.find({'task': task, 'mode': 'test'})
    task_test_images = [r for r in task_test_images]

    scores = app.data.driver.db['score']
    ups = scores.find_one({'user_project_id': str(user_id)+'__'+task})
    if ups is None:
        ups = {}
        ups['user_project_id'] = str(user_id)+'__'+task
        ups['user'] = user_id
        ups['task'] = task
        ups['n_subs'] = 0
        ups['n_try'] = 0
        ups['n_test'] = 0
        ups['total_score'] = 0
        ups['ave_score'] = 0
        ups['roll_scores'] = []
        ups['roll_ave_score'] = 0
        scores.insert_one(ups)

    train_roll = randint(1, test_per_train+1)

    # Decide if user will get a train or test image
    if (ups['roll_ave_score'] >= test_thresh) & (train_roll < test_per_train) & (len(task_test_images) > 0):

        # Getting a novel test image if possible
        mode = 'test'
        imode = 'test'

        unseen_images = images.find({'_id': {'$nin': seen_test_ids},
                                     'mode': imode,
                                     'task': task},
                                    {'_id': 1})
        unseen_images = [r['_id'] for r in unseen_images]

        if len(unseen_images) > 0:
            lookup['_id'] = ObjectId(choice(unseen_images, 1)[0])
            lookup['mode'] = imode
            if images.find_one({'_id':lookup['_id'], 'mode':lookup['mode']}) is None:
                raise Exception("Image id %s not found. Image ID was looked up from the unseen test images"%lookup['_id'])
            
        else:
            least_seen = list(seen_test_images.loc[seen_test_images['count'] == seen_test_images['count'].min(), '_id'].values)
            lookup['_id'] = ObjectId(choice(least_seen, 1)[0])
            lookup['mode'] = imode
            if images.find_one({'_id':lookup['_id'], 'mode':lookup['mode']}) is None:
                raise Exception("Image id %s not found. Image ID was looked up from the least seen test images"%lookup['_id'])
            

    elif randint(1, train_repeat+1) == train_repeat:
        # Getting a repeated training image
        mode = 'try'
        imode = 'train'
        seen_images, seen_ids = get_seen_images(user_id, mode, task)

        if len(seen_ids) > 0:
            lookup['_id'] = ObjectId(choice(seen_ids, 1)[0])
            lookup['mode'] = imode
            if images.find_one({'_id':lookup['_id'], 'mode':lookup['mode']}) is None:
                raise Exception("I have a mask for this image, but I can't find the image anymore. Repeat.")
        else:
            lookup['mode'] = imode

    else:
        # Getting a novel training image if possible
        # If not, get a training image they've seen the fewest number of times
        # Find the images a user has seen
        mode = 'try'
        imode = 'train'
        seen_images, seen_ids = get_seen_images(user_id, mode, task)
        unseen_images = images.find({'_id': {'$nin': seen_ids},
                                     'mode': imode,
                                     'task': task},
                                    {'_id': 1})
        unseen_images = [r['_id'] for r in unseen_images]
        if len(unseen_images) > 0:
            lookup['_id'] = choice(unseen_images, 1)[0]
            lookup['mode'] = imode
            if images.find_one({'_id':lookup['_id'], 'mode':lookup['mode']}) is None:
                raise Exception("Image id %s not found. Image ID was looked up from the unseen training images"%lookup['_id'])
        elif len(seen_ids) == 0:
            raise Exception("Seen Ids and Unseen Ids are both empty. FML.")
        else:
            least_seen = list(seen_images.loc[seen_images['count'] == seen_images['count'].min(), '_id'].values)
            lookup['_id'] = choice(least_seen, 1)[0]
            lookup['mode'] = imode
            if images.find_one({'_id':lookup['_id'], 'mode':lookup['mode']}) is None:
                raise Exception("I have a mask for this image, but I can't find the image anymore. Least Seen")

```

---

## brianhelba on 2017-10-05T05:50:27.933Z

Thanks for bringing this up. Eve provides an interesting mix of libraries and ideas, so a port like this could provide some nice insight.


I’ll have more time to look into your code tomorrow, but at first glance, I think you’re definitely on the right track. Note:


* It may be possible to not give up Cerberus entirely. The model `validate` method only cares if a `ValidationError` is raised, but don’t care how, so I wonder if it’s possible to hook up Cerberus for this task. Alternatively, you could use JSONSchema, which Girder has better support for. Explicit logic to check fields will hopefully be only a last resort.
* It’d seem logical to add a new `Resource` for your image endpoints, but I’m not sure exactly what you mean by “wrapping the other resource routes”. If you mean adding POST, PUT, DELETE, etc. routes, then that makes sense, as the `/api/v1/resource` endpoints are most useful for utility purposes. Having native routes for your custom model type is a good bet.


If you start to write any Girder code, feel free to link to it.


---

## Shotgunosine on 2017-10-09T20:52:30.832Z

Thanks for your advice Brian. I was able to use cerberus for validation without too much trouble. After chatting with some of the other folks on the project, I think we are going to stick with eve for now, but we’ll keep an eye on Girder and may come back to it in the future.


---

