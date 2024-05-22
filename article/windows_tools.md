---
title: windows实用工具
date: 2022-11-11 22:22:22
categories:
  - tool
---

## window10 实用软件

### chrome

插件：

- [油猴](https://wwn.lanzouy.com/i6eOx0bmd9ub)
- [adblockPlus](https://wwn.lanzouy.com/iXH250bmd0li)
- [ublock](https://wwn.lanzouy.com/ineO40bme6qf)
- [草料二维码](https://wwbk.lanzoue.com/iRIjx0bme6uj)
- [github 加速](https://wwk.lanzouq.com/i1VHd0bme6ti)
- [vue2](https://wwbk.lanzoue.com/i2ykx0ht4wfi)
- [vue3](https://wwbk.lanzoue.com/iOqHN0ht4xaj)
- [react](https://wwbk.lanzoue.com/i8wch0ht4xed)
- [redux](https://wwbk.lanzoue.com/i5baZ0ht4wwf)
- [vimiumC](https://wwbk.lanzoue.com/iBCXa0ht4wgj)
- [editThisCookie](https://wwbk.lanzoue.com/isrNg0ht4wle)
- [surfingkeys](https://wwbk.lanzoum.com/ilPSS0wpm6vc)
- [FeHelper 前端助手](https://wwk.lanzouq.com/iWL4U1wpulxi)
- [字节火山翻译](https://bytedance.larkoffice.com/docx/CMu2dZjXKojmGfxjibscjm2BnJh)

### other

- [IDM 下载工具](https://wwbk.lanzoue.com/iduIc0bmfhcd)
- [winscp+putty](https://wwbk.lanzoue.com/i7IKb0hsyaid)
- [截图工具 snipaste](https://wwn.lanzouy.com/i0Lmyxe5nwh)
- [鼠标右键管理工具](https://wwn.lanzouy.com/iujpA0bm9jda)
- 无广告输入法：[手心输入法](http://www.xinshuru.com/)
- [解压工具 bandizip 破解版](https://wwn.lanzouy.com/izNvH0bm9j9g)
- [解压工具 7-zip](https://wwn.lanzouy.com/iR7jo0bmclsf)
- 免费简洁思维导图[blumind](https://wwn.lanzouy.com/iEtBj0bm9hfa)
- 精简小巧屏幕录制工具[FSCature](https://wwn.lanzouy.com/ium4t0bmagsd)
- 电脑控制手机[scrcpy](https://wwn.lanzouy.com/iCzdu0bmc0cd)
- window 性能监控[Traffic Monitor](https://wwbk.lanzoum.com/i2Pkr0o8tdej)
- 干净简单轻量级看图工具[Honeyview](https://wwbk.lanzoum.com/iBWdi0o8tfif)
- 轻量级图片编辑工具[paint.NET]()
- WPS 教育考试专用版，纯净无广告[wps](https://ncre.neea.edu.cn/html1/report/1507/861-1.html)
- WPS 校园版,去掉了附加的增值服务以及广告（停止维护）
- windows 本地播放器[potPlayer](https://flowus.cn/share/4dc25551-ad00-4d81-9421-b3fee98757a9)
- obs
  直播录制: 例如，我希望进行带弹幕的直播录制，但不影响电脑前台使用。请注意，不带弹幕的纯直播录制，推荐使用不同平台各自的直播流抓取录制软件，例如 B 站录播姬。对于主播本人使用 OBS 推流并希望录制的情况，推荐使用 OBS 直接进行录制，没有额外的性能开销。
  游戏回放录制: 例如，我进行了一场英雄联盟对局，事前没有开启录制，希望事后录下整局回放，但不影响电脑前台使用。

  原理
  画面: 使用 OBS 的窗口捕获以捕获当前活动的窗口，随后最小化或移动到其他虚拟桌面也不受影响。我们可以简单地开始录制之后最小化，也可以把整个录屏工作流放在另一个虚拟桌面。需要说明的是，OBS 仅录制该窗口的画面，您在非该窗口进行的操作不会被录制，但在该窗口进行的操作会被录制。

  声音: 通过设置或软件，改变需要录制音频的播放设备，到虚拟声卡的播放设备，在 OBS 设置中选择虚拟声卡的播放设备进行录制，来达到只录制您想要的音频，且不影响桌面其他音频的目的。这种方法会使声音不再正常播出，如果您同时希望能听到录制的声音，您可以在 OBS 的高级音频属性中打开“监听”选项。

  虚拟声卡: 虚拟声卡并不是真实的音频输入输出设备，顾名思义，它会虚拟出新的播放设备和录制设备。有时，在您安装显卡驱动时，会附带安装虚拟声卡，即 NVIDIA / AMD High Definition Audio。

  工具
  OBS Studio: 笔者唯一指定录屏/推流软件，这是一个免费开源软件，官网下载页。

  EarTrumpet: 此软件的主功能是（在 Windows 10）分别调节不同软件的音量，也有改变软件音频播放设备的功能，我们需要的就是后者。这是一个免费开源软件，发布在 Microsoft Store 上，需要您自行前往搜索和下载。

  VB-CABLE Virtual Audio Device: 如果您没有现成的虚拟声卡，这是笔者推荐的一个轻量的虚拟声卡，功能是简单地把，在其虚拟播放设备上播放的声音，转接到其虚拟录制设备上。值得一提的是，OBS 可以直接从播放设备获取音频，而不必到其虚拟录制设备获取。（实际上，那将是有一定延迟的）官网下载页。

  虚拟桌面: Windows 10 已经自带虚拟桌面功能，按 Win + Tab 进入虚拟桌面管理，可以创建新的虚拟桌面和把窗口在不同虚拟桌面间拖动，使用 Ctrl + Win + ← 和 Ctrl + Win + → 快速切换虚拟桌面。

### 字体

> mono 等宽，serif 衬线, Sans-serif(无衬线) nerd(图标字符)
> 区分 0O、iIlL1

- [notoSansMonoCjkSc](https://github.com/googlefonts/noto-cjk)：google 开源免费商,中国程序员中文字体
- [monaco](https://www.lanzouy.com/iOqD50bm9jcj)：苹果系统
- [ubuntuMono](https://design.ubuntu.com/font)：开源免费商用圆润
- [droidSansMono](https://wwn.lanzouy.com/i5xTI0bm9jbi)：安卓系统字体 0O、iIlL1 区别不行
- [arasa Gothic (更纱黑体) ](https://gitee.com/mirrors/Sarasa-Gothic)由 Noto Sans / Iosevka 和思源黑体的汉字部分合并而来,开源免费字体
- [inconsolata](https://wwn.lanzouy.com/ifuCp0bm9jah)：google 开源字体等宽支持连字,不包含中文
- [jetBrainsMono](https://www.jetbrains.com/lp/mono)：jetbrains 公司开源商用等宽
- [FiraCode](https://wwk.lanzouq.com/iHqrW1wqqpab)：mozilla 开源支持连字,不包含中文
- [SourceCodePro](https://github.com/adobe-fonts/source-code-pro)：Adobe 开源免费商用
- [losevka](https://github.com/be5invis/Iosevka)：开源支持连字,不包含中文
- [sourceHanSans](https://github.com/adobe-fonts)google&adobe 开源 adobe 中文字体
- [notoHanSans](https://github.com/googlefonts)google&adobe 开源 google 中文字体
- [oppo 字体](https://wwk.lanzouq.com/ijkmS1wqqzgh)
- [harmoneyOs 字体](https://wwk.lanzouq.com/iXpUw1wqr5yb)
