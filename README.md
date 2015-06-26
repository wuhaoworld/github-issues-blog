## 基于 Github issues 的单页面静态博客

玉伯的博客（https://github.com/lifesinger/lifesinger.github.com/issues ）让我第一次知道 github issues 还可以这样用 ，作者发了很多干货技术文章，让我不由得感叹 ，文章不在于形式，也不在于写在哪里，只要是好文，总不会被埋没。

即便如此，很多人仍然希望能有一个独立域名、可以自由修改主题的博客。Wordpress 、Typecho 太重，还要买 VPS、部署服务器环境、安装插件、主题，太折腾人，于是我想，完全可以利用 Github 提供的 API 来实现一个只有一个静态页面的博客，具体思路如下：

1. 作者在 Github issues 上写文章（写 issues）
2. 博客页面通过 JS Ajax 请求 Github API 来获取文章内容，进行页面的渲染
3. 通过社会化评论插件实现评论功能

于是花了几天时间实现了这个设想， DEMO：http://wuhao.pub/

博客的 demo 内容是读取的玉伯博客的 issues。

## 1. 部署方法(方案1)

1. fork 本项目，然后再新建一个用于存放 blog（issues）的 repo。 （fork 的项目是没有 issue 的，所以得新建个项目）
2. 修改 gh-pages 分支下根目录的 config.js，填写好对应的博客名称，你自己的 github 用户名、对应项目名、多说 ID，保存。多说账号在这里申请http://duoshuo.com/


        var _config = {
            blog_name : '用于演示的博客',  // 博客名称
            owner: 'lifesinger',          // github 用户名
            repo: 'lifesinger.github.com',// 用于存放 blog（issues）的项目名
            duoshuo_id : 'hello1234',     // 在第三方评论插件多说申请站点的子域名
            // access_token: '',          // 请求量大时需要在 github 后台单独设置一个读取公开库的 token
            per_page: '15'                // 默认一页显示几篇文章
        }


保存后即可通过 `http://用户名.github.io/github-issues-blog` 访问

    注意：至少得有一次提交，github 的 pages 功能才会生效，直接 fork，没有任何修改是不行的。

如果你想绑定独立域名，修改根目录的 CNAME 文件，将其中的网址修改为你的域名，并把你的域名 CNAME 到 `用户名.github.io` 即可

## 2. 部署方法(方案2)

1.克隆本项目，修改根目录的 config.js

    var _config = {
        blog_name : '用于演示的博客',   // 博客名称
        owner: 'lifesinger',          // github 用户名
        repo: 'lifesinger.github.com',// github 中对应项目名
        duoshuo_id : 'hello1234',     // 在第三方评论插件多说申请站点的子域名
        // access_token: '',          // 请求量大时需要在 github 后台单独设置一个读取公开库的 token
        per_page: '15'                // 默认一页显示几篇文章
    }
 
2.填写好对应的博客名称，你自己的 github 用户名、对应项目名和多说 ID，保存。多说账号在这里申请http://duoshuo.com/    
3.将所有文件上传到一个静态空间，打开首页即可看到效果。  

接下来就是在对应的 repo 的 issues 下写文章了！

## 3. 提高 API 访问次数的配额

默认情况下你是用匿名权限访问 github 接口的， github 的访问限制是一个小时最多 60 次请求，这显然是不够的，如何提高限制呢？ 

1. 到个人设置下的 Personal access tokens 页（https://github.com/settings/tokens ），如下图，点击右上角的 Generate new token
    
    ![](http://img-storage.qiniudn.com/15-6-12/56879685.jpg)

2. 填写名称，只勾选 `public_repo`,然后保存，github 会生成一个可访问你公开项目的 access_token，将它填入到配置文件的 access_token 的值中，并取消注释。
    ![](http://img-storage.qiniudn.com/15-6-12/64340386.jpg)
    
3. 打开 `app.js`,取消掉第 17 行和 88 行的注释，保存后重新上传即可

        data:{
            // access_token:_config['access_token']
        },
