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
