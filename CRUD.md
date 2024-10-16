# Operaciones CRUD MySQL

Las operaciones _CRUD_ son un conjunto de 4 operaciones fundamentales en el manejo de bases de datos y aplicaciones web. CRUD es una acronimo que representa las siguientes operaciones:

- **C**REATE (CREAR).
- **R**EAD (LEER).
- **U**PDATE (ACTUALIZAR).
- **D**ELETE (ELIMINAR).

**Primero creamos una tabla:**

```SQL
CREATE TABLE Usuarios(
id_usuarios INT PRIMARY KEY AUTO_INCREMENT,
email VARCHAR (100) UNIQUE NOT NULL CHECK (email LIKE "%_@_%_%"),
password VARCHAR (15) NOT NULL CHECK (LENGTH(password) >= 8)
);
```

La opearción _crear_ es respondable de insertar nuevos datos en la base de datos en languaje SQL esto se realiza con la sentencia `INSERT INTO` y en el caso de MySQL `INSERT` tambien funciona. El propocito de la operación es añadir el registro a una tabla.

```SQL
-- Ejemplo de una inserción valida usando todos los campos.
INSERT INTO Usuarios VALUES(1, "ejemplo@email.com", "12345678");
```

```SQL
-- Ejemplo de una inserción valida usando el comanod DEFAULT.
INSERT INTO Usuarios VALUES(DEFAULT, "ejemplo2@email.com", "12345678");
```

```SQL
-- Ejemplo de una inserción sin incluir el ID usuario.
INSERT Usuarios (email, password) VALUES("ejemplo3@hotmail.com", "12345678");
```

### EJERCICIOS.

- Identifica los tipos de errores que pueden salir en esta tabla.

- Inserta **4 registros nuevos** en un solo insert.

```SQL

-- Instertar 4 datos en un solor INSERT.
INSERT INTO Usuarios (email, password) VALUES
("ejemplo4@gmail.com", "987654321",),
("ejemplo5@gmail.com", "159753264",),
("ejemplo6@gmail.com", "753159846",),
("ejemplo7@gmail.com", "123654879",);
```

## READ

La operacion _LEER_ es utilizada para consultar o recuperar datos de la base de datos. Esto no modifica los datos simplemente los extrae. En MySQL esta operación se realiza con la sentencia `SELECT`

```SQL
-- Ejemplo de una consulta para todos los datos de una tabla.
SELECT * FROM Usuarios;
-- Ejemplo de consulta para un registro en especifico a traves del ID.
SELECT * FROM Usuarios WHERE id_usuario = 1; -- <- Siendo esta una condición.
-- Ejemplo de consulta para un registro con un email en especifico.
SELECT * FROM Usuarios WHERE email = "ejemplo@mail.com";
-- Ejemplo de consulta con solo los datos de email y password.
SELECT email, password FROM Usuarios;
-- Ejemplo de consulta con un condicional lógico.
SELECT * FROM Usuarios WHERE LENGTH(password) >= 9;
```

### EJERCICIOS :(

- Realiza una consulta que muestre solo el email pero que coincida con una contraseña de mas de 8 caracteres y otra que realice una consulta a los ID's pares.

```SQL
-- Consulta que muestra solo el email con una contraseña mayor a 8 caracteres.
SELECT email FROM Usuarios WHERE LENGTH (password) >= 8;
-- Consulta que muestra solo el email con los ID's que sean pares.
SELECT email FROM Usuarios WHERE MOD (id_usuario, 2) = 0;
```

## UPDATE

La operación _ACTUALIZAR_ es utiliza para modificar registros existentes en la base de datos. Esto se hace con la sentenica `UPDATE`

```SQL
-- Ejemplo para actualizar la contraseña por su ID.
UPDATE Usuarios SET password = "1a2b3c4d5" WHERE id_usuario = 1; -- <-Esta es la restricción.
-- Ejemplo para actualizar el email y password de un usuario en especiifico.
UPDATE Usuarios SET password = "1a2b3c4d5" email = "ejemplo2@gmail.com" WHERE id_usuario = 1;
```

### EJERCICIO

- Intenta actualizar registros con valores que violen las restricciones (minimo 3).

## DELETE

La operación _ELEMINAR_ se usa par aborrar registros de la base de datos. Esto se realizar con la sentencia `DELETE`. **Debemos ser muy cuidadosos con esta operación, ya que una ves qeu los datos son eliminados, no pueden ser recuperados**

```SQL
-- Eliminar el usuario por el ID.
DELETE * FROM Usuarios WHERE id_usuario = 4;
-- Elminar los usuarios con el email especifico
DELETE * FROM Usuarios WHERE email = "ejemplo2@gmail.com";
```

### EJERCICIOS

- Eliminar usuarios cuyo email contenga uno o mas cincos.
- Eliminar usuarios que tengan una contraseña que contenga letras mayusculas usando expresiones regulares (cadena de texto [REGEXP] investigar expresines regulares).
- Eliminar usuarios con contraseñas que contengan solo números.
- Eliminiar usuarios con correos que no tengan el dominio GMAIL.

```SQL
--Sentencia para eliminar usuarios cuyo email tengan uno o mas cincos.
DELETE usuario FROM Usuarios WHERE email LIKE '%5%';
--
DELETE usuario FROM nombre_de_la_tabla WHERE password REGEXP '[A-Z]';
--
DELETE FROM nombre_de_la_tabla WHERE password REGEXP '^[0-9]+$';
--
DELETE FROM nombre_de_la_tabla WHERE email NOT LIKE '%@gmail.com';
```

- NOTA: REGEXP es un forma de buscar patrones especificos en texto. Piensa en ella como una lupa magiaca que encuentra ciertos conjuntos de caracteres.
  Aqui va una comparacion mas simple.
  **NORMALMENTE**: Buscas una palabra especifica en un libro.
  Con **REGEXP**: Puedes buscar todas las palabras que empiezan con "A", o con todas las palabras que tengan "ing" al final.

Cuando usamos [A-Z] , le decimos a SQL: "Encuetra cualquier letra mayuscula entren A y Z". Así que si usa una contraseña tiene alguna letra mayúscula, REGEXP '[A-Z]' la encontrará.
