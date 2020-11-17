# 【翻译】Inkdrop 我是如何通过一个Markdown编辑器获得1300刀/月的？

作者 [Takuya Matsuyama](https://medium.com/@inkdrop)

原文链接 [How I Built a Markdown Editor Earning $1300/mo Profit — Inkdrop](https://blog.inkdrop.info/how-i-built-a-markdown-editor-earning-1300-mo-profit-inkdrop-ddf6ad702c42)

译者 [litang0908](https://github.com/litang0908)

此翻译文章，经过原作者授权许可，将作为 [w2solo.com](http://www.w2solo.com) 社区的内容，未经许可，禁止转载。

#### 作者简介
Takuya Matsuyama 是来自日本的一位独立开发者，做独立开发者的时间已经有3年半，他的产品是 [Inkdrop](https://www.inkdrop.app/)，一款跨平台的 Markdown 编辑器，自2016年发布后，获得很多用户的好评。Takuya Matsuyam 除了作为开发者，他也承担着运营、营收、设计等事项，他的博客除了技术分享，也有很多对独立开发的思考，非常值得学习。

以下为正文内容（写于 2017.09.25)
***

在这里，我将分享我是如何独自创建一个跨平台应用-Inkdrop，并且获得了 1300刀/月 收入，我是在东京做独立开发者时开发的这款App，期间花了大量时间完成它，最近也写过另外一篇文章 [How to Price Yourself as a Freelance Developer](https://blog.inkdrop.info/how-to-price-yourself-as-a-freelance-developer-3453dfd59d91) 。

![Inkdrop — Jot down your daily hacking endeavors. https://inkdrop.app/](http://res.qianjiapp.com/inkdrop/inkdrop1.jpeg)
> Inkdrop — Jot down your daily hacking endeavors. https://inkdrop.app



我去年（2016）开始开发Inkdrop，截止今年7月每月收入是360美元，如果能够保持持续增长的话，明年我就可以结束我的自由职业生涯。

![现在inkdrop每个月的收入是 1361 美元（1美元=110日元）](http://res.qianjiapp.com/inkdrop/inkdrop2.jpeg)
> 现在inkdrop每个月的收入是 1361 美元（1美元=110日元）


#### 文章概要

1. 发现你的痛点
2. 创建MVP产品，知道你满意为止
3. 在 Beta 阶段获得忠实用户
4. 注重价格的可持续性
5. 使用 Stripe 处理支付业务
6. 制作一个完美的落地页
7. 专注于提供良好的用户支持
8. 记录下你在项目中学到的东西
9. 持续提高质量
10. 忽视所有批评


### 发现你的痛点

我是一个普通人，并没有什么特别之处。也就是说，我并不孤独，如果我遇到了什么问题，那很可能成千上万的人跟我一样。所以，我可以提供一项服务，来解决我的遇到的问题，这也是属于我的市场。

大约两年前，我对做关于软件开发的笔记感到非常沮丧，我曾尝试过很多 Markdown 类型的应用，但是始终没有找到一个合适的存储系统。像 Wiki 这种基于 Web 的服务，通常很难从众多tab里面查找，而且无法离线使用。而类似 Dropbox 这种云存储的App，同步慢，且需要消耗大量CPU资源，因为我存储的文件非常多。另外让我无法忍受的是，这些App功能缺失，或者太多复杂功能，又或者对我而言不够美观，这些个人喜好对我而言至关重要。所以我最终创建了我自己的笔记App。

### 创建MVP产品，知道你满意为止

一个App的核心价值是能够**解决问题**。在一个 MVP（minimal viable product）中，我本可以添加其它不错的功能，但是我没有这么做，因为 MVP 是为了验证产品的核心价值。

当我创建 MVP 后，如果我无法确认这就是我想要的，那么它就没有解决问题，我曾一直在反复打磨，直到我满意为止。

我的笔记App的目标如下：
- 支持 Github 的 Markdow 语法
- 简洁
- 美观的UI
- 快速同步
- 支持离线使用

除此之外的需求无关紧要。

我选择了 [Electron](https://electron.atom.io/) 来创建跨平台的桌面App，而 ReactJS 是一个很棒的选择，因为后期我使用了 React Native 来构建移动端版本。为了实现快速云同步，我选择了 [CouchDB](http://couchdb.apache.org/) 和 [PouchDB](https://pouchdb.com/) ，因为这两者内置同步功能，在 Electron app 中使用起来非常完美。如果你从离线状态回到在线，它可以自动同步。

归功于 [CodeMirror](https://codemirror.net/) ，我可以毫不费力地实现一个很棒的 Markdown 编辑器。同时在处理 UI 设计时，我来灵感来源于 [Airmail](http://airmailapp.com/) 。

因为是第一次使用 Electron 和 ReactJS 开发，所以我是通过 fork [Kitematic](https://kitematic.com/) 开始开发的，这是一个开源的 Docker 容器管理，在实践中已经解决了很多问题。

![http://res.qianjiapp.com/inkdrop/inkdrop3.png](http://res.qianjiapp.com/inkdrop/inkdrop3.png)


### 在 Beta 阶段获得忠实用户

当我的 MVP 足够好用，而且需要获得一些用户反馈后，我决定进入封闭测试阶段。不过，我从未发布过公测版本，正如 [DHH](https://medium.com/u/54bcbf647830) 在他的书（[Getting Real](https://gettingreal.37signals.com/)）中所言：

>不要把 Beta 版本当作替罪羊，它会把责任推给你的用户，如果你对自己发布的版本没有足够的信心，那你怎么能指望用户有信心？封闭测试挺好的，但是公测就是胡扯。如果不适合给用户消费，那就不要让用户消费。

>【原内容】：Don’t use “beta” as a scapegoat … Beta passes the buck to your customers. If you’re not confident enough about your release then how can you expect the public to be? Private betas are fine, public betas are bullshit. If it’s not good enough for public consumption don’t give it to the public to consume.


当封闭测试发布后，我在 [Hacker News](https://news.ycombinator.com/item?id=11849127) 发布了一篇文章，幸运的是，这篇文章排在了顶部，而且为我带来了超过1000名测试用户。 这些测试用户为我提供了很多建议和Bug，他们帮助我把产品完善，直到可以发布正式版本。

![http://res.qianjiapp.com/inkdrop/inkdrop4.png](http://res.qianjiapp.com/inkdrop/inkdrop4.png)
> Hacker News 带来的很多流量

在封闭测试阶段，我时常去查看使用数据，以便去发现是否有重度用户每天使用，因为这能表明我产品能否成功。我使用的数据分析工具是 [Mixpanel](https://mixpanel.com/) 。假如没有看到重度用户，则意味着 App 不够好。

### 注重价格的可持续性
最重要的是要确保一切都是可持续的。如果是买断制，则要求你不断地增加各种功能来提升销售收入，而如果是订阅制，你只需要持续提供有价值的服务。

我决定以 $4.99/月 或者 $49.9/年 的价格销售 Inkdrop，同时附带 60天 的试用期。因为并不像创建一个 Google 这样的公司，我并不需要抓住数亿用户，Inkdrop 只是一个大公司不愿意投资的小领域。所以尽管Inkdrop 对一些人而言比较贵，但是它的价格并没有必要去参考诸如 Evernote 这样的产品。Inkdrop 的目标用户是那些与我相似的人，他们会毫不犹豫地为喜欢的工具付费。

一些人看到价格后很生气，但另一方面，这个应用开始一点点地赚钱。也就是说，如果你认为作为一个用户，价格是合理的，那么可以继续销售，因为肯定有人跟你一样。我在网站解释了一下定价的理由（[reasons of the pricing on the website](https://inkdrop.app/pricing)），以便用户可以理解我的决定。


### 使用 Stripe 处理支付业务
[Stripe](https://stripe.com/) 是一个很流行的支付平台，它同样支持在日本时，可以接收来自其它国家的支付订单。当我开始使用时，发现 Stripe 的 API 非常简洁强大。我喜欢 Stripe 的理由如下：

- 支持订阅时试用
- 可以通过优惠券进行打折
- 精心设计的网页 Hooks 允许我们在使用过期前三天发送通知
- 支持电子邮件收据
- 当用户更改计划时，可以按比例计算订阅成本
- 它的客户端 SDK 不需要用户直接将信用卡信息发送到服务器

有了这些优秀的特性，我非常迅速地完成了支付功能。


### 制作一个完美的落地页

我查看了很多我喜欢的产品的落地页，比如 Sketch, Stripe, Mixpanel, Airmail, Basecamp 等，并且了解了他们在网页上提供了什么内容。以下是一个完美落地页需要的注意事项：

- 快速的说明
- 提供Demo（截图，视频，举例，无需注册可体验的App等）
- 主要优势和好处
- 价格
- 推荐信
- 行为召唤（Call to Action 译者注：参考 [如何理解行为召唤](http://www.woshipm.com/operate/225162.html))

推广视频非常有用，因为它可以向人们展示应用的实际外观以及如何使用它，而不需要用户在页面间跳转。视频质量并不重要，所以我是自己通过  Adobe After Effects 和 iMovie 制作了一个简单的推广视频。

Inkdrop 的宣传视频，可以在这里查看 [https://vimeo.com/186246591](https://vimeo.com/186246591)

视频中的这个人并不是我的朋友，而是从 [VideoHive](https://videohive.net/) 获得的片段，在 [AudioJungle](https://audiojungle.net/item/uplifting-piano/16413832) 同样有很多背景音乐，所以我成功地花了几天时间制作了这段视频。

我使用 After Effects 来制作带动态过渡效果的 Demo ，但是 After Effects 处理非常耗时，所以我使用 iMove 来合成视频片段。

![http://res.qianjiapp.com/inkdrop/inkdrop5.png](http://res.qianjiapp.com/inkdrop/inkdrop5.png)
>Inkdrop的落地页


### 专注于提供良好的用户支持

快速的反馈时间应该是首要任务，无论如何，人们不喜欢被要求等待，但大公司通常要求人们等待几天，直到他们做出反馈。而你可以马上给出一个靠谱的回应，使自己真正区别于竞争对手。

当我通过电子邮件、用户论坛和 twitter 收到询问时，我停止手头的一切工作，并尽快做出回应。此外，我经常会在几天内修复错误，用户通常对此感到惊讶。

因此，通过提供快速周到的用户支持来建立忠诚度，对于获得付费用户非常有效。


### 记录下你在项目中学到的东西

无论结果如何，每一次经历都值得分享给社区。在市场营销方面，播客非常有利于提高知名度，而且在不花费大量资金的情况下，事情会朝着正确的方向发展，因为传授经验是一件好事情，而读者则会通过社交网络传播你的文章。你都不需要花钱买广告。

3个月前，我写了一篇文章，用日语描述了构建这个应用程序的经历，这篇文章为我带来很多的注册用户，他们中一部分也转化为付费用户。

![http://res.qianjiapp.com/inkdrop/inkdrop6.png](http://res.qianjiapp.com/inkdrop/inkdrop6.png)

![http://res.qianjiapp.com/inkdrop/inkdrop7.png](http://res.qianjiapp.com/inkdrop/inkdrop7.png)


下面是这篇文章的英文版，同样给网站带来很多用户：

![http://res.qianjiapp.com/inkdrop/inkdrop8.png](http://res.qianjiapp.com/inkdrop/inkdrop8.png)


### 持续提高质量

根据 [Paul Graham’s definition](http://www.paulgraham.com/growth.html) 所言，创业追求快速成长。从企业家的角度而言，我的方法很明显是一个迂回的盈利方式。我只是在追求我的快乐生活，而不是获得其它的“成功”，我很喜欢 [TJ Holowaychuck’s](https://blog.apex.sh/announcing-apex-software-inc-5008c454002) 说得这段话：

>生活方式也越来越重要，我工作越多，越意识到我应该以健康的节奏去做我喜欢的事情，而不需要担心其它的 “成功”。

>【原内容】：Lifestyle is also increasingly important, the more I work, the more I realize I should be doing things I enjoy, at a healthy pace, and not worry about the other kinds of “success”.

同样，我也非常喜欢开发我热爱的应用，我相信随着产品质量的提高，回报也会随之而来。

在 Inkdrop 正式版发布后，它并没有像封闭测试阶段那样走红，不过仍然有少部分重度用户贡献了 $1000/月 的销售额。随着应用的成熟，收入增长速度也在一点点加快。

我专注于开发，而不是一遍又一遍地给网络博主发电子邮件，以便让他们报道这个应用。一些用户告诉我，他们已经把这个应用告诉了他们的朋友。所以我想培育这个小社区。


### 忽视所有批评

如果你想取悦所有人，你将无法取悦任何人。

自从封闭测试发布后，我收到了很多的批评，比如：

“当应用消失后会发生什么”

“它看着像是一个 Electron 做的应用，因此会导致性能和内存明显下降”

“$50/年 太扯了！”

“我无法信任远程服务器的安全性”

“啊，这是一个订阅制的App，不适合我”

批评的人总是很情绪化，而满意的人什么也不会说，所以你会觉得用户充满了消极的想法。但是你也可以认为这是一个好的现象，因为他们不会对自己不感兴趣的东西说任何话。最糟糕的情况是当你发布后，什么事情也没有发生，消极的反应其实证明你解决了用户痛点。

我忽略了所有的批评，专注于满足那些喜欢它的用户，这些努力应该能提升应用的价值。


### （待续）我是怎么独自开发的...

因为在这里待很久，我想把技术内容写在单独的文章里，比如我是如何设计并开发桌面端和移动端、Server 端的，欢迎在 [Twitter](https://twitter.com/inkdrop_app) 或者 [Medium](https://medium.com/@inkdrop) 关注我。

希望这对你们的副业有所帮助。
