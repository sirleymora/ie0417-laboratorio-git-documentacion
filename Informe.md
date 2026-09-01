# Parte III — Documentación del proyecto C++ con Doxygen

## 7.1 Configuración de Doxygen

Para documentar el proyecto C++ seleccionado, se utilizó el repositorio **Catch2**, ubicado localmente en:

```text
~/ie0417-laboratorio-git-documentacion/Proyectos/Catch2
```

El commit exacto analizado fue obtenido mediante:

```bash
git rev-parse HEAD
```

obteniendo:

```text
317ac1ed4c0bb6e6b91eafc817e05c488feffcb3
```

A partir de la configuración generada inicialmente mediante:

```bash
doxygen -g Doxyfile
```

se creó y modificó una configuración propia para el laboratorio. La documentación se configuró para analizar el directorio `src/` de manera recursiva, ya que este contiene la implementación principal de Catch2 y sus subdirectorios internos.

Entre las opciones configuradas se encuentran:

```text
PROJECT_NAME           = "Catch2"
PROJECT_NUMBER         = "Commit 317ac1ed4c0bb6e6b91eafc817e05c488feffcb3"
OUTPUT_DIRECTORY       = doxygen
INPUT                  = src
RECURSIVE              = YES
GENERATE_HTML          = YES
EXTRACT_ALL            = YES
SOURCE_BROWSER         = YES
REFERENCES_RELATION    = YES
REFERENCES_LINK_SOURCE = YES
HAVE_DOT               = YES
CALL_GRAPH             = YES
CALLER_GRAPH           = YES
CLASS_DIAGRAMS         = YES
```

La opción `INPUT = src` establece el directorio principal de entrada, mientras que `RECURSIVE = YES` permite que Doxygen explore también los subdirectorios. Esto es necesario debido a la organización del proyecto, que contiene, entre otros, el directorio:

```text
src/catch2/internal/
```

También se habilitó `EXTRACT_ALL = YES` para que Doxygen extraiga las entidades encontradas aunque no todas posean comentarios estructurados. Esto permite analizar posteriormente qué información puede obtenerse directamente de las declaraciones y definiciones del código fuente y qué información depende de los comentarios escritos por las personas desarrolladoras.

La generación HTML se habilitó mediante `GENERATE_HTML = YES` y se estableció `doxygen/` como directorio de salida. Como resultado, las páginas HTML se encuentran en:

```text
doxygen/html/
```

También se habilitó la exploración del código fuente mediante `SOURCE_BROWSER = YES`, así como las referencias entre entidades. Esto permite navegar desde elementos documentados hacia el código fuente y observar relaciones entre diferentes componentes.

Para generar diagramas se habilitó Graphviz mediante:

```text
HAVE_DOT = YES
```

y se activaron los diagramas de clases y los grafos de llamadas:

```text
CLASS_DIAGRAMS = YES
CALL_GRAPH     = YES
CALLER_GRAPH   = YES
```

Durante la generación se observaron archivos de diagramas como:

```text
structCatch_1_1TestRunStats__coll__graph.png
structCatch_1_1true__given__inherit__graph.png
structCatch_1_1Totals_a1a94a654f5f3786b75695e081fc9bca2_icgraph.png
```

Esto confirma que Graphviz fue utilizado correctamente para generar relaciones visuales entre entidades del proyecto.

### Página principal

Como parte de la configuración propia, se creó una página principal en:

```text
doxygen/mainpage.md
```

La página identifica el proyecto documentado, el commit analizado, el lenguaje utilizado, el directorio de código fuente y el repositorio original.

La configuración utiliza:

```text
USE_MDFILE_AS_MAINPAGE = doxygen/mainpage.md
```

De esta manera, la documentación generada presenta una portada identificable y específica para el laboratorio, en lugar de depender exclusivamente de una configuración existente del repositorio original.

---

## 7.2 Generación de la documentación

La documentación se generó utilizando el siguiente comando:

```bash
doxygen Doxyfile 2>&1 | tee doxygen/build.log
```

El comando permite ejecutar Doxygen y almacenar simultáneamente su salida en `doxygen/build.log`. Esto permite conservar evidencia del proceso de generación y revisar posteriormente errores y advertencias.

La generación finalizó correctamente y el registro terminó con:

```text
finished...
```

Además, se verificó la existencia de la página principal mediante:

```bash
ls -lh doxygen/html/index.html
```

obteniéndose:

```text
-rw-rw-r-- 1 vboxuser vboxuser 3.0K Aug 31 05:29 doxygen/html/index.html
```

Por lo tanto, se confirmó que la documentación HTML fue generada correctamente.

La carpeta `doxygen/html/` contiene numerosas páginas correspondientes a clases, estructuras, archivos y otras entidades del proyecto. También contiene recursos necesarios para la navegación y presentación de la documentación, como hojas de estilo, scripts, imágenes y archivos asociados a los diagramas.

### Advertencias durante la generación

La generación produjo **30 advertencias**, obtenidas mediante:

```bash
grep -ic "warning:" doxygen/build.log
```

El resultado fue:

```text
30
```

Las advertencias observadas corresponden principalmente a dos categorías.

La primera corresponde a mensajes del tipo:

```text
warning: no matching class member found for
```

Estos mensajes aparecen, entre otros archivos, en:

```text
src/catch2/internal/catch_jsonwriter.cpp
src/catch2/internal/catch_stringref.cpp
src/catch2/internal/catch_xmlwriter.cpp
```

Estas advertencias indican que Doxygen encontró elementos de documentación que no pudo asociar con un miembro de clase correspondiente según la información disponible durante el análisis. No implican necesariamente un error de compilación del proyecto, sino una dificultad de Doxygen para establecer la correspondencia entre determinados comentarios y entidades del código.

La segunda categoría está relacionada con los diagramas de inclusión generados mediante Graphviz. Se observaron mensajes como:

```text
Include graph for 'catch_all.hpp' not generated, too many nodes (115), threshold is 50.
```

y:

```text
Included by graph for 'catch_move_and_forward.hpp' not generated, too many nodes (57), threshold is 50.
```

Estas advertencias indican que algunos grafos superaron el límite predeterminado de nodos utilizado por Doxygen. En particular, `catch_all.hpp` alcanzó 115 nodos, mientras que el límite configurado por defecto era de 50. Por esta razón, determinados diagramas de inclusión no fueron generados.

Las advertencias no fueron ocultadas ni eliminadas del registro, ya que forman parte de la evidencia del proceso de documentación. A pesar de estas advertencias, Doxygen terminó correctamente y produjo el sitio HTML completo.

---

## Evidencia de la generación

La estructura generada puede verificarse mediante:

```bash
ls -lh doxygen/html/
```

Entre los resultados se encuentran páginas HTML correspondientes a entidades del proyecto, por ejemplo:

```text
structCatch_1_1TestRunStats.html
structCatch_1_1Totals.html
structCatch_1_1Version.html
structCatch_1_1WaitForKeypress.html
structCatch_1_1WarnAbout.html
```

También se generaron diagramas mediante Graphviz, entre ellos:

```text
structCatch_1_1TestRunStats__coll__graph.png
structCatch_1_1true__given__inherit__graph.png
structCatch_1_1Totals__coll__graph.png
```

La presencia de estos archivos demuestra que la documentación no se limitó a generar páginas HTML básicas, sino que también incorporó referencias y representaciones gráficas de relaciones entre entidades.

---

# 7.3 Análisis de la documentación Doxygen

### Evidencia 1 — Página principal de Doxygen

La página principal de la documentación identifica el proyecto Catch2 y el commit específico analizado. Además, presenta la barra de navegación que permite acceder a los espacios de nombres, clases y archivos documentados. La interfaz también incorpora el buscador proporcionado por Doxygen.

![Página principal de la documentación Doxygen](evidencia-doxygen-01-main-page.png)

### Evidencia 2 — Organización del namespace `Catch`

La segunda evidencia corresponde a la página **Catch Namespace Reference** generada por Doxygen.

Esta página muestra cómo Doxygen organiza las entidades del proyecto dentro del espacio de nombres principal `Catch`. En la parte superior se presentan los diferentes subespacios de nombres, entre ellos `Benchmark`, `Clara`, `Detail`, `Generators`, `literals`, `Matchers`, `TestCaseTracking` y `TextFlow`.

Debajo se presenta el listado de clases y estructuras detectadas dentro del proyecto, incluyendo elementos como `Approx`, `AssertionHandler`, `AssertionInfo`, `AssertionReaction`, `AssertionResult` y `AssertionResultData`.

La página también proporciona navegación hacia otras categorías de entidades generadas automáticamente, como **Namespaces, Classes, Typedefs, Enumerations, Functions y Variables**. Esto facilita explorar el proyecto desde diferentes niveles de abstracción sin necesidad de recorrer directamente los archivos fuente.

Esta evidencia permite observar que Doxygen no solamente genera páginas individuales para las clases, sino que también construye una estructura de navegación que representa la organización lógica del código fuente.

![Referencia del namespace Catch](evidencia-doxygen-02-namespace.png)


### Evidencia 3 — Documentación de la clase `AssertionResult`

La tercera evidencia corresponde a la página de documentación de la clase `AssertionResult`, generada automáticamente por Doxygen.

Esta página permite consultar la información asociada con una clase específica del proyecto Catch2 y muestra cómo Doxygen organiza sus elementos internos. En ella se presentan la clase y sus diferentes miembros, incluyendo las funciones disponibles y la documentación asociada a cada elemento cuando esta se encuentra disponible en el código fuente.

La página permite pasar de una visión general del proyecto a una entidad concreta de su implementación. De esta manera, una persona desarrolladora puede consultar las operaciones proporcionadas por `AssertionResult` y acceder a la información correspondiente sin tener que buscar manualmente dentro de los archivos `.hpp` y `.cpp`.

Además, la página conserva la navegación general proporcionada por Doxygen, permitiendo regresar al listado de clases, consultar los espacios de nombres, acceder a los archivos fuente y explorar otras entidades relacionadas.

La documentación generada permite distinguir entre la información estructural obtenida directamente del código, como la existencia de la clase y sus miembros, y las descripciones adicionales provenientes de los comentarios estructurados presentes en el código fuente.

