# 🎯 SQL Desde Cero - Aprende Haciendo
### Consultas básicas explicadas con casos reales

---

## 🤔 PARTE 1: Entendiendo SQL - ¿Qué es y por qué existe?

### El problema que SQL resuelve

Imagina que tienes miles de estudiantes en tu sistema. Necesitas:
- Encontrar todos los mayores de 18 años
- Cambiar el correo de Juan Pérez
- Borrar estudiantes que ya se graduaron
- Crear una lista de los mejores promedios

**Sin SQL:** Tendrías que programar cada una de estas tareas manualmente.  
**Con SQL:** Una sola línea hace el trabajo.

### ¿Qué es SQL exactamente?

**SQL (Structured Query Language)** es el idioma que usas para hablar con bases de datos. Es como darle órdenes a tu base de datos:

- "Dame todos los estudiantes" → `SELECT * FROM estudiantes`
- "Crea una nueva tabla" → `CREATE TABLE ...`
- "Actualiza este registro" → `UPDATE ...`

### Lo que hace especial a SQL

SQL es **declarativo**, lo que significa:
- Tú dices **QUÉ** quieres
- La base de datos decide **CÓMO** hacerlo

**Ejemplo:**
```sql
-- Tú escribes esto (QUÉ quieres)
SELECT nombre FROM estudiantes WHERE edad > 18;

-- La base de datos decide cómo buscar eficientemente
-- (podría usar índices, caché, optimizaciones, etc.)
```

---

## 📚 PARTE 2: Historia y contexto

### Los orígenes (Años 70)

- **Creado por:** IBM
- **Nombre original:** SEQUEL (Structured English Query Language)
- **Por qué cambió:** Problemas legales con el nombre
- **Resultado:** Se quedó como SQL

### Evolución hasta hoy

1. **Años 70:** IBM desarrolla SEQUEL/SQL
2. **1986:** Se convierte en estándar ANSI
3. **1987:** Estándar ISO
4. **Hoy:** Usado por millones de aplicaciones en todo el mundo

### Dato curioso sobre los estándares

Aunque SQL es un "estándar", cada sistema tiene sus propias extensiones:

| Sistema    | Extensión especial                    |
|------------|---------------------------------------|
| MySQL      | `LIMIT` para limitar resultados       |
| PostgreSQL | `RETURNING` para obtener datos insertados |
| SQL Server | `TOP` en lugar de `LIMIT`             |
| Oracle     | `ROWNUM` para limitar filas           |

**Esto significa:** El 95% del SQL funciona igual en todos lados, pero ese 5% puede variar.

---

## 🎨 PARTE 3: Fundamentos teóricos

### La teoría detrás de SQL

SQL se basa en dos conceptos matemáticos:

1. **Teoría de conjuntos:** Las tablas son conjuntos de datos
2. **Álgebra relacional:** Las operaciones son relaciones matemáticas

**En la práctica esto significa:**
```sql
-- Unión de conjuntos
SELECT * FROM estudiantes_2023
UNION
SELECT * FROM estudiantes_2024;

-- Intersección (estudiantes que están en ambas tablas)
SELECT * FROM estudiantes_2023
INTERSECT
SELECT * FROM estudiantes_2024;
```

### ¿Qué son las bases de datos relacionales?

Son bases de datos donde la información se organiza en **tablas conectadas**:

**Ejemplo visual:**
```
Tabla: ESTUDIANTES              Tabla: INSCRIPCIONES
+----+---------+                +----+---------------+----------+
| id | nombre  |                | id | estudiante_id | curso_id |
+----+---------+                +----+---------------+----------+
| 1  | Ana     | ←──────────── | 1  | 1             | 101      |
| 2  | Pedro   | ←──────────── | 2  | 2             | 102      |
+----+---------+                +----+---------------+----------+
                                       ↑
                                  Clave foránea
                                (conecta las tablas)
```

**Ventaja:** Si cambias el nombre de Ana en la tabla ESTUDIANTES, automáticamente se refleja en todas sus inscripciones.

---

## 🛠️ PARTE 4: SQL en MySQL - Aplicación práctica

### Las operaciones esenciales que harás cada día

#### 1. Definir estructuras (Crear tablas)

```sql
CREATE TABLE estudiantes (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    edad INT
);
```

**Lo que hace:** Crea un "contenedor" para almacenar estudiantes.

#### 2. Insertar información

```sql
INSERT INTO estudiantes (id, nombre, edad) 
VALUES (1, 'Ana García', 20);
```

**Lo que hace:** Agrega un nuevo estudiante a la tabla.

#### 3. Consultar información

