# Entrega-Final-SQL-54910

<center>
<img src="https://objetivo.news/download/multimedia.normal.bcba10cea1861629.Y29kZXJob3VzZS1xdWUtZXMtcXVlLWhhY2VuX25vcm1hbC53ZWJw.webp" style="width: 100% ; aspect-ratio:16/9">
</center>


# <center>Entrega de proyecto final</center>
Alumno: Franco Carucci

Comision: SQL 54910

Tutor: Nancy Elizabeth Villena Reines

Docente: Anderson Michel Torres

---

## Tematica del proyecto

El equipo del proyecto Huellitas es una iniciativa centrada en facilitar y promover la adopción responsable de animales, con un enfoque en mejorar el bienestar de mascotas en situación de refugio. 

Este sistema permite gestionar toda la información relacionada con el proceso de adopción de manera organizada y accesible, ayudando a los refugios y organizaciones de rescate animal a conectar mascotas en espera de un hogar con adoptantes potenciales.


## Modelo de negocio

El modelo de negocio de Huellitas en la ciudad de La Plata se centra en la recepción, cuidado,
tratamiento y adopción de mascotas sin hogar.

Este refugio tiene como objetivo principal proporcionar un hogar temporal a las mascotasdesamparadas, brindarles la atención veterinaria necesaria y facilitar su adopción a personas responsables y dispuestas a cuidarlas. Teniendo en cuenta todas las partes influyentes en este proceso, desde las mascotas y adoptantes hasta los otros refugios y proveedores de insumos

El objetivo es agilizar el dia a dia del refugio, ordenar sus datos y facilitar la toma de decisiones para la
supervivencia de la organizacion a lo largo del tiempo


## Diagrama entidad relacion (DER)


<img
src="https://github.com/FrancoCarucci/Entrega-Final-Franco-Carucci/blob/main/DER_Huellitas.png">


## Listado de tablas y descripcion
<img
src="https://github.com/FrancoCarucci/Entrega-Final-Franco-Carucci/blob/main/Esquema_huellitas.png">

## Tabla de Datos
<img
src="https://github.com/FrancoCarucci/Entrega-Final-Franco-Carucci/blob/main/HUELLITAS_Tabla.png">

## Estructura e ingesta de datos
* Se realiza principalmente por medio del archivo population.sql
  
## *OBJETOS*

## Funciones

### `fn_adoptada()`
Esta función sirve para devolver un valor (por ejemplo, 1 para indicar que la mascota está adoptada y 0 si no lo está) en función de si hay un registro en la tabla de adopciones correspondiente a una mascota específica.

**Ejemplo de uso:**
```sql
SELECT fn_adoptada(20) AS mascota_adoptada;
```
En este ejemplo comprobamos que la mascota todavia no fue adoptada

### `fn_ultima_adopcion()`
Esta funcion fue diseñada para devolver la fecha de la última adopción realizada por un adoptante específico..

**Ejemplo de uso:**
```sql
SELECT fn_ultima_adopcion(3) AS ultima_adopcion;
```
En este ejemplo vemos como el adoptante (3) realizo la ultima adopcion el 2024-11-01

## Vistas

### `vista_mascotas_sin_adoptar`
Si existe una tabla de mascotas que no han sido adoptadas, esta vista muestra las mascotas que aún no han sido adoptadas.

**Ejemplo de uso:**
```sql
SELECT * FROM vista_mascotas_sin_adoptar;
```

### `vista_adopciones_complejas`
Esta vista combina información de adoptantes, colaboradores y mascotas para mostrar un resumen completo de las adopciones.

**Ejemplo de uso:**
```sql
SELECT *
FROM vista_adopciones_compleja
WHERE fecha_adopcion >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH);
```

## Procedimientos

## `sp_actualizar_colaborador()`
Actualiza los datos de un colaborador

**Parámetros:**
- `id_colaborador`
- `nombre`
- `apellido`
- `fecha_nacimiento`
- `domicilio`
- `telefono`
- `correo_electronico`

**Ejemplo de uso:**
```sql
CALL sp_actualizar_colaborador(1, 'Juan', 'Pérez', '1980-05-12', 'Calle Nueva 456', '5559876543', 'juan.perez@example.com');
```

### `sp_registrar_adopcion()`
Este procedimiento se utiliza para registrar una nueva adopcion

**Parámetros:**
- `id_adoptante`
- `id_colaborador`
- `id_mascota`
- `fecha`

**Ejemplo de uso:**
```sql
CALL sp_registrar_adopcion(1, 2, 3, '2024-10-10');
```

## Triggers

### `before_insert_colaborador`
Este trigger valida que los colaboradores tengan al menos 18 años antes de ser insertados en la base de datos.

**Ejemplo de uso:**
```sql
INSERT INTO colaboradores (nombre, apellido, fecha_nacimiento, domicilio, telefono, correo_electronico)
VALUES ('Juan', 'Pérez', '2010-05-15', 'Av. Juventud 123', '5551234567', 'juan.perez@example.com');
```

### `antes_de_borrar_mascota`
Este trigger se activa antes de que se elimine una mascota. Si la mascota ha sido adoptada, se impide la eliminación.

**Ejemplo de uso:**
```sql
DELETE FROM mascotas WHERE id_mascota = 8;
```

## Transacciones

Actualización de Historial Médico de una Mascota con Verificación
```sql
INSERT INTO HISTORIAL_SALUD (mascota_id, fecha_consulta, diagnostico, tratamiento)
VALUES (7, CURDATE(), 'Resfriado', 'Descanso y líquidos');

IF ROW_COUNT() > 0 THEN
    UPDATE MASCOTA
    SET estado_salud = 'en tratamiento'
    WHERE mascota_id = 7;
```
Usando COMMIT agrego los cambios, de lo contrario, con ROLLBACK, se deshacen

## Roles y permisos
`[./objects/roles_users.sql](https://github.com/FrancoCarucci/Entrega-Final-Franco-Carucci/blob/main/roles/roles.sql)`

Se generan tres roles:

1. `role_select_vistas`: Puede realizar solo consultas (SELECT) en las vistas vista_adopciones_completas y vista_mascotas_sin_adoptar.
2. `role_crud_mascotas`: Puede realizar todas las operaciones (ALL PRIVILEGES) en las tablas mascotas y razas.
3. `role_crud_adopciones`: Puede realizar todas las operaciones en la tabla adopciones.

Además, debido al tamaño de la organizacion solo se crea un usuario por cada rol y les asigna los roles correspondientes.

```
