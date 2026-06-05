# Sistema de Gestión de Videoclub

## 1. Interpretación del problema

El sistema busca administrar la información de un videoclub, permitiendo registrar socios, películas, actores, alquileres y archivadores donde se almacenan las películas. Además, debe controlar las relaciones entre estos elementos para facilitar la consulta y gestión de la información.

---

## 2. Identificación de entidades y atributos

### Socios

**Atributos:**

* id_socios (PK)
* número
* nombre
* dirección
* teléfono
* directores_favoritos

**Tipos de atributos:**

* Simples: número, nombre, dirección, teléfono.
* Multivaluado: directores_favoritos.

**Clave primaria:**

* id_socios

---

### Películas

**Atributos:**

* id_películas (PK)
* título
* género
* director
* año

**Tipos de atributos:**

* Simples: título, género, director, año.

**Clave primaria:**

* id_películas

---

### Actores

**Atributos:**

* id_actor (PK)
* nombre

**Tipos de atributos:**

* Simples: nombre.

**Clave primaria:**

* id_actor

---

### Almacenamiento

**Atributos:**

* id_almacenamiento (PK)
* número_serie
* ubicación
* número_estanterías
* fecha_compra

**Tipos de atributos:**

* Simples: todos sus atributos.

**Clave primaria:**

* id_almacenamiento

---

### Alquileres

**Atributos:**

* id_alquileres (PK)
* fecha_alquiler
* fecha_devolución
* precio

**Tipos de atributos:**

* Simples: todos sus atributos.

**Clave primaria:**

* id_alquileres

---

## 3. Justificación de relaciones y cardinalidades

### Socios - Películas (Miran)

**Cardinalidad:** N:M

Un socio puede interesarse o visualizar muchas películas y una película puede ser vista por muchos socios. Debido a esta relación muchos a muchos se creó la tabla intermedia `socio_pelicula`.

---

### Películas - Actores (Actúan_en)

**Cardinalidad:** N:M

Una película puede tener varios actores y un actor puede participar en varias películas. Para resolver esta relación se creó la tabla `peliculas_actores`.

---

### Películas - Almacenamiento (Archivadas_en)

**Cardinalidad:** N:1

Cada película se almacena en un único archivador, mientras que un archivador puede contener múltiples películas. Por ello la clave foránea `id_almacenamiento` se encuentra en la tabla Películas.

---

### Socios - Alquileres (Solicitan)

**Cardinalidad:** N:M

Un socio puede realizar varios alquileres a lo largo del tiempo y una película puede ser alquilada por distintos socios en diferentes momentos. Esta relación fue representada mediante la tabla intermedia `socios_alquileres`.

---

## 4. Transformación al modelo relacional

Cada entidad identificada en el modelo conceptual fue transformada en una tabla dentro del modelo relacional:

* Socios
* Películas
* Actores
* Almacenamiento
* Alquileres

Las relaciones N:M fueron transformadas en tablas intermedias para mantener la integridad de los datos:

* socio_pelicula
* peliculas_actores
* socios_alquileres

### Resolución de atributos multivaluados

El atributo `directores_favoritos` fue identificado como multivaluado debido a que un socio puede tener varios directores favoritos. En el modelo presentado se mantuvo como atributo de la entidad Socios, aunque en una implementación completamente normalizada podría separarse en una tabla adicional.

La relación entre películas y actores también representa múltiples valores, por lo que fue resuelta mediante la tabla intermedia `peliculas_actores`.

### Claves foráneas utilizadas

**Películas**

* id_almacenamiento → Almacenamiento(id_almacenamiento)

**socio_pelicula**

* id_socios → Socios(id_socios)
* id_películas → Películas(id_películas)

**peliculas_actores**

* id_películas → Películas(id_películas)
* id_actor → Actores(id_actor)

**socios_alquileres**

* id_socios → Socios(id_socios)
* id_alquileres → Alquileres(id_alquileres)

---

## 5. Decisiones de diseño

### Uso de identificadores únicos

Se utilizaron identificadores únicos para cada entidad con el fin de simplificar las relaciones y facilitar la administración de las claves foráneas.

### Creación de tablas intermedias

Las tablas `socio_pelicula`, `peliculas_actores` y `socios_alquileres` fueron creadas para resolver relaciones de tipo muchos a muchos (N:M), evitando redundancia de información y manteniendo la normalización de la base de datos.

### Relación Películas - Almacenamiento

Se modeló como una relación 1:N debido a que un archivador puede almacenar muchas películas, pero cada película solo puede estar almacenada en un archivador.

### Integridad de los datos

Las claves primarias garantizan la identificación única de cada registro y las claves foráneas permiten mantener la integridad referencial entre las tablas relacionadas.