![Documentación de la clase AssertionResult](evidencia-doxygen-03-assertionresult.png)


### 8.4 Análisis de la documentación Sphinx

La documentación del proyecto Django fue generada utilizando **Sphinx**, a partir de archivos escritos en formato *reStructuredText* (`.rst`) y de la información obtenida directamente del código fuente mediante mecanismos de documentación automática. El resultado final se publicó en formato HTML, permitiendo navegar por la documentación mediante índices, enlaces internos y herramientas de búsqueda.

La estructura principal de la documentación se encuentra organizada mediante una página de portada, una sección narrativa de introducción y una sección destinada a la documentación automática de la API del paquete principal `django`.

#### 8.4.1 Portada y funcionamiento del `toctree`

La portada de la documentación corresponde al archivo `index.rst`. Esta página presenta el título del proyecto **Django**, una breve descripción y un índice de las diferentes secciones disponibles.

En la configuración utilizada se definió el siguiente árbol de contenidos:

```rst
.. toctree::
   :maxdepth: 2
   :caption: Contenido:

   introduccion
   api/modules
```

La directiva `toctree` permite establecer la estructura jerárquica de navegación de Sphinx. En este caso, se incorporaron dos documentos principales: `introduccion.rst`, que contiene la explicación narrativa del proyecto, y `api/modules.rst`, que funciona como punto de entrada para la documentación automática de la API.

En la portada generada se observa que ambas secciones aparecen como enlaces navegables. Esto permite que una persona usuaria pueda pasar desde la descripción general del proyecto hacia la introducción o directamente hacia la documentación de los módulos.

**Evidencia 1:** La portada muestra el título *Django*, la descripción del proyecto y los enlaces hacia la introducción y la documentación de la API.

> **Captura sugerida:** insertar aquí la captura de la página principal de Django generada por Sphinx.

**Enlace representativo:**

```text
sphinx/build/html/index.html
```

La portada demuestra que el `toctree` fue procesado correctamente y que Sphinx construyó una estructura de navegación a partir de los documentos `.rst`.

---

#### 8.4.2 Representación de paquetes, módulos, clases, funciones y métodos

La documentación automática se generó a partir del paquete principal `django/`. Para ello, Sphinx obtuvo información de los módulos Python existentes en el proyecto y generó archivos `.rst` individuales para diferentes componentes.

Entre los archivos generados se encuentran, por ejemplo:

```text
django.rst
django.apps.rst
django.conf.locale.ar.rst
django.core.rst
django.core.files.rst
django.db.rst
django.db.models.rst
django.forms.rst
django.http.rst
django.middleware.rst
django.urls.rst
django.utils.rst
django.views.rst
```

Esta organización permite representar jerárquicamente la estructura del código fuente. Los paquetes aparecen como agrupaciones de módulos y los módulos contienen la información correspondiente a sus objetos Python.

Dentro de las páginas de los módulos, Sphinx puede representar elementos como:

- clases;
- funciones;
- métodos;
- atributos;
- constantes;
- excepciones;
- firmas de funciones y métodos.

Por ejemplo, una página correspondiente a un módulo de Django permite pasar de la descripción general del módulo hacia los objetos que este contiene. De esta manera, la documentación HTML funciona como una representación navegable de la estructura del código fuente.

**Evidencia 2:** La página de introducción muestra que la documentación se concentra en el paquete principal `django/` y que este contiene diferentes subpaquetes relacionados con las funcionalidades del framework.

> **Captura sugerida:** utilizar la captura donde se observa la sección **“Organización del código”**.

---

#### 8.4.3 Información obtenida automáticamente de firmas y *docstrings*

Una de las principales ventajas de utilizar Sphinx con documentación automática es la posibilidad de obtener información directamente del código Python.

A partir de los módulos del proyecto, Sphinx puede representar automáticamente elementos como:

- nombre de las funciones;
- nombre de las clases;
- métodos;
- parámetros;
- firmas;
- valores predeterminados;
- tipos de datos cuando están disponibles;
- valores de retorno cuando están documentados;
- excepciones;
- documentación escrita mediante *docstrings*.

Por ejemplo, una función documentada dentro del código puede aparecer en la documentación HTML acompañada de su firma, indicando los parámetros que recibe. De forma similar, una clase puede mostrar sus métodos disponibles.

Esto evita tener que escribir manualmente una página independiente para cada función o clase del proyecto.

La documentación automática resulta especialmente importante en un proyecto de gran tamaño como Django, debido a que el código contiene una gran cantidad de módulos y objetos. En este laboratorio se generaron cientos de archivos `.rst`, correspondientes a diferentes componentes de la biblioteca.

**Evidencia 3:** La documentación generada incluye una sección específica para la API de Django, a partir de la cual se puede navegar hacia los diferentes módulos documentados automáticamente.

> **Captura sugerida:** insertar una captura de una página concreta de la API donde se observen clases, funciones o métodos.

---

#### 8.4.4 Contenido narrativo escrito manualmente

No toda la documentación debe generarse automáticamente a partir del código fuente. En este laboratorio también se creó contenido narrativo manual mediante el archivo:

```text
sphinx/source/introduccion.rst
```

