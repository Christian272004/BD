# ExercicisUF3
Ejercicios de base de datos de la UF3 

La base de datos para realizar estos ejercicios es la de [recursos humanos](https://www.sapalomera.cat/moodlecf/pluginfile.php/43573/mod_resource/content/0/bbdd_rrhh_v2%20%286%29.sql)

Exercici 4: Validació de dades d’entrada Mitjançant triggers volem dur el control de dades d’entrada d’una taula. Concretament volem dur el control del camp salari de la taula empleats. Aquest salari ha de ser un valor dins del rang marcat pels camps salari_min i salari_max de la taula feines. En definitiva, volem controlar que el salari dels empleats estigui dins dels rangs de salaris marcats per el tipus de feina que fa l’empleat. 
```mysql
DELIMITER //
DROP TRIGGER IF EXISTS salario;
CREATE TRIGGER salario BEFORE INSERT ON empleats FOR EACH ROW
BEGIN
	DECLARE vSalariMin,vSalariMax INT;
    SELECT salari_min,salari_max INTO vSalariMin,vSalariMax
		FROM feines
	WHERE feina_codi = NEW.feina_codi;
    
	IF (NEW.salari < vSalariMin OR NEW.salari > vSalariMax) THEN
		SIGNAL SQLSTATE '43000'
        SET MESSAGE_TEXT ='Error';
	END IF;
END //
DELIMITER ;
DELIMITER //
DROP TRIGGER IF EXISTS salario;
CREATE TRIGGER salario BEFORE UPDATE ON empleats FOR EACH ROW
BEGIN
	DECLARE vSalariMin,vSalariMax INT;
    SELECT salari_min,salari_max INTO vSalariMin,vSalariMax
		FROM feines
	WHERE feina_codi = OLD.feina_codi;
    
	IF (NEW.salari < vSalariMin OR NEW.salari > vSalariMax) THEN
		SIGNAL SQLSTATE '43000'
        SET MESSAGE_TEXT ='Error';
	END IF;
END //
DELIMITER ;
```
Exercici 5: Manteniment del camp num_empleats Afegeix un el camp num_empleats a la taula departaments. Aquest camp simbolitza/modela el número d’empleats que té aquell departament. Implementa mitjançant triggers el manteniment d’aquest camp de manera automàtica. Per exemple si s’afegeix un nou empleat del departament amb codi 10 cal augmentar en 1 aquest camp del departament_id = 10 
```mysql
DELIMITER //
DROP TRIGGER IF EXISTS num_empleados;
CREATE TRIGGER num_empleados AFTER INSERT ON empleats FOR EACH ROW
BEGIN
	IF(NEW.departament_id IN (SELECT departament_id FROM departaments)) THEN
		UPDATE departaments
		SET num_empleats = num_empleats + 1
		WHERE departament_id = NEW.departament_id;
	END IF;
END //
DELIMITER ;
DELIMITER //
DROP TRIGGER IF EXISTS num_empleados;
CREATE TRIGGER num_empleados AFTER DELETE ON empleats FOR EACH ROW
BEGIN
	IF(OLD.departament_id IN (SELECT departament_id FROM departaments)) THEN
		UPDATE departaments
		SET num_empleats = num_empleats - 1
		WHERE departament_id = OLD.departament_id;
	END IF;
END //
DELIMITER ;
```
