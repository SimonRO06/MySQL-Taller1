CREATE DATABASE vtaszfs;

USE vtaszfs;

CREATE TABLE IF NOT EXISTS Clientes(
id VARCHAR(50) PRIMARY KEY,
nombre VARCHAR(50),
email VARCHAR(50) UNIQUE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Direcciones(
id INT AUTO_INCREMENT PRIMARY KEY,
direccion VARCHAR(255)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Paises(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre_pais VARCHAR(50) UNIQUE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Regiones(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre_region VARCHAR(50) UNIQUE,
id_pais INT,
CONSTRAINT FK_id_pais FOREIGN KEY (id_pais) REFERENCES Paises(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Ciudades(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre_ciudad VARCHAR(50) UNIQUE,
id_region INT,
CONSTRAINT FK_id_region FOREIGN KEY (id_region) REFERENCES Regiones(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS CodigoPostal(
id INT AUTO_INCREMENT PRIMARY KEY,
codigo VARCHAR(50) UNIQUE,
id_ciudad INT,
CONSTRAINT FK_id_ciudad FOREIGN KEY (id_ciudad) REFERENCES Ciudades(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Proveedores(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Ubicaciones(
id INT AUTO_INCREMENT PRIMARY KEY,
id_cliente VARCHAR(50),
CONSTRAINT FK_id_cliente FOREIGN KEY (id_cliente) REFERENCES Clientes(id),
id_proveedor INT,
CONSTRAINT FK_id_proveedor FOREIGN KEY (id_proveedor) REFERENCES Proveedores(id),
id_direccion INT,
CONSTRAINT FK_id_direccion FOREIGN KEY (id_direccion) REFERENCES Direcciones(id),
id_codigopostal INT,
CONSTRAINT FK_id_codigopostal FOREIGN KEY (id_codigopostal) REFERENCES CodigoPostal(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Puestos(
id INT AUTO_INCREMENT PRIMARY KEY,
cargo VARCHAR(50),
descripción TEXT
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS DatosEmpleados(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50),
salario DECIMAL(10,2),
fecha_contrato DATE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Empleados(
id INT AUTO_INCREMENT PRIMARY KEY,
id_datoempleado INT,
CONSTRAINT FK_id_datoempleado FOREIGN KEY (id_datoempleado) REFERENCES DatosEmpleados(id),
id_puesto INT,
CONSTRAINT FK_id_puesto FOREIGN KEY (id_puesto) REFERENCES Puestos(id)
) ENGINE = INNODB; 

CREATE TABLE IF NOT EXISTS PuestosEmpleados(
id INT AUTO_INCREMENT PRIMARY KEY,
id_cargo INT,
CONSTRAINT FK_id_cargo FOREIGN KEY (id_cargo) REFERENCES Puestos(id),
id_empleado INT,
CONSTRAINT FK_empleado_id FOREIGN KEY (id_empleado) REFERENCES Empleados(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS TelefonoClientes(
id INT AUTO_INCREMENT PRIMARY KEY,
numero VARCHAR(50),
código_area VARCHAR(3),
id_cliente VARCHAR(50),
CONSTRAINT FK_clienteid FOREIGN KEY (id_cliente) REFERENCES Clientes(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS ContactoProveedores(
id INT AUTO_INCREMENT PRIMARY KEY,
email VARCHAR(50) UNIQUE,
id_proveedor INT,
CONSTRAINT FK_idproveedor FOREIGN KEY (id_proveedor) REFERENCES Proveedores(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS TelefonoProveedores(
id INT AUTO_INCREMENT PRIMARY KEY,
numero VARCHAR(50),
código_area VARCHAR(3),
id_contactoproveedor INT,
CONSTRAINT FK_id_contactoproveedor FOREIGN KEY (id_contactoproveedor) REFERENCES ContactoProveedores(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS EmpleadosProveedores(
id_proveedor INT,
CONSTRAINT FK_proveedorid FOREIGN KEY (id_proveedor) REFERENCES Proveedores(id),
id_empleado INT,
CONSTRAINT FK_empleadoid FOREIGN KEY (id_empleado) REFERENCES Empleados(id),
PRIMARY KEY (id_proveedor, id_empleado)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS TiposProductos(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50),
descripción TEXT
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Productos(
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50),
precio DECIMAL(10,2),
id_proveedor INT,
CONSTRAINT FK_proveedor_id FOREIGN KEY (id_proveedor) REFERENCES Proveedores(id),
id_tipoproducto INT,
CONSTRAINT FK_id_tipoproducto FOREIGN KEY (id_tipoproducto) REFERENCES TiposProductos(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS Pedidos(
id INT AUTO_INCREMENT PRIMARY KEY,
id_cliente VARCHAR(50),
CONSTRAINT FK_clienteid FOREIGN KEY (id_cliente) REFERENCES Clientes(id),
fecha DATE,
id_empleado INT,
CONSTRAINT FK_idempleado FOREIGN KEY (id_empleado) REFERENCES Empleados(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS DetallesPedido(
id INT AUTO_INCREMENT PRIMARY KEY,
pedido_id INT,
CONSTRAINT FK_pedido_id FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
producto_id INT,
CONSTRAINT FK_producto_id FOREIGN KEY (producto_id) REFERENCES Productos(id),
cantidad INT,
precio DECIMAL(10,2)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS HistorialPedidos(
id INT AUTO_INCREMENT PRIMARY KEY,
cliente_nuevo VARCHAR(50),
fecha_nueva DATE,
id_pedido INT,
CONSTRAINT FK_idpedido FOREIGN KEY (id_pedido) REFERENCES Pedidos(id)
) ENGINE = INNODB;

INSERT INTO Clientes (id, nombre, email) VALUES ('CL001', 'Juan', 'juan@email.com'),('CL002', 'María', 'maria@email.com'),('CL003', 'Carlos', 'carlos@email.com'),('CL004', 'Ana', 'ana@email.com'),('CL005', 'Luis', 'luis@email.com'),('CL006', 'Sofía', 'sofia@email.com'),('CL007', 'Pedro', 'pedro@email.com'),('CL008', 'Laura', 'laura@email.com');

INSERT INTO Direcciones (direccion) VALUES ('Calle Principal 123'),('Avenida Central 456'),('Boulevard Norte 789'),('Calle Sur 101'),('Avenida Este 202'),('Calle Oeste 303'),('Boulevard Sur 404'),('Avenida Norte 505');

INSERT INTO Paises (nombre_pais) VALUES ('México'),('Estados Unidos'),('España'),('Colombia'),('Argentina'),('Chile'),('Brasil'),('Perú');

INSERT INTO Regiones (nombre_region, id_pais) VALUES ('Norte',1),('Sur',1),('Este',1),('Oeste',1),('Centro',1),('Noroeste',1),('Sureste',1),('Noreste',1);

INSERT INTO Ciudades (nombre_ciudad, id_region) VALUES ('Ciudad de México',1),('Guadalajara',2),('Monterrey',3),('Puebla',4),('Tijuana',5),('Leon',6),('Queretaro',7),('Merida',8);

INSERT INTO CodigoPostal (codigo, id_ciudad) VALUES ('01000',1),('44100',2),('64000',3),('72000',4),('22000',5),('37000',6),('76000',7),('97000',8);

INSERT INTO Proveedores (nombre) VALUES ('Proveedor A'),('Proveedor B'),('Proveedor C'),('Proveedor D'),('Proveedor E'),('Proveedor F'),('Proveedor G'),('Proveedor H');

INSERT INTO Puestos (cargo, descripción) VALUES ('Gerente','Responsable del departamento'),('Vendedor','Atención a clientes y ventas'),('Almacenista','Control de inventario'),('Contador','Manejo de finanzas'),('Soporte Técnico','Asistencia técnica'),('Marketing','Promoción y publicidad'),('Recursos Humanos','Gestión de personal'),('Logística','Distribución y envíos');

INSERT INTO TiposProductos (nombre, descripción) VALUES ('Electrónicos','Dispositivos electrónicos y gadgets'),('Ropa','Prendas de vestir para hombre y mujer'),('Alimentos','Productos alimenticios y comestibles'),('Hogar','Artículos para el hogar y decoración'),('Juguetes','Juguetes y artículos para niños'),('Deportes','Artículos deportivos y fitness'),('Libros','Libros y material educativo'),('Belleza','Productos de cuidado personal y belleza');

INSERT INTO Productos (nombre, precio, id_proveedor, id_tipoproducto) VALUES ('Laptop',15000.00,1,1),('Smartphone',8000.00,2,1),('Camisa',500.00,3,2),('Pantalón',700.00,4,2),('Arroz',25.00,5,3),('Frijol',30.00,6,3),('Lámpara',400.00,7,4),('Jarrón',350.00,8,4);

INSERT INTO DatosEmpleados (nombre, salario, fecha_contrato) VALUES ('Roberto',12000.00,'2020-01-10'),('Patricia',9500.00,'2019-03-15'),('Fernando',8500.00,'2021-02-20'),('Adriana',11000.00,'2020-07-01'),('Jorge',8000.00,'2018-11-12'),('Lucía',9000.00,'2022-01-05'),('Ricardo',10000.00,'2019-06-20'),('Verónica',10500.00,'2020-09-15');

INSERT INTO Empleados (id_datoempleado, id_puesto) VALUES (1,1),(2,2),(3,3),(4,4),(5,5),(6,6),(7,7),(8,8);

INSERT INTO PuestosEmpleados (id_cargo, id_empleado) VALUES (1,1),(2,2),(3,3),(4,4),(5,5),(6,6),(7,7),(8,8);

INSERT INTO TelefonoClientes (numero, código_area, id_cliente) VALUES ('5551111','55','CL001'),('5552222','55','CL002'),('5553333','55','CL003'),('5554444','55','CL004'),('5555555','55','CL005'),('5556666','55','CL006'),('5557777','55','CL007'),('5558888','55','CL008');

INSERT INTO ContactoProveedores (email, id_proveedor) VALUES ('contacto@proveedora.com',1),('contacto@proveedorb.com',2),('contacto@proveedorc.com',3),('contacto@proveedord.com',4),('contacto@proveedore.com',5),('contacto@proveedorf.com',6),('contacto@proveedorg.com',7),('contacto@proveedorh.com',8);

INSERT INTO TelefonoProveedores (numero, código_area, id_contactoproveedor) VALUES ('5559999','55',1),('5550000','55',2),('5551111','55',3),('5552222','55',4),('5553333','55',5),('5554444','55',6),('5555555','55',7),('5556666','55',8);

INSERT INTO EmpleadosProveedores (id_proveedor, id_empleado) VALUES (1,1),(2,2),(3,3),(4,4),(5,5),(6,6),(7,7),(8,8);

INSERT INTO Ubicaciones (id_cliente, id_proveedor, id_direccion, id_codigopostal) VALUES ('CL001',1,1,1),('CL002',2,2,2),('CL003',3,3,3),('CL004',4,4,4),('CL005',5,5,5),('CL006',6,6,6),('CL007',7,7,7),('CL008',8,8,8);

INSERT INTO Pedidos (id_cliente, fecha, id_empleado) VALUES ('CL001','2023-01-15',1),('CL002','2023-02-20',2),('CL003','2023-03-10',3),('CL004','2023-04-05',4),('CL005','2023-05-12',5),('CL006','2023-06-18',6),('CL007','2023-07-22',7),('CL008','2023-08-30',8);

INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad, precio) VALUES (1,1,1,15000.00),(2,2,1,8000.00),(3,3,2,1000.00),(4,4,1,700.00),(5,5,5,125.00),(6,6,3,90.00),(7,7,1,400.00),(8,8,2,700.00);

INSERT INTO HistorialPedidos (cliente_nuevo, fecha_nueva, id_pedido) VALUES ('CL001','2023-01-15',1),('CL002','2023-02-20',2),('CL003','2023-03-10',3),('CL004','2023-04-05',4),('CL005','2023-05-12',5),('CL006','2023-06-18',6),('CL007','2023-07-22',7),('CL008','2023-08-30',8);

## Joins

SELECT Pedidos.id, Clientes.nombre AS cliente, Pedidos.fecha
FROM Pedidos
INNER JOIN Clientes ON Pedidos.id_cliente = Clientes.id;

SELECT Productos.nombre AS producto, Proveedores.nombre AS proveedor
FROM Productos
INNER JOIN Proveedores ON Productos.id_proveedor = Proveedores.id;

SELECT Pedidos.id, Clientes.nombre, Ubicaciones.id AS ubicacion_id
FROM Pedidos
LEFT JOIN Clientes ON Pedidos.id_cliente = Clientes.id
LEFT JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente;

SELECT Empleados.id, DatosEmpleados.nombre, Pedidos.id AS pedido_id
FROM Empleados
LEFT JOIN DatosEmpleados ON Empleados.id_datoempleado = DatosEmpleados.id
LEFT JOIN Pedidos ON Empleados.id = Pedidos.id_empleado;

SELECT TiposProductos.nombre AS tipo, Productos.nombre AS producto
FROM TiposProductos
INNER JOIN Productos ON TiposProductos.id = Productos.id_tipoproducto;

SELECT Clientes.nombre, COUNT(Pedidos.id) AS total_pedidos
FROM Clientes
LEFT JOIN Pedidos ON Clientes.id = Pedidos.id_cliente
GROUP BY Clientes.id, Clientes.nombre;

SELECT Pedidos.id, Pedidos.fecha, DatosEmpleados.nombre AS empleado
FROM Pedidos
INNER JOIN Empleados ON Pedidos.id_empleado = Empleados.id
INNER JOIN DatosEmpleados ON Empleados.id_datoempleado = DatosEmpleados.id;

SELECT Productos.nombre
FROM DetallesPedido
RIGHT JOIN Productos ON DetallesPedido.producto_id = Productos.id
WHERE DetallesPedido.producto_id IS NULL;

SELECT Clientes.nombre, COUNT(Pedidos.id) AS total_pedidos, Direcciones.direccion
FROM Clientes
LEFT JOIN Pedidos ON Clientes.id = Pedidos.id_cliente
LEFT JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente
LEFT JOIN Direcciones ON Ubicaciones.id_direccion = Direcciones.id
GROUP BY Clientes.id, Clientes.nombre, Direcciones.direccion;

SELECT Proveedores.nombre AS proveedor, Productos.nombre AS producto, TiposProductos.nombre AS tipo
FROM Productos
INNER JOIN Proveedores ON Productos.id_proveedor = Proveedores.id
INNER JOIN TiposProductos ON Productos.id_tipoproducto = TiposProductos.id;

## Consultas Simples

SELECT nombre, precio
FROM Productos
WHERE precio > 50;

SELECT Clientes.nombre
FROM Clientes, Ubicaciones, CodigoPostal, Ciudades
WHERE Clientes.id = Ubicaciones.id_cliente
AND Ubicaciones.id_codigopostal = CodigoPostal.id
AND CodigoPostal.id_ciudad = Ciudades.id
AND Ciudades.nombre_ciudad = 'Monterrey';

SELECT id, nombre, fecha_contrato
FROM DatosEmpleados
WHERE fecha_contrato >= DATE_SUB(CURDATE(), INTERVAL 2 YEAR);

SELECT Proveedores.id, Proveedores.nombre, COUNT(Productos.id) AS total_productos
FROM Proveedores
JOIN Productos ON Proveedores.id = Productos.id_proveedor
GROUP BY Proveedores.id, Proveedores.nombre
HAVING COUNT(Productos.id) > 5;

SELECT Clientes.id, Clientes.nombre
FROM Clientes
LEFT JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente
WHERE Ubicaciones.id_cliente IS NULL;

SELECT Clientes.id, Clientes.nombre, SUM(DetallesPedido.precio * DetallesPedido.cantidad) AS total_ventas
FROM Clientes
JOIN Pedidos ON Clientes.id = Pedidos.id_cliente
JOIN DetallesPedido ON Pedidos.id = DetallesPedido.pedido_id
GROUP BY Clientes.id, Clientes.nombre;

SELECT AVG(salario) AS salario_promedio
FROM DatosEmpleados;

SELECT id, nombre
FROM TiposProductos;

SELECT id, nombre, precio
FROM Productos
ORDER BY precio DESC
LIMIT 3;

SELECT Clientes.id, Clientes.nombre, COUNT(Pedidos.id) AS total_pedidos
FROM Clientes
JOIN Pedidos ON Clientes.id = Pedidos.id_cliente
GROUP BY Clientes.id, Clientes.nombre
ORDER BY total_pedidos DESC
LIMIT 1;

## Consultas Multitablas

SELECT Pedidos.id, Pedidos.fecha, Clientes.nombre AS cliente
FROM Pedidos
JOIN Clientes ON Pedidos.id_cliente = Clientes.id;

SELECT Clientes.nombre, Direcciones.direccion, Pedidos.id AS pedido_id
FROM Pedidos
JOIN Clientes ON Pedidos.id_cliente = Clientes.id
JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente
JOIN Direcciones ON Ubicaciones.id_direccion = Direcciones.id;

SELECT Productos.nombre AS producto, Proveedores.nombre AS proveedor, TiposProductos.nombre AS tipo
FROM Productos
JOIN Proveedores ON Productos.id_proveedor = Proveedores.id
JOIN TiposProductos ON Productos.id_tipoproducto = TiposProductos.id;

SELECT DISTINCT DatosEmpleados.nombre AS empleado, Ciudades.nombre_ciudad
FROM Pedidos
JOIN Clientes ON Pedidos.id_cliente = Clientes.id
JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente
JOIN CodigoPostal ON Ubicaciones.id_codigopostal = CodigoPostal.id
JOIN Ciudades ON CodigoPostal.id_ciudad = Ciudades.id
JOIN Empleados ON Pedidos.id_empleado = Empleados.id
JOIN DatosEmpleados ON Empleados.id_datoempleado = DatosEmpleados.id
WHERE Ciudades.nombre_ciudad = 'Monterrey';

SELECT Productos.nombre, SUM(DetallesPedido.cantidad) AS total_vendidos
FROM DetallesPedido
JOIN Productos ON DetallesPedido.producto_id = Productos.id
GROUP BY Productos.id, Productos.nombre
ORDER BY total_vendidos DESC
LIMIT 5;

SELECT Clientes.nombre, Ciudades.nombre_ciudad, COUNT(Pedidos.id) AS total_pedidos
FROM Pedidos
JOIN Clientes ON Pedidos.id_cliente = Clientes.id
JOIN Ubicaciones ON Clientes.id = Ubicaciones.id_cliente
JOIN CodigoPostal ON Ubicaciones.id_codigopostal = CodigoPostal.id
JOIN Ciudades ON CodigoPostal.id_ciudad = Ciudades.id
GROUP BY Clientes.id, Clientes.nombre, Ciudades.nombre_ciudad;

SELECT DISTINCT Clientes.nombre AS cliente, Proveedores.nombre AS proveedor, Ciudades.nombre_ciudad
FROM Clientes
JOIN Ubicaciones uc ON Clientes.id = uc.id_cliente
JOIN CodigoPostal cpc ON uc.id_codigopostal = cpc.id
JOIN Ciudades ciu_c ON cpc.id_ciudad = ciu_c.id

JOIN Ubicaciones up ON Proveedores.id = up.id_proveedor
JOIN CodigoPostal cpp ON up.id_codigopostal = cpp.id
JOIN Ciudades ciu_p ON cpp.id_ciudad = ciu_p.id
JOIN Proveedores ON up.id_proveedor = Proveedores.id
WHERE ciu_c.id = ciu_p.id;

SELECT TiposProductos.nombre AS tipo, SUM(DetallesPedido.precio * DetallesPedido.cantidad) AS total_ventas
FROM DetallesPedido
JOIN Productos ON DetallesPedido.producto_id = Productos.id
JOIN TiposProductos ON Productos.id_tipoproducto = TiposProductos.id
GROUP BY TiposProductos.id, TiposProductos.nombre;

SELECT DISTINCT DatosEmpleados.nombre AS empleado
FROM Pedidos
JOIN DetallesPedido ON Pedidos.id = DetallesPedido.pedido_id
JOIN Productos ON DetallesPedido.producto_id = Productos.id
JOIN Proveedores ON Productos.id_proveedor = Proveedores.id
JOIN Empleados ON Pedidos.id_empleado = Empleados.id
JOIN DatosEmpleados ON Empleados.id_datoempleado = DatosEmpleados.id
WHERE Proveedores.nombre = 'Proveedor A';