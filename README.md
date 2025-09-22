# 📚 Práctica de Base de Datos – Biblioteca (PostgreSQL)

[Las capturas de pantalla con la salida estan a partir del punto 4]

---

## 1 Creación de la Base de Datos

sql:
CREATE DATABASE biblioteca;

## 2 Creación de usuarios

-- Usuario administrador
CREATE USER admin_biblio WITH PASSWORD 'admin123';

-- Usuario solo lectura
CREATE USER usuario_biblio WITH PASSWORD 'usuario123';

CREATE ROLE lectores;

-- Permisos de solo consulta en todo el esquema public
GRANT CONNECT ON DATABASE biblioteca TO lectores;
GRANT USAGE ON SCHEMA public TO lectores;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lectores;

-- Nuevas tablas tendrán automáticamente permisos de lectura
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO lectores;

-- Asignar el rol al usuario de lectura
GRANT lectores TO usuario_biblio;


SELECT rolname, rolsuper, rolcreaterole, rolcreatedb, rolcanlogin
FROM pg_roles;


ALTER USER usuario_biblio WITH PASSWORD 'nueva_clave';

REVOKE INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public FROM usuario_biblio;


##  3 Creación de la Base de Datos

-- Autores
CREATE TABLE autores (
    id_autor SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50)
);

-- Libros
CREATE TABLE libros (
    id_libro SERIAL PRIMARY KEY,
    titulo VARCHAR(200) NOT NULL,
    año_publicacion INT,
    id_autor INT NOT NULL,
    CONSTRAINT fk_autor FOREIGN KEY(id_autor) REFERENCES autores(id_autor)
);

-- Préstamos
CREATE TABLE prestamos (
    id_prestamo SERIAL PRIMARY KEY,
    id_libro INT NOT NULL,
    fecha_prestamo DATE NOT NULL,
    fecha_devolucion DATE,
    usuario_prestatario VARCHAR(50) NOT NULL,
    CONSTRAINT fk_libro FOREIGN KEY(id_libro)
        REFERENCES libros(id_libro)
        ON DELETE CASCADE  -- opcional
);

## 4 

-- Autores (el autor 0 es Anónimo)
INSERT INTO autores (id_autor, nombre, nacionalidad) VALUES
(0, 'Anónimo', 'Desconocida'),
(DEFAULT, 'Gabriel García Márquez', 'Colombia'),
(DEFAULT, 'Isabel Allende', 'Chile'),
(DEFAULT, 'J.K. Rowling', 'Reino Unido'),
(DEFAULT, 'Haruki Murakami', 'Japón');
![](1.png)

-- Libros
INSERT INTO libros (titulo, año_publicacion, id_autor) VALUES
('Cien años de soledad', 1967, 2),
('Crónica de una muerte anunciada', 1981, 2),
('La casa de los espíritus', 1982, 3),
('Harry Potter y la piedra filosofal', 1997, 4),
('Harry Potter y la cámara secreta', 1998, 4),
('Kafka en la orilla', 2002, 5),
('1Q84', 2009, 5),
('Libro sin autor', 2025, 0);

-- Préstamos (usar IDs reales de libros)
INSERT INTO prestamos (id_libro, fecha_prestamo, fecha_devolucion, usuario_prestatario) VALUES
(1, '2025-09-01', '2025-09-10', 'usuario1'),
(2, '2025-09-02', '2025-09-12', 'usuario2'),
(3, '2025-09-03', '2025-09-13', 'usuario3'),
(4, '2025-09-04', '2025-09-14', 'usuario4'),
(5, '2025-09-05', NULL,      'usuario5'); -- préstamo pendiente


## 5 Consultas Básicas

## 6 Consultas con Agregación

## 7 Modificación de Datos

## 8 Creación de Vistas

## 9 Funciones y Consultas Avanzadas
