# Error while attempting to mount volume

## ajohn15 on 2022-01-25T18:37:07.048Z

I am trying to mount a volume to dsa\_girder using the docker run \-v command it returns a getsocketopt failed strangley error. This is in order to mount a large number images from the host system to be accessible by DSA. Am I approaching this incorrectly, would girder mount be used in this case?


---

## David_Manthey on 2022-01-25T18:44:37.602Z

docker run \-v is the usual way to mount volumes for a dockerized install of Girder or DSA (or using the equivalent volume command inside a docker\-compose file). `girder mount` is used in the same container as girder to provide file\-like access to resources regardless of whether they are on local or remote assetstores; it is not used to mount specific volumes.


– David


---

## ajohn15 on 2022-01-25T18:58:55.431Z

Thank you, would I use docker run \-v on the dsa\_girder image or would it be another?


---

## ajohn15 on 2022-01-26T21:50:41.017Z

I was able to successfully mount the desired volume using docker compose. But I am having an issue importing the data into DSA. It hangs after a certain number of files, would there be something I am missing?


---

## David_Manthey on 2022-01-26T21:56:02.389Z

There shouldn’t be any resource exhaustion based on number of files. There are some cases where, if they are large image files, the default cache options can end up using up available memory – but the default docker\-compose for DSA should be set to not have such issues.


Are there any distinct properties of your files that you can share to help diagnose this?


---

## ajohn15 on 2022-01-26T22:19:04.344Z

It is a large .ndpi (3\.2GB). I tried to view its metadata using openslide but was unsuccessful, I will try to get more information.


---

## ajohn15 on 2022-01-26T22:36:21.029Z

What would I need to change in the docker\-compose to alleviate the cache issue. I did leave everything default excecpt adding the volume.


---

