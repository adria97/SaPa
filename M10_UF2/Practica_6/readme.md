# Optimització de consultes


## Part 1

Escriu les sentències SQL per tal d’obtenir els que se’ns demana. A més a més si creus que la sentència es pot millorar amb la incorporació d’un índex i/o modificació de l’esquema (sense alterar-ne el comportament),etc... Afegeix la sentència DDL i l’output de EXPLAIN mostrant la millora (EXPLAIN sense índex i EXPLAIN amb índex). Si creus que la consulta no es pot millorar mitjançant índexs justifica el perquè. Nota: No milloreu les sentències amb índexs complerts. </br></br>

1. Obtenir el nom i l’adreça dels hotels de 4 estrelles. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

Explains sense índex: </br> ![*_Exp1](IMG/Exp_1.png) </br>

Explains amb índex: </br> ![*_EI1](IMG/EI_01.png) </br></br>


2. Obtenir el nom dels clients (Nom i cognom) que el seu cognom comenci per vocal (sense tenir en compte els accents). </br>

        SELECT nom, cognom1
          FROM clients
          WHERE cognom1 RLIKE "^[aeiou]";

Explains sense índex: </br> ![*_Exp2](IMG/Exp_2.png) </br>

Explains amb índex: </br> ![*_EI2](IMG/EI_02.png) </br></br>


3. Quina és la reserva_id que té més nits. Indica també la quantitat de nits. </br>

        SELECT r1.reserva_id, TIMESTAMPDIFF(DAY,r1.data_inici,r1.data_fi) AS Nits
          FROM reserves r1
          WHERE TIMESTAMPDIFF(DAY,r1.data_inici,r1.data_fi) = (SELECT MAX(TIMESTAMPDIFF(DAY,r2.data_inici,r2.data_fi))
                                                            FROM reserves r2);

Explains sense índex: </br> ![*_Exp3](IMG/Exp_3.png) </br>

Explains amb índex: </br> ![*_EI3](IMG/EI_03.png) </br></br>

4. Quantes reserves va rebre l’hotel ‘Catalonia Ramblas’ de Barcelona durant tot  l’any 2015 (una reserva pertany al 2015 si alguna nit d’aquesta reserva era del 2015). </br>

        SELECT COUNT(*) AS Reserves_2015
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2015 AND h.nom = 'Catalonia Ramblas' ;

Explains sense índex: </br> ![*_Exp4.2](IMG/Exp_4.2.png) </br>

Explains amb índex: </br> ![*_EI4](IMG/EI_04.png) </br></br>

5. Obtenir el nom i cognoms dels clients que varen néixer el mes de Març. </br>

        SELECT nom, cognom1, data_naix
          FROM clients
          WHERE MONTH(data_naix) = 3;

Explains sense índex: </br> ![*_Exp5](IMG/Exp_5.png) </br>

Explains amb índex: </br> ![*_EI5](IMG/EI_05.png) </br></br>

6. Quantitat d’hotels de 4 estrelles de la població de Barcelona. </br>

        SELECT COUNT(*)
          FROM hotels h
          WHERE h.categoria = 4 AND h.poblacio_id = (SELECT po.poblacio_id
                                                  FROM poblacions po
                                                  WHERE po.nom = "Barcelona");

Explains sense índex: </br> ![*_Exp6](IMG/Exp_6.png) </br>

Explains amb índex: </br> ![*_EI6](IMG/EI_06.png) </br></br>

7. De l’any 2015 volem obtenir els seu histograma de reserves. És a dir volem saber el número de reserves de cadascun dels mesos. Una reserva pertany a un mes si la alguna nit d’aquella reserva cau a dins de l’any 2015. </br>

        SELECT MONTH(data_inici), COUNT(*) AS Reserves_2015
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2015
          GROUP BY MONTH(data_inici);

Explains sense índex: </br> ![*_Exp7](IMG/Exp_7.png) </br>

Explains amb índex: </br> ![*_EI7](IMG/EI_07.png) </br></br>

8. El nom dels hotels que tenen com a mínim una habitació lliure durant les dates ‘2015-05-01’ i ‘2015-05-17’. </br>

        SELECT h.nom, (h.habitacions-COUNT(hab.hotel_id)) AS Habitacions_lliures
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE (r.data_inici <= '2015-05-01' AND r.data_fi >= '2015-05-17') 
          GROUP BY h.hotel_id;

Explains sense índex: </br> ![*_Exp8](IMG/Exp_8.png) </br>

Explains amb índex: </br> ![*_EI8](IMG/EI_08.png) </br></br>

