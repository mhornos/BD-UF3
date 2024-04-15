# 1. Handlers i Cursors

### Ex. 1
Fes un procediment tal que donat un codi d’empleat, crei una taula
anomenada empleat_copia ( en cas que no existeixi ) amb els camps:
id_empleat, nom i cognoms. 

A continuació ha de fer una còpia (INSERT) de les dades de l’empleat a la
nova taula. 

En cas que l’empleat indicat no existeixi en la taula empleats, registrarà la
incidència en la taula logs_usuaris amb la següent informació.

``` mysql
  DELIMITER //
DROP PROCEDURE IF EXISTS spCopiaEmpleats;
CREATE PROCEDURE spCompiaEmpleats(IN pEmpleatId INT)
BEGIN
	DECLARE vEmpleatId INT DEFAULT NULL;
    
    DECLARARE CONTINUE HANDLER FOR NOT FOUND  INSERT INTO log_usuaris (usuari,data,taula,accio,valor_pk,error)
												VALUES (CURRENT_USER(), NOW(), 'empleat_copia', 'COPIA_EMPL', pEmpleatId, 1);
                                                

	DECLARE CONTINUE HANDLER FOR SQLSTATE '23000' INSERT INTO log_usuaris (usuari,data,taula,accio,valor_pk,error)
												VALUES (CURRENT_USER(), NOW(), 'empleat_copia', 'COPIA_EMPL', pEmpleatId, 2);

	CREATE TABLE IF NOT EXISTS empleat_copia(
	empleat_id 	INT PRIMARY KEY,
	nom 		VARCHAR(20),
	cognoms		VARCHAR(25)
	);
	
	SELECT empleat_id INTO vEmpleatId
		FROM empleats
	WHERE empleat_id = pEmpleatId;

	IF vEmpleatId IS NOT NULL THEN
		INSERT INTO empleat_copia(empleat_id, nom, cognoms)
			SELECT empleat_id, nom, cognoms
				FROM logs_usuaris
			WHERE empleat_id = pEmpleatId
	END IF;	
END  
//
 ```
