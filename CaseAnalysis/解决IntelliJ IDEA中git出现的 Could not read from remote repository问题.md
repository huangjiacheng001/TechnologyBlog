# 解决IntelliJ IDEA中git出现的 Could not read from remote repository问题

2019.2.26更新
我也不知道原因是什么，这是搬运的stackoverflow上的方法，碰巧奏效，就分享了，有兴趣的朋友可以深入探究一下。

------

最近用IDEA上的git功能出现了可以commit但无法push和pull的问题，测试发现原因是Could not read from remote repository，在Stack Overflow上发现了解决方法。

在Settings->Version Control->Git中，将SSH executable设置为Native即可，如图，红色方框中是要修改的地方。
![这里写图片描述](https://img-blog.csdn.net/20180227135803327?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3RldmVsaXUxMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)