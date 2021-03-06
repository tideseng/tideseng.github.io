# 使用github pages快速搭建个人博客

## 快速搭建

搭建过程非常简单，只需要五个步骤即可

1.  登录 [Github](https://github.com/login)

    进入 [Github](https://github.com/login) 进行登录，没有账号先进行注册
  
    ![](https://upload-images.jianshu.io/upload_images/9110802-e80a1d0bc4007655.png)

2.  Fork [我的仓库](https://github.com/tideseng/tideseng.github.io)

    登录成功后，点击 [我的仓库](https://github.com/tideseng/tideseng.github.io) 进入我的博客模板仓库页面

    点击 `Fork` 按钮，刷新页面该仓库就会拉取到你的账户下

    ![](https://upload-images.jianshu.io/upload_images/9110802-507e6d2a23321b58.png)

3.  修改仓库名

    查看拉取后的仓库，点击 `Settings` 按钮，进入仓库设置页面

    修改Repository Name仓库名成你的仓库名，仓库名格式：`你的github账户名.github.io`

    然后点击 `Rename` 按钮，修改成功

   ![](https://upload-images.jianshu.io/upload_images/9110802-02ef902bbce4b372.png)

4.  检查配置

    再次点击`Settings` 按钮进入仓库设置页面，当 **GitHub Pages** 下显示站点链接，说明仓库信息配置成功

    ![](https://upload-images.jianshu.io/upload_images/9110802-80edc44aeaa83b0f.png)

5.  访问博客

    现在，你的个人博客就搭建好了，没错，就是这么简单

    复制上面的链接，在浏览器打开就能访问你的个人博客了

## 修改设置

修改设置是为了让博客网站相关信息根据你的需要来定制化，配置文件是仓库首页的 `_config.yml` 文件，点击即可修改，当然也可以使用 git 提交修改

1.  修改系统设置

    根据自身需要修改相应的内容

   ```yaml
   # 站点设置
   title: "Tideseng" 		# 网站主页背景图的标题
   SEOTitle: "tideseng的博客" # 网站窗口页面的标题
   header-img: "https://upload-images.jianshu.io/upload_images/9110802-0df29ac6fb7651ed.jpg" # 主页背景图，可以用本地图片(img/xxx.jpg) 或在线图片
   description: "tideseng's Personal Blog" # 网站介绍
   keyword: "tideseng的博客, tideseng, Java, 程序员, 架构师, 工程师, 开发"
   url: "https://tideseng.github.io" # 个人博客网站地址
   baseurl: "" # 不建议填写
   github_repo: "https://github.com/tideseng/tideseng.github.io.git" # 对应的仓库页面
   
   permalink: pretty
   paginate: 10    # 每页博客显示的数量
   exclude: ["less","node_modules","Gruntfile.js","package.json","README.md"]
   anchorjs: true
   
   # 标签设置
   featured-tags: true   # 网站页面是否使用首页标签
   featured-condition-size: 0   # 相同标签数量大于指定数，才会出现在首页
   ```

2.  修改侧边栏设置

 ```yaml
 # 侧边栏
 sidebar: true # 是否关闭侧边栏
 sidebar-avatar: "https://upload-images.jianshu.io/upload_images/9110802-d63f5973fce57458.jpg"   # 侧边栏个人头像，可以使用本地图片(img/xxx.jpg)
 sidebar-about-description: "on pain on gain" # 侧边栏个人描述
 email: "tidesengli@gmail.com" # 侧边栏个人联系邮箱
   
 # 社交账号
 RSS: false
 github_username: "tideseng"   # 必填，其它地方需要该参数值
 jianshu_username: "f76a2424b7a2"
 #zhihu_username: xxx
 #weibo_username: xxx
 #facebook_username: xxx
 #twitter_username: xxx
 #linkedin_username: xxx
   
 # 友情链接
 #friends: [
 #{
 #  title: "简书",
 #  href: "http://www.jianshu.com/u/f76a2424b7a2"
 #},{
 #  title: "Github",
 #  href: "https://github.com"
 #}
 #]
 ```

## 编写博客

文章统一放在 `_posts` 目录下，图片统一放在 `img` 目录下。由于我对图片的来源进行了扩展，所以可以使用在线图片，不必使用本地图片，这样访问更快

1.  创建文件名

    文件名格式——日期+标题，如：2020-04-11-修改hosts提速访问github

2.  编写文章

   ```yml
   ---
   layout: post   					# 使用的布局（不需要改）
   title: 修改hosts提速访问github 	# 文章标题 
   subtitle:    					# 文章副标题
   date: 2020-04-09 				# 时间
   author: tideseng 				# 作者
   header-img: 					# 文章背景图片（可以是在线图片/本地图片）
   header-mask: 					# 文章背景图片透明度
   catalog: true 					# 是否归档
   tags:							# 标签
       - github
   ---
   
   1. 添加/修改文章配置
   2. 编写正文，正文使用 MarkDown 标记语言编写，简单上手
   建议：直接 copy 一份文章，修改文件名、文章配置、编写文章正文
   ```

3.  上传到Github

    这样你的第一篇博客就创建好了，刷新首页，在主页上就会出现

    另外，如果你是通过github网页创建文件、编辑文件，就不用上传了

## 功能扩展

### 网站统计

*   百度统计

1.  登录 [百度统计](http://tongji.baidu.com/web/welcome/login)

    用百度账号登录即可

2. 添加网站域名信息

   填写相关信息即可

3. 添加ba_track_id配置

   将下图中百度统计生成值设置到_config.yml的ba_track_id中

   ![](https://upload-images.jianshu.io/upload_images/9110802-f04721dd0467f461.png)

4. 提交到github

   将修改后的_config.yml文件提交到github，当然也可以直接在github用网页修改

5. 刷新检测

   一般情况下，检测是不能通过的，需要手动检查

   ![](https://upload-images.jianshu.io/upload_images/9110802-8d70db70a1324767.png)

   ![](https://upload-images.jianshu.io/upload_images/9110802-e5374e0bf977e59e.png)

6. 手动检查是否生效

> 第一步：参照 [参考文档](https://tongji.baidu.com/web/help/article?id=93&type=0) 查看hm.js文件是否加载
> 
> 第二步：添加时间戳手动检查(避免缓存)

  ![](https://upload-images.jianshu.io/upload_images/9110802-210b06783989422c.png)  

*   谷歌统计

访问 [Google Analytics](http://www.google.cn/analytics/) 进行相关设置即可

### 评论系统

1.  [注册应用](https://github.com/settings/applications/new)

    ![](https://upload-images.jianshu.io/upload_images/9110802-e2143183530fbe73.png)

2.  获取clientID、clientSecret

    注册成功，即可生成clientID、clientSecret

3.  修改gitalk配置

    将获取到的clientID、clientSecret等信息设置到_config.yml的gitalk配置中

4.  提交到github

    如果你是使用github网页进行修改，不用提交

5.  刷新博客文章页面

    ![](https://upload-images.jianshu.io/upload_images/9110802-d136dc1fbac4df12.png)

6.  登录授权

    ![](https://upload-images.jianshu.io/upload_images/9110802-0bd77ebd5bcae8ba.png)

7.  刷新页面

    这样你的博客文章页面下方就会出现评论窗口
