# hyperGPT

> 长按电源键唤起 ChatGPT 语音，代替小爱同学。
>
> 源码：<https://github.com/ziyuw-Plank/hyperGPT>

English: an LSPosed module for Xiaomi HyperOS (China ROM) that makes a long press of the power key open **ChatGPT Voice** instead of Xiao Ai. It runs only in System Framework, modifies no partitions, and the power menu still appears when you keep holding the key for about 3 seconds.

## 功能

把 HyperOS「长按电源键 → 唤醒小爱同学」换成打开 ChatGPT 语音助手
（`com.openai.chatgpt/com.openai.voice.assistant.AssistantActivity`）。

- 只替换「启动小爱」这一个动作，按键事件、长按时长和关机菜单逻辑都不改。
- **继续按住约 3 秒，关机菜单照常弹出**。
- ChatGPT 未安装、组件不存在或启动失败时，**自动回退到原版小爱**，电源键不会失灵。
- 锁屏时启动 ChatGPT 并弹出解锁界面，验证通过后显示 ChatGPT（不会绕过锁屏验证）。
- 纯 LSPosed 模块：不改 system / vendor / boot 等任何分区，不写系统设置或文件；无界面、无权限、无网络。

## 要求

- 小米 / 红米设备，**HyperOS 1 / 2 / 3 国行（China）ROM**（Android 14 – 16）
- Root + **LSPosed / Vector**（或其它兼容 Xposed API 93+ 的框架）
- 已安装 **ChatGPT**（`com.openai.chatgpt`），并且至少打开、登录过一次

> 国际版 HyperOS 的长按电源键通常是「Google 助理」，不经过小爱路径，本模块不处理。

## 使用方法

1. 安装本模块，在 LSPosed 管理器中启用 **hyperGPT**。
2. 作用域 **只勾选「系统框架 / System Framework」**（启用时会自动勾上），不要勾选其它 App。
3. 系统设置：**设置 → 更多设置 → 快捷手势 → 长按电源键 → 选「唤醒小爱同学」**
   （部分版本在 设置 → 小爱同学 → 电源键唤醒）。
   可用命令确认：`su -c 'settings get system long_press_power_key'` 应输出 `launch_voice_assistant`。
4. **重启手机**。

之后：短按住电源键 → ChatGPT 语音；继续按住到约 3 秒 → 关机菜单。

## 可选开关（系统属性，立即生效，无需重启）

```sh
# 临时关闭模块逻辑（完全恢复原版小爱）；改回 1 重新启用
su -c 'setprop persist.sys.powergpt.enable 0'
su -c 'setprop persist.sys.powergpt.enable 1'

# 锁屏时仍用原版小爱（默认 unlock = 启动 ChatGPT 并弹出解锁界面）
su -c 'setprop persist.sys.powergpt.keyguard xiaoai'
```

## 验证

LSPosed 管理器 → 日志 → 模块日志，搜索 `[PowerGPT]`：

- 开机后应有 `[PowerGPT] hooked ... launchVoiceAssistant(...)`
- 长按电源键后应有 `[PowerGPT] launched ChatGPT voice (long_press_power_key, user 0)`
- 完全没有 `[PowerGPT]`：检查模块是否启用、作用域是否勾了「系统框架」、是否已重启。

## 回滚

- 在 LSPosed 中停用本模块（或直接卸载）→ 重启即可，不留任何残留。
- 只想临时恢复小爱：`su -c 'setprop persist.sys.powergpt.enable 0'`，无需重启。
- 万一无法开机（理论上不应发生，所有 Hook 都有异常保护）：进入 Magisk / KernelSU / APatch 安全模式（开机时按住或连按「音量-」）禁用 LSPosed，再停用本模块。

## 反馈与源码

- 源码、详细原理、问题反馈：<https://github.com/ziyuw-Plank/hyperGPT>
- 作者：[ziyuw-Plank](https://github.com/ziyuw-Plank)
