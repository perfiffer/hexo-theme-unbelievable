**预览图**
+ 首页1
![](https://linux-learning.cn/blog_img/preview/index1.png)
+ 首页2
![](https://linux-learning.cn/blog_img/preview/index2.png)

### 写在前面

博客源码在主题 [amazing](https://github.com/removeif/hexo-theme-amazing)的基础上进行了修改，`amazing` 是基于 [icarus](https://github.com/ppoffice/hexo-theme-icarus) 主题进行了修改，有兴趣的可以去查看最基础的版本，尝试修改。
感谢所有模块的原作者,orz👻,辛苦了。  
**使用之前请详细阅读此文档，以及主题配置文件_config.yml**  
**使用之前请详细阅读此文档，以及主题配置文件_config.yml**  
**使用之前请详细阅读此文档，以及主题配置文件_config.yml**  
**相关使用问题也在此[issue](https://github.com/perfiffer/hexo-theme-unbelievable/issues/1)有说明，请先查看**

线上博客：[欢迎围观](https://linux-learning.cn/)

### 一、amazing 主题之上主要改动
+ 修改了头部导航栏，增加了“相册”、“书籍”导航栏，原“碎碎念”导航栏名称更改“锋言疯语”
+ 修改了看板娘的配置
+ 一些显示样式的修改

### 二、部分配置说明：

#### 本机环境：
```jshelllanguage
$ node -v
v17.4.0
$ npm -v
8.3.1
```  
#### 在博客目录下clone主题代码
```jshelllanguage
git clone https://github.com/perfiffer/hexo-theme-unbelievable.git themes/unbelievable
```
#### 开始部分配置：
**敲黑板！！！！首先全局以及主题中的`_config.yml`配置成自己的对应参数。**  

把主题中ex_pages文件夹中的文件复制到博客主目录相应目录下面。包含了文章模板、关于页、相册页、留言板、锋言疯语、书籍、音乐页面（各个页面的.md文件可自定义修改内容），可以自己选择性需要哪些页面复制哪些过去，同时对应配置主题中`_config.yml`需要删除以及保留相应的menu，如下配置
```yaml
navbar:
    # Naviagtion menu items
    menu:
        首页: /
        归档: /archives
        分类: /categories
        标签: /tags
        相册: /album
        锋言疯语: /self-talking
        留言: /message
	书籍: /book
	音乐: /music
        关于: /about
```
#### 1.热门推荐，最新评论：
**热门推荐仅支持gitalk，最新评论支持gitalk & valine**
对应主题中的`_config.yml`要开启如下配置（此为gitalk，valine配置文件中也有示例），xxx换成自己的，否则无效。**对于gitalk部署博客后需要到相应文章评论处点击初始化issue评论，完成评论的初始化。**
```yaml
comment:
    type: gitalk
    owner: xxx         # (required) GitHub user name
    repo: xxx          # (required) GitHub repository name
    client_id: xxx     # (required) OAuth application client id
    client_secret: xxx # (required) OAuth application client secret
    admin: xxx  #此账户一般为用户名 GitHub user name 文章中能创建issue需要此用户登录才可以，点了创建issue后刷新一遍才能看到！！！！
    create_issue_manually: true
    distraction_free_mode: true
    has_hot_recommend: true # 是否有热门推荐
    has_latest_comment: true #是否有最新评论
```
说明：
+ `has_hot_recommend: true` 是否开启首页热评，false-不开启，true-开启
+ `has_latest_comment: true` 是否开启最新评论，false-不开启，true-开启
+ 热门推荐数据为评论数最多的文章，🔥后面的数字：根据文章的评论数*101 。  
+ 最新评论：为该仓库下，所有issue中的最新评论。  
+ 目前的最新评论有1分钟的本地缓存，评论后可能1分钟后才能看见最新评论，出于性能优化，每次请求接口处理还是挺耗时，comment-issue-data.js中可以自己去掉。  

#### 2.关于页面时间轴记录数据文件：
文件路径：themes/unbelievable/source/json_data/record.json   
相应格式增加自己需要的数据。

#### 3.看板娘配置
主题中的`_config.yml`配置如下设置
```text
has_live_2D_switch: true #live2D开关 true为打开,false为关闭
```

#### 4.书籍数据配置
文件路径：themes/unbelievable/source/json_data/book.json
按照相应格式增加自己需要的数据。

#### 5.置顶设置：
.md文章头部数据中加入top值，top值越大越靠前，大于0显示置顶图标。
修改依赖包中文件 node_modules/hexo-generator-index/lib/generator.js 如下：
```js 
'use strict';
const pagination = require('hexo-pagination');
module.exports = function(locals){
    var config = this.config;
    var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top == undefined){
            a.top = 0;
        }
        if(b.top == undefined){
            b.top = 0;
        }
        if(a.top == b.top){
            return b.date - a.date;
        }else{
           return b.top - a.top;
        }
    });
    var paginationDir = config.pagination_dir || 'page';
    return pagination('', posts, {
        perPage: config.index_generator.per_page,
        layout: ['index', 'archive'],
        format: paginationDir + '/%d/',
        data: {
            __index: true
        }
    });
};
```
#### 6.配置文章中推荐文章模块  
根据配置的recommend值（必须大于0），值越大越靠前，相等取最新的，最多取5条。recommend（6.中top值也在下面示例）配置在.md文章头中，如下  
```yaml
title: 博客源码分享
top: 1
toc: true
recommend: 1 
keywords: categories-github
date: 2019-09-19 22:10:43
thumbnail: https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20190919221611.png
tags: 工具教程
categories: [工具教程,主题工具]
```
#### 7.文章中某个代码块折叠的方法
代码块头部加入标记 `>folded`，如下代码块中使用。
```java main.java >folded
    // 使用示例，.md 文件中头行标记">folded"
    // ```java main.java >folded
    // import main.java
    // private static void main(){
    //     // test
    //     int i = 0;
    //     return i;
    // }
    // \\``` 
```
#### 8.加入加密文章
如下需要加密的文章头部加入以下代码
```text
---
title: 2019成长记01
top: -1
toc: true
keywords: categories-java
encrypt: true
password: 123456 #此处为文章密码
abstract: 咦，这是一篇加密文章，好像需要输入密码才能查看呢！
message: 嗨，请准确无误地输入密码查看哟！
wrong_pass_message: 不好意思，密码没对哦，在检查检查呢！
wrong_hash_message: 不好意思，信息无法验证！
---
```
注：**加密文章不会出现在最新文章列表widget中，也不会出现在文章中推荐列表中，首页列表中需要设置top: -1 让它排在最后比较合理一些。**
#### 9.锋言疯语的使用
在github中，创建碎碎念issue，并且打上对应的label（`eg:Gitalk,666666`）如下图，此处666666对应下面配置代码中的id，填写到：博客目录/source/self-talking/index.md文件中如下对应位置，其余配置也要改成自己的，如clientID等。
![](https://linux-learning.cn/blog_img/self_talking/issue.png)
```js
<script>
    $.getScript("/js/gitalk_self.min.js", function () {
        var gitalk = new Gitalk({
            clientID: '46a9f3481b46ea0129d8',
            clientSecret: '79c7c9cb847e141757d7864453bcbf89f0655b24',
            id: '666666',
            repo: 'issue_database',
            owner: 'perfiffer',
            admin: "perfiffer",
            createIssueManually: true,
            distractionFreeMode: false
        });
        gitalk.render('comment-container1');
    });
</script>
```
如下：
![锋言疯语](https://linux-learning.cn/blog_img/self_talking/demo.png)
#### 10.本博客样式（透明无界）
只需要放开themes/unbelievable/source/css/base.styl文件中以下样式代码注释即可，默认是注释的没启用
```css 
//=================本博客使用样式   start

// 首页去图
.body_hot_comment .comment-content .card-comment-item .ava, .media-left, .is-6-widescreen .card-image {
    display: none;
}

hover-color = #deeafb;
// 去card
.card {
    background-color: unset;
    box-shadow: unset;
}

.navbar, footer.footer {
    background-color: unset;
}

body:not(.night) .navbar:hover,
body:not(.night) .footer:hover,
body:not(.night) .card:hover,
body:not(.night) .pagination:hover,
body:not(.night) .post-navigation:hover{
    background-color: hover-color;
    box-shadow: 0 4px 10px rgba(0,0,0,0.05),0 0 1px rgba(0,0,0,0.1);
}

.pagination, .post-navigation{
    padding: 10px;
}

.pagination .pagination-link:not(.is-current), .pagination .pagination-previous, .pagination .pagination-next {
    background-color:rgba(255,255,255,0);
}

.timeline .media:last-child:after {
    background: unset;
}
.content .gt-container .gt-comment-admin .gt-comment-content {
    border: 2px solid #deeafb;
}

//=================本博客使用样式   end
```
#### gitalk评论增加评论开关，评论列表中标记博主
需要关闭评论的在文章头部加入 `comments: false`,原来已经评论的依然会显示，如下

#### 其余配置
完整配置，请仔细阅读主题中**_config.yml**
```yaml
has_hitokoto: true #左边一言开关，true-开，false-关 
has_latest_modify_time: true #是否显示最后修改时间 true开启，false-关闭   
busuanzi_only_count: false #当上面plugins中busuanzi: true时，此配置busuanzi_only_count为true时，网站不显示不蒜子统计数据，但是会每次统计。false时显示统计数据。
has_copyright: true # 文中是否显示copyright true开启，false-关闭   
# http://sachinchoolur.github.io/lightGallery/docs/api.html 
lightgallery_is_full: true #图片灯箱是否显示完整的插件(包括放大，分享等)，true-显示，false-显示简洁版
website_start_time: 2018/11/11 00:00:00 #网站运行开始时间,不填不显示
footer_registered_no: 测试-川ICP备20001070号-1 #备案号
footer_copyright_dsec: true #footer 版权说明 true-开 false-关
has_live_2D_switch: true #live2D开关 true-开 false-关
side_music_netease_id: 2364053447 #侧边栏网易云歌单id
use_pjax: false #是否开启pjax，false-不开启，true-开启，开启后局部更新网页信息，切换页面背景音乐不间断等特性
```
#### 以上配置好后
```yaml
$ npm install #安装依赖包（只需要执行一次）可直接把本文最后的json文件内容复制到博客下面的依赖文件package.json后在执行此命令，如果原来已有node_modules文件夹，请先删除在执行此命令
$ hexo clean #清除缓存
$ hexo g #编译 
$ hexo s #启动服务 
$ hexo d #推到远程 
```
安装依赖包（只需要执行一次），以后修改了代码 只需要执行后面几条就好。  

### 写在后面
如果你有问题请反馈: [issues](https://github.com/perfiffer/hexo-theme-unbelievable/issues) （请务必先于issues中寻找答案）  
如果你喜欢该主题: [star](https://github.com/perfiffer/hexo-theme-unbelievable)  
如果你想定制主题: [fork](https://github.com/perfiffer/hexo-theme-unbelievable) 
### License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/perfiffer/hexo-theme-unbelievable/blob/master/LICENSE) file for details.

### 其余主题彩蛋
**文章中横竖图demo；对于横竖图推荐分开使用，且长宽一致的，如统一手机拍照、电脑截图**
使用方法：md文章中放入以下代码
```html

+ 横竖图

<div class="justified-gallery">

![张芷溪](http://wx1.sinaimg.cn/large/b5d1b710ly1g6bz7n92s7j212w0nr1kx.jpg) ![李一桐](http://wx2.sinaimg.cn/mw1024/005RAHfgly1fvfc4f19qfj33402c0qv9.jpg) ![gakki](http://wx1.sinaimg.cn/mw1024/70396e5agy1g5qe457i6yj21660ogtap.jpg) ![李一桐](http://wx1.sinaimg.cn/mw1024/005RAHfgly1fuzz17s2q3j32e43cku0x.jpg) ![彭小苒](http://wx1.sinaimg.cn/mw1024/d79c9b94ly1g1pb1uthr5j21f02iox6t.jpg)</div>

+ 横图4

<div class="img-x">

![v4](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191022182226.png) ![v3](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191018114126.png) ![v4](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191022182226.png) ![v3](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191018114126.png)</div>

+ 竖图5

<div class="img-y">

![电池](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191024145940.jpg) ![打王者荣耀](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191024141906.jpg) ![支付宝付款](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191024141926.jpg) ![锤子便签](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191024145956.jpg) ![电池](https://cdn.jsdelivr.net/gh/removeif/blog_image/img/2019/20191024145940.jpg)</div>

```
#### 效果如下
[查看效果](https://linux-learning.cn)

