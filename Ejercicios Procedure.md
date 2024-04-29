# ExercicisUF3
Ejercicios de base de datos de la UF3 

La base de datos para realizar estos ejercicios es la de [recursos humanos](https://www.sapalomera.cat/moodlecf/pluginfile.php/43573/mod_resource/content/0/bbdd_rrhh_v2%20%286%29.sql)

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
Exercici 5 - Fes un procediment per mostrar un llistat dels empleats. Volem veure el
id_empleat, nom_empleat, nom_departament i el nom de la localització del departament
```mysql

```
Exercici 6 - Fes un procediment que donat un codi d’empleat, ens doni la informació de
l’empleat ( agafa la informació que creguis rellevant)
```mysql

```
Exercici 7 - Volem fer un registre dels usuaris que entren al nostre sistema. Per fer-ho
primer caldrà crear una taula amb dos camps, un per guardar l’usuari i l’altre per guardar
la data i hora de l’accés.
```mysql

```
Exercici 8 - A continuació feu un procediment sense arguments, de manera que cada
vegada que el crideu, insereixi en aquesta taula l’usuari actual i la data i hora en que s’ha
executat el procediment.
```mysql

```
Exercici 9 - Fes un procediment que ens permeti afegir un nou departament però amb la
següent particularitat: En cas que la localització no existeixi a la taula localitzacions, ens
posarà un NULL en el camp id_localtizacio de la taula departaments. Al procediment li
hem de passar el codi de departament, el nom del departament i el codi de la localització.
```mysql

```
Exercici 10 - Fes un procediment que donat un codi d’empleat, ens posi en paràmetres
de sortida el nom i el cognom. Indica com ho pots fer per comprovar si el procediment et
funciona.
```mysql

```
Exercici 11 - Fes un procediment que ens permeti modificar el nom i cognom d’un
empleat. 
```mysql

```
