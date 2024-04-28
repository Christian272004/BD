# ExercicisUF3
Ejercicios de base de datos de la UF3 

La base de datos para realizar estos ejercicios es la de [recursos humanos](https://www.sapalomera.cat/moodlecf/pluginfile.php/43573/mod_resource/content/0/bbdd_rrhh_v2%20%286%29.sql)

# Ejercicios de Funciones
Exercici 1 -  Fes una funció anomenada spData, tal que donada una data en format MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA 

Exemple : SELECT spData('1988-12-01') => 01-12-1988
``` Mysql
DELIMITER //
CREATE FUNCTION spData(pDate DATE) RETURNS VARCHAR(10)
BEGIN
  DECLARE fecha VARCHAR(10);
  SELECT DATE_FORMAT(pDate, '%d-%m-%Y') INTO fecha;
  RETURN fecha;
END;
//
DELIMITER ;
```
Exercici 2 -  Fes una funció anomenada spPotencia, tal que donada una base i un exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW. 

Exemple : SELECT spPotencia(2,3) => 8
```Mysql
DELIMITER //
CREATE FUNCTION spPotencia(base INT, exponente INT) RETURNS INT
BEGIN
  DECLARE resultado INT;
  SET resultado = 1;
  WHILE exponente > 0 DO
    SET resultado = resultado * base;
    SET exponente = exponente - 1;
  END WHILE;
  RETURN resultado;
END;
//
DELIMITER ;
```
Exercici 3 -  Fes una funció anomenada spIncrement que donat un codi d’empleat i un % de increment, ens calculi el salari sumant aquest percentatge. 

Per exemple, suposem que l’ empleat amb id_empleat = 124 té un salari de 1000 

Exemple: SELECT spIncrement(124,10) obtindriem -> 1100
```Mysql
DELIMITER //
DROP FUNCTION IF EXISTS spIncrement//
CREATE FUNCTION spIncrement(pCodiempleat int,pIncrement DECIMAL(5,2)) RETURNS INT
BEGIN
	DECLARE vSalari INT;
    
	SELECT (salari + (salari * (pIncrement/100))) INTO vSalari
		FROM empleats
	WHERE empleat_id = pCodiempleat;
    RETURN vSalari;
    
END //
DELIMITER ;
```

Exercici 4 -  Fes una funció anomenada spPringat, tal que li passem un codi de departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.
```Mysql
DELIMITER //
DROP FUNCTION IF EXISTS spPringat;
CREATE FUNCTION spPringat(pDepartament_id INT) RETURNS INT
BEGIN
	DECLARE vEmpleat INT;
    DECLARE vSalariMin INT;
    SELECT MIN(salari) INTO vSalariMin
		FROM empleats
	WHERE departament_id = pDepartament_id;
    SELECT empleat_id INTO vEmpleat
		FROM empleats
	WHERE departament_id = pDepartament_id AND salari = vSalariMin;
    RETURN vEmpleat;
END//
DELIMITER ;
```

Exercici 5 -  Utilitzant la funció spPringat fes una consulta per obtenir de cada departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.
```Mysql
DELIMITER //
DROP FUNCTION IF EXISTS spCategoria;
CREATE FUNCTION spCategoria(pCodiEmpleat INT) RETURNS VARCHAR(30)
BEGIN
	DECLARE vAnys INT;
    DECLARE vCategoria VARCHAR(30);
    SELECT TIMESTAMPDIFF(day,data_contractacio,NOW()) INTO vAnys
		FROM empleats
	WHERE empleat_id = pCodiEmpleat;
    
    IF (vAnys > 0 AND vAnys < 1) THEN 
		SET vCategoria = 'Auxiliar';
    ELSEIF (vAnys > 2 AND vAnys < 10) THEN
		SET vCategoria = 'Oficial de Segona';
	ELSEIF (vAnys > 11 AND vAnys < 20) THEN
		SET vCategoria = 'Oficial de Primera';
	ELSE 
		SET vCategoria = 'Que es Jubilu!';
    END IF;
    RETURN vCategoria;
END//
DELIMITER ;
```

Exercici 7 - Fes una consulta utilitzant la funció anterior perquè mostri mostri de cada
empleat, el codi d’empleat, el nom, els anys treballats i la categoria professional a la que
pertany.
```mysql
SELECT empleat_id, nom,TIMESTAMPDIFF(day,data_contractacio,NOW()), spCategoria(empleat_id) AS Categoria_Profesional
	FROM empleats;
```

Exercici 8 - Fes una funció anomenada spEdat, tal que donada una data per paràmetre
ens retorni l'edat d'una persona. Les dates posteriors a la data d'avui han de retornar 0.
```mysql
DELIMITER //
DROP FUNCTION IF EXISTS spEdat;
CREATE FUNCTION spEdat(pFecha DATE) RETURNS INT
BEGIN
	DECLARE vEdat int;
    SET vEdat = TIMESTAMPDIFF(year,pFecha,NOW());
    IF (vEdat < 0) THEN
		SET vEdat = 0;
    END IF;
    RETURN vEdat;
END //
DELIMITER ;
```
Exercici 9 - Fes una funció que ens retorni el número de directors (caps) diferents tenim.
```mysql
DELIMITER //
DROP FUNCTION IF EXISTS spDirectors;
CREATE FUNCTION spDirectors() RETURNS INT
BEGIN
	DECLARE vDif INT;
	SELECT COUNT(DISTINCT(id_cap)) INTO vDif
		FROM empleats;
	RETURN vDif;
END //
DELIMITER ;
```
Exercici 10 - Quina instrucció utilitzarem si volem veure el contingut de la funció
spPringat?
```mysql
SHOW CREATE FUNCTION spPringat;
```

# Ejercicios de Procedimientos
Exercici 1 -  Fes un procediment que permeti obtenir la data i hora del sistema i l’usuari actual. 
```Mysql
DELIMITER //
CREATE PROCEDURE spInformacion()
BEGIN
  SELECT NOW() AS Tiempo, CURRENT_USER() AS Usuario;
END;
DELIMITER ;
```

Exercici 2 -  Fes un procediment que intercanvii el sou de dos empleats passats per paràmetre.
```Mysql
DELIMITER //
CREATE procedure spSwapSous (IN pEmpleatId1 INT, IN pEmpleatId2 INT)
BEGIN
	DECLARE vSalariTimp DECIMAL(8,2);
    
    SELECT salari INTO vSalariTimp
		FROM empleats
	WHERE empleat_id = pEmpleatId1;
    
    UPDATE empleats
		SET salari = (select salari
						FROM empleats
					  WHERE empleat_id = pEmpleatId2)
	WHERE empleat_id = pEmpleatId1;
    
    UPDATE empleats
		SET salari = vSalariTimp
	WHERE empleat_id = pEmpleatId2;
    END
    DELIMITER ;
```
Exercici 3 - Fes un procediment que donat dos Ids d'empleat assigni el codi de
departament del primer en el segon.
```mysql
DELIMITER //
DROP PROCEDURE IF EXISTS cambiarId;
CREATE PROCEDURE cambiarId(IN pId_primer INT, IN pId_segon INT)
BEGIN
	DECLARE vDep1 INT;
    SELECT departament_id INTO vDep1
		FROM empleats
	WHERE empleat_id = pId_primer;
    UPDATE empleats
    SET departament_id = vDep1
    WHERE empleat_id = pId_segon;
END //
DELIMITER ;
```
Exercici 4 - Fes un procediment que donat dos codis de departament assigni tots els
empleats del segon en el primer. Un cop executat el procediment el departament que
correspont en el segon paràmetre ha de quedar desert/sense cap empleat.
```mysql
DELIMITER //
DROP PROCEDURE IF EXISTS MoverEmpleados;
CREATE PROCEDURE MoverEmpleados(IN pDep1 INT, In pDep2 INT)
BEGIN
	UPDATE empleats
	SET departament_id = pDep1
    WHERE departament_id = pDep2;
END //
DELIMITER ;
```