Este archivo contiene información que no necesariamente puede deducirse de las firmas o *docstrings* del código.

Entre el contenido escrito manualmente se encuentra:

- el propósito del proyecto;
- una explicación general de Django;
- la organización del código;
- una descripción de los principales componentes;
- la explicación del proceso de generación de documentación;
- el alcance de la documentación realizada.

Por ejemplo, se explicó que Django es un framework web de alto nivel desarrollado en Python y que proporciona componentes reutilizables para facilitar el desarrollo de aplicaciones web.

Este contenido narrativo es necesario porque una herramienta de documentación automática puede describir qué elementos existen en el código, pero no necesariamente puede explicar de forma clara **por qué existe el proyecto, cómo se organiza conceptualmente o qué debería conocer una persona que lo utiliza por primera vez**.

Por esta razón, la documentación automática y la documentación narrativa cumplen funciones complementarias.

**Evidencia 4:** La página *Introducción al proyecto Django* presenta información narrativa que fue escrita manualmente para proporcionar contexto antes de entrar en la documentación técnica de la API.

> **Captura sugerida:** utilizar la captura donde aparece el título **“Introducción al proyecto Django”** y la sección **“Propósito”**.

---

#### 8.4.5 Parámetros, tipos, retornos, excepciones, índices, búsqueda y enlaces

La documentación HTML generada por Sphinx proporciona diferentes mecanismos para facilitar la consulta de una API de software.

En las páginas correspondientes a módulos y objetos Python pueden aparecer las firmas de funciones y métodos, permitiendo identificar los parámetros que recibe cada elemento.

Cuando el código contiene información suficiente en sus firmas o *docstrings*, la documentación puede presentar información relacionada con:

- parámetros de entrada;
- tipos de parámetros;
- valores de retorno;
- tipos de retorno;
- excepciones;
- atributos;
- métodos disponibles.

Además de la información propia de cada módulo, Sphinx genera automáticamente elementos de navegación como el **índice general** y el **índice de búsqueda**.

Durante la compilación realizada en el laboratorio se obtuvo:

```text
generating indices... genindex done
writing additional pages... search done
dumping search index in English (code: en)... done
dumping object inventory... done
```

Esto demuestra que Sphinx generó tanto un índice general como una estructura de búsqueda para facilitar la localización de elementos dentro de la documentación.

La interfaz HTML también incorpora enlaces de navegación entre las diferentes páginas. Por ejemplo, desde la portada se puede acceder a la introducción y posteriormente a la documentación de la API.

**Evidencia 5:** La documentación HTML presenta una barra de búsqueda y una sección de navegación que permiten localizar contenido dentro del proyecto documentado.

> **Captura sugerida:** tomar una captura de una página de la API donde se vea la barra de búsqueda y, preferiblemente, información de una clase o función con su firma y documentación.

---

#### 8.4.6 Conocimientos que puede adquirir una persona nueva en el proyecto

Una persona desarrolladora que no conozca previamente el proyecto puede utilizar la documentación generada para obtener una visión progresiva del código.

En primer lugar, la portada permite identificar el proyecto y acceder a sus diferentes secciones. Posteriormente, la introducción proporciona una explicación general sobre el propósito de Django y la organización del código.

La sección de organización del código permite identificar que el paquete principal `django/` contiene diferentes componentes relacionados con funcionalidades como:

- administración;
- aplicaciones;
- autenticación;
- caché;
- bases de datos;
- formularios;
- HTTP;
- middleware;
- modelos;
- sesiones;
- plantillas;
- utilidades;
- vistas.

Finalmente, la documentación de la API permite profundizar en módulos, clases, funciones y métodos concretos.

Por lo tanto, la documentación permite pasar de una visión **general del proyecto** a una visión **estructural del código** y posteriormente a una consulta **específica de elementos de la API**.

Esta organización resulta útil tanto para una persona que está aprendiendo el proyecto como para una desarrolladora que necesita consultar rápidamente el funcionamiento o la interfaz de un componente determinado.

---

#### 8.4.7 Elementos incompletos, poco claros o sin documentar

A pesar de que la generación de la documentación terminó correctamente, se presentaron **905 advertencias** durante la compilación:

```text
build succeeded, 905 warnings.
```

Es importante diferenciar estas advertencias de un error de compilación. Sphinx indicó explícitamente que la construcción fue exitosa, por lo que las páginas HTML fueron generadas correctamente.

Sin embargo, la cantidad de advertencias indica que existen elementos de la documentación que podrían mejorarse. En un proyecto grande como Django, algunas referencias cruzadas, objetos, módulos o relaciones entre componentes pueden no quedar completamente resueltas durante la generación automática.

Además, la documentación automática depende de la información disponible en el propio código fuente. Cuando una función, clase o método no posee un *docstring* suficientemente descriptivo, Sphinx puede mostrar la firma y estructura del objeto sin proporcionar una explicación conceptual completa.

También existe una diferencia entre documentar la estructura de una API y explicar cómo utilizarla. La documentación automática permite conocer qué módulos y objetos existen, pero no necesariamente proporciona ejemplos de uso, decisiones de diseño, flujos de trabajo o recomendaciones para personas que están comenzando.

