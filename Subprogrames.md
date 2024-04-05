# 1. Subprogrames

### Ex. 1
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











