# Diseño de la API

## 1. Propósito

La API de Atlas Mitológico permitirá consultar de forma pública y estructurada la información almacenada en la plataforma.

Debe poder ser consumida por:

- aplicaciones web;
- aplicaciones móviles;
- proyectos educativos;
- visualizaciones;
- herramientas de investigación;
- integraciones externas;
- scripts y servicios de terceros.

La primera versión será de solo lectura.

---

## 2. URL base

### Entorno local

```text
http://localhost:8000/api/v1
```

### Entorno de producción

```text
https://dominio.com/api/v1
```

Todas las rutas públicas estarán versionadas.

---

## 3. Formato de respuesta

La API devolverá información en formato JSON.

Ejemplo:

```json
{
  "data": {
    "id": 1,
    "name": "Zeus",
    "slug": "zeus",
    "summary": "Dios griego del cielo y el trueno.",
    "mythology": {
      "name": "Griega",
      "slug": "greek"
    },
    "category": {
      "name": "Deidad",
      "slug": "deity"
    }
  }
}
```

Las respuestas individuales utilizarán la propiedad:

```text
data
```

Las colecciones paginadas incluirán:

```text
data
meta
links
```

---

## 4. Versionado

La primera versión pública será:

```text
/api/v1
```

Los cambios compatibles podrán añadirse sin cambiar la versión.

Los cambios incompatibles requerirán una nueva versión:

```text
/api/v2
```

Ejemplos de cambios incompatibles:

- eliminar un campo;
- cambiar el tipo de un campo;
- modificar la estructura principal de una respuesta;
- cambiar el significado de un parámetro;
- eliminar un endpoint.

---

## 5. Endpoints de entidades

### Listar entidades

```http
GET /api/v1/entities
```

Este endpoint devuelve una colección paginada de entidades.

### Parámetros disponibles

```text
q
mythology
category
domain
tradition
gender
review_status
review_level
is_featured
sort
page
per_page
```

### Ejemplos

```http
GET /api/v1/entities?mythology=greek
```

```http
GET /api/v1/entities?category=deity
```

```http
GET /api/v1/entities?domain=war
```

```http
GET /api/v1/entities?q=zeus
```

```http
GET /api/v1/entities?mythology=greek&category=hero&sort=name
```

---

### Consultar una entidad

```http
GET /api/v1/entities/{slug}
```

Ejemplo:

```http
GET /api/v1/entities/zeus
```

Como los slugs pueden repetirse entre mitologías, la implementación final deberá resolver uno de estos enfoques:

```text
/api/v1/entities/greek/zeus
```

o:

```text
/api/v1/entities/zeus?mythology=greek
```

La opción recomendada es:

```text
/api/v1/entities/{mythology}/{entity}
```

Ejemplo:

```http
GET /api/v1/entities/greek/zeus
```

Esto evita ambigüedades.

---

### Consultar relaciones de una entidad

```http
GET /api/v1/entities/{mythology}/{entity}/relationships
```

Ejemplo:

```http
GET /api/v1/entities/greek/zeus/relationships
```

Parámetros opcionales:

```text
type
tradition
certainty
direction
page
per_page
```

Valores posibles para `direction`:

```text
outgoing
incoming
both
```

---

### Consultar fuentes de una entidad

```http
GET /api/v1/entities/{mythology}/{entity}/sources
```

Ejemplo:

```http
GET /api/v1/entities/greek/zeus/sources
```

---

### Consultar dominios de una entidad

```http
GET /api/v1/entities/{mythology}/{entity}/domains
```

Ejemplo:

```http
GET /api/v1/entities/greek/poseidon/domains
```

---

### Consultar nombres alternativos

```http
GET /api/v1/entities/{mythology}/{entity}/names
```

Ejemplo:

```http
GET /api/v1/entities/greek/heracles/names
```

---

## 6. Endpoints de mitologías

### Listar mitologías

```http
GET /api/v1/mythologies
```

Ejemplo de respuesta:

```json
{
  "data": [
    {
      "id": 1,
      "name": "Griega",
      "slug": "greek",
      "region": "Mediterráneo oriental",
      "entities_count": 423
    }
  ]
}
```

---

### Consultar una mitología

