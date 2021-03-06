# Pràctica 3

## Activitat 1. REALITZA I/O RESPON ELS SEGÜENTS APARTATS


1. Indica quins són els motors d’emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d’aquesta.</br>

Per veure els motors d'emmagatzematge que podem utilitzar, utilitzarem la comanda <i><b>SHOW ENGINES;</b></i>.
![show engines](img/Screenshot_1.png) </br></br>

2. Com puc saber quin és el motor d’emmagatzematge per defecte? Mostra com canviar aquest paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor MyISAM?

Amb la mateixa comanda anterior,  a la columna "SUPPORT", podem veure els motors d'emmagatzematge actius (<i>YES</i>), els que no ho estàn (<i>NO</i>) i el motor d'emmagatzematge per defecte (<i>DEFAULT</i>). Per canviar el motor d'emmagatzematge per defecte a MyISAM, hem d'editar el fitxer de configuració <i><b>mysqld.cnf</b></i> i posar el paràmetre <i>default-storage-engine=[Nom]</i>. ![canviar engine per defecte](img/Screenshot_2.png) </br>

Per veure si el canvi ha estat correcte, tornem a utilitzar la comanda <i><b>SHOW ENGINES;</b></i>. </br>
![canvi engine correcte](img/Screenshot_3.png) </br></br>

3. Explica els passos per instal·lar i activar l'ENGINE MyRocks. MyRocks és un motor d'emmagatzematge per MySQL basat en RocksDB (SGBD incrustat de tipus clau-valor).</br>

Per instal·lar el motor d'emmagatzematge MyRocks, hem d'utilitzar la comanda <i><b>yum install Percona-Server-rocksdb-57.x86_64</b></i> (el número "57" pot variar segons la versió MySQL que tenim instal·lada). </br> ![instalar myrocks](img/Screenshot_4.png)</br>

    Consell: Per utilitzar comandes de Linux dins del shell de MySQL, podem fer servir \!

Un cop instal·lat l'engine MyRocks, l'hem d'activar. Per fer-ho, només hem de posar la comanda <i><b>ps-admin --enable-rocksdb -u root -p[contrasenya]</b></i>. Més informació sobre MyRocks a Percona Server: https://www.percona.com/doc/percona-server/LATEST/myrocks/install.html</br> ![activar myrocks](img/Screenshot_5.png)</br>

Per finalitzar, comprovem que MyRocks estigui instal·lat i activat utilitzant la comanda <i><b>SHOW ENGINES;</b></i>. </br> ![myrocks activat](img/Screenshot_6.png) </br></br>

4. Importa la BD Sakila com a taules MyISAM. Fes els canvis necessaris per importar la BD Sakila perquè totes les taules siguin de tipus MyISAM. Mira quins són els fitxers físics que ha creat, quan ocupen i quines són les seves extensions. Mostra'n una captura de pantalla i indica què conté cada fitxer.</br>

Per importar una BD amb l'engine que volem, hem de modificar l'esquema DDL de la BD, i posar-hi el motor d'emmagatzematge amb el que volem guardar les taules utilitzant <i><b>ENGINE=MyISAM</b></i>. També és aconsellable tenir MyISAM com a engine predeterminat a l'hora d'importar la BD. </br> ![canviar engine a myisam](img/Screenshot_7.png) </br>

Ara només falta importar l'esquema de la BD Sakila. Per fer-ho, podem utilitzar la comanda <i><b>SOURCE /arrel/schema.sql</b></i> dins la shell de MySQL. </br> ![importar sakila myisam](img/Screenshot_8.png) </br>

Per veure els fitxers que ens ha creat la BD i quant ocupen, utilitzarem la comanda <i><b>ls -ls /var/lib/mysql/sakila</b></i>, que és el directori on es situa l'informació de la nostra BD. </br> ![veure fitxers sakila myisam](img/Screenshot_9.png) </br>

Hi ha 6 extensions diferents, i cada document, segons al seva extensió, una conté certa informació sobre cada taula. </br>

- .frm: Guarda la informació de la estructura de la taula.
- .MYI: Guarda la informació dels índex de la taula.
- .MYD: Guarda les dades de la taula.
- .opt: Guarda la configuració de la DB.
- .TRG: Conté els triggers que estan associats a la taula.
- .TRN: Conté el nom dels triggers associats a la taula. Juntament amb el .TRG, aquest fitxer defineix el trigger.

</br>

## Activitat 2. INNODB part I. REALITZA ELS SEGÜENTS APARTATS


