---
date: 2025-08-17
---

---

### 🔹 **-c 选项**（Command 执行）
- **描述**：执行 `lnav` 命令、SQL 查询或脚本。
- **格式**：`-c <command>`
- **注意**：
  - 命令类型需用特定前置字符区分，例如：
    - `:` 表示输入命令；
    - `;` 表示 SQL 查询；
    - `|`、`/` 等也可能有不同含义；
  - 此选项可以出现多次；
- **示例**：
  ```bash
  lnav -c ":goto 1234" -c ";SELECT * FROM log";
  ```
- **来源标记**：
  > <markerow8>Execute the given lnav command, SQL query, or lnav script.  The argument must be prefixed with the character used to enter the prompt to distinguish between the different types (i.e. :, ;, |, /). This option can be given multiple times.</markerow8>

---

### 🔹 **-n 选项**
- **描述**：在无 curses UI 模式下运行 (`headless mode`)。
- **用法**：通常用于脚本或非交互模式。
- **来源标记**：
  > <markerow8>Run without the curses UI (headless mode).</markerow8>

---

### 🔹 **-r 选项**
- **描述**：从给定的基目录中**递归加载文件**。
- **用法**：适用于需要一次性加载目录树下多个日志文件的情况。
- **来源标记**：
  > <markerow8>Recursively load files from the given base directories.</markerow8>

---

## ✅ 其他补充说明
- **CLI 两种模式**：
  - 默认模式（文件查看模式）；
  - 管理模式（`-m` 选项，用于管理配置或 log format）；
- **命令执行方式**：
  - `-c` 用于执行单条命令、SQL 或脚本；
  - `-f` 用于执行命令文件；
  - `-e` 用于执行 Shell 命令并展示输出（等同于 `:sh`）；
  - `-i` 可用于安装 log 格式、SQL、脚本到 `~/.lnav`；
- **调试与配置**：
  - `-d <path>` 用于写入调试日志；
  - `-I <path>` 添加配置目录；
  - `-v` 输出更多信息；`-q` 静默操作；
  - `-C` 用于检查配置是否错误；
  - `-u` 更新配置中已安装的 Git 格式；

---

## 📝 示例高亮
### ✅ 使用 `-c` 执行多条命令/SQL
```bash
lnav -c ":goto 100" -c ";SELECT * FROM error_log WHERE level='ERROR'"
```

### ✅ 启动 headless 模式分析日志并输出结果：
```bash
lnav -n -c ";EXPORT TO output.csv SELECT * FROM logs WHERE severity >= 3" access_log.log
```

### ✅ 从目录中递归加载所有日志文件：
```bash
lnav -r /var/log/myapp
```

---

如需了解更多命令和子命令（如管理模式下的 `format test`、`config blame` 等），可参考完整文档的 `lnav --help` 或访问官方网站。

