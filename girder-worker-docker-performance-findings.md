# Girder worker (docker) performance findings

## Edern_Haumont on 2019-04-16T11:10:31.322Z

Here are my findings concerning performance of girder workers with docker. Below is a table with data collected while running [this](https://github.com/girder/interactive_thumbnails) example that uses dcm files contained in an item to produce some outputs.





| Number of .dcm files | Memory used by worker (Mo) | Time of execution (s) |
| --- | --- | --- |
| 2 | 70 | 16 |
| 10 | 80 | 18 |
| 100 | 300 | 36\.7 |
| Special case: 100\+100 (2 workers) | 599 | 50 (concurrent run) |
| Special case: Hello world | \<20 | 5 |


Note 1: The timings using two workers are not reliable. Indeed I used the same item for both workers so one’s results may have interfered with the other’s.  

Note 2: Running the hello world image alone takes 2\.5s on my computer.


### Conclusions


#### Memory


* the memory cost of a worker is negligible.
* the memory cost of a worker \+ using the image of the example is about 70 Mo. So instantiating docker images costs some memory, depending on the image.
* the memory costs of workers running the same image are independent.


#### Time


* It is more costly to run a docker image from a worker than alone.
* The “starting” time (before actually running the code) depends on the image.


### Question


What are your data (any metric) concerning the overhead induced by using the docker\_run task vs executing the same script in a non\-docker task ?


---

