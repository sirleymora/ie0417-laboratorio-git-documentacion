# IE0417 — Laboratorio de Git y Documentación

**Estudiante:** Sirley Mora Chavarría
**Carné:** C15059

**Curso:** IE0417
**Laboratorio:** Git y Documentación

---

### Repositorio de entrega

https://github.com/sirleymora/ie0417-laboratorio-git-documentacion

### Sitio público

https://sirleymora.github.io/ie0417-laboratorio-git-documentacion/

### Documentación

**C++ — Doxygen**
https://sirleymora.github.io/ie0417-laboratorio-git-documentacion/cpp/

**Python — Sphinx**
https://sirleymora.github.io/ie0417-laboratorio-git-documentacion/python/

### Proyectos seleccionados

**Catch2 — C++**
https://github.com/catchorg/Catch2

**Django — Python**
https://github.com/django/django

### Regeneración

La documentación de **C++** se genera mediante Doxygen utilizando:

```bash
doxygen doxygen/Doxyfile
```

La documentación de **Python** se genera mediante Sphinx utilizando:

```bash
cd Proyectos/django
sphinx-build -b html sphinx/source sphinx/build/html
```

### Herramientas utilizadas

**Git:** 2.43.0  
**Doxygen:** 1.9.8  
**Graphviz:** 2.43.0  
**Python:** 3.12.3  
**Sphinx:** `PENDIENTE`
