# Tesoro1
Los codigos en MYSQL

/*Crear Tablas */
DROP TABLE IF EXISTS NAUFRAGIO; 
DROP TABLE IF EXISTS RESCATE_REALIZADO;
DROP TABLE IF EXISTS rescaterealizado;
DROP TABLE IF EXISTS EDAD_HISTORIA;
DROP TABLE IF EXISTS TIPO_RESCATE;
DROP TABLE IF EXISTS TIPO_CARGAMENTO;
DROP TABLE IF EXISTS ZONAS_NAUFRAGIO; 

CREATE TABLE ZONAS_NAUFRAGIO (
idZona INTEGER not null auto_increment,
nombreZona VARCHAR (20), 
primary key (idZona)
);

CREATE TABLE EDAD_HISTORIA (
idEdad INTEGER primary key auto_increment, 
nombreEdad VARCHAR (60), 
fechaInicio year not null, 
fechaFin datetime not null 
);

CREATE TABLE TIPO_RESCATE (
idTipoRescate INTEGER primary key auto_increment, 
Categoria VARCHAR(20),
MaterialNecesario VARCHAR(100)
);
CREATE TABLE NAUFRAGIO (
idNaufragio INTEGER PRIMARY KEY AUTO_INCREMENT, 
nombre VARCHAR(60) not null, 
procedencia VARCHAR(20) not null, 
lugarNaufragio VARCHAR(100) NOT NULL, 
idZonaNaufragio INTEGER NOT NULL, 
fechaNaufragio datetime,  /*He puesto varchar es para que me acepte varios formatos de fechas: año, año-mes-dia...*/ 
ruta VARCHAR(100), 
cargamento VARCHAR(100), 
hayRescate VARCHAR(5) DEFAULT 'n' not null,
infoRescate VARCHAR(100),
idTipoRescate INTEGER not null,
notas VARCHAR(200),
CONSTRAINT hayRescate_check CHECK (hayRescate IN ('s' , 'n')),
CONSTRAINT FOREIGN KEY (idZonaNaufragio) REFERENCES ZONAS_NAUFRAGIO (idZona), 
CONSTRAINT FOREIGN KEY (idTipoRescate) REFERENCES TIPO_RESCATE (idTipoRescate)
);

CREATE TABLE RESCATE_REALIZADO (
idRescate INTEGER auto_increment, 
nombreBarco VARCHAR(20), 
idZonaNaufragio INTEGER not null, 
fechaRescate datetime,
miembrosImplicados VARCHAR(20), 
porcentaje INTEGER DEFAULT 0.2, 
CONSTRAINT PRIMARY KEY (idRescate), 
CONSTRAINT FOREIGN KEY (idZonaNaufragio) references ZONAS_NAUFRAGIO (idZona),
CONSTRAINT porcentaje_check CHECK (0<porcentaje <100)
);

/*Modificar para facilitar cambiar insert into*/
alter table edad_historia add tipo varchar(20) not null;
alter table edad_historia modify fechaFin int unsigned;
alter table edad_historia modify fechaInicio int unsigned; 

/*Las zonas de naufragio y Edades*/

