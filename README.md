> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)
> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc. 

> 👉 Si te resultó útil, **se agradece que lo compartas para que le llegue a más gente!**

# ![Notas sobre Bases de Datos - Relaciones](https://i.imgur.com/5fwpN9v.png)

## Notas relacionadas

### `databases`

- [**Bases De Datos - Intro**](https://github.com/undefinedschool/notes-dbs)
- [**SQL**](https://github.com/undefinedschool/notes-sql/)

### `backend`

- [**NodeJS**](https://github.com/undefinedschool/notes-nodejs)
- [**Express**](https://github.com/undefinedschool/notes-expressjs)
- [**APIs**](https://github.com/undefinedschool/notes-apis)

## Contenido

- [Relaciones](https://github.com/undefinedschool/notes-dbs-relationships#relaciones)
  - [Tipos de relaciones](https://github.com/undefinedschool/notes-dbs-relationships#tipos-de-relaciones)
    - [1 to 1](https://github.com/undefinedschool/notes-dbs-relationships#1-to-1)
    - [1 to many](https://github.com/undefinedschool/notes-dbs-relationships#1-to-many)
    - [many to many](https://github.com/undefinedschool/notes-dbs-relationships#many-to-many)

---

## Relaciones

Necesitamos describir formalmente las relaciones entre las tablas de nuestra base de datos. **La forma en que describimos las relaciones es a través de las claves** (Primary Keys, Foreign Keys).

[↑ Ir al inicio](https://github.com/undefinedschool/notes-dbs-relationships#contenido)

### Tipos de relaciones

#### 1 to 1

**En las relaciones _1 a 1_, una _entidad_ sólo tiene relación con otra y viceversa**. Por ejemplo, un número de pasaporte puede estar asociado a una única persona y cada persona puede tener sólo 1 número de pasaporte.

Para diseñar relaciones _1 to 1_, la forma más común es poner los datos en diferentes columnas de la misma tabla. Por ejemplo, si queremos relacionar un `user_id` con un `username`

| user_id | fist_name | last_name | username  |
|---------|-----------|-----------|-----------|
| 1       | Linus     | Torvalds  | ltorvalds |

En este caso, el `user_id` 1 está asociado sólo con el `username` ltorvalds y viceversa.

[![Database Design - Designing One-to-One Relationships](https://img.youtube.com/vi/M-Rw21NGORo/0.jpg)](https://www.youtube.com/watch?v=M-Rw21NGORo)
> Ver [Database Design - Designing One-to-One Relationships](https://www.youtube.com/watch?v=M-Rw21NGORo)

También podemos tener relaciones _1 to 1_ con los datos distribuídos en 2 tablas, por ejemplo, si queremos relacionar usuarios con tarjetas de crédito, podemos hacer

| id | fist_name | last_name | card_id |
|---------|-----------|-----------|---------|
| 1       | Caleb     | Curry     | 50      |

| id | issue_date | max_ammount | user_id |
|----|------------|-------------|---------|
| 50 | 2020/03/01 | 5000        | 1       |

[↑ Ir al inicio](https://github.com/undefinedschool/notes-dbs-relationships#contenido)

#### 1 to many

**En las relaciones _1 a muchos_, una _entidad_ puede tener relaciones con 1 o más entidades (pero no a la inversa)**. 

Por ejemplo, las siguientes tablas se encuentran relacionadas a través del `customer_id`. En la tabla `Customer`, cumple el rol de [_Primary Key_](https://github.com/undefinedschool/notes-dbs#primary-key-1), mientras que en la tabla `Order`, cumple el rol de [_Foreign Key_](https://github.com/undefinedschool/notes-dbs#foreign-key-1).

> ⚠️ **Notar que esto implica que `customer_id` debe ser único en la tabla `Customer`, pero no necesariamente en `Order`**.

### `Customer`

| customer_id | first_name | last_name | email          | address       |
|-------------|------------|-----------|----------------|---------------|
| 367         | Michelle   | Blackwell | mblackwell@... | 22 Acacia...  |
| 368         | Lynn       | Allen     | la1942@...     | 1016B 1st...  |
| 369         | Lee        | Stout     | lee@...        | 47 Main St... |

### `Order`

| order_id | date     | quantity | total    | customer_id |
|----------|----------|----------|----------|-------------|
| 1198     | 3/1/2011 | 17       | $340.00  | 367         |
| 1199     | 3/2/2011 | 47       | $902.00  | 367         |
| 1200     | 3/2/2011 | 104      | $1500.00 | 368         |

Este tipo de relación se conoce como [_1 to many_](https://github.com/undefinedschool/notes-dbs-relationships#1-to-many), ya que cada `customer` puede tener asociadas 1 o más `orders`.

> ⚠️ **Notar que la inversa no es cierta, en este caso cada orden puede tener 1 (y sólo 1) cliente asociado**.

Este es el tipo de relación más común en bases de datos relacionales.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-dbs-relationships#contenido)

#### many to many

**En las relaciones _muchos a muchos_, muchas _entidades_ pueden tener relaciones con 1 o más entidades y viceversa**. Por ejemplo, muchos estudiantes pueden estar cursando más de 1 asignatura y a la vez cada asignatura puede tener más de 1 estudiante anotado.

> 👉 **En la mayoría de las bases de datos relacionales no podemos expresar relaciones _many to many_ directamente**, sino que tendremos que hacerlo indirectamente.

Por ejemplo, podemos tener una tabla de autores (`authors`), otra de libros (`books`) y relacionar los autores con los libros de forma tal que un/a autor/a pueda estar asociado a uno o más libros, es decir, una relación del tipo [_1 to many_](https://github.com/undefinedschool/notes-dbs#1-to-many). 

##### `authors`

| author_id (PK) | first_name | last_name | email          |
|-----------|------------|-----------|----------------|
| 445       | Tucker     | Morrison  | tmorrison@...  |
| 446       | Robert     | Allen     | rallen@...     |
| 447       | Jordan     | Winters   | jwinters64@... |

##### `books`

| book_id (PK) | title                  | list_price | author_id (FK) |
|--------------|------------------------|------------|----------------|
| 1145         | Designing Databases    | $45        | 447            |
| 1146         | PostgreSQL Made Simple | $39.95     | 446            |
| 1147         | Y U Don't Need MongoDB | $19.95     | 447            |

El problema aparece si un mismo libro puede tener varios autores, cómo representamos esa relación? No podemos asociar un libro con múltiples _Foreign Keys_ (`authorID` sería una [_Foreign Key_](https://github.com/undefinedschool/notes-dbs#foreign-key-1) de la tabla `books`), al menos no con una única columna `author_id`. Una posible solución podría ser agregar una segunda columna, `author_id_2` (_nulleable_, en el caso de que haya un único autor), como se muestra abajo, pero esta no es una práctica recomendada y lleva a un mal diseño, ya que estamos repitiendo información (columnas), por lo que vamos a intentar evitarla.

| book_id (PK) | title                  | list_price | author_id_1 (FK) | author_id_2 (FK) |
|--------------|------------------------|------------|------------------|------------------|
| 1145         | Designing Databases    | $45        | 447              | 446              |
| 1146         | PostgreSQL Made Simple | $39.95     | 446              | (null)           |
| 1147         | Y U Don't Need MongoDB | $19.95     | 447              | 445              |

[↑ Ir al inicio](https://github.com/undefinedschool/notes-dbs-relationships#contenido)
