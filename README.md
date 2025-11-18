# Práctica de Acceso a Datos — Migración entre Bases de Datos (prac2 → prac2migra)

**Autora:** Cecilia Molina  
**Asignatura:** Acceso a Datos  
**Curso:** 2º DAM  

---

## 📌 Descripción del Proyecto
Este proyecto implementa una **migración completa de datos** entre dos bases de datos **MySQL**:

- **prac2** → Base de datos original  
- **prac2migra** → Base de datos destino  

El objetivo es aplicar conceptos avanzados de **JDBC**, **DAOs**, **patrones de diseño**, **logs** y (opcionalmente) **transacciones**, simulando un proceso real de migración de información entre entornos.

La práctica incluye:
- Inserción inicial de datos en la base de datos origen  
- Capa de modelos (POJOs)  
- Capa DAO para acceso estructurado a cada tabla  
- Proceso de migración (`cliente → cliente_migra`, `producto → producto_migra`, `pedido → pedido_migra`)  
- Registro del proceso mediante **logging**  

---


## 🧩 Componentes principales

### ✔️ 1. Modelos (POJOs)
Representan las tablas de ambas bases de datos:
- `Cliente` / `ClienteMigra`
- `Producto` / `ProductoMigra`
- `Pedido` / `PedidoMigra`

Cada clase contiene:
- Atributos correspondientes  
- Getters/Setters  
- Constructores  
- `toString()`  

---

### ✔️ 2. DAO Origen
Clases responsables de consultar datos en la BD **prac2**:
- `obtenerTodos()`  
- `existe()`  
- `insert()` / `update()` / `deleteById()` *(opcional)*  

> Usan **PreparedStatement** para prevenir inyecciones SQL.

---

### ✔️ 3. DAO Destino
Clases responsables de insertar registros migrados en la BD **prac2migra**.  
Cada clase implementa:
- `insertar(modeloMigra)`  
- Logs en cada operación  

---

### ✔️ 4. Inserción Inicial de Datos
La clase `InsertarDatosIniciales` genera:
- 50 clientes  
- 20 productos  
- 60 pedidos  

> Utiliza **batch updates** (`addBatch()` + `executeBatch()`) para eficiencia.

---

### ✔️ 5. Proceso de Migración (`MainMigracion`)
Realiza:
- Obtener clientes de **prac2**  
- Insertarlos en **prac2migra**  
- Repetir para productos  
- Repetir para pedidos  

Se usa **Logger** con distintos niveles:
- `INFO` → Estados generales  
- `FINE` → Cada inserción individual  
- `SEVERE` → Errores inesperados  

---

## 📝 Logs
La aplicación utiliza `java.util.logging.Logger`.


---

## 🚀 Ejecución
- Para lanzar la migración:  
  `MainMigracion.java`  

- Para insertar datos iniciales:  
  `InsertarDatosIniciales.java`  

---

## ⭐ Objetivos de aprendizaje alcanzados
- Manejo avanzado de **JDBC**  
- Patrones **DAO** y separación por capas  
- Migración de datos entre bases  
- Logging profesional  
- Batches para inserciones masivas  
- Estructuras limpias y escalables  

---

## 📌 Posibles mejoras
- Añadir transacciones reales con **commit/rollback**  
- Implementar **Savepoints**  
- Migración incremental (solo datos nuevos)  
- Comprobación automática de consistencia  
