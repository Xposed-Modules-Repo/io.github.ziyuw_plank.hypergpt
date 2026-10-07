# hyperGPT

> 长按电源键唤起 ChatGPT 语音，代替小爱同学。
>
> 源码 / Releases：<https://github.com/ziyuw-Plank/hyperGPT>

English: LSPosed module for Xiaomi HyperOS (China). Long-press power opens ChatGPT Voice instead of Xiao Ai. System Framework only; power menu still works (~3s hold).

## 要求

- 小米 / 红米，**HyperOS 国行**
- Root + **LSPosed / Vector**
- 已安装并登录过 **ChatGPT**

## 安装

1. 安装本模块（若有旧包名 `io.github.zyw.powergpt`，先卸载）
2. LSPosed → 启用 **hyperGPT** → 作用域只勾 **系统框架**
3. 设置 → 更多设置 → 快捷手势 → 长按电源键 → **唤醒小爱同学**
4. **重启**

长按电源键 → ChatGPT；继续按住约 3 秒 → 关机菜单。

## 可选开关

```sh
su -c 'setprop persist.sys.powergpt.enable 0'          # 临时恢复小爱
su -c 'setprop persist.sys.powergpt.keyguard xiaoai'   # 锁屏仍用小爱
```

## 回滚

LSPosed 中停用（或卸载）→ 重启。

源码与更新：<https://github.com/ziyuw-Plank/hyperGPT>
