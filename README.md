¡Atención! Esta wiki está en proceso de traducción. Algunos documentos podrían verse en otro idioma, o mostrar sólo una traducción parcial. También es posible que se encuentren algunas lagunas de contenido.

# ¡Bienvenido a la wiki de código abierto de Nebulas!

La comunidad de Nebulas es abierta; cualquier persona puede realizar contribuciones y crear un mundo descentralizado junto a nosotros.

La wiki de Nebulas es una herramienta colaborativa para la comunidad, cuyo objetivo primordial es el de ofrecer documentos tales como guías de uso, guías para desarrolladores y recursos de aprendizaje, entre otros.

La sección de desarrollo en _mainnet_ que citamos aquí está basada en [esta wiki](https://github.com/nebulasio/wiki).

## Otras versiones de este documento

* [en-US](https://github.com/nebulasio/nebdocs)
* [pt-PT](https://github.com/nebulasio/nebdocs/tree/pt-PT)
* [zh-CN](https://github.com/nebulasio/nebdocs/tree/zh-CN)
* [ko-KR](https://github.com/nebulasio/nebdocs/tree/ko-KR)

## Licencia

Esta wiki se publicó bajo los términos y condiciones de la **Licencia Pública General Reducida de GNU**, cuyo contenido original puede leerse [aquí](https://github.com/nebulasio/nebdocs/tree/es/LICENSE).

[Debido a los propios términos y condiciones](https://www.gnu.org/licenses/translations.es.html) de la Fundación GNU, nos vemos obligados a ofrecer la misma en el idioma original (inglés).

## Cómo colaborar

En Nebulas apuntamos a ofrecer un ecosistema en permanente mejora, lo cual significa que necesitamos la colaboración de nuestra comunidad. ¡Necesitamos tus contribuciones!

Estas colaboraciones no se limitan sólo al desarrollo del código, sino también a reportar errores, traducir documentos, difundir los principios de Nebulas y responder preguntas de usuarios novatos.

[Sepa más acerca de cómo colaborar](https://wiki.nebulas.io/es/latest/how-to-contribute.html).

## Introducción

Este proyecto fue creado mediante [sphinx](http://www.sphinx-doc.org/en/master/) y subido a [readthedocs](https://readthedocs.org/) para su alojamiento. La URL de la documentación, luego de subida al hosting, es: [https://nebdocs.readthedocs.io/es/latest/](https://nebdocs.readthedocs.io/es/latest/).

Esta wiki soporta dos formatos distintos para sus documentos:

* Markdown (.md)
* reStructuredText (.rst)

La estructura de carpetas de cada documento se define en el archivo README.rst del directorio relacionado.

### Reglas para la traducción y el _branching_:
1) Cada versión traducida se manejará en forma independiente a través de _branches_. Actualmente, los lenguajes existentes son los siguientes:

* master: versión en inglés;
* zh-CN: versión en chino simplificado;
* pt-PT: versión en portugués;
* es: versión en español;
* ko-KR: versión en coreano.

2) Para facilitar la administración de los documentos, la estructura en cada _branch_ deberá ser similar en todo lo posible a la versión master;

3) Cada traducción puede tener su propia versión temporal. Se recomienda añadir un sufijo que indique el número de versión; por ejemplo en1.0 o bien zh-CN1.1;

### Cómo compilar la documentación

1) Descarga una copia de la wiki desde github mediante el siguiente comando:

```bash
git clone https://github.com/nebulasio/nebdocs.git
```

2) Instala a continuación los componentes Python necesarios para la tarea:

```bash
pip install sphinx==1.5.6 sphinx-autobuild sphinx_rtd_theme recommonmark sphinx_markdown_tables
```
**Observación:** pip no es muy fehaciente. Use el gestor de paquetes de su sistema operativo si possible.

3) Compila el proyecto mediante este comando:

```bash
cd nebdocs
make html
```

### Cómo añadir nuevos documentos y subdirectorios

#### Si necesitas añadir un archivo:

1) Añade ese archivo al directorio correspondiente;

2) Ubica el archivo README.rst en el directorio en donde está ubicado tu archivo (si estás en el directorio raíz, el archivo se llama **index.rst**).

3) Ábrelo para edición y añade el nombre del nuevo archivo a la lista de archivos por debajo de la palabra clave 'toctree'. Por ejemplo:

Si tienes esta estructura de archivos:

    +--folder
    |
    +--README.rst
    +--config.md
    +--contributors.md
    +--newFile.md

El contenido del archivo README.rst de ese directorio debería verse así:

    .. toctree::
    :titlesonly:
    Config.md
    Contributors.md
    newFile.md

#### Si necesitas añadir un subdirectorio:

En este caso necesitas añadir un nuevo archivo README.rst para definir la estructura del directorio.

Añade el nombre de todos los archivos contenidos en ese directorio en el README.rst luego de la palabra clave 'toctree'. Si lo necesitas, puedes consultar los otros archivos README.rst existentes en otros directorios a modo de guía.

La lista de archivos en el README.rst del directorio inmediatamente superior debe contener la ruta relativa al README.rst del nuevo directorio. Por ejemplo:

Para una estructura de archivos tal como la siguiente:


    +--folder
       |
       +--README.rst
       +--config.md
       +--contributors.md
       +--newDirectory
          |
          +--README.rst
          +--newFile.md

Los contenidos del nuevo archivo README.rst deberán ser:

    .. toctree::
        :titlesonly:
        Config.md
        Contributors.md
        newDirectory/README.rst

### Cómo añadir un _branch_ para albergar una nueva traducción

1) Crea un nuevo _branch_; para la versión en español **si todavía no existe**. Por ejemplo:

```bash
git checkout -b es
```

2) Modifica la configuración de git en ```./docs/conf.py```, busca la definición de ```'html_context'```  y cambia el valor del campo ```'github_version'``` al nuevo nombre del _branch_; por ejemplo: ```'es'```, de esta manera:

```python
# VCS options:
Html_context = {
     "display_github": True, # Integrate GitHub
     "github_user": "nebulasio", # Username
     "github_repo": "nebdocs", # Repo name
     "github_version": "es", # Version
     "conf_py_path": "/", # Path in the checkout to the docs root
}
```

3) Reemplaza aquellos documentos que necesitas traducir con la nueva versión en tu idioma.

4) Envía los archivos a github:

```bash
git push --set-upstream origin es
```

5) Notifica al administrador del repositorio git para que añada la nueva versión traducida al índice de readthedocs'.

#### ATENCIÓN

Es muy probable que necesites hacer un _rebase_ del _master_ en el _branch_ para que el equipo de Nebulas pueda unificar el _pull request_ de una forma limpia y ordenada. Para ello, y para evitar los conflictos que puedan aparecer, haz lo siguiente: desde tu _branch_ de trabajo local, y luego de haber actualizado el master con el comando ```git pull origin master```:

```bash
git rebase -Xtheirs master
git push -f
```
