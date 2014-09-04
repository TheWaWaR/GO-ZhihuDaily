GO-ZhihuDaily
=============

#[知乎日报 Web版（GoLang实现）](http://zhihudaily.ahorn.me/)

点上面的链接可以跳转

写这个的最初目的是因为PC上没有一个很好的阅读知乎日报的方式    
即那种一眼望过去，几天内的内容尽收眼底，然后点开自己感兴趣的 Title 继续阅读  

##USAGE

####ImageMagick
需要用到`ImageMagick` 的 `convert` 命令来裁剪官方图片

Linux:  
```bash
yum install imagemagick
```

OSX:  
[ImageMagick](http://www.imagemagick.org/script/binary-releases.php#macosx)

####启动：

```bash
cd GO-ZhihuDaily
go run main.go  
```
####浏览器 URL:   
`0.0.0.0:8000`   

####自带 API:  
`http://zhihudaily.ahorn.me/api/1`
1 可以是 2 3 4 ...

---

##API（点击看大图）:

![](http://cl.ly/image/013N0v0H2g3i/WEB%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5.png)  

红色的已作废，蓝色的可以用  

##License

GNU GENERAL PUBLIC LICENSE

- - -

1. 感谢 @[faceair](https://github.com/faceair/zhihudaily)，他做了最早的web版(PHP)（API就是从他的代码里找到的）
2. SQLite 存储 API 返回的 JSON 数据，减小访问官网次数
3. 每小时更新一次当天数据
4. [Martini](https://github.com/codegangsta/martini) 框架
5. 蹭朋友的 VPS ~~[貌似十分不稳定，动不动就 502 了，（好吧，是我的小程序不稳定）]~~  


##开发日志
2014-09-01 09:41:36

服务已恢复  

由于之前缓存的图片以及数据库全部木有了   
这次需要首次启动   

然后...我就知道 `fork` 这个项目的孩纸们的心情了 →_→  
对不住泥们了  

首先木有 `main.db`，我只好又用 `sqlite3` 创建了一个，以及两个 `table` （之前其实是有代码的，服务稳定后，我就删掉了...）  

然后如果整个数据库为空，也木有去日爆官方去重新申请数据  

现在这些问题已修复  

---

2014-08-27 15:48:10

小伙伴 VPS 到期了，经过询问才知道 `Linode` 7月份就给他 Email 了，但他手头有点紧张，没续费...  

好吧，当时这个就是为了自己在电脑上看才写的，属于玩具项目  
但从 `Google Analytics` 的数据分析来看，这大半年来，每天都有近 200 人过来看一眼，周末时相对少些  
我也没做过推广，所以基本上都是网页刚刚架好时的那些用户  

所以在这里要对泥们说声对不起啦   

这件事情也让我认识到，有时候**云服务**可能靠谱一点，虽然限制多多   

吃一堑长一智，`iOS` 的后台服务，我就挂在 `Heroku` 上好了  

---

2014-07-28  

小环童鞋想把页面改个样式，但想用异步请求的方式练手   
所以需要提供 JSON API   

我记得不是很难，万万没想到 `Martini` 中只要把：
```go
r.HTML(200, "content", XXX)
```
改成：
```go
r.JSON(200, XXX)
```
就万事大吉了

真心方便\~\~\~

---

2014-05-19 18:14:09

小伙伴 [Misa](http://www.cnblogs.com/misadancer/) 看到主页后，说 “加些透明度，鼠标效果什么的就赞了”
真是站着说话不腰疼， You can You UP !
不想她居然来真的，还一边改一边嫌弃我代码写的丑（只限前端）  

然后看她大刀阔斧的一劈，原来好好的主页就变成下面的样纸了  

![](http://cl.ly/image/323T2B2u0e18/GO-ZhihuDaily-1.png)
我下了一跳，经咨询发现她有厌世情绪，怪不得都是黑白的  

过了两天我才反应过来，丫这个墓志铭主题原来是写了给我送终的啊啊啊  

- - -

2014-04-28 15:01:48

原来所有的页面一下全部打印出来，丑死了  

然后改了改  
仍然很丑  →_→  

怪不得有射击狮这个职位

只有用这句话安慰自己了：  
长得丑不要紧，能用就行

---

2014-04-23

快俩月了，今天乍一看内存占用，以为又要爆   
又在这傻逼的优化，弄完了才发现把`VmSize`看成了`VmRSS`了

不过，之前4.5+%， 现在0.9%，缩小成大约原来的五分之一吧

也好

---

2014-03-28

挂挂挂，总是挂
开始以为是并发   
仔细检查发现居然是`checkerr`内部爆出`panic`   
[呃……]σ(-_-メ)  ，搞什么   

go好牛逼，两步一err，三步一check  

---

2014-03-26   

也没人issues我，知乎上发现有人评论才发现 又 502 了    
大惊，肿么又挂了啊...   

找BUG的过程中不停的想起《代码大全》里的一句话：  

>写的时候只有我和上帝知道是什么意思，而现在**只有上帝知道了**  

还好最后发现是日爆API改了，没有分享的图片了，招致臭名昭著的**空指针**  

话说这么老早的API了，还变JSON字段，还让不让我等P民活了丫   

---

2014-03-09

直接运行是只能下载裁剪当天的图片的，要在另一个地方调用  
理论上我应该加个注释，然后重构精简下，再加个图片下载成功判断   
但人都有惰性的，能跑就想着有时间再来好好优化  
最最重要的原因是，最近失恋了( ＞﹏＜)，对不住fork的大家了  


---

2014-03-05

用`convert crop`了图片头部，缓存不到200M，载入页面也快了

PS: 后台有一些奇怪的数据，比如没访问主页`/`，直接跳到`/page/62`，还有一些老的跳转`/url/**`，是因为浏览器缓存吗？统统重定向到主页`/`去了

---

2014-03-04  

我去，好像昨天图片全挂了，开始以为网络有问题  
后来发现好像被知乎日报官方屏蔽了，图片`403 forbidden`

只有先换成了文字顶上  

小伙伴伸出了援助之手，说图片缓存到他的VPS上好了  

（他是不知道啊，为啥我之前酱紫做呢？一共6W多条记录，所有图片下下来高达2G多）  

既然他说了，那就放上去好了  ( ≖‿≖)   

加了个下载，递归二分，8个狗同时跑

---

2014-02-28

貌似内存已经稳定了

请轻拍

---

2014-02-25

V友太凶残了，先是502，下午论坛上发现后去SSH，发现进程已经没了，重开了一次   
晚上看htop，发现又已经吃掉VPS 25%的内存（1G），然后小伙伴生气啦(๑′°︿°๑)   

赶紧找BUG，这里改改那里动动，内存增速居然放缓了，赶紧先布上去再说   

然后慢慢改，好像是查询数据库后没close，还有上次更新时间忘重新赋值了，还有...，还有... （┬＿┬）   
之后剥离判断，加了个自动更新    

好不容易写个东东，那么简单还冒出来这么多问题，脸丢完了都╮(╯▽╰)╭

还好现在稳定了，据观察，一段时间后，内存不增反减  

GC好神奇  

---

2014-02-24

图省事先用图片代替了

后面考虑用文字，这样复制粘贴也容易点

好吧，是我HTML/CSS不会啊，写起来步步维艰，现在真心做不到(>_<)

希望做成的样纸是 OldReader 那样，左边一栏标题，右边内容，实现滚动阅读

---
以前没做过Web开发，边写边学GoLang/Git/HTML/CSS/GAE

( ⊙o⊙ )哇，这么多 '/'  
弱爆了有木有

域名、VPS都是蹭朋友的
太惨了

( >﹏<。)～呜呜呜……