Por esta razón, la documentación manual incluida en `introduccion.rst` resulta necesaria como complemento de la documentación automática.

En resumen, los principales aspectos que quedaron limitados fueron:

1. **La presencia de 905 advertencias durante la compilación**, que indica que existen referencias o elementos que podrían revisarse.
2. **La dependencia de los** ***docstrings*** **existentes**, ya que la calidad de la documentación automática depende de la información disponible en el código.
3. **La ausencia de explicaciones conceptuales para todos los componentes**, debido a que Sphinx documenta principalmente la estructura de la API.
4. **La falta de ejemplos de uso para algunos elementos**, ya que la documentación generada automáticamente no sustituye completamente una guía de usuario.
5. **La gran cantidad de módulos del proyecto**, que hace que la documentación sea extensa y pueda resultar compleja para una persona que recién comienza.

---

#### 8.4.8 Síntesis del análisis

La experiencia demuestra que Sphinx permite combinar dos tipos de documentación: **documentación narrativa escrita manualmente** y **documentación técnica generada automáticamente a partir del código fuente**.

La documentación manual proporciona el contexto necesario para comprender el propósito y la organización del proyecto, mientras que la documentación automática permite consultar de manera sistemática los módulos y objetos que forman parte de la API.

En este laboratorio, el archivo `index.rst` permitió construir la portada y organizar las diferentes secciones mediante `toctree`. A partir de `introduccion.rst` se incorporó información conceptual sobre Django, mientras que los archivos ubicados en `api/` permitieron representar automáticamente la estructura del paquete principal.

La compilación terminó exitosamente y generó las páginas HTML dentro de:

```text
sphinx/build/html/
```

Aunque se produjeron 905 advertencias, Sphinx confirmó:

```text
build succeeded
```

Por lo tanto, se consiguió generar una documentación navegable del proyecto, con portada, introducción, documentación de API, índices, búsqueda y enlaces internos.

---

#### 8.4.9 Evidencias representativas

## Evidencia: Visualización de la documentación generada con Sphinx

### Evidencia 1 — Página principal de la documentación

La primera evidencia muestra la página principal de la documentación HTML generada mediante Sphinx para el proyecto Django. En ella se observa el título **Django**, una descripción general del proyecto y el índice de contenidos generado a partir de los archivos `.rst`.

El índice permite acceder a la sección de **Introducción al proyecto Django** y a la documentación automática de la API mediante el módulo `django`.

Esta evidencia demuestra que Sphinx procesó correctamente el archivo `index.rst` y que las diferentes secciones de documentación fueron integradas en una página principal navegable.

**Resultado:** Se obtiene correctamente la página principal de la documentación HTML del proyecto Django.


### Evidencia 2 — Introducción al proyecto Django

La segunda evidencia muestra la sección **Introducción al proyecto Django**, generada a partir del archivo `introduccion.rst`.

En esta sección se presenta el propósito del proyecto, indicando que Django es un framework web de alto nivel desarrollado en Python. También se explica la organización del código fuente y se describen algunos de los principales componentes del framework, como administración, aplicaciones, autenticación, caché, bases de datos, formularios, HTTP, middleware, modelos, sesiones, plantillas, utilidades y vistas.

La página demuestra que el contenido escrito manualmente en formato reStructuredText fue convertido correctamente por Sphinx a una página HTML estructurada y legible.

**Resultado:** La documentación introductoria se genera correctamente y queda integrada dentro de la documentación general del proyecto.


### Evidencia 3 — Organización del código y generación automática de documentación

La tercera evidencia muestra la continuación de la sección de introducción, donde se detalla la organización del código fuente de Django y se explica el proceso utilizado para generar la documentación.

Se observa que la documentación automática se concentra en el paquete principal `django/` y que se utiliza Sphinx junto con sus extensiones para obtener información de los módulos Python.

Esta evidencia permite comprobar que el proyecto fue preparado para generar documentación a partir del código fuente y que la información resultante puede consultarse mediante la interfaz HTML generada por Sphinx.

**Resultado:** Se verifica la integración entre el código fuente del proyecto Django, Sphinx y la documentación HTML generada automáticamente.

# 9. Parte V - Comparación entre Doxygen y Sphinx

La comparación entre Doxygen y Sphinx permite observar dos estrategias diferentes para generar documentación técnica a partir de proyectos de software. En este laboratorio se utilizó **Doxygen** para documentar el proyecto C++ **Catch2** y **Sphinx** para documentar el proyecto Python **Django**. En ambos casos se obtuvo una documentación HTML navegable, pero las herramientas presentan diferencias importantes en la forma en que obtienen la información, organizan el contenido y facilitan la comprensión del proyecto.

La comparación se realiza tomando como referencia los resultados obtenidos durante el laboratorio y no únicamente las características generales de cada herramienta.

