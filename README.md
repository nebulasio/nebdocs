# 星云文档（nebdocs）

本项目是通过[`sphinx`](http://www.sphinx-doc.org/en/master/)创建，并上传到[`readthedocs`](https://readthedocs.org/)上托管, 托管后的在线文档地址：https://nebdocs.readthedocs.io/zh_CN/latest/index.html。

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
pip install sphinx==1.5.6 sphinx-autobuild sphinx_rtd_theme recommonmark
```
3. 开始编译项目:

```bash
cd nebdocs
make html
```

## 如何添加新的语言版本？
1. 创建新的分支，例如中文版本：
```bash
git checkout -b zh-CN
```
2. 修改./docs/conf.py里面的github配置, 找到html_context定义，将其github_version字段的值修改为新的分支名zh-CN，如下：

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
