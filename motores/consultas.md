# 1. videncia de los registros de cada tabla

## 1.1 Registros de la tabla products

``` sql
SELECT * FROM SuministroPro.products;
```

![](images/clipboard-2348138003.png)

## 1.2 Registro de la tabla suppliers

``` sql
SELECT * FROM SuministroPro.suppliers ;
```

![](images/clipboard-3265339394.png)

## 1.3 Registro de la tabla purchases

``` sql
SELECT * FROM SuministroPro.purchases;
```

![](images/clipboard-2551854913.png)

## 1.4 Registro de la tabla purchase_details

``` sql
SELECT * FROM SuministroPro.purchase_details;
```

![](images/clipboard-556282369.png)

## 1.5 Registro de la tabla inventories

``` sql
SELECT * FROM SuministroPro.inventories;
```

![](images/clipboard-2254988930.png)

## 1.6 Registro de la tabla customers

``` sql
SELECT * FROM SuministroPro.customers;
```

![](images/clipboard-4136100851.png)

## 1.7 Registro de la tabla sales

``` sql
SELECT * FROM SuministroPro.sales;
```

![](images/clipboard-415449818.png)

## 1.8 Registro de la tabla sale_details

``` sql
SELECT * FROM SuministroPro.sale_details;
```

![](images/clipboard-587043589.png)

## 1.9 Registro de la tabla accounts_receivable

``` sql
SELECT * FROM SuministroPro.accounts_receivable;
```

![](images/clipboard-2546185796.png)

## 1.10 Registro de la tabla receivable_payments

``` sql
SELECT * FROM SuministroPro.receivable_payments;
```

![](images/clipboard-528738911.png)

## 1.11 Registro de la tabla returns

``` sql
SELECT * FROM SuministroPro.returns;
```

![](images/clipboard-2385880107.png)

# 2. Consultas avanzadas e implementación de tiggers en MySQL:

## 2.1 Mostrar algunos de los registros de la tabla customers

Para este punto quise mostrar los datos básicos de los clientes, como su nombre completo, el tipo y número de documento, y si están activos o no en el sistema. Opté por hacer la consulta especificando exactamente esos campos en lugar de traer toda la tabla entera, porque así es más directo, se ve solo la información relevante que se necesita revisar y no cargamos la base de datos de manera innecesaria.

``` sql
SELECT full_name, document_type, document_number, status FROM customers;
```

![](images/clipboard-1776810327.png)

## Stored Procedure

![](images/clipboard-2809681513.png)

![](images/clipboard-3825976838.png)

## 2.2 Mostrar de forma ordenada (DESC) las ventas desde su comienzo

En esta consulta quise revisar el historial de ventas para ver primero las más recientes y de ahí ir bajando hacia las más antiguas. Para eso seleccioné el id de la venta, la fecha, el total y el estado, y opté por usar ORDER BY con la fecha de forma descendente (DESC) para que la información quede organizada cronológicamente comenzando por las últimas transacciones realizadas.

``` sql
SELECT id, sale_date, total, status FROM sales ORDER BY sale_date DESC;
```

![](images/clipboard-2696830787.png)

## Stored Procedure

![](images/clipboard-344682806.png)

![](images/clipboard-1277882468.png)

## 2.3 Consultas a múltiples tablas mediante WHERE

En este punto quise relacionar las ventas con los clientes que realizaron cada compra. Para lograr esto, consulté las tablas sales y customers al mismo tiempo, utilizando alias (s y c) para simplificar el código y filtrando con la cláusula WHERE para que solo se crucen los registros donde el ID del cliente coincida en ambas tablas (c.id = s.customer_id).

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id;
```

![![](images/clipboard-3637097975.png)](images/clipboard-3237375928.png)

![](images/clipboard-691916665.png)

## Stored Procedure

![](images/clipboard-3945838195.png)

![](images/clipboard-3021423229.png)

## 2.4 Consultas a múltiples tablas mediante JOIN

Para este punto utilicé la cláusula JOIN para relacionar la tabla de clientes con la tabla de ventas de forma explícita y estándar en SQL. Seleccioné el nombre completo y el correo del cliente junto con los datos de sus ventas asociadas, uniendo ambas tablas a través de la coincidencia entre el ID del cliente y la clave foránea en ventas.

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id);
```

![](images/clipboard-3465154707.png)

![](images/clipboard-2245255808.png)

## Stored Procedure

![](images/clipboard-3046561871.png)

![](images/clipboard-1629027885.png)

![](images/clipboard-2165228402.png)

## 2.5 Condiciones en las Consultas o filtros en las Consultas

Esta consulta responde a la necesidad del negocio de monitorear únicamente las operaciones vigentes en el sistema. Se aplicó el filtro por estado activo para aislar las transacciones efectivas y descartar aquellas que fueron canceladas o están inactivas, garantizando que los reportes reflejen el flujo real de ventas sin distorsionar los totales.

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id AND s.status = 'active';
```

![![](images/clipboard-1992643145.png)](images/clipboard-781554155.png)

![](images/clipboard-3285347994.png)

## Stored Procedure

![](images/clipboard-592714543.png)

![](images/clipboard-2118971499.png)

![](images/clipboard-2994454127.png)

![](images/clipboard-3536888877.png)

![](images/clipboard-3147407247.png)

## forma 2

El propósito de esta consulta es cruzar la información de los clientes con el historial de sus compras haciendo uso de la sintaxis estándar JOIN, optimizando así el rendimiento de la base de datos frente a consultas tradicionales. La inclusión del filtro en el estado permite auditar o analizar específicamente las operaciones inactivas sin traer datos innecesarios a la memoria.

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id) 
WHERE s.status = 'inactive';
```

![![](images/clipboard-2747926933.png)](images/clipboard-2024714984.png)

## Stored Procedure

![](images/clipboard-1358458763.png)

![](images/clipboard-286643677.png)

![](images/clipboard-3319044300.png)

# 2.6 Consultas con filtros condicional LIKE

