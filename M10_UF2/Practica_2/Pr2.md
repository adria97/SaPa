# Configuració SGBD

1. Quins són els logs activats per defecte? Com ho has fet per comprovar-ho?
Entrant a mysql i provant la següent sentencia SQL <b><i>      SHOW GLOBAL VARIABLES WHERE Variable_name LIKE ‘%log%’ AND (Value = ‘1’ OR Value = ‘ON’)</i></b>. </br>
Activa si no ho estan i indica les configuracions necessàries per activar-los. Indica les rutes dels fitxer de log de Binary, Slow Query i General. Quins paràmetres has modificat? </br>
![alt text](img/P1/Screenshot_3.png) </br>