9. Obtenir la quantitat de reserves que s’inicien en cadascun dels dies de la setmana. Tenint en compte només l’any 2016. </br>

        SELECT DAYOFWEEK(data_inici), COUNT(*) AS Reserves_2016
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2016
          GROUP BY DAYOFWEEK(data_inici);

Explains sense índex: </br> ![*_Exp9](IMG/Exp_9.png) </br>

Explains amb índex: </br> ![*_EI9](IMG/EI_09.png) </br></br>

10. Durant 2014 qui va realitzar més reserves? Els homes o les dones? Mostra el sexe i el número de reserves. </br>

        SELECT IF(SUM(c.sexe='F')>SUM(c.sexe='M'),'Dones','Homes') AS MesReserves, COUNT(*)
          FROM reserves r
        INNER JOIN clients c ON c.client_id = r.client_id
        WHERE YEAR(r.data_inici) = 2014;

Explains sense índex: </br> ![*_Exp10](IMG/Exp_10.png) </br>

Explains amb índex: </br> ![*_EI10](IMG/EI_10.png) </br></br>

11. Quina és la mitjana de dies de reserva per l’hotel «HTOP Royal Star» de Blanes durant l’any 2016? (Una reserva pertany el 2016 si alguna nit cau en aquest any). </br>

        SELECT nom, categoria, adreca,(SELECT max(num_hab)
                                FROM (SELECT count(hab_id) AS num_hab,hotel_id 
                                        FROM habitacions GROUP BY hotel_id)max_hab2)AS num_hab
          FROM hotels
        WHERE hotel_id = (SELECT hotel_id 
          FROM habitacions 
        GROUP BY hotel_id
        HAVING count(hab_id)=(SELECT max(num_hab)
                                        FROM (SELECT count(hab_id) AS num_hab,hotel_id 
                                                FROM habitacions GROUP BY hotel_id)max_hab));

Explains sense índex: </br> ![*_Exp11](IMG/Exp_11.png) </br>

Explains amb índex: </br> ![*_EI11](IMG/EI_11.png) </br></br>

12. El nom, categoria, adreça i número d’habitacions de l’hotel amb més habitacions de la BD. </br>

        SELECT ho.nom,ho.categoria,ho.adreca,COUNT(ha.hab_id) AS num_habitacions
          FROM hotels ho
        INNER JOIN habitacions ha ON ho.hotel_id = ha.hotel_id
        GROUP BY ho.hotel_id
        ORDER BY num_habitacions DESC LIMIT 1;

Explains sense índex: </br> ![*_Exp12](IMG/Exp_12.png) </br>

Explains amb índex: </br> ![*_EI12](IMG/EI_12.png) </br></br>

13. Rànquing de 5 països amb més reserves durant l’any 2016. Per cada país mostrar el nom del país i el número de reserves. </br>

        SELECT COUNT(*) AS numReserves, p2.nom
                FROM reserves r2
                INNER JOIN clients c2 ON c2.client_id = r2.client_id
                INNER JOIN paisos p2 ON p2.pais_id = c2.pais_origen_id
                WHERE  YEAR(r2.data_inici) = 2016
        GROUP BY p2.pais_id
        ORDER BY numReserves DESC
        LIMIT 5;

Explains sense índex: </br> ![*_Exp13](IMG/Exp_13.png) </br>

Explains amb índex: </br> ![*_EI13](IMG/EI_13.png) </br></br>

14. Codi client, Nom, Cognom, del client que ha realitzat més reserves de tota la BD. </br>

        SELECT c.client_id,c.nom,c.cognom1,COUNT(r.reserva_id) AS num_reserves
          FROM clients c
        INNER JOIN reserves r ON r.client_id = c.client_id
        GROUP BY c.client_id 
        ORDER BY num_reserves DESC LIMIT 1;

Explains sense índex: </br> ![*_Exp14](IMG/Exp_14.png) </br>

Explains amb índex: </br> ![*_EI14](IMG/EI_14.png) </br></br>

15. Codi client, Nom, Cognom, del client que ha realitzat més reserves durant el mes d’agost de l’any 2016. Les reserves a comptabilitzar són totes aquelles que en algun dia del seu període cau en el mes d’agost. </br>

        SELECT  r.client_id AS Client, c.nom, c.cognom1, COUNT(*) AS NumReserves
          FROM reserves r
        INNER JOIN clients c ON c.client_id = r.client_id
        WHERE (r.data_inici <= '2016-08-31' 
              AND r.data_fi >= '2016-08-1')
        GROUP BY r.client_id
        HAVING COUNT(*) = (SELECT MAX(contReserves.NumReserves)
                             FROM (SELECT COUNT(*) AS NumReserves, r.client_id AS Client, c.nom
                                     FROM reserves r
                                   INNER JOIN clients c ON c.client_id = r.client_id
                                   WHERE (r.data_inici <= '2016-08-31' 
                                         AND r.data_fi >= '2016-08-1')
                                   GROUP BY r.client_id) contReserves);