| Dimensión | Doxygen en C++ | Sphinx en Python |
|---|---|---|
| **Fuente principal de la información** | Obtiene información principalmente del código fuente C++, incluyendo clases, estructuras, funciones, archivos, namespaces, relaciones y comentarios de documentación. En este laboratorio se utilizó `EXTRACT_ALL = YES`, lo que permitió extraer entidades aunque no todas tuvieran comentarios estructurados. | Obtiene información del código Python mediante mecanismos de documentación automática y se complementa con archivos `.rst` escritos manualmente. En este laboratorio se utilizaron páginas automáticas para la API y un archivo `introduccion.rst` para el contenido narrativo. |
| **Configuración y proceso de generación** | Requiere configurar un archivo `Doxyfile`. En este laboratorio se configuraron opciones como `INPUT = src`, `RECURSIVE = YES`, `GENERATE_HTML = YES`, `SOURCE_BROWSER = YES`, `HAVE_DOT = YES`, `CALL_GRAPH = YES` y `CALLER_GRAPH = YES`. La generación se realizó mediante `doxygen Doxyfile`. | Requiere configurar la estructura de Sphinx, los archivos `.rst`, el `toctree` y los mecanismos de documentación automática. La generación se realiza mediante el proceso de compilación de Sphinx. En este laboratorio se creó `index.rst`, `introduccion.rst` y la sección `api/`. |
| **Organización y navegación** | Organiza la documentación mediante namespaces, clases, estructuras, archivos, funciones y otras categorías. La interfaz incluye navegación entre entidades y un buscador. | Organiza la documentación mediante una portada y un árbol de contenidos construido con `toctree`. Permite navegar desde la introducción hasta los módulos de la API y cuenta con índices y búsqueda. |
| **Documentación de API** | Es especialmente adecuada para representar APIs C++, mostrando clases, estructuras, miembros, funciones, namespaces, archivos y relaciones entre entidades. La página de `AssertionResult` es un ejemplo concreto del nivel de detalle alcanzado. | Permite documentar módulos, clases, funciones y métodos Python. La información se genera a partir del código y puede complementarse con documentación escrita manualmente. |
| **Diagramas y referencias cruzadas** | Presenta una ventaja importante en este laboratorio debido a la integración con Graphviz. Se generaron diagramas de clases, grafos de llamadas y relaciones entre entidades. Sin embargo, algunos grafos no pudieron generarse debido al límite de nodos. | Permite generar referencias y navegación entre elementos de la documentación, pero en esta práctica el resultado estuvo más orientado a índices, navegación y documentación de API que a una representación gráfica extensa de la arquitectura. |
| **Contenido narrativo** | Puede incorporar páginas adicionales, pero el resultado de esta práctica estuvo principalmente orientado a la documentación técnica extraída del código. Se creó una página principal mediante `mainpage.md`. | Presenta una ventaja clara para combinar documentación técnica y contenido narrativo. El archivo `introduccion.rst` permitió explicar el propósito de Django, su organización, sus componentes y el proceso de documentación. |
| **Dependencia de comentarios o *docstrings*** | La información estructural puede obtenerse directamente del código, pero las explicaciones detalladas dependen de los comentarios de documentación disponibles. `EXTRACT_ALL = YES` permitió documentar entidades aunque no todas tuvieran comentarios. | La documentación automática depende de la información disponible en las firmas y *docstrings*. Cuando estos son insuficientes, puede existir información estructural sin una explicación conceptual completa. |
| **Facilidad de mantenimiento** | Una vez configurado el `Doxyfile`, la documentación puede regenerarse directamente a partir del código. Sin embargo, es necesario mantener correctamente la configuración y los comentarios de documentación. | La documentación automática puede regenerarse a partir del código, pero además deben mantenerse los archivos `.rst` escritos manualmente. Esto permite mayor control sobre el contenido narrativo, aunque introduce más archivos que mantener. |
| **Audiencia principal** | Resulta especialmente apropiado para personas desarrolladoras que necesitan consultar una API C++ compleja, sus clases, archivos, namespaces y relaciones internas. | Puede servir tanto a personas desarrolladoras que consultan la API como a personas nuevas en el proyecto gracias a la posibilidad de combinar documentación técnica con explicaciones narrativas. |
| **Fortalezas y limitaciones** | Su principal fortaleza en este laboratorio fue la representación estructural y gráfica del código C++, incluyendo clases, namespaces, archivos y relaciones mediante Graphviz. Como limitación, produjo 30 advertencias y algunos diagramas de inclusión no fueron generados debido al límite de nodos. | Su principal fortaleza fue la combinación de documentación automática y contenido narrativo mediante `toctree`. Como limitación, la compilación produjo 905 advertencias, lo que muestra que existen elementos que podrían requerir revisión y que la documentación automática depende de la calidad de los *docstrings* y referencias disponibles. |

### 9.1 ¿Cuál herramienta produjo información útil con menos configuración y por qué?

Considerando el trabajo realizado en ambos proyectos, **Doxygen produjo información útil con una configuración relativamente más directa**.

En el caso de Catch2 fue suficiente establecer un archivo `Doxyfile` con el directorio de entrada, la generación HTML, la extracción de entidades y las opciones relacionadas con Graphviz. Posteriormente, la documentación pudo generarse mediante:

```bash
doxygen Doxyfile
```

El resultado permitió obtener automáticamente páginas para namespaces, clases, estructuras, archivos, funciones y otras entidades del proyecto. Además, se generaron diagramas y referencias cruzadas.

