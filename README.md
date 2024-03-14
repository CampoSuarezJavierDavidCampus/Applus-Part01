# PRUEBA PARTE 1
Crear un nuevo proyecto con PHP, MySQL y JQuery o AngularJS (Plus).

Notas:

- Apreciamos que nos envíen el código usando un repositorio de GitHub.
- Trabajar con Bootstrap para el problema número 1.
- Para el problema número 2 se debe correr el SQL enviado con la prueba, este contiene la estructura y datos necesarios.
- Entregar en un máximo de 24 horas a partir de la recepción de estas instrucciones.

## Objetivos
- Crear entidades
- Listar productos.
- Crear productos.
- Editar productos.
- Eliminar productos
- Solicitar confirmacion al eliminar

## SQL DB
```sql
CREATE DATABASE IF NOT EXISTS `shop`;
USE `shop`;

CREATE TABLE IF NOT EXISTS `category` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL,
  `createAt`  datetime default now(),
  `updateAt`  datetime default now() ,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

CREATE TABLE IF NOT EXISTS `product` (  
  `code` varchar(50)  CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci  primary key,
  `name` varchar(50) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci DEFAULT NULL,
  `categoryId` int NOT NULL,
  `price` float,
  `createAt`  datetime default now(),
  `updateAt`  datetime default now()
) DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

alter table `product` Add constraint 
fk_Product_Category foreign key (categoryId) 
references category (ID);

INSERT INTO `category` ( `name`) VALUES
	( 'office'),
	( 'home'),
	( 'tecnologies'),
	( 'pets');
    
INSERT INTO `product` (`code`, `name`, `categoryId`, `price`) VALUES
	('off1', 'office-item-1',1,19.2),
    ('off2', 'office-item-2',1,10.2),
    ('off3', 'office-item-3',1,1.2),
    
	('hom1', 'home-item-2',2,89.9),
    ('hom2', 'home-item-1',2,9.9),
    ('hom3', 'home-item-3',2,5.6),
    
	('tec1', 'tecnologies-item-3',3,6.99),
    ('tec2', 'tecnologies-item-2',3,96.99),
    ('tec3', 'tecnologies-item-1',3,0.99),
    
	('pet1', 'pets-item-1',4,8.95),
    ('pet2', 'pets-item-2',4,6.95),
    ('pet3', 'pets-item-3',4,54.6);

```





# PRUEBA PARTE 2

## Consulta 1 - Libros Prestados:
• Encuentra el título y el autor de los libros actualmente prestados, junto
con el nombre del usuario que los tiene prestados. Incluye también la
fecha de préstamo y la fecha de devolución.
``` sql
Select 
    p.id as id,
    l.titulo as tituloLibro,
    l.autor as autorLibro,
    u.nombre as usuario,
    p.fecha_prestamo as fechaPrestamo,
    p.fecha_devolucion as fechaDevolucion
from prestamo as p 
    join libro l on p.libro_id = l.id
    join usuario u on p.usuario_id = u.id
order by p.fecha_prestamo;
```
## Consulta 2 - Libros No Devueltos en 7 días:
• Encuentra los títulos y autores de los libros que fueron prestados hace
más de 7 días y aún no han sido devueltos. Incluye el nombre del
usuario que los tiene prestados y la fecha de préstamo.
```sql
/* Se agregan algunos préstamos con fechas vencidas */
INSERT INTO `prestamo` (`id`, `libro_id`, `fecha_prestamo`, `fecha_devolucion`, `usuario_id`) VALUES
	(16, 1, '2023-12-15', '2024-01-15', 1),
	(17, 2, '2023-12-20', '2023-12-29', 2),
	(18, 3, '2023-12-25', '2024-01-03', 3),
	(19, 1, '2023-12-30', '2024-01-30', 1);

Select 
    p.id as id,
    l.titulo as tituloLibro,
    l.autor as autorLibro,
    u.nombre as usuario,
    p.fecha_prestamo as fechaPrestamo,
    p.fecha_devolucion as fechaDevolucion
from prestamo as p 
    join libro l on p.libro_id = l.id
    join usuario u on p.usuario_id = u.id
where 
    p.fecha_devolucion is NULL OR
    (
        p.fecha_devolucion IS NOT NULL 
        AND datediff(p.fecha_devolucion,p.fecha_prestamo)>7
    )
```