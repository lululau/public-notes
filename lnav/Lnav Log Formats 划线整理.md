---
date: 2025-08-18
---

---

### 被高亮标记的部分：
1. `body-field` 的说明
2. `opid-field` 的说明
3. `tags` 对象的说明
4. 示例命令安装格式文件

---

## 1. `body-field`
### 描述：
- `body-field` 字段指定了包含日志消息主体内容的字段名。
- 默认值为 `"body"`。
- 此字段在日志分析中非常关键，因为它将整个主文本内容作为解析日志的依据。

### 上下文：
该字段适用于 JSON 编码的日志，可以指定包含消息主体的字段名称。如果没有显式定义 `body-field`，`lnav` 将自动使用默认字段 `body`。如果定义了其他字段（如 `msg` 或 `log`），则会使用这些字段作为主体。

### 示例说明：
```json
{
  "example_log": {
    "title": "Example Log Format",
    "description": "Log format used in the documentation example.",
    "body-field": "body",
    "regex": {
      "basic": {
        "pattern": "^(?P<timestamp>\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}Z) (?P<body>.*)$"
      }
    }
  }
}
```
在这个示例中，`body-field` 被设置为 "body"，而 `body` 捕获了日志行中的剩余字符内容，成为主要消息体。

---

## 2. `opid-field`
### 描述：
- `opid-field` 指定了日志中的 "操作 ID" 字段。
- 通过操作 ID，用户可以跟踪某个操作、事务或请求在整个日志系统中的执行过程。
- 用户可以通过快捷键 `o` 或 `Shift+O` 向前/向后跳转查看具有相同操作 ID 的日志条目。

### 上下文：
该字段多用于 JSON 编码日志中，以通过唯一的标识符（如事务 ID）来追踪操作，尤其适用于分布式系统的日志分析。

### 示例说明：
```json
{
  "example_log": {
    "opid-field": "transaction_id",
    "regex": {
      "basic": {
        "pattern": "^(?P<timestamp>\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}Z) \\[ID=(?P<transaction_id>.+?)\\] (?P<body>.*)$"
      }
    }
  }
}
```
在这个示例中，日志的结构包含事务 ID (`transaction_id`)，`opid-field` 设定为该字段后，用户可以使用 `o` 查看具有相同 ID 的日志，帮助分析事务执行过程。

---

## 3. `tags` 对象
### 描述：
- `tags` 对象包含日志应该自动添加的标签。
- 通过正则表达式定义标签，匹配某行后，整个日志条目会附加特定的标记。
- 标签可以用于快速筛选或分类日志条目，有助于快速分析。

### 上下文：
标签是日志处理的补充功能，可以对日志根据内容添加元信息。例如，匹配错误信息时添加 "ERROR" 标签，或者根据路径规则对特殊文件生成标签。

### 示例说明：
```json
{
  "example_log": {
    "tags": {
      "error_lines": {
        "pattern": "(?i)error",  // 忽略大小写的 error 匹配
        "paths": [
          {
            "glob": "/var/log/error.log"  // 仅对某个特定路径匹配
          }
        ]
      }
    }
  }
}
```
在此配置中，所有符合正则模式 `error` 的日志条目（不区分大小写）将被打上 "error_lines" 标签，并且仅适用于 `/var/log/error.log` 这一路径的日志。

---

## 4. `format 文件安装`
### 描述：
- 在 `lnav` 中，可以用命令行或可执行 JSON 文件安装格式文件。
- 安装路径包括 `/etc/lnav/formats` 和 `~/.lnav/formats/`。
- 使用命令行选项 `-i` 安装 JSON、SQL 或 `.lnav` 文件。

### 上下文：
用户可以手动或通过工具安装和更新格式文件，确保对特定日志进行解析。这种方式简化了日志格式的扩展支持，尤其是在处理自定义或特殊格式日志时。

### 示例说明：
#### 使用命令安装格式文件：
```bash
# 通过命令安装文件：
lnav -i myformat.json
```
输出：
```
info: installed: /home/example/.lnav/formats/installed/myformat_log.json
```

#### 通过可执行的 JSON 文件安装：
```bash
# 在 JSON 前加入 shebang:
#!/usr/bin/env lnav -i
{
  "myformat_log": {
    ...
  }
}

# 赋予执行权限并执行：
chmod +x myformat.json
./myformat.json
```
输出：
```
info: installed: /home/example/.lnav/formats/installed/myformat_log.json
```

---

## 总结表格

| 高亮部分           | 作用                                     | 示例字段值           |
|--------------------|------------------------------------------|------------------------|
| `body-field`       | 指定日志主体字段名称                     | body, msg, log         |
| `opid-field`       | 跟踪操作 ID，支持跳转查看同一操作日志     | transaction_id, opid   |
| `tags`             | 定义自动添加的标签，方便筛选日志          | error_lines, warning   |
| 安装配置命令       | 手动或执行 JSON 文件安装格式支持          | `lnav -i file.json`    |

---

这些被高亮的内容为日志的解析、分类以及操作提供了实用的功能，能够显著提高使用 `lnav` 工具进行日志分析的效率和准确性。


