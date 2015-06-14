title: 用Hexo 3 搭建github blog
date: 
categories: hexo
tags: [git,blog,hexo]
---



Hexo更新到3后很多设置都变更了,网上大多数教程都是面向Hexo2的, 自己在建立blog的时候遇到了一些问题,现在记录下来

##Hexo安装前置条件
这部分不详述, 都是通用的, 有很多教程可以参考
* git安装
    * For Windows
    Download & install [msysgit][].
    * For Linux
       * Ubuntu,Debian: sudo apt-get install git-core
       * Fedora,Red Hat, CentOS: sudo yum install git-core
    * For mac
      用[Homebrew][]/[MacPorts][]/[Installer][]安装
* Node.js安装
	* [安装地址][Node.js主页]

##[Hexo][]安装


前置准备完成后

```bash
	$ npm install -g hexo-cli
```
>在OS X上面安装完毕之运行hexo时可能会提示这样的错误:
>
>```
[SyntaxError: Unexpected token ILLEGAL]
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```

>安装时,可以用`npm install hexo --no-optional`来解决
>
>详情参见[issues1055][]

在Hexo3中,hexo-server独立出来了,如要本地调试,需要先安装

```bash
$ npm install hexo-server --save
```

其余独立出来的还包括下面这些,特别是hexo-deployer-git这个模块,不安装的话后面在运行`hexo deploy`时会提示找不到git,参见这个[issues1040][].

>详细的[Hexo2to3][]信息




```bash
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```



可以到\node_modules目录下面看自己是不是已经安装了这些模块.

如果已经执行过[后面这个步骤](#部署)了,那么进入hexo所在的文件夹,来安装缺失的模块,如下:
```bash
		$ cd <hexo_folder>
		$ npm install <module_2bInstalled> --save
```


##Hexo部署
* <A ID="部署"> </A>安装完成之后运行如下命令 
	
	```bash
		$ hexo init <folder>
		$ cd <folder>
		$ npm install
	```
	
>若npm install 安装过程卡住不动
>修改 npm 的安装目录下的 npmrc文件 增加一条 `registry=http://registry.cnpmjs.org`
>
>
>```bash
$ npm config set registry http://registry.cnpmjs.org
```
	

* 生成静态文档
	```
		$ hexo generate
	```
* 开启本地server调试
	
	```
		$ hexo server
	```
打开localhost:4000则可以在本地看到生成的页面

* 上传到Github
	在config.yml文件中配置好`deploy`参数:
	
	```
	deploy:
  		type: git
  		repo: <repository>
	```
	
* 然后执行

	```
		$ hexo deploy
	```

##编写博客
编写博客比较简单,教程很容易找,官方文档见[hexo写作][].
下面聊聊如何实现多处来同步编辑博客,主要有如下几个方案

* VPS
    
    VPS方案需要一台VPS主机,在上面配置好Hexo所需要的环境,SSH到主机编辑同步即可

* 云coding环境
	
	比较有名的有[Koding][],[Cloud9][].该方案类似于主机,好处是可以直接在线编辑,koding平台在国内貌似被墙.
	参见[koding非教程贴][]
* github管理

	直接用把Blog所在的文件夹当成一个git库来同步,好处是有git就行了,但是在想要编辑blog的机器上都要配置Hexo所需的环境,几个需要注意的事项参见[issues752]:
	*  如果主题是通过git管理的，需要将主题文件夹下的.git文件夹删除，才能同步Blog文件夹（.git文件夹是隐藏的，需要显示隐藏文件才能删除，Linux下需要`rm -rf`命令才能删除，Mac没用过，不清楚）。
	* 按照Blog目录下自带的.gitignore文件,node_modules文件夹是不会同步的,所以同步之后需要自己再次进行`npm install`，但是注意，不要进行`hexo init`了，否则\_config.yml全都白弄了。
	

##更换主题
hexo可用[主题列表][]
其中[pacman][]和[Jacman][]个人比较喜欢,但是pacman目前还不支持Hexo3,代码高亮会有问题.

更新方法如下:

* 执行如下命令
    
    ```
            git clone https://<git/repository> themes/<themename>
    ```
    
* 将`_config.yml`文件中的`theme`字段设置为与 `<themename>`一致
* 更新主题
	
	更新前要记得备份`themes/<themename>` 目录下的`_config.yml`		
	
	```
		cd themes/<themename>
		git pull
	```

[Node.js主页]: https://nodejs.org/
[msysgit]: http://msysgit.github.io/
[Homebrew]: http://brew.sh/
[MacPorts]: http://www.macports.org/
[Installer]: http://sourceforge.net/projects/git-osx-installer/
[Hexo]: http://hexo.io/docs/ (hexo文档地址)
[主题列表]: https://github.com/hexojs/hexo/wiki/Themes
[issues1040]: https://github.com/hexojs/hexo/issues/1040
[Hexo2to3]: https://github.com/hexojs/hexo/wiki/Migrating-from-2.x-to-3.0
[issues1055]: https://github.com/hexojs/hexo/issues/1055
[hexo写作]: http://hexo.io/zh-cn/docs/writing.html
[Jacman]: http://wuchong.me/jacman/2014/11/20/how-to-use-jacman/
[pacman]: http://yangjian.me/pacman/hello/introducing-pacman-theme/
[koding非教程贴]: http://hahack.com/tools/koding_intro/
[Cloud9]: https://c9.io/
[Koding]: https://koding.com
[issues752]: https://github.com/hexojs/hexo/issues/752