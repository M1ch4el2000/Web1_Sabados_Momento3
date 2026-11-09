# Base de datos de reclutamiento del UNSC

Esta es la base de datos de nuestro proyecto de reclutamiento del UNSC. Guarda los reclutas, las ramas, las unidades, los centros de reclutamiento y las evaluaciones para entrar a las divisiones ONI y ODST.

## Datos técnicos

- Motor: SQL Server
- Nombre de la base: `reclutamiento`
- Tablas: 6
- Procedimientos almacenados: 2

## Diccionario de datos

**ramas_unsc** (ramas y cuerpos del UNSC)
- `id`: número de la rama, se genera solo
- `nombre`: nombre de la rama, no se repite
- `descripcion`: qué hace la rama

**divisiones_especiales** (ONI y ODST)
- `id`: número de la división
- `nombre`: nombre de la división, no se repite
- `tipo`: solo puede ser ONI u ODST
- `cupo_anual`: cuántos reclutas recibe al año, por defecto 10

**unsc** (unidades militares)
- `id`: número de la unidad
- `nombre_unidad`: nombre de la unidad, no se repite
- `base`: dónde opera, por defecto "No se asigno una base"
- `ramas_unsc_id`: rama a la que pertenece

**entidad** (centros de reclutamiento)
- `id`: número del centro
- `nombre`: nombre del centro, no se repite
- `ciudad`: ciudad donde queda
- `unsc_id`: unidad de la que depende

**recluta**
- `id`: número del recluta
- `nombre` y `apellido`
- `edad`: mínimo 18
- `peso`: en kilos
- `altura`: en metros
- `ramas_unsc_id`: rama a la que aspira
- `fecha_registro`: cuándo se inscribió, por defecto la fecha de hoy
- `estado`: En evaluacion, Apto o No apto. Por defecto "En evaluacion"
- `entidad_id`: centro donde se inscribió

**apto_division_especial** (evaluaciones)
- `id`: número de la evaluación
- `apto_servicio`: 1 si fue apto, 0 si no
- `recluta_id`: recluta evaluado
- `divisiones_especiales_id`: división para la que se evaluó
- `fecha_evaluacion`: cuándo se evaluó, por defecto la fecha de hoy
- `puntaje`: nota de 0 a 100

Las tablas se relacionan así: una rama tiene varias unidades y varios reclutas, una unidad tiene varios centros de reclutamiento y cada centro recibe reclutas. Un recluta puede tener varias evaluaciones, y por eso hicimos la tabla `apto_division_especial`, que une a los reclutas con las divisiones.

## Relación con los otros submódulos

El proyecto tiene tres partes que entregamos en materias distintas: esta base de datos, el backend en Java (Programación Orientada a Objetos) y el frontend en HTML, CSS y JavaScript. Todavía no están conectadas, eso lo vamos a hacer en el tercer semestre. Por ahora cada una funciona por separado, pero las tres parten del mismo tema.

Las clases de Java se parecen a las tablas: la clase `Recluta` tendría los mismos datos que la tabla `recluta`, y así con las demás. Lo que en Java es un objeto, aquí es una fila.

Las pantallas del front usarían estos datos. El formulario de inscripción llenaría la tabla `recluta` (sería como llamar a `registrar_recluta`) y la pantalla para consultar a un recluta mostraría lo que devuelve `estado_recluta_report`. Cuando se conecten, el front le pasaría los datos al backend en Java y este se encargaría de hablar con la base de datos.

## Cómo usar el script

Abrir el `.sql` en SQL Server Management Studio y ejecutarlo completo en un servidor donde no exista la base `reclutamiento`, porque el script la crea.
