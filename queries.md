Actividad 32 - Postgresql Avanzado
En esta actividad trabajaremos con las diferentes queries desde el terminal de postgres.

Para desarrollar esta actividad, tendrán que anotar cada una de las queries que utilizaron en un archivo txt o md y subir los archivos comprimidos en formato zip a la plataforma Empieza.

Deben también ingresar, al final de este archivo (txt), el nombre de los integrantes del grupo que participaron en el desarrollo de esta actividad.

Cada integrante debe subir el archivo a la plataforma.

Ejercicio 1
Se busca desarrollar una aplicación llamada Pintagram. Esta aplicación debe permitir a los usuarios subir imágenes y que, a su vez, estas imágenes pertenezcan a ese usuario. Además los usuarios podrán darle like a las imágenes de otros usuarios. Cada una de las imágenes tendrá varios tags y cada uno de esos tags podrá referenciar a varias imágenes. En este ejercicio se tomarán en cuenta las consultas a 2 o más tablas y los constrains al momento de la creación de la tabla.

##Restricciones (constraints)

En esta parte del ejercicio se debe investigar como aplicar relgas a la base de datos para que se cumpla:

Un usuario solo puede darle like 1 vez a cada imagen.
Una imagen no puede tener tags repetidos.
Crear base de datos en base a los requerimientos indicados.
Checkpoints
Antes de empezar a crear la base de datos deben leer todas las instrucciones, modelar la base y generar un diagrama que tendrán que adjuntar a las respuestas de este ejecrcicio.
Ingresar 2 imágenes por usuario.
Ingresar 3 likes por cada imagen.
Ingresar 8 tags.
Ingresar 3 tags por imagen.
Crear una consulta que muestre el nombre de la imagen y la cantidad de likes que tiene esa imagen.
Crear una consulta que muestre el nombre del usuario y los nombres de las fotos que le pertenecen.
Crear una consulta que muestre el nombre del tag y la cantidad de imagenes asociadas a ese tag.

## respuestas

CREATE DATABASE pinstagram_DB;
\c pinstagram_db
pinstagram_db=#

