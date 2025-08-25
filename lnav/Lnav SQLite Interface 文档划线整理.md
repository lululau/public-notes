---
date: 2025-08-18
---

---

### **1. 数据表中的每个日志格式都有对应的虚拟表**
**上下文:**
在使用 **lnav** 的 SQLite 接口分析日志时，每个**日志格式** (format) 都有一个对应的虚拟表 (virtual table) ，这些表包含日志中每一条解析后的消息，并以**行**的形式进行访问。

**高亮内容:**
"The tables have the same name as the log format and each message is its own row in the table"

**示例:**
对于 Apache 的访问日志，在 LNAV 中，会对应一个名为 **access_log** 的表，如下日志信息：
```
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326
```
这条日志就会对应到 **access_log** 表中的一条行数据，并且解析后的字段可以直接作为列查询使用。

---

### **2. 隐藏列的说明**
**上下文:**
某些列默认隐藏，但可以通过显式查询访问，如 `log_path`、`log_text`、`log_body` 和 `log_raw_text`。

**高亮内容:**
"hidden columns: **log_path**, **log_text**, **log_body**, and **log_raw_text**" (在文档中被多个 `markerow8` 高亮)

**示例:**
若要查询这些隐藏列，可以显式地在 SQL 查询中指定：
```sql
SELECT log_path, log_text, log_body, log_raw_text FROM access_log;
```

---

### **3. 返回到日志视图**
**上下文:**
在执行完 SQL 查询之后（例如查看数据库结果），可以使用特定的键（`q` 或 `v`）切换回日志视图。

**高亮内容:**
"Press **q** to return to the log view and press **v** to return to the log view." *(被 markerow8 标记)*

**示例:**
执行 SQL 查询后，按下：
- `q`: 退出查询结果视图，返回日志消息视图。
- `v`: 可进一步操作，在日志视图中切换。

**注意:**
- `q` 和 `v` 功能可能有所区分。若使用 `v` 仅是切换当前视图，则可以保留原始查询状态。

---

### **4. 环境变量访问**
**上下文:**
在 LNAV 的 SQL 查询中，可以使用类似 `$VAR_NAME` 的语法来访问环境变量。

**高亮内容:**
"Environment variables can be accessed in queries using the usual syntax of **$VAR_NAME**."
(被 markerow8 高亮：https://example.com 格式中环境变量访问)

**示例:**
查询系统中的环境变量 `USER`：
```sql
SELECT $USER;
```

---

### **5. Attach 数据库**
**上下文:**
可以将一个 SQLite 数据库文件附加 (ATTACH) 到当前的 LNAV 查询中，并通过指定前缀 (schema-name) 对其进行引用。

**高亮内容:**
"Attach a database file to the current connection." (被 markerow8 高亮：附加数据库操作说明)

**示例:**
附加数据库文件 `/tmp/customers.db` 到当前会话，并命名为 **customers**:
```sql
ATTACH DATABASE '/tmp/customers.db' AS customers;
```

---

### **6. GLOB 匹配**
**上下文:**
SQLite 提供的 GLOB 运算符，可以用于通配符匹配（与 LIKE 类似，但更贴近 *nix 文件过滤中的 glob 规则）。

**高亮内容:**
"Match an expression against a glob pattern." (被 markerow8 高亮：`GLOB` 函数的功能说明)

**示例:**
匹配 `*.log` 文件名模式：
```sql
SELECT 'foobar.log' GLOB '*.log';  -- 结果为 1 表示匹配成功
```

---

### **7. REGEXP 表达式**
**上下文:**
通过 `REGEXP` 关键字，可以使用正则表达式对字段进行匹配操作。

**高亮内容:**
"Match an expression against a regular expression." (被 markerow8 高亮：正则支持说明)

**示例:**
匹配字符串 `file-23` 是否匹配 `file-\d+` 正则：
```sql
SELECT 'file-23' REGEXP 'file-\d+';
-- 返回 1 表示匹配成功
```

---

### **8. 日志列**
**上下文:**
LNAV 提供多类型解析后的日志字段，包括基础列如 `log_line` 和隐藏列如 `log_path`/`log_raw_text` 等。

**高亮内容:**
"built-in columns", "hidden by default" (被 markerow8 覆盖：日志数据库内建列)

**示例:**
查询日志的完整原始内容：
```sql
SELECT log_line, log_time, log_path, log_raw_text
FROM access_log
WHERE log_line = 0;
```

---

### **9. PRQL 集成**
**上下文:**
LNAV 支持替代性查询语言 **PRQL** (v0.12.1+)，它相比 SQL 提供更直观的语法并可进行可视化。

**高亮内容:**
"Log analysis in **lnav** can be done using the SQLite interface."

**示例:**
PRQL 查询日志中的 `c_ip` 字段平均值和最大值：
```elm
from access_log | stats.by c_ip { average sc_bytes, max sc_bytes }
```

---

### **10. 样式化表字段（table styling）**
**上下文:**
可以通过添加 `__lnav_style__` 列，使用样式化（JSON 格式）自定义数据库查询结果视图。

**高亮内容:**
"You can include style by adding `__lnav_style__` column"

**示例:**
为 `cs_uri_stem` 列加上语义高亮：
```sql
SELECT cs_uri_stem, sc_bytes
FROM access_log
ORDER BY sc_bytes DESC
LIMIT 10
```
并应用样式:
```json
{
    "columns": {
        "cs_uri_stem": {
            "color": "semantic()"
        }
    }
}
```

---

### **11. SQLite Collators - 字符串自然排序 (natteralcase)**
**上下文:**
LNAV 的 SQLite 扩展提供多个 **collators**，例如 `naturalcase` 按照自然方式排序字符串。

**高亮内容:**
"Compare strings naturally so that number values in the string are compared based on their numeric value..."
(被标记：`naturalcase` 与 `naturalnocase` 的说明)

**示例：**
```sql
SELECT 'a2' < 'a10' COLLATE naturalnocase;  -- 返回 1 表示正确排序
```

---

### **12. 使用 SELECT 筛选日志列信息**
**上下文:**
使用 `SELECT` 可以查询日志表格的特定字段组合（显式列筛选）。

**高亮内容:**
"Use the `.schema` command in the SQL prompt to examine the database schema."

**示例:**
查询 Apache 访问日志中的 IP 地址 (`c_ip`) 和响应大小 (`sc_bytes`):
```sql
SELECT c_ip, sc_bytes, log_time
FROM access_log
WHERE sc_bytes > 2000
ORDER BY sc_bytes DESC;
```

---

这些高亮部分提供了 LNAV 使用 SQLite 接口进行日志分析的结构和关键函数，帮助我们更高效地筛选、处理和可视化日志数据。以下内容也可以结合实践进行扩展：

#### 扩展：如何利用 GLOB 模式进行日志搜索
```sql
SELECT log_raw_text
FROM access_log
WHERE log_path GLOB '/var/log/apache/*.log';
```

#### 扩展：日志中的 IP 匹配（ipv6 & ipv4）
```sql
SELECT log_time, c_ip, sc_status
FROM access_log
WHERE c_ip COLLATE ipaddress = '192.168.0.1';
```

#### 扩展：使用样式化优化结果展示
```elm
from access_log
| select c_ip, sc_status
| style.c_ip { color: red, background-color: white }
```

#### 扩展：环境变量与查询结合
```sql
SELECT c_ip, sc_bytes
FROM access_log
WHERE sc_bytes > $MAX_BYTES_THRESHOLD;
```

这些笔记和高亮部分整理后，对于日志分析和 LNAV 的使用有非常直接的参考价值。


