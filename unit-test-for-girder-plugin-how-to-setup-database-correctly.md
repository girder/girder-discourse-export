# Unit test for Girder plugin: How to setup database correctly?

## Charles on 2019-07-29T16:58:48.044Z

I am creating unit tests for a girder plugin, using a structure similar to the one used in the [dicom\_viewer](https://github.com/girder/girder/tree/master/plugins/dicom_viewer), but I have a problem with the `setUp` function. To make it short, I have girder exception: `An assetstore with that name already exists` if the `setUp` function is called more than once.  

To go further into the details, I have notice that during the `setUp` function, my table **girder\_test\_plugin** (coming from the girder config) is not really dropped but replaced by another table **girder\_test\_\[a long hash]**. The name of this table is the “default database name” of the mongo client object: `_modelSingletons[0].database` contains the bad name.  

I have no idea where this name comes from as this is not the one in my **girder.cfg** file and I do not observe this behavior in other girder plugins like the dicom\_viewer. Does someone know where does this bad name comes from and how can I change it ?


Best,  

Charles


---

## danlamanna on 2019-07-30T14:46:40.723Z


> I am creating unit tests for a girder plugin, using a structure similar to the one used in the [dicom\_viewer](https://github.com/girder/girder/tree/master/plugins/dicom_viewer) , but I have a problem with the `setUp` function.


So you’re using cmake and unittest instead of pytest?



> To make it short, I have girder exception: `An assetstore with that name already exists` if the `setUp` function is called more than once.


This sounds right, `setUp` isn’t meant to be called more than once. In fact, I don’t think `setUp` is supposed to be invoked by the caller \- the unittest module takes care of calling it for you.



> To go further into the details, I have notice that during the `setUp` function, my table **girder\_test\_plugin** (coming from the girder config) is not really dropped but replaced by another table **girder\_test\_\[a long hash]** .


I’m a bit confused here because the hash is created by pytest. However, pytest tests don’t have a `setUp` function, they use fixtures for setup and teardown.


For background on the hashing: the pytest runner sets up a database for each test function, and the old runner (unittest/cmake) set up a database per *module*. Since test function names can be long (`test_my_function_does_this_correctly`) and Mongo doesn’t allow extremely long database names, we hash the function name to truncate it in a way that provides uniqueness.


The [cookiecutter plugin](https://github.com/girder/cookiecutter-girder-plugin/tree/master/%7B%7Bcookiecutter.package_name%7D%7D) really is the best reference here. It doesn’t include a test that accesses the database, but that’s as simple as writing a regular pytest test that has a `db` fixture in the argument list. In fact, once pytest is installed you can view the fixtures provided by pytest\-girder by running `pytest --fixtures`:



```
----------------- fixtures defined from pytest_girder.fixtures -----------------
smtp -- pytest_girder/pytest_girder/fixtures.py:137
    Provides a mock SMTP server for testing.
admin -- pytest_girder/pytest_girder/fixtures.py:161
    Require an admin user.
    
    Provides a user with the admin flag set to True.
user -- pytest_girder/pytest_girder/fixtures.py:175
    Require a user.
    
    Provides a regular user with no additional privileges. Note this fixture requires
    the admin fixture since an administrative user must exist before a regular user can.
fsAssetstore -- pytest_girder/pytest_girder/fixtures.py:190
    Require a filesystem assetstore. Its location will be derived from the test function name.
bcrypt -- pytest_girder/pytest_girder/fixtures.py:21
    Mock out bcrypt password hashing to avoid unnecessary testing bottlenecks.
db -- pytest_girder/pytest_girder/fixtures.py:31
    Require a Mongo test database.
    
    Provides a Mongo test database named after the requesting test function. Mongo databases are
    created/destroyed based on the URI provided with the --mongo-uri option and tear-down
    behavior is modified by the --keep-db option.
server -- pytest_girder/pytest_girder/fixtures.py:79
    Require a CherryPy embedded server.
    
    Provides a started CherryPy embedded server with a request method for performing
    local requests against it. Note: this fixture requires the db fixture.

```

---

## Charles on 2019-07-30T15:19:47.943Z


> So you’re using cmake and unittest instead of pytest?


No, I want to use pytest. And calling pytest on this plugin seems to work. However, the `from tests import base` seems to import unittest… so I am bit lost here.



> This sounds right, `setUp` isn’t meant to be called more than once. In fact, I don’t think `setUp` is supposed to be invoked by the caller \- the unittest module takes care of calling it for you.


I don’t call it manually, but it is called by my tests environment automatically prior to each call of my `test_` functions.



> I’m a bit confused here because the hash is created by pytest. However, pytest tests don’t have a `setUp` function, they use fixtures for setup and teardown.


Thanks for the clarification about the hash, it may explain why I have this behavior and the plugin\_viewer doesn’t.  

I think I might have a bad cocktail of test environments mixing up together…  

I need to fully use the pytest version and not the old unittest one.  

I will remove the `setUp` function and the `from tests import base` a try to use the **db** fixture.  

Thanks for your help.


---

## danlamanna on 2019-07-30T15:26:05.875Z


> I think I might have a bad cocktail of test environments mixing up together…  
> 
> I need to fully use the pytest version and not the old unittest one.  
> 
> I will remove the `setUp` function and the `from tests import base` a try to use the **db** fixture.


Sounds good! I slightly misspoke earlier, since the cookiecutter test uses the `server` fixture and that depends on the `db` fixture, the database is being used there. For proof, this is the output of `pytest --setup-plan` when run on the cookiecutter generated plugin:



```
test/test_load.py::test_import 
SETUP    S fastCrypt
        SETUP    F db
        SETUP    F server (fixtures used: db)
        test/test_load.py::test_import (fixtures used: db, fastCrypt, server)
        TEARDOWN F server
        TEARDOWN F db
TEARDOWN S fastCryptCoverage.py warning: No data was collected. (no-data-collected)


```

---

## Charles on 2019-07-30T15:53:00.204Z

Thank you for your help, I am reaching a much better solution !  

I have a last question:





![](https://discourse.girder.org/user_avatar/discourse.girder.org/danlamanna/48/50_2.png) danlamanna:

> For background on the hashing: the pytest runner sets up a database for each test function, and the old runner (unittest/cmake) set up a database per *module* .



Is it possible to setup a database per module instead of per function ? I have to upload some stuff and then to test the requests and setting up the database each time would take too much time.


---

## Charles on 2019-07-30T15:54:51.507Z

I found out ! I just have to remove the server fixture.


Thanks again for your help !


---