insert into edad_historia (nombreEdad, fechaInicio, fechaFin, tipo) values ( "Edad Antigua" , '0', '0401', "Generales");
insert into edad_historia (nombreEdad, fechaInicio, fechaFin,tipo) values ( "Edad Media" , '0401', '1492', "Generales");
insert into edad_historia (nombreEdad, fechaInicio, fechaFin,tipo) values ( "Edad de los Descubrimientos", '1401', '1601', "Especificas");
insert into edad_historia (nombreEdad, fechaInicio, fechaFin,tipo) values ( "Edad Moderna" , '1492', '1789', "Generales");
insert into edad_historia (nombreEdad, fechaInicio, fechaFin,tipo) values ( "Edad Contemporánea" , '1789', '2023', "Generales");
alter table zonas_naufragio modify nombreZona varchar(60);
insert into zonas_naufragio (nombreZona)values("Costa Occidental de Norteamérica y Pacífico Norte");
insert into zonas_naufragio (nombreZona)values("Golfo de México y Bermudas");
insert into zonas_naufragio (nombreZona)values("Caribe");
insert into zonas_naufragio (nombreZona) values ("Sudamérica, África Occidental y Atlántico sur");
insert into zonas_naufragio (nombreZona) values ("Costa oriental de Norteamérica");
insert into zonas_naufragio (nombreZona) values ("Europa del Norte y Atlántico Norte");
insert into zonas_naufragio (nombreZona) values ("Reino Unido e Irlanda");
insert into zonas_naufragio (nombreZona) values ("Bahía de Vizcaya al sur del Mar del Norte");
insert into zonas_naufragio (nombreZona) values ("España Atlántica, noroeste de África y Azores");
insert into zonas_naufragio (nombreZona) values ("Mediterráneo");
insert into zonas_naufragio (nombreZona) values ("Madagascar y África Oriental");
insert into zonas_naufragio (nombreZona) values ("Océano Índico, Mar Rojo y Golfo Pérsico");
insert into zonas_naufragio (nombreZona) values ("África del sur");
insert into zonas_naufragio (nombreZona) values ("India, Sri Lanka y Bahía de Bengala");
insert into zonas_naufragio (nombreZona) values ("Australia y Nueva Zelanda");
insert into zonas_naufragio (nombreZona) values ("Islas de Asia y Filipinas");
insert into zonas_naufragio (nombreZona) values ("Malasia y Sumatra");
insert into zonas_naufragio (nombreZona) values ("Mar del Sur de China y Golfo de Tailandia");
insert into zonas_naufragio (nombreZona) values ("Japón, Corea y Este de China");
INSERT INTO tipo_rescate (idTipoRescate, Categoria, MaterialNecesario) VALUES (-1,"General", "no requiere nada específico en el rescate, se evalúa a la vuelta");
/*Para empezar desde 0*/
update tipo_rescate set idTipoRescate=0 where idTipoRescate = -1;

