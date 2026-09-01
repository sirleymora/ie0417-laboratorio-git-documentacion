# Selección del proyecto Python para Sphinx

## 1. Nombre y descripción breve

**Nombre del proyecto:** Django

Django es un framework web de alto nivel desarrollado en Python que permite construir aplicaciones web de manera estructurada. Facilita el desarrollo de aplicaciones mediante componentes para el manejo de modelos, vistas, formularios, autenticación, administración, persistencia de datos y otras funcionalidades comunes de aplicaciones web.

El proyecto seleccionado corresponde al repositorio oficial de Django y será utilizado para evaluar la generación de documentación automática mediante Sphinx.

---

## 2. URL del repositorio original

El repositorio original del proyecto se encuentra disponible en:

https://github.com/django/django

---

## 3. Licencia

El proyecto Django se distribuye bajo la **BSD 3-Clause License**, una licencia permisiva que permite utilizar, modificar y redistribuir el software bajo las condiciones establecidas por la licencia.

---

## 4. Lenguaje principal

El lenguaje principal del proyecto es **Python**.

Django es un proyecto de gran tamaño compuesto principalmente por código Python, organizado mediante paquetes y módulos que implementan las diferentes funcionalidades del framework.

---

## 5. Commit exacto analizado

El commit utilizado para realizar el análisis corresponde al estado exacto del repositorio utilizado durante el laboratorio.

El identificador del commit se obtuvo mediante el siguiente comando:

```bash
git rev-parse HEAD
```

Resultado:

```text
[5babd2e21ac0877d66c5452da17b97608b337fba]
```

Este commit identifica de manera inequívoca la versión del código fuente que será utilizada para generar y analizar la documentación mediante Sphinx.

---

## 6. Cantidad de archivos y líneas de código relevantes

Para determinar el tamaño del código Python del proyecto se utilizó la herramienta `cloc`.

El comando ejecutado fue:

```bash
cloc django --include-lang=Python
```

El resultado obtenido fue:

```text
2308 text files.
2245 unique files.
2779 files ignored.

github.com/AlDanial/cloc v 1.98  T=5.13 s (147.5 files/s, 32213.7 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Python                         756          22443          27613         115043
-------------------------------------------------------------------------------
SUM:                           756          22443          27613         115043
```

Las métricas relevantes del código Python son:

| Métrica               | Cantidad |
| --------------------- | -------: |
| Archivos Python       |      756 |
| Líneas en blanco      |   22,443 |
| Líneas de comentarios |   27,613 |
| Líneas de código      |  115,043 |

El proyecto contiene **756 archivos Python** y **115,043 líneas de código Python**. Estas dimensiones hacen que Django sea un proyecto suficientemente grande para evaluar de forma representativa las capacidades de generación de documentación de Sphinx.

La cantidad de comentarios también resulta relevante para el análisis, ya que permite estudiar la relación entre el código fuente, los comentarios y los *docstrings* utilizados por las herramientas de documentación.

---

## 7. Comando utilizado para obtener las métricas

Las métricas del código fuente se obtuvieron mediante el siguiente comando:

```bash
cloc django --include-lang=Python
```

El parámetro `--include-lang=Python` permite limitar el análisis de `cloc` al lenguaje Python, evitando que otros lenguajes o archivos presentes en el repositorio afecten las métricas utilizadas para evaluar el proyecto.

La ejecución produjo las siguientes métricas principales:

* **756 archivos Python.**
* **22,443 líneas en blanco.**
* **27,613 líneas de comentarios.**
* **115,043 líneas de código.**

---

## 8. Razón por la cual el proyecto es apropiado para Sphinx

Django es apropiado para la generación de documentación mediante Sphinx debido a que es un proyecto desarrollado principalmente en Python y presenta una estructura extensa basada en paquetes y módulos.

La organización del código permite utilizar mecanismos como:

* `sphinx.ext.autodoc` para obtener información directamente de los módulos Python.
* `sphinx-apidoc` para generar automáticamente archivos de documentación correspondientes a paquetes y módulos.
* `autosummary` para presentar de forma organizada elementos de la API.
* `napoleon` cuando sea necesario interpretar estilos de *docstrings*.
* `viewcode` para proporcionar enlaces hacia el código fuente.

Además, el tamaño del proyecto permite evaluar Sphinx sobre una aplicación real y no solamente sobre un proyecto pequeño creado específicamente para el laboratorio.

La presencia de cientos de módulos y archivos Python permite analizar cómo Sphinx organiza la documentación de paquetes, módulos, clases, funciones y métodos.

Por estas razones, Django permite evaluar aspectos importantes de Sphinx como la generación automática de documentación, la navegación de una API extensa, el procesamiento de *docstrings*, la integración de documentación narrativa y las dificultades relacionadas con las dependencias necesarias para importar los módulos.

---

## 9. Presencia y calidad inicial de comentarios y docstrings

El proyecto Django contiene comentarios y documentación asociada a diferentes elementos del código fuente. En particular, existen *docstrings* en distintos módulos, clases y funciones que pueden ser utilizados por Sphinx mediante `autodoc`.

La presencia de documentación estructurada permite que parte de la información de la API pueda obtenerse directamente desde el código fuente, incluyendo descripciones de determinados elementos y, cuando están disponibles, parámetros, valores de retorno y otras características.

Sin embargo, la cobertura y el nivel de detalle de la documentación no son necesariamente uniformes en todos los componentes del proyecto. Por ello, durante la generación se deberá determinar qué información puede ser extraída automáticamente mediante `autodoc` y qué información debe complementarse con documentación narrativa escrita manualmente.

