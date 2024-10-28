# Error running histomicsTK analysis on docker

## doori on 2021-02-22T13:47:43.207Z

Hi,  

I have HistomicsUI running on docker\-compose, and I was able to pull the histomicstk plugin.  

However when I run a job, I get errors related to openblas and numpy:


OpenBLAS blas\_thread\_init: pthread\_create failed for thread 22 of 64: Resource temporarily unavailable  

OpenBLAS blas\_thread\_init: RLIMIT\_NPROC 1048576 current, 1048576 max  

…  

ImportError: PyCapsule\_Import could not import module “datetime”


I see that the celery worker is online, when I go to Task Information.  

I’ve also tried setting environment variables OPENBLAS\_NUM\_THREADS\=1, and uninstalling then reinstalling numpy and setuptools through pip, but I get the same error.


Thank you in advance for your time and help.


---

## doori on 2021-02-22T20:38:12.902Z

This issue was resolved by adding


ENV OPENBLAS\_NUM\_THREADS 1


in histomicstk Dockerfile and rebuilidng the image.


---

