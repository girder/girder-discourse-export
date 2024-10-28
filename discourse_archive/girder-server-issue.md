# Girder server issue

## wangcuiting on 2018-09-04T08:18:43.871Z

I tried to run the girder sever,when I followed the instructions : girder serve . It didn’t work . :\\


![2018-09-04%2016-08-12%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE](https://discourse.girder.org/uploads/default/original/1X/85fea3b0aaa7fc1d6c9b6f915aceae92fdd690a3.png)


Warning: entry point could not be loaded. Contact its author for help.


I don’t know how to start it.


---

## Jonathan_Beezley on 2018-09-04T11:52:22.976Z

The version of `six` is incompatible. Try running the following in your virtual environment:



```
pip install -U --upgrade-strategy eager girder

```

---

## wangcuiting on 2018-09-05T08:11:47.065Z

Thank you very much, it’s fixed


---