Este aspecto resulta importante para el laboratorio porque permite comparar la información que Sphinx obtiene directamente de los *docstrings* con la información que debe proporcionar la persona encargada de documentar el proyecto.

La calidad final de esta documentación será evaluada posteriormente mediante la documentación HTML generada y las advertencias producidas durante el proceso de construcción.

---

## 10. Dependencias o dificultades previstas para generar la documentación

Debido al tamaño y complejidad de Django, se prevén varias dificultades durante la generación de la documentación con Sphinx.

### 10.1 Tamaño del proyecto

El proyecto contiene **756 archivos Python** y más de **115 mil líneas de código**, por lo que la generación de documentación puede requerir más tiempo y recursos que un proyecto pequeño.

### 10.2 Dependencias

`autodoc` necesita importar los módulos Python para poder inspeccionarlos. Algunos módulos pueden depender de otros paquetes o componentes del framework.

Por esta razón, será necesario utilizar un entorno virtual exclusivo para el laboratorio y registrar las dependencias necesarias para generar la documentación.

### 10.3 Importación de módulos

La utilización de `sphinx.ext.autodoc` puede producir advertencias o errores si algún módulo no puede ser importado correctamente.

En caso de que existan dependencias que no sean necesarias para el objetivo del laboratorio, se podrá utilizar la configuración:

```python
autodoc_mock_imports
```

para simular dichas dependencias y permitir que Sphinx continúe con la generación.

Cualquier dependencia simulada deberá quedar documentada en la configuración final.

### 10.4 Configuración del acceso al paquete

Será necesario configurar correctamente `conf.py` para que Sphinx pueda encontrar e importar el código fuente de Django.

Para esto se deberá establecer correctamente la ruta correspondiente al proyecto mediante `sys.path` u otro mecanismo apropiado.

### 10.5 Cantidad de módulos

La gran cantidad de paquetes y módulos puede producir una documentación extensa. Por esta razón, será necesario definir cuidadosamente la estructura del `toctree` y determinar qué partes del proyecto serán incluidas en la documentación.

### 10.6 Advertencias de Sphinx

Durante la ejecución de `sphinx-build` pueden aparecer advertencias relacionadas con:

* Módulos que no pueden importarse.
* Referencias que no pueden resolverse.
* Elementos sin documentación.
* Documentación duplicada.
* Problemas con *docstrings*.
* Referencias cruzadas incompletas.
* Dependencias faltantes.

Estas advertencias no serán ocultadas. Se registrarán en:

```text
sphinx/build.log
```

y posteriormente serán clasificadas y analizadas en el informe.

### 10.7 Dependencias de documentación

Las dependencias utilizadas exclusivamente para construir la documentación deberán registrarse en un archivo reproducible:

```text
requirements-docs.txt
```

De esta manera, otra persona podrá recrear el entorno necesario para generar la documentación.

---

## 11. Justificación final de la selección

Django fue seleccionado como proyecto Python para este laboratorio debido a su tamaño, estructura y naturaleza.

El proyecto cuenta con **756 archivos Python** y **115,043 líneas de código**, lo que proporciona una base suficientemente amplia para evaluar la generación automática de documentación mediante Sphinx.

Su organización en paquetes y módulos permite analizar la documentación de diferentes tipos de elementos de Python, incluyendo módulos, clases, funciones y métodos. Además, la presencia de *docstrings* permite estudiar cómo Sphinx utiliza la documentación existente en el código fuente mediante `autodoc`.

El proyecto también representa un caso realista para analizar las dificultades de generación de documentación en proyectos de software de gran tamaño, especialmente en relación con dependencias, importación de módulos, advertencias, navegación y referencias cruzadas.

Por estas características, Django resulta adecuado para evaluar las capacidades de Sphinx y posteriormente compararlas con la documentación generada mediante Doxygen para el proyecto C++ seleccionado.

---

## 12. Resumen de la selección

| Característica                         | Información                                                                        |
| -------------------------------------- | ---------------------------------------------------------------------------------- |
| **Proyecto**                           | Django                                                                             |
| **Descripción**                        | Framework web de alto nivel desarrollado en Python                                 |
| **Repositorio**                        | https://github.com/django/django                                                   |
| **Licencia**                           | BSD 3-Clause License                                                               |
| **Lenguaje principal**                 | Python                                                                             |
| **Archivos Python**                    | 756                                                                                |
| **Líneas de código**                   | 115,043                                                                            |
| **Líneas en blanco**                   | 22,443                                                                             |
| **Líneas de comentarios**              | 27,613                                                                             |
| **Herramienta asignada**               | Sphinx                                                                             |
| **Comando de métricas**                | `cloc django --include-lang=Python`                                                |
| **Commit analizado**                   | `[PEGAR HASH DE git rev-parse HEAD]`                                               |
| **Documentación existente**            | Comentarios y *docstrings* en diferentes componentes                               |
| **Principales dificultades previstas** | Dependencias, importación de módulos, tamaño del proyecto y advertencias de Sphinx |

---

## 13. Comandos utilizados durante la selección

### Verificación del commit

```bash
git rev-parse HEAD
```

### Conteo de archivos Python y líneas de código

```bash
cloc django --include-lang=Python
```

### Resultado principal de las métricas

```text
Python                         756          22443          27613         115043
```

Estas métricas corresponden al estado del proyecto identificado por el commit registrado anteriormente y serán utilizadas como referencia para el resto del laboratorio.
