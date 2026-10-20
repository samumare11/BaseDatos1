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
Objetivo: Garantizar que cada atributo contenga un solo valor atómico y no existan listas o valores multivaluados en una sola celda
Acciones realizadas: Se eliminaron los campos compuestos o repetitivos en los tickets/facturas de compra (como listas de varios productos dentro de una sola venta).
Se separaron las notas de venta en registros individuales mediante la relación entre VENTA y PRODUCTO, creando una estructura de detalle (CONTIENE) donde cada fila registra un único producto con su CANTIDAD, PRECIO_UNITARIO y SUBTOTAL.
Se aseguró que los campos de identificación (ID_CLIENTE, ID_EMPLEADO, ID_VENTA, ID_PRODUCTO, ID_PROVEEDOR) sean claves primarias únicas y atómicas.
