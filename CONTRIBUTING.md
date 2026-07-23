# Contribuir a Atlas Mitológico

¡Gracias por tu interés en contribuir a Atlas Mitológico!

Este proyecto busca convertirse en una base de datos abierta, estructurada y verificable sobre las mitologías del mundo. Toda ayuda es bienvenida, ya sea corrigiendo errores, mejorando la documentación, agregando nuevas entidades o desarrollando nuevas funcionalidades.

Antes de contribuir, por favor lee esta guía.

---

# Filosofía del proyecto

Atlas Mitológico tiene como objetivo:

- preservar información mitológica de forma estructurada;
- documentar variantes sin eliminar contradicciones;
- separar hechos documentados de interpretaciones modernas;
- utilizar fuentes confiables;
- mantener una base de datos reutilizable mediante una API pública;
- desarrollar una plataforma abierta para la comunidad.

El proyecto **no intenta establecer una versión oficial o definitiva de ninguna tradición mitológica**.

Cuando existan múltiples versiones de un mismo mito, todas podrán coexistir siempre que estén correctamente documentadas.

---

# Formas de contribuir

Puedes colaborar de muchas maneras.

## Documentación

- corregir errores ortográficos;
- mejorar explicaciones;
- traducir documentación;
- añadir ejemplos;
- proponer mejoras en la organización.

---

## Base de datos

Puedes ayudar agregando:

- nuevas entidades;
- nombres alternativos;
- dominios;
- relaciones;
- tradiciones;
- fuentes;
- referencias bibliográficas.

---

## Desarrollo

Puedes colaborar con:

- Laravel
- React
- TypeScript
- PostgreSQL
- Tailwind
- pruebas automatizadas
- optimización
- accesibilidad
- rendimiento

---

## Reporte de errores

Antes de abrir un Issue verifica si el problema ya fue reportado.

Cuando reportes un error intenta incluir:

- descripción clara;
- pasos para reproducirlo;
- comportamiento esperado;
- comportamiento observado;
- capturas de pantalla cuando sea posible;
- versión utilizada;
- sistema operativo;
- navegador (si aplica).

---

# Flujo de trabajo recomendado

1. Haz un Fork del repositorio.

2. Clona tu Fork.

```bash
git clone https://github.com/TU_USUARIO/atlas-mitologico.git
```

3. Crea una rama nueva.

```bash
git checkout -b feature/nombre-de-la-funcionalidad
```

Ejemplos:

```text
feature/api-search
feature/import-greek

fix/entity-duplicates
fix/api-pagination

docs/database
docs/readme

refactor/services
```

4. Realiza tus cambios.

5. Ejecuta las pruebas.

6. Haz commit.

7. Sube la rama.

```bash
git push origin feature/nombre-de-la-funcionalidad
```

8. Abre un Pull Request.

---

# Convenciones para ramas

Utiliza alguno de los siguientes prefijos.

## Nuevas funcionalidades

```text
feature/
```

Ejemplo

```text
feature/entity-images
```

---

## Corrección de errores

```text
fix/
```

Ejemplo

```text
fix/search-pagination
```

---

## Documentación

```text
docs/
```

Ejemplo

```text
docs/api-guide
```

---

## Refactorización

```text
refactor/
```

---

## Pruebas

```text
test/
```

---

## Optimización

```text
perf/
```

---

# Convención para commits

Se recomienda utilizar Conventional Commits.

Ejemplos

```text
feat: add entity search endpoint

feat: add mythology importer

fix: resolve duplicate relationship creation

docs: update editorial guide

docs: improve README

refactor: move import logic into service

test: add relationship feature tests

perf: optimize search query
```

---

# Estilo de código

## PHP

Seguir PSR-12.

Utilizar Laravel Pint cuando sea posible.

Antes de enviar un Pull Request ejecutar:

```bash
./vendor/bin/pint
```

---

## JavaScript

Utilizar ESLint.

---

## TypeScript

Evitar el uso innecesario de:

```text
any
```

---

## React

Preferir componentes pequeños.

Separar lógica y presentación.

Evitar componentes excesivamente grandes.

---

# Arquitectura

Antes de crear una nueva funcionalidad revisa:

```
docs/ARCHITECTURE.md
```

La lógica de negocio no debe colocarse dentro de los controladores.

Preferir:

- Services
- Actions
- Form Requests
- API Resources

---

# Base de datos

Antes de modificar el modelo revisar:

```
docs/DATABASE.md
```

No crear relaciones utilizando columnas de texto cuando ya exista una estructura relacional.

Ejemplo incorrecto

```text
parents = "Crono, Rea"
```

Ejemplo correcto

```
relationships
```

---

# Guía editorial

Todo cambio relacionado con datos mitológicos debe seguir:

```
docs/EDITORIAL_GUIDE.md
```

Especialmente:

- categorías;
- relaciones;
- fuentes;
- variantes;
- tradiciones;
- sincretismos.

---

# Pull Requests

Un Pull Request debe tener un objetivo claro.

Incluye:

- descripción;
- motivo del cambio;
- posibles efectos secundarios;
- capturas de pantalla cuando haya cambios visuales.

Si corrige un Issue:

```text
Fixes #123
```

---

# Qué revisar antes de enviar un Pull Request

## Código

- [ ] Compila correctamente.
- [ ] No genera errores.
- [ ] Sigue el estilo del proyecto.
- [ ] No rompe funcionalidades existentes.

---

## Base de datos

- [ ] Migraciones probadas.
- [ ] Relaciones verificadas.
- [ ] No elimina información existente.

---

## API

- [ ] Respuestas consistentes.
- [ ] Códigos HTTP correctos.
- [ ] Validaciones implementadas.
- [ ] Recursos JSON correctos.

---

## Documentación

- [ ] README actualizado cuando sea necesario.
- [ ] Nuevas funciones documentadas.
- [ ] Ejemplos agregados si aplica.

---

## Pruebas

- [ ] PHPUnit ejecutado.
- [ ] Todas las pruebas pasan.
- [ ] Nuevas pruebas añadidas cuando corresponda.

---

# Datos mitológicos

Si agregas una nueva entidad procura incluir:

- nombre principal;
- mitología;
- categoría;
- resumen;
- nombres alternativos;
- dominios;
- relaciones;
- fuentes.

Si alguno de estos datos no está disponible, puede completarse posteriormente.

---

# Fuentes

Siempre que sea posible utiliza:

- textos primarios;
- publicaciones académicas;
- editoriales reconocidas;
- instituciones culturales;
- museos;
- universidades.

Evita utilizar únicamente:

- blogs;
- foros;
- videos sin referencias;
- contenido generado por IA sin revisión;
- páginas sin autor identificado.

---

# Derechos de autor

No copies grandes fragmentos de:

- libros;
- artículos;
- enciclopedias;
- sitios web.

Las descripciones deben redactarse de forma original.

Las citas breves deben incluir atribución.

---

# Compatibilidad

Actualmente el proyecto utiliza:

- Laravel
- PostgreSQL
- React
- Inertia
- Tailwind CSS

Las nuevas contribuciones deberían mantener compatibilidad con estas tecnologías.

---

# Seguridad

Si descubres una vulnerabilidad de seguridad:

**No abras un Issue público.**

En su lugar, contacta al mantenedor del proyecto para que pueda revisarla antes de hacerla pública.

---

# Licencia

Al contribuir aceptas que tu código y documentación serán publicados bajo la licencia MIT del proyecto.

---

# Agradecimientos

Toda contribución, por pequeña que sea, ayuda a construir una mejor referencia abierta sobre las mitologías del mundo.

¡Gracias por formar parte de Atlas Mitológico!