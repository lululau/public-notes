---
date: 2025-08-13
---

# UV 工具使用指南

## 📖 简介

### 为脚本添加依赖

```shell
uv add --script demo.py requests
```

### 使用 uvx 在临时环境中运行工具

`uvx` 是 `uv tool run` 的别名。

```shell
uvx pycowsay "Hello, World!"
```

### 在当前目录中使用特定的 Python 版本

将 `.python-version` 固定为 `3.11`

```shell
uv python pin -p 3.11
```

## 🚀 核心功能

### Python 版本管理

安装和管理 Python 本身。

-   `uv python install`: 安装 Python 版本
-   `uv python list`: 查看可用的 Python 版本
-   `uv python find`: 查找已安装的 Python 版本
-   `uv python pin`: 将当前项目固定到使用特定的 Python 版本
-   `uv python uninstall`: 卸载 Python 版本

### 脚本执行

执行独立的 Python 脚本，例如 `example.py`。

-   `uv run`: 运行脚本
-   `uv add --script`: 为脚本添加依赖
-   `uv remove --script`: 从脚本中移除依赖

### 项目管理

创建和开发 Python 项目，即带有 `pyproject.toml` 的项目。

-   `uv init`: 创建新的 Python 项目
-   `uv add`: 为项目添加依赖
-   `uv remove`: 从项目中移除依赖
-   `uv sync`: 同步项目的依赖与环境
-   `uv lock`: 为项目的依赖创建锁定文件
-   `uv run`: 在项目环境中运行命令
-   `uv tree`: 查看项目的依赖树
-   ==`uv build`: 将项目构建为分发包==
-   `uv publish`: 将项目发布到包索引

### 工具管理

运行和安装发布到 Python 包索引的工具，例如 `ruff` 或 `black`。

-   `uvx` / `uv tool run`: 在临时环境中运行工具
-   `uv tool install`: 全局安装工具
-   `uv tool uninstall`: 卸载工具
-   `uv tool list`: 列出已安装的工具
-   `uv tool update-shell`: 更新 shell 以包含工具可执行文件

## 🔄 自动 Python 下载

使用 uv 时不需要显式安装 Python。默认情况下，uv 会在需要时自动下载 Python 版本。例如，如果未安装 Python 3.12，以下命令会自动下载：

```shell
uvx -p 3.11 python -c "import sys; print(sys.version)"

uvx -p 3.12 python -c "import sys; print(sys.version)"
```

## 📦 构建分发包

`uv build` 可用于为您的项目构建源码分发包和二进制分发包（wheel）。

默认情况下，`uv build` 将在当前目录中构建项目，并将构建的产物放置在 `dist/` 子目录中：

```shell
uv build
ls dist/
```

## 📈 管理项目版本

### ==更新到确切版本，将其作为位置参数提供==

```shell
uv version 1.0.0
```

### ==要增加包的语义版本，使用 `--bump` 选项：==

```shell
uv version --bump minor  # hello-world 1.2.3 => 1.3.0
```

## 🚀 发布包

### 使用 `uv publish` 发布您的包：

