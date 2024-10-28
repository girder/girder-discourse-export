# Dropping Python 2 support

## Zach_Mullen on 2020-03-03T18:59:33.540Z

Hello,


On April 1, 2020, we plan to remove support for Girder running on Python 2\.7\. At that time:


* The minimum version of Python that Girder will support is Python 3\.6\.
* We will tag and publish a minor release that downstreams can pin to if they require more time to migrate.
* We will disable automated testing with Python 2 as part of our CI process.
* New code changes that contain Python 3\+ language features will be allowed.


Thanks,


\-The Girder support team


---

## Zach_Mullen on 2020-03-03T18:59:43.330Z


---

## Zach_Mullen on 2020-04-01T18:13:05.146Z

The changes to drop Python 2 support have officially landed in master. Prior to merging it, we published a final release of Girder that will support Python 2\.7, which is version 3\.1\.0\.


Thanks,


\-The Girder support team


---

## Zach_Mullen on 2020-04-27T14:27:00.402Z

One follow\-up to this: all users of the `girder/girder` docker image should no longer use the `-py3` suffixed containers. They will no longer be automatically built. For example, if you were using `latest-py3`, please instead switch to `latest` in order to keep up with the master branch.


Thanks,


\-The Girder support team


---