```http
GET /api/v1/mythologies/{slug}
```

Ejemplo:

```http
GET /api/v1/mythologies/greek
```

---

### Consultar entidades de una mitología

```http
GET /api/v1/mythologies/{slug}/entities
```

Ejemplo:

```http
GET /api/v1/mythologies/norse/entities
```

Este endpoint podrá aceptar los mismos filtros que el listado general de entidades.

---

### Consultar tradiciones de una mitología

```http
GET /api/v1/mythologies/{slug}/traditions
```

Ejemplo:

```http
GET /api/v1/mythologies/egyptian/traditions
```

---

## 7. Endpoints de categorías

### Listar categorías

```http
GET /api/v1/categories
```

### Consultar una categoría

```http
GET /api/v1/categories/{slug}
```

### Consultar entidades de una categoría

```http
GET /api/v1/categories/{slug}/entities
```

Ejemplo:

```http
GET /api/v1/categories/deity/entities
```

---

## 8. Endpoints de dominios

### Listar dominios

```http
GET /api/v1/domains
```

### Consultar un dominio

```http
GET /api/v1/domains/{slug}
```

### Consultar entidades de un dominio

```http
GET /api/v1/domains/{slug}/entities
```

Ejemplo:

```http
GET /api/v1/domains/war/entities
```

---

## 9. Endpoints de tradiciones

### Listar tradiciones

```http
GET /api/v1/traditions
```

Filtros opcionales:

```text
mythology
region
```

### Consultar una tradición

```http
GET /api/v1/traditions/{mythology}/{tradition}
```

Ejemplo:

```http
GET /api/v1/traditions/greek/hesiodic
```

### Consultar entidades de una tradición

```http
GET /api/v1/traditions/{mythology}/{tradition}/entities
```

---

## 10. Endpoints de relaciones

### Listar relaciones

```http
GET /api/v1/relationships
```

Filtros disponibles:

```text
source
target
type
mythology
tradition
certainty
is_disputed
direction
page
per_page
```

Ejemplo:

```http
GET /api/v1/relationships?type=parent_of&mythology=greek
```

---

### Consultar una relación

```http
GET /api/v1/relationships/{id}
```

Ejemplo:

```http
GET /api/v1/relationships/125
```

---

### Listar tipos de relación

```http
GET /api/v1/relationship-types
```

### Consultar un tipo de relación

```http
GET /api/v1/relationship-types/{slug}
```

Ejemplo:

```http
GET /api/v1/relationship-types/parent_of
```

---

## 11. Endpoints de fuentes

### Listar fuentes

```http
GET /api/v1/sources
```

Filtros disponibles:

```text
q
author
source_type
language
sort
page
per_page
```

---

### Consultar una fuente

```http
GET /api/v1/sources/{slug}
```

Ejemplo:

```http
GET /api/v1/sources/theogony
```

---

### Consultar entidades asociadas a una fuente

```http
GET /api/v1/sources/{slug}/entities
```

Ejemplo:

```http
GET /api/v1/sources/theogony/entities
```

---

## 12. Endpoint de búsqueda

```http
GET /api/v1/search
```

Parámetro obligatorio:

```text
q
```

Ejemplo:

```http
GET /api/v1/search?q=zeus
```

La búsqueda podrá incluir:

- nombre principal;
- nombres alternativos;
- transliteraciones;
- epítetos;
- resumen;
- descripción;
- dominios;
- categorías;
- mitologías.

Filtros opcionales:

```text
type
mythology
category
limit
```

Ejemplo:

```http
GET /api/v1/search?q=sol&type=entity&mythology=egyptian
```

---

## 13. Paginación

Los endpoints de colección utilizarán paginación.

Ejemplo:

```http
GET /api/v1/entities?page=1&per_page=25
```

Respuesta:

```json
{
  "data": [],
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 39,
    "per_page": 25,
    "to": 25,
    "total": 954
  },
  "links": {
    "first": "http://localhost:8000/api/v1/entities?page=1",
    "last": "http://localhost:8000/api/v1/entities?page=39",
    "prev": null,
    "next": "http://localhost:8000/api/v1/entities?page=2"
  }
}
```

### Reglas

