利用 Github :couple: Hexo 制作个人博客

---

> ps
> 我用Docker构建了Hexo环境的image: `centos7+git+nodejs`
> `docker pull zhang21/hexo:init`


<br>


# 参考文档

- [Hexo官方文档](https://hexo.io/zh-cn/docs/index.html);
- [NexT使用文档](http://theme-next.iissnan.com/getting-started.html);
- [GitHub:hexo-theme-next](https://github.com/iissnan/hexo-theme-next);
- [FaontAwsome图标](http://fontawesome.dashgame.com/);
- [知乎专栏:利用GitHub+Hexo搭建个人网站](https://zhuanlan.zhihu.com/p/26625249);
- [Hexo Troubleshooting](https://hexo.io/docs/troubleshooting.html).

**NexT美化及集成第三方服务，都可以参考此NexT官方文档，很重要！**

<br/>

# 关于

**About [Hexo](https://hexo.io/zh-cn/)**

- Hexo是一个快速、简介且高效的博客框架，由Node.js提供支持；
- Hexo使用Markdown解析文章，在几秒内，即可利用靓丽的主题生成静态网页；
- 安装Hexo前需先装Node.js和Git，安装和配置详见各官方文档。


<br/>

**About [GitHub](https://zh.wikipedia.org/wiki/GitHub)**

- GitHub是一个通过git进行版本控制的源代码托管服务；
- 安装Git后做一个SSH无秘钥认证，把公钥放在GitHub上，避免每次推送代码需要输入账号密码;
- 生成秘钥之后把公钥写入GitHub SSH Keys中。

```
git config --global user.name "GitHub-username"
git config --global user.email "GitHub-Email"
ssh-keygen -t rsa -C "GitHub-Email"
```



<br/>

**About [Node.js](https://zh.wikipedia.org/wiki/Node.js)**

- Node.js是一个能够在服务器端运行JavaScript的开放源代码、跨平台JavaScript运行环境；
- Node.js会包含环境变量和npm的安装。



<br>
<br/>

# 安装NexT主题

- [NexT-github](https://github.com/iissnan/hexo-theme-next)
- 安装完成后在全局配置里面修改主题名  `theme: next`
- NexT配置详见：[NexT使用文档](http://theme-next.iissnan.com/getting-started.html)

```
cd ./themes
git clone https://github.com/iissnan/hexo-theme-next themes/next
./theme/next/_config.yml为配置文件
 ```


<br>
<br/>

# 关于NexT的几个注意事项

<br>

***强烈推荐[查阅 NexT网站说明](http://theme-next.iissnan.com/)和[NexT-issues](https://github.com/iissnan/hexo-theme-next/issues)***


**有关户名下面的几个menu点击后没有页面或者是空白页的问题:**

```sh
#Create a menu page
hexo new page "tags"


#Edit menu page,set page type to menu-item in ./tags/index.md file:
type: "tags"    #这就是空白页的原因


#Add tags to theme _config.yml:

menu:<br/>
   home: /
   tags: /tags
   menu-item: /menu-item

#其他menu都是一样的方法
```

<br>

![](/images/menu.png "NexT-menu")


<br/>

**有关添加背景图片:**

将背景图片放置于`blog/themes/next/source/images/bg.jpg`，建议图片都放置于此。

修改`blog/themes/next/source/css/_custom/custom.styl`，在首部添加：

```
<!--前端知识-->
.header（body,sidebar具体位置取决于你想放在哪） {
    background: url(/images/bg.jpg); <!--网络地址也可以-->
    background-attachment: fixed(固定背景图片)；
}
```


<br/>

**有关动态效果:**

在主题配置文件 _config.yml 中：

```
参见 canvas 这个属性
```

<br>

![](/images/sidebar.png)


<br/>

**有关加头像:**

在NexT主题配置文件 _config.yml中:

```
avatar: /images/touxiang.png    #建议放在images目录下
# vatar: http://example.com/avatar.png
# URL既可以是本地地址可以是网络地址
```


<br>

**有关外部链接的标志:**

在NexT主题配置文件 _config.yml中:

```
social:
  E-mail: xxx@example.com || envelop
  GitHub: https://github.com/xxx || github
  Weibo: https://weibo.com/xxx || weibo
```

关于国外的这些软件的图标在FontAwsome中都已经存在，但国内的某些应用(如微信、知乎、微博、  豆瓣等还需要在FontAwsome的 "标志" 上看看存在与否，以及图标对应的名称是什么。

*支持的图标参考 [FontAwsome官网](http://fontawesome.dashgame.com/)*


<br/>

**有关插入网易云音乐链接:**

选择一首歌，生成对应的 HTML 代码，注意是否自动播放，并将代码添加到:

```sh
/blog/themes/next/layout/_macro/sidabar.swig文件中
`{% include '../_custom/sidebar.swig' %}代码上面`
![](/images/163music1.png)
![](/images/163music2.png)
```


<br/>

**有关MathJax支持:**

修改主题配置`_config.yml`文件: `参考MathJax选项`


<br/>

**有关添加统计:**

我使用了推荐的：`不蒜子统计`



<br/>

**有关评论系统:**

我使用了推荐的：`Disqus`


<br>

**添加搜索功能：**

1. 安装`hexo-generator-searchdb`，在站点根目录下：

```
npm install hexo-generator-searchdb --save
```


2. 编辑站点配置文件`_config.yml`，新增一下内容到任意位置(我放在最后)：

```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

3. 编辑主机配置文件`_config.yml`，启用本地搜索功能：

```
local_search:
  enable: true
```



<br/>
<br>

# 创建和部署文章

```
#新建post
hexo n "博客名字"

#生成
hexo g

#本地server
hexo s

#部署到github
hexo d
```



