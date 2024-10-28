# Docker images are now all on Python 3

## brianhelba on 2020-01-08T15:58:51.413Z

[Girder’s Docker images](https://hub.docker.com/repository/docker/girder/girder) will soon all be moving to use a base of Python 3\. Consumers who only use the images for unmodified execution should not be affected.


Accordingly, new Docker image tags with a suffix of `-py3` are now deprecated; for now, these will simply be aliases for tags of new releases and `latest` (e.g. `3.1.0` and `3.1.0-py3` will be the same image). The `-py3` tags will be discontinued in Girder’s next major release.


We’d welcome any feedback from users of these Docker images.


---

## Zach_Mullen on 2020-03-03T19:26:13.570Z


---

