# Pràctica 3

## Activitat 1. REALITZA I/O RESPON ELS SEGÜENTS APARTATS

1. Indica quins són els motors d’emmagatzematge que pots utilitzar (quins estan actius)? Mostra al comanda utilitzada i el resultat d’aquesta.</br>

Per veure els motors d'emmagatzematge que podem utilitzar, utilitzarem la comanda <i><b>SHOW ENGINES;</b></i>. ![show engines](img/Screenshot_1.png) </br>

2. Com puc saber quin és el motor d’emmagatzematge per defecte? Mostra com canviar aquest paràmetre de tal manera que les noves taules que creem a la BD per defecte utilitzin el motor MyISAM?

Amb la mateixa comanda anterior, podem veure els motors d'emmagatzematge actius (<i>YES</i>), els que no ho estàn (<i>NO</i>) i el motor d'emmagatzematge per defecte (<i>DEFAULT</i>) a la columna "SUPPORT". Per canviar el motor d'emmagatzematge per defecte a MyISAM, hem d'editar el fitxer de configuració <i><b>my.cnf</b></i> i posar el paràmetre <i>default-storage-engine=[Nom]</i>. ![canviar engine per defecte](img/Screenshot_2.png)
