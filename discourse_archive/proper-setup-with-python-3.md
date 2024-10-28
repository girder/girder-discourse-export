# Proper setup with Python 3

## rbaustin on 2020-01-28T22:30:43.744Z

I’m setting up a Girder instance on an Amazon Lightsail box. The box comes with Python 2 (`python`) and Python 3 (`python3`) installed.


I’m wondering what is the right approach to setup Girder using Python 3, since 2 is deprecated.


So far, I have a bash script for the box that looks like this:



```
sudo curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -

sudo apt update && sudo apt install -y --no-install-recommends nodejs golang-go python-pip python-virtualenv virtualenv

virtualenv girder_env
source girder_env/bin/activate

pip install -U pip setuptools dnspython girder girder-homepage girder-worker large-image[all] girder-large-image-annotation[tasks] --find-links https://girder.github.io/large_image_wheels histomicsui --find-links https://girder.github.io/large_image_wheels

girder build

girder serve --host 0.0.0.0 --database [database URL]

```

I tried replacing the `python-pip` install with `python3-pip`, and then the `pip install` line with `pip3 install`. That part all worked fine, but then the `girder build` line throws the error:



```
girder: command not found

```

If I try to run the `pip install` without the `3`, I get the Python 2 deprecated warning.


Is there something I’m missing here? Sorry if this is a stupid question. I’m not a Python developer.


---

## Jonathan_Beezley on 2020-01-29T13:51:43.224Z

You should create your virtual environment with



```
virtualenv -p python3 girder_env

```

From there, you can keep everything the same as it will use the pip installed within your virtual environment.


One other minor point, you should upgrade pip and setuptools **before** installing the rest of the packages. When you do it in the same line, the upgraded versions aren’t actually used.


---

## rbaustin on 2020-01-29T18:01:17.759Z

Ok cool. This was super helpful. I realize this was more related to virtualenv than Girder now. With those changes, everything built as expected.


---

