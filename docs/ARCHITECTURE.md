# Arquitectura del proyecto

## 1. Objetivo

Atlas Mitológico es una plataforma web y una API pública para consultar entidades, relaciones, tradiciones, dominios y fuentes de distintas mitologías.

La arquitectura debe permitir:

- ejecución local;
- publicación en un servidor;
- consumo desde otros proyectos;
- crecimiento a miles de entidades;
- importación masiva de datos;
- panel administrativo;
- documentación pública de la API.

---

## 2. Stack tecnológico

### Backend

- Laravel
- PHP
- Laravel Sanctum
- API REST versionada

### Frontend

- React
- Inertia
- TypeScript
- Tailwind CSS

### Base de datos

- PostgreSQL

### Desarrollo local

- Composer
- Node.js
- Vite
- Docker Compose en una etapa posterior

---

## 3. Tipo de arquitectura

La primera versión utilizará una arquitectura monolítica modular.

Laravel contendrá:

- API pública;
- interfaz web;
- panel administrativo;
- autenticación;
- importadores;
- lógica de negocio;
- validación;
- acceso a base de datos.

No se crearán microservicios durante la primera etapa.

La prioridad es mantener una arquitectura simple, documentada y fácil de desplegar.

---

## 4. Capas principales

### 4.1 Presentación

Incluye:

- páginas React;
- componentes reutilizables;
- formularios;
- filtros;
- panel administrativo;
- navegación;
- visualización de entidades;
- visualización de relaciones;
- documentación visible de la API.

La interfaz se comunicará con Laravel mediante Inertia y, cuando corresponda, mediante la API REST.

---

### 4.2 API

La capa API será responsable de:

- recibir solicitudes HTTP;
- validar parámetros;
- aplicar filtros;
- paginar resultados;
- ordenar resultados;
- devolver recursos JSON;
- manejar errores de forma consistente;
- mantener el versionado de endpoints.

Las respuestas deberán usar API Resources de Laravel.

La API pública será inicialmente de solo lectura.

---

### 4.3 Aplicación

Esta capa contendrá los casos de uso del sistema.

Ejemplos:

- crear una entidad;
- actualizar una entidad;
- importar registros;
- resolver relaciones;
- normalizar nombres;
- detectar duplicados;
- asociar fuentes;
- validar datos editoriales.

La lógica de negocio no debe concentrarse en los controladores.

Cuando una operación sea compleja, deberá implementarse mediante servicios, acciones o clases especializadas.

---

### 4.4 Dominio

El dominio representa los conceptos principales del proyecto.

Conceptos iniciales:

- mitología;
- entidad;
- categoría;
- tradición;
- dominio;
- relación;
- tipo de relación;
- fuente;
- nombre alternativo;
- estado de revisión.

El dominio debe mantenerse independiente de detalles de interfaz cuando sea posible.

---

### 4.5 Persistencia

La persistencia incluirá:

- modelos Eloquent;
- migraciones;
- relaciones entre modelos;
- índices;
- restricciones;
- tablas pivote;
- seeders;
- importadores;
- consultas optimizadas.

Las relaciones mitológicas deberán almacenarse de forma estructurada.

No se guardarán padres, hijos, parejas o equivalencias como texto libre dentro de la tabla principal de entidades.

---

## 5. Módulos iniciales

La aplicación se organizará conceptualmente en los siguientes módulos:

```text
Mythologies
Entities
Categories
Domains
Traditions
Relationships
Sources
Import
Authentication
Admin
Public API
```

Cada módulo deberá concentrar su propia responsabilidad.

Los controladores deben ser pequeños y delegar la lógica a servicios, acciones o consultas especializadas.

---

## 6. API pública

La API pública será de solo lectura durante la versión 0.1.0.

Ejemplos de endpoints:

```text
GET /api/v1/entities
GET /api/v1/entities/{slug}
GET /api/v1/entities/{slug}/relationships
GET /api/v1/mythologies
GET /api/v1/categories
GET /api/v1/domains
GET /api/v1/traditions
GET /api/v1/sources
GET /api/v1/search
```

La consulta pública no requerirá autenticación en la primera versión.

Todas las rutas públicas estarán versionadas bajo:

```text
/api/v1
```

---

## 7. Panel administrativo

El panel administrativo permitirá gestionar el contenido del proyecto.

