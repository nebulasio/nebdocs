# nebdocs

Este projecto foi criado usando [sphinx](http://www.sphinx-doc.org/en/master/) e carregado para [readthedocs](https://readthedocs.org/) para o alojar. O Url da documentação online depois de ser alojada é: [https://nebdocs.readthedocs.io/en/latest/](https://nebdocs.readthedocs.io/en/latest/).

Este projecto suporta documentos nos seguintes formatos:

- Markdown(.md)
- reStructuredText(.rst)

A estructura do directório do documento está definida no ficheiro README.rst, no mesmo directório.

	## Regras para versões noutras línguas e branches:
1. A versão de múltiplas línguas é gerida separadamente por branches diferentes. As línguas presentemente suportadas são:
- master: versão Inglesa;
- zh-CN: versão de Chinês Simplificado;
- pt-PT: versão Portuguea;
2. De modo a facilitar a gestão de documentos, a estructura das várias branches deve ser o mais consistente possível com a branch principal;
3. Cada língua pode ter a sua versão temporária. É recomendado adicionar um número sufixo a cada nome, por exemplo, en1.0, zh-CN1.1;

## Como compilar este projecto?
1. clone o projecto do github:

```bash
git clone https://github.com/nebulasio/nebdocs.git
```

2. instale os componentes do python necessários:

```bash
pip install sphinx==1.5.6 sphinx-autobuild sphinx_rtd_theme recommonmark sphinx_markdown_tables
```
**Observação:** a instalação do sphinx através do pip não é fiável. Use o gestor de pacotes do seu sistema operativo se possível.

3. compile o projecto:

```bash
cd nebdocs
make html
```

## Como adicionar um documento?
### Se for um único ficheiro
1. Adicione o ficheiro ao directório apropriado;
2. Procure o ficheiro README.rst no directório onde o ficheiro se encontra (para o directório raíz do projecto é o ficheiro index.rst), abra-o, e adicione o novo ficheiro à lista depois da palavra-chave 'toctree'. Por exemplo:

Para esta estructura de ficheiros:
```
+--folder
   |
   +--README.rst
   +--config.md
   +--contributors.md
   +--newFile.md
```

Os conteúdos de README.rst devem-se parecer com o seguinte:
```
.. toctree::
    :titlesonly:

    Config.md
    Contributors.md
    newFile.md
```

### Se precisar de adicionar um directório
Neste caso, terá de criar um novo ficheiro README.rst para definir a estructura de documentos do novo directório. Adicione o nome de outros ficheiro à lista depois da palavra-chave 'toctree'. Para mais detalhes, refira-se a outros ficheirosREADME.rst. A lista de ficheiros num ficheiro README.rst de um directório prévio deverá conter o caminho relativo da ficheiro README.rst do directório em uso, por exemplo:

Para esta estructura de ficheiros:
```
+--folder
   |
   +--README.rst
   +--config.md
   +--contributors.md
   +--newDirectory
      |
      +--README.rst
      +--newFile.md
```
Os conteúdos do ficheiro directorio/README.rst deverão ser:
```
.. toctree::
    :titlesonly:

    Config.md
    Contributors.md
    newDirectory/README.rst
```

## Como adicionar uma nova língua?

1. Crie uma nova branch, por exemplo, para a versão Chinesa:
```bash
Git checkout -b zh-CN
```
2. Modifique a configuração do github em ./docs/conf.py, procure a definição 'html_context' e mude o valor do campo 'github_version' para o nome do novo branch 'zh-CN', como exemplificado:

```python
# VCS options:
Html_context = {
     "display_github": True, # Integrate GitHub
     "github_user": "nebulasio", # Username
     "github_repo": "nebdocs", # Repo name
     "github_version": "zh-CN", # Version
     "conf_py_path": "/", # Path in the checkout to the docs root
}
```

3. Altere os documentos que precisam de ser traduzidos.

4. Submeta os ficheiros para o github:

```bash
Git push --set-upstream zh-CN
```

**Note:** existe uma probabilidade alta de você ter que fazer um rebase do master para a branch em que está a trabalhar, de modo a facilitar a amalgamação da pull request de forma limpa. Para tal, e para evitar todos os conflictos que isto criará, faça o seguinte, da branch da língua, após ter actualizado o master com ```git pull origin master```:
```bash
git rebase -Xtheirs master
git push -f
```

5. Alerte o gestor do repositório para adicionar a nova versão à documentação online no readthedocs.