Esta consulta atiende la necesidad de realizar búsquedas parciales o coincidencias de texto dentro del módulo de administración de clientes. El uso de la cláusula LIKE permite buscar registros basándose en patrones de texto en lugar de valores exactos, facilitando la localización de usuarios cuando el usuario o el sistema solo cuenta con el inicio de la dirección de correo electrónico.

``` sql
SELECT * 
FROM customers AS c 
WHERE c.email LIKE 'm%';
```

![](images/clipboard-2547520077.png)

## **Mostrar todos los correos de los clientes que contengan el dominio** jemplo

``` sql
SELECT *
FROM customers AS c
WHERE c.email LIKE CONCAT('%', 'ejemplo.com', '%');
```

## ![](images/clipboard-3712172.png)

## Combinación del punto 1.5 y la implementación del LIKE

``` sql
SELECT 
    c.full_name, 
    c.email, 
    s.*
FROM customers AS c
JOIN sales AS s 
    ON c.id = s.customer_id
WHERE s.status = 'inactive'
  AND c.email LIKE '%@ejemplo.com';
```

![](images/clipboard-3283914937.png)

## Stored Procedure

![](images/clipboard-3621708952.png)

![](images/clipboard-1863319425.png)

![](images/clipboard-1465153308.png)

## 2.7 Consultas con filtros condicionales BETWEEN

Esta consulta resuelve el requerimiento de generar reportes consolidados dentro de un rango cronológico específico. Integrando datos de clientes, ventas, cuentas por cobrar, detalle de ventas y productos, se logra una trazabilidad completa de la operación comercial. La condición BETWEEN delimita el análisis a un periodo de tiempo determinado, mientras que el ordenamiento ascendente por fecha de venta facilita la lectura cronológica de las transacciones.

``` sql
SELECT 
    c.full_name, c.email, s.sale_date, s.status, ar.issue_date, p.sku
FROM customers c
JOIN sales s 
    ON c.id = s.customer_id
JOIN accounts_receivable ar 
    ON s.id = ar.sale_id
JOIN sale_details sd 
    ON s.id = sd.sale_id
JOIN products p ON p.id = sd.product_id
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
ORDER BY s.sale_date ASC;
```

![![](images/clipboard-5278111.png)](images/clipboard-3672346416.png)

## Forma 2:

``` sql
SELECT c.full_name, c.email, s.sale_date, s.status, ar.issue_date, p.sku
FROM customers c, sales s, accounts_receivable ar, sale_details sd, products p
WHERE c.id = s.customer_id
  AND s.id = ar.sale_id
  AND s.id = sd.sale_id
  AND p.id = sd.product_id
  AND s.sale_date BETWEEN '2026-08-15 00:00:00' 
                      AND '2026-09-17 23:59:59'
ORDER BY s.sale_date ASC;
```

![![](images/clipboard-1310587781.png)](images/clipboard-2233497195.png)

## Stored Procedure

![](images/clipboard-2363389419.png)

## ![](images/clipboard-153945368.png)

![](images/clipboard-1374585114.png)

## 2.8 Consultas con agrupamiento GROUP BY

En esta parte agrupé la información para sacar resúmenes de ventas por cliente, probando dos formas distintas de filtrar los datos según lo que se necesite consultar. La primera forma usa WHERE para limitar las ventas a unas fechas específicas antes de agruparlas, lo cual ayuda a que la base de datos no trabaje de más procesando información vieja. La segunda forma usa HAVING para filtrar los resultados después de hacer las sumas, lo que sirve para buscar únicamente a los clientes que hayan comprado bastante y superen un monto determinado. Combinar ambas ideas nos permite generar reportes de ventas rápidos, ordenados y enfocados en los clientes más importantes para el negocio.

## **Forma 1 con el WHERE:**

``` sql
SELECT 
    c.id, 
    c.full_name, 
    SUM(s.total) AS TotalSuma, 
    COUNT(s.id) AS CuentaTotal, 
    AVG(s.total) AS Promedio 
FROM customers AS c 
JOIN sales AS s 
    ON c.id = s.customer_id 
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' 
                      AND '2026-09-17 23:59:59' 
GROUP BY c.id, c.full_name 
ORDER BY TotalSuma DESC;
```

![![](images/clipboard-4186742505.png)](images/clipboard-621797528.png)

## Forma 2 con el HAVING:

``` sql
SELECT c.id, c.full_name, SUM(s.total) AS TotalSuma, AVG(s.total) AS PromedioVenta 
FROM customers AS c 
JOIN sales AS s ON c.id = s.customer_id  
GROUP BY c.id, c.full_name 
HAVING SUM(s.total) >= 100  
ORDER BY TotalSuma DESC;
```

![![](images/clipboard-3793654666.png)](images/clipboard-1902348430.png)

## Stored Procedure

![](images/clipboard-3671814137.png)

![](images/clipboard-1685933665.png)

## ![](images/clipboard-3979654757.png)

## 2.9 Subconsultas y teoría de conjuntos

Aquí busqué identificar a los clientes inactivos o que no han realizado compras dentro de un rango de fechas determinado, probando dos técnicas de la teoría de conjuntos: la subconsulta con NOT IN y la combinación con LEFT JOIN filtrando los valores nulos. Aunque ambas opciones entregan exactamente el mismo resultado, opté por la estructura con LEFT JOIN para el procedimiento almacenado porque el motor de la base de datos suele procesarla con mayor rapidez al cruzar los registros de forma directa. Esta consulta nos sirve a nivel de negocio para detectar clientes ausentes y crear campañas de reactivación o mercadeo enfocadas en ellos.

``` sql
SELECT * 
FROM customers AS c 
WHERE c.id NOT IN (
    SELECT s.customer_id 
    FROM sales AS s 
    WHERE s.sale_date BETWEEN '2026-08-15' AND '2026-09-17'
);
```

![](images/clipboard-3692064337.png)

## Forma 2:

``` sql
SELECT * 
FROM customers AS c 
LEFT JOIN sales AS s 
    ON (
        c.id = s.customer_id 
        AND s.sale_date BETWEEN '2026-08-15' AND '2026-09-17'
    ) 
WHERE s.customer_id IS NULL;
```

![](images/clipboard-3969721277.png)