Funciones previstas:

- crear entidades;
- editar entidades;
- eliminar entidades mediante borrado lógico;
- registrar nombres alternativos;
- registrar relaciones;
- asociar fuentes;
- administrar categorías;
- administrar dominios;
- administrar tradiciones;
- revisar registros importados;
- consultar errores de importación;
- aprobar contenido.

El panel se desarrollará después de completar la API pública básica.

---

## 8. Autenticación y autorización

Las operaciones públicas de lectura no requerirán autenticación.

Las operaciones administrativas requerirán autenticación mediante Laravel Sanctum.

Roles iniciales:

- administrador;
- editor;
- revisor.

Responsabilidades generales:

### Administrador

- acceso total;
- administración de usuarios;
- configuración del sistema;
- aprobación de cambios;
- mantenimiento de catálogos.

### Editor

- crear y editar entidades;
- registrar relaciones;
- asociar fuentes;
- enviar registros a revisión.

### Revisor

- revisar contenido;
- aprobar registros;
- marcar información como disputada;
- corregir errores editoriales.

El sistema de roles podrá implementarse en una etapa posterior.

---

## 9. Convenciones de nombres

### 9.1 Código

Las clases, modelos, métodos y variables se escribirán en inglés.

Ejemplos:

```text
Entity
Mythology
Relationship
Source
ReviewStatus
```

### 9.2 Base de datos

Los nombres de tablas se escribirán en inglés y plural.

Ejemplos:

```text
mythologies
entities
relationships
sources
traditions
domains
```

### 9.3 Rutas

Las rutas de API usarán nombres en inglés.

Ejemplo:

```text
/api/v1/entities
```

### 9.4 Contenido

Los nombres visibles, resúmenes y descripciones podrán almacenarse inicialmente en español.

La internacionalización se añadirá en una etapa posterior.

---

## 10. Principios técnicos

El proyecto seguirá estos principios:

- los controladores deben ser pequeños;
- la validación debe realizarse mediante Form Requests;
- las respuestas de la API deben usar API Resources;
- las consultas públicas deben estar paginadas;
- los slugs deben ser estables;
- los importadores deben ser idempotentes;
- la API debe estar versionada;
- las relaciones mitológicas deben ser estructuradas;
- los datos históricos no deben eliminarse sin trazabilidad;
- las relaciones contradictorias deben poder coexistir;
- las consultas frecuentes deben usar índices;
- los endpoints deben evitar respuestas excesivamente grandes;
- las reglas editoriales deben separarse de la presentación.

---

## 11. Importación de datos

Los datos iniciales se importarán desde archivos JSON o CSV normalizados.

Ubicación sugerida:

```text
database/data/
```

Ejemplos:

```text
database/data/entities.json
database/data/relationships.json
database/data/sources.json
```

La importación deberá realizarse mediante un comando personalizado:

```bash
php artisan atlas:import
```

El importador deberá:

- validar los registros;
- detectar duplicados;
- crear catálogos faltantes cuando esté permitido;
- importar entidades;
- resolver relaciones;
- registrar errores;
- generar un resumen de resultados;
- poder ejecutarse varias veces sin duplicar información.

---

## 12. Manejo de errores

La API deberá devolver errores en formato JSON consistente.

Ejemplo:

```json
{
  "message": "Entity not found.",
  "error": {
    "code": "ENTITY_NOT_FOUND"
  }
}
```

Los errores deberán incluir:

- mensaje legible;
- código interno estable;
- código HTTP adecuado;
- detalles de validación cuando corresponda.

---

## 13. Rendimiento

Desde la primera versión se deberán considerar:

- paginación;
- índices en campos de búsqueda;
- carga anticipada de relaciones cuando sea necesaria;
- prevención del problema N+1;
- límites máximos de resultados;
- selección explícita de columnas;
- caché en una etapa posterior.

No se implementará optimización prematura, pero tampoco se diseñarán consultas que dependan de cargar toda la base de datos en memoria.

---

## 14. Seguridad

Medidas iniciales:

- validación de entradas;
- protección CSRF para formularios web;
- autenticación en operaciones administrativas;
- autorización por rol;
- limitación de solicitudes en la API;
- variables sensibles en `.env`;
- exclusión de `.env` del repositorio;
- uso de consultas mediante Eloquent o Query Builder;
- escape de contenido mostrado en la interfaz.

