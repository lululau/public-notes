---
date: 2025-08-18
---
## **1. lnav 脚本概述**

**上下文**
`lnav` 支持将命令和 SQL 查询组合成脚本（`.lnav` 后缀），以自动执行任务，例如分析日志信息、生成报告等。内置脚本如 `partition-by-boot` 可用于根据内核日志划分日志视图。

### **1.1 脚本组成**

#### **支持的脚本语言混合**
- **SQL（半角分号 `;` 开头）**
- **lnav 命令（冒号 `:` 开头）**
- **其他脚本引用（竖线 `|` 开头）**
- **注释（井号 `#` 开头）**

```bash
# 示例脚本片段
# :eval 命令用于变量替换
:eval :filter-out ${pattern}
```

### **1.2 脚本执行方式**
- 可使用完整路径调用脚本，例如：
  ```bash
  | /path/to/script.lnav
  ```
- 或将脚本**安装在格式目录**下，通过基础名调用（无需路径）。
  - 安装方法见：[Installing Format Files](formats.html#installing-format-files)

---

## **2. 脚本中可用的变量**

| 变量名             | 描述                |
| --------------- | ----------------- |
| `0`             | 正在执行的脚本路径         |
| `1-N`           | 传入的参数值            |
| `#`             | 传入参数个数            |
| `__all__`       | 所有参数组合成的字符串（空格分隔） |
| `LNAV_HOME_DIR` | 用户配置目录路径          |
| `LNAV_WORK_DIR` | lnav 缓存工作目录路径     |

> ⚠ 警告：在大多数 `lnav` 命令中引用变量，必须使用 `:eval`。

---

## **3. 脚本帮助信息格式**

**作用**
为脚本添加交互式帮助提示。

**支持标签：**

| 标签             | 示例 |
|------------------|------|
| `@synopsis`      | # @synopsis: hello-world `<name1>` [ `<name2>` ... ]
| `@description`   | # @description: Say hello to the given names. |
| `@output-format` | # @output-format: text/markdown |

**示例注释头：**
```bash
# @synopsis: hello-world <name1> [<name2> ...]
# @description: Say hello to the given names.
# @output-format: text/markdown
```

---

## **4. 实用技巧：变量替换**

### **使用 `:eval` 进行变量替换**

**背景**
某些 `lnav` 命令（如 `:filter-out`）不支持变量直接插入，此时可以使用 `:eval`。

### ✅ 示例：
```bash
:eval :filter-out ${pattern}
```
- `:eval` 会先将 `${pattern}` 替换为变量值，再执行 `:filter-out` 命令。

---

## **5. VSCode 插件支持**

**插件名称**
👉 [lnav VSCode Extension](https://marketplace.visualstudio.com/items?itemName=lnav.lnav)

**功能**
- 为 `.lnav` 脚本文件提供语法高亮，提升编写和调试效率。

---

## **6. 内置脚本参考（Builtin Scripts）**

### **6.1 `find-msg`**
- 用途：查找与当前聚焦日志消息相关联的消息。
- 默认支持字段查找，也可以使用自定义字段。
- [源码地址](https://github.com/tstack/lnav/blob/master/src/scripts/find-msg.lnav)

### **6.2 `find-chained-msg`**
- 与 `find-msg` 类似，但支持将匹配的字段拆分到相邻日志行的不同字段中。
- 适用于链式日志逻辑。
- [源码地址](https://github.com/tstack/lnav/blob/master/src/scripts/find-chained-msg.lnav)

### **6.3 `report-access-log`**
- 用途：基于 `access_log` 表生成类似 `goaccess` 的访问报告。
- 输出可读性高，适合分析访问模式。
- [源码地址](https://github.com/tstack/lnav/blob/master/src/scripts/report-access-log.lnav)

---

## **7. 总结**

| 内容 | 说明 |
|------|------|
| **脚本后缀** | `.lnav` |
| **多语言混合支持** | SQL、lnav 命令、脚本调用 |
| **变量** | 使用 `:eval` 替换 |
| **帮助结构** | 使用注释标签提供交互帮助 |
| **执行方式** | 文件路径或安装目录调用 |
| **格式文件** | 可参考安装说明将其放置在格式目录中 |
| **开发支持** | VSCode 插件提供语法高亮支持 |

---

如需深入使用 `lnav` 脚本功能，建议结合上述内置脚本代码，进行实践调试和二次开发。

