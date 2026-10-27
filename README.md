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
* 🔹Eliminación de dependencias transitivas en Proveedores: Los datos del distribuidor (NOMBRE_DISTRIBUIDOR, CELULAR) dependían funcionalmente del proveedor y no directamente del código del producto. Al separarlos en la entidad independiente PROVEEDOR, se eliminó la dependencia transitiva, dejando en PRODUCTO únicamente la clave foránea ID_PROVEEDOR.
* 🔹Eliminación de dependencias transitivas en Clientes y Empleados: Atributos como el nombre completo o celular de un cliente o empleado no deben depender de la transacción (ID_VENTA). Al aislarlos en sus respectivas tablas (CLIENTE y EMPLEADO), se asegura que dichos datos dependan exclusivamente de sus propias claves primarias (ID_CLIENTE e ID_empleado), evitando redundancias y anomalías de actualización.
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