```sql
SELECT nombre, edad 
FROM estudiantes 
WHERE edad > 18;
```

**Lo que hace:** Busca todos los estudiantes mayores de 18 años.

#### 4. Actualizar registros

```sql
UPDATE estudiantes 
SET edad = 21 
WHERE nombre = 'Ana García';
```

**Lo que hace:** Cambia la edad de Ana a 21 años.

#### 5. Eliminar datos

```sql
DELETE FROM estudiantes 
WHERE id = 1;
```

**Lo que hace:** Elimina el registro del estudiante con ID 1.

---

## 📦 PARTE 5: Los tres pilares de SQL

SQL se divide en **tres categorías** según lo que quieras hacer:

### DDL (Data Definition Language) - Define la estructura

**Piensa en ello como:** El arquitecto que diseña el edificio.

**Comandos principales:**

| Comando   | Para qué sirve                          | Ejemplo                                    |
|-----------|------------------------------------------|-------------------------------------------|
| `CREATE`  | Crear nuevas tablas, bases de datos     | `CREATE DATABASE mi_escuela;`             |
| `ALTER`   | Modificar estructuras existentes        | `ALTER TABLE estudiantes ADD email VARCHAR(100);` |
| `DROP`    | Eliminar tablas o bases de datos        | `DROP TABLE estudiantes;`                 |
| `TRUNCATE`| Vaciar una tabla (borrar todos los datos) | `TRUNCATE TABLE estudiantes;`           |

**Ejemplo práctico completo:**
```sql
-- Crear una tabla
CREATE TABLE estudiantes (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    edad INT
);

-- Agregar una nueva columna después
ALTER TABLE estudiantes 
ADD email VARCHAR(100);

-- Si necesitas empezar de cero (borra todos los datos)
TRUNCATE TABLE estudiantes;

-- Si ya no necesitas la tabla (elimínala completamente)
DROP TABLE estudiantes;
```

### DML (Data Manipulation Language) - Manipula los datos

**Piensa en ello como:** Los habitantes que viven en el edificio.

**Comandos principales:**

| Comando  | Para qué sirve              | Ejemplo |
|----------|----------------------------|---------|
| `INSERT` | Agregar nuevos registros   | `INSERT INTO estudiantes VALUES (1, 'Ana', 20);` |
| `UPDATE` | Modificar registros existentes | `UPDATE estudiantes SET edad = 21 WHERE id = 1;` |
| `DELETE` | Eliminar registros específicos | `DELETE FROM estudiantes WHERE edad < 18;` |

**Escenario real:**
```sql
-- Día 1: Inscribir nuevos estudiantes
INSERT INTO estudiantes (id, nombre, edad) VALUES 
(1, 'Ana García', 20),
(2, 'Pedro López', 19),
(3, 'María Torres', 17);

-- Día 30: María cumplió años
UPDATE estudiantes 
SET edad = 18 
WHERE nombre = 'María Torres';

-- Fin de semestre: Estudiantes que se graduaron
DELETE FROM estudiantes 
WHERE id IN (1, 2);
```

### DQL (Data Query Language) - Consulta los datos

**Piensa en ello como:** El inspector que revisa el edificio.

**Comando principal:**

| Comando  | Para qué sirve                    |
|----------|-----------------------------------|
| `SELECT` | Recuperar y consultar información |

**Ejemplos de consultas del mundo real:**
```sql
-- Consulta básica: Dame todos los estudiantes
SELECT * FROM estudiantes;

-- Consulta con filtro: Solo mayores de edad
SELECT nombre, edad 
FROM estudiantes 
WHERE edad >= 18;

-- Consulta ordenada: De mayor a menor edad
SELECT nombre, edad 
FROM estudiantes 
ORDER BY edad DESC;

-- Consulta con agregación: ¿Cuántos estudiantes hay?
SELECT COUNT(*) as total_estudiantes 
FROM estudiantes;
```

---

## 🎓 PARTE 6: Funciones avanzadas de MySQL

Más allá de lo básico, MySQL ofrece herramientas profesionales:

### Procedimientos almacenados
**Qué son:** Código SQL que guardas para reutilizar.

```sql
-- Crear un procedimiento para inscribir estudiantes
CREATE PROCEDURE inscribir_estudiante(
    IN p_nombre VARCHAR(50),
    IN p_edad INT
)
BEGIN
    INSERT INTO estudiantes (nombre, edad) 
    VALUES (p_nombre, p_edad);
END;

-- Usarlo
CALL inscribir_estudiante('Carlos Ruiz', 22);
```

### Vistas
**Qué son:** Consultas guardadas que parecen tablas.

