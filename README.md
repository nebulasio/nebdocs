# 星云文档（nebdocs）

本项目是通过[sphinx](http://www.sphinx-doc.org/en/master/)创建并上传到[readthedocs](https://readthedocs.org/)上托管, 托管后的在线文档地址：[https://nebdocs.readthedocs.io/zh_CN/latest/](https://nebdocs.readthedocs.io/zh_CN/latest/)。

本项目支持如下两种格式的文档：
- markdown（.md）
- reStructuredText（.rst）

文档的目录结构放在同目录下的README.rst文件中定义

## 语言版本和分支的规则：
1. 多语言版本通过不同分支来分别管理，当前支持的语言如下：
- master：英文版本；
- zh-CN：简体中文版本；
2. 为方便文档管理，不同分支的文档结构尽量与主分支保持一致；
3. 每种语言允许有自己的临时版本，建议在版本名末尾加上版本号，例如，en1.0, zh-CN1.1；

## 如何编译本项目?
1. 从github 克隆项目，这里演示的是中文版本:

```bash
git clone -b zh-CN https://github.com/nebulasio/nebdocs.git
```

2. 安装必须的python组件:

```bash
pip install sphinx==1.5.6 sphinx-autobuild sphinx_rtd_theme recommonmark sphinx-markdown-tables
```
3. 开始编译项目:

```bash
cd nebdocs
make html
```

## 如何添加新文档？
### 如果只是添加文件
1. 将文件添加到合适的位置；
2. 在文件所在的目录里找到README.rst文件(项目根目录为index.rst文件)，打开该文件，在'toctree'关键字后面的文件列表添加新加的文件名。例如：

对于文件结构：
```
+--folder
   |
   +--README.rst
   +--config.md
   +--contributors.md
   +--新文件.md
```

README.rst的内容应该是这样：
```
.. toctree::
    :titlesonly:

    config.md
    contributors.md
    新文件.md
```
### 如果需要添加新目录
这种情况需要添加一个README.rst文件用来定义本目录下的文档目录结构，将其他文件名放入'toctree'关键字后面的文件列表中，具体内容可以参照其他README.rst文件，并在上一层目录的README.rst文件里的文件列表加上本目README.rst文件的相对路径，例如：

对于文件结构：
```
+--folder
   |
   +--README.rst
   +--config.md
   +--contributors.md
   +--新目录
      |
      +--README.rst
      +--新文件.md
```
folder/README.rst文件内容应该为：
```
.. toctree::
    :titlesonly:

    config.md
    contributors.md
    新目录/README.rst
```

## 如何添加新的语言版本？
1. 创建新的分支，例如中文版本：
```bash
git checkout -b zh-CN
```
2. 修改./docs/conf.py里面的github配置, 找到html_context定义，将其github_version字段的值修改为新的分支名zh-CN，如下：

```python
# VCS options: 
html_context = {
    "display_github": True, # Integrate GitHub
    "github_user": "nebulasio", # Username
    "github_repo": "nebdocs", # Repo name
    "github_version": "zh-CN", # Version
    "conf_py_path": "/", # Path in the checkout to the docs root
}
```

3. 将需要翻译的文档替换成新语言版本。

4. 提交到github：

```bash
git push --set-upstream zh-CN
```
5. 通知管理员在readthedocs的在线文档上添加新的语言版本。