## Stored Procedure

![![](images/clipboard-1994097068.png)](images/clipboard-2115958117.png)

![](images/clipboard-1486329784.png)

# creacción de triggers en la tabla sales

Para la tabla de ventas (sales), creé un sistema de auditoría mediante triggers que registra automáticamente un historial en la tabla sales_audit ante cualquier evento de inserción (INSERT), modificación (UPDATE) o eliminación (DELETE). Estos registros guardan la información estructurada en formato JSON, lo que permite auditar el estado exacto de la venta antes y después de cada cambio.

## Despues de Insertar

``` {.sql .sq}
CREATE DEFINER=`admin`@`%` TRIGGER `ai_sales_audit` AFTER INSERT ON `sales` FOR EACH ROW BEGIN
  SET @from_sales_trigger = 1;

  INSERT INTO sales_audit (sale_id, actionSale, before_data, after_data)
  VALUES (
    NEW.id,
    'INSERT',
    NULL,
    JSON_OBJECT(
      'id', NEW.id,
      'customer_id', NEW.customer_id,
      'sale_date', NEW.sale_date,
      'subtotal', NEW.subtotal,
      'taxes', NEW.taxes,
      'total', NEW.total,
      'state', NEW.state,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );

  SET @from_sales_trigger = NULL;
END
```

![](images/clipboard-4251206590.png)

![](images/clipboard-3060444594.png)

## Despues de Actualizar

``` sql
CREATE DEFINER=`admin`@`%` TRIGGER `au_sales_audit` AFTER UPDATE ON `sales` FOR EACH ROW BEGIN
  SET @from_sales_trigger = 1;

  INSERT INTO sales_audit (sale_id, actionSale, before_data, after_data)
  VALUES (
    NEW.id,
    'UPDATE',
    JSON_OBJECT(
      'id', OLD.id,
      'customer_id', OLD.customer_id,
      'sale_date', OLD.sale_date,
      'subtotal', OLD.subtotal,
      'taxes', OLD.taxes,
      'total', OLD.total,
      'state', OLD.state,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    JSON_OBJECT(
      'id', NEW.id,
      'customer_id', NEW.customer_id,
      'sale_date', NEW.sale_date,
      'subtotal', NEW.subtotal,
      'taxes', NEW.taxes,
      'total', NEW.total,
      'state', NEW.state,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );

  SET @from_sales_trigger = NULL;
END
```

![![](images/clipboard-3837821089.png)](images/clipboard-3190720633.png)

## Despues de Eliminar

``` sql
CREATE DEFINER=`admin`@`%` TRIGGER `ad_sales_audit` AFTER DELETE ON `sales` FOR EACH ROW BEGIN
  SET @from_sales_trigger = 1;

  INSERT INTO sales_audit (sale_id, actionSale, before_data, after_data)
  VALUES (
    OLD.id,
    'DELETE',
    JSON_OBJECT(
      'id', OLD.id,
      'customer_id', OLD.customer_id,
      'sale_date', OLD.sale_date,
      'subtotal', OLD.subtotal,
      'taxes', OLD.taxes,
      'total', OLD.total,
      'state', OLD.state,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    NULL
  );

  SET @from_sales_trigger = NULL;
END
```

![](images/clipboard-2049648380.png)

![](images/clipboard-2769084825.png)

# evidencia de cuando inserto un nuevo dato

![](images/clipboard-809296386.png)

![](images/clipboard-2094333278.png)

# evidencia de cuando actualizo un nuevo dato

![![](images/clipboard-1583138793.png)](images/clipboard-806716718.png)

#### Podemos observar que el cambio que realizamos fue el del total que de estar en 119.00 lo actualizamos a 750.00 con estos datos podemos observar que nuestras tablas de auditorias estan funcionando correctamente

# creacción de triggers en la tabla products

Para la tabla de productos (products), implementé un sistema de auditoría con triggers que registra de forma automática los eventos de inserción (INSERT), actualización (UPDATE) y eliminación (DELETE) en la tabla products_audit. La información se almacena en formato JSON, permitiendo rastrear el estado previo y posterior de datos críticos como precios, descripciones y estado del producto.

## Despues de Insertar

``` sql
CREATE TRIGGER SuministroPro.ai_products_audit
AFTER INSERT ON SuministroPro.products
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.products_audit (product_id, actionProduct, before_data, after_data)
  VALUES (
    NEW.id,
    'INSERT',
    NULL,
    JSON_OBJECT(
      'id', NEW.id,
      'sku', NEW.sku,
      'name', NEW.name,
      'description', NEW.description,
      'price', NEW.price,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );
END
```

![](images/clipboard-1616791812.png)

![](images/clipboard-2266530656.png)

## Despues de Actualizar

``` sql
CREATE TRIGGER SuministroPro.au_products_audit
AFTER UPDATE ON SuministroPro.products
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.products_audit (product_id, actionProduct, before_data, after_data)
  VALUES (
    NEW.id,
    'UPDATE',
    JSON_OBJECT(
      'id', OLD.id,
      'sku', OLD.sku,
      'name', OLD.name,
      'description', OLD.description,
      'price', OLD.price,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    JSON_OBJECT(
      'id', NEW.id,
      'sku', NEW.sku,
      'name', NEW.name,
      'description', NEW.description,
      'price', NEW.price,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );
END
```

![![](images/clipboard-2387652079.png)](images/clipboard-3057459020.png)

## Despues de Eliminar

``` sql
CREATE TRIGGER SuministroPro.ad_products_audit
AFTER DELETE ON SuministroPro.products
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.products_audit (product_id, actionProduct, before_data, after_data)
  VALUES (
    OLD.id,
    'DELETE',
    JSON_OBJECT(
      'id', OLD.id,
      'sku', OLD.sku,
      'name', OLD.name,
      'description', OLD.description,
      'price', OLD.price,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    NULL
  );
END
```

![![](images/clipboard-3246114954.png)](images/clipboard-4246723582.png)

# evidencia de cuando actualizo un nuevo dato

![](images/clipboard-914768930.png)

![![](images/clipboard-2609256876.png)](images/clipboard-1202444481.png)

