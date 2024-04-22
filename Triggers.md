# Exercicis TRIGGERS (part 1)

**Exercici 4: Validació de dades d’entrada**

Mitjançant triggers volem dur el control de dades d’entrada d’una taula. 
Concretament volem dur el control del camp salari de la taula empleats. Aquest 
salari ha de ser un valor dins del rang marcat pels camps salari_min i 
salari_max de la taula feines.
En definitiva, volem controlar que el salari dels empleats estigui dins dels rangs de 
salaris marcats per el tipus de feina que fa l’empleat.

```mysql
DROP TRIGGER IF EXISTS trg_salari_empleats;
DELIMITER //
CREATE TRIGGER trg_salari_empleats BEFORE UPDATE ON empleats FOR EACH ROW
BEGIN
        IF((NEW.salari < (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi)) OR (NEW.salari > (SELECT salari_max FROM feines WHERE feina_codi = NEW.feina_codi))) THEN
                SET NEW.salari = OLD.salari;
    END IF;
END //

CREATE TRIGGER trg_salari_empleats_Insert BEFORE INSERT ON empleats FOR EACH ROW
BEGIN
        IF((NEW.salari < (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi)) OR (NEW.salari > (SELECT salari_max FROM feines WHERE feina_codi = NEW.feina_codi))) THEN
                SET NEW.salari = (SELECT salari_min FROM feines WHERE feina_codi = NEW.feina_codi);
    END IF;
END //
DELIMITER ;
```

**Exercici 5: Manteniment del camp num_empleats**

Afegeix un el camp num_empleats a la taula departaments. Aquest camp 
simbolitza/modela el número d’empleats que té aquell departament.
Implementa mitjançant triggers el manteniment d’aquest camp de manera 
automàtica.

Per exemple si s’afegeix un nou empleat del departament amb codi 10 cal 
augmentar en 1 aquest camp del departament_id = 10

```mysql
DROP TRIGGER IF EXISTS trg_numempleats_departaments;
DELIMITER // 
CREATE TRIGGER trg_numempleats_departaments AFTER INSERT ON departaments FOR EACH ROW
BEGIN
	UPDATE departaments
		SET num_empleats = spObtenirEmpleats(NEW.departament_id)
	WHERE departament_id = NEW.departament_id;

END //
```
