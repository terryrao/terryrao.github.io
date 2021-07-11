# lombok cannot find symbol
;date: 2019-05-27 14:53:25
;tags:


使用 lombok 的时候遇到一个坑，使用 windows 和 mac idea 编译及maven 打包的时候没有报错，正常运行，
但是使用在 linux 上使用 jenkins打包的时候却出总出现编译错误，由于我们的其它项目里也使用过 lombok 且正常运行，因此，把环境和 lombok 的 jar 包都调到与另一个一致，包括 linux 的环境。然后测试了一下，仍然报错：

![编译错误](https://github.com/terryrao/picturebed/raw/master/blogs/lombok_cannot_find_symbol/WX20190527-150110%402x.png)

查看第一行报错的时候发现有个类重复的问题，可能是当时其它人在生成类的时候没有大写，后把另外一个人又添加一个名称相同的类，类文件名首字母大写了，但是 idea 编译的时候并没有给于警告或者提示。把这个小写类名的文件去掉后，再重新把包之后，就可以正常编译了。

之所以找了这么长时间的问题，还是因为自己没有认真看错误提示，看到后面都是`cannot find symbol` 本能的认为是 `lombok` 的问题。导致查询方向有问题，做了长时间的无用功。
