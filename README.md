# Fundamentos de Bases de Datos

Este documento detalla las instrucciones básicas para trabajar con bases de datos en Oracle utilizando SQL\*Plus y SQL Developer. Contiene operaciones esenciales para crear, modificar y administrar estructuras y datos.

---

## Conexión y Operaciones Básicas

1. **Ingresar a SQL\*Plus**

   ```sql
   sqlplus system/[password]
   ```

2. **Operaciones con la tabla DUAL:**

   - Obtener información del usuario:
     ```sql
     SELECT USER FROM DUAL;
     ```
   - Obtener la fecha actual:
     ```sql
     SELECT SYSDATE FROM DUAL;
     ```
   - Ejecutar cualquier consulta:
     ```sql
     SELECT * FROM DUAL;
     ```

3. **Abrir SQL Developer y ejecutar las consultas anteriores.**

---

## Administración de Tablespaces

1. **Crear un Tablespace:**

   ```sql
   CREATE TABLESPACE NombreTabla1
   DATAFILE 'ruta' SIZE 200M;
   ```

2. **Ver los tablespaces existentes:**

   ```sql
   SELECT * FROM DBA_TABLESPACES;
   SELECT * FROM DBA_DATA_FILES;
   ```

3. **Crear un Tablespace temporal:**

   ```sql
   CREATE TEMPORARY TABLESPACE NombreTabla2
   TEMPFILE 'ruta' SIZE 200M;
   ```

---

## Creación y Administración de Roles

1. **Activar scripts personalizados:**

   ```sql
   ALTER SESSION SET "_ORACLE_SCRIPT"=TRUE;
   ```

2. **Crear un rol:**

   ```sql
   CREATE ROLE NombreRol;
   ```

3. **Asignar permisos al rol:**

   ```sql
   GRANT CREATE SESSION TO NombreRol;
   GRANT CREATE ANY TABLE TO NombreRol;
   GRANT CREATE ROLE TO NombreRol;
   GRANT CREATE USER TO NombreRol;
   GRANT CREATE VIEW TO NombreRol;
   GRANT CREATE ANY INDEX TO NombreRol;
   GRANT CREATE TRIGGER TO NombreRol;
   GRANT CREATE PROCEDURE TO NombreRol;
   GRANT CREATE SEQUENCE TO NombreRol;
   ```

4. **Ver los roles existentes:**

   ```sql
   SELECT ROLE FROM DBA_ROLES;
   ```

---

## Creación y Administración de Usuarios

1. **Crear un usuario:**

   ```sql
   CREATE USER "Nombre" IDENTIFIED BY "admin"
   DEFAULT TABLESPACE NombreTabla1
   TEMPORARY TABLESPACE NombreTabla2;
   ```

2. **Asignar permisos al usuario:**

   ```sql
   GRANT UNLIMITED TABLESPACE TO "Nombre";
   GRANT NombreRol TO "Nombre";
   ```

3. **Ver los usuarios existentes:**

   ```sql
   SELECT * FROM DBA_USERS;
   ```

---

## Gestión de Tablas

1. **Crear una tabla:**

   ```sql
   CREATE TABLE usuario (
       rut NUMBER(8) NOT NULL,
       nombre VARCHAR2(30) NOT NULL,
       apellido VARCHAR2(30) NOT NULL,
       CONSTRAINT pk_usuario PRIMARY KEY (rut)
   );
   ```

2. **Eliminar una tabla:**

   ```sql
   DROP TABLE usuario;
   ```

3. **Observar el contenido de una tabla:**

   ```sql
   SELECT * FROM usuario;
   ```

4. **Modificar una tabla:**

   - Añadir una columna:
     ```sql
     ALTER TABLE usuario ADD clave VARCHAR2(30) NOT NULL;
     ```
   - Modificar una columna:
     ```sql
     ALTER TABLE usuario MODIFY clave VARCHAR2(8);
     ```
   - Añadir otra columna:
     ```sql
     ALTER TABLE usuario ADD ciudad VARCHAR2(30) NOT NULL;
     ```

5. **Insertar valores:**

   ```sql
   INSERT INTO usuario VALUES (21313213, 'Juan', 'Perez', '123321', 'Santiago');
   ```

6. **Actualizar valores:**

   - Cambiar una ciudad:
     ```sql
     UPDATE usuario SET ciudad = 'Valparaiso';
     ```
   - Cambiar la ciudad de un rut específico:
     ```sql
     UPDATE usuario SET ciudad = 'SANTIAGO' WHERE rut = 21313213;
     ```

7. **Eliminar registros:**

   ```sql
   DELETE FROM usuario WHERE rut = 21313213;
   ```

8. **Ordenar resultados:**

   - Descendente:
     ```sql
     SELECT * FROM usuario ORDER BY nombre DESC;
     ```
   - Ascendente:
     ```sql
     SELECT * FROM usuario ORDER BY nombre;
     ```

---

## Transacciones y Backups

1. **Commit:**

   Confirma de forma permanente los cambios realizados en la base de datos:
   ```sql
   COMMIT;
   ```

2. **Rollback:**

   Revierte los cambios realizados desde el último COMMIT o SAVEPOINT:
   ```sql
   ROLLBACK;
   ```

3. **Backups:**

   Para realizar un respaldo de la base de datos, puede utilizar herramientas como RMAN o exportaciones mediante SQL Developer.
   Ejemplo con exportación:
   ```
   expdp usuario/password DIRECTORY=backup_dir DUMPFILE=backup.dmp LOGFILE=backup.log;
   ```

---

## Triggers

1. **Definición:**

   Un **trigger** es un bloque de código que se ejecuta automáticamente en respuesta a un evento en una tabla o vista (INSERT, UPDATE, DELETE).

2. **Creación de un trigger:**

   Ejemplo: Mantener un registro de auditoría al insertar datos:
   ```sql
   CREATE OR REPLACE TRIGGER audit_trigger
   AFTER INSERT ON usuario
   FOR EACH ROW
   BEGIN
       INSERT INTO audit_log (user_id, action_date, action)
       VALUES (:NEW.rut, SYSDATE, 'INSERT');
   END;
   ```

3. **Ver triggers existentes:**

   ```sql
   SELECT * FROM USER_TRIGGERS;
   ```

---

## Conexión y Pruebas

- Los usuarios y consultas pueden probarse tanto en SQL\*Plus como en SQL Developer.
- Se pueden crear conexiones para explorar las tablas filtradas.

---


