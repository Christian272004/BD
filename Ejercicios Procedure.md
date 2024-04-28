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