1. Importa la BD Sakila com a taules InnoDB. </br>

Abans d'importar la BD, comprovem que totes les taules de l'esquema Sakila tinguin l'engine de les taules amb InnoDB (igual que a la primera imatge de l'exercici 4 de l'activitat 1). També configurem el fitxer <i>my.cnf</i> amb el paràmetre <i>default-storage-engine=InnoDB</i> perquè el motor per defecte sigui InnoDB (com en l'exercici 2 de activitat 1).

Un cop fets aquests passos, importem la BD amb la comanda <i><b>SOURCE</b></i>. </br> ![importar sakila innodb](img/Screenshot_10.png) </br></br>

2. Quin/quins són els fitxers de dades? A on es troben i quin és la seva mida? </br>

Un cop importada la BD, podem veure els fitxers amb la comanda utilitzada en l'exercici anterior: <i><b>ls -ls /var/lib/mysql/sakila</b></i>, i veiem que alguns fitxers tenen una extensió diferent a quan hem utilitzat MyISAM. </br> ![veure fitxers sakila innodb](img/Screenshot_11.png) </br>

La principal diferència és que no té extensions .MYI ni .MYD, però les extensions .frm, .opt, .TRG i .TRN les manté. L'extensió nova és la següent: </br>

- .ibd: Guarda les dades i els índexs de les taules o de cada taula en cas de que estiguin els fitxers individuals.

Utilitzant l'engine InnoDB, les dades es guardaran a la ruta <i><b>/var/lib/mysql</b></i>, i el fitxer s'anomena <i>ibdata1</i>. </br> ![fitxers dades innodb](img/Screenshot_12.png) </br></br>

3. Canvia la configuració del MySQL perquè: </br>

    1. Canviar la localització dels fitxers del tablespace de sistema per defecte a /discs-mysql/
    2. Tinguem dos fitxers corresponents al tablespace de sistema.
    3. Tots dos han de tenir la mateixa mida inicial (5MB) 
    4. El tablespace ha de creixer de 1MB en 1MB.
    5. Situa aquests fitxers (de manera relativa a la localització per defecte) en una nova localització simulant el següent:
        1. /discs-mysql/disk1/primer fitxer de dades → simularà un disc dur
        2. /discs-mysql/disk2/segon fitxer de dades → simularà un segon disc dur. </br>
        
Per canviar la ruta dels fitxers de tablespace, hem de posar el paràmetre de configuració <i>innodb_data_home_dir</i> amb la ruta que volem al fitxer <i>mysqld.cnf</i>.

Per poder generar dis fitxers i amb la mida que nosaltres volem, hem d'utilitzar el paràmetre <i>innodb_data_file_path</i>. Per definir la mida de cada fitxer, utilitzarem dos punts (:) i per definir un altre fitxer, utilitzarem punt i coma (;). També podem definir-hi la ruta, juntament amb el nom. En aquest cas hem creat un fitxer al directori <i><b>disc1/</b></i> i l'altre a <i><b>disc2/</b></i>. Com a mínim ha de ser de 5MB.

Per poder canviar la mida de l'autoincrement del nostre tablespace, utilitzarem el paràmetre <i>innodb_autoextend_increment</i>. Per defecte, si no confgurem aques paràmetre, s'autoincrementarà de 8MB en 8MB. </br>

La configuració quedarà així: </br> ![configuracio activitat2](img/Screenshot_15.png) </br>

I aquest és el missatge d'error que veiem al .log quan la mida inicial està configurada en només 1MB. </br> ![error configuracio activitat2](img/Screenshot_16.png) </br>

</br>

## Activitat 3. INNODB part II. REALITZA ELS SEGÜENTS APARTATS


1. Partint de l'esquema anterior configura el Percona Server perquè cada taula generi el seu propi tablespace en una carpeta anomenada tspaces (aquesta pot estar situada a on vulgueu). </br>

    1. Indica quins són els canvis de configuració que has realitzat.
    2. Després del canvi què ha passat amb els fitxers que contenien les dades de la BD de Sakila? Fes les captures necesàries per complementar la resposta. </br>
    
InnoDB per defecte guarda les dades i els índexs en un fitxer conjunt (.ibd), tot i que també podem fer que ens generi un fixer per cada taula. </br>

Per activar la creació d'un tablespace per taula, hem de poser el paràmetre <i>innodb_file_per_table=ON</i> a l'arxiu de configuració. </br> ![file per table](img/Screenshot_17.png) </br>