```sql
-- Crear una vista de estudiantes adultos
CREATE VIEW estudiantes_adultos AS
SELECT * FROM estudiantes WHERE edad >= 18;

-- Consultar la vista como si fuera una tabla
SELECT * FROM estudiantes_adultos;
```

### Triggers
**Qué son:** Acciones automáticas que ocurren cuando algo cambia.

```sql
-- Registrar automáticamente cuando se elimina un estudiante
CREATE TRIGGER antes_eliminar_estudiante
BEFORE DELETE ON estudiantes
FOR EACH ROW
BEGIN
    INSERT INTO log_eliminaciones (estudiante_id, fecha)
    VALUES (OLD.id, NOW());
END;
```

### Transacciones
**Qué son:** Grupos de operaciones que se ejecutan todas juntas o ninguna.

```sql
-- Todo o nada: transferir un estudiante de grupo
START TRANSACTION;

DELETE FROM grupo_a WHERE estudiante_id = 1;
INSERT INTO grupo_b (estudiante_id) VALUES (1);

-- Si todo salió bien
COMMIT;

-- Si algo falló, deshacer todo
-- ROLLBACK;
```

---

## 🎯 PARTE 7: Ejemplo completo de la vida real

### Escenario: Sistema de inscripciones escolares

```sql
-- PASO 1: Crear la estructura (DDL)
CREATE DATABASE escuela;
USE escuela;

CREATE TABLE estudiantes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    edad INT,
    email VARCHAR(100)
);

-- PASO 2: Agregar estudiantes (DML)
INSERT INTO estudiantes (nombre, edad, email) VALUES 
('Ana García', 20, 'ana@email.com'),
('Pedro López', 19, 'pedro@email.com'),
('María Torres', 18, 'kevin@email.com');

-- PASO 3: Consultar información (DQL)
-- ¿Quiénes son mayores de 18?
SELECT nombre, edad 
FROM estudiantes 
WHERE edad > 18;

-- PASO 4: Actualizar datos (DML)
-- Ana cambió su email
UPDATE estudiantes 
SET email = 'ana.garcia@newemail.com' 
WHERE nombre = 'Ana García';

-- PASO 5: Gestionar estructura (DDL)
-- Necesitamos agregar teléfonos
ALTER TABLE estudiantes 
ADD telefono VARCHAR(15);

-- PASO 6: Limpiar datos viejos (DML)
-- Eliminar estudiantes que ya no están activos
DELETE FROM estudiantes 
WHERE id = 2;
```

---

## 📊 PARTE 8: Resumen visual - ¿Qué comando usar?

```
¿Qué necesitas hacer?
│
├─ 🏗️ Crear/Modificar estructura
│   └─ Usa DDL
│       ├─ CREATE (crear)
│       ├─ ALTER (modificar)
│       ├─ DROP (eliminar)
│       └─ TRUNCATE (vaciar)
│
├─ ✏️ Modificar datos
│   └─ Usa DML
│       ├─ INSERT (agregar)
│       ├─ UPDATE (actualizar)
│       └─ DELETE (eliminar)
│
└─ 🔍 Consultar información
    └─ Usa DQL
        └─ SELECT (buscar)
```

---

## ✨ PARTE 9: Tips finales

### Lo que debes recordar siempre

1. **DDL afecta la estructura:** Crear, modificar, eliminar tablas
2. **DML afecta los datos:** Insertar, actualizar, eliminar registros
3. **DQL consulta información:** Solo lectura, no modifica nada

### Diferencias importantes

**DROP vs TRUNCATE vs DELETE:**
```sql
-- DROP: Elimina la tabla completa (estructura + datos)
DROP TABLE estudiantes;  -- ¡La tabla ya no existe!

-- TRUNCATE: Vacía la tabla pero mantiene la estructura
TRUNCATE TABLE estudiantes;  -- Tabla vacía, pero sigue existiendo

-- DELETE: Elimina registros específicos
DELETE FROM estudiantes WHERE edad < 18;  -- Solo algunos registros
```

### Convenciones profesionales

```sql
-- ✅ Buena práctica: Comandos en mayúsculas, nombres en minúsculas
SELECT nombre, edad 
FROM estudiantes 
WHERE edad > 18;

-- ✅ También bueno: Usar indentación para legibilidad
INSERT INTO estudiantes (
    nombre,
    edad,
    email
) VALUES (
    'Juan Pérez',
    22,
    'juan@email.com'
);
```

---

**🎉 ¡Ahora entiendes los fundamentos de SQL!**

*Esta guía cubre la teoría esencial y los comandos básicos que usarás en el 90% de tus proyectos*