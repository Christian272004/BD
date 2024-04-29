# ExercicisUF3
Ejercicios de base de datos de la UF3 

La base de datos para realizar estos ejercicios es la de [recursos humanos](https://www.sapalomera.cat/moodlecf/pluginfile.php/43573/mod_resource/content/0/bbdd_rrhh_v2%20%286%29.sql)

Exercici 1 
```mysql
DELIMITER //
DROP PROCEDURE IF EXISTS CrearCopiaEmpleats;
CREATE PROCEDURE CrearCopiaEmpleats(IN pId_empleat INT)
BEGIN
    DECLARE vEmpleadoId INT;
    CREATE TABLE IF NOT EXISTS empleat_copia(
	id_empleat		INT,
    nom				VARCHAR(20),
    cognoms			VARCHAR(25)
	);
	CREATE TABLE IF NOT EXISTS logs_usuaris ( 
				usuari    VARCHAR(100), 
                data      DATETIME, 
                taula     VARCHAR(64),       
                accio     VARCHAR(20),       
                valor_pk  VARCHAR(200),       
                error     INT(4) 
	);
    
    IF pId_empleat IN (SELECT empleat_id FROM empleats) THEN
		INSERT INTO empleat_copia(id_empleat,nom,cognoms)
			SELECT empleat_id,nom,cognoms
						FROM empleats
					WHERE empleat_id = pId_empleat;
	ELSE
		IF pId_emplet NOT IN (SELECT valor_pk FROM logs_usuaris) THEN
			INSERT INTO logs_usuaris(usuari,data,taula,accio,valor_pk,erro)
				VALUES (CURRENT_USER(),NOW(),'empleat_copia','COPIA_EMPL',pId_empleat,1);
		ELSE 
			INSERT INTO logs_usuaris(usuari,data,taula,accio,valor_pk,erro)
				VALUES (CURRENT_USER(),NOW(),'empleat_copia','COPIA_EMPL',pId_empleat,2);
		END IF;
	END IF;
END //
DELIMITER ;
```
Exercici 2 -  Crea la taula següent: CREATE TABLE categories ( 
					codi CHAR(2),
     					PRIMARY KEY, 
	  				nom VARCHAR(30), 
       					quantitat SMALLINT UNSIGNED ); 
Modifica la funció sp_Categoria creada a l'activitat de subprogrames per tal de que retorni un codi de categoria amb la nomenclatura següent: 
	C1 = Auxiliar 
	C2 = Oficial de Segona 
	C3 = Oficial de Primera 
	C4 = Que es jubili! 
Crea un subprograma encarregat de comptabilitzar el número d'empleats de cada categoria i assigni aquest valor en el camp quantitat de la taula categories destinada a guardar aquesta informació. Realitza aquesta acció amb l’ajuda d’un cursor.
```mysql

```
Exercici 3 -  Afegeix un camp de tipus DECIMAL  a la taula departaments anomenat salari_avg. Crea un subprograma que ompli aquest camp amb la mitjana de salaris dels empleats del departament corresponent. Nota: Fes-lo utilitzant
```mysql

```
Exercici 4 -  Per tal de saber quins són els «Pringats» de cada departament es demana crear una nova taula anomenada pringats a on hi hagi l'identificador de l'empleat i l'identificador del departament el qual pertany. En aquesta taula s'hi guardaran tots els «pringats» de cada departament. CREATE TABLE pringats ( empleat_id      INT, departament_id INT,  CONSTRAINT PK_PRINGATS PRIMARY KEY (departament_id, empleat_id) ) ; Aquesta taula s'ha d'omplir mitjançant un subprograma mitjançant cursors. Aquest ha d'esborrar el contingut de la taula a l'inici de la seva execució. 
```mysql

```
Exercici 5 -  Modifica el subprograma anterior o creen un de nou de tal manera que el subprograma només afegeixi a la taula de pringats només aquells que no hi existeixin prèviament. 
```mysql

```
Exercici 6 -  Es vol segregar els empleats pel mes de l'any que van ser contractats. Per això es vol crear una nova taula anomenada empleats_segregats que conté els camps id_empleat i el número del mes de l'any quan va ser contractat. Crea un subprograma que mitjançant cursors realitzi aquesta feina. 
```mysql

```
Exercici 7 -  Afegeix un nou camp a la taula FEINES a on volem guardar la quantitat de treballadors que han treballat d'aquest perfil en algun moment de la seva vida laboral dins l'empresa. Crea un subprograma que mitjançant cursors ompli aquest nou camp.
