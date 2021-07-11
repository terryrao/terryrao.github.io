# idea 项目可以打包，代码里依然显示找不到类的错误
;date: 2019-03-21 10:02:29
;tags:

今天遇到一个 IDEA 的问题。在导入 jar 包之后，仍然无法引用 jar 中的类。先后偿试了好几种方法，但是仍然无法生效。

1. 项目上点击右键 maven -> reimport 。

尝试过好几次，依然不行，但是 右边的 maven 窗口下，该项目的 dependencies 中却有该项目。

![maven 窗口](<https://github.com/terryrao/picturebed/raw/master/blogs/1553134093032.jpg>)

2. 删除该模块，重新导入，依然没有效果

前思后想怎么也想不出问题在哪里，打开源代码发现 netty源代码里也是各种报红，那就有可能是 idea 的 maven 插件出什么问题了，因此我手动把 netty 包里的类引入到自定义类的 main 方法中，然后再使用 maven 工具打包试试。果然没有报错。

![报错](<https://github.com/terryrao/picturebed/raw/master/blogs/1553134572229.jpg>)

![编译通过](<https://github.com/terryrao/picturebed/raw/master/blogs/1553134778606.jpg>)

然后尝试使用清除缓存试一下看看可不可以，点击` file ->invalidate caches/ restart` ，重启后，错误提示就消失了。

![清除缓存](<https://github.com/terryrao/picturebed/raw/master/blogs/1553134939879.jpg>)
