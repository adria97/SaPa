# Configuració SGBD

1. Quins són els logs activats per defecte? Com ho has fet per comprovar-ho? </br>
Entrant a mysql i provant la següent sentencia SQL <b><i>SHOW GLOBAL VARIABLES WHERE Variable_name LIKE ‘%log%’ AND (Value = ‘1’ OR Value = ‘ON’)</i></b>, que ens mostrara una llista de tots els logs que estiguin activats. </br>
Activa si no ho estan i indica les configuracions necessàries per activar-los. Indica les rutes dels fitxer de log de Binary, Slow Query i General. Quins paràmetres has modificat? </br>
Per activar els logs primer tenim que editar el fitxer  <b><i> /etc/percona-server </i> </b> i insertar lo següent. </br>
![alt text](img/Screenshot_4.png) </br>
Teni que crear també les carpetes i donar-li permisos. </br>
![alt text](img/Screenshot_3.png) </br>

2. Comprova l'estat de les opcions de log que has utilitzat mitjançant una sessió de mysql client. </br>
Per comprovar si estan activats fem servir la sentencia SQL que em fet servir abans. </br>
![alt text](img/Screenshot_N1.png) </br>
3. Modifica el fitxer de configuració i desactiva els logs de binary, slow query i genral. Nota: Simplament desactiva'ls no borris altres paràmetres com la ruta dels fitxers, etc... </br>
Per fer-ho nomes tenim que editar el fitxer i canviar el valor d'activacio que es <b><i>''</i></b> per el valor de desactivació que es <b><i>'0'</i></b>. </br>
![alt text](img/Screenshot_N2.png) </br>

5. Carrega la BD Sakila localitzada a la web. </br>
![alt text](img/Screenshot_1.png) </br>
