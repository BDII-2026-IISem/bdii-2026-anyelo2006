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

##  1.5 Registro de la tabla inventories

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

##  1.11 Registro de la tabla returns

``` sql
SELECT * FROM SuministroPro.returns;
```

![](images/clipboard-2385880107.png)

# 2. Consultas avanzadas en MySQL:

## 2.1 Mostrar algunos de los registros de la tabla customers

``` sql
SELECT full_name, document_type, document_number, status FROM customers;
```

![](images/clipboard-1776810327.png)

## 2.2 Mostrar de forma ordenada (DESC) las ventas desde su comienzo

``` sql
SELECT id, sale_date, total, status FROM sales ORDER BY sale_date DESC;
```

![](images/clipboard-2696830787.png)

## 2.3 Consultas a múltiples tablas mediante WHERE

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id;
```

![![](images/clipboard-3637097975.png)](images/clipboard-3237375928.png)

![](images/clipboard-691916665.png)

## 2.4 Consultas a múltiples tablas mediante JOIN

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id);
```

![](images/clipboard-3465154707.png)

![](images/clipboard-2245255808.png)

## 2.5 Condiciones en las Consultas o filtros en las Consultas

``` sql
SELECT *
FROM sales s, customers c 
WHERE c.id = s.customer_id AND s.status = 'active';
```

![![](images/clipboard-1992643145.png)](images/clipboard-781554155.png)

![](images/clipboard-3285347994.png)

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id) 
WHERE s.status = 'inactive';
```

![![](images/clipboard-2747926933.png)](images/clipboard-2024714984.png)

# 2.6 Consultas con filtros condicional LIKE

``` sql
SELECT * 
FROM customers AS c 
WHERE c.email LIKE 'm%';
```

![](images/clipboard-2547520077.png)

## **Mostrar todos los correos de los clientes que contengan el dominio gmail**

``` sql
SELECT * 
FROM customers AS c 
WHERE c.email LIKE CONCAT('%', 'gmail', '%');
```

## ![](images/clipboard-1863704578.png)

## Combinación del punto 1.5 y la implementación del LIKE

``` sql
SELECT c.full_name, c.email, s.* 
FROM customers AS c 
JOIN sales AS s ON (c.id = s.customer_id) 
WHERE s.status = 'inactive' AND c.email LIKE 'm%';
```

![](images/clipboard-897229524.png)