Explains sense índex: </br> ![*_Exp15](IMG/Exp_15.png) </br>

Explains amb índex: </br> ![*_EI15](IMG/EI_15.png) </br></br>

16. Quin és el país que en tenim menys clients? </br>

        SELECT p.nom, COUNT(*) AS num_clients
          FROM clients c
          INNER JOIN paisos p ON c.pais_origen_id = p.pais_id
        GROUP BY p.pais_id
        ORDER BY num_clients ASC LIMIT 1;

Explains sense índex: </br> ![*_Exp16](IMG/Exp_16.png) </br>

Explains amb índex: </br> ![*_EI16](IMG/EI_16.png) </br></br>

17. Quina és la mitjana de nits dels clients provinents d’‘HOLANDA’ per l’any 2016? </br>

        SELECT AVG(anys.numReserves) AS MitjanaReserves
          FROM (SELECT COUNT(*) AS numReserves
                        FROM reserves r2
                            INNER JOIN clients c2 ON c2.client_id = r2.client_id
                            INNER JOIN paisos p2 ON p2.pais_id = c2.pais_origen_id
                            WHERE  p2.nom = 'HOLANDA'
                            GROUP BY YEAR(r2.data_inici))anys;

Explains sense índex: </br> ![*_Exp17](IMG/Exp_17.png) </br>

Explains amb índex: </br> ![*_EI17](IMG/EI_17.png) </br></br>

18. Digues el nom i cognoms dels clients que el seu cognom sigui ‘Bahi’. </br>

        SELECT nom,cognom1 AS cognoms
          FROM clients
        WHERE cognom1="Bahi";

Explains sense índex: </br> ![*_Exp18](IMG/Exp_18.png) </br>

Explains amb índex: </br> ![*_EI18](IMG/EI_18.png) </br></br>

19. Quins clients (nom, cognoms) segueixen el patró de que el seu cognom comenci per la lletra ‘p’  i seguida d’una vocal. </br>

        SELECT nom, cognom1
            FROM clients
          WHERE LOWER(cognom1) REGEXP '^p[a,e,i,o,u]';

Explains sense índex: </br> ![*_Exp19](IMG/Exp_19.png) </br>

Explains amb índex: </br> ![*_EI19](IMG/EI_19.png) </br></br>

20. Quin és l’hotel de 4 estrelles amb més reserves durant tot el 2015 ( una reserva pertany el 2015 si alguna de les nits hi pertany). </br>

        SELECT ho2.hotel_id, ho2.nom, ho2.categoria
          FROM hotels ho2
        INNER JOIN habitacions h2 on ho2.hotel_id = h2.hotel_id
        WHERE h2.hab_id = (SELECT hab_id
                            FROM (SELECT h1.hab_id
                                    FROM reserves r1
                                  INNER JOIN habitacions h1 ON r1.hab_id = h1.hab_id
                                  INNER JOIN hotels ho1 ON h1.hotel_id = ho1.hotel_id
                                  WHERE (YEAR(r1.data_inici)= 2015 OR YEAR(r1.data_fi)= 2015) AND ho1.categoria=4)hotel4
                          GROUP BY hab_id
                          HAVING COUNT(hab_id) = (SELECT MAX(num_res)
                                                    FROM (SELECT hab_id, count(hab_id) AS num_res
                                                            FROM (SELECT h1.hab_id
                                                                    FROM reserves r1
                                                                  INNER JOIN habitacions h1 ON r1.hab_id = h1.hab_id
                                                                  INNER JOIN hotels ho1 ON h1.hotel_id = ho1.hotel_id
                                                                  WHERE (YEAR(r1.data_inici)= 2015 OR YEAR(r1.data_fi)= 2015) AND ho1.categoria=4)hotel4
                                                  GROUP BY hab_id)reserves));

Explains sense índex: </br> ![*_Exp20](IMG/Exp_20.png) </br>

Explains amb índex: </br> ![*_EI20](IMG/EI_20.png) </br></br>

21. Quin és l’hotel amb més reserves (tota la BD). </br>

        SELECT  h.hotel_id, ho.nom, ho.categoria
          FROM habitacions h 
        INNER JOIN hotels ho ON ho.hotel_id = h.hotel_id
          WHERE hab_id = (SELECT hab_id
            FROM reserves
          GROUP BY hab_id
          HAVING COUNT(hab_id)=(SELECT MAX(num_hab)
              FROM(SELECT hab_id, COUNT(hab_id) AS num_hab
                    FROM reserves
                   GROUP BY hab_id)max_reserves));

