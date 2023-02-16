# rime-config
我的输入法配置。

## Install

-  Homebrew：`brew install --cask squirrel`
- [官网安装](https://rime.im/)
- [github release](https://github.com/rime/squirrel)

我使用的 **Homebrew** 安装。

note：
  1. 据文档提及，安装完后在 `设置/键盘/输入法` 中，无法找到 *鼠须管* 输入法；此时需要退出用户登录（或者简单粗暴重启）
  2. 一定要看[帮助文档](https://rime.im/docs/)，尤其[说明书](https://github.com/rime/home/wiki/UserGuide)和[定制指南](https://github.com/rime/home/wiki/CustomizationGuide)（大概扫一眼，以防有问题不知道在哪里找）。

[配置参考](https://github.com/ssnhd/rime)

## Setting

由于上面提到的配置参考是主要设置的全拼的，而本人是使用双拼，以及还有一些其他的个性化需求，则进一步配置。

### Vim Mode

主要针对 *VSCode*、*Obsidian*、*Warp*、*Terminal* 添加了配置。

在 `squirrel.custom.yaml` 中的 `app_options` 中添加：

```yaml
    com.apple.Terminal:              # 终端
      ascii_mode: true
      vim_mode: true                 # 开启vim mode 后，使用 Esc / ctrl + [ 退出 vim 模式时会自动切换为英文输入法
    com.nektony.App-Cleaner-SII:     # App Cleaner & Uninstaller
      ascii_mode: true
    com.microsoft.VSCode:            # VScode
      ascii_mode: true
      vim_mode: true
    md.obsidian:                     # Obsidian
      ascii_mode: true
      vim_mode: true
    dev.warp.Warp-Stable:            # Warp
      ascii_mode: true
      vim_mode: true
```
由于原来的配置中已经有设置了一些其他的软件，可以根据自己的需求把原有的删掉。

#### 如何添加其他的软件？

以 VSCode 为例：
- 在 `finder` 中的 `应用程序` 中找到 `VSCode`，在图标上点击右键，选择`显示包内容`，
- 在 `contents` 内有个 `infop.list` 文件, 打开该文件（可以用vscode打开）找到 `CFBundleIdentifier` 对应的值，如 `VSCode` 的为 `com.microsoft.VSCode`，这个就是上面要设置对应软件时用到的值。

### 模糊输入

由于本人是 lanfan 人，普通话太普通，分不清翘舌，前后鼻音等，故需要此配置。

根据上面提到的配置参考，本人原本参考着 `luna_pinyin_simp.custom.yaml` 中的模糊拼音设置在 `double_pinyin_flypy.custom.yaml` 的，但是无法起效，请看[本人问题描述](https://gist.github.com/lotem/2320943?permalink_comment_id=4472421#gistcomment-4472421)，最终无法，配置在了 `double_pinyin_flypy.schema.yaml` 中。

把以下这段添加到 `speller` 的 `algebra` 中，注意要在 `xfrom*` 的配置前：
```yaml
    # 模糊音定義先於簡拼定義，方可令簡拼支持以上模糊音
    - derive/^([zcs])h/$1/ # zh, ch, sh => z, c, s
    - derive/^([zcs])([^h])/$1h$2/ # z, c, s => zh, ch, sh
    - derive/([aei])ng$/$1n/ # ang =>an, eng => en, ing => in
    - derive/([aei])n$/$1ng/ # an => ang, en => eng, in => ing
    - derive/^n/l/  # n => l
    - derive/^l/n/  # l => n 
```

如我的配置：
```yaml
speller:
  alphabet: zyxwvutsrqponmlkjihgfedcba
  delimiter: " '"
  algebra:
    - erase/^xx$/
    - derive/^([jqxy])u$/$1v/
    - derive/^([aoe])([ioun])$/$1$1$2/
    
    # 模糊音定義先於簡拼定義，方可令簡拼支持以上模糊音
    - derive/^([zcs])h/$1/ # zh, ch, sh => z, c, s
    - derive/^([zcs])([^h])/$1h$2/ # z, c, s => zh, ch, sh
    - derive/([aei])ng$/$1n/ # ang =>an, eng => en, ing => in
    - derive/([aei])n$/$1ng/ # an => ang, en => eng, in => ing
    - derive/^n/l/  # n => l
    - derive/^l/n/  # l => n 

    - xform/^([aoe])(ng)?$/$1$1$2/
    - xform/iu$/Q/
    - xform/(.)ei$/$1W/
    - xform...
```
