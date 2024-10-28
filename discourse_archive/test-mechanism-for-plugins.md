# Test mechanism for plugins

## Charles on 2019-07-25T13:37:01.875Z

I would like to add unit tests on a girder plugin we are currently developing.  

I have read the [readthedoc page](https://girder.readthedocs.io/en/stable/plugin-development.html#automated-testing-for-plugins) and have some questions about the right way to do it.


I have read [here](https://girder.readthedocs.io/en/stable/development.html#running-the-tests-with-ctest) that girder is currently transitioning from ctest to pytest. It means that plugins should also adopt pytest as ctest will certainly be dropped someday right ?  

On the other hand, it seems that pytest is not supported yet for plugins (from [here](https://girder.readthedocs.io/en/stable/plugin-development.html#testing-server-side-code)). So what would be the good practice in order to create unit tests for a girder plugin today ?


PS. I have not completely understood the [Testing Server\-side Code](https://girder.readthedocs.io/en/stable/plugin-development.html#testing-server-side-code) in the RtD.


---

## Zach_Mullen on 2019-07-25T13:59:19.191Z


> I have read [here](https://girder.readthedocs.io/en/stable/development.html#running-the-tests-with-ctest) that girder is currently transitioning from ctest to pytest. It means that plugins should also adopt pytest as ctest will certainly be dropped someday right ?


Yes!



> On the other hand, it seems that pytest is not supported yet for plugins (from [here](https://girder.readthedocs.io/en/stable/plugin-development.html#testing-server-side-code)). So what would be the good practice in order to create unit tests for a girder plugin today ?


It should be supported, you will just need to use the [pytest\_girder](https://pypi.org/project/pytest-girder/) package. Then you can just put the tests in your own repository and run it via the `pytest` command.


---

## Zach_Mullen on 2019-07-25T14:01:55.118Z

By the way, if you have questions about best practices, we should start a discussion topic for that, I think it would be great. One thing I think we have all come to agree on is that `tox` should be used to harness your CI process.


---

## Jonathan_Beezley on 2019-07-25T14:03:38.787Z

The statement on RTD is about the latest stable release. The eminent 3\.0 release docs are at [https://girder.readthedocs.io/en/latest/plugin\-development.html](https://girder.readthedocs.io/en/latest/plugin-development.html). These go into how to do plugin testing using pytest.


---

## Jonathan_Beezley on 2019-07-25T14:04:58.502Z

Actually, I just noticed that we still have a TODO in that section of the docs. It’s something we should resolve before release.


---

## Charles on 2019-07-25T14:15:07.146Z

Thank you for your answer,



> It should be supported, you will just need to use the [pytest\_girder](https://pypi.org/project/pytest-girder/) package. Then you can just put the tests in your own repository and run it via the `pytest` command.


Is a girder instance launched for my test when I run `pytest` ? Will I be able to test functions that require a connection to the database or do I need to do something special for that ?


---

## Zach_Mullen on 2019-07-25T14:19:11.330Z

The `pytest_girder` library contains a set of fixtures for stuff like database connectivity and the girder server (WSGI app). These will be available by virtue of having the package installed, so you can just use them out of the box. See <https://github.com/girder/girder/blob/master/pytest_girder/pytest_girder/fixtures.py>


---

## Jonathan_Beezley on 2019-07-25T14:19:59.679Z

You can also look at the Girder’s own tests for usage examples. <https://github.com/girder/girder/blob/master/test>


---

## Charles on 2019-07-25T14:20:50.911Z

Thank you for your answers and all the resources


---

## danlamanna on 2019-07-25T14:31:11.718Z

We haven’t done much in the way of advertising yet, but we’re trying to push developers towards using our [cookiecutter template for Girder 3 plugins](https://github.com/girder/cookiecutter-girder-plugin). This includes a lot of our learned best practices in regards to setup.py, tox configuration, linting, etc. There’s also a sample test in the plugin which can be invoked via `tox` once generated.


---

## David_Manthey on 2019-07-25T14:38:39.961Z

If it is of use, the girder large\_image plugin has a girder\-3 branch that uses tox and runs both server and client tests. (see [https://github.com/girder/large\_image/tree/girder\-3](https://github.com/girder/large_image/tree/girder-3)).


---

