# Guía editorial

## 1. Propósito

Esta guía define las reglas para registrar, revisar y publicar información en Atlas Mitológico.

El objetivo es mantener una base de datos:

- consistente;
- verificable;
- neutral;
- estructurada;
- ampliable;
- respetuosa con las distintas tradiciones;
- útil para consulta humana y consumo mediante API.

Atlas Mitológico no pretende establecer una única versión correcta de cada mito.

Cuando existan versiones distintas, contradictorias o regionales, deberán conservarse y documentarse por separado.

---

## 2. Principios editoriales

Todo contenido deberá seguir estos principios:

- no presentar una variante como universal;
- distinguir entre fuente primaria e interpretación moderna;
- registrar nombres alternativos sin crear duplicados innecesarios;
- mantener separadas las entidades de culturas distintas;
- documentar contradicciones;
- asociar relaciones con tradiciones y fuentes cuando sea posible;
- evitar afirmaciones absolutas;
- utilizar lenguaje neutral;
- conservar trazabilidad editorial;
- diferenciar mito histórico de adaptación contemporánea.

---

## 3. Qué se considera una entidad

Una entidad representa un concepto mitológico identificable.

Puede pertenecer a alguna de estas categorías:

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
personification
syncretic_entity
```

Ejemplos:

```text
Zeus
Odín
Ra
Medusa
Aquiles
Mjölnir
Ragnarök
Monte Olimpo
Musas
Amón-Ra
```

Una entidad debe tener identidad suficiente para poder ser consultada y relacionada con otros registros.

---

## 4. Criterios para crear una entidad

Antes de crear una entidad debe verificarse que:

- no exista ya con otro nombre;
- no sea únicamente una diferencia ortográfica;
- no sea solo una transliteración;
- no sea un epíteto sin identidad independiente;
- no sea una traducción moderna;
- no sea únicamente un título;
- exista dentro de una tradición mitológica documentada;
- pueda clasificarse dentro del modelo de datos.

Debe buscarse previamente por:

```text
nombre principal
nombres alternativos
transliteraciones
epítetos
nombre original
equivalentes culturales
formas latinizadas
```

---

## 5. Nombre principal

Cada entidad tendrá un nombre principal.

El nombre principal debe ser:

- reconocible;
- ampliamente utilizado;
- consistente con el idioma principal del proyecto;
- suficientemente específico;
- distinto de otras entidades dentro de la misma mitología.

Ejemplo:

```text
Nombre principal: Heracles
```

No se recomienda usar como nombre principal:

```text
El gran dios del trueno
El dios solar
La gran madre
```

Esos términos podrán registrarse como títulos, epítetos o descripciones.

---

## 6. Nombres alternativos

Los nombres alternativos deben almacenarse en `entity_names`.

Tipos iniciales:

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

Ejemplo:

```text
Entidad: Heracles
Nombre alternativo: Hércules
Tipo: localized
Idioma: es
```

Otro ejemplo:

```text
Entidad: Zeus
Nombre alternativo: Ζεύς
Tipo: original
Idioma: grc
Sistema de escritura: Greek
```

No deben crearse dos entidades distintas únicamente por diferencias de transliteración.

---

## 7. Epítetos

Un epíteto es un título o forma de invocación asociada con una entidad.

Ejemplos:

```text
Atenea Partenos
Zeus Olímpico
Apolo Febo
Afrodita Urania
```

Un epíteto debe registrarse como nombre alternativo cuando:

- representa un título;
- describe una función;
- identifica una advocación;
- no cuenta con identidad narrativa propia.

Un epíteto podrá convertirse en entidad separada únicamente cuando exista evidencia de:

- culto independiente;
- genealogía diferente;
- iconografía propia;
- identidad local claramente diferenciada;
- tradición narrativa autónoma;
- sincretismo reconocido.

---

## 8. Transliteraciones

Las transliteraciones deben conservarse cuando aporten valor lingüístico o histórico.

Ejemplo:

```text
Nombre original: Ἡρακλῆς
Transliteración: Hēraklēs
Forma localizada: Heracles
Forma latinizada: Hercules
```

Las transliteraciones deben incluir, cuando sea posible:

```text
idioma
sistema de escritura
tipo de nombre
notas
```

No deben usarse transliteraciones diferentes como entidades independientes.

---

## 9. Categorías

Cada entidad tendrá una categoría principal.

La categoría debe representar su naturaleza, no únicamente su función.

Ejemplo correcto:

```text
Ares
Categoría: deity
Dominio: war
```

Ejemplo incorrecto:

```text
Ares
Categoría: war
```

`war` es un dominio, no una categoría.

Otro ejemplo:

```text
Medusa
Categoría: creature
```

No debe clasificarse automáticamente como `deity` únicamente porque tenga origen divino.

---

## 10. Dominios

Los dominios representan funciones, áreas de influencia o conceptos asociados.

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
healing
prophecy
```

