# pip install girder issue

## sulsj on 2018-08-21T22:52:28.362Z

Hi


I am just trying to install Girder with virtualenv. Have you seen this error message?


(girder\_env) sulsj@ssul\-dm \~$ pip install girder  

Collecting girder  

Using cached [https://files.pythonhosted.org/packages/1e/47/0ccb4923851fe66c106a114af4033a9cc4b81f10ea5df15169b0e24df256/girder\-2\.5\.0\.tar.gz](https://files.pythonhosted.org/packages/1e/47/0ccb4923851fe66c106a114af4033a9cc4b81f10ea5df15169b0e24df256/girder-2.5.0.tar.gz)  

Complete output from command python setup.py egg\_info:  

/System/Library/Frameworks/Python.framework/Versions/2\.7/lib/python2\.7/distutils/dist.py:267: UserWarning: Unknown distribution option: ‘python\_requires’  

warnings.warn(msg)  

error in girder setup command: ‘install\_requires’ must be a string or list of strings containing valid project/version requirement specifiers; Expected version spec in funcsigs ; python\_version \< ‘3’ at ; python\_version \< ‘3’



```
----------------------------------------

```

Command “python setup.py egg\_info” failed with error code 1 in /private/var/folders/8b/khv7605j59s7cdm9\_xfm\_jf00000gp/T/pip\-install\-6\_CNhk/girder/


Thanks,  

Seung


---

## Jonathan_Beezley on 2018-08-22T13:11:40.136Z

This is caused by an old version of setuptools in your python environment. Running the following should fix it:



```
pip install -U pip setuptools

```

---

## sulsj on 2018-08-22T18:10:02.302Z

Dear Jonathan


I’ve tried a few solutions including yours but it did not work. I am using macOS High Sierra. Any idea?


(girder\_env) sulsj@ssul\-dm \~/work/git\-repos/girder$ pip install \-U pip setuptools  

Requirement already up\-to\-date: pip in /Library/Python/2\.7/site\-packages (18\.0\)  

Collecting setuptools  

Using cached [https://files.pythonhosted.org/packages/66/e8/570bb5ca88a8bcd2a1db9c6246bb66615750663ffaaeada95b04ffe74e12/setuptools\-40\.2\.0\-py2\.py3\-none\-any.whl](https://files.pythonhosted.org/packages/66/e8/570bb5ca88a8bcd2a1db9c6246bb66615750663ffaaeada95b04ffe74e12/setuptools-40.2.0-py2.py3-none-any.whl)  

Installing collected packages: setuptools  

Found existing installation: setuptools 18\.5  

Not uninstalling setuptools at /System/Library/Frameworks/Python.framework/Versions/2\.7/Extras/lib/python, outside environment /Users/sulsj/girder\_env  

Can’t uninstall ‘setuptools’. No files were found to uninstall.  

Successfully installed setuptools\-40\.2\.0


Thanks,  

Seung


---

## Jonathan_Beezley on 2018-08-22T18:30:10.537Z

Hmmm… maybe try `pip install --user -U setuptools`.


---

## sulsj on 2018-08-22T18:50:32.796Z

Afraid not. It seems like being affected by the system’s setuptools. I am wondering if there is other way to install and setup Girder.


---

## Jonathan_Beezley on 2018-08-22T19:00:24.069Z

It’s possible `--ignore-installed` would force it to install setuptools in the virtualenv.


It’s probably best to avoid using the system python. On a Mac, I usually install an external python using [homebrew](https://brew.sh/) (`brew install python`). That should install the latest version of setuptools in an isolated python environment for you.


---

## sulsj on 2018-08-22T19:35:31.167Z

Thank you for your help.


I am using conda env mostly for my dev environment. So I’ve never had a problem with system’s installation.


Anyway, I’ve tried the flag to ignore the system’s setuptools, but “pip install girder” still does not work.


**girder\_env) sulsj@ssul\-dm \~$ pip install \-U pip setuptools \-\-ignore\-installed**  

Collecting pip  

Using cached [https://files.pythonhosted.org/packages/5f/25/e52d3f31441505a5f3af41213346e5b6c221c9e086a166f3703d2ddaf940/pip\-18\.0\-py2\.py3\-none\-any.whl](https://files.pythonhosted.org/packages/5f/25/e52d3f31441505a5f3af41213346e5b6c221c9e086a166f3703d2ddaf940/pip-18.0-py2.py3-none-any.whl)  

Collecting setuptools  

Using cached [https://files.pythonhosted.org/packages/66/e8/570bb5ca88a8bcd2a1db9c6246bb66615750663ffaaeada95b04ffe74e12/setuptools\-40\.2\.0\-py2\.py3\-none\-any.whl](https://files.pythonhosted.org/packages/66/e8/570bb5ca88a8bcd2a1db9c6246bb66615750663ffaaeada95b04ffe74e12/setuptools-40.2.0-py2.py3-none-any.whl)  

Installing collected packages: pip, setuptools  

Successfully installed pip\-18\.0 setuptools\-40\.2\.0


**(girder\_env) sulsj@ssul\-dm \~$ pip install girder**  

Collecting girder  

Using cached [https://files.pythonhosted.org/packages/1e/47/0ccb4923851fe66c106a114af4033a9cc4b81f10ea5df15169b0e24df256/girder\-2\.5\.0\.tar.gz](https://files.pythonhosted.org/packages/1e/47/0ccb4923851fe66c106a114af4033a9cc4b81f10ea5df15169b0e24df256/girder-2.5.0.tar.gz)  

Complete output from command python setup.py egg\_info:  

/System/Library/Frameworks/Python.framework/Versions/2\.7/lib/python2\.7/distutils/dist.py:267: UserWarning: Unknown distribution option: ‘python\_requires’  

warnings.warn(msg)  

error in girder setup command: ‘install\_requires’ must be a string or list of strings containing valid project/version requirement specifiers; Expected version spec in funcsigs ; python\_version \< ‘3’ at ; python\_version \< ‘3’



```
----------------------------------------

```

Command “python setup.py egg\_info” failed with error code 1 in /private/var/folders/8b/khv7605j59s7cdm9\_xfm\_jf00000gp/T/pip\-install\-mi0ggQ/girder/


Thanks,  

Seung


---

## Jonathan_Beezley on 2018-08-22T19:40:34.594Z

Using a conda env should work fine here. You just have to make sure that you are using `pip` from conda rather than the system.


---

## sulsj on 2018-08-22T20:10:15.953Z

It’s installed using “pip install girder” under my conda env. Thank you!


---

## sulsj on 2018-08-22T23:44:35.685Z

Well… After running “pip install girder” under a conda env, I can see the below files under “bin”.


girder\-install  

girder\-server  

girder\-sftpd  

girder\-shell


However, “girder” cannot be found for testing “girder serve”.


---

## danlamanna on 2018-08-23T00:14:36.000Z

Is it possible you have an older version of Girder already installed? girder\-server does the same thing as the newer girder serve command.


---

## sulsj on 2018-08-23T20:57:25.007Z

There is no older version of Girder installed. Again I am using conda and executed “pip install girder” to install it.


And I’ve tested with “girder\-server” and found this.


(qaqc2\) sulsj@ssul\-dm \~/work/git\-repos/girder$ girder\-server serve  

Traceback (most recent call last):  

File “/Users/sulsj/miniconda2/envs/qaqc2/bin/girder\-server”, line 7, in   

from girder.**main** import main  

ImportError: cannot import name main


Thanks,  

Seung


---

## Jonathan_Beezley on 2018-08-24T15:13:34.755Z

It definitely looks like you have scripts left over from girder 2\.4\. I just tried installing with miniconda3, and it worked fine. I wonder if you might have an installation of girder from conda\-forge. Could you try looking at `conda list` to see if girder 2\.4 is listed? You might also need to make an new virtualenv if the old version cannot be cleaned up successfully.


---

## sulsj on 2018-08-24T19:30:58.084Z

With a clean env, it works like a charm. Thanks for the help.


One more question. Is there any use case to use Girder for bridging geographically remote file systems or HPSSs? Have you seen any 3rd party plugin related with that?


Thanks,  

Seung


---

## Jonathan_Beezley on 2018-08-27T12:08:09.424Z


> One more question. Is there any use case to use Girder for bridging geographically remote file systems or HPSSs? Have you seen any 3rd party plugin related with that?


I think we’ve done some experimenting with Globus integration, but I don’t know what the status is. [@Zach\_Mullen](/u/zach_mullen) can probably tell you more.


---

## sulsj on 2018-08-28T18:46:11.946Z

Thanks for the info. [@Zach\_Mullen](/u/zach_mullen), Is there any way I can access the related info on the Globus integration?


---

## Zach_Mullen on 2018-08-28T18:48:11.566Z

Hi [@sulsj](/u/sulsj),


As of right now, we don’t have anything concrete to point to regarding Globus integration, other than using Globus’s OAuth provider for identity provision here: <https://github.com/girder/girder/blob/master/plugins/oauth/server/providers/globus.py>


A few of us on the Girder team are talking with some Globus developers tomorrow to try and see what further integrations we can do between the technologies. If we decide on anything to do then, I will update this thread.


Thanks,


\-Zach


---

## sulsj on 2018-08-29T20:28:15.324Z

Hi Zach,


Thank you for the info. I will be looking forward to hear an update.


Thanks,  

Seung


---

