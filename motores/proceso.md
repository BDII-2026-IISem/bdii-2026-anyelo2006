# creación de la base de datos mysql

## creación de la base de datos por el terminal DBeaver

``` sql
CREATE DATABASE SuministroPro;
```

![](images/clipboard-3507846455.png)

## creación de la tabla products en el terminal Dbeaver mysql

``` sql
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY ,
    sku VARCHAR(50) NOT NULL UNIQUE,
    name VARCHAR(150) NOT NULL,
    description VARCHAR(225),
    price DECIMAL(12,2) NOT NULL,
    status enum('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
);
```

![](images/clipboard-2551221203.png)
