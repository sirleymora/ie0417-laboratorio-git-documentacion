Introducción al proyecto Django
===============================

Propósito
---------

Django es un framework web de alto nivel desarrollado en Python. El proyecto
proporciona componentes reutilizables para facilitar el desarrollo de
aplicaciones web, incluyendo herramientas para modelos, vistas, formularios,
autenticación, administración y manejo de solicitudes HTTP.

En este laboratorio se utiliza Django como proyecto de referencia para
evaluar las capacidades de Sphinx en la generación automática de
documentación a partir de un proyecto Python real.

Organización del código
-----------------------

El código fuente principal se encuentra dentro del paquete ``django/``.
Este directorio contiene diferentes subpaquetes que implementan las
funcionalidades principales del framework.

Entre los componentes disponibles se encuentran módulos relacionados con:

* administración;
* aplicaciones;
* autenticación;
* caché;
* bases de datos;
* formularios;
* HTTP;
* middleware;
* modelos;
* sesiones;
* plantillas;
* utilidades;
* vistas.

El proyecto también contiene un directorio ``tests/`` con una gran cantidad de
pruebas utilizadas para verificar el funcionamiento del framework.

Para este laboratorio, la documentación automática se concentra en el
paquete principal ``django/``.

Generación de la documentación
------------------------------

La documentación se genera utilizando Sphinx y sus extensiones.

``sphinx.ext.autodoc`` permite obtener información de los módulos Python
mediante la importación del código fuente.

``sphinx.ext.autosummary`` permite generar resúmenes de los elementos
documentados.

``sphinx.ext.napoleon`` permite interpretar determinados estilos de
docstrings.

``sphinx.ext.viewcode`` permite enlazar la documentación con el código fuente
cuando esta funcionalidad está disponible.

La estructura de la referencia de API será generada mediante
``sphinx-apidoc``.

Alcance
-------

El objetivo de esta documentación no es reemplazar la documentación oficial
de Django. Su propósito es demostrar el proceso de generación de
documentación mediante una configuración propia de Sphinx y analizar qué
información puede obtenerse automáticamente desde el código fuente.

El resultado será utilizado posteriormente para comparar Sphinx con Doxygen,
utilizado en el proyecto C++ seleccionado para este laboratorio.
