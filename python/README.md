<div align="center">
    <h2><img src="https://readme-typing-svg.herokuapp.com?font=Optima&size=35&duration=6000&color=FF5733&center=true&vCenter=true&width=800&lines=代码是写给人看的,+顺便能在机器上运行" alt="Typing SVG"/></h2>
</div>


###  [Google开源项目风格指南](https://google.github.io/styleguide/pyguide.html), [国内中文版](https://zh-google-styleguide.readthedocs.io/en/latest/)

#### <mark>项目结构建议</mark>
```plaintext
my_project/
├── .github/                 # GitHub Actions workflow 配置
├── docs/                    # 项目文档
├── deploy/                  # 部署相关文件，如 Dockerfile、K8s 配置、CI/CD 脚本
├── logs/                    # 日志文件目录
│   └── .gitkeep             # 占位文件，使 Git 保留空目录
├── src/                     # 源代码目录
│   └── my_project/          # Python 包根目录
│       ├── __init__.py      # 包初始化文件
│       ├── __main__.py      # 入口文件，可用 `python -m my_project` 运行
│       ├── py.typed         # 空文件, 告诉类型检查工具（mypy、Pyright 等）——这个库自带类型提示
│       ├── config/          # 配置相关模块
│       │   └── settings.py  # 配置文件，如读取 .env 或 YAML 配置
│       ├── constants/       # 常量文件模块
│       ├── core/            # 核心业务逻辑模块
│       ├── services/        # 服务层，处理业务逻辑、外部接口调用
│       ├── repositoies/     # 数据访问层，处理数据库或外部存储的操作
│       ├── schemas/         # 数据结构定义, 如请求/响应的 Pydantic Schema
│       ├── models/          # 数据模型，例如 ORM 或 Pydantic 模型
│       ├── api/             # API 层（如 FastAPI 路由）
│       └── utils/           # 工具函数/模块
├── tests/                   # 单元测试、集成测试
├── .env.example             # 配置模板：只放 key，不放 value
├── .gitignore               # 忽略真实配置、缓存、虚拟环境
├── pyproject.toml           # Python 项目配置（依赖、打包、工具链）
├── Makefile                 # 工程快捷键：run / test / lint / ci
├── README.md                # 项目说明文档
└── uv.lock                  # 使用uv安装同步依赖
```

### <mark>uv</mark><sub>用 Rust 开发的超快 Python 包与项目管理工具</sub>
```bash
# 初始化项目
uv init myproject --lib  # 创建库项目（带 --lib 选项, 生成 pyproject.toml、src 目录等）
uv init myproject --python 3.12  # 指定 Python 版本初始化（.venv 在首次 uv run/sync 时创建）
cd myproject

# 手动创建虚拟环境
uv venv -p 3.12

# ---------- 在已有项目中创建指定版本的虚拟环境并激活 ----------
uv venv --python 3.10          # 创建 .venv（使用 Python 3.10，没有会自动下载）
source .venv/bin/activate      # 激活环境（Windows: .venv\\Scripts\\activate）
uv python pin 3.10             # 固定项目 Python 版本（写入 .python-version）
uv sync                        # 根据 pyproject.toml / uv.lock 同步依赖

# 添加依赖（自动创建虚拟环境、更新 pyproject.toml 和 uv.lock）
uv add requests
uv add "flask>=3.0"
uv add pandas numpy scipy           # 同时添加多个

# 添加开发依赖
uv add --dev pytest ruff mypy
uv add --group lint ruff             # 添加到自定义依赖组

# 移除依赖
uv remove requests

# 同步环境（根据 uv.lock 安装所有依赖）
uv sync

# 仅同步生产依赖（排除 dev）
uv sync --no-dev

# 更新锁文件
uv lock

# 更新特定包到最新版本
uv lock --upgrade-package requests

# 查看依赖树
uv tree
```


#### 1.<mark>模块导入规范</mark><sub> pip install isort</sub>

>标准库导入
>
>第三方库导入
>
>本地模块导入

```bash
# `touch .isort.cfg, 一般位于项目根目录, 推荐内容如下:`
[tool.isort]
line_length = 100    # 指定每行代码的最大字符数
multi_line_output = 3   # 定义多行导入的格式
include_trailing_comma = True   # 在多行导入的最后一个模块后添加尾随逗号
force_grid_wrap = 0    # 定义何时使用网格格式
use_parentheses = True   # 使用圆括号而不是反斜杠
ensure_newline_before_comments = True    # 在导入语句上方的注释之前确保有一个新行
known_first_party = ['my_module', 'my_other_module']   # import my_module, from my_other_module import xxx
default_section = 'THIRDPARTY'     # 为未分类模块设置默认第三方库
sections = ['FUTURE', 'STDLIB', 'THIRDPARTY', 'FIRSTPARTY', 'LOCALFOLDER']     # 定义导入语句的排序顺序

```

使用:

```bash
isort your_script.py    # 格式化单个文件
isort .    # 格式化整个项目目录
```