- `per_page` por defecto será 25.
- `per_page` mínimo será 1.
- `per_page` máximo será 100.
- valores inválidos devolverán error 422.

---

## 14. Ordenamiento

El parámetro será:

```text
sort
```

Ejemplos:

```http
GET /api/v1/entities?sort=name
```

```http
GET /api/v1/entities?sort=-name
```

```http
GET /api/v1/entities?sort=-updated_at
```

El prefijo `-` indica orden descendente.

Campos permitidos inicialmente:

```text
name
created_at
updated_at
review_level
```

No se permitirán columnas arbitrarias.

---

## 15. Inclusión de relaciones

En una etapa posterior podrá utilizarse el parámetro:

```text
include
```

Ejemplo:

```http
GET /api/v1/entities/greek/zeus?include=domains,names,relationships,sources
```

Valores permitidos:

```text
mythology
category
tradition
domains
names
relationships
sources
```

La API no devolverá todas las relaciones por defecto para evitar respuestas demasiado grandes.

---

## 16. Formato de una entidad

Ejemplo de respuesta detallada:

```json
{
  "data": {
    "id": 1,
    "name": "Zeus",
    "slug": "zeus",
    "summary": "Dios griego del cielo y el trueno.",
    "description": null,
    "gender": "male",
    "review_status": "imported",
    "review_level": 2,
    "is_featured": true,
    "mythology": {
      "name": "Griega",
      "slug": "greek"
    },
    "category": {
      "name": "Deidad",
      "slug": "deity"
    },
    "primary_tradition": {
      "name": "Hesiódica",
      "slug": "hesiodic"
    },
    "domains": [
      {
        "name": "Cielo",
        "slug": "sky"
      },
      {
        "name": "Trueno",
        "slug": "thunder"
      }
    ],
    "alternate_names": [
      {
        "name": "Ζεύς",
        "language": "grc",
        "name_type": "original"
      }
    ],
    "created_at": "2026-07-23T18:00:00Z",
    "updated_at": "2026-07-23T18:00:00Z"
  }
}
```

---

## 17. Formato de una relación

Ejemplo:

```json
{
  "data": {
    "id": 125,
    "source": {
      "name": "Crono",
      "slug": "cronus",
      "mythology": "greek"
    },
    "relationship_type": {
      "name": "Padre de",
      "slug": "parent_of",
      "inverse_slug": "child_of",
      "is_bidirectional": false
    },
    "target": {
      "name": "Zeus",
      "slug": "zeus",
      "mythology": "greek"
    },
    "tradition": {
      "name": "Hesiódica",
      "slug": "hesiodic"
    },
    "certainty": "confirmed",
    "is_disputed": false,
    "description": null,
    "source_reference": {
      "title": "Teogonía",
      "slug": "theogony"
    }
  }
}
```

---

## 18. Respuestas de error

La API deberá usar una estructura consistente.

### Recurso no encontrado

```json
{
  "message": "Entity not found.",
  "error": {
    "code": "ENTITY_NOT_FOUND"
  }
}
```

### Validación

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "per_page": [
      "The per page field must not be greater than 100."
    ]
  }
}
```

### Error de autenticación

```json
{
  "message": "Unauthenticated.",
  "error": {
    "code": "UNAUTHENTICATED"
  }
}
```

### Error de autorización

```json
{
  "message": "This action is unauthorized.",
  "error": {
    "code": "FORBIDDEN"
  }
}
```

### Límite de solicitudes

```json
{
  "message": "Too many requests.",
  "error": {
    "code": "RATE_LIMIT_EXCEEDED"
  }
}
```

---

## 19. Códigos HTTP

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
429 Too Many Requests
500 Internal Server Error
```

La API pública inicial utilizará principalmente:

```text
200
404
422
429
500
```

---

## 20. Autenticación

La consulta pública no requerirá autenticación en la versión 0.1.0.

Los endpoints administrativos utilizarán Laravel Sanctum.

Ejemplos futuros:

```http
POST /api/v1/admin/entities
PUT /api/v1/admin/entities/{id}
DELETE /api/v1/admin/entities/{id}
```

Estos endpoints requerirán:

```http
Authorization: Bearer {token}
```

---

