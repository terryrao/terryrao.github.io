# github + hexo 搭建个人博客
;date: 2019-02-17 10:38:32
;tags:

使用github pages 和 hexo 搭建一个个人博客网站。[github pages](https://pages.github.com/) 是 github 公司提供的免费的静态网站托管服务。从主页进去我们可以看到它主要有两个用途：作为个人或者组织的站点或者作为项目的站点，因此我们可以用这个服务来搭建自己的博客，使用 jekyll 或者 hexo 等工具可以创建自己风格的主题。

# 1. github pages

搭建过程在 [github pages](https://pages.github.com/) 上有详细的介绍，首先注册一个 github 账号，然后在自己的电脑里下载 git 。

 1. Windows

    Windows 系统下可以下载 [github desktop](https://desktop.github.com) 也可以下载其它的 git 工具，不过由于其它原因可能比较慢，最好是使用代理或者在国内的云盘如百度云里找个可用的安装包来安装。

 2. Mac 

    Mac 用户可以使用 brew install git 或者直接下载安装包安装，如果提示需要安装 Command Line Tools 请先在 appStore 里下载 Xcode，然后进入 Preferences -> Download -> Command Line Tools -> Install 安装命令行工具。

 3. Linux 

    Sudo yum install git-core 或者 apt-get install git

### 创建自己的 github pages

1. 创建仓库

转到 [GitHub](https://github.com/) [创建一个新的仓库](https://github.com/new)，将名称命名为 yourusername.github.io 。需要注意的是这个 yourusername 必须是你的用户名，并且区分大小写，如图：

![建立新库](https://github.com/terryrao/picturebed/raw/master/blogs/1550389323701.jpg)

图上显示的警告是因为我已经有该库，选择库的类型为 public ，然后直接创建，就有一个新库了。

### 本地 git 的使用

1. 将公钥上传到 github 上

   在 mac/linux 上 进入自己的 home（以自己的用户命名的目录） 目录，使用以下命令

   ```bash
   ~ ssh-keygen
   
   ```

   然后一路按 enter 键下来。 然后切换到 .ssh 目录来 就可以看到一个叫 id_rsa.pub 的文件，将里面的本文 copy 出来，切换到 github 页面上，点击用户图像 ，点击 settings 进入配置页，点击 SSH and GPG keys -> new SSH key

   ,取个名保存即可，如图所示：

   ![我的设置](https://github.com/terryrao/picturebed/raw/69481d454f/blogs/1550542503010.jpg)

   ![SSH Key](https://github.com/terryrao/picturebed/raw/69481d454f/blogs/WX20190219-101744_2x.png)

   >  参考： [git-scm 生成 SSH 公钥](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5) 

2. 上传内容

   找到自己建立的库，找到 clone or download  ，复制 `git@github.com:yourusername/terryrao.github.io.git` 到粘贴板。然后打开本地的 命令行窗口，输入以下内容:

   ```bash
    git clone git@github.com:yourusername/terryrao.github.io.git
    cd yourusername.github.io
    echo "Hello World" > index.html
    git add --all 
    git commit -m "Initial comment" 
    git push -u origin master
   
   ```

   Push 成功后到自己的 github 的 yourusername.github.io 里查看是否有刚才提交的 index.html 页面，如果有就代表 push 成功 。然后在浏览器里输入 yourusername.github.io 看到 `hello world` 两个字就代表成功了，接下来就使用 hexo 写博客了。

# 2. Hexo

[Hexo](https://hexo.io/zh-cn/docs/index.html) 是一个快速、简洁的博客框架，使用 markdown 解析文件，生成静态页面。Hexo 是基于 nodejs 开发的，因此安装之前需要先安装 nodejs。

1. 安装 nodejs

网上有很多教程，对于 windows 可以直接在 [nodejs](https://nodejs.org/) 的官网上下载安装包直接安装。对于 mac/linux 推荐使用 [nvm](https://github.com/creationix/nvm) 安装。下面介绍一下 nvm 安装的步骤：

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
nvm install  stable
```

2. 安装 hexo

安装 nodejs 之后就可以使用 npm 来安装 hexo 了，如下：

```bash
npm install -g hexo-cli
```

3. 初始化 hexo

在自己习惯的目录下新建一个文件夹，如 my_blogs ，切换到 my_blogs 目录，使用以下命令初始化 hexo 环境。

```bash
mkdir my_blogs
hexo init my_blogs
cd my_blogs
npm install
```

之后就会看到生成的一些文件和文件夹，表示已经初始化好环境了，接下来就可以写作了

4. 新建文章

```bash
hexo new "你的文章名称"
hexo g
hexo s
```

hexo new 命令完成之后，新建的文件就在 source/_post下，使用文本编辑器，以 markdown 的格式写入文本，保存文件。然后使用 hexo g 命令生成 文件，最后用 hexo s 启动 web 服务 ，在浏览器中输入 http://localhost:4000/ 就可以看到刚才输入的内容了。

5. 添加主题

[Hexo themes]( https://hexo.io/themes/) 官网上有很多模板，可以选择自己喜欢的模板，进入到它的 github 连接里，使用 git clone 命令下载到 theme/ 目录下：

```bash
hexo clean
cd my_blogs/themes
git clone git@github.com:Haojen/hexo-theme-Anisina.git
```

成功后就会看到 themes 下多了个 hexo-theme-Anisina 目录，这个目录就是刚才下下来的主题。切换到 my_blogs 目录，打开 _config.yml 文件，找到 theme ，改为刚下载的主题：

```yml
theme: hexo-theme-Anisina
```

然后直接运行命令 hexo g ，再打开 http://localhost:4000 查看效果。

6. 部署文章到 github 上

进入 my_blogs 目录  打开 `_config.yml` 文件，在 `deploy` ，type 修改为 git ，添加自己的 yourusername.github.io 库。

```yml
deploy:
  type: git
  repo: git@github.com:terryrao/terryrao.github.io.git
  branch: master

```

然后使用命令：

```bash
hexo d
```

就可以将生成好的静态文件发布到 github 上去了。

7. 添加 gitment

Gitment 
