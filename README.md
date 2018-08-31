# nebdocs

This project was created by [`sphinx`] (http://www.sphinx-doc.org/en/master/) and uploaded to [`readthedocs`] (https://readthedocs.org/) for hosting. Url of online documentation after hosting is: https://nebdocs.readthedocs.io/en_US/latest/index.html.

## Language version and branch rules:
1. The multi-language version is managed separately by different branches. The currently supported languages are as follows:
- master: English version;
- zh-CN: Simplified Chinese version;
2. To facilitate document management, the document structure of different branches is as consistent as possible with the main branch;
3. Each language is allowed to have its own temporary version. It is recommended to add the suffix version number to the version name, for example, en1.0, zh-CN1.1;

## How to build this project?
1. clone the project from github, here is for master bratch:

```bash
git clone https://github.com/nebulasio/nebdocs.git
```

2. install the necessary python components:

```bash
pip install sphinx==1.5.6 sphinx-autobuild sphinx_rtd_theme recommonmark
```
3. build the project:

```bash
cd nebdocs
make html
```