# Modelo de base de datos

## 1. Propósito

La base de datos de Atlas Mitológico debe representar información mitológica de forma estructurada, flexible y trazable.

El modelo debe permitir:

- almacenar múltiples mitologías;
- registrar miles de entidades;
- conservar nombres alternativos;
- representar genealogías;
- registrar relaciones contradictorias;
- asociar datos con tradiciones específicas;
- documentar fuentes;
- manejar distintos niveles de certeza;
- registrar equivalencias culturales;
- representar sincretismos;
- ampliar el sistema sin rediseñar la base principal.

El modelo no debe asumir que existe una única versión correcta de cada mito.

---

## 2. Principios del modelo

La base de datos seguirá estos principios:

- una entidad representa un concepto mitológico identificable;
- una entidad puede tener varios nombres;
- una entidad puede pertenecer a una mitología principal;
- una entidad puede relacionarse con muchas otras entidades;
- una relación puede depender de una tradición concreta;
- distintas versiones contradictorias pueden coexistir;
- una afirmación puede estar respaldada por una o varias fuentes;
- las relaciones no deben almacenarse como texto libre;
- las equivalencias culturales no deben fusionar entidades distintas;
- la información histórica no debe eliminarse sin trazabilidad.

---

## 3. Diagrama conceptual

```text
mythologies
    │
    ├── traditions
    │
    └── entities
          ├── entity_names
          ├── entity_domain ── domains
          ├── entity_source ── sources
          └── relationships
                 ├── relationship_types
                 ├── traditions
                 └── sources
```

Las tablas principales serán:

```text
mythologies
categories
traditions
entities
entity_names
domains
entity_domain
relationship_types
relationships
sources
entity_source
```

---

## 4. Tabla `mythologies`

Representa una mitología, tradición cultural general o sistema religioso.

Ejemplos:

```text
Griega
Nórdica
Egipcia
Romana
Celta
Mesopotámica
Japonesa
Maya
Mexica
```

### Campos propuestos

```text
id
name
slug
description
region
historical_period
is_active
created_at
updated_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `name` | varchar | Nombre visible |
| `slug` | varchar | Identificador legible para URLs |
| `description` | text nullable | Descripción general |
| `region` | varchar nullable | Región geográfica principal |
| `historical_period` | varchar nullable | Periodo histórico aproximado |
| `is_active` | boolean | Indica si se muestra públicamente |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |

### Restricciones

- `slug` debe ser único.
- `name` debe ser obligatorio.
- `is_active` debe ser verdadero por defecto.

### Ejemplo

```text
name: Griega
slug: greek
region: Mediterráneo oriental
historical_period: Antigüedad griega
```

---

## 5. Tabla `categories`

Representa la clasificación principal de una entidad.

Ejemplos:

```text
deity
primordial
titan
hero
demigod
creature
spirit
giant
mortal
king
queen
place
object
event
group
```

### Campos propuestos

```text
id
name
slug
description
created_at
updated_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `name` | varchar | Nombre visible |
| `slug` | varchar | Nombre técnico único |
| `description` | text nullable | Explicación de la categoría |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |

### Restricciones

- `slug` debe ser único.
- `name` debe ser obligatorio.

---

## 6. Tabla `traditions`

Representa una versión, escuela, región, autor o corriente específica dentro de una mitología.

Ejemplos:

```text
Hesiódica
Homérica
Órfica
Heliopolitana
Menfita
Tebana
Edda poética
Edda prosaica
```

### Campos propuestos

