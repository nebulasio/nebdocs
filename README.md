# nebdocs

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