#### podemos observar que al momento de actualizar el precio este nos muestra el valor que tenia ante con el que actualizamos que es de 1915.40 y lo actualizamos a 1950.00 por lo que podemos decir que nuestras tablas de auditorias funcionan perfectamente

# creacción de triggers en la tabla purchases

Para la tabla de compras (purchases), implementé un sistema de auditoría basado en triggers que guarda automáticamente un historial en la tabla purchases_audit tras cada evento de inserción (INSERT), actualización (UPDATE) o eliminación (DELETE). La información se estructura en formato JSON para registrar los valores antiguos y nuevos de totales, fechas, proveedores y estados.

## Despues de Insertar

``` sql
CREATE TRIGGER SuministroPro.ai_purchases_audit
AFTER INSERT ON SuministroPro.purchases
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.purchases_audit (purchase_id, actionPurchase, before_data, after_data)
  VALUES (
    NEW.id,
    'INSERT',
    NULL,
    JSON_OBJECT(
      'id', NEW.id,
      'supplier_id', NEW.supplier_id,
      'purchase_date', NEW.purchase_date,
      'subtotal', NEW.subtotal,
      'taxes', NEW.taxes,
      'total', NEW.total,
      'state', NEW.state,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );
END
```

![![](images/clipboard-2201218102.png)](images/clipboard-1140758894.png)

## Despues de Actualizar

``` sql
CREATE TRIGGER SuministroPro.au_purchases_audit
AFTER UPDATE ON SuministroPro.purchases
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.purchases_audit (purchase_id, actionPurchase, before_data, after_data)
  VALUES (
    NEW.id,
    'UPDATE',
    JSON_OBJECT(
      'id', OLD.id,
      'supplier_id', OLD.supplier_id,
      'purchase_date', OLD.purchase_date,
      'subtotal', OLD.subtotal,
      'taxes', OLD.taxes,
      'total', OLD.total,
      'state', OLD.state,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    JSON_OBJECT(
      'id', NEW.id,
      'supplier_id', NEW.supplier_id,
      'purchase_date', NEW.purchase_date,
      'subtotal', NEW.subtotal,
      'taxes', NEW.taxes,
      'total', NEW.total,
      'state', NEW.state,
      'status', NEW.status,
      'created_at', NEW.created_at,
      'updated_at', NEW.updated_at
    )
  );
END
```

![![](images/clipboard-2462882651.png)](images/clipboard-500697553.png)

## Despues de Eliminar

``` sql
CREATE TRIGGER SuministroPro.ad_purchases_audit
AFTER DELETE ON SuministroPro.purchases
FOR EACH ROW
BEGIN
  INSERT INTO SuministroPro.purchases_audit (purchase_id, actionPurchase, before_data, after_data)
  VALUES (
    OLD.id,
    'DELETE',
    JSON_OBJECT(
      'id', OLD.id,
      'supplier_id', OLD.supplier_id,
      'purchase_date', OLD.purchase_date,
      'subtotal', OLD.subtotal,
      'taxes', OLD.taxes,
      'total', OLD.total,
      'state', OLD.state,
      'status', OLD.status,
      'created_at', OLD.created_at,
      'updated_at', OLD.updated_at
    ),
    NULL
  );
END
```

![![](images/clipboard-2733119888.png)](images/clipboard-1120456474.png)

# evidencia de cuando actualizo un nuevo dato

![](images/clipboard-965398622.png)

![![](images/clipboard-3495150069.png)](images/clipboard-3936170803.png)

#### podemos observar que el valor total se registro con exito antes y despues de actualizarlo ante de actualizarlo era de 114.86 y despues de actualizarlo fue de 800.00 por lo que podemos decir que nuestras tablas de auditoria funcionan correctamente

# conclusión

La implementación de las vistas, consultas avanzada y el sistema de auditoría mediante triggers fortalece la arquitectura de la base de datos de SuministroPro. Al registrar automáticamente las acciones en ventas, productos y compras mediante capturas en formato JSON, el sistema garantiza un control total, trazabilidad de cambios y seguridad financiera sin afectar el rendimiento ni la experiencia operativa.

# 3. Consultas avanzadas e implementación de tiggers en PostgreSQL

## 3.1 Mostrar algunos de los registros de la tabla customers

Para este punto quise mostrar los datos básicos de los clientes, como su nombre completo, el tipo y número de documento, y si están activos o no en el sistema. Opté por hacer la consulta especificando exactamente esos campos en lugar de traer toda la tabla entera, porque así es más directo, se ve solo la información relevante que se necesita revisar y no cargamos la base de datos de manera innecesaria.

``` sql
SELECT full_name, document_type, document_number, status FROM customers;
```

![](images/clipboard-30983778.png)

## Stored Procedure

![](images/clipboard-1027780791.png)

![](images/clipboard-249708663.png)

## 3.2 Mostrar de forma ordenada (DESC) las ventas desde su comienzo

En esta consulta quise revisar el historial de ventas para ver primero las más recientes y de ahí ir bajando hacia las más antiguas. Para eso seleccioné el id de la venta, la fecha, el total y el estado, y opté por usar ORDER BY con la fecha de forma descendente (DESC) para que la información quede organizada cronológicamente comenzando por las últimas transacciones realizadas.

``` sql
SELECT id, sale_date, total, status FROM sales ORDER BY sale_date DESC;
```

![](images/clipboard-2273550326.png)

## Stored Procedure

![![](images/clipboard-4284951093.png)](images/clipboard-196683627.png)

## 3.3 Consultas a múltiples tablas mediante WHERE

