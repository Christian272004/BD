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
``` Mysql
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
``` Mysql

```

Exercici 4 -  Fes una funció anomenada spPringat, tal que li passem un codi de departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.
``` Mysql
```

Exercici 5 -  Utilitzant la funció spPringat fes una consulta per obtenir de cada departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.
``` Mysql
```