Després d'haver posat el paràmetre anterior, carreguem la BD Sakila i en comprovem els seus fitxers de configuració. Cada taula ha creat el seu propi fitxer tablespace, com podem veure en aquestes dues imatges. </br> ![sakila tspace](img/Screenshot_16.5.png) </br>

</br>

## Activitat 4. INNODB part III. REALITZA ELS SEGÜENTS APARTATS.


1. Crea un tablespace anomenat 'ts1' situat a /discs-mysql/disc1/ i col·loca les taules actor, address i category de la BD Sakila. </br>

Creem el tablespace <i>ts1</i> amb les taules i l'engine indicat i la ruta que pertoca amb la següent sentència a través del shell de MySQL: 

    CREATE TABLESPACE ts1
    ADD DATAFILE "/discs-mysql/disc1/ts1.ibd"
    ENGINE=InnoDB;
    
![creacio ts1](img/2017-11-07_19_28_34.png) </br>

2. Crea un altre tablespace anomenat 'ts2' situat a /discs-mysql/disc2/ i col·loca-hi la resta de taules. </br>

Per fer aquest punt, farem exactament el mateix que en l'anterior, però canviant el directori i nom del tablespace per <i>/discs-mysql/disc2/ts2.ibd</i>. Així quedaria el resultat final amb els dos discs creats: </br> ![creacio disks ts](img/2017-11-14_16_48_13.png) </br> 

3. Comprova que pots realitzar operacions DML a les taules dels dos tablespaces. </br> 

Ara provarem de fer un INSERT a una de les taules, i com podem veure, ha funcionat perfectament. </br> ![consulta dml](img/2017-11-14_17_11_22.png) </br>

4. Quines comandes i configuracions has realitzat per fer els dos apartats anteriors? </br>

Per fer aquests dos apartats, només hem fet servir un parell de comandes. La de creació dels tablespaces i la sentència DML, en aquest cas un insert, per comprovar si realment ens funciona el tablespace. Per posar un exemple ràpid, hem importat BD Sakila, hem fet un <i><b>USE Sakila</b></i> i importar la taula que volem al tablespace <i>ts1</i>. </br> ![consulta dml](img/2017-11-07_19_31_01.png) </br>

</br>

## Activitat 7. Storage Engine CSV


1. Documenta i posa exemple de com utilitzar ENGINE CSV. </br>

Per utilitzar ENGINE CSV és molt senzill, només hem de modificar l'engine de la taula, al igual que hem fet anteriorment a l'activitat 1.4. Creem una taula i la declarem perquè tingui l'engine com a CSV amb la següent sentència: <i><b>ENGINE=CSV</b></i>. </br> ![engine csv](img/2017-11-14_17_58_07.png) </br>

2. Cal documentar els passos que has hagut de realitzar per preparar l'exemple: configuracions, instruccions DML, DDL, etc... </br>

Per fer aquest exemple hem creat la taula com hem documentat en l'anterior exercici. Només hem modificat l'engine i l'hem creat com fariem normalment. </br>

Per seguir amb l'exemple, hem inserit dades a la taula com fariem normalment utilitzant un INSERT. A part de l'usuari "Adrià", també hi hem afegit "Guille" i "Enrique" de la mateixa manera que aquesta captura. </br> ![afegir usuaris csv](img/2017-11-14_17_59_33.png) </br>

Un cop creats els usuaris, comprovem que s'hagin creat correctament els usuaris fent un SELECT a la taula. </br> ![select taula csv](img/2017-11-14_18_01_26.png) </br>

Per últim, podem mirar l'arxiu de dades de la nostra BD per veure com ha guardat la informació en format CSV. La ruta pot veriar segons on ho haguem indicat nosaltres anteriorment.</br> ![fitxer csv](img/2017-11-14_19_26_24.png) </br>

Com a curiositat, l'ENGINE CSV no permet valors nuls a les taules. Com es pot veure en el següent exemple, ens marcarà un error quan intentem crear una taula amb un camp sense afegir-hi el <i><b>NOT NULL</b></i>. </br> ![error camp nul csv](img/2017-11-14_19_38_41.png)</br>

</br>

## Webgrafia


Glossari d'extensions de Percona Server: https://www.percona.com/doc/percona-xtrabackup/LATEST/glossary.html </br>
Diccionari de paràmetres d'InnoDB: https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html </br>
Paràmetre file-per-table d'InnoDB: https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_file_per_table </br>
