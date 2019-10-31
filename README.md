# 学习笔记

- [学习笔记](#%e5%ad%a6%e4%b9%a0%e7%ac%94%e8%ae%b0)
  - [1 安装 Sphinx](#1-%e5%ae%89%e8%a3%85-sphinx)
    - [1.1 配置 Python](#11-%e9%85%8d%e7%bd%ae-python)
    - [1.2 配置 pip](#12-%e9%85%8d%e7%bd%ae-pip)
    - [1.3 配置 Sphinx](#13-%e9%85%8d%e7%bd%ae-sphinx)
      - [1.3.1 Sphinx 版本问题](#131-sphinx-%e7%89%88%e6%9c%ac%e9%97%ae%e9%a2%98)
  - [2 设置 Sphinx 项目源目录](#2-%e8%ae%be%e7%bd%ae-sphinx-%e9%a1%b9%e7%9b%ae%e6%ba%90%e7%9b%ae%e5%bd%95)
    - [2.1 创建 Sphinx 项目](#21-%e5%88%9b%e5%bb%ba-sphinx-%e9%a1%b9%e7%9b%ae)
    - [2.2 定义文档结构](#22-%e5%ae%9a%e4%b9%89%e6%96%87%e6%a1%a3%e7%bb%93%e6%9e%84)
    - [2.3 增加内容](#23-%e5%a2%9e%e5%8a%a0%e5%86%85%e5%ae%b9)
    - [2.4 运行编译](#24-%e8%bf%90%e8%a1%8c%e7%bc%96%e8%af%91)
  - [3 配置 Sphinx](#3-%e9%85%8d%e7%bd%ae-sphinx)
    - [3.1 配置主题为 sphinx_rtd_theme](#31-%e9%85%8d%e7%bd%ae%e4%b8%bb%e9%a2%98%e4%b8%ba-sphinxrtdtheme)
    - [3.2 支持 Markdown 文件](#32-%e6%94%af%e6%8c%81-markdown-%e6%96%87%e4%bb%b6)
  - [4 推送文件](#4-%e6%8e%a8%e9%80%81%e6%96%87%e4%bb%b6)
  - [5 导入到 ReadtheDocs](#5-%e5%af%bc%e5%85%a5%e5%88%b0-readthedocs)
    - [5.1 在 ReadtheDocs 网站手动导入](#51-%e5%9c%a8-readthedocs-%e7%bd%91%e7%ab%99%e6%89%8b%e5%8a%a8%e5%af%bc%e5%85%a5)
    - [5.2 ReadtheDocs 中文字符构建失败](#52-readthedocs-%e4%b8%ad%e6%96%87%e5%ad%97%e7%ac%a6%e6%9e%84%e5%bb%ba%e5%a4%b1%e8%b4%a5)

文档采用 [reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickstart.html) 语法。

使用 [Sphinx](http://www.sphinx-doc.org/en/master/) + [Github](https://github.com/) + [ReadtheDocs](https://readthedocs.org/) 自动生成文档。

参考 [Sphinx 安装教程](http://www.sphinx-doc.org/en/master/usage/installation.html)。[中文教程](http://www.pythondoc.com/sphinx/tutorial.html)。

## 1 安装 Sphinx

### 1.1 配置 Python

本地环境是 ubuntu 16.04。

```sh
# 查看本地的 Python 版本
python
# python      python2     python2.7   python3     python3.5   python3.5m  python3m
# 配置 python2.7 和 python3
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 100
# update-alternatives: using /usr/bin/python2.7 to provide /usr/bin/python (python) in auto mode
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
# 配置使用 python3，选择 2，即 /usr/bin/python3
sudo update-alternatives --config python
```

### 1.2 配置 pip

pip 是 Python 包管理工具。

```sh
sudo apt-get install python-pip python3-pip
# 可能会有 warning: not replacing /usr/bin/pip with a link 错误， ll /usr/bin/pip* 查看可知道 /usr/bin/pip 不是一个软链接，可以删除 /usr/bin/pip，再重新执行 --install 命令
sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip2 100
sudo update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 2
# 配置使用 pip3，选择 2，即 /usr/bin/pip3
sudo update-alternatives --config pip
```

### 1.3 配置 Sphinx

```sh
# 使用系统命令安装
sudo apt-get install python3-sphinx
# 使用 PyPI 安装
pip install -U sphinx
```

版本：python-3.8 pip-19.2.3 sphinx-2.2.1

#### 1.3.1 Sphinx 版本问题

当前 ReadtheDocs 网站使用的版本是：python-3.7 sphinx-1.8.5。使用最新版本的 `sphinx-quickstart` 脚本生成的配置文件在 ReadtheDocs 网站编译失败，所以需要安装和 ReadtheDocs 网站相同的版本。

```sh
# 使用 PyPI 安装
pip install sphinx==1.8.5
```

## 2 设置 Sphinx 项目源目录

### 2.1 创建 Sphinx 项目

一个 Sphinx 的 reStructuredText 文档集合根目录被称为`源目录`。

其中，`conf.py` 是 Sphinx 配置文件，可以配置 Sphinx 如何阅读源文件并编译文档。

`sphinx-quickstart` 是 Sphinx 的一个脚本，可以设置源目录并创建默认的 `conf.py`。

直接运行 `sphinx-quickstart`，回答问题，且同意使用 autodoc 插件。

```sh
# 运行 `sphinx-quickstart`
sphinx-quickstart
```

### 2.2 定义文档结构

运行 `sphinx-quickstart` 之后，会创建一个源目录，包含 `conf.py` 和一个[主文档](http://www.sphinx-doc.org/en/master/glossary.html#term-master-document) `index.rst`(同意默认选项)。主文档的主要功能是作为欢迎页面，并且包含“内容树目录”的根(toctree)。

当前目录有：

- build 目录：运行 `make` 命令后，保存生成的文件
- source 目录：保存文档的源文件
- make.bat：Windows 下支持`make` 编译，批处理命令
- Makefile：Linux 下支持 `make` 编译

学习 [toctree 指令](http://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#toctree-directive)。

### 2.3 增加内容

Sphinx 源文件可以使用大多标准 reStructuredText 特性，并且增加了一些特性。

浏览 [reStructuredText 文档](http://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html)学习 reStructuredText 指令。

浏览 [reStructuredText 入门](http://www.pythondoc.com/sphinx/rest.html#rst-primer)学习 reStructuredText 指令。

浏览 [Sphinx 标记结构](http://www.pythondoc.com/sphinx/markup/index.html#sphinxmarkup)学习 Sphinx 增加的标记（标识符）列表。

### 2.4 运行编译

```sh
sphinx-build -b html sourcedir builddir
```

`sourcedir` 是源目录，`builddir` 是放置编译文件的位置。`-b` 选项指定编译器，这里 Sphinx 会编译 HTML 文件。

浏览 [Sphinx-build man 页](http://www.sphinx-doc.org/en/master/man/sphinx-build.html)学习 sphinx-build 支持的选项。

但是，`sphinx-quickstart` 脚本创建的 `Makefile` 和 `make.bat` 使得编译更加简单。查看文件可知，内部调用的仍然是 `sphinx-build` 命令。

可以执行下面命令编译 HTML 文件到选择的编译目录。

```sh
# Linux 下使用 Makefile 编译
make html
# Windows 下使用 make.bat 编译
make.bat html
```

**生成 PDF 文件**：执行命令 `make latexpdf` 运行 `Latex builder`，调用 pdfTeX 插件。

## 3 配置 Sphinx

### 3.1 配置主题为 sphinx_rtd_theme

本地安装可使用：`pip install sphinx-rtd-theme`。

在 Sphinx 工程中使用 sphinx_rtd_theme 主题需要修改 `conf.py`：

```py
# 导入 sphinx_rtd_theme 包
import sphinx_rtd_theme

# 增加插件
extensions = [
    ...
    "sphinx_rtd_theme",
]

# 修改主题
html_theme = "sphinx_rtd_theme"
```

### 3.2 支持 Markdown 文件

本地安装可使用：`pip install recommonmark`。

在 Sphinx 工程中使用 Markdown 需要修改 `conf.py`：

```py
# 增加插件
extensions = [
    ...
    "recommonmark",
]
```

## 4 推送文件

```sh
# 初始化版本控制
git init
# 添加所有文件
git add .
# 添加提交信息
git commit -m "sphinx start"
# 添加远程仓库路径
git remote add origin https://github.com/xueqing/study-notes.git
# 推送到远程仓库
git push origin master
```

## 5 导入到 ReadtheDocs

在 ReadtheDocs 网站[注册账号](https://readthedocs.org/accounts/signup/)，绑定 github 账号。也可以直接使用 github 账号登录。

参考 [Webhooks](https://docs.readthedocs.io/en/stable/webhooks.html) 导入 github 项目。

### 5.1 在 ReadtheDocs 网站手动导入

在 ReadtheDocs 网站，点击头像菜单栏，选择 `My Projects`

- `Import a Repository` >> `+` 自动补全 `Project Details` 页面相关信息
- `Import Manually` 手动填写源码目录地址

在 `Project Details` 页面填写相关信息：

- **name**：比如 `kiki-study-notes`，在 ReadtheDocs 网站预览时会拼接在 URL 里面 `https://kiki-study-notes.readthedocs.io/en/latest/`
- **Repository URL**：此处是 `https://github.com/xueqing/study-notes.git`。表示源码目录
- **Repository type**：此处是 Git。可根据源码托管服务器选择对应类型

点击 `Next`，之后会自动拉取源码进行编译。

之后可以在头像菜单栏选择 `My Projects` 查看已经导入的项目，并预览每个项目。

### 5.2 ReadtheDocs 中文字符构建失败

ReadtheDocs 构建报错 `! Package inputenc Error: Unicode char 整 (U+6574) (inputenc) not set up for use with LaTeX.`。
解决方法参考 [Sphinx PDFs with Unicode](https://docs.readthedocs.io/en/stable/guides/pdf-non-ascii-languages.html#sphinx-pdfs-with-unicode)。

如果不是用中文或者日文编写的文档，包构建报 Unicode 错误时，尝试修改 `conf.py` 中的 `latex_engine` 默认配置(`pdflatex`)：

```py
latex_engine = 'xelatex'
```

如果中文编写，在 `conf.py` 中添加下面的设置：

```py
latex_engine = 'xelatex'
latex_use_xindy = False
latex_elements = {
    'preamble': '\\usepackage[UTF8]{ctex}\n',
}
```

用日文编写的文档，修改 `conf.py`：

```py
latex_engine = 'platex'
latex_use_xindy = False
```
