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

## creación de la tabla sales en la terminal de dbeaver mysql

``` sql
CREATE TABLE sales (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    sale_date DATETIME NOT NULL,
    subtotal DECIMAL(12,2) NOT NULL,
    taxes DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

![](images/clipboard-2589085581.png)

## creación de la tabla sale_details en la terminal de dbeaver mysql

``` sql
CREATE TABLE sale_details (
    id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    notes TEXT,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (sale_id) REFERENCES sales(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

![](images/clipboard-2208481574.png)

## creación de la tabla accounts_receivable en la terminal dbeaver mysql

``` sql
CREATE TABLE accounts_receivable (
    id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT NOT NULL UNIQUE,
    issue_date DATETIME NOT NULL,
    amount DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    notes TEXT,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (sale_id) REFERENCES sales(id)
);
```

![](images/clipboard-3100390865.png)

## creacion de la tabla receivable_pyments en la terminal dbeaver mysql

``` sql
CREATE TABLE receivable_payments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    account_receivable_id INT NOT NULL,
    payment_date DATETIME NOT NULL,
    amount DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    notes TEXT,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (account_receivable_id) REFERENCES accounts_receivable(id)
);
```

![](images/clipboard-312231579.png)

## creación de la tabla returns en la terminal dbeaver mysql

``` sql
CREATE TABLE returns (
    id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT NOT NULL,
    return_date DATETIME NOT NULL,
    reason TEXT NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    status ENUM('active','inactive'),
    created_at DATETIME NOT NULL,
    updated_at DATETIME NOT NULL,
    FOREIGN KEY (sale_id) REFERENCES sales(id)
);
```

![](images/clipboard-1201859032.png)

## creación de la base de datos en el gestor workbench

![](images/clipboard-3702421125.png)

## creación de la tabla products en workbench

![](images/clipboard-1982452136.png)

## creación de la tabla suppliers  en workbench

![](images/clipboard-284856169.png)

## creación de la tabla purchases  en workbench

![](images/clipboard-3407294112.png)

## creación de la tabla parchease_details   en workbench

![](images/clipboard-1755828797.png)

## creación de la tabla inventories en workbench

![](images/clipboard-556122535.png)

## creación de la tabla customers en workbench

![](images/clipboard-109304435.png)

## creación de la tabla sales en workbench

![](images/clipboard-3756534524.png)

## creación de la tabla sale_details en workbench

![](images/clipboard-686804646.png)

## creación de la tabla accounts_receivable en workbench

![](images/clipboard-1756397758.png)

## creación de la tabla receivable_payments en workbench

![](images/clipboard-3982806396.png)

## creación de la tabla returns en workbench

![](images/clipboard-3171669395.png)

# creación de mi base de datos en postgres

# creación de mi base dato en la termina dbeaver

``` sql
create database SumintroPro;
```

![](images/clipboard-4049943986.png)

# creación de la tabla products en la termina dbeaver postgres

``` sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    sku VARCHAR(50) NOT NULL UNIQUE,
    name VARCHAR(150) NOT NULL,
    description VARCHAR(225),
    price DECIMAL(12,2) NOT NULL,
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')) DEFAULT 'active',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

![](images/clipboard-1131604180.png)

# creación de la tabla suppliers en la termina dbeaver postgres

``` sql
CREATE TABLE suppliers (
    id SERIAL PRIMARY KEY,
    nit VARCHAR(30) NOT NULL UNIQUE,
    company_name VARCHAR(150) NOT NULL,
    contact_person VARCHAR(100),
    phone VARCHAR(30),
    email VARCHAR(100),
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL
);
```

![](images/clipboard-634459916.png)

# creación de la tabla purchases en la termina dbeaver postgres

``` sql
CREATE TABLE purchases (
    id SERIAL PRIMARY KEY,
    supplier_id INT NOT NULL,
    purchase_date TIMESTAMP NOT NULL,
    subtotal DECIMAL(12,2) NOT NULL,
    taxes DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    state VARCHAR(30) NOT NULL,
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    FOREIGN KEY (supplier_id) REFERENCES suppliers(id)
);
```

![](images/clipboard-2236403077.png)

# creación de la tabla purchase_details en la termina dbeaver postgres

``` sql
CREATE TABLE purchase_details (
    id SERIAL PRIMARY KEY,
    purchase_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(12,2) NOT NULL,
    total DECIMAL(12,2) NOT NULL,
    notes TEXT,
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    FOREIGN KEY (purchase_id) REFERENCES purchases(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

![](images/clipboard-2378527036.png)

# creación de la tabla inventories en la termina dbeaver postgres

``` sql
CREATE TABLE inventories (
    id SERIAL PRIMARY KEY,
    location_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    minimum_stock INT NOT NULL,
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL,
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

![](images/clipboard-1220782752.png)

# creación de la tabla customers en la termina dbeaver postgres

``` sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    document_type VARCHAR(20) NOT NULL,
    document_number VARCHAR(30) NOT NULL UNIQUE,
    full_name VARCHAR(150) NOT NULL,
    phone VARCHAR(30),
    email VARCHAR(100),
    status VARCHAR(10) CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP NOT NULL
);
```

![](images/clipboard-2220114413.png)
