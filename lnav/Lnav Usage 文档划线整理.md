---
date: 2025-08-18
---
### 1. **文件查看与管理**
- **打开文件**
文件可以使用命令行指定，也可以通过 `:open` 命令传递路径：
  ```bash
  lnav /path/to/file.log
  ```
  或
  ```lnav
  :open /path/to/file.log
  ```

- **目录处理**
当路径是**目录**时，目录下的全部文件将被自动加载，并且在运行期间如果目录内容变化（增删文件），将自动监控这些文件并更新视图。

- **压缩文件支持**
LNav 能自动识别并解压**压缩文件或归档文件**（如 tar.gz、zip 等），前提是已编译了 **libarchive** 支持。解压内容会存储到临时文件夹：
  ```
  $TMPDIR/lnav-user-${UID}-work/archives/
  ```
  为提高性能，以下文件会自动隐藏：
  - 二进制文件
  - 大于 128KB 的纯文本文件
  - 重复的日志文件

- **远程文件支持**
通过 SSH 支持远程日志查看。确保 SSH 无需密码验证即可连接。使用以下命令打开远程文件：
  ```bash
  lnav user@remote:/path/to/file.log
  ```

  **注意：**
  1. 若通过 `snap` 安装的 lnav，需手动连接 ssh-keys 接口：
     ```bash
     sudo snap connect lnav:ssh-keys
     ```
  2. 远程传输使用的是 [αcτµαlly pδrταblε εxεcµταblε](https://justine.lol/ape.html)（APE 二进制文件）实现，它是一个独立可执行文件，跨平台兼容（支持 Linux、Mac、Windows、FreeBSD 等）。
     APE 二进制文件名如：`tailer.bin.XXXXXX`，正常运行下在执行结束后自动删除。

---

### 2. **Docker日志查看**
LNav 可直接查看 Docker 日志，使用 `docker://` URL scheme，如：
```lnav
docker://container_name:/path/to/logs
```

例如：
```bash
lnav docker://mycontainer
```

将运行 `docker logs mycontainer`。若提供路径（如日志文件），LNav 会使用 `docker exec` 并通过 `tail -F` 操作实时查看容器内日志文件。

---

### 3. **高亮搜索与跳转**
LNav 支持基于日志级别（error、warning 等）和时间的高亮：
- `e`：跳转到下一条 error 日志
- `Shift + e`：跳转至上一条 error 日志
- `/`：开始正则匹配搜索
- `t`：切换到纯文本次视图

---

### 4. **过滤与隐藏信息**
LNav 提供多种方式过滤日志，减少干扰：

#### 4.1 正则表达式过滤
- `:filter-out PATTERN`：隐藏符合正则表达式的日志
- `:filter-in PATTERN`：仅显示符合正则日志

#### 4.2 SQL 表达式过滤
支持使用 SQLite 表达式过滤日志：
```lnav
:filter-expr sqlite_expression
```

#### 4.3 时间过滤
限制日志显示的时间范围：
- `:hide-lines-before 2024-10-01`：仅显示指定日期之后的日志
- `:hide-lines-after 2024-10-30`：仅显示指定日期之前的日志

#### 4.4 日志等级过滤
按日志级别过滤，例如隐藏“info”级别以下日志：
```lnav
:set-min-log-level info
```

---

### 5. **标记与注释**
在浏览日志时，可标记或添加注释以记录重要发现。

#### 5.1 标记日志
使用 `:tag` 命令为日志行添加标签（tag）：
```lnav
:tag error-line
```
按下 `u` 或 `Shift + u` 可在标记者间快速跳转

#### 5.2 添加注释
使用 `:comment` 为日志添加自由格式注释：
```lnav
:comment This is a critical error.
```

支持基本 Markdown 格式，支持使用链接跳转到其他日志行。

#### 5.3 分组/分区日志
通过定义 **log partition** 可对日志进行上下文分区：
```lnav
:partition-name TestRun1
```
在状态栏显示当前分组，方便浏览大型日志序列。

#### 5.4 使用 SQLite 管理标记与注释
可以使用 SQL 对日志表进行更新：
- 更新 `log_tags` 添加标签
- 更新 `log_comment` 添加注释
- 更新 `log_mark` 作为书签

例如，标记所有包含 "something interesting" 的日志行：
```sql
UPDATE all_logs SET log_mark = 1 WHERE log_body LIKE '%something interesting%';
```

更高级示例：
自动标记 dhclient IP 地址变更的日志行：
```sql
-- 创建搜索表提取 IP
CREATE SEARCH TABLE dhclient_ip AS 'bound to (?<ip>\\S+)';

-- 创建视图比较前后行 IP
CREATE VIEW dhclient_ip_changes AS
SELECT log_line, ip, lag(ip) OVER (ORDER BY log_line) AS prev_ip FROM dhclient_ip;

-- 更新日志行，添加 #ipchanged 标签
UPDATE syslog_log
SET log_tags = json_concat(log_tags, '#ipchanged')
WHERE log_line IN (SELECT log_line FROM dhclient_ip_changes WHERE ip != prev_ip);
```

---

### 6. **导出会话与协作**
使用 `:export-session-to` 导出当前配置，生成一个可运行脚本，包含：
- 文件加载命令
- SQL 过滤及注释规则
- 高亮设置

导出脚本后可分享，他人仅需运行即可还原你的日志分析环境。

---

### 7. **自定义 URL scheme**
LNav 支持自定义 URL Scheme，用于灵活定义远程日志或外部数据加载方式。例如，`docker://` 就是内置示例。配置见 `/tuning/url-schemes`，支持使用脚本处理 URL。

---

### 8. **其他技巧**
- 设为终端的默认 `PAGER`，获得日志高亮和结构化导航：
  ```bash
  export PAGER="lnav -q"
  ```

- **命令行调用**：
  ```bash
  lnav -e 'your shell command'
  ```

  显示执行结果并自动跳转错误高亮等。

---

该高亮整理涵盖使用 **LNav** 常用的核心功能，包括远程日志查看、SQL 操作、注释标记、自动日志分析脚本等实用技巧。