INSERT INTO tipo_rescate ( Categoria, MaterialNecesario) VALUES ("Metales preciosos", "oro y plata. Requieren caja fuerte");
insert into tipo_rescate (Categoria, MaterialNecesario) values ("Joyas","requieren de limpieza con productos y herramientas específicos");
insert into tipo_rescate (Categoria, MaterialNecesario) values ("Objetos valiosos", "necesitan un equipo de restauración.");
insert into tipo_rescate (Categoria, MaterialNecesario) values ("Documentos", "requieren de tratamiento con productos químicos especiales para evitar
que se deshagan");
insert into tipo_rescate (Categoria, MaterialNecesario) values ("Metales tóxicos", "requieren contenedores especiales");


/*3. Información fidedigna*/
ALTER TABLE NAUFRAGIO ADD casillaMapa VARCHAR(20) not null; 
/*CasillaMapa antes de idZonaNufragio*/
alter table naufragio modify column casillaMapa varchar (20) after idZonaNaufragio; 
/*idTipoRescate no es necesario*/
alter table naufragio modify column idTipoRescate integer;
/*Cambio de nombre . Cambiar el nombre de atributo en NAUFRAGIO es 325 linea */
alter table rescate_realizado rename RescateRealizado;
alter table rescaterealizado modify miembrosImplicados varchar (50);
alter table tipo_rescate rename tipo_cargamento;
alter table tipo_cargamento rename column idTipoRescate to idTipoCargamento; 
/*Quíta La edad de los Descubrimiento */
delete from edad_historia where idEdad = 3;
/*Rescate_Realizado crear tabla*/
alter table rescaterealizado add column exito varchar (10) check (exito in ('s', 'n'));
alter table rescaterealizado add column notas varchar (50);

/*4. Los Naufragios*/
INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("San Agustín", "Española", "Norte de la Bahía de San Francisco, California, EUA", 1, "M6", '1595-01-01', "Manila a Acapulco", "Porcelana y oro", "n", 3);

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate)
VALUES
("Pacific", "Estadounidense", "65 km al sur del cabo Flattery, Washington, EUA", 1, "M4", '1875-04-11', "Victoria (Canadá) a San Francisco (EUA)", "Tesoro valorado en 80000 dólares", "n", 1);

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, infoRescate)
VALUES
("Islander", "Canadiense", "Steven's Passage, cerca de Juneau, Alaska, EUA", 1, "L2", '1901-08-15', "Alaska (EUA) a Vancouver (Canadá)", "Oro valorado en 3 millones de dólares", "s", "Rescate parcial durante los años 30");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate)
VALUES
("Restos en Agana", "Española", "Puerto de Agana, Guam", 1, "B9", '1686-01-01', "Acapulco a Manila", "Monedas de plata", "n", 1);


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Barco de Erasso", "Pirata", "Frente al oeste de Cuba", 2, "E7", '1577-01-01', "Botín y tesoro", "n", 1, "Contenía botín de los piratas Barker, Coxe y Roche");


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate, notas)
VALUES
("Sea Venture", "Británica", "Punta oriental de Bermuda", 2, "L3", '1609-01-01', "Reino Unido a las Américas", "Monedas y artefactos", "n", 3, "Capitán: George Somers. Base de La Tempestad, de Shakespeare.");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("Barco de Lord Belhaven", "Británica", "Bermuda", 2, "L3", '1721-01-01', "Reino Unido a Barbados", "Objetos valiosos de plata", "n", 3);

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Gallega", "Española", "Río Belén, Santa María de Belén, Panamá", 3, "E9", '1503-01-01' , "Artefactos, seda", "n", 3, "Una de las naves perdidas por Colón en su 4 viaje. Rumor: tapiz de oro y seda con la ruta a El Dorado");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio,cargamento, hayRescate, infoRescate, idTipoRescate)
VALUES
("St Anthony", "Francesa", "8.50N, 77.25O", 3, "F9", '1698-12-25' , "60.000 piezas de ocho en oro y plata", "s", "Rescate parcial al naufragar", 1);

-- Sudamérica, África occidental y Atlántico Sur

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("Empire Kohinor", "Británica", "240km SO de Monrivia, Liberia, 60.20N 16.30O", 4, "I1", '1917-07-02', "Table Bay, Sudáfrica a Reino Unido", "Dos paquetes de diamantes", "n", 2);

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Manuela", "Británica", "Estrecho de Magallanes", 4, "C9", '1850-04-01',  "Tesoro: 46000 pesos y la Máscara de Mictlantecuhtli", "n", 3, "Capitán y tripulación fueron recogidos por el Taboga. Insisten en que Mictlantecuhtli hundió el barco por robar su máscara. Murieron todos en un periodo de menos de un año");


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, infoRescate, idTipoRescate)
VALUES
("Sakkarah", "Alemana", "Guamblin o isla de Socorro, Chile", 4, "B8", '1902-05-13', "Valparaíso a Hamburgo, Alemania", "Oro y metálico valorado en 1.5m dólares", "s","Rescate parcial al naufragar",1);


-- Costa oriental de Norteamérica
INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, infoRescate, idTipoRescate)
VALUES
("Leónidas", "Británica", "Costa de Cabo Bretón, isla de Scatarie (Canadá)", 5, "I6", '1932-08-28',  "170 cajas de monedas", "s", "Rescate parcial: 25 cajas de monedas", 1);

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Black Hawk", "Estadounidense", "Lago Michigan, 6km al norte de Frankfort, Michigan, EUA", 5, "D7", '1862-09-01', "Metálico y cristales de color", "n", 3, "Cristales de color: vidriera de catedral");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, hayRescate, idTipoRescate)
VALUES
("Samoa", "Noruega", "145 km frente a la costa de Virginia, EUA, 37.3N, 72.10O", 5, "G9", '1918-06-14', "Sudáfrica a Nueva York, Plata", "n", 1);

