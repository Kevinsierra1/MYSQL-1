# 🚀 MySQL: De Cero a Héroe
### Guía práctica organizada por escenarios reales

---

## 📦 PARTE 1: Configurando tu entorno

### Primer día: ¿MySQL está funcionando?

```bash
# ¿Está activo MySQL?
sudo systemctl status mysql.service

# Si no está activo, inícialo
sudo systemctl start mysql.service

# ¿Problemas? Reinicia el servicio
sudo systemctl restart mysql.service
```

**⚠️ Si ves este warning:**
```
Warning: The unit file, source configuration file or drop-ins of mysql.service changed on disk.
```
**Solución rápida:**
```bash
sudo systemctl daemon-reload
```

### Entrando por primera vez

```bash
# Acceso como administrador (root)
mysql -u root -p

# Acceso remoto (desde otra máquina)
mysql -u kevin -h 172.16.101.104 -p
```

💡 **Tip:** La terminal te pedirá la contraseña después de presionar Enter.

---

## 🏗️ PARTE 2: Creando tu proyecto desde cero

### Escenario: Vas a crear una aplicación de gestión de personas

#### Paso 1: Crear la base de datos

```sql
CREATE DATABASE explicaciondia1;
```

❌ **Error típico de principiantes:**
```sql
CREATE explicaciondia1;  -- ¡Falta la palabra DATABASE!
```

#### Paso 2: Activar tu base de datos

```sql
USE explicaciondia1;
```

#### Paso 3: Verificar que estás en el lugar correcto

```sql
-- Ver todas las bases de datos disponibles
SHOW DATABASES;

-- Ver las tablas dentro de tu base de datos actual
SHOW TABLES;
```

**Salida típica al inicio:**
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| explicaciondia1    |  ← Tu nueva base de datos
+--------------------+
```

---

## 👥 PARTE 3: Gestionando usuarios y permisos

### Escenario: Tu compañero Andrés necesita acceso

#### Crear el usuario

```sql
CREATE USER 'kevin'@'%' IDENTIFIED BY 'contraseña_segura';
```

**Significado de los símbolos:**
- `'%'` = Andrés puede conectarse desde cualquier IP
- `'localhost'` = Solo desde la máquina local
- `'192.168.1.%'` = Solo desde la red 192.168.1.x

#### Darle permisos específicos

```sql
-- Andrés puede insertar y actualizar datos en la tabla Persona
GRANT INSERT, UPDATE ON explicaciondia1.Persona TO 'kevin'@'%';

-- También puede consultar datos
GRANT SELECT ON explicaciondia1.Persona TO 'kevin'@'%';

-- Puede crear bases de datos en todo el servidor
GRANT CREATE ON *.* TO 'kevin'@'%';

-- ⚡ IMPORTANTE: Aplicar los cambios
FLUSH PRIVILEGES;
```

**❌ Error común:**
```sql
GRANT INSERT.UPDATE ON ...  -- ¡Punto incorrecto!
```

**✅ Forma correcta:**
```sql
GRANT INSERT, UPDATE ON ...  -- Usa comas
```

#### Verificar qué permisos tiene

```sql
SHOW GRANTS FOR 'kevin'@'%';
```

#### Revocar permisos cuando ya no los necesita

```sql
REVOKE SELECT ON explicaciondia1.Persona FROM 'kevin'@'%';
FLUSH PRIVILEGES;
```

---

## 🎯 PARTE 4: Flujo de trabajo completo

### Escenario real: Nuevo proyecto con equipo

```sql
-- 1️⃣ Crear el proyecto
CREATE DATABASE mi_aplicacion;

-- 2️⃣ Activar el proyecto
USE mi_aplicacion;

-- 3️⃣ Crear usuario para el desarrollador
CREATE USER 'dev_juan'@'%' IDENTIFIED BY 'pass123';

-- 4️⃣ Dar permisos de desarrollo completo
GRANT INSERT, UPDATE, SELECT, DELETE ON mi_aplicacion.* TO 'dev_juan'@'%';

-- 5️⃣ Crear usuario para la aplicación (solo lectura/escritura)
CREATE USER 'app_user'@'%' IDENTIFIED BY 'pass456';
GRANT INSERT, SELECT ON mi_aplicacion.* TO 'app_user'@'%';

-- 6️⃣ Aplicar todos los cambios
FLUSH PRIVILEGES;