```text
id
mythology_id
name
slug
description
region
historical_period
created_at
updated_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `mythology_id` | foreign key | Mitología a la que pertenece |
| `name` | varchar | Nombre visible |
| `slug` | varchar | Identificador técnico |
| `description` | text nullable | Descripción de la tradición |
| `region` | varchar nullable | Región asociada |
| `historical_period` | varchar nullable | Periodo aproximado |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |

### Restricciones

- `mythology_id` debe existir en `mythologies`.
- La combinación `mythology_id + slug` debe ser única.
- Una tradición no debe pertenecer a más de una mitología principal.

---

## 7. Tabla `entities`

Es la tabla principal del sistema.

Una entidad puede representar:

- una deidad;
- una criatura;
- un héroe;
- un mortal;
- un lugar;
- un objeto;
- un evento;
- un grupo colectivo;
- una personificación;
- una entidad sincrética.

### Campos propuestos

```text
id
mythology_id
category_id
primary_tradition_id
name
slug
summary
description
gender
entity_type
review_status
review_level
is_featured
is_active
created_at
updated_at
deleted_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `mythology_id` | foreign key | Mitología principal |
| `category_id` | foreign key | Categoría principal |
| `primary_tradition_id` | foreign key nullable | Tradición principal asociada |
| `name` | varchar | Nombre principal |
| `slug` | varchar | Identificador para URLs |
| `summary` | text nullable | Descripción breve |
| `description` | long text nullable | Descripción extensa |
| `gender` | varchar nullable | Género o clasificación equivalente |
| `entity_type` | varchar nullable | Tipo técnico adicional |
| `review_status` | varchar | Estado editorial |
| `review_level` | small integer | Nivel de completitud |
| `is_featured` | boolean | Marca para contenido destacado |
| `is_active` | boolean | Control de publicación |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |
| `deleted_at` | timestamp nullable | Borrado lógico |

### Restricciones

- `mythology_id` debe ser obligatorio.
- `category_id` debe ser obligatorio.
- `name` debe ser obligatorio.
- La combinación `mythology_id + slug` debe ser única.
- `review_level` debe estar entre 0 y 5.
- `review_status` debe usar valores controlados.
- La eliminación debe realizarse mediante `soft deletes`.

### Ejemplo

```text
name: Zeus
slug: zeus
mythology: Griega
category: deity
gender: male
review_status: imported
review_level: 2
```

---

## 8. Estados editoriales

El campo `review_status` utilizará inicialmente los siguientes valores:

```text
draft
imported
under_review
reviewed
verified
disputed
```

### Significado

| Estado | Descripción |
|---|---|
| `draft` | Registro incompleto creado manualmente |
| `imported` | Registro generado mediante importación |
| `under_review` | Registro actualmente en revisión |
| `reviewed` | Registro revisado en contenido y estructura |
| `verified` | Registro respaldado por fuentes suficientes |
| `disputed` | Registro con interpretación controvertida |

---

## 9. Niveles de revisión

El campo `review_level` indicará el grado de completitud.

```text
0 - solo nombre
1 - clasificación básica
2 - genealogía básica
3 - dominios y relaciones
4 - fuentes asociadas
5 - revisión editorial completa
```

El nivel no sustituye al estado editorial.

Una entidad puede estar en estado `under_review` y tener nivel 4.

---

## 10. Tabla `entity_names`

Almacena nombres alternativos, transliteraciones, epítetos y variantes lingüísticas.

### Campos propuestos

