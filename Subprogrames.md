# 1. Subprogrames

## ENUNCIATS DE FUNCIONS

### Ex. 1
Fes una funció anomenada spData, tal que donada una data en format
MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA

Exemple : SELECT spData('1988-12-01') => 01-12-1988

``` mysql
DELIMITER //
DROP FUNCTION IF EXISTS spData;
CREATE FUNCTION spData(pData DATE)
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE vRetorn VARCHAR(10);
    SET vRetorn = DATE_FORMAT(pData, '%d-%m-%Y');
    RETURN vRetorn;
END //
DELIMITER ;

 SELECT spData('1988-12-01');
 ````

### Ex. 2
Fes una funció anomenada spPotencia, tal que donada una base i un
exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW.

Exemple : SELECT spPotencia(2,3) => 8
``` mysql
DELIMITER //
DROP FUNCTION IF EXISTS spPotencia;
CREATE FUNCTION spPotencia(pBase INT, pExponent INT)
RETURNS BIGINT
DETERMINISTIC
BEGIN
    DECLARE vResultat INT DEFAULT 1;
    DECLARE i INT;
    IF pExponent < 0 THEN
        SET pBase = 1 / pBase;
        SET pExponent = -pExponent;
    END IF;
    SET i = 1;
    WHILE i <= pExponent DO
        SET vResultat = vResultat * pBase;
        SET i = i + 1;
    END WHILE;
    RETURN vResultat;
END //
DELIMITER ;

SELECT spPotencia(2,3);
```

### Ex. 3

Fes una funció anomenada spIncrement que donat un codi d’empleat i un
% de increment, ens calculi el salari sumant aquest percentatge.

Per exemple, suposem que l’ empleat amb id_empleat = 124 té un salari de 1000

Exemple: SELECT spIncrement(124,10) obtindriem -> 1100

``` mysql
DELIMITER //
DROP FUNCTION IF EXISTS spIncrement;
CREATE FUNCTION spIncrement(pIdEmpleat INT, pIncrementPercent INT)
RETURNS DECIMAL(10,2)
NOT DETERMINISTIC READS SQL DATA
BEGIN
    DECLARE pSalariActual DECIMAL(10,2);
    DECLARE vNouSalari DECIMAL(10,2);


    SELECT salari INTO pSalariActual
    FROM empleats
    WHERE empleat_id = pIdEmpleat;

    SET vNouSalari = pSalariActual + (pSalariActual * (pIncrementPercent / 100));

    RETURN vNouSalari;
END //
DELIMITER ;


SELECT spIncrement(124,10);
```

## ENUNCIATS DE PROCEDIMENTS

### Ex. 2

Fes un procediment que intercanvii el sou de dos empleats passats per
paràmetre.

``` mysql
DROP PROCEDURE IF EXISTS spSwapSous;
DELIMITER //
CREATE PROCEDURE spSwapSous (IN pEmpleatId1 INT, IN pEmpleatId2 INT)
BEGIN

	DECLARE vSalariTmp DECIMAL (8,2);

	IF (SELECT empleat_id
			FROM empleats
		WHERE empleat_id = pEmpleatId1) IS NOT NULL
        AND
        (SELECT empleat_id
			FROM empleats
		WHERE empleat_id = pEmpleatId2) IS NOT NULL
        THEN
	SELECT salari INTO vSalariEmp1
		FROM empleats
	WHERE empleat_id = pEmpleatId1;

	UPDATE empleats 
		SET salari = (SELECT salari
							FROM empleats
						WHERE empleat_id = pEmpleat2)
	WHERE empleat_id = pEmpleatId1;

	UPDATE empleats
		SET salari = vSalariEmp1
	WHERE empleat_id = pEmpleatId2;
    
END 
``` 

### Ex. 3

Fes un procediment que donat dos Ids d'empleat assigni el codi de
departament del primer en el segon.


``` mysql
DELIMITER //
CREATE PROCEDURE spSwapDepId (IN pEmpleatId1 INT, IN pEmpleatId2 INT )
BEGIN

	-- Obtenir departament_id de pEmpleatId1
    
    -- modificar departament_id de pEmpleatId2 
    
    UPDATE empleats
		SET departament_id = (SELECT departament_id
							FROM empleats
						WHERE empleat_id = pEmpleatId1)
		WHERE empleat_id = pEmpleatId2;
END // 

```

### Ex. 4

Fes un procediment que donat dos codis de departament assigni tots els
empleats del segon en el primer. Un cop executat el procediment el departament que
correspont en el segon paràmetre ha de quedar desert/sense cap empleat.
``` mysql
DROP PROCEDURE IF EXISTS spMoureEmpleats;
DELIMITER//
CREATE PROCEDURE spMoureEmpleats (IN pDepId1 INT, IN pDepId2 INT)
BEGIN
	
    IF pDepId1 IS NOT NULL THEN
		UPDATE  empleats
			SET departament_id = pDepid1
		WHERE departament_id = pDepId2;
	END IF;
    
END;

``` 
### Ex. 5

Fes un procediment per mostrar un llistat dels empleats. Volem veure el
id_empleat, nom_empleat, nom_departament i el nom de la localització del departament.
``` mysql
DROP PROCEDURE IF EXISTS spMostrarEmpleats;
DELIMITER//
CREATE PROCEDURE spMostrarEmpleats ()
BEGIN

	SELECT  e.id_empleat, e.nom AS nom_empleat, d.nom
		FROM empleats e
	JOIN departaments d ON e.departament_id = d.departament_id
    
    
END;
```