Por otra parte, Sphinx ofreció mayor flexibilidad, pero requirió organizar explícitamente una estructura de documentación mediante archivos `.rst`. Fue necesario crear una portada mediante `index.rst`, configurar el `toctree`, crear una introducción narrativa y preparar la sección destinada a la API.

Por lo tanto, para obtener rápidamente una representación técnica del código fuente, **Doxygen resultó más directo en este laboratorio**. Sphinx requirió más preparación, pero esa configuración adicional permitió obtener un resultado más controlado y adaptable.

---

### 9.2 ¿Cuál resultado ayuda mejor a comprender la arquitectura del proyecto?

Para comprender la **arquitectura interna del proyecto**, el resultado de **Doxygen sobre Catch2** resulta más útil.

La documentación de Doxygen presenta diferentes niveles estructurales del código: namespaces, clases, estructuras, archivos, funciones y relaciones entre entidades. Además, en este laboratorio se habilitaron Graphviz, los diagramas de clases, los grafos de llamadas y las referencias entre entidades.

Por ejemplo, la documentación de `AssertionResult` permite pasar desde el namespace `Catch` hasta una clase concreta y consultar sus miembros. Asimismo, los diagramas generados permiten observar relaciones entre diferentes componentes.

Esto proporciona una representación bastante cercana a la estructura real del código fuente.

Sphinx también permite comprender la organización de Django, especialmente mediante la estructura de paquetes y módulos y la navegación proporcionada por `toctree`. Sin embargo, en la configuración realizada el énfasis estuvo principalmente en la navegación documental y la descripción de la API, y no en la generación de diagramas de arquitectura.

Por lo tanto, **Doxygen proporciona en este laboratorio una representación más directa de la arquitectura estructural del código**.

---

### 9.3 ¿Cuál resultado ayuda mejor a aprender a utilizar la API?

Para aprender a utilizar la API, **Sphinx proporciona un resultado más completo en el contexto de este laboratorio**.

La principal razón es que Sphinx permitió combinar la documentación automática de la API con contenido narrativo escrito específicamente para una persona que no conoce previamente el proyecto.

La página `introduccion.rst` explica qué es Django, cuál es su propósito y cómo se organiza el código. Posteriormente, la documentación permite navegar hacia módulos, clases, funciones y métodos.

Esta combinación permite seguir una progresión:

```text
Introducción
      ↓
Organización del proyecto
      ↓
Paquetes y módulos
      ↓
Clases y funciones
      ↓
Métodos y firmas
```

Doxygen también proporciona información muy útil para consultar una API, especialmente en proyectos C++ complejos. Sin embargo, en este laboratorio la documentación de Catch2 estuvo más orientada a representar la estructura técnica del código que a enseñar progresivamente cómo utilizar el proyecto.

Por esta razón, **Sphinx resulta más adecuado para aprender la API cuando se combina la documentación automática con contenido narrativo y ejemplos**.

---

### 9.4 ¿Qué problemas del código fuente quedaron expuestos al generar la documentación?

La generación de documentación permitió detectar problemas o limitaciones que no necesariamente serían evidentes al observar únicamente que el proyecto compila.

En **Catch2**, Doxygen produjo **30 advertencias**. Entre ellas se encontraron mensajes como:

```text
warning: no matching class member found for
```

Estos mensajes aparecieron, entre otros lugares, en archivos como:

```text
src/catch2/internal/catch_jsonwriter.cpp
src/catch2/internal/catch_stringref.cpp
src/catch2/internal/catch_xmlwriter.cpp
```

Esto indica que Doxygen encontró determinados elementos documentados que no pudo asociar correctamente con miembros de clase durante su análisis.

También se encontraron problemas relacionados con los diagramas de inclusión generados por Graphviz. Por ejemplo:

```text
Include graph for 'catch_all.hpp' not generated, too many nodes (115), threshold is 50.
```

Esto muestra que la estructura de dependencias de algunos archivos es suficientemente grande como para superar el límite utilizado para generar determinados diagramas.

En **Django**, Sphinx terminó la construcción correctamente, pero produjo **905 advertencias**:

```text
build succeeded, 905 warnings.
```

Esto indica que la documentación pudo generarse, pero que existen elementos que podrían revisarse, especialmente referencias, objetos o relaciones que no fueron resueltos completamente durante la construcción.

Además, el proceso mostró la importancia de la calidad de los *docstrings*. Cuando estos no proporcionan suficiente información, la documentación automática puede mostrar la existencia de una función, clase o método, pero no necesariamente explicar claramente su propósito o forma de utilización.

Por lo tanto, la documentación actuó también como una herramienta de diagnóstico: permitió identificar **problemas de asociación entre comentarios y miembros, referencias no resueltas, estructuras demasiado complejas para determinados diagramas y documentación insuficiente**.

---

### 9.5 ¿Qué cambios integraría al flujo de desarrollo para mantener la documentación actualizada?

Para mantener la documentación actualizada, integraría su generación directamente dentro del flujo de desarrollo del proyecto.

En primer lugar, establecería una regla para que los cambios en la API incluyan también los cambios correspondientes en la documentación. Cuando se agregue, modifique o elimine una clase, función o método, se debería actualizar su comentario o *docstring* correspondiente.

