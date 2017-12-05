# Pràctica 5

## REPLICACIÓ via Binlog


Es vol muntar entorn SGBD MySQL Percona amb rèplica. Es vol tenir un MySQL <i>master</i> a on s'aniran enviant totes les instruccions SQL d'inserció, modificació i esborrat. Es vol tenir un MySQL <i>esclau</i> del <i>master</i> anteriorment esmentat. </br>

Cal que que al realitzar un INSERT en el master veiem les dades a l'esclau al cap d'un instant de temps. </br>

### Configuració <i>Master</i>

</br>

- Realitza una còpia del fitxer de configuració del MySQL /etc/my.conf --> /etc/my.conf.bkp </br>

Copiem l'arxiu utilitzant la comanda de còpia d'arxius de Linux <i><b>cp /etc/my.cnf /etc/my.cnf.bkp</b></i>. </br> ![backup my.cnf](img/2017-11-28_16_20_40.png) </br>

Finalment, comprovem si hem copiat l'arxiu correctament al directori on pertoca. </br> ![ls backup](img/2017-11-28_16_21_43.png) </br> </br>

- Modifica el fitxer <i>/etc/my.conf</i> i activa el paràmetre <i><b>log-bin</b></i> (tal i com vàreu fer a M02) amb el nom: <i><PRIMER LLETRA DEL NOM + 1r COGNOM>rep</i>. </br>

Modifiquem el fitxer <i>my.cnf</i> afegint-hi el paràmetre <i><b>log-bin=enorat_gorriols_atarensi.log</b></i>. </br> ![conf bin-log](img/2017-11-28_16_32_29.png) </br> </br>

- Verifica que el paràmetre server-id té un valor numèric (per defecte és 1). </br>

Hem de reiniciar el servei de MySQL després d'haver fet el canvi anterior per guardar-lo, i un cop ho haguem fet, comprovem el log que hem creat utilitzant la comanda <i><b>mysqlbinlog /usr/mysql/log/enorat_gorriols_atarensi.000001</b></i>. </br> ![comprovacio log](img/2017-11-28_16_41_29.png) </br> </br>

- Verifica que tots els paràmetres de InnoDB estiguin descomentats. </br>

Tots els paràmetres venen descomentats a la versió 5.7 de Percona-Server, per tant, no hem hagut de modificar res. En cas de que volguem comentar algun paràmetre, només hem d'utilitzar el hashtag (<i>#</i>). </br> </br>

- Canvia el paràmetre <i>innodb_log_buffer</i> a 10M. </br>

Entrem al fitxer de configuració i hi afegim el paràmetre <i><b>innodb_log_buffer_size=10MB</b></i>. El log-buffer és un espai de memòria que manté les dades abans de ser escrites en el fitxer físic de redo log. Aquest paràmetre és el que en determina la mida. </br> ![log buffer size](img/2017-11-28_16_53_44.png) </br> </br>

- Canvia o afegeix el paràmetre <i>innodb_log_files_in_group</i> a 2. </br>

Afegim el paràmetre al fitxer de configuració com hem fet ja anteriorment. La sentència hauria de ser <i><b>innodb_log_files_in_group=2</b></i>. Aquest paràmetre determina el número de logs màxim en un grup de logs, i el seu paràmetre mínim és 2. </br> ![files-in-group log](img/2017-11-28_17_25_40.png) </br> </br>

- Aquí parem el servei de MySQL, borrem tots els logs d'InnoDB del directori <i>/var/lib/mysql</i> i tornem a engegar el procés de MySQL. </br>

- Quants fitxers comencen amb el nom <i><PRIMER LLETRA DEL NOM + 1r COGNOM>rep</i> dins el directori <i>/var/lib/mysql</i>? Digues quins són. </br>

Comprovem que hi ha dins la ruta <i>/var/lib/mysql</i>, i s'hi poden veure dos fitxers que comencen per <b>enorat_gorriols_atarensi</b>:

  1. <i><b>enorat_gorriols_atarensi.000001</b></i>: És el fitxer que inclou les dades de log, és a dir, el log en sí. 
  2. <i><b>enorat_gorriols_atarensi.index</b></i>: És el fitxer que indexa els logs. 
  
![fitxers rep](img/2017-11-28_17_31_19.png) </br> </br>

- Realitza un instrucció DML, per exemple INSERT, UPDATE o DELETE. </br>

Per fer aquest exemple, hem creat una taula anomenada "pro". Per fer la prova farem varis INSERT dins d'aquesta taula. </br> ![inserts prova](img/2017-11-28_17_43_17.png) </br> 

També hem decidit provar amb un UPDATE a la mateixa taula, per tal de fer més proves i que el resultat sigui més divers. </br> ![updates prova](img/2017-11-28_17_53_09.png) </br> </br>

- Obre un altre terminal i utilitzant l'eina mysqlbinlog mira el contingut del fitxer <PRIMER LLETRA DEL NOM + 1r COGNOM>rep.000001. Quin és el seu contingut? </br>

Per comprovar el contingut del fitxer log on hi ha la informació dela INSERTS i UPDATE anteriors, hem d'utilitzar la sentència <i><b>mysqlbinlog enorat_gorriols_atarensi.000001</b></i>. </br> ![updates prova](img/2017-11-28_18_51_07.png) </br> </br>

- Fes un FLUSH DELS LOGS utilitzant la comanda <b>FLUSH LOGS</b> dins del MySQL. Realitza una comprovació dels logs com a master mitjançant <b>SHOW MASTER LOGS</b>.

La comanda <i>FLUSH LOGS;</i> serveix per generar un nou registre (log). Recordem que en l'anterior exercici, l'extensió del log era <i><b>.000001</b></i>, i després de fer el FLUSH, ens en crearà un altre. Ho podem, comprovar amb la sentència <i>SHOW MASTER LOGS;</i>, que ens mostra els logs que tenim a la nostra BD. </br> ![flush logs](img/2017-11-28_18_52_06.png) </br> 

</br>

### Configuració <i>Slave i Master</i>

</br>

- Realitza una còpia de la màquina virtual a on tinguis SGBD MySQL. Aquesta nova màquina serà que farà d'eslau. </br>

- Esbrina quina IP tenen cadascuna de les màquines (master, slave). </br>

Hem fet la còpia de la màquina MASTER. La màquina MASTER té la IP 192.168.47.10, mentre que la màquina SLAVE té la IP 192.168.47.12. </br> </br>

- Crea un backup de la BD a la màquina master utilitzant la sentència <i><b>$> mysqldump –-user=root –-password=vostrepwd -–master-data=2 sakila > /tmp/master_backup.sql</b></i>. </br>

Utilitzem la sentència. Un cop executada, simplement ens avisarà de que és perillós utilitzar la contrasenya a la línia de comandes. </br> ![backup master](img/2017-11-28_19_17_06.png) </br>

- Edita el fitxer master_backup.sql i busca la línia que comenci per <i><b>--CHANGE MASTER TO...</b></i> i busca els valors MASTER_LOG_FILE i MASTER_LOG_POS. </br>

Editem el fitxer indicat i busquem la línia <i>--CHANGE MASTER TO</i>. Els dos paràmetres que hi haa a la línia són MASTER_LOG_FILE i MASTER_LOG_POS. Aquest paràmetres són els que es coordinen la màquina SLAVE amb la MASTER; en el primer s'hi especifica un log concret, i en el segon 
