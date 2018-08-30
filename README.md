# nebdocs

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