```text
id
entity_id
name
language
script
name_type
is_preferred
notes
created_at
updated_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `entity_id` | foreign key | Entidad relacionada |
| `name` | varchar | Nombre alternativo |
| `language` | varchar nullable | Idioma |
| `script` | varchar nullable | Sistema de escritura |
| `name_type` | varchar | Tipo de nombre |
| `is_preferred` | boolean | Indica si es preferido en ese idioma |
| `notes` | text nullable | Observaciones |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |

### Tipos de nombre

```text
alternate
original
transliteration
epithet
romanized
localized
modern
former
```

### Reglas

- No deben crearse entidades nuevas solo por una variante ortográfica.
- Un epíteto no debe convertirse automáticamente en entidad.
- La combinación `entity_id + name + language + name_type` debe evitar duplicados.

### Ejemplo

```text
entity: Heracles
name: Hércules
language: es
name_type: localized
```

---

## 11. Tabla `domains`

Representa áreas de influencia, funciones o conceptos asociados a una entidad.

Ejemplos:

```text
war
wisdom
sea
death
love
fertility
sun
moon
thunder
magic
justice
hunting
agriculture
underworld
```

### Campos propuestos

```text
id
name
slug
description
created_at
updated_at
```

### Restricciones

- `slug` debe ser único.
- `name` debe ser obligatorio.

---

## 12. Tabla pivote `entity_domain`

Relaciona entidades con dominios.

Una entidad puede tener muchos dominios y un dominio puede pertenecer a muchas entidades.

### Campos propuestos

```text
id
entity_id
domain_id
tradition_id
source_id
notes
created_at
updated_at
```

### Descripción

| Campo | Descripción |
|---|---|
| `entity_id` | Entidad asociada |
| `domain_id` | Dominio relacionado |
| `tradition_id` | Tradición específica, si aplica |
| `source_id` | Fuente que respalda la asociación |
| `notes` | Aclaraciones editoriales |

### Reglas

- `entity_id` y `domain_id` son obligatorios.
- La misma asociación puede coexistir si pertenece a tradiciones distintas.
- Deben evitarse duplicados exactos.

### Ejemplo

```text
entity: Poseidón
domain: sea
tradition: Hesiódica
```

---

## 13. Tabla `relationship_types`

Define los tipos de relaciones disponibles.

### Campos propuestos

```text
id
name
slug
inverse_name
inverse_slug
is_bidirectional
category
description
created_at
updated_at
```

### Ejemplos

```text
parent_of
child_of
partner_of
sibling_of
creator_of
created_by
enemy_of
ally_of
equivalent_to
syncretized_with
owns
owned_by
rules
ruled_by
killed
killed_by
member_of
has_member
```

### Descripción de campos

| Campo | Descripción |
|---|---|
| `name` | Nombre visible |
| `slug` | Identificador técnico |
| `inverse_name` | Nombre visible de la relación inversa |
| `inverse_slug` | Identificador inverso |
| `is_bidirectional` | Indica si funciona igual en ambos sentidos |
| `category` | Clasificación general |
| `description` | Explicación del tipo de relación |

### Categorías sugeridas

```text
genealogy
romantic
social
conflict
ownership
creation
authority
equivalence
syncretism
membership
event
```

### Ejemplo

```text
name: Padre de
slug: parent_of
inverse_name: Hijo de
inverse_slug: child_of
is_bidirectional: false
category: genealogy
```

Para una relación simétrica:

```text
name: Hermano de
slug: sibling_of
inverse_name: Hermano de
inverse_slug: sibling_of
is_bidirectional: true
category: genealogy
```

---

## 14. Tabla `relationships`

Representa relaciones estructuradas entre entidades.

### Campos propuestos

```text
id
source_entity_id
target_entity_id
relationship_type_id
tradition_id
source_id
description
certainty
is_disputed
is_primary
created_at
updated_at
deleted_at
```

### Descripción de campos

| Campo | Tipo sugerido | Descripción |
|---|---|---|
| `id` | bigint | Identificador principal |
| `source_entity_id` | foreign key | Entidad de origen |
| `target_entity_id` | foreign key | Entidad de destino |
| `relationship_type_id` | foreign key | Tipo de relación |
| `tradition_id` | foreign key nullable | Tradición específica |
| `source_id` | foreign key nullable | Fuente de respaldo |
| `description` | text nullable | Explicación adicional |
| `certainty` | varchar | Nivel de certeza |
| `is_disputed` | boolean | Marca de controversia |
| `is_primary` | boolean | Relación principal |
| `created_at` | timestamp | Fecha de creación |
| `updated_at` | timestamp | Fecha de actualización |
| `deleted_at` | timestamp nullable | Borrado lógico |

### Ejemplo

```text
source_entity: Crono
relationship_type: parent_of
target_entity: Zeus
tradition: Hesiódica
certainty: confirmed
```

### Reglas

- La entidad de origen debe existir.
- La entidad de destino debe existir.
- El tipo de relación debe existir.
- Deben evitarse relaciones duplicadas exactas.
- Una relación puede repetirse si cambia la tradición o la fuente.
- Se utilizarán `soft deletes`.
- Una entidad no debe relacionarse consigo misma salvo casos documentados.
- No se almacenará automáticamente la relación inversa como otro registro, salvo que la implementación lo requiera.

---

## 15. Niveles de certeza

El campo `certainty` utilizará inicialmente estos valores:

```text
confirmed
probable
possible
disputed
modern_interpretation
unknown
```

### Significado

| Valor | Descripción |
|---|---|
| `confirmed` | Respaldado claramente por la fuente registrada |
| `probable` | Interpretación ampliamente aceptada |
| `possible` | Asociación plausible, pero no concluyente |
| `disputed` | Existen desacuerdos importantes |
| `modern_interpretation` | Interpretación moderna o posterior |
| `unknown` | No se ha evaluado la certeza |

---

## 16. Relaciones inversas

Al consultar una relación, el sistema debe poder interpretar su dirección inversa.

Ejemplo almacenado:

```text
Crono -> parent_of -> Zeus
```

Consulta desde Zeus:

```text
Zeus -> child_of -> Crono
```

La relación inversa se determinará mediante `relationship_types.inverse_slug`.

Esto evita almacenar dos filas para una sola afirmación.

Las relaciones bidireccionales tendrán el mismo tipo en ambos sentidos.

Ejemplo:

```text
Isis -> partner_of -> Osiris
Osiris -> partner_of -> Isis
```

---

## 17. Tabla `sources`

Representa fuentes primarias, estudios académicos, inscripciones o referencias modernas.

### Campos propuestos

```text
id
title
slug
author
source_type
publication_year
original_period
language
publisher
citation
url
isbn
notes
created_at
updated_at
```

### Tipos de fuente

```text
classical_text
religious_text
inscription
archaeological
oral_tradition
academic_book
academic_article
reference_book
modern_reference
website
```

### Descripción de campos

| Campo | Descripción |
|---|---|
| `title` | Título de la obra |
| `slug` | Identificador único |
| `author` | Autor o atribución |
| `source_type` | Tipo de fuente |
| `publication_year` | Año de publicación moderna |
| `original_period` | Periodo original aproximado |
| `language` | Idioma |
| `publisher` | Editorial |
| `citation` | Referencia bibliográfica completa |
| `url` | Enlace público, cuando exista |
| `isbn` | ISBN para publicaciones |
| `notes` | Observaciones editoriales |

### Reglas

- No se debe depender de la URL como identificador único.
- Deben evitarse duplicados por título, autor y edición.
- Las fuentes antiguas pueden no tener autor conocido.
- `slug` debe ser único.

---

## 18. Tabla pivote `entity_source`

Relaciona entidades con fuentes.

### Campos propuestos

```text
id
entity_id
source_id
tradition_id
citation_location
notes
reliability
created_at
updated_at
```

### Descripción

| Campo | Descripción |
|---|---|
| `entity_id` | Entidad documentada |
| `source_id` | Fuente relacionada |
| `tradition_id` | Tradición, si aplica |
| `citation_location` | Libro, capítulo, línea, página o sección |
| `notes` | Comentarios editoriales |
| `reliability` | Evaluación interna de confiabilidad |

### Valores sugeridos para `reliability`

```text
primary
high
medium
low
reference_only
```

---

## 19. Integridad referencial

Las claves foráneas deberán seguir estas reglas generales:

### Eliminación restringida

Se utilizará eliminación restringida cuando borrar el registro causaría pérdida de integridad.

Ejemplos:

```text
entities.mythology_id
entities.category_id
relationships.relationship_type_id
```

### Nulos permitidos

Algunas referencias opcionales podrán convertirse en nulas.

Ejemplos:

```text
entities.primary_tradition_id
relationships.tradition_id
relationships.source_id
entity_domain.source_id
```

### Eliminación en cascada

Podrá usarse en tablas dependientes que no tengan sentido sin su entidad principal.

Ejemplos:

```text
entity_names
entity_domain
entity_source
```

La decisión final deberá reflejarse explícitamente en las migraciones.

---

## 20. Restricciones de unicidad

Se recomiendan las siguientes restricciones:

```text
mythologies.slug
categories.slug
domains.slug
sources.slug
relationship_types.slug
```

Restricciones compuestas:

```text
traditions: mythology_id + slug
entities: mythology_id + slug
entity_names: entity_id + name + language + name_type
entity_domain: entity_id + domain_id + tradition_id + source_id
relationships:
source_entity_id +
target_entity_id +
relationship_type_id +
tradition_id +
source_id
```

Las columnas opcionales deben evaluarse cuidadosamente porque PostgreSQL permite múltiples valores nulos en restricciones únicas.

Cuando sea necesario, la detección de duplicados también deberá realizarse desde la aplicación.

---

## 21. Índices recomendados

### Entidades

```text
entities.name
entities.slug
entities.mythology_id
entities.category_id
entities.primary_tradition_id
entities.review_status
entities.review_level
entities.is_active
```

### Nombres alternativos

```text
entity_names.name
entity_names.entity_id
entity_names.language
```

### Relaciones

```text
relationships.source_entity_id
relationships.target_entity_id
relationships.relationship_type_id
relationships.tradition_id
relationships.certainty
relationships.is_disputed
```

### Fuentes

```text
sources.title
sources.slug
sources.author
sources.source_type
```

### Dominios

```text
entity_domain.entity_id
entity_domain.domain_id
domains.slug
```

En una etapa posterior podrá añadirse búsqueda de texto completo mediante PostgreSQL.

---

## 22. Borrado lógico

Se utilizarán `soft deletes` inicialmente en:

```text
entities
relationships
```

Esto permitirá:

- recuperar registros eliminados;
- conservar historial;
- evitar pérdida accidental;
- mantener relaciones editoriales;
- auditar cambios.

No se permitirá el borrado definitivo desde el panel administrativo normal.

---

## 23. Slugs

Los slugs se utilizarán en URLs públicas.

Ejemplos:

```text
/api/v1/entities/zeus
/api/v1/mythologies/greek
/api/v1/domains/war
```

### Reglas

- deben escribirse en minúsculas;
- deben usar guiones;
- no deben contener caracteres especiales;
- deben ser estables;
- no deben cambiar automáticamente cuando cambie el nombre visible;
- deben ser únicos dentro del alcance correspondiente.

Ejemplo:

```text
Nombre: Quetzalcóatl
Slug: quetzalcoatl
```

---

## 24. Idioma del contenido

La primera versión almacenará principalmente contenido en español.

Los campos iniciales serán:

```text
name
summary
description
```

En una etapa posterior podrá añadirse internacionalización mediante tablas como:

```text
entity_translations
mythology_translations
category_translations
domain_translations
```

No se añadirá esa complejidad en la versión 0.1.0.

---

## 25. Entidades que pertenecen a varias mitologías

Una entidad puede estar vinculada históricamente con varias culturas.

Sin embargo, en la primera versión cada entidad tendrá una mitología principal mediante:

```text
entities.mythology_id
```

Las conexiones con otras mitologías se representarán mediante relaciones como:

```text
equivalent_to
syncretized_with
derived_from
influenced_by
```

Ejemplo:

```text
Zeus -> equivalent_to -> Júpiter
Amón -> syncretized_with -> Ra
```

No deben fusionarse entidades de culturas diferentes únicamente por compartir atributos.

---

## 26. Entidades sincréticas

Una entidad sincrética puede tener un registro propio si cuenta con identidad histórica independiente.

Ejemplos:

```text
Amón-Ra
Serapis
Hermanubis
```

La entidad se relacionará con sus componentes.

Ejemplo:

```text
Amón-Ra -> syncretized_with -> Amón
Amón-Ra -> syncretized_with -> Ra
```

---

## 27. Grupos colectivos

Los grupos mitológicos podrán registrarse como entidades.

Ejemplos:

```text
Musas
Nereidas
Valquirias
Einherjar
Cuatro Hijos de Horus
```

Los miembros individuales podrán relacionarse mediante:

```text
member_of
has_member
```

Esto permitirá consultar tanto el grupo como sus integrantes.

---

## 28. Lugares, objetos y eventos

Los lugares, objetos y eventos también se almacenarán en `entities`.

La categoría indicará su naturaleza.

Ejemplos:

```text
Monte Olimpo
Mjölnir
Ragnarök
Inframundo
Libro de Thot
Guerra de Troya
```

Ventajas:

- reutilización del sistema de relaciones;
- búsqueda unificada;
- vínculos entre personajes y objetos;
- vínculos entre personajes y lugares;
- participación en eventos.

En el futuro podrán añadirse tablas especializadas si estos tipos requieren atributos propios.

---

## 29. Atributos específicos por tipo

No se añadirán inicialmente columnas específicas como:

```text
weapon_material
place_coordinates
event_start_date
creature_size
```

Estos atributos pueden no aplicar a la mayoría de las entidades.

En una etapa posterior se evaluará:

- tablas especializadas;
- atributos JSON controlados;
- sistema de propiedades extensibles.

La primera versión debe mantener el modelo simple y relacional.

---

## 30. Importación inicial

Los 954 registros iniciales se importarán desde archivos normalizados.

Ubicación sugerida:

```text
database/data/entities.json
database/data/relationships.json
database/data/sources.json
```

La importación deberá:

- resolver mitologías;
- resolver categorías;
- crear slugs;
- detectar nombres duplicados;
- validar campos obligatorios;
- insertar o actualizar registros;
- registrar errores;
- generar un resumen;
- ser idempotente.

No se deberán crear seeders PHP con cientos de registros escritos manualmente.

---

## 31. Ejemplo de entidad en JSON

```json
{
  "external_id": "greek-zeus",
  "name": "Zeus",
  "slug": "zeus",
  "mythology": "greek",
  "category": "deity",
  "summary": "Dios griego del cielo y el trueno.",
  "gender": "male",
  "review_status": "imported",
  "review_level": 2,
  "alternate_names": [
    {
      "name": "Ζεύς",
      "language": "grc",
      "script": "Greek",
      "name_type": "original"
    }
  ],
  "domains": [
    "sky",
    "thunder",
    "kingship"
  ]
}
```

---

## 32. Ejemplo de relación en JSON

```json
{
  "source": "greek-cronus",
  "relationship_type": "parent_of",
  "target": "greek-zeus",
  "tradition": "hesiodic",
  "certainty": "confirmed",
  "source_reference": "theogony",
  "description": null
}
```

---

## 33. Ejemplo de fuente en JSON

```json
{
  "title": "Teogonía",
  "slug": "theogony",
  "author": "Hesíodo",
  "source_type": "classical_text",
  "original_period": "Siglos VIII-VII a. C.",
  "language": "Griego antiguo",
  "citation": "Hesíodo, Teogonía"
}
```

---

## 34. Datos de auditoría

En una etapa posterior se podrán agregar campos como:

```text
created_by
updated_by
reviewed_by
verified_by
reviewed_at
verified_at
```

También podrá añadirse una tabla:

```text
change_logs
```

Esta tabla registraría:

- usuario;
- modelo modificado;
- registro;
- valores anteriores;
- valores nuevos;
- fecha;
- motivo del cambio.

No es obligatorio para la versión 0.1.0, pero debe considerarse en el diseño del panel editorial.

---

## 35. Tablas futuras

Posibles tablas para etapas posteriores:

```text
entity_translations
mythology_translations
media
entity_media
change_logs
user_contributions
contribution_reviews
tags
entity_tag
events
event_participants
locations
entity_attributes
api_clients
api_usage_logs
```

Estas tablas no deben crearse hasta que exista una necesidad concreta.

---

## 36. Orden recomendado de migraciones

```text
001_create_mythologies_table
002_create_categories_table
003_create_traditions_table
004_create_domains_table
005_create_entities_table
006_create_entity_names_table
007_create_sources_table
008_create_relationship_types_table
009_create_relationships_table
010_create_entity_domain_table
011_create_entity_source_table
```

Las migraciones deben respetar el orden de dependencias entre claves foráneas.

---

## 37. Relaciones Eloquent previstas

### Mythology

```text
hasMany Traditions
hasMany Entities
```

### Category

```text
hasMany Entities
```

### Tradition

```text
belongsTo Mythology
hasMany Entities
hasMany Relationships
```

### Entity

```text
belongsTo Mythology
belongsTo Category
belongsTo PrimaryTradition
hasMany EntityNames
belongsToMany Domains
belongsToMany Sources
hasMany OutgoingRelationships
hasMany IncomingRelationships
```

### Relationship

```text
belongsTo SourceEntity
belongsTo TargetEntity
belongsTo RelationshipType
belongsTo Tradition
belongsTo Source
```

### Source

```text
belongsToMany Entities
hasMany Relationships
```

---

## 38. Decisiones pendientes

Todavía deben definirse:

- implementación exacta de enums;
- uso de enums PHP o tablas de catálogo;
- búsqueda de texto completo;
- estrategia de traducciones;
- estructura para imágenes;
- sistema de auditoría;
- manejo de entidades compartidas entre culturas;
- política de actualización de slugs;
- estrategia de relaciones inversas;
- sistema de atributos específicos;
- reglas definitivas de eliminación en cascada.