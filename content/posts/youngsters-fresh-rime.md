---
title: "年轻人的第一个自定义输入法 - RIME 开箱 + 配置"
date: 2022-08-06T11:19:51+08:00
---

> 本文使用 RIME 编写。如果你也想要自定义你的输入法，可以去 [RIME 的官方网站](https://rime.im) 看看呀（ 
> 
> 本文在编写的时候参考了很多人的博文。参考文献在文章最后。
>
> 其实我是一边配置 `RIME` 一边用 `RIME` 写这篇博文的（


暑假的最后一天（没错啦 我暑假 8 月 7 号就结束啦 我们高中真棒呀！），在看到了有一帮人在用 `RIME` 打字以后，我也决定去搞一个（

> 为您省流：如果以下的配置你看着喜欢，可以直接去 [这里](https://github.com/franci-e/misc/tree/main/RRime/) 下载配置文件哦（

## RIME 的字体
应该是默认配置的问题吧，在下载了 `weasel` 以后，UI 的默认字体就是微软雅黑。不是说这个字体难看吧，就是... 下都下了，能自定义一下的东西肯定要自定义一下啦。而且，其实我也更喜欢用 **霞鹜新晰黑** (LXGW New Clear Gothic, [`LXGW` 老师的 repo 在这里](https://github.com/lxgw/LxgwNewClearGothic)) 一点。

<!-- ### Font patching
因为， emm...，思源黑体的英文不是说它不好看吧，主要是我更习惯用等宽字体一点（`jiu'xiang'zhe'yang`）所以，在简单看了一下 [`RIME` 的把玩指南（雾）](https://github.com/rime/home/wiki/CustomizationGuide) 以后，我惊奇地发现：`RIME` 好像只能全局使用一个字体耶—— [^onlyOneFont]（脑子宕机.jpg）

所以，我打算 `patch` 一下字体，把我比较喜欢的 `Source Code Pro` 字体和 **霞鹜新晰黑** 组合到一起，达到中文字体是霞鹜新晰黑，英文字体是 `Source Code Pro` 的效果（

然后，我就 Google 了一下 `Source Code Pro Noto Sans`，然后就找到了日本的一位老师的博文：[Source Code Pro と Noto Sans JP を組み合わせてフォントをつくりました](https://seeorder.hatenablog.com/entry/2020/12/13/190107)。凭借我城乡结合部水平的日语（that is, 只看的懂汉字），我大概知道了这位老师是使用 `Python` 的 `fontforge` 包把这两个字体 `patch` 到一起的。（源码在 [这边](https://github.com/mshioda/relaxed-typing-mono-jp/blob/main/script.py)）所以我大概改了一下代码，然后硬是把 `Source Code Pro` 和 **霞鹜新晰黑** `patch` 到了一起，叫做 `Source Code Patched New Clear Gothic`（可以在 [我的 GitHub](https://github.com/franci-e/misc/rime/blob/main/SSCPro_LXGWNCGothic.ttf) 上下载哦） -->

### Config Settings
在 `weasel.custom.yaml` 中的 `patch` 下面增加这两行：
```yaml
"style/font_face": "LXGW New Clear Gothic"
"style/font_point": 12
```

## RIME 的标点
As we all know，我是一个比较喜欢打直角括号的人。所以，我打算在中文模式下，把默认的双引号（`“”`）改成直角括号（`「」`， 其实在用 `RIME` 的时候在键盘上打 `[]1` 就行了，但是本着 `Geek` 的精神（？和我在改的东西有什么关系啦！），我还是决定改一改（））

我选用的输入方式是 `luna_pinyin_simp`，所以我就去 `luna_pinyin_simp.custom.yaml` 下面稍微改了几行（
```yaml
patch:
    punctuator/full_shape: 
        "\"": {pair: ["「", "」"]}
    punctuator/half_shape:
        "\"": {pair: ["「", "」"]}
```

然后让本来处理 `「」` 两个键的 <kbd>[</kbd> (`bracketleft`) 和 <kbd>]</kbd>(`bracketright`) 乖乖回去承担输入 `【` 和 `】` 的责任啦（

所以最后关于标点的配置长下面这样：

```yaml
patch:
    punctuator/full_shape: 
        "\"": {pair: ["「", "」"]}
        "[": "【"
        "]": "】"
    punctuator/half_shape:
        "\"": {pair: ["「", "」"]}
        "[": "【"
        "]": "】"
```

整完了就按一下「重新部署」就可以应用更改啦！

## RIME 的词库
众所周知，一般来说，在我们输入中文的时候，我们大概率是需要一个词库的。所以，我决定整几个词库来玩（

在网络上，我们可以找到一些已经整理好的词库，比如清华的 [THUOCL](https://github.com/thunlp/THUOCL) 和搜狗的 [细胞词库](https://pinyin.sogou.com/dict/)。在下载好词库以后，我们可以使用 [imewlconverter](https://github.com/studyzy/imewlconverter) 给它转换成 `RIME` 支持的格式。

> 不知道为什么，我用 `studyzy` 老师的 `imewlconverter` 转换词库就会报错，但是使用了 `yfdyh000` 老师 `fork` 的 `imewlconverter` 就可以了（）`yfdyh000` 老师的 `imewlconverter` 的 `repo` [在这里](https://github.com/yfdyh000/imewlconverter)。

我配置的词库有：
- [THUOCL](https://github.com/thunlp/THUOCL) 的全部词库
- [萌娘百科](https://moegirl.org.cn/) 的词库
- [中文维基百科](https://zh.wikipedia.org/) 的词库 

在 `imewlconverter` 中，我们可以把搜狗的 `.scel` 格式的词库转换为 `RIME` 词库。当然，如果你和我一样懒，也可以在下载完 `imewlconverter` 以后发现在 [`Dwsy` 老师的 `rime` repo 里](https://github.com/Dwsy/rime) 就有已经转换好了的词库，直接 `git clone` 一下就能用的那种（

## 其他的设置
### 翻页！
因为我比较喜欢用 <kbd>-</kbd> 和 <kbd>=</kbd> 键切换候选页面，所以我在 `default.custom.yaml` 中加了这几行：
```yaml
patch:
    "key_binder/bindings":
        - {accept: minus, send: Page_Up, when: composing}
        - {accept: equal, send: Page_Down, when: composing}
```
### 横向！
默认的 `RIME` 是采取纵向排布候选结果的，但是我比较喜欢在电脑上用横向的布局，所以我又在 `weasel.custom.yaml` 中加上了这一行：
```yaml
patch:
    "style/horizontal": true
```

那横向了，自然能装下来的候选结果就变多了，所以把每页的候选结果个数换一下：
```yaml
patch:
    "menu/page_size": 7
```
## 结束！
看一下最终的成果吧：
![Final Picture](rimeSettings.png)

## 参考文献
[Lufs X](https://isteed.cc/) 老师的 [好用好看好玩的输入法 —— 鼠须管配置使用](https://blog.isteed.cc/post/squirrel-customization-2022/), Accessed Aug 6, 2022.

[ahxxm](https://github.com/ahxxm) 老师的 [rime_config](https://github.com/ahxxm/rime_config) `repo`

<!-- [^onlyOneFont]: 也有可能是我看错了，或者是我太菜了不知道怎么改（ -->