Una entidad puede tener varios dominios.

Ejemplo:

```text
Entidad: Apolo

Dominios:
- prophecy
- music
- healing
- plague
- archery
```

Los dominios deben asociarse con una tradición o fuente cuando existan diferencias importantes.

---

## 11. Mitología principal

Cada entidad tendrá una mitología principal.

Ejemplo:

```text
Zeus -> Greek
Jupiter -> Roman
Odin -> Norse
Ra -> Egyptian
```

Aunque dos entidades sean equivalentes culturales, deben permanecer separadas.

Ejemplo:

```text
Zeus != Jupiter
Ares != Mars
Aphrodite != Venus
```

Estas asociaciones deben representarse mediante relaciones.

---

## 12. Equivalencias culturales

Una equivalencia cultural indica similitud funcional, histórica o interpretativa entre entidades de tradiciones diferentes.

Tipo de relación:

```text
equivalent_to
```

Ejemplo:

```text
Zeus -> equivalent_to -> Jupiter
```

La equivalencia no implica que ambas entidades sean idénticas.

No deben fusionarse sus:

- genealogías;
- narraciones;
- atributos;
- cultos;
- nombres;
- fuentes;
- relaciones.

---

## 13. Sincretismo

El sincretismo representa una combinación histórica, religiosa o cultural entre entidades.

Tipo de relación:

```text
syncretized_with
```

Ejemplos:

```text
Amón-Ra
Serapis
Hermanubis
Zeus-Amón
```

Una entidad sincrética puede tener un registro independiente cuando exista:

- culto propio;
- identidad histórica;
- iconografía particular;
- documentación suficiente;
- función distinta de sus componentes.

Ejemplo:

```text
Amón-Ra -> syncretized_with -> Amón
Amón-Ra -> syncretized_with -> Ra
```

---

## 14. Tradiciones

Una tradición representa una versión regional, literaria, religiosa o histórica dentro de una mitología.

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

La tradición debe registrarse cuando influya en:

- genealogía;
- origen;
- atributos;
- dominios;
- relaciones;
- identidad;
- interpretación;
- desenlace narrativo.

No debe crearse una tradición distinta únicamente por una diferencia menor de traducción.

---

## 15. Variantes mitológicas

Las variantes deben coexistir.

Ejemplo:

```text
Tradición A:
Entidad X es hija de A.

Tradición B:
Entidad X es hija de B.
```

Ambas relaciones deben registrarse.

Cada variante deberá incluir, cuando sea posible:

```text
tradición
fuente
certeza
nota editorial
```

No debe eliminarse una variante únicamente porque contradiga a otra.

---

## 16. Genealogías

Las genealogías deben almacenarse mediante relaciones estructuradas.

Ejemplo:

```text
Crono -> parent_of -> Zeus
```

No deben almacenarse así:

```text
Padres: Crono y Rea
```

dentro de una columna de texto como única fuente de información.

La descripción narrativa puede mencionar la genealogía, pero la relación debe existir también en la tabla `relationships`.

---

## 17. Relaciones

Toda relación debe incluir:

- entidad de origen;
- entidad de destino;
- tipo de relación;
- tradición cuando aplique;
- fuente cuando exista;
- nivel de certeza;
- nota editorial cuando sea necesaria.

Ejemplo:

```text
source: Crono
relationship_type: parent_of
target: Zeus
tradition: Hesiodic
certainty: confirmed
source_reference: Theogony
```

---

## 18. Tipos de relaciones

Tipos iniciales:

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
associated_with
protector_of
worshipped_at
participated_in
```

No debe crearse un nuevo tipo de relación cuando ya exista uno equivalente.

Antes de añadir un tipo nuevo debe verificarse:

- si ya existe;
- si puede expresarse mediante una relación existente;
- si requiere relación inversa;
- si es bidireccional;
- a qué categoría pertenece.

---

## 19. Relaciones bidireccionales

Algunas relaciones funcionan igual en ambos sentidos.

Ejemplos:

```text
sibling_of
partner_of
ally_of
enemy_of
equivalent_to
syncretized_with
associated_with
```

Ejemplo:

```text
Isis -> partner_of -> Osiris
Osiris -> partner_of -> Isis
```

La aplicación podrá interpretar la relación bidireccional sin almacenar dos filas idénticas.

---

## 20. Relaciones inversas

Las relaciones direccionales deben tener una forma inversa.

Ejemplos:

```text
parent_of <-> child_of
creator_of <-> created_by
owns <-> owned_by
rules <-> ruled_by
killed <-> killed_by
member_of <-> has_member
```

Una relación almacenada como:

```text
Crono -> parent_of -> Zeus
```

debe poder consultarse desde Zeus como:

```text
Zeus -> child_of -> Crono
```

---

## 21. Niveles de certeza

Toda afirmación discutible debe incluir un nivel de certeza.

Valores iniciales:

```text
confirmed
probable
possible
disputed
modern_interpretation
unknown
```

### Confirmed

La fuente consultada afirma claramente la relación o atributo.

### Probable

La interpretación es ampliamente aceptada, aunque no sea explícita.

### Possible

La interpretación es plausible, pero existen dudas.

### Disputed

Existen desacuerdos importantes entre fuentes o especialistas.

### Modern interpretation

La asociación procede principalmente de estudios o adaptaciones modernas.

### Unknown

Todavía no se ha evaluado la certeza.

---

## 22. Fuentes

Siempre que sea posible, la información debe asociarse con una fuente.

Orden de prioridad recomendado:

1. textos primarios;
2. inscripciones;
3. evidencia arqueológica;
4. estudios académicos;
5. libros de referencia;
6. enciclopedias especializadas;
7. referencias modernas;
8. sitios web generales.

Wikipedia puede utilizarse para localizar información inicial, pero no debe ser la única fuente de un registro marcado como `verified`.

---

## 23. Fuentes primarias

Una fuente primaria puede incluir:

```text
textos clásicos
textos religiosos
inscripciones
papiros
tablillas
poemas épicos
crónicas antiguas
tradición oral documentada
```

Ejemplos:

```text
Teogonía
Ilíada
Odisea
Edda poética
Edda prosaica
Textos de las Pirámides
Libro de los Muertos
```

Cuando sea posible debe registrarse:

```text
obra
autor
libro
capítulo
línea
sección
traducción consultada
```

---

## 24. Fuentes secundarias

Una fuente secundaria puede incluir:

```text
libros académicos
artículos científicos
enciclopedias especializadas
catálogos arqueológicos
estudios históricos
diccionarios mitológicos
```

Debe preferirse material de:

- editoriales académicas;
- universidades;
- revistas especializadas;
- museos;
- instituciones culturales;
- investigadores reconocidos.

---

## 25. Uso de sitios web

Los sitios web pueden utilizarse como referencia inicial.

Deben evaluarse según:

- autoría;
- fecha;
- bibliografía;
- institución responsable;
- estabilidad;
- reputación;
- calidad editorial.

No deben marcarse datos como verificados únicamente con base en:

```text
blogs personales
wikis sin referencias
foros
redes sociales
videos sin bibliografía
páginas de entretenimiento
```

---

## 26. Citas

Las citas deben ser suficientemente precisas para localizar la información.

Ejemplos:

```text
Hesíodo, Teogonía, versos 453-500
Homero, Ilíada, libro I
Snorri Sturluson, Edda prosaica, Gylfaginning
```

Para libros modernos:

```text
Autor, título, editorial, edición, año, página
```

Para artículos:

```text
Autor, título, revista, volumen, número, año, páginas
```

---

## 27. Resúmenes

El campo `summary` debe ser breve, neutral y descriptivo.

Longitud recomendada:

```text
entre 80 y 300 caracteres
```

Ejemplo correcto:

```text
Dios griego del cielo, el trueno y la autoridad soberana, considerado gobernante de los dioses olímpicos.
```

Ejemplo incorrecto:

```text
Zeus fue el dios más poderoso y el mejor de todos los dioses griegos.
```

El resumen no debe incluir:

- opiniones;
- lenguaje promocional;
- afirmaciones absolutas sin fuente;
- detalles excesivos;
- referencias a adaptaciones modernas.

---

## 28. Descripciones extensas

La descripción puede incluir:

- identidad;
- funciones;
- genealogía;
- atributos;
- relaciones;
- culto;
- tradiciones;
- variantes;
- iconografía;
- fuentes relevantes.

Estructura sugerida:

```text
Identidad general
Origen y genealogía
Funciones y dominios
Relaciones principales
Variantes
Culto o relevancia cultural
Fuentes
```

La descripción no debe reemplazar los datos estructurados.

---

## 29. Lenguaje editorial

El contenido debe utilizar un tono:

- neutral;
- informativo;
- claro;
- respetuoso;
- no confesional;
- no sensacionalista.

Evitar:

```text
La verdadera historia es...
La versión correcta afirma...
Sin duda...
Definitivamente...
El dios más importante...
La criatura más poderosa...
```

Preferir:

```text
Según Hesíodo...
En la tradición órfica...
Algunas fuentes posteriores indican...
Una interpretación moderna propone...
No existe consenso sobre...
```

---

## 30. Género

El campo de género debe utilizarse con precaución.

Valores iniciales sugeridos:

```text
male
female
non_binary
variable
collective
not_applicable
unknown
```

Ejemplos:

```text
Zeus: male
Atenea: female
grupo colectivo: collective
lugar: not_applicable
entidad cambiante: variable
información desconocida: unknown
```

No debe inferirse el género únicamente por traducciones modernas.

---

## 31. Lugares

Los lugares mitológicos pueden registrarse como entidades.

Ejemplos:

```text
Monte Olimpo
Asgard
Duat
Valhalla
Tártaro
Yggdrasil
```

Deben clasificarse como:

```text
place
```

Pueden relacionarse mediante:

```text
rules
ruled_by
lives_in
located_in
worshipped_at
associated_with
```

No deben registrarse coordenadas modernas como definitivas cuando el lugar sea simbólico o mítico.

---

## 32. Objetos

Los objetos mitológicos pueden registrarse como entidades.

Ejemplos:

```text
Mjölnir
Égida
Tridente de Poseidón
Ojo de Horus
Vellocino de Oro
```

Categoría:

```text
object
```

Relaciones posibles:

```text
owns
owned_by
created_by
used_by
stolen_by
associated_with
```

---

## 33. Eventos

Los eventos mitológicos pueden registrarse como entidades.

Ejemplos:

```text
Guerra de Troya
Ragnarök
Titanomaquia
Gigantomaquia
Juicio de Osiris
```

Categoría:

```text
event
```

Relaciones posibles:

```text
participated_in
caused
killed_during
occurred_at
associated_with
```

No deben asignarse fechas exactas cuando la tradición no las establezca.

---

## 34. Grupos colectivos

Los grupos colectivos podrán registrarse como entidades.

Ejemplos:

```text
Musas
Nereidas
Valquirias
Erinias
Einherjar
Cuatro Hijos de Horus
```

Categoría:

```text
group
```

Los integrantes se relacionarán mediante:

```text
member_of
has_member
```

Un grupo no debe sustituir los registros individuales cuando sus miembros tengan identidad propia.

---

## 35. Personificaciones

Una personificación representa un concepto abstracto tratado como entidad.

Ejemplos:

```text
Nike
Tánatos
Némesis
Eros
Ma'at
```

Categoría sugerida:

```text
personification
```

Puede clasificarse también como deidad según la tradición.

La decisión debe documentarse y mantenerse consistente.

---

## 36. Entidades híbridas o de clasificación difícil

Algunas entidades pueden pertenecer a varias clasificaciones posibles.

Ejemplo:

```text
Medusa
criatura
mortal transformada
figura ctónica
```

Debe elegirse una categoría principal y registrar las demás características mediante:

- dominios;
- relaciones;
- descripción;
- etiquetas futuras;
- notas editoriales.

No debe duplicarse la entidad para representar cada clasificación.

---

## 37. Adaptaciones modernas

Las adaptaciones modernas no deben mezclarse con la tradición histórica.

Ejemplos:

```text
películas
series
anime
videojuegos
cómics
novelas contemporáneas
juegos de rol
```

No deben incorporarse al perfil histórico como si fueran fuentes mitológicas.

En el futuro podrán registrarse en un módulo separado.

Ejemplo:

```text
Modern Adaptations
```

---

## 38. Contenido ficticio moderno

No deben registrarse como entidades mitológicas históricas:

```text
personajes originales de videojuegos
personajes de películas
versiones de superhéroes
interpretaciones exclusivas de una franquicia
fan fiction
```

Ejemplo:

```text
Thor de Marvel
```

no debe sustituir ni mezclarse con:

```text
Thor de la mitología nórdica
```

---

## 39. Estados editoriales

Estados iniciales:

```text
draft
imported
under_review
reviewed
verified
disputed
```

### Draft

Registro incompleto creado manualmente.

### Imported

Registro incorporado mediante una importación automática o masiva.

### Under review

Registro en proceso de revisión.

### Reviewed

Registro revisado en estructura, ortografía y clasificación.

### Verified

Registro revisado y respaldado por fuentes suficientes.

### Disputed

Registro con datos controvertidos o sin consenso.

---

## 40. Niveles de revisión

Niveles iniciales:

```text
0 - solo nombre
1 - clasificación básica
2 - genealogía básica
3 - dominios y relaciones
4 - fuentes asociadas
5 - revisión editorial completa
```

### Nivel 0

Incluye únicamente:

```text
nombre
mitología
```

### Nivel 1

Incluye:

```text
categoría
resumen básico
género cuando aplica
```

### Nivel 2

Incluye:

```text
genealogía básica
nombres alternativos
```

### Nivel 3

Incluye:

```text
dominios
relaciones principales
tradiciones
```

### Nivel 4

Incluye:

```text
fuentes
citas
variantes documentadas
```

### Nivel 5

Incluye:

```text
revisión completa
consistencia editorial
fuentes suficientes
validación de relaciones
```

---

## 41. Criterios para marcar como `reviewed`

Una entidad puede marcarse como `reviewed` cuando:

- tiene nombre correcto;
- tiene mitología;
- tiene categoría;
- tiene slug válido;
- no es un duplicado;
- el resumen es neutral;
- los nombres alternativos están separados;
- las relaciones básicas están estructuradas;
- no contiene errores evidentes.

No requiere necesariamente fuentes completas.

---

## 42. Criterios para marcar como `verified`

Una entidad puede marcarse como `verified` cuando:

- ha sido revisada editorialmente;
- tiene al menos una fuente confiable;
- sus relaciones principales están documentadas;
- sus variantes importantes están registradas;
- no presenta duplicados;
- los nombres y transliteraciones han sido comprobados;
- las afirmaciones controvertidas están señaladas;
- existe trazabilidad suficiente.

---

## 43. Duplicados

Antes de crear una entidad debe buscarse por:

```text
nombre
slug
nombre original
nombres alternativos
epítetos
transliteraciones
formas latinizadas
```

Ejemplos de posibles duplicados:

```text
Heracles / Hércules
Odysseus / Ulises
Cronus / Kronos / Crono
Thoth / Tot / Djehuty
```

No deben fusionarse automáticamente entidades distintas solo porque compartan un nombre parecido.

---

## 44. Resolución de duplicados

Cuando se detecte un duplicado:

1. comparar mitología;
2. revisar nombres alternativos;
3. revisar categoría;
4. revisar genealogía;
5. revisar fuentes;
6. determinar si son la misma entidad;
7. conservar el registro más completo;
8. migrar relaciones;
9. registrar el nombre eliminado como alternativo;
10. conservar trazabilidad del cambio.

No debe eliminarse información sin revisión.

---

## 45. Slugs

Los slugs deben:

- estar en minúsculas;
- usar guiones;
- omitir acentos;
- evitar caracteres especiales;
- ser estables;
- ser legibles;
- ser únicos dentro de una mitología.

Ejemplos:

```text
Quetzalcóatl -> quetzalcoatl
Amón-Ra -> amun-ra
Huitzilopochtli -> huitzilopochtli
Jörmungandr -> jormungandr
```

Los slugs no deben modificarse automáticamente después de publicarse.

---

## 46. Idiomas

La primera versión tendrá contenido principal en español.

Los códigos de idioma deben usar formatos estandarizados cuando sea posible.

Ejemplos:

```text
es
en
la
grc
non
egy
ar
```

Cuando el idioma no pueda determinarse, podrá utilizarse:

```text
und
```

---

## 47. Sistemas de escritura

El campo `script` podrá indicar el sistema de escritura.

Ejemplos:

```text
Latin
Greek
Runic
Hieroglyphic
Cyrillic
Arabic
Devanagari
```

Debe utilizarse solo cuando aporte información útil.

---

## 48. Fechas y periodos

Las fechas mitológicas suelen ser aproximadas o simbólicas.

No deben inventarse fechas exactas.

Preferir:

```text
Periodo arcaico griego
Reino Nuevo egipcio
Era vikinga
Siglos VIII-VII a. C.
```

Evitar:

```text
Zeus nació en el año 1400 a. C.
```

salvo que se esté citando una interpretación histórica concreta.

---

## 49. Regiones

Las regiones deben describirse con términos históricos o geográficos claros.

Ejemplos:

```text
Grecia continental
Escandinavia
Valle del Nilo
Mesopotamia
Mesoamérica
Península de Yucatán
```

No deben imponerse fronteras políticas modernas a tradiciones antiguas sin explicación.

---

## 50. Reglas para importaciones

Los datos importados deben marcarse inicialmente como:

```text
review_status: imported
```

El importador deberá:

- conservar identificadores externos;
- evitar duplicados;
- generar reporte de errores;
- no sobrescribir datos revisados sin autorización;
- registrar campos faltantes;
- validar mitología;
- validar categoría;
- normalizar slugs;
- separar nombres alternativos;
- registrar procedencia del archivo.

Los registros importados no deben marcarse automáticamente como `verified`.

---

## 51. Reglas para contribuciones externas

Toda contribución deberá:

- respetar el modelo de datos;
- incluir explicación clara;
- evitar duplicados;
- incluir fuentes cuando sea posible;
- separar hechos de interpretación;
- identificar variantes;
- utilizar lenguaje neutral;
- cumplir las convenciones de nombres.

Las contribuciones podrán rechazarse cuando:

- mezclen ficción moderna con tradición histórica;
- no incluyan información verificable;
- dupliquen entidades;
- utilicen contenido copiado sin atribución;
- presenten opiniones como hechos;
- alteren datos sin justificación.

---

## 52. Derechos de autor

No debe copiarse contenido extenso de:

```text
libros
artículos
enciclopedias
sitios web
bases de datos comerciales
```

Las descripciones deben redactarse de forma original.

Se permiten:

- datos factuales;
- nombres;
- relaciones;
- resúmenes originales;
- citas breves con atribución;
- referencias bibliográficas;
- textos en dominio público cuando la licencia lo permita.

Siempre debe respetarse la licencia de las fuentes utilizadas.

---

## 53. Imágenes

Las imágenes deberán tener licencia compatible.

Fuentes preferidas:

```text
dominio público
Creative Commons compatible
Wikimedia Commons
museos con política abierta
imágenes creadas para el proyecto
```

Cada imagen deberá registrar:

```text
autor
fuente
licencia
URL original
fecha de consulta
entidad asociada
```

No deben incorporarse imágenes encontradas en internet sin verificar su licencia.

---

## 54. Correcciones editoriales

Una corrección debe incluir:

- qué se modificó;
- motivo;
- fuente o justificación;
- usuario responsable;
- fecha.

Las correcciones importantes deberán conservar historial.

Ejemplos:

```text
corrección de nombre
cambio de categoría
fusión de duplicados
modificación de genealogía
actualización de fuente
cambio de certeza
```

---

## 55. Contenido controvertido

Cuando existan desacuerdos importantes:

- no eliminar versiones;
- registrar cada variante;
- indicar la tradición;
- añadir fuentes;
- marcar `is_disputed`;
- utilizar certeza `disputed`;
- explicar brevemente el conflicto.

Ejemplo:

```text
Algunas fuentes consideran a Eros una deidad primordial.
Otras lo presentan como hijo de Afrodita.
```

Ambas versiones pueden coexistir.

---

## 56. Neutralidad cultural

El proyecto debe tratar todas las tradiciones con respeto.

No debe:

- ridiculizar creencias;
- declarar una tradición superior;
- usar términos despectivos;
- reducir sistemas religiosos a entretenimiento;
- presentar prácticas culturales sin contexto;
- imponer categorías occidentales sin explicación.

Cuando un término moderno sea aproximado, debe aclararse.

---

## 57. Consistencia terminológica

Los términos técnicos deben utilizarse de forma consistente.

Ejemplo:

```text
mythology
tradition
entity
category
domain
relationship
source
review_status
certainty
```

No deben utilizarse varios términos para el mismo concepto dentro del código o la documentación.

Ejemplo incorrecto:

```text
pantheon
culture
mythology
religion
```

como si fueran siempre equivalentes.

---

## 58. Lista de verificación para nuevas entidades

Antes de publicar una nueva entidad debe comprobarse:

- [ ] Tiene nombre principal.
- [ ] Tiene slug válido.
- [ ] Tiene mitología.
- [ ] Tiene categoría.
- [ ] No es un duplicado.
- [ ] Tiene resumen neutral.
- [ ] Los nombres alternativos están separados.
- [ ] Los dominios son apropiados.
- [ ] Las relaciones están estructuradas.
- [ ] Las variantes están identificadas.
- [ ] Las fuentes están registradas cuando existen.
- [ ] El nivel de certeza es adecuado.
- [ ] El estado editorial es correcto.
- [ ] No contiene contenido moderno mezclado.
- [ ] No infringe derechos de autor.

---

## 59. Lista de verificación para relaciones

Antes de publicar una relación debe comprobarse:

- [ ] La entidad de origen existe.
- [ ] La entidad de destino existe.
- [ ] El tipo de relación es correcto.
- [ ] La dirección es correcta.
- [ ] La relación inversa está definida.
- [ ] La tradición está indicada cuando aplica.
- [ ] La fuente está registrada cuando existe.
- [ ] La certeza es adecuada.
- [ ] No existe un duplicado exacto.
- [ ] La relación no fusiona entidades distintas.
- [ ] La descripción es neutral.

---

## 60. Lista de verificación para fuentes

Antes de publicar una fuente debe comprobarse:

- [ ] Tiene título.
- [ ] Tiene tipo de fuente.
- [ ] Tiene autor cuando se conoce.
- [ ] Tiene periodo o año cuando aplica.
- [ ] Tiene cita bibliográfica.
- [ ] La URL es válida cuando existe.
- [ ] No es un duplicado.
- [ ] La fuente respalda realmente la afirmación.
- [ ] Su nivel de confiabilidad es apropiado.

---

## 61. Decisiones editoriales pendientes

Todavía deben definirse:

- política definitiva de transliteración;
- idioma preferido para nombres principales;
- tratamiento de entidades históricas divinizadas;
- tratamiento de santos sincretizados;
- clasificación de ancestros legendarios;
- reglas para personajes semihistóricos;
- política de fuentes web;
- criterios exactos para nivel 5;
- formato bibliográfico oficial;
- proceso de revisión comunitaria;
- política de imágenes;
- mecanismo de apelación editorial.