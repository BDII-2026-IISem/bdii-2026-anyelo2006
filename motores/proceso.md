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

## creación de la tabla suppliers en la terminal Dbeaver mysql

``` sql
CREATE TABLE suppliers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nit VARCHAR(30) NOT NULL UNIQUE,
    company_name VARCHAR(150) NOT NULL,
    contact_person VARCHAR(100),
    phone VARCHAR(30),
    email VARCHAR(100),
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
);
```

![](images/clipboard-3004942745.png)

## creación de la tabla purchaces en el terminal Dbeaver mysql

``` sql
CREATE TABLE purchases (
    id INT AUTO_INCREMENT PRIMARY KEY,
    supplier_id INT NOT NULL,
    purchase_date DATETIME NOT NULL,
    subtotal DECIMAL(12,2) NOT NULL,
    taxes DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (supplier_id) REFERENCES suppliers(id)
);
```

![](images/clipboard-1358966974.png)

## creación de la tabla parchease_details en la terminal dbeaver mysql

``` sql
CREATE TABLE purchase_details (
    id INT AUTO_INCREMENT PRIMARY KEY,
    purchase_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    notes TEXT,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (purchase_id) REFERENCES purchases(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

![](images/clipboard-3124421861.png)

## creación de la tabla inventories en la terminal dbeaver mysql

``` sql
CREATE TABLE inventories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    location_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    minimum_stock INT NOT NULL,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

![](images/clipboard-3712426370.png)

## creación de la tabla customers en la terminal de dbeaver mysql

``` sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    document_type VARCHAR(20) NOT NULL,
    document_number VARCHAR(30) NOT NULL UNIQUE,
    full_name VARCHAR(150) NOT NULL,
    phone VARCHAR(30),
    email VARCHAR(100),
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL
);
```

![](images/clipboard-1812522684.png)
