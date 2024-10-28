# Design Questions about external data for Bazel

## EricCousineau-TRI on 2017-11-22T22:16:47.923Z

Howdy [@Michael\_Grauer](/u/michael_grauer) and [@danlamanna](/u/danlamanna),


The design questions I have are related to the current iteration of our [Evolutionary Prototype](https://github.com/EricCousineau-TRI/external_data_bazel).  

The workflows this is intended to support are partially covered [here](https://github.com/EricCousineau-TRI/external_data_bazel/blob/ed9bfd7f47cf80c6895c3efc21af91396197fa1c/docs/workflows.md) and with some simple test code [here](https://github.com/EricCousineau-TRI/external_data_bazel/blob/ed9bfd7f47cf80c6895c3efc21af91396197fa1c/test/basic_workflows_test.sh), with Girder\-specific testing done [here](https://github.com/EricCousineau-TRI/external_data_bazel/tree/ed9bfd7f47cf80c6895c3efc21af91396197fa1c/test/backends/girder). An adaptation of JC’s demo using this prototype is available [here](https://github.com/EricCousineau-TRI/bazel-large-files-with-girder/tree/feature/external_data) (though the documentation is not updated).  

The more descriptive workflows are detailed in the Google Document(s) that I had sent out a week or so ago.


Ultimately, I would like this project to ultimately support (a) reproducibility in testing (like `CMake/ExternalData`), (b) ease of use within Bazel and (c) possibly CMake, (d) useful in an open\-source development model (enabling contributors to test their changes on some other CI), and (d) allow drawing from multiple (possibly heterogeneous) sources within the same project.


For the short\-term, the main high\-level items we need are (a) and (c).


Short term design questions:


1. Does the name `external_data_bazel` seem appropriate, or might that generate confusion with `CMake/ExternalData`? (Given that this approach is probably geared to a broader scope?)
2. (High\-level and fuzzy) Could you possibly run through the tutorial code briefly (included in the archive in the prior email), review the testing code in the prototype (the general workflows, and that specific to Girder), and let me know if you see any areas for improvement as far as what is tested, and how it is tested?


Longer\-term design questions (lower priority, does not have to be answered soon):


As mentioned in a prior email chain, we did not use `git-lfs` or `git-annex` as they did not serve our main purpose (ease of integration with Bazel); given that, I’d like to have an interface that can be agnostic to the frontend being used, whether they be `CMake/ExternalData`\-style hash files, `git-lfs` pointer files, `git-annex` symlinks, or whatever. To accommodate this, the CLI can query file information from the frontend based on the filepath (without any hash\-file suffix), which will ultimately yield the hash that is needed for the given filepath. This can be used to download the file (or on the reverse side, this metadata can be used when uploading).


Additionally, I wanted the backend to be flexible as well; it can use a mock backend for testing, Girder\+Hashsum for production, or `git-lfs` (server or client) or `git-annex` in other areas.


From your experiences, can I ask if you see any immediate pitfalls that this may fall into?


Thanks,


* Eric


P.S. If you would like more explanation about why we are not using `git-lfs` / `git-annex` directly, and how it does not satisfy ease of integration Bazel, please feel free to ask that here!


---

## EricCousineau-TRI on 2017-12-06T09:05:54.492Z

Howdy [@Michael\_Grauer](/u/michael_grauer) and [@danlamanna](/u/danlamanna)! May I ask if y’all have had a chance to review this?


---

## danlamanna on 2017-12-06T16:38:44.000Z

Yes! Sorry for the delay, see my comments below.


Thanks for the great documentation, it helped quite a bit when going  

through this. I’ve only just started working with external\_data\_bazel so  

I’m not familiar with everything that’s going on here, but I wanted to  

give my first impressions. Keep in mind I haven’t used Bazel before, so  

I might suggest things that don’t make sense in a Bazel context.


To clear things up for me, this is the impromptu glossary I’ve been  

using:


Backend: responsible for uploading/downloading of a project file.


Remote: delegate uploading/downloading to their backend, while adding  

cache/overlay functionality.


Frontend: responsible for determining the correct remote and file  

information given a particular “pointer file”.



> 1. Does the name `external_data_bazel` seem appropriate, or might that  
> 
> generate confusion with `CMake/ExternalData`? (Given that this  
> 
> approach is probably geared to a broader scope?)


It seems the goal is to make an external data system that can be used  

outside of the context of Bazel entirely, so a more generic name would  

seem preferable. Though to clarify, it seems like the primary goal is to  

support Bazel as a first class citizen. With that said, did you mistype  

when you said a and c? I had thought a and b were the more relevant  

priorities.



> 2. (High\-level and fuzzy) Could you possibly run through the tutorial  
> 
> code briefly (included in the archive in the prior email), review the  
> 
> testing code in the prototype (the general workflows, and that  
> 
> specific to Girder), and let me know if you see any areas for  
> 
> improvement as far as what is tested, and how it is tested?


If you could help me understand how often the different actions in  

workflows would be executed it might help shape my thoughts. One thing I  

noticed was that while uploading/downloading is performed using  

external\_data, registering files within the system is still a fairly  

manual process. Perhaps this isn’t viable in a Bazel world, but it would  

be nice if a CLI tool could manage the entirety of the workflow.


Here are some first impressions of the code and testing infrastructure:


* There are a number of places where code that should be handled by  

libraries is done by shelling out to curl, ln, cp, etc. If at all  

possible this should really be avoided. Particularly with curl, using  

the requests library (packaged in major distros) would remove a  

non\-trivial amount of code.
* File locking  

There are libraries for this as well (filelock), since this is one of  

those things that’s very difficult to get right. I don’t know that  

there’s anything actionable here, just to be sure that code is aware  

that things may not go as planned.
* Docker usage in testing  

It’s possible that using a tool such as docker\-compose could cut down  

the test bootstrapping code a bit. There are also tools like  

[GitHub \- vishnubob/wait\-for\-it: Pure bash script to test and wait on the availability of a TCP host and port](https://github.com/vishnubob/wait-for-it) which can be less fragile  

than sleep. Using docker and docker\-compose with Girder for testing  

more complex applications is something we’re also looking into  

currently \- so expect us to share any of that information in the  

future.
* There’s also some code in core.py that uses exec which could lead to  

some difficult to debug and/or unexpected behavior. Admittedly, I don’t  

fully understand what’s going on there.



> From your experiences, can I ask if you see any immediate pitfalls  
> 
> that this may fall into?


The only thing that jumps out at me is the level of abstraction. I see  

it as a scale where on the one end everything is being abstracted to  

work with anything (git\-lfs, git\-annex, girder\+hashsum, bazel, cmake,  

etc), and on the other extreme is making this only work with  

bazel/girder\+hashsum. I think leaning towards the narrower scope is  

better, keeping in mind the future may hold things on the opposite end  

of the scale. In other words, trying to make a narrow goal and iterating  

on that has led to much better outcomes for us than generalizing a lot  

from the start.


All of that being said, let me know what you think and how we should  

approach next steps in terms of the work that needs to be done going  

forward.


Thanks!


---

## EricCousineau-TRI on 2017-12-12T00:46:48.559Z

Thank you for the follow\-up!



> To clear things up for me, this is the impromptu glossary I’ve been  
> 
> using:


Yep, those are accurate!



> Though to clarify, it seems like the primary goal is to  
> 
> support Bazel as a first class citizen. With that said, did you mistype  
> 
> when you said a and c? I had thought a and b were the more relevant  
> 
> priorities.


Yes, I had mis\-typed that, and did mean (a) and (b). (I believe I had re\-ordered those at some point.)



> If you could help me understand how often the different actions in workflows would be executed it might help shape my thoughts.


I don’t have any concrete numbers for you, but if I had to make some status up:


* Downloading: Any and all Drake users, hopefully without having to configure tokens, etc. For a developer, will probably happen 1\-20x a week, with about 10\-15 developers. For a user, maybe 1\-5x a month?
* Uploading: Probably once or twice a week, though possibly in batches (with 1 \- 20 files at a time, totaling 100 MB \- 1 GB).
	+ New Uploads: Probably 60%
	+ Updates: Probably 40%
* Adding to Bazel: Same frequency as uploading.



> One thing I noticed was that while uploading/downloading is performed using  
> 
> external\_data, registering files within the system is still a fairly  
> 
> manual process. Perhaps this isn’t viable in a Bazel world, but it would  
> 
> be nice if a CLI tool could manage the entirety of the workflow.


I’d prefer to leave that entirely to Bazel. There is an option to glob the files [here](https://github.com/EricCousineau-TRI/external_data_bazel/blob/master/docs/workflows.md#use-sha512-groups-in-build), if a user does not want to manually enter each in. That’s generally what we do in Drake at present as well (see [example](https://github.com/RobotLocomotion/drake/blob/c8a6b5157983c1472392f09ca3770951bbce90a4/manipulation/models/iiwa_description/BUILD.bazel#L29) and [library code](https://github.com/RobotLocomotion/drake/blob/c8a6b5157983c1472392f09ca3770951bbce90a4/tools/install/install_data.bzl#L53) \- please ignore the fact that it’s being installed).



> Here are some first impressions of the code and testing infrastructure:


Thanks for the feedback! The code is definitely hacky, and was namely cobbled together when porting JC’s bash script; will definitely clean it up in prior to an initial submission. Thank you for the advice on `filelock`, `requests` (some colleagues have used this, and really like it), `docker compose` / `wait-for-it` as well \- will try those out.



> There’s also some code in core.py that uses exec which could lead to  
> 
> some difficult to debug and/or unexpected behavior.


That was just me getting excited / allowing feature creep; it was meant to allow users to define / register custom backends for their projects.



> I think leaning towards the narrower scope is better, keeping in mind the future may hold things on the opposite end  
> 
> of the scale.


Sounds like solid advice!  

I’d like to keep a little bit of the abstraction in place (at least some general delegation), but will pare down the end product. The main thing I’d like to keep is the overlay structure for `devel` vs. `master`, but perhaps I can initially delegate that to Girder (if it’s easily testable).



> All of that being said, let me know what you think and how we should  
> 
> approach next steps in terms of the work that needs to be done going  
> 
> forward.


At a lower feature level on the Girder end, I’d say having the ability to retrieve file information based on hashsum ([per this topic](https://discourse.girder.org/t/enable-retrieving-file-information-from-the-hashsum-plugin/68/4)) would be the next best step for me.  

(Just to check, is this what you meant by “next steps”? Or were you thinking at a higher level?)


**EDIT**:


Forgot to answer about the naming. Yes, this is of at kind of an awkward middle ground of prioritizing Bazel (since that is what we as developers are using, especially for testing), but I would like it to eventually support consumption from CMake (for myself and peers in future projects, etc). However, since we are not yet at that point, maybe the first deployment can just be named to be Bazel\-specific.  

(Maybe keeping the name `bazel_external_data` / `external_data_bazel`, adopting another more general name later.)


---

## Michael_Grauer on 2017-12-13T21:55:07.607Z

[@EricCousineau\-TRI](/u/ericcousineau-tri)


What do you mean having the ability to retrieve file info based on the hashsum topic?


That topic has been merged–do you mean rolling out that update to the TRI Girder instance, or having us work with you to consume that endpoint somehow? If it is consuming the endpoint, please let us know some concrete tasks.


I think generally (the higher level) we want to understand where the split of work will be, will you want to continue doing all the work in your repo with our advising, or will you want us to start making PRs to your repo based on what we all agree to?


Thanks


---

## EricCousineau-TRI on 2017-12-13T22:28:45.073Z


> That topic has been merged–\[…]


Ah, my apologies! I had not subscribed to the topic, and had not checked it.  

I will try this out on my end.



> \[…] will you want to continue doing all the work in your repo with our advising \[…] ?


For now, I believe the Bazel interface should be relatively simple (in fact, I believe I need to simplify it a tad more from what we have), and will go ahead and continue doing the work with y’all advising. However, if that changes, I will certainly let you know.


Thank you!


---

## Michael_Grauer on 2017-12-13T23:34:48.134Z

Great thanks, I’m glad we have you unblocked and a path forward. Please reach out as you move along.


---

