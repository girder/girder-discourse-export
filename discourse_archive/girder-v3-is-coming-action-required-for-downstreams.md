# Girder v3 is coming: action required for downstreams

## Zach_Mullen on 2018-09-10T15:04:12.686Z

Girder developers, users, and interested parties:


We are excited to announce that the next major version of Girder is on the horizon. In the very near future, we will be opening up a merge window for breaking changes prior to Girder’s version 3 release. This merge window is a period prior to the version 3 release in which the “master” branch of the Girder repository will contain breaking changes, departing from compatibility with version 2\.x.


To prevent any disruption of downstream deployments, CI, and other critical infrastructure, please ensure you are currently tracking the [2\.x\-maintenance](https://github.com/girder/girder/tree/2.x-maintenance) branch instead of master, as Girder’s master branch will no longer support version 2 plugins after the merge window opens. Once this merge window begins, all PRs to Girder will be made against master, to support version 3 natively, but can optionally be backported onto 2\.x\-maintenance to support existing deployments as well.


The first and most significant change slated to occur will require all plugins to be installed as [standalone python packages](https://packaging.python.org/tutorials/packaging-projects/) that register themselves with Girder via [entrypoints](https://amir.rachum.com/blog/2017/07/28/python-entry-points/). Girder will no longer provide a `girder-install plugin`; CLI or support plugins being copied directly into its “plugins” directory. All plugins, include web client only, will now require a “[setup.py](https://github.com/girder/girder/blob/pip-installable-plugins/plugins/jobs/setup.py)” and must be installed into Girder’s python environment to function.


We realize this change is necessarily disruptive and will require migration for downstream plugins to become compatible with Girder 3, however we believe the changes will provide a number of substantial benefits to Girder’s ecosystem for both developers and users:


* Plugins will be installed, uninstalled, and upgraded using standard python tools (pip) rather than custom scripts. Among other benefits, this will allow IDEs to support static analysis and debugging out of the box.
* Plugins will be able to coexist inside and outside of Girder’s execution context without complicated import guards. In addition, importing modules from other plugins will no longer require deferring imports until after the server loads.
* Plugins having complex or optional dependencies will be substantially easier to install and can even be bundled into conda packages if desired.
* The version system that has always been exposed for Girder plugins will now acquire formal semantics in line with normal pip packages, enabling capabilities like pinning dependencies of one plugin on a specific version of another plugin.


The work on this effort resides in [girder\#2552](https://github.com/girder/girder/pull/2552) on github. Once it is finalized and merged, we will provide a migration guide for existing plugins so their developers can perform the necessary changes prior to Girder’s 3\.0 release.


We will post a follow\-up announcement on this thread once the window is officially opened.


As always, questions and concerns can be raised on our [discourse board](https://discourse.girder.org/).


---

## Zach_Mullen on 2018-09-10T15:05:05.424Z


---

## Zach_Mullen on 2018-09-10T15:05:44.214Z

To discuss this topic, please use the [Development board](https://discourse.girder.org/c/development).


---

## Zach_Mullen on 2018-09-19T21:08:26.852Z

The release window is now officially opened! We are beginning to merge breaking changes into the **master** branch. Downstreams using girder from git should be tracking the **2\.x\-maintenance** branch.


---

