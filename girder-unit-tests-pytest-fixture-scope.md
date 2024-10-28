# Girder unit tests - pytest fixture scope

## Thibault_Pelletier on 2019-12-11T14:21:01.247Z

Hi,


I am currently working with girder and for my unit tests I have the following scope use cases :


* function scope (current girder fixture scope) : Test different file parsing and associated state on the data base. Usually loads a few files at a time and is relatively fast.
* module scope : Setup a data base once for the module to test some processing with the data as it is loaded on the data base. Setups whole DICOM volumes and sometimes adds in some post processing.
* session scope : Integration or end to end tests requiring a complex test data base but which only needs to be loaded once. Setups a complex database with full post processing features.


As of now the implementation of the fixtures limits their usage to function scope. I was wondering what your thoughts were on using a different implementation for these fixtures to let the user have more flexibility on how they use it. Here are some implementations which would permit user defined scopes :


* Add [dynamic scoping](https://docs.pytest.org/en/latest/fixture.html#dynamic-scope) with option scope parameter (which would enabling grouping the scope of the database using the conftest.py file)
* Split the implementation into functions and helper functions using context manager. This in turn would enable a user to create their fixture with the following syntaxe :



> @pytest.fixture(scope\=my\_scope)  
> 
> def my\_db\_fixture(request):  
> 
> with pytest\_girder.fixture\_helpers.configure\_db(request) as db :  
> 
> \# Further config  
> 
> yield db


Best regards,  

Thibault


---

## Jonathan_Beezley on 2019-12-11T14:45:40.288Z

I think adding configurable scoping at least to the `db` fixture makes sense. From the docs, I can see how you could use it to set the fixture scope for **all** tests in your project. If you need to control the scope for different modules, it isn’t immediately clear to me how it would work.


I think we would be open to looking at changes to make the `db` fixture more modular, but it would require careful consideration. There are quite a few subtly complex interactions occurring in that code. Breaking it up and making those interactions part of the public API could add more maintenance burden than we are comfortable with.


---

## Thibault_Pelletier on 2019-12-11T15:55:39.642Z

Hi Jonathan,


Thanks for your answer.  

I have played around with pytest conftest.py file and it turns out it is scoped to the submodule it is defined in.


Using this feature and a configuration option, you could theoretically setup the scope of the database for each test sub module as long as it is dynamically scoped.


The filesystem would look like this :



```
> function_scope_tests
>> conftest.py
>> test_function.py
> module_scope_tests
>> conftest.py
>> test_module.py

etc.

```

Each conftest would override the config option to suite the scope.


Here is a toy project illustrating the mechanism : [https://github.com/Thibault\-Pelletier/PytestConftestScope](https://github.com/Thibault-Pelletier/PytestConftestScope)


The change to the current implementation should be minimal and the usage simple enough using the pytest.ini options.


The only drawback to this method would be mixing different scopes for different girder ressources. But I’m not sure this use case would be very common.


---

## Jonathan_Beezley on 2019-12-12T13:11:03.142Z

That sounds like a useful change to me. If you want to put a PR together, I can review it for you.


---

## Thibault_Pelletier on 2019-12-12T14:31:55.611Z

Thanks for your feedback. I will work on it in the next few weeks and when I have a working solution I will create a PR for it.


---