Nunca deberán incluirse credenciales reales en GitHub.

---

## 15. Estructura orientativa

```text
app/
├── Actions/
├── Console/
│   └── Commands/
├── Enums/
├── Http/
│   ├── Controllers/
│   ├── Requests/
│   └── Resources/
├── Models/
├── Services/
└── Support/

database/
├── data/
├── factories/
├── migrations/
└── seeders/

docs/

resources/
├── css/
├── js/
│   ├── Components/
│   ├── Layouts/
│   └── Pages/
└── views/

routes/
├── api.php
└── web.php

tests/
├── Feature/
└── Unit/
```

Esta estructura es orientativa y podrá evolucionar según las necesidades reales del proyecto.

---

## 16. Flujo general de una consulta

Ejemplo de consulta pública:

```text
Cliente
   ↓
Ruta API
   ↓
Controlador
   ↓
Form Request o validación
   ↓
Servicio o consulta
   ↓
Modelo Eloquent
   ↓
PostgreSQL
   ↓
API Resource
   ↓
Respuesta JSON
```

Ejemplo de creación administrativa:

```text
Usuario autenticado
   ↓
Formulario React
   ↓
Ruta administrativa
   ↓
Autorización
   ↓
Form Request
   ↓
Action o Service
   ↓
Modelo Eloquent
   ↓
Base de datos
   ↓
Respuesta o redirección
```

---

## 17. Estrategia de pruebas

Se utilizarán pruebas unitarias y de integración.

Las pruebas iniciales deberán cubrir:

- creación de modelos;
- restricciones de base de datos;
- importación de entidades;
- importación idempotente;
- filtros de la API;
- paginación;
- búsqueda;
- respuesta de entidad inexistente;
- relaciones entre entidades;
- validaciones;
- permisos administrativos.

Las pruebas de endpoints se ubicarán principalmente en:

```text
tests/Feature/
```

La lógica aislada podrá probarse en:

```text
tests/Unit/
```

---

## 18. Despliegue

La aplicación deberá poder ejecutarse:

- localmente;
- mediante servidor web tradicional;
- mediante Docker en una etapa posterior;
- en un servicio compatible con PHP y PostgreSQL.

El despliegue deberá incluir:

- instalación de dependencias;
- configuración de variables de entorno;
- migraciones;
- importación inicial;
- compilación de assets;
- configuración del servidor;
- tareas programadas cuando existan;
- almacenamiento persistente.

---

## 19. Decisiones arquitectónicas iniciales

### Decisión 1: monolito modular

Se utilizará un monolito modular porque:

- simplifica el desarrollo inicial;
- facilita el despliegue;
- reduce complejidad operativa;
- permite compartir autenticación, modelos y validaciones;
- es suficiente para la primera versión pública.

### Decisión 2: API REST

Se utilizará REST porque:

- es ampliamente compatible;
- puede consumirse desde múltiples plataformas;
- facilita la documentación;
- permite versionado claro;
- resulta adecuada para consultas estructuradas.

### Decisión 3: PostgreSQL

Se utilizará PostgreSQL porque:

- ofrece buena integridad relacional;
- soporta índices avanzados;
- facilita búsquedas y filtros;
- escala adecuadamente;
- es apropiado para relaciones complejas.

### Decisión 4: relaciones estructuradas

Las relaciones mitológicas se almacenarán en una tabla independiente.

Esto permitirá representar:

- genealogías;
- parejas;
- alianzas;
- enemistades;
- equivalencias;
- sincretismos;
- posesión;
- creación;
- muerte;
- relaciones contradictorias.

### Decisión 5: variantes coexistentes

El sistema no reemplazará una versión de un mito por otra.

Las variantes podrán coexistir mediante:

- tradiciones;
- fuentes;
- niveles de certeza;
- notas editoriales;
- estados de disputa.

---

## 20. Decisiones pendientes

Todavía deben definirse:

- herramienta de documentación OpenAPI;
- estrategia de almacenamiento de imágenes;
- sistema de historial de cambios;
- implementación de roles y permisos;
- estrategia de internacionalización;
- sistema de caché;
- límites públicos de consumo;
- generación de claves API;
- política de contribuciones externas;
- moderación de cambios;
- proceso de publicación de versiones de datos.