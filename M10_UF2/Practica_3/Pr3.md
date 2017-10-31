# Pràctica 3

## Activitat 1. REALITZA I/O RESPON ELS SEGÜENTS APARTATS

1. Indica quins són els motors d’emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d’aquesta.</br>

Per veure els motors d'emmagatzematge que podem utilitzar, utilitzarem la comanda <i><b>SHOW ENGINES;</b></i>.
![show engines](img/Screenshot_1.png) </br></br>

2. Com puc saber quin és el motor d’emmagatzematge per defecte? Mostra com canviar aquest paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor MyISAM?

Amb la mateixa comanda anterior,  a la columna "SUPPORT", podem veure els motors d'emmagatzematge actius (<i>YES</i>), els que no ho estàn (<i>NO</i>) i el motor d'emmagatzematge per defecte (<i>DEFAULT</i>). Per canviar el motor d'emmagatzematge per defecte a MyISAM, hem d'editar el fitxer de configuració <i><b>my.cnf</b></i> i posar el paràmetre <i>default-storage-engine=[Nom]</i>. ![canviar engine per defecte](img/Screenshot_2.png) </br>

Per veure si el canvi ha estat correcte, tornem a utilitzar la comanda <i><b>SHOW ENGINES;</b></i>.
![canvi engine correcte](img/Screenshot_3.png) </br></br>

3. Explica els passos per instal·lar i activar l'ENGINE MyRocks. MyRocks és un motor d'emmagatzematge per MySQL basat en RocksDB (SGBD incrustat de tipus clau-valor).</br>

Per instal·lar el motor d'emmagatzematge MyRocks, hem d'utilitzar la comanda <i><b>yum install Percona-Server-rocksdb-57.x86_64</b></i> (el número "57" pot variar segons la versió MySQL que tenim instal·lada)
