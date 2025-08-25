---
date: 2025-08-18
---
---

# ✅ lnav 键位映射定义（Keymap Definitions）整理笔记

## 🔹 概述

在 [lnav](https://lnav.org/) 中，** Keymap **将指定的按键序列（key sequence）映射到一条命令（command）上，允许用户通过键盘快捷键快速执行任务。

每当你按下键盘上的某个键时，**lnav 会将按键转换为一个十六进制编码的字符串**，并在键位映射中查找匹配项。匹配成功后，就会执行该键对应的命令。

## 🔹 特点概览

- **支持命令类型**：
  - `lnav` 自带的命令（如 `:goto next hour`）
  - SQL语句或查询
  - 外部脚本（Script）
- **高亮提示**：
  - 可以在键位映射中添加 `alt-msg` 字段，用于在界面底部右侧显示帮助信息
- **调试辅助**：
  - 若按下的是未绑定的按键，lnav 会在命令提示符显示配置方法（可用于绑定该按键）

---

## 📌 键位编码规则（Key Sequence Encoding）

每个按键都会被转换成一段可以用于查找的字符串。规则如下：

| 按键类型     | 编码方式说明                                                | 示例                   |
|--------------|-------------------------------------------------------------|------------------------|
| 功能键 (F1~F12) | 使用 `f` 加数字编号表示功能键                                | `f5` 表示 F5 键         |
| 其他普通键     | 按 UTF-8 字节以小写形式进行编码，并在前面加上 `x`             | 字符 `£` 转换为 `xc2xa3` |

> **💡 提示**：`lnav` 会将未映射的按键信息显示在命令框中，包括：
> - 配置命令（`:config`）
> - JSON 指针位置（用于修改配置文件）

![](https://lnav.org/images/key-encoding-prompt.png)
*Screenshots 显示未绑定键触发配置提示*

---

## 🔧 示例：自定义一个键位映射

### 🧩 配置结构

```json
{
  "/ui/keymap-defs/normal": {
    "x2f": {
      "command": ":goto next hour",
      "alt-msg": "Jump to next hour in log."
    },
    "f5": {
      "command": ":execute-sql SELECT * FROM log_body LIMIT 10",
      "alt-msg": "Run sample SQL query"
    },
    "x3c": {
      "command": ":source ~/.lnav/scripts/clear-log.sh",
      "alt-msg": "Clear current log view"
    }
  }
}
```

### 📑 JSON 字段解释

| 字段        | 类型   | 说明                                                   | 示例值                     |
|-------------|--------|--------------------------------------------------------|----------------------------|
| `<key_seq>` | object | 表示一个键序列的映射。键名为按键的 hex 编码            | `"x2f"`、`"f5"` 等         |
| `command`   | string | 要运行的命令。可为 lnav 命令、SQL语句或脚本路径         | `":goto next hour"`        |
| `alt-msg`   | string | 命令执行后显示的帮助信息                               | `"Jump to next hour..."`   |

---

## ⚠ 注意事项

1. **不支持所有功能映射**
   并非所有 lnav 的功能都可通过命令行或 SQL 暴露出来，因此某些功能可能无法绑定快捷键。

2. **不能监听修饰键（modifier keys）**
   因为 lnav 是终端应用，它无法检测单独的 `Shift/Ctrl/Alt` 等修饰键的按下动作。

3. **键重复编码问题**
   终端传输的按键可能受输入法、终端模拟器影响，同一物理按键在不同系统/环境下的编码可能不同。

---

## 📚 相关参考

- 官方网站：[https://lnav.org/](https://lnav.org/)
- 配置帮助：[Keymap Definitions](#keymap-definitions)
- SQL 扩展文档：[SQL Extensions](sqlext.html#sql-ext)
- JSON Pointer 规范：[RFC 6901](https://tools.ietf.org/html/rfc6901)

---

## 📝 总结

在 `lnav` 中使用 **键位映射（Keymap）** 可以显著提高你的使用效率。通过自定义键序列，你可以快速访问：

- lnav 内建命令
- SQL 查询脚本
- 外部 Shell 或 Lua 脚本

记得利用 `lnav` 提供的调试功能（如自动提示 hex 编码），可以很方便地为你的常用操作绑定快捷键，实现更流畅的日志浏览体验。

---

如需进一步了解自定义脚本绑定或其他配置，可以继续查阅 `lnav` 的官方文档或 `.lnav/config` 子模块配置示例。


