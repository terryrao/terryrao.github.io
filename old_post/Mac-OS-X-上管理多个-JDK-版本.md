# Mac OS X 上管理多个 JDK 版本
; date: 2019-03-14 11:52:38
; tags:

### JDK 下载安装



JDK 可以在 oracle 官网上下载，直接运行 .dmg 文件即可。安装完毕后，可以在本地的 `/Library/Java/JavaVirtualMachines` 目录下看到自己安装的版本，使用 java 命令查看一下是否安装完毕：

```bash
java -version
java version "11.0.2" 2019-01-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)
```

### 修改 .bash_profile 或者 .zshrc 

找到自己的 jdk 路径，然后修改 `.bash_profile` ，zsh 修改 .zshrc，按自己的目录输入以下内容：

```bash
# jdk 8
export JAVA_8_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_191.jdk/Contents/Home"
# jdk 11
export JAVA_11_HOME="/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home"
# 默认jdk
export JAVA_HOME=$JAVA_11_HOME
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
alias jdk11="export JAVA_HOME=$JAVA_11_HOME"
```

然后 输入`source ~/.bash_profile ` 或者 `source ~/.zshrc` ，输入以下命令验证刚才的配置是否成功：

```bash
~ jdk8
~ java -version
  	java version "1.8.0_191"
  	Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
  	Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)

~ jdk11
~ java -version
	java version "11.0.2" 2019-01-15 LTS
	Java(TM) SE Runtime Environment 18.9 (build 11.0.2+9-LTS)
	Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.2+9-LTS, mixed mode)
```

### 原理

Mac OS X 已经内置了管理 jdk 版本的工具，通过 `/usr/libexec/java_home` 命令追踪可以查看当前的 `$JAVA_HOME` 。

```bash
~ /usr/libexec/java_home
	/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
~ /usr/libexec/java_home -V
	Matching Java Virtual Machines (2):
   	 	11.0.2, x86_64:	"Java SE 11.0.2"	/Library/Java/JavaVirtualMachines/jdk-				11.0.2.jdk/Contents/Home
    	1.8.0_191, x86_64:	"Java SE 8"					       /Library/Java/JavaVirtualMachines/jdk1.8.0_191.jdk/Contents/Home
```

参考：

https://www.jianshu.com/p/af79ae7f732c

https://segmentfault.com/a/1190000013131276