## 21. Rate limiting

La API pública tendrá limitación de solicitudes.

Configuración inicial sugerida:

```text
60 solicitudes por minuto por dirección IP
```

Los límites podrán ajustarse según el despliegue.

En el futuro podrán existir:

```text
acceso anónimo
clientes registrados
claves API
planes de uso
```

La API seguirá siendo gratuita en su funcionalidad pública esencial.

---

## 22. Caché

La caché no será obligatoria en la primera implementación.

En etapas posteriores podrá aplicarse a:

- listado de mitologías;
- categorías;
- dominios;
- detalle de entidades populares;
- búsquedas frecuentes;
- estadísticas.

La caché no debe impedir que los cambios editoriales se reflejen correctamente.

---

## 23. Convenciones de nombres

Las claves JSON se escribirán en inglés y en formato `snake_case`.

Ejemplos:

```text
review_status
review_level
primary_tradition
alternate_names
is_disputed
created_at
```

Los valores visibles podrán estar en español.

---

## 24. Fechas

Las fechas se devolverán en formato ISO 8601.

Ejemplo:

```text
2026-07-23T18:00:00Z
```

La zona horaria de las respuestas públicas será UTC.

---

## 25. Valores nulos

Los campos opcionales podrán devolver:

```json
null
```

No deberán omitirse de forma arbitraria si forman parte estable del contrato de la API.

Ejemplo:

```json
{
  "description": null,
  "primary_tradition": null
}
```

---

## 26. Filtros combinados

Los filtros podrán combinarse.

Ejemplo:

```http
GET /api/v1/entities?mythology=greek&category=deity&domain=war&sort=name
```

Todos los filtros aplicarán semántica `AND`, salvo que un endpoint indique lo contrario.

---

## 27. Parámetros desconocidos

La primera versión podrá ignorar parámetros desconocidos o devolver error.

La opción recomendada es rechazarlos con un error 422 en endpoints estrictos.

Esto ayuda a detectar errores de integración.

---

## 28. Documentación OpenAPI

La API deberá documentarse posteriormente mediante OpenAPI.

La documentación debe incluir:

- endpoints;
- parámetros;
- ejemplos;
- respuestas;
- errores;
- esquemas;
- autenticación;
- límites.

La herramienta concreta queda pendiente de decisión.

Posibles opciones:

```text
Scribe
L5-Swagger
documentación OpenAPI manual
```

---

## 29. Compatibilidad

La API deberá mantener compatibilidad dentro de cada versión mayor.

Ejemplo:

```text
v1.0
v1.1
v1.2
```

Los clientes que consuman `/api/v1` no deberían romperse por cambios menores.

---

## 30. Endpoints administrativos futuros

Los siguientes endpoints se implementarán en versiones posteriores:

```http
POST /api/v1/admin/entities
PUT /api/v1/admin/entities/{id}
DELETE /api/v1/admin/entities/{id}

POST /api/v1/admin/relationships
PUT /api/v1/admin/relationships/{id}
DELETE /api/v1/admin/relationships/{id}

POST /api/v1/admin/sources
PUT /api/v1/admin/sources/{id}
DELETE /api/v1/admin/sources/{id}
```

También podrán añadirse:

```text
review
approve
reject
restore
import
export
```

---

## 31. Pruebas mínimas de la API

La implementación deberá incluir pruebas para:

- listado de entidades;
- detalle de una entidad;
- búsqueda por nombre;
- filtrado por mitología;
- filtrado por categoría;
- filtrado por dominio;
- paginación;
- límite de `per_page`;
- ordenamiento;
- entidad inexistente;
- formato de errores;
- relaciones entrantes y salientes;
- consulta de fuentes;
- consulta de mitologías;
- respuesta sin autenticación en rutas administrativas.

---

## 32. Decisiones pendientes

Todavía deben definirse:

- formato final de rutas para entidades;
- herramienta OpenAPI;
- estrategia de claves API;
- límites definitivos;
- campos permitidos en `include`;
- búsqueda de texto completo;
- política CORS;
- estrategia de caché;
- endpoints de estadísticas;
- exportación JSON y CSV;
- soporte para GraphQL;
- política de compatibilidad entre versiones.