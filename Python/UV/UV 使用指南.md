---
date: 2025-03-08
---
## 1. 列出系统中安装的 Python 版本

```shell
uv python list
```

修改默认 Python 版本

方法1:
```shell
echo 'export UV_PYTHON=3.11' >> ~/.zshenv'

```

方法2:

```shell
# Writes the pinned Python version to a `.python-version` file in the uv user configuration directory: `XDG_CONFIG_HOME/uv` on Linux/macOS and `%APPDATA%/uv` on Windows.
uv python pin --global 3.11
```

## 2. uv run 命令

### 2.1 运行脚本

```shell
uv run example.py
```

### 2.2 运行指定版本的 Python

```shell
uv run --python 3.11 example.py
```

### 2.3 为指定命令安装依赖

```shell
uv run --with requests example.py
```

## 3. Script Dependency

```shell
uv add --script example.py requests
```

## 4. tool 子命令

### 4.1 临时运行一个命令 (在一个临时虚拟环境中安装包及其依赖)

```shell
uvx run
# 等价于：
uv tool run
```

### 4.2 uvx: 当命令和包名不一致时

```shell
uvx --from httpie http
```

### 特定包版本

```shell
uvx ruff@0.3.0

uvx ruff@latest
```

### 从 github 安装

```shell
uvx --from git+https://github.com/httpie/cli@master httpie
```

### 附加的依赖

```shell
uvx --with mkdocs-material mkdocs --help
```
### 4.3 运行指定版本的 Python

```shell
uvx python@3.12 -c "print('Hello, World!')"
```

### 4.4 在固定环境中安装一个命令

```shell
uv tool install mysql-mcp-server
```

### 4.5 从 GitHub 安装

```shell
uv tool install git+https://github.com/mysql/mysql-mcp-server
```

## 5. 项目管理

### 5.1 初始化

```shell
uv init

uv init -p 3.11  # 指定 pyproject.toml 和 .python-version 中的 Python 版本
```

### 5.2 添加依赖

```shell
uv add requests
```

### 5.3 删除依赖

```shell
uv remove requests
```

### 5.4 在项目环境中运行命令

```shell
uv run python --version
```

### 5.5 运行项目中定义的脚本

```toml
[tool.uv.scripts]
hello = "hello.cli:main"
```

```shell
uv pip install -e .
uv run hello
```

### 5.6 查看项目的依赖树

```shell
uv tree
```

### 5.7 构建和发布项目

```shell
uv build

uv publish
```

### 5.8 锁定项目 Python 版本

```shell
uv python pin 3.11
```


### 5.9 生成 .lock 文件

```shell
uv lock
```


### 5.10 为已有项目安装依赖

```shell
uv sync
```


## 6. 查看目录

```shell
uv cache dir
uv tool dir
uv python dir
```


## 7. 设置镜像源

在 `~/.config/uv/uv.toml` 或者 `/etc/uv/uv.toml` 填写下面的内容：

### 阿里源

```toml
[[index]]
url = "https://mirrors.aliyun.com/pypi/simple/"
default = true
```
### 清华源

```toml
[[index]]
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple/"
default = true
```
