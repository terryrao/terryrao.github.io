# 解决idea、ideavim 快键冲突
;date: 2019-04-30 22:26:12
;tags:

vim 用过一段时间后感觉很不错，写代码的效率比以前有所提高。大部分操作不再需要鼠标，快键熟练之后靠肌肉记忆能够快速编辑代码。但是也有感觉不好的地方，那就是 idea 本身的快键与 vim 的快键有非常多的冲突，特别是在 windows 上。16 年的时候使用 windows 的 idea 时就尝试过使用 ideaVim ，便是因为冲突键太多，找不到好的解决方案而放弃。后来换了 mac ，使用 mac 版的 idea 后，再使用 ideaVim 冲突键就减少很多，用着也比较顺畅。不过既然有冲突，还是想看看大牛们怎么解决这个问题。在网上搜了一下，也有人在 overstackflow 上提过类似的问题。按照上面的答案操作了一下，发现还可以。思路是找一个没有被绑定的快键，然后将这个键做为前缀，再将前缀跟冲突键配置在 `~/.ideavimrc` 中，映射到  idea 的 Action 上，具体操作如下：

1. 找到冲突鍵：

点击 `settings ` -> `editor` -> `vim Emulation` ，就会看到所有与 vim 冲突的键，如图：

![shotcut conflics](https://github.com/terryrao/picturebed/raw/master/blogs/WX20190430-230407%402x.png)

第一列是冲突的快键，第二列是 idea 的动作，第三列是处理方式，如果选择 vim 则表示按下该快键使用 vim 的快键效果，如果选择 IDE 则按下该快键使用的是 idea 的快键效果，如果是 undefined，则会在按下该快键时会给出冲突提示。这里我使用的全是 vim 的效果，然后再 `~/.ideavimrc` 里配置相应的 idea 动作。

2. 打开 `~/ideavimrc` 编辑 idea 的快键

假设我们使用 `ctrl + z` 作为命令前缀，在  vim 里 `ctrl + d` 是向下滚动窗口，但是在 mac 里面是 debug。我们要达到的效果是 `ctrl + d` 是 vim 的滚动窗口，而按 `ctrl + z` 再 `ctrl + d` 就启动了 idea 的 debug 效果配置如下：

   ```  nnoremap <C-Z><C-D> :action Debug<CR> ```

这样就可以避免快键冲突了。

另外要学习 vim 强烈推荐一本书 [《vim 实用技巧 第 2 版》](https://book.douban.com/subject/25869486/)。正是这本书教我怎么使用 vim 的，里面有很多示例，超赞。

参考

https://stackoverflow.com/questions/31970413/escape-to-intellij-idea-shortcuts-from-ideavim