-- 7️⃣ Verificar configuración
SHOW GRANTS FOR 'dev_juan'@'%';
SHOW GRANTS FOR 'app_user'@'%';
```

---

## 🔧 PARTE 5: Mantenimiento y administración

### Gestión de usuarios

```sql
-- Ver todos los usuarios del sistema
SELECT user, host FROM mysql.user;

-- Eliminar un usuario que ya no trabaja en el proyecto
DROP USER 'ex_empleado'@'%';

-- Cambiar contraseña de un usuario
ALTER USER 'kevin'@'%' IDENTIFIED BY 'nueva_contraseña';
```

### Tareas administrativas útiles

```bash
# Ver historial de comandos ejecutados
history

# Editar configuración avanzada de MySQL
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

---

## 💬 PARTE 6: Documentando tu código

```sql
-- Esto es un comentario de una línea
-- Úsalo para explicar comandos individuales

/* 
   Esto es un comentario de múltiples líneas
   Úsalo para explicar bloques completos
   o deshabilitar temporalmente código
*/

CREATE DATABASE ejemplo;  -- Base de datos de pruebas
```

---

## 🚨 PARTE 7: Troubleshooting - Resolviendo problemas

### Problema 1: "No puedo conectarme a MySQL"

**Síntoma:**
```
Can't connect to MySQL server
```

**Diagnóstico:**
```bash
sudo systemctl status mysql.service
```

**Solución:**
```bash
sudo systemctl start mysql.service
```

---

### Problema 2: "Error de sintaxis"

**Síntoma:**
```
ERROR 1064 (42000): You have an error in your SQL syntax
```

**Causas comunes:**
- Olvidaste la palabra `DATABASE` en `CREATE DATABASE`
- Usaste punto (`.`) en lugar de coma (`,`) en `GRANT`
- Olvidaste el punto y coma (`;`) al final

**Solución:** Revisa la estructura del comando letra por letra

---

### Problema 3: "Acceso denegado"

**Síntoma:**
```
Access denied for user 'kevin'@'localhost'
```

**Diagnóstico:**
```sql
SHOW GRANTS FOR 'kevin'@'%';
```

**Solución:** Otorga los permisos necesarios y ejecuta `FLUSH PRIVILEGES;`

---

## 📋 PARTE 8: Cheat Sheet - Referencia rápida

| Necesito...                   | Comando                                                |
|-------------------------------|--------------------------------------------------------|
| Ver bases de datos            | `SHOW DATABASES;`                                      |
| Ver tablas                    | `SHOW TABLES;`                                         |
| Ver permisos de usuario       | `SHOW GRANTS FOR 'usuario'@'%';`                      |
| Crear base de datos           | `CREATE DATABASE nombre;`                              |
| Usar una base de datos        | `USE nombre;`                                          |
| Crear usuario                 | `CREATE USER 'user'@'%' IDENTIFIED BY 'pass';`        |
| Dar permisos                  | `GRANT privilegios ON bd.tabla TO 'user'@'%';`        |
| Quitar permisos               | `REVOKE privilegios ON bd.tabla FROM 'user'@'%';`     |
| Aplicar cambios               | `FLUSH PRIVILEGES;`                                    |
| Eliminar usuario              | `DROP USER 'usuario'@'%';`                            |

---

## ✨ PARTE 9: Buenas prácticas del profesional

### ✅ Siempre haz esto:

1. **Termina con punto y coma:** Cada comando SQL debe terminar con `;`
2. **Usa FLUSH PRIVILEGES:** Después de modificar permisos
3. **Documenta tu código:** Usa comentarios para explicar decisiones
4. **Usa mayúsculas para comandos SQL:** Es convención (pero no obligatorio)

### ⚠️ Nunca hagas esto:

1. **No des permisos de administrador a todos:** Usa el principio de mínimo privilegio
2. **No uses la cuenta root para aplicaciones:** Crea usuarios específicos
3. **No olvides FLUSH PRIVILEGES:** Los cambios no se aplicarán inmediatamente

### 🎯 Convenciones de nomenclatura:

```sql
-- ✅ Bien: Comandos en mayúsculas, nombres en minúsculas
CREATE DATABASE mi_proyecto;
GRANT SELECT ON mi_proyecto.usuarios TO 'app'@'%';

-- También funciona, pero menos legible
create database MI_PROYECTO;
grant select on MI_PROYECTO.USUARIOS to 'app'@'%';
```


**🎉 ¡Felicidades! Ahora dominas los fundamentos de MySQL**

*Esta guía cubre el 80% de las tareas diarias que realizarás con MySQL*