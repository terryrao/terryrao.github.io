# python3 install webpy
; date: 2019-03-28 16:21:15
;tags:

学习 [web.py](<http://webpy.org/tutorial3.zh-cn>) 时，遇到 web.py 安装的问题。

Python 3 环境下，如果直接使用 pip install webpy 会报错。在网上查了一下，需要指定 webpy的版本：

```bash
pip install web.py==0.40.dev0
```

这个时候直接运行，也会报错：

```bash
···\web\utils.py", line 526, in take
    yield next(seq)
StopIteration
```

网上给出的解决方法是把当前环境下的 utils.py 的报错行替换成下面的语句：

```python
## 将此行 yield next(seq) 替换成下面语句 
try:
    yield next(seq)
except StopIteration:
    return
```

然后再运行，就可以正常启动了。

```bash
/pytest/testPy.py
http://0.0.0.0:8080/

```



参考：

[[“RuntimeError: generator raised StopIteration” every time I try to run app](https://stackoverflow.com/questions/51700960/runtimeerror-generator-raised-stopiteration-every-time-i-try-to-run-app)]