En este punto quise relacionar las ventas con los clientes que realizaron cada compra. Para lograr esto, consulté las tablas sales y customers al mismo tiempo, utilizando alias (s y c) para simplificar el código y filtrando con la cláusula WHERE para que solo se crucen los registros donde el ID del cliente coincida en ambas tablas (c.id = s.customer_id).

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id;
```

![![](images/clipboard-1991853356.png)](images/clipboard-2357805901.png)

![![](images/clipboard-1044929568.png)](images/clipboard-3895462131.png)

## Stored Procedure

![](images/clipboard-424473364.png)

![](images/clipboard-3345420570.png)

## 3.4 Consultas a múltiples tablas mediante JOIN

Para este punto utilicé la cláusula JOIN para relacionar la tabla de clientes con la tabla de ventas de forma explícita y estándar en SQL. Seleccioné el nombre completo y el correo del cliente junto con los datos de sus ventas asociadas, uniendo ambas tablas a través de la coincidencia entre el ID del cliente y la clave foránea en ventas.

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id)
WHERE c.id = 1;
```

## ![](images/clipboard-2454741921.png)

## Stored Procedure

![](images/clipboard-238543542.png)

![![](images/clipboard-3774317364.png)](images/clipboard-929203973.png)

## 3.5 Condiciones en las Consultas o filtros

Esta consulta responde a la necesidad del negocio de monitorear únicamente las operaciones vigentes en el sistema. Se aplicó el filtro por estado activo para aislar las transacciones efectivas y descartar aquellas que fueron canceladas o están inactivas, garantizando que los reportes reflejen el flujo real de ventas sin distorsionar los totales.

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id AND s.status = 'active';
```

![](images/clipboard-3675739435.png)

## Stored Procedure

![![](images/clipboard-4076071146.png)](images/clipboard-1528683312.png)

![](images/clipboard-3183932025.png)

## 3.6 Consultas con filtros condicional LIKE

Esta consulta atiende la necesidad de realizar búsquedas parciales o coincidencias de texto dentro del módulo de administración de clientes. El uso de la cláusula LIKE permite buscar registros basándose en patrones de texto en lugar de valores exactos, facilitando la localización de usuarios cuando el usuario o el sistema solo cuenta con el inicio de la dirección de correo electrónico.

``` sql
SELECT 
    c.full_name, 
    c.email, 
    s.*
FROM customers AS c
JOIN sales AS s 
    ON c.id = s.customer_id
WHERE s.status = 'inactive'
  AND c.email LIKE '%@ejemplo.com';
```

![![](images/clipboard-430045453.png)](images/clipboard-249442452.png)

## Stored Procedure

![![](images/clipboard-3345595788.png)](images/clipboard-71373370.png)

![](images/clipboard-3914762883.png)

## 3.7 Consultas con filtros condicionales BETWEEN

Esta consulta resuelve el requerimiento de generar reportes consolidados dentro de un rango cronológico específico. Integrando datos de clientes, ventas, cuentas por cobrar, detalle de ventas y productos, se logra una trazabilidad completa de la operación comercial. La condición BETWEEN delimita el análisis a un periodo de tiempo determinado, mientras que el ordenamiento ascendente por fecha de venta facilita la lectura cronológica de las transacciones.

``` sql
SELECT 
    c.full_name, c.email, s.sale_date, s.status, ar.issue_date, p.sku
FROM customers c
JOIN sales s 
    ON c.id = s.customer_id
JOIN accounts_receivable ar 
    ON s.id = ar.sale_id
JOIN sale_details sd 
    ON s.id = sd.sale_id
JOIN products p ON p.id = sd.product_id
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
ORDER BY s.sale_date ASC;
```

![](images/clipboard-1119281997.png)

## Stored Procedure

![](images/clipboard-2335957980.png)

## ![](images/clipboard-3085428548.png)

![](images/clipboard-2228068598.png)

## 3.8 Consultas con agrupamiento GROUP BY

En esta parte agrupé la información para sacar resúmenes de ventas por cliente, probando dos formas distintas de filtrar los datos según lo que se necesite consultar. La primera forma usa WHERE para limitar las ventas a unas fechas específicas antes de agruparlas. La segunda forma usa HAVING para filtrar los resultados después de hacer las sumas, lo que sirve para buscar únicamente a los clientes que superen un monto determinado.

``` sql
SELECT c.id, c.full_name, SUM(s.total) AS TotalSuma, AVG(s.total) AS PromedioVenta 
FROM customers AS c 
JOIN sales AS s ON c.id = s.customer_id  
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
GROUP BY c.id, c.full_name 
HAVING SUM(s.total) >= 100  
ORDER BY TotalSuma DESC;
```

![](images/clipboard-3823370572.png)

## Stored Procedure

![![](images/clipboard-1991228297.png)](images/clipboard-2789524828.png)

![](images/clipboard-544204902.png)

## 3.9 Subconsultas y teoría de conjuntos

Aquí busqué identificar a los clientes inactivos o que no han realizado compras dentro de un rango de fechas determinado, probando la combinación con LEFT JOIN filtrando los valores nulos para mayor eficiencia del motor de PostgreSQL. Esta consulta nos sirve a nivel de negocio para detectar clientes ausentes y crear campañas de reactivación.

``` sql
SELECT c.* 
FROM customers AS c 
LEFT JOIN sales AS s 
    ON (
        c.id = s.customer_id 
        AND s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
    ) 
