---
layout: post
title:  "github+Jekyll搭建个人主页细节以及一些坑"
date:   2021-04-02 17:14:54
categories: jekyll
tags: jekyll RubyGems 
---
* * content
{:toc}
## 主体流程

* 使用来自[Gaohaoyang](https://github.com/xudailong/xudailong.github.io) 的模板

* 参照：[Gaohaoyang](https://github.com/xudailong/xudailong.github.io)  这是博主的github开源项目，但现在中文版安装流程打不开，可以参照 [中文版链接](https://blog.csdn.net/xudailong_blog/article/details/78762262)  这个链接中介绍的比较详细，并且是中文 。       
* 但是不管是博主的，还是介绍的时间都比较久了，各种软件的版本已经更新多代，搭建环境的过程中遇到了一些文中没有的坑，还有一些没接触过的细节。          

* 配置本地环境Ruby-jekyll的目的是方便在本地修改展示效果直到确保正确再push。	  	

## 坑

1. 无法加载webrick  
`servlet.rb:3:in 'require': cannot load such file -- webrick (LoadError)`  
错误出现在启动jekyll服务，也就是控制台输入`jekyll serve`  ,结果如下：  
```
/home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `require_relative'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `setup'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:100:in `process'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `each'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
	from /home/argilo/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
	from /home/argilo/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
	from /home/argilo/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
	from /home/argilo/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
	from /home/argilo/.gem/ruby/3.0.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
	from /home/argilo/.gem/ruby/3.0.0/gems/jekyll-4.2.0/exe/jekyll:15:in `<top (required)>'
	from /home/argilo/.gem/ruby/3.0.0/bin/jekyll:23:in `load'
	from /home/argilo/.gem/ruby/3.0.0/bin/jekyll:23:in `<top (required)>'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:63:in `load'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:63:in `kernel_load'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli/exec.rb:28:in `run'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:497:in `exec'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:30:in `dispatch'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/cli.rb:24:in `start'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/bundler-2.2.3/libexec/bundle:49:in `block in <top (required)>'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/3.0.0/bundler/friendly_errors.rb:130:in `with_friendly_errors'
	from /home/argilo/.rubies/ruby-3.0.0/lib/ruby/gems/3.0.0/gems/bundler-2.2.3/libexec/bundle:37:in `<top (required)>'
	from /home/argilo/.rubies/ruby-3.0.0/bin/bundle:23:in `load'
	from /home/argilo/.rubies/ruby-3.0.0/bin/bundle:23:in `<main>'
```
* 原因：Ruby3.0 的gem中不再绑定webrick
* 解决方案：控制台输入`bundle add webrick`，将webrick作为一个依赖
* 参照：<https://github.com/jekyll/jekyll/issues/8523>  

2. 缺少其他的gem   
* 解决方案：`gem install 包名`  
3. github的上传问题  
	之前用github基本上只下载没有试过push，今天发现了一个问题：`OpenSSL SSL_read: Connection was reset, errno 10054`，这种错误clone时可能也会出现
* 原因：服务器的SSL证书没有经过第三方机构的签署
* 解决方案：`git config --global http.sslVerify "false"`   
4. 上传的markdown中的时间要求  
* 不能使用当前日期，比如今天是2021.04.03， 你设置的2021-04-02，如果时间点过了12：00：00会四舍五入显示下一天，但是没到下一天，所以这个文件的时间错误不会在网站中展示；如果使用当天12点之前的时间点也不行
* 设置2021-04-02 12:00:00之后会显示2021-04-03，12点之前显示2021-04-02  

##  一些细节（我是新手）

1. 统计ID  
统计ID用于展示你的网站点击量等。
*  [百度统计](https://tongji.baidu.com/web/10000342251/welcome/login)
  流程：登录百度统计——选择“管理”标签——在左侧导航中选择“代码管理->代码获取“——右上角新建网站——代码中形如`//hm.baidu.com/hm.js?xxxxxxxxxxxx`?后的部分就是ID
*  [谷歌统计](https://analytics.google.com/analytics/web/#/)
  流程：登陆——新建网站——衡量ID  
2. Disqus评论  
   	用户使用Disqus，在不同网站上评论，无需重复注册账号，只需使用Disqus账号或者第三方平台账号，即可方便的进行评论，且所有评论都会存储、保存在Disqus账号后台，方便随时查看、回顾。  
   	而且，当有用户回复自己的评论时，可以选择使用邮箱接收相关信息，保证所有评论的后续行为都可以随时掌握。  
   	流程：注册时要注意，填入的用于注册的name就是你的short_name。  
3. 更改网站图标  
   	我是用画图自己简单设计再用[这个网页](http://www.bitbug.net/ )转为ico格式。

## 希望对各位有帮助!

~~这里有很多东西都没有接触过，ruby、jekyll、minGW、disqus，挖个坑以后有机会来填。~~
​    




