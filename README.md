# Sistema de Registro de Ventas - Ferretería "Quique"

Repositorio para la gestión del diseño de base de datos, estructurado a partir del análisis de requerimientos y la entrevista inicial con el propietario.

## Descripción del Proyecto
Este proyecto modela el sistema de control de ventas, inventario y proveedores para la ferretería "Quique", abarcando el flujo operativo desde la atención de vecinos y maestros albañiles hasta el registro de transacciones.

## Entrevista de Requerimientos
Puedes revisar la redacción completa de la entrevista realizada en el archivo [entrevista.md](./entrevista.md).

## Diagrama Entidad-Relación (DER)
A continuación se presenta el modelo conceptual Entidad-Relación del sistema:

![Diagrama Entidad Relacion](./TAREA%20FERRETERIA.drawio.png)

## Proceso de Normalización (1FN, 2FN, 3FN)
### Primera Forma Normal (1FN):
> **Objetivo:** Garantizar que cada atributo contenga un solo valor atómico y no existan listas o valores multivaluados en una sola celda.
**Acciones realizadas:** Se eliminaron los campos compuestos o repetitivos en las facturas de compra (como listas de varios productos dentro de una sola venta).
* 🔹 Se separaron las notas de venta en registros individuales mediante la relación entre VENTA y PRODUCTO, creando una estructura de detalle (CONTIENE) donde cada fila registra un único producto con su CANTIDAD, PRECIO_UNITARIO y SUBTOTAL.
* 🔹 Se aseguró que los campos de identificación (ID_CLIENTE, ID_EMPLEADO, ID_VENTA, ID_PRODUCTO, ID_PROVEEDOR) sean claves primarias únicas y atómicas.
### Segunda Forma Normal (2FN):
> **Objetivo:** Garantizar que el esquema esté en 1FN y que todos los atributos que no forman parte de la clave primaria dependan de toda la clave primaria y no de solo una parte de ella (en claves compuestas).
**Acciones realizadas:**
* 🔹En la relación de detalle entre VENTA y PRODUCTO (relación N:M que genera la tabla intermedia Detalle_Venta), los atributos CANTIDAD, PRECIO_UNITARIO y SUBTOTAL dependen directamente de la combinación de (ID_VENTA, ID_PRODUCTO).
* 🔹 Datos propios del producto (como NOMBRE, STOCK_ACTUAL y PRECIO_AL_MENOR) se mantuvieron de manera independiente dentro de la entidad PRODUCTO, evitando que dependan parcialmente de la transacción de venta.
### Tercera Forma Normal (3FN):
* 🔹Proveedores: Se separaron los datos del proveedor (ID_PROVEEDOR, NOMBRE_DISTRIBUIDOR, CELULAR) en la entidad PROVEEDOR, evitando registrar datos del distribuidor dentro de cada producto individual. La entidad PRODUCTO solo guarda la referencia clave ID_PROVEEDOR y el PRECIO_ULTIMA_COMPRA.
* 🔹Clientes y Empleados: Los datos personales como NOMBRE_COMPLETO o CELULAR dependen directamente de ID_CLIENTE o ID_EMPLEADO, y no de ID_VENTA. La tabla VENTA únicamente relaciona las claves foráneas correspondientes.

```text
CLIENTE                 VENTA                       DETALLE_VENTA                 PRODUCTO                 PROVEEDOR
+------------------+    +----------------------+    +--------------------+        +--------------------+   +--------------------+
| ID_CLIENTE  {PK} |    | ID_VENTA        {PK} |    | ID_VENTA   {PK, FK}|        | ID_PRODUCTO   {PK} |   | ID_PROVEEDOR  {PK} |
| NOMBRE_COMPLETO  | 1--*| FECHA_HORA           | 1--*| ID_PRODUCTO{PK, FK}| *-----1| NOMBRE             | *-1| NOMBRE_DISTRIB.    |
| CELULAR          |    | MONTO_TOTAL          |    | CANTIDAD           |        | PRECIO_AL_MENOR    |   | CELULAR            |
+------------------+    | TIPO_PAGO            |    | PRECIO_UNITARIO    |        | STOCK_ACTUAL       |   +--------------------+
                        | ID_CLIENTE       {FK}|    | SUBTOTAL           |        | PRECIO_ULT_COMPRA  |
                        | ID_EMPLEADO      {FK}|    +--------------------+        | ID_PROVEEDOR   {FK}|
                        +----------------------+                                  +--------------------+
                                   |
                                   *
                                   |
                                   1
                        +----------------------+
                        | EMPLEADO             |
                        +----------------------+
                        | ID_EMPLEADO     {PK} |
                        | NOMBRE_EMPLEADO      |
                        +----------------------+