##CREACION DE TABLAS
pinstagram_db=# CREATE TABLE users (
pinstagram_db(# id SERIAL PRIMARY KEY,
pinstagram_db(# name VARCHAR(50)
pinstagram_db(# );
CREATE TABLE

pinstagram_db=# CREATE TABLE images (
pinstagram_db(# id SERIAL PRIMARY KEY,
pinstagram_db(# user_id integer REFERENCES users (id)
pinstagram_db(# ,
pinstagram_db(# name VARCHAR(50)
pinstagram_db(# )
pinstagram_db-# ;
CREATE TABLE

pinstagram_db=# CREATE TABLE likes (
pinstagram_db(# id SERIAL PRIMARY KEY,
pinstagram_db(# user_id integer REFERENCES users (id),
pinstagram_db(# image_id integer REFERENCES images (id)
pinstagram_db(# UNIQUE (user_id, image_id),
pinstagram_db(# )
pinstagram_db-# ;
CREATE TABLE


pinstagram_db=# CREATE TABLE tags (
pinstagram_db(# id SERIAL PRIMARY KEY,
pinstagram_db(# name VARCHAR(50)
pinstagram_db(# )
pinstagram_db-# ;
CREATE TABLE

pinstagram_db=# CREATE TABLE tags_images (
pinstagram_db(# id SERIAL PRIMARY KEY,
pinstagram_db(# tag_id integer REFERENCES tags (id),
pinstagram_db(# image_id integer REFERENCES images (id),
pinstagram_db(# UNIQUE (tag_id, image_id)
pinstagram_db(# )
pinstagram_db-# ;
CREATE TABLE

## INGRESAR 2 IMAGENES POR usuario
pinstagram_db=# INSERT INTO users(name)
pinstagram_db-# VALUES
pinstagram_db-# ('Pedro'),
pinstagram_db-# ('Seba'),
pinstagram_db-# ('Ed'),
pinstagram_db-# ('Fred')
pinstagram_db-# ;
INSERT 0 4

(2, 'fotoSeba1'),
(2, 'fotoSeba2'),
(3, 'fotoEd1'),
(3, 'fotoEd2'),
(4, 'fotoFred1'),
(4, 'fotoFred2'),

pinstagram_db=# INSERT INTO images(user_id, name)
pinstagram_db-# VALUES
pinstagram_db-# (1, 'fotoPedro1'),
pinstagram_db-# (1, 'fotoPedro2'),
pinstagram_db-# (2, 'fotoSeba1'),
pinstagram_db-# (2, 'fotoSeba2'),
pinstagram_db-# (3, 'fotoEd1'),
pinstagram_db-# (3, 'fotoEd2'),
pinstagram_db-# (4, 'fotoFred1'),
pinstagram_db-# (4, 'fotoFred2'),
pinstagram_db-# (4, 'fotoFred3')
pinstagram_db-# ;
INSERT 0 9


##  Ingresar 3 likes por cada imagen.

INSERT INTO likes (user_id, image_id)
VALUES
(1,1),
(2,1),
(3,1),
(1,2),
(2,2),
(3,2),
(1,3),
(2,3),
(3,3),
(1,4),
(2,4),
(3,4),
(1,5),
(2,5),
(3,5),
(1,6),
(2,6),
(3,6),
(1,7),
(2,7),
(3,7),
(1,8),
(2,8),
(3,8),
(1,9),
(2,9),
(3,9)
;


##Ingresar 8 tags.

INSERT INTO tags (name)
VALUES
('PYME'),
('WHolesales'),
('NOVA'),
('Everyday Banking'),
('retail'),
('Finanzas'),
('CNB'),
('BciLabs')
;


## cada imagen con 3 tags

INSERT INTO tags_images (tag_id, image_id)
VALUES
(1,1),
(2,1),
(8,1),
(1,2),
(2,2),
(8,2),
(1,3),
(2,3),
(8,3),
(1,4),
(2,4),
(8,4),
(1,5),
(2,5),
(8,5),
(1,6),
(2,6),
(8,6),
(1,7),
(2,7),
(8,7),
(1,8),
(2,8),
(8,8),
(1,9),
(2,9),
(8,9),
(3,1),
(4,1)
;


##
Crear una consulta que muestre el nombre de la imagen y la cantidad de likes que tiene esa imagen.


pinstagram_db=# SELECT name, count(likes.id) from images inner join likes on (images.id = likes.image_id) group by name;
    name    | count
------------+-------
 fotoEd1    |     3
 fotoPedro1 |     3
 fotoFred3  |     3
 fotoSeba2  |     3
 fotoFred2  |     3
 fotoFred1  |     3
 fotoSeba1  |     3
 fotoPedro2 |     3
 fotoEd2    |     3
(9 rows)

## Crear una consulta que muestre el nombre del usuario y los nombres de las fotos que le pertenecen.

pinstagram_db=# SELECT users.name, images.name from users inner join images on (users.id = images.user_id);
 name  |    name    
-------+------------
 Pedro | fotoPedro1
 Pedro | fotoPedro2
 Seba  | fotoSeba1
 Seba  | fotoSeba2
 Ed    | fotoEd1
 Ed    | fotoEd2
 Fred  | fotoFred1
 Fred  | fotoFred2
 Fred  | fotoFred3
(9 rows)


##Crear una consulta que muestre el nombre del tag y la cantidad de imagenes asociadas a ese tag.

pinstagram_db=# SELECT tags.name, count(tags_images.image_id) from tags inner join tags_images on (tags.id = tags_images.tag_id) group by tags.name;
       name       | count
------------------+-------
 Everyday Banking |     1
 NOVA             |     1
 BciLabs          |     9
 WHolesales       |     9
 PYME             |     9
(5 rows)
