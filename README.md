¿Por qué usaste LEFT JOIN para la Consulta 1 y no INNER JOIN? ¿Qué se perdería si usaras INNER JOIN?

R:

Utilicé LEFT JOIN porque la consulta busca mostrar todos los productos del catálogo, incluso aquellos que nunca fueron vendidos. Al colocar la tabla productos a la izquierda del JOIN, SQL devuelve todos los registros de esa tabla y agrega la información de ventas cuando existe una coincidencia.

Si hubiera utilizado INNER JOIN, solo aparecerían los productos que tienen al menos una venta registrada. En este caso se perderían los productos 108 (Hub USB-C 7p) y 109 (Parlante Bluetooth), ya que no tienen registros asociados en la tabla ventas.

Por lo tanto, INNER JOIN no permitiría responder correctamente la pregunta de negocio sobre qué productos nunca fueron vendidos.

¿Por qué usaste RIGHT JOIN para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?

R:

Utilicé RIGHT JOIN porque necesitaba conservar todas las filas de la tabla ventas, incluso aquellas que no tienen un producto válido en el catálogo.

En la consulta, la tabla productos está a la izquierda y la tabla ventas está a la derecha:

FROM productos p
RIGHT JOIN ventas v
ON p.producto_id = v.producto_id

El RIGHT JOIN garantiza que todas las ventas aparezcan en el resultado. De esta forma es posible detectar registros huérfanos, es decir, ventas cuyos productos no existen en la tabla de productos.

Gracias a esto se identifica la venta con producto_id = 999, que no tiene correspondencia en el catálogo.

¿Qué representan los valores NULL en cada resultado? Explicá con un ejemplo concreto de los datos qué significa que venta_id sea NULL en la Consulta 1 y que producto_id de productos sea NULL en la Consulta 2.

R:

Los valores NULL indican que no existe una coincidencia entre las tablas para esa fila.

Consulta 1 (LEFT JOIN)

Cuando venta_id aparece como NULL, significa que el producto existe en el catálogo pero nunca fue vendido.

Por ejemplo, los productos:

Hub USB-C 7p (producto_id 108)
Parlante Bluetooth (producto_id 109)

aparecen con venta_id = NULL porque no tienen registros relacionados en la tabla ventas.

Consulta 2 (RIGHT JOIN)

Cuando las columnas provenientes de la tabla productos aparecen como NULL, significa que existe una venta cuyo producto no está registrado en el catálogo.

Por ejemplo, la venta:

venta_id = 10
producto_id = 999

aparece con nombre = NULL y categoria = NULL porque el producto 999 no existe en la tabla productos.

¿Cuándo usarías FULL OUTER JOIN en un caso real de negocio?

R:

Utilizaría FULL OUTER JOIN cuando necesite realizar una auditoría completa de la información y no quiera perder registros de ninguna de las dos tablas.

Por ejemplo, en una empresa de comercio electrónico podría comparar el catálogo de productos con las ventas registradas para detectar simultáneamente:

Productos que existen pero nunca se han vendido.
Ventas registradas con códigos de producto incorrectos o inexistentes.

En este ejercicio, el FULL OUTER JOIN permite visualizar al mismo tiempo los productos 108 y 109 sin ventas, y la venta con producto_id = 999 que no existe en el catálogo. Esto facilita identificar inconsistencias y problemas de calidad de datos.
