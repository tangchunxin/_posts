---
title: hostory.go(-1)
date: 2018-05-09 22:00:20
tags:
---

## hostory.go(-1)
```
  if (! $platform['pprovince']) {
        echo "<script>alert('省份名称不能为空');history.go(-1);</script>";
        exit();
    }
```
#### 两个方法的区别
既然history.back(-1)和history.go(-1)都是返回之前页面，但是方法不同，所以肯定是有区别的：

- history.back(-1)//直接返回当前页的上一页，数据全部消息，是个新页面
- history.go(-1)//也是返回当前页的上一页，不过表单里的数据全部还在
#### 总结
返回、前进页面的方法下面总结一下：

- window.location.reload() //刷新
- window.history.go(1) //前进
- window.history.go(-1) //后退
- window.history.forward() //前进
- window.history.back() 后退+刷新
- 