Explains sense índex: </br> ![*_Exp21](IMG/Exp_21.png) </br>

Explains amb índex: </br> ![*_EI21](IMG/EI_21.png) </br></br>

22. Quin és el país amb més reserves? (tots els anys) O sigui, quin és el país d’on han vingut més turistes. </br>

        SELECT p.nom,COUNT(*) AS num_reserves
          FROM paisos p
          INNER JOIN clients c ON c.pais_origen_id = p.pais_id
          INNER JOIN reserves r ON r.client_id = c.client_id
        GROUP BY p.pais_id
        ORDER BY num_reserves DESC LIMIT 1;

Explains sense índex: </br> ![*_Exp22](IMG/Exp_22.png) </br>

Explains amb índex: </br> ![*_EI22](IMG/EI_22.png) </br></br>

### Explicació dels índex per taules

</br>

<b>Hotels:</b> En aquest cas, hem decidit que estaria bé classificar els hotels per dos camps generals: població i categoria. Considerem que el primer és un que ens permet acotar/indexar molt els camps, i la segona és també molt eficaç en aquest aspecte. El primer, al ser més genèric, és millor. </br></br> ![*_Index Hotels](IMG/Ind_01.png) </br>

<b>Clients:</b> En aquesta BD en concret, el nom és el camp que més es busca i repeteix, per tant, hem decidit que aquest sigui l'índex juntament amb el cognom i la data de naixement. </br></br> ![*_Index Clients](IMG/Ind_02.png) </br>

<b>Reserves:</b> Aquí ens hem trobat que la majoria de consultes es fan segons el rang d'inici/fi de la reserva per tant, que aquests camps siguin l'índex d'aquesta taula, seria el cas més ideal. </br></br> ![*_Index Clients](IMG/Ind_05.png) </br>

<b>Població:</b> No hem vist necessari crear-hi cap índex, ja que aquesta taula de té molts pocs registres. Si en tinguéssim més, com a molt podríem utilitzar un índex per la clau primària. També tindria sentit posar-hi un índex en cas que hi hagués moltes poblacions i de varis països, ja que podríem indexar pais_id i poblacio_id. </br>

<b>Països:</b> Passa el mateix que a la taula poblacions, no és necessari crear-hi cap índex, però és encara més clar. Aproximadament hi ha uns 200 països al món, i difícilment creixerà la llista. Una taula amb 200 registres i únicament dos camps, no té gairebé impacte en quant al rendment. </br>

<b>Habitacions:</b> No és necessari, ja que normalment quan busquem habitacions, les busquem a través del camp hotel_id i normalent des d'un INNER JOIN, per tant, ja ve filtrat. </br></br></br>

## Part 2. Query Cache

Escull 5 sentències SQL de la Part 1 (intenta que els temps d’execució siguin significatius).

1. Quins temps d'execució t'han sortit per cada consulta?

- Consulta 9 => (0.3-0.4)

![*_Consulta9_QCOFF](IMG2/Query_OFF/QOf_04.png)

- Consulta 13 => (0.4)

![*_Consulta9_QCOFF](IMG2/Query_OFF/QOf_05.png)

- Consulta 15 => (0.5)

![*_Consulta9_QCOFF](IMG2/Query_OFF/QOf_03.png)

- Consulta 20 => (0.8-0.9)

![*_Consulta9_QCOFF](IMG2/Query_OFF/QOf_01.png)

- Consulta 21 => (0.4)

![*_Consulta9_QCOFF](IMG2/Query_OFF/QOf_02.png)

2. Activa la Query Cache (ON). Reexecuta les consultes anteriors 2 vegades seguides. Ha millorat el temps d'execució? Sí, ha millorat en tots els casos.

- Consulta 9 => (0.0)

![*_Consulta9_QCOFF](IMG2/Query_ON/QOn_04.png)

- Consulta 13 => (0.0)

![*_Consulta9_QCOFF](IMG2/Query_ON/QOn_05.png)

- Consulta 15 => (0.0)

![*_Consulta9_QCOFF](IMG2/Query_ON/QOn_03.png)

- Consulta 20 => (0.0)

![*_Consulta9_QCOFF](IMG2/Query_ON/QOn_01.png)

- Consulta 21 => (0.0)

![*_Consulta9_QCOFF](IMG2/Query_ON/QOn_02.png)