En el caso de Sphinx, también se deberían actualizar los archivos `.rst` narrativos cuando cambie la arquitectura o el funcionamiento general del proyecto.

El flujo podría organizarse de la siguiente manera:

```text
Cambio en el código
        ↓
Actualización de comentarios/docstrings
        ↓
Actualización de documentación narrativa
        ↓
Generación automática
        ↓
Revisión de advertencias
        ↓
Publicación
```

Además, utilizaría una estructura de documentación versionada junto con el código fuente. De esta manera, la documentación correspondiente a una versión determinada del proyecto permanecería asociada al código que describe.

Para proyectos como Catch2 y Django, también sería conveniente establecer criterios mínimos de documentación para nuevas APIs, por ejemplo:

- Toda clase pública nueva debe tener documentación.
- Toda función o método público debe indicar su propósito.
- Los parámetros importantes deben estar documentados.
- Los valores de retorno deben describirse cuando no sean evidentes.
- Las excepciones relevantes deben documentarse.
- Los cambios importantes en la arquitectura deben reflejarse en la documentación narrativa.
- Las referencias rotas deben corregirse antes de publicar una nueva versión.

De esta manera, la documentación dejaría de ser una tarea realizada únicamente al final del proyecto y pasaría a formar parte del proceso normal de desarrollo.

---

### 9.6 ¿Qué verificaciones automatizaría en integración continua?

En integración continua automatizaría principalmente la **generación de la documentación y la detección de errores o advertencias**.

Para Doxygen, ejecutaría automáticamente la generación utilizando el archivo `Doxyfile` y verificaría que el proceso termine correctamente. También revisaría el registro de salida para detectar advertencias relacionadas con miembros no encontrados, referencias incorrectas o problemas en los diagramas.

Para Sphinx, ejecutaría automáticamente la construcción de la documentación y comprobaría que el proceso termine correctamente. Además, prestaría especial atención a las advertencias generadas durante la compilación, ya que una construcción puede terminar con éxito aun cuando existan cientos de advertencias.

Las verificaciones podrían incluir:

1. **Comprobar que Doxygen termine correctamente.**
2. **Comprobar que Sphinx termine correctamente.**
3. **Detectar enlaces o referencias rotas.**
4. **Detectar objetos documentados que no existan en el código.**
5. **Detectar advertencias nuevas respecto a una ejecución anterior.**
6. **Comprobar que se genere la página principal HTML.**
7. **Comprobar que los índices y mecanismos de búsqueda sean generados.**
8. **Verificar que los archivos de diagramas esperados sean creados cuando corresponda.**
9. **Comprobar que los archivos `.rst` y la documentación narrativa sean válidos.**
10. **Publicar automáticamente la documentación cuando la construcción sea exitosa.**

Una estrategia especialmente útil sería tratar las advertencias como parte de la calidad del proyecto. No necesariamente sería conveniente hacer fallar inmediatamente la integración ante cualquier advertencia, pero sí registrar su cantidad y establecer como objetivo que esta cantidad disminuya progresivamente.

Por ejemplo, en este laboratorio se obtuvieron:

```text
Doxygen: 30 advertencias
Sphinx: 905 advertencias
```

Estos valores podrían utilizarse como punto de referencia inicial. En futuras modificaciones, la integración continua podría comprobar que el número de advertencias no aumente y, posteriormente, establecer metas para reducirlas.

---

### 9.7 Conclusión de la comparación

Los resultados obtenidos muestran que **Doxygen y Sphinx cumplen funciones similares, pero presentan enfoques diferentes**.

Doxygen destacó en la documentación estructural de Catch2. Con una configuración relativamente directa fue posible generar páginas para namespaces, clases, estructuras, archivos y funciones, además de diagramas y relaciones entre entidades mediante Graphviz. Esto lo convierte en una herramienta especialmente apropiada para explorar la arquitectura interna de proyectos C++ grandes.

Sphinx, por su parte, destacó por su capacidad para combinar documentación automática y contenido narrativo. La utilización de `index.rst`, `toctree` e `introduccion.rst` permitió construir una documentación que no solamente describe la API de Django, sino que también proporciona contexto para una persona que se aproxima al proyecto por primera vez.

La principal diferencia observada es que **Doxygen está más orientado a extraer y representar la estructura técnica del código**, mientras que **Sphinx proporciona mayor flexibilidad para construir un sistema documental completo alrededor del código**.

En consecuencia, la elección de una herramienta depende del objetivo principal. Si se necesita obtener rápidamente una representación detallada de la estructura de un proyecto C++, Doxygen ofrece ventajas importantes. Si se busca construir una documentación que combine referencia de API, explicación conceptual, navegación personalizada y contenido narrativo, Sphinx ofrece mayor flexibilidad.

Finalmente, ambos procesos demostraron que la generación automática de documentación también puede funcionar como una forma de control de calidad. Las **30 advertencias de Doxygen** y las **905 advertencias de Sphinx** muestran que la documentación puede revelar inconsistencias, referencias no resueltas, problemas de asociación y limitaciones en la información disponible. Por ello, la generación de documentación debería integrarse al flujo de desarrollo y a la integración continua, en lugar de considerarse únicamente una actividad final del proyecto.