# vscode python venv 环境无法调度的问题
;date: 2019-05-06 00:24:08
;tags:

学习 python 的时候使用的是 pycharm ，后来听说 vscode 也相当不错，基于尝鲜的想法下载了一个 vscode 试着学习一下 python 。一般教程里学习 python 的时候基本都都会说要使用虚拟环境来隔离，而且我用的 mac 自带 2.7 为了防止意外下载包破坏系统环境，因此一般使用的时候虚拟环境，因为 pycharm 建立工程的时候默认会创建一套虚拟环境，所以之前没有关注它。这此配置 vscode 的环境时就出问题了。

### 设置环境

其实就是按照[官网](https://code.visualstudio.com/docs/python/environments)的步骤下来，下载插件，建立工程，然后运行 debug。运行 helloWord 程序无问题，但是 debug 就是无法进入断点。找了好几个小时依然找不找问题所在。后来看到 github 上有人已经提过这个问题。仔细看了一下才发现是目录的问题。我是按 [python 官网教程](https://docs.python.org/zh-cn/3/tutorial/venv.html)建立的虚拟环境:

```shell
python3 -m venv tutorial-env
```

我是以这个命令建立了一个目录，这个时候我使用的是 tutorial-env 作为项目的根目录：

```
 tutorial-env 
 -- bin
 -- lib
 -- pyvenv.cfg
```

然后我在里面建立了一个 hello.py 的文件。然后选择 venv 的 python 解释器。这样就一直调试不了(pycharm 可以)。[github](https://github.com/Microsoft/vscode-python/issues/2470) 上作者给出了答案

​		That's working as expected then. You need to go up a level in your directory structure and then Python will find the virtual environment (i.e. open the directory that contains your 'env' directory, not 'env' itself). Virtual environment directories aren't not meant to be worked in.

大意就是说，打开一个包含虚拟环境的目录，而不是在 venv 里面。按照这个我重新建立了一个 test_python 目录，进入到里面创建 venv 环境。

```shell
mkdir test_python
python3 -m venv venv
echo "print('hello world')" >> hello.py
```

这一次运行 debug  就成功了。后来我继续读了一下官网的教程，发现里面写着推荐使用以下命令生成虚拟环境：

```shell
python3 -m venv .venv
```

不仔细看文档惹的祸。这个故事告诉我们要好好学习英文，认真看文档。😂