WHERE s.customer_id IS NULL;
```

![](images/clipboard-2062352407.png)

## Stored Procedure

![](images/clipboard-2015244677.png)

## ![](images/clipboard-3913715626.png)

![](images/clipboard-82663239.png)

# creacción de triggers en la tabla sales

La tabla sales la usamos principalmente para llevar el control y el registro de todas las ventas que se van realizando en el sistema. Básicamente, aquí se guarda la cabecera de cada factura o transacción; es decir, cada registro cuenta con su propio identificador y se conecta con el cliente que hizo la compra por medio de su código respectivo. También nos permite almacenar la fecha exacta en la que se hizo la venta junto con sus respectivas marcas de auditoría para saber cuándo se creó o se modificó el registro. En cuanto a la parte de dinero, la tabla divide muy bien los montos registrando el subtotal, los impuestos calculados y el valor final o total a pagar. Por último, incluye campos especiales para manejar el estado y la situación actual de cada venta dentro del flujo del negocio.

## Despues de Insertar

``` sql
CREATE OR REPLACE FUNCTION process_sales_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    VALUES (NEW.id, 'INSERT', NULL, to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ai_sales_audit ON sales;

CREATE TRIGGER ai_sales_audit
AFTER INSERT ON sales
FOR EACH ROW EXECUTE FUNCTION process_sales_insert();
```

![![](images/clipboard-474224990.png)](images/clipboard-71048146.png)

## Despues de actualizar

``` sql
CREATE OR REPLACE FUNCTION process_sales_update()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    VALUES (NEW.id, 'UPDATE', to_jsonb(OLD), to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS au_sales_audit ON sales;

CREATE TRIGGER au_sales_audit
AFTER UPDATE ON sales
FOR EACH ROW EXECUTE FUNCTION process_sales_update();
```

![](images/clipboard-1402349565.png)

![](images/clipboard-3733705788.png)

## Despues de eliminar

``` sql
CREATE OR REPLACE FUNCTION process_sales_delete()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    VALUES (OLD.id, 'DELETE', to_jsonb(OLD), NULL);
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ad_sales_audit ON sales;

CREATE TRIGGER ad_sales_audit
AFTER DELETE ON sales
FOR EACH ROW EXECUTE FUNCTION process_sales_delete();
```

![](images/clipboard-432025639.png)

![](images/clipboard-2543774820.png)

# evidencia de cuando actualizo un nuevo dato

![](images/clipboard-3047595988.png)

![![](images/clipboard-1422623750.png)](images/clipboard-1031802441.png)

#### como podemos ver yo actualice el datos sumandole 10 al total el cual podemos observa en la imagen que la tabla de auditoria si nos mostro la actualizacion

# **creacción de triggers en la tabla products**

La tabla products la utilizamos para almacenar y administrar todo el catálogo de artículos o mercancías disponibles en el sistema. En ella se registra la información esencial de cada producto, comenzando por su identificador único y su código de referencia o SKU, los cuales nos permiten identificarlo de forma rápida. También guardamos el nombre comercial y una breve descripción para detallar las características del artículo. En el aspecto financiero, la tabla maneja el precio unitario del producto con precisión decimal, además de incluir un campo para controlar su estado actual dentro del inventario. Por último, cuenta con sus respectivas marcas de tiempo para llevar el control exacto de cuándo se dio de alta el producto o cuándo sufrió alguna modificación.

## **Despues de Insertar**

``` sql
CREATE OR REPLACE FUNCTION process_products_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    VALUES (NEW.id, 'INSERT', NULL, to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ai_products_audit ON products;

CREATE TRIGGER ai_products_audit
AFTER INSERT ON products
FOR EACH ROW EXECUTE FUNCTION process_products_insert();
```

![![](images/clipboard-275107231.png)](images/clipboard-4086618632.png)

## **Despues de Actualizar**

``` sql
CREATE OR REPLACE FUNCTION process_products_update()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    VALUES (NEW.id, 'UPDATE', to_jsonb(OLD), to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS au_products_audit ON products;

CREATE TRIGGER au_products_audit
AFTER UPDATE ON products
FOR EACH ROW EXECUTE FUNCTION process_products_update();
```

![![](images/clipboard-2233491984.png)](images/clipboard-3745442103.png)

## **Despues de Eliminar**

``` sql
CREATE OR REPLACE FUNCTION process_products_delete()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    VALUES (OLD.id, 'DELETE', to_jsonb(OLD), NULL);
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ad_products_audit ON products;

CREATE TRIGGER ad_products_audit
AFTER DELETE ON products
FOR EACH ROW EXECUTE FUNCTION process_products_delete();
```

![![](images/clipboard-2583236790.png)](images/clipboard-3625818161.png)

# **evidencia de cuando actualizo un nuevo dato**

![](images/clipboard-1129619761.png)

![](images/clipboard-2289294590.png)

![](images/clipboard-3300741603.png)

#### podemos observar que en la actualizacion le sumamos 5.00 al total y lo cual se ve reflejado en nuestra tabla de auditoria

# **creacción de triggers en la tabla purchases**

La tabla purchases se encarga de registrar y administrar todas las operaciones de abastecimiento o compras de mercancía realizadas a los diferentes proveedores del sistema. En su estructura principal se almacena el identificador único de la compra junto con el código del proveedor asociado, permitiendo un control exacto de quién nos suministró los productos. Asimismo, la tabla maneja la fecha de la transacción y los valores financieros detallados, tales como el subtotal, los impuestos calculados y el valor total a pagar. Cuenta también con campos para gestionar el estado administrativo y las condiciones de la orden, acompañados de sus respectivas marcas de tiempo para asegurar una trazabilidad completa desde su registro inicial hasta cualquier modificación posterior.

## **Despues de Insertar**

``` sql
CREATE OR REPLACE FUNCTION process_purchases_insert()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    VALUES (NEW.id, 'INSERT', NULL, to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ai_purchases_audit ON purchases;

CREATE TRIGGER ai_purchases_audit
AFTER INSERT ON purchases
FOR EACH ROW EXECUTE FUNCTION process_purchases_insert();
```

![![](images/clipboard-4207308227.png)](images/clipboard-143161497.png)

## **Despues de Actualizar**

``` sql
CREATE OR REPLACE FUNCTION process_purchases_update()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    VALUES (NEW.id, 'UPDATE', to_jsonb(OLD), to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS au_purchases_audit ON purchases;

CREATE TRIGGER au_purchases_audit
AFTER UPDATE ON purchases
FOR EACH ROW EXECUTE FUNCTION process_purchases_update();
```

![![](images/clipboard-2915613415.png)](images/clipboard-1887177245.png)

## **Despues de Eliminar**

``` sql
CREATE OR REPLACE FUNCTION process_purchases_delete()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    VALUES (OLD.id, 'DELETE', to_jsonb(OLD), NULL);
    RETURN OLD;
END;
$$ LANGUAGE plpgsql;

DROP TRIGGER IF EXISTS ad_purchases_audit ON purchases;

CREATE TRIGGER ad_purchases_audit
AFTER DELETE ON purchases
FOR EACH ROW EXECUTE FUNCTION process_purchases_delete();
```

![![](images/clipboard-2306453826.png)](images/clipboard-3129528895.png)

# **evidencia de cuando actualizo un nuevo dato**

![](images/clipboard-757611997.png)

![](images/clipboard-1248742110.png)

![](images/clipboard-2168781126.png)

#### podemos observar que en la actualizacion le sumamos un valor de 25.00 al total lo cual lo muestra perfectamente en nuestra tabla de auditoria

# conclusión

La implementación de la migración, consultas avanzadas y el sistema de auditoría mediante triggers fortalece la arquitectura de la base de datos de SuministroPro. Al registrar automáticamente las acciones en ventas, productos y compras mediante capturas en formato JSON, el sistema garantiza un control total, trazabilidad de cambios y seguridad financiera sin afectar el rendimiento ni la experiencia operativa.

# 4. Consultas avanzadas e implementación de triggers en SQL Server

## 4.1 Mostrar algunos de los registros de la tabla customers

Para este punto quise mostrar los datos básicos de los clientes, como su nombre completo, el tipo y número de documento, y si están activos o no en el sistema. Opté por hacer la consulta especificando exactamente esos campos en lugar de traer toda la tabla entera, porque así es más directo, se ve solo la información relevante que se necesita revisar y no cargamos la base de datos de manera innecesaria.

``` sql
SELECT full_name, document_type, document_number, status FROM customers;
```

![](images/clipboard-102749784.png)

## **Stored Procedure**

![](images/clipboard-962425814.png)

![](images/clipboard-2735823316.png)

## 4.2 Mostrar de forma ordenada (DESC) las ventas desde su comienzo

En esta consulta quise revisar el historial de ventas para ver primero las más recientes y de ahí ir bajando hacia las más antiguas. Para eso seleccioné el id de la venta, la fecha, el total y el estado, y opté por usar ORDER BY con la fecha de forma descendente (DESC) para que la información quede organizada cronológicamente comenzando por las últimas transacciones realizadas.

``` sql
SELECT id, sale_date, total, status FROM sales ORDER BY sale_date DESC;
```

![](images/clipboard-2693159057.png)

## Stored Procedure

![](images/clipboard-4130903444.png)

## ![](images/clipboard-2162122743.png)

## 4.3 Consultas a múltiples tablas mediante WHERE

En este punto quise relacionar las ventas con los clientes que realizaron cada compra. Para lograr esto, consulté las tablas sales y customers al mismo tiempo, utilizando alias (s y c) para simplificar el código y filtrando con la cláusula WHERE para que solo se crucen los registros donde el ID del cliente coincida en ambas tablas (c.id = s.customer_id).

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id;
```

![![](images/clipboard-1140220240.png)![](images/clipboard-3105844118.png)![](images/clipboard-1291904566.png)](images/clipboard-3817553916.png)

## Stored Procedure

![](images/clipboard-2643667487.png)

![![](images/clipboard-2622119438.png)](images/clipboard-3762978676.png)

## 4.4 Consultas a múltiples tablas mediante JOIN

Para este punto utilicé la cláusula JOIN para relacionar la tabla de clientes con la tabla de ventas de forma explícita y estándar en SQL. Seleccioné el nombre completo y el correo del cliente junto con los datos de sus ventas asociadas, uniendo ambas tablas a través de la coincidencia entre el ID del cliente y la clave foránea en ventas.

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id)
WHERE c.id = 1;
```

![![](images/clipboard-2490328575.png)](images/clipboard-685414014.png)

## Stored Procedure

![](images/clipboard-1745043906.png)

![](images/clipboard-1778475911.png)

# 4.5 Condiciones en las Consultas o filtros

Esta consulta responde a la necesidad del negocio de monitorear únicamente las operaciones vigentes en el sistema. Se aplicó el filtro por estado activo para aislar las transacciones efectivas y descartar aquellas que fueron canceladas o están inactivas, garantizando que los reportes reflejen el flujo real de ventas sin distorsionar los totales.

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id AND s.status = 'active';
```

![![](images/clipboard-3826557653.png)](images/clipboard-1337295472.png)

## Stored Procedure

![](images/clipboard-3294540130.png)

![](images/clipboard-2886259683.png)

# 4.6 Consultas con filtros condicional LIKE

Esta consulta atiende la necesidad de realizar búsquedas parciales o coincidencias de texto dentro del módulo de administración de clientes. El uso de la cláusula LIKE permite buscar registros basándose en patrones de texto en lugar de valores exactos, facilitando la localización de usuarios cuando el usuario o el sistema solo cuenta con el inicio de la dirección de correo electrónico.

``` sql
SELECT 
    c.full_name, 
    c.email, 
    s.*
FROM customers AS c
JOIN sales AS s 
    ON c.id = s.customer_id
WHERE s.status = 'inactive'
  AND c.email LIKE '%@ejemplo.com';
```

![![](images/clipboard-4034975129.png)](images/clipboard-2815714143.png)

## Stored Procedure

![](images/clipboard-2149353991.png)

![](images/clipboard-4256479140.png)

## 4.7 Consultas con filtros condicionales BETWEEN

Esta consulta resuelve el requerimiento de generar reportes consolidados dentro de un rango cronológico específico. Integrando datos de clientes, ventas, cuentas por cobrar, detalle de ventas y productos, se logra una trazabilidad completa de la operación comercial. La condición BETWEEN delimita el análisis a un periodo de tiempo determinado, mientras que el ordenamiento ascendente por fecha de venta facilita la lectura cronológica de las transacciones.

``` sql
SELECT 
    c.full_name, c.email, s.sale_date, s.status, ar.issue_date, p.sku
FROM customers c
JOIN sales s 
    ON c.id = s.customer_id
JOIN accounts_receivable ar 
    ON s.id = ar.sale_id
JOIN sale_details sd 
    ON s.id = sd.sale_id
JOIN products p ON p.id = sd.product_id
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
ORDER BY s.sale_date ASC;
```

![![](images/clipboard-2848936864.png)](images/clipboard-1178857074.png)

# Stored Procedure

![![](images/clipboard-1374877828.png)](images/clipboard-1166263987.png)

![](images/clipboard-2928061790.png)

## 4.8 Consultas con agrupamiento GROUP BY

En esta parte agrupé la información para sacar resúmenes de ventas por cliente, probando dos formas distintas de filtrar los datos según lo que se necesite consultar. La primera forma usa WHERE para limitar las ventas a unas fechas específicas antes de agruparlas. La segunda forma usa HAVING para filtrar los resultados después de hacer las sumas, lo que sirve para buscar únicamente a los clientes que superen un monto determinado.

``` sql
SELECT c.id, c.full_name, SUM(s.total) AS TotalSuma, AVG(s.total) AS PromedioVenta 
FROM customers AS c 
JOIN sales AS s ON c.id = s.customer_id  
WHERE s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
GROUP BY c.id, c.full_name 
HAVING SUM(s.total) >= 100  
ORDER BY TotalSuma DESC;
```

![](images/clipboard-2831318159.png)

## Stored Procedure

![](images/clipboard-1750667301.png)

![](images/clipboard-1240842944.png)

## 4.9 Subconsultas y teoría de conjuntos

Aquí busqué identificar a los clientes inactivos o que no han realizado compras dentro de un rango de fechas determinado, probando la combinación con LEFT JOIN filtrando los valores nulos para mayor eficiencia. Esta consulta nos sirve a nivel de negocio para detectar clientes ausentes y crear campañas de reactivación.

``` sql
SELECT c.* 
FROM customers AS c 
LEFT JOIN sales AS s 
    ON (
        c.id = s.customer_id 
        AND s.sale_date BETWEEN '2026-08-15 00:00:00' AND '2026-09-17 23:59:59'
    ) 
WHERE s.customer_id IS NULL;
```

![](images/clipboard-2970122769.png)

## Stored Procedure

![](images/clipboard-3506068325.png)

![](images/clipboard-980261770.png)

## Creación de triggers en la tabla sales

Para la tabla sales se implementó un sistema de auditoría mediante triggers que registra automáticamente un historial en la tabla sales_audit ante operaciones de inserción, actualización y eliminación, estructurando la información mediante FOR JSON AUTO para capturar el estado antes y después de cada modificación.

## Después de Insertar

``` sql
CREATE TRIGGER trg_sales_insert_audit
ON sales
AFTER INSERT
AS
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    SELECT 
        i.id,
        'INSERT',
        NULL,
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i;
END;
```

![](images/clipboard-4245570444.png)

## Después de Actualizar 

``` sql
CREATE TRIGGER trg_sales_update_audit
ON sales
AFTER UPDATE
AS
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    SELECT 
        i.id,
        'UPDATE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i
    INNER JOIN deleted d ON i.id = d.id;
END;
```

![](images/clipboard-3324960962.png)

## Después de Eliminar

``` sql
CREATE TRIGGER trg_sales_delete_audit
ON sales
AFTER DELETE
AS
BEGIN
    INSERT INTO sales_audit (sale_id, action_sale, before_data, after_data)
    SELECT 
        d.id,
        'DELETE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        NULL
    FROM deleted d;
END;
```

![](images/clipboard-3227479877.png)

## Creación de triggers en la tabla products

Para la tabla products, los triggers de auditoría almacenan automáticamente las modificaciones de precios, SKUs, descripciones y estados en products_audit utilizando formato JSON.

## Después de Insertar

``` sql
CREATE TRIGGER trg_products_insert_audit
ON products
AFTER INSERT
AS
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    SELECT 
        i.id,
        'INSERT',
        NULL,
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i;
END;
```

![](images/clipboard-4125422979.png)

## Después de Actualizar 

``` sql
CREATE TRIGGER trg_products_update_audit
ON products
AFTER UPDATE
AS
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    SELECT 
        i.id,
        'UPDATE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i
    INNER JOIN deleted d ON i.id = d.id;
END;
```

![](images/clipboard-1557634912.png)

## Después de Eliminar

``` sql
CREATE TRIGGER trg_products_delete_audit
ON products
AFTER DELETE
AS
BEGIN
    INSERT INTO products_audit (product_id, action_product, before_data, after_data)
    SELECT 
        d.id,
        'DELETE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        NULL
    FROM deleted d;
END;
```

![](images/clipboard-2665998297.png)

## Creación de triggers en la tabla purchases

Para la tabla purchases, el sistema de auditoría recopila y estructura mediante JSON las transacciones de abastecimiento, totales, impuestos y estados de proveedores en la tabla purchases_audit.

## Después de Insertar 

``` sql
CREATE TRIGGER trg_purchases_insert_audit
ON purchases
AFTER INSERT
AS
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    SELECT 
        i.id,
        'INSERT',
        NULL,
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i;
END;
```

![](images/clipboard-1895264182.png)

## Después de Actualizar 

``` sql
CREATE TRIGGER trg_purchases_update_audit
ON purchases
AFTER UPDATE
AS
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    SELECT 
        i.id,
        'UPDATE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        (SELECT * FROM inserted WHERE id = i.id FOR JSON AUTO, INCLUDE_NULL_VALUES)
    FROM inserted i
    INNER JOIN deleted d ON i.id = d.id;
END;
```

![](images/clipboard-2836726489.png)

## Después de Eliminar

``` sql
CREATE TRIGGER trg_purchases_delete_audit
ON purchases
AFTER DELETE
AS
BEGIN
    INSERT INTO purchases_audit (purchase_id, action_purchase, before_data, after_data)
    SELECT 
        d.id,
        'DELETE',
        (SELECT * FROM deleted WHERE id = d.id FOR JSON AUTO, INCLUDE_NULL_VALUES),
        NULL
    FROM deleted d;
END;
```

![](images/clipboard-2311686489.png)

## verificacion cuando actualizo un dato

![](images/clipboard-3427696473.png)

## **Conclusión:**

El proceso realizado en SQL Server demostró la importancia de estructurar correctamente las consultas y encapsularlas en Stored Procedures para optimizar la interacción con la base de datos. Asimismo, la implementación de restricciones (CHECK constraints) y triggers de auditoría con formato JSON permitió garantizar la integridad transaccional y registrar de forma automatizada e histórica cualquier cambio o modificación realizada en las tablas principales.