使用 `--token` 或 `UV_PUBLISH_TOKEN` 设置 PyPI 令牌，或使用 `--username` 或 `UV_PUBLISH_USERNAME` 设置用户名，使用 `--password` 或 `UV_PUBLISH_PASSWORD` 设置密码。对于从 GitHub Actions 发布到 PyPI，您不需要设置任何凭据。相反，[为 PyPI 项目添加受信任的发布者](https://docs.pypi.org/trusted-publishers/adding-a-publisher/)。

### 发布到自定义索引

如果您通过 `[[tool.uv.index]]` 使用自定义索引，请添加 `publish-url` 并使用 `uv publish --index <name>`。例如：

```toml
[[tool.uv.index]]
name = "testpypi"
url = "https://test.pypi.org/simple/"
publish-url = "https://test.pypi.org/legacy/"
explicit = true
```

## 🔄 从 `pip` 到 `uv` 项目

### `uv.lock` 自动创建/填充

锁定文件将在添加依赖时自动创建和填充，但您可以使用 `uv lock` 显式创建它。

### ==导入 requirements 文件==

```shell
uv add -r requirements.in
uv add -r requirements.txt
```

## 🏗️ 创建项目

### 两个基本模板

创建项目时，uv 支持两个基本模板：[`--app`](https://docs.astral.sh/uv/concepts/projects/init/#applications) 和 [`--lib`](https://docs.astral.sh/uv/concepts/projects/init/#libraries)。默认情况下，uv 将创建一个应用程序项目。可以使用 `--lib` 标志来创建库项目。

### 默认模板

应用程序是 `uv init` 的默认目标，但也可以使用 `--app` 标志指定。

### 打包应用程序

许多用例需要 [打包](https://docs.astral.sh/uv/concepts/projects/config/#project-packaging)。例如，如果您正在创建一个将发布到 PyPI 的命令行界面，或者如果您想在专用目录中定义测试。

==可以使用 `--package` 标志创建打包应用程序：==

```shell
uv init --package example-pkg
```

源代码被移动到 `src` 目录中，其中包含模块目录和 `__init__.py` 文件：

```bash
$ tree example-pkg
example-pkg
├── .python-version
├── README.md
├── pyproject.toml
└── src
    └── example_pkg
        └── __init__.py
```

==使用 `--lib` 意味着 `--package`。库总是需要打包项目。==

## 📦 管理依赖

### 正常依赖规范

```toml
[project]
name = "example"
version = "0.1.0"
dependencies = ["httpx>=0.27.2"]
```

### 替代依赖组

可以使用 [`--dev`](https://docs.astral.sh/uv/concepts/projects/dependencies/#development-dependencies)、[`--group`](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups) 或 [`--optional`](https://docs.astral.sh/uv/concepts/projects/dependencies/#optional-dependencies) 标志将依赖添加到替代字段。

### 来自其他注册表索引的依赖

当从包注册表以外的源添加依赖时，uv 将在 sources 字段中添加条目。例如，当从 GitHub 添加 `httpx` 时：

```shell
uv add "httpx @ git+https://github.com/encode/httpx"
```

`pyproject.toml` 将包含 [Git 源条目](https://docs.astral.sh/uv/concepts/projects/dependencies/#git)：

```toml
[project]
name = "example"
version = "0.1.0"
dependencies = [
    "httpx",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
```

### 更改依赖

要更改现有依赖，例如，为 `httpx` 使用不同的约束：

```shell
uv add "httpx>0.1.0"
```

### 平台特定依赖

要确保依赖仅在特定平台或特定 Python 版本上安装，请使用 [环境标记](https://peps.python.org/pep-0508/#environment-markers)。

```shell
uv add "jax; sys_platform == 'linux'"
```

类似地，要在 Python 3.11 及更高版本上包含 `numpy`：

```shell
uv add "numpy; python_version >= '3.11'"
```

### 索引特定依赖

要从特定索引添加 Python 包，请使用 `--index` 选项：

```shell
uv add torch --index pytorch=https://download.pytorch.org/whl/cpu
```

```toml
[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
```

### Git 依赖

#### 标签

```shell
uv add git+https://github.com/encode/httpx --tag 0.27.0
```

```toml
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", tag = "0.27.0" }
```

#### 分支

```shell
uv add git+https://github.com/encode/httpx --branch main
```

```toml
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", branch = "main" }
```

#### 修订

```shell
uv add git+https://github.com/encode/httpx --rev 326b9431c761e1ef1e00b9f760d1f654c8db48c6
```

```toml
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", rev = "326b9431c761e1ef1e00b9f760d1f654c8db48c6" }
```

### 工作区成员依赖

要声明对工作区成员的依赖，请使用 `{ workspace = true }` 添加成员名称。所有工作区成员必须明确声明。工作区成员总是 [可编辑的](https://docs.astral.sh/uv/concepts/projects/dependencies/#editable-dependencies)。有关工作区的更多详细信息，请参阅 [工作区](https://docs.astral.sh/uv/concepts/projects/workspaces/) 文档。

```toml
[project]
dependencies = ["foo==0.1.0"]

[tool.uv.sources]
foo = { workspace = true }

[tool.uv.workspace]
members = [
  "packages/foo"
]
```

### 可选依赖

发布为库的项目通常会使某些功能可选，以减少默认依赖树。例如，Pandas 有一个 [`excel` 额外功能](https://pandas.pydata.org/docs/getting_started/install.html#excel-files) 和一个 [`plot` 额外功能](https://pandas.pydata.org/docs/getting_started/install.html#visualization)，以避免安装 Excel 解析器和 `matplotlib`，除非有人明确要求它们。使用 `package[<extra>]` 语法请求额外功能，例如 `pandas[plot, excel]`。

可选依赖在 `[project.optional-dependencies]` 中指定，这是一个 TOML 表，从额外功能名称映射到其依赖，遵循 [依赖说明符](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-specifiers-pep-508) 语法。

可选依赖可以在 `tool.uv.sources` 中有条目，与正常依赖相同。

```toml
[project]
name = "pandas"
version = "1.0.0"

[project.optional-dependencies]
plot = [
  "matplotlib>=3.6.3"
]
excel = [
  "odfpy>=1.4.1",
  "openpyxl>=3.1.0",
  "python-calamine>=0.1.7",
  "pyxlsb>=1.0.10",
  "xlrd>=2.0.1",
  "xlsxwriter>=3.0.5"
]
```

要添加可选依赖，请使用 `--optional <extra>` 选项：

```shell
uv add httpx --optional network
```

### 开发依赖

与可选依赖不同，开发依赖是仅本地的，在发布到 PyPI 或其他索引时**不会**包含在项目要求中。因此，开发依赖不包含在 `[project]` 表中。

开发依赖可以在 `tool.uv.sources` 中有条目，与正常依赖相同。

要添加开发依赖，请使用 `--dev` 标志：

```shell
uv add --dev pytest
```

```toml
[dependency-groups]
dev = [
  "pytest >=8.1.1,<9"
]
```

### 环境标记

某些依赖仅在特定环境中需要，例如特定的 Python 版本或操作系统。例如，要为 `importlib.metadata` 模块安装 `importlib-metadata` 回退，请使用 `importlib-metadata >=7.1.0,<8; python_version < '3.10'`。要在 Windows 上安装 `colorama`（但在其他平台上省略），请使用 `colorama >=0.4.6,<5; platform_system == "Windows"`。

标记与 `and`、`or` 和括号组合，例如 `aiohttp >=3.7.4,<4; (sys_platform != 'win32' or implementation_name != 'pypy') and python_version >= '3.10'`。请注意，标记内的版本必须用引号括起来，而标记**外**的版本**不能**用引号括起来。

## 🔒 锁定和同步

### 检查锁定文件是否是最新的

您可以通过向 `uv lock` 传递 `--check` 标志来检查锁定文件是否是最新的：

```shell
uv lock --check
```

### 创建锁定文件

虽然锁定文件是 [自动创建](https://docs.astral.sh/uv/concepts/projects/sync/#automatic-lock-and-sync) 的，但也可以使用 `uv lock` 显式创建或更新锁定文件：

```shell
uv lock
```

### 同步环境

虽然环境是 [自动同步](https://docs.astral.sh/uv/concepts/projects/sync/#automatic-lock-and-sync) 的，但也可以使用 `uv sync` 显式同步：

```shell
uv sync
```

### 导出锁定文件

如果您需要将 uv 与其他工具或工作流集成，可以使用 `uv export --format requirements-txt` 将 `uv.lock` 导出为 `requirements.txt` 格式。生成的 `requirements.txt` 文件然后可以通过 `uv pip install` 安装，或使用其他工具如 `pip`。

## ⚙️ 配置项目

### Python 版本要求

项目可以在 `pyproject.toml` 的 `project.requires-python` 字段中声明项目支持的 Python 版本。

```toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
```

### 命令行界面

```toml
[project.scripts]
hello = "example:hello"
```

```shell
uv pip install -e .  # https://stackoverflow.com/questions/79280838/after-uv-init-adding-script-to-project-scripts-does-not-work
uv run hello
```

## 🏢 使用工作区

在定义工作区时，您必须指定 `members`（必需）和 `exclude`（可选）键，它们指导工作区分别包含或排除特定目录作为成员，并接受 glob 列表：

```toml
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["bird-feeder", "tqdm>=4,<5"]

[tool.uv.sources]
bird-feeder = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
exclude = ["packages/seeds"]
```

由 `members` glob 包含（且不被 `exclude` glob 排除）的每个目录都必须包含 `pyproject.toml` 文件。但是，工作区成员可以是 [应用程序](https://docs.astral.sh/uv/concepts/projects/init/#applications) 或 [库](https://docs.astral.sh/uv/concepts/projects/init/#libraries)；在工作区上下文中都支持两者。

每个工作区都需要一个根，它**也是**一个工作区成员。在上面的示例中，`albatross` 是工作区根，工作区成员包括 `packages` 目录下的所有项目，除了 `seeds`。

默认情况下，`uv run` 和 `uv sync` 在工作区根上运行。例如，在上面的示例中，`uv run` 和 `uv run --package albatross` 将是等效的，而 `uv run --package bird-feeder` 将在 `bird-feeder` 包中运行命令。

## 🛠️ 工具管理

### 在 `uvx` 中指定版本

```shell
uvx ruff@0.6.0 --version
```

### 请求最新版本

```shell
uvx ruff@latest --version
```

一旦使用 `uv tool install` 安装了工具，`uvx` 将默认使用已安装的版本。

### 显示工具安装目录的路径

```shell
uv tool dir
```

### 升级工具

```shell
uv tool upgrade black
```

### 包含其他依赖

```shell
uv tool install --with <extra-package> <tool-package>
```

可以提供多个 `--with` 选项来包含其他包。

可以使用 `-w` 简写代替 `--with` 选项

### 从其他包安装可执行文件

```shell
uv tool install --with-executables-from ansible-core,ansible-lint ansible
```

请注意，`--with-executables-from` 与 `--with` 不同：

- `--with` 包含其他包作为依赖，但不安装它们的可执行文件
- `--with-executables-from` 既包含包作为依赖，又安装它们的可执行文件

## 🐍 Python 版本

### Python 版本文件

`.python-version` 文件可用于创建默认的 Python 版本请求。uv 在工作目录及其每个父目录中搜索 `.python-version` 文件。如果未找到，uv 将检查用户级配置目录。可以使用上述任何请求格式，但建议使用版本号以与其他工具互操作。

可以使用 [`uv python pin`](https://docs.astral.sh/uv/reference/cli/#uv-python-pin) 命令在当前目录中创建 `.python-version` 文件。

可以使用 [`uv python pin --global`](https://docs.astral.sh/uv/reference/cli/#uv-python-pin) 命令在用户配置目录中创建全局 `.python-version` 文件。

### Python 版本的发现

搜索 Python 版本时，会检查以下位置：

- `UV_PYTHON_INSTALL_DIR` 中管理的 Python 安装
- `PATH` 上作为 `python`、`python3` 或 `python3.x`（在 macOS 和 Linux 上）或 `python.exe`（在 Windows 上）的 Python 解释器

### 禁用自动 Python 下载

默认情况下，uv 会在需要时自动下载 Python 版本。

可以使用 [`python-downloads`](https://docs.astral.sh/uv/reference/settings/#python-downloads) 选项禁用此行为。默认情况下，它设置为 `automatic`；设置为 `manual` 以仅在 `uv python install` 期间允许 Python 下载。

`uv.toml`:

```toml
python-downloads = "manual"
```

## 🔧 环境变量

### `.env` 文件

`uv run` 可以从 dotenv 文件（例如 `.env`、`.env.local`、`.env.development`）加载环境变量，由 [`dotenvy`](https://github.com/allan2/dotenvy) crate 提供支持。

要从专用位置加载 `.env` 文件，请设置 `UV_ENV_FILE` 环境变量，或向 `uv run` 传递 `--env-file` 标志。

例如，要从当前工作目录中的 `.env` 文件加载环境变量：

```shell
echo "MY_VAR='Hello, world!'" > .env
uv run --env-file .env -- python -c 'import os; print(os.getenv("MY_VAR"))'
```

可以提供多个 `--env-file` 标志，后续文件会覆盖先前文件中定义的值。要通过 `UV_ENV_FILE` 环境变量提供多个文件，请用空格分隔路径（例如，`UV_ENV_FILE="/path/to/file1 /path/to/file2"`）。

要禁用 dotenv 加载（例如，覆盖 `UV_ENV_FILE` 或 `--env-file` 命令行参数），请将 `UV_NO_ENV_FILE` 环境变量设置为 `1`，或向 `uv run` 传递 `--no-env-file` 标志。

==如果同一变量在环境和 `.env` 文件中定义，环境中的值将优先。==