-- Europa del Norte y Atlántico Norte
INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate, notas)
VALUES
("Flota de Erik Raudi", "Norte Europa", "Costa oriental de Groenlandia", 6, "E3", '0986-01-01', "Islandia a Groenlandia", "Objetos valiosos", "n", 3, "Primer viaje de colonización a Groenlandia");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("Barco del obispo Olaf", "Noruega", "Cabo Hitarnes, Islandia", 6, "G2", '1266-01-01', "Igaliku, Groenlandia, a Islandia", "Marfil de morsa", "n", 0);


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Vrouw María", "Holandesa", "Cerca de Turku, Finlandia", 6, "K3", '1771-10-04', "Holanda a Federación Rusa", "Pinturas", "n", 3, "Colección Braamcamp de pintura para Catalina la Grande, zarina de Rusia");


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate, notas)
VALUES
("Titanic", "Británica", "41.43.45N 49.46.50O", 6, "D6", '1912-04-15', "Southampton (Inglaterra) a Nueva York", "Artefactos y artículos personales", "s", 0, "Interés puramente coleccionista, rescate demasiado complicado");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("Soekaboemi", "Holandesa", "47.25N 25.20O", 6, "F5", '1942-12-28', "Glasgow (Escocia) a Salvador (Brasil)", "Piedras preciosas", "n", 2);

INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta,  hayRescate, idTipoRescate) 
VALUES ("Samoa", "Noruega", "145 km frente a la costa de Virginia, EUA, 37.3N, 72.10O", 5, "G9", '1918-06-14', "Sudáfrica a Nueva York, Plata", "n", 1);

-- Europa del Norte y Atlántico Norte
INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Flota de Erik Raudi", "Norte Europa", "Costa oriental de Groenlandia", 6, "E3", '0986-01-01', "Islandia a Groenlandia", "Objetos valiosos", "n", 3, "Primer viaje de colonización a Groenlandia");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate)
VALUES
("Barco del obispo Olaf", "Noruega", "Cabo Hitarnes, Islandia", 6, "G2", '1266-01-01', "Igaliku, Groenlandia, a Islandia", "Marfil de morsa", "n", 0);


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Vrouw María", "Holandesa", "Cerca de Turku, Finlandia", 6, "K3", '1771-10-04', "Holanda a Federación Rusa", "Pinturas", "n", 3, "Colección Braamcamp de pintura para Catalina la Grande, zarina de Rusia");


INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas)
VALUES
("Titanic", "Británica", "41.43.45N 49.46.50O", 6, "D6", '1912-04-15', "Southampton (Inglaterra) a Nueva York", "Artefactos y artículos personales", "s", 0, "Interés puramente coleccionista, rescate demasiado complicado");

INSERT INTO Naufragio (
nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate)
VALUES
("Soekaboemi", "Holandesa", "47.25N 25.20O", 6, "F5", '1942-12-28', "Glasgow (Escocia) a Salvador (Brasil)", "Piedras preciosas", "n", 2);

--
INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio,
idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,
idTipoRescate, notas) 
VALUES("Navío de Flossi", "Vikinga", "Sonda de Westray, Orkney, Escocia",
7, "N3", '1501-01-01', "Noruega a Islandia", "Artículos valiosos,
dinero", "n", 3, "El barco se hundió por ir sobrecargado.");




INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio,
idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,
 idTipoRescate, notas) 
VALUES("Pegasus", "Británica", "Goodwind Sands, Inglaterra",
 8, "7", '1598-01-01', "Puerto Rico a Reino Unido", "Tesoro", "n", 1, "Viaje número 12 del Duque de Cumberland, al regreso del saqueo de Puerto Rico");


-- Bahía vizcaya a sur del mar del Norte
INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio,
idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas) 
VALUES
("Nathaniel", "Británica", "725 km frente a Lizard Point, Inglaterra", 8 ,"F5", '1685-02-10', "India a Reino Unido", "Joyas y porcelana", "n", 2, "Nave de la compañía de las Indias Orientales");


INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Telemaque", "Francesa", "Estuario del río Sena, frente a Quillebeuf, Francia", 8, "M3", '1790-01-01', "Artículos valiosos, oro", "n", 3, "Aristocracia francesa a bordo, huyendo de la revolución");


-- España atlántica, noroeste de África y Azores

INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Nuestra Señora de Guía", "Española", "Sur de Terceira, Azores", 9, "C8", '1589-01-01', "Veracruz (México) a España", "Tesoro", "n", 1, "El barco fue hundido por corsarios ingleses de la expedición del Duque de Cumberland");

INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Nuestra Señora de Guía", "italiano", "Sur de Terceira, Azores", 9, "C8", '1589-01-01', "Veracruz (México) a España", "Tesoro", "n", 2, "El barco fue hundido por corsarios ingleses de la expedición del Duque de Cumberland");

INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Nuestra Señora de Guía", "Española", "Sur de Terceira, Azores", 9, "C8", '1589-01-01', "Veracruz (México) a España", "Tesoro", "n", 3, "El barco fue hundido por corsarios ingleses de la expedición del Duque de Cumberland");
INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate) 
VALUES("Tholen", "Holandesa", "naufraga a 117km del Estrecho de Gibraltar hacia Cádiz", 9, "M5", '1687-01-01', "llevaba oro", "n");


-- Mediterráneo
INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate,idTipoRescate, notas) 
VALUES("Nave de San Pablo", "Griega", "Sonda de Westray, Orkney, Escocia", 10, "N3", '1101-06-18', "Noruega a Islandia", "Artículos valiosos, dinero", "n", 3, "El barco se hundió por ir sobrecargado.");


-- Madagascar y África oriental
INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Samaritan", "Inglesa", "Comoros", 11, "F5", '1615-01-01',  "Botín de navíos mongoles, oro y plata", "n", 1, "Parte de una empresa corsaria, patrocinada por Endymion Potter");


INSERT INTO Naufragio ( nombre, procedencia, lugarNaufragio, idZonaNaufragio, casillaMapa, fechaNaufragio, ruta, cargamento, hayRescate, idTipoRescate, notas) 
VALUES("Soleil D'Orient", "Francesa", "Cerca del fuerte Dauphin, SE de Madagascar", 11, "F9", '1681-01-01', "Tailandia a Francia", "Objetos valiosos del rey de Siam a Luis XIV de Francia", "n", 3, "Buscado en los 90, aún no descubierto");
/*Cambiar nombre del atributo en Naufragio*/
alter table naufragio rename column idTipoRescate to idTipoCargamento;


INSERT INTO RescateRealizado(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Orange", 13, '2013-10-15', "Silvia (capitana), Paco, Imanol");

INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Orange", 13, '2013-11-13', 'Silvia (capitana), Paco, Imanol');

INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Dageraad", 13, '2013-12-03', 'Silvia (capitana), Paco, Imanol');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Matoaka", 15, '2015-02-01', 'Iván (capitán), Al, Peter, Paco');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados, exito, notas)
VALUES
("Niágara", 15, '2016-06-21', 'Paco (capitán), Daniel, Imanol, Al', 'n', 'se rompió el taladro submarino');

INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Niágara", 15, '2016-07-24', 'Paco (capitán), Daniel, Imanol, Al');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Sultana", 16, '2017-12-28', 'Silvia (capitana), Boreal, Nadia, Lucía');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Oneida", 19, '2018-03-15', 'Iván (capitán), Al, Peter, Paco');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Toonan", 19, '2018-05-04', 'Iván (capitán), Al, Peter, Paco');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Santa Croix", 19, '2019-07-10', 'Paco (capitán), Daniel, Lucía, Max');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Aliakmon Runner", 12, '2019-10-03', 'Silvia (capitana), Boreal, Nadia, Nora');


INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados)
VALUES
("Breslau", 8, '2020-03-10', 'Imanol (capitán), Daniel, Lucía, Boreal');

INSERT INTO RescateRealizado
(nombreBarco, idZonaNaufragio, fechaRescate, miembrosImplicados, exito, notas)
VALUES
("Breslau", 8, '2018-04-04', 'Iván (capitán), Al, Peter, Paco', 'n', 'Tifón en la zona');

/*5. Noticias*/

update naufragio set procedencia ='Vikinga' where fechaNaufragio > '0793-01-01' and fechaNaufragio < '1300-01-01';

insert into naufragio (nombre, procedencia, lugarNaufragio, idZonaNaufragio,hayRescate, infoRescate,notas) values 
("Tholen", "Vikinga", "Atlántico Norte", 6,"s", "Rescate parcial","Se han encontrado dos cajas de doblones de oro"); 

insert into naufragio (nombre, procedencia, lugarNaufragio, idZonaNaufragio,hayRescate, infoRescate,notas) values 
("San Agustín", "Vikinga", "Atlántico Norte", 6,"s", "Rescate parcial","Se han encontrado varias piezas de porcelana");

insert into naufragio (nombre, procedencia, lugarNaufragio, idZonaNaufragio,hayRescate, infoRescate,notas) values 
("Samoa", "Vikinga", "Atlántico Norte", 6,"s", "Rescate parcial","Se han encontrado un baúl con joyería de plata"); 
 
/*6. A trabajar*/
select distinct nombre, notas from naufragio where notas like '%Cumberland%'; 

select nombre, procedencia from naufragio where procedencia = 'Vikinga';

select distinct nombre, procedencia from naufragio where ruta like '%España%' and not procedencia = 'Española';

select nombre, fechaNaufragio as "Edad de los Descubrimientos" from naufragio where fechaNaufragio > '1401-01-01' and fechaNaufragio < '1701-01-01';

select nombre, fechaNaufragio as "Antes de 1482", idTipoCargamento as "Tipo" from naufragio where idTipoCargamento =2 OR idTipoCargamento=3 and fechaNaufragio<'1789-01-01' and procedencia = 'Francesa' ;
 
/*select distinct nombre as "Barco", idZonaNaufragio as "zona", idTipoCargamento as "tipo de cargamente ", count (idNaufragio) zona from naufragio 
where idTipoCargamento = 1 or idTipoCargamento =0 group by nombre, idZonaNaufragio,idTipoCargamento;
*/
SELECT distinct nombre, idZonaNaufragio, idTipoCargamento, COUNT(DISTINCT idNaufragio) AS num_barcos
FROM naufragio where idTipoCargamento = 1 or idTipoCargamento =0
GROUP BY nombre, idZonaNaufragio, idTipoCargamento order by idZonaNaufragio desc ;

select naufragio.idTipoCargamento, zonas_naufragio.nombreZona from naufragio join zonas_naufragio on naufragio.idZonaNaufragio = zonas_naufragio.idZona where naufragio.idTipoCargamento =2;

select miembrosImplicados, idZonaNaufragio, count(*) as Rescate from rescaterealizado group by miembrosImplicados, idZonaNaufragio order by Rescate desc limit 1;

select naufragio.idZonaNaufragio, zonas_naufragio.nombreZona, count(*) as cantidad 
FROM naufragio join zonas_naufragio on naufragio.idZonaNaufragio = zonas_naufragio.idZona 
group by naufragio.idZonaNaufragio order by cantidad desc limit 1;

select naufragio.idTipoCargamento, zonas_naufragio.nombreZona, count(*) as cantidad 
from naufragio
join zonas_naufragio on naufragio.idZonaNaufragio = zonas_naufragio.idZona where naufragio.idTipoCargamento =3
group by naufragio.idZonaNaufragio
order by cantidad desc limit 1 ;
