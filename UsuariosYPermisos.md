# 4. Usuaris i permisos

### Prova: 
- crear 2 usuaris (usuari1 i usuari2)
- usuari1 desde la propia maquina podra crear, insert, update, delete y select a la BD rrhh
-  usuari2 nomes podra fer selects y nomes pot fer update de la columa salari de la taula empleats a la BD rrhh
-  usuari1 quan es connecti desde qualsevol només podrà fer SELECT de la BD rrhh

```sql
CREATE USER 'usuari1'@'localhost' IDENTIFIED BY 'P@ssw0rd';
CREATE USER 'usuari2'@'localhost' IDENTIFIED BY 'P@ssw0rd';

GRANT CREATE,INSERT,UPDATE,DELETE,SELECT ON rrhh.* TO 'usuari1'@'localhost';

GRANT SELECT ON rrhh.* TO 'usuari2'@'localhost';
GRANT UPDATE(salari) ON rrhh.empleats TO 'usuari2'@'localhost';

GRANT SELECT ON rrhh. * TO 'usuari1'@'%';
```
