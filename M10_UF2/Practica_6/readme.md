# Optimització de consultes


## Part 1

Escriu les sentències SQL per tal d’obtenir els que se’ns demana. A més a més si creus que la sentència es pot millorar amb la incorporació d’un índex i/o modificació de l’esquema (sense alterar-ne el comportament),etc... Afegeix la sentència DDL i l’output de EXPLAIN mostrant la millora (EXPLAIN sense índex i EXPLAIN amb índex). Si creus que la consulta no es pot millorar mitjançant índexs justifica el perquè. Nota: No milloreu les sentències amb índexs complerts. </br></br>

1. Obtenir el nom i l’adreça dels hotels de 4 estrelles. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

2. Obtenir el nom dels clients (Nom i cognom) que el seu cognom comenci per vocal (sense tenir en compte els accents). </br>

        SELECT nom, cognom1
          FROM clients
          WHERE cognom1 RLIKE "^[aeiou]";

3. Quina és la reserva_id que té més nits. Indica també la quantitat de nits. </br>

        SELECT r1.reserva_id, TIMESTAMPDIFF(DAY,r1.data_inici,r1.data_fi) AS Nits
          FROM reserves r1
          WHERE TIMESTAMPDIFF(DAY,r1.data_inici,r1.data_fi) = (SELECT MAX(TIMESTAMPDIFF(DAY,r2.data_inici,r2.data_fi))
                                                            FROM reserves r2);

4. Quantes reserves va rebre l’hotel ‘Catalonia Ramblas’ de Barcelona durant tot  l’any 2015 (una reserva pertany al 2015 si alguna nit d’aquesta reserva era del 2015). </br>

        SELECT COUNT(*) AS Reserves_2015
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2015 AND h.nom = 'Catalonia Ramblas' ;

5. Obtenir el nom i cognoms dels clients que varen néixer el mes de Març. </br>

        SELECT nom, cognom1, data_naix
          FROM clients
          WHERE MONTH(data_naix) = 3;

6. Quantitat d’hotels de 4 estrelles de la població de Barcelona. </br>

        SELECT COUNT(*)
          FROM hotels h
          WHERE h.categoria = 4 AND h.poblacio_id = (SELECT po.poblacio_id
                                                  FROM poblacions po
                                                  WHERE po.nom = "Barcelona");

7. De l’any 2015 volem obtenir els seu histograma de reserves. És a dir volem saber el número de reserves de cadascun dels mesos. Una reserva pertany a un mes si la alguna nit d’aquella reserva cau a dins de l’any 2015. </br>

        SELECT MONTH(data_inici), COUNT(*) AS Reserves_2015
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2015
          GROUP BY MONTH(data_inici);

8. El nom dels hotels que tenen com a mínim una habitació lliure durant les dates ‘2015-05-01’ i ‘2015-05-17’. </br>

        SELECT h.nom, (h.habitacions-COUNT(hab.hotel_id)) AS Habitacions_lliures
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE (r.data_inici <= '2015-05-01' AND r.data_fi >= '2015-05-17') 
          GROUP BY h.hotel_id;

9. Obtenir la quantitat de reserves que s’inicien en cadascun dels dies de la setmana. Tenint en compte només l’any 2016. </br>

        SELECT DAYOFWEEK(data_inici), COUNT(*) AS Reserves_2016
          FROM reserves r
          INNER JOIN habitacions hab ON hab.hab_id = r.hab_id
          INNER JOIN hotels h ON h.hotel_id = hab.hotel_id
          WHERE YEAR(data_inici) = 2016
          GROUP BY DAYOFWEEK(data_inici);

10. Durant 2014 qui va realitzar més reserves? Els homes o les dones? Mostra el sexe i el número de reserves. </br>

        SELECT IF(SUM(c.sexe='F')>SUM(c.sexe='M'),'Dones','Homes') AS MesReserves, COUNT(*)
          FROM reserves r
        INNER JOIN clients c ON c.client_id = r.client_id
        WHERE YEAR(r.data_inici) = 2014;

11. Quina és la mitjana de dies de reserva per l’hotel «HTOP Royal Star» de Blanes durant l’any 2016? (Una reserva pertany el 2016 si alguna nit cau en aquest any). </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

12. El nom, categoria, adreça i número d’habitacions de l’hotel amb més habitacions de la BD. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

13. Rànquing de 5 països amb més reserves durant l’any 2016. Per cada país mostrar el nom del país i el número de reserves. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

14. Codi client, Nom, Cognom, del client que ha realitzat més reserves de tota la BD. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

15. Codi client, Nom, Cognom, del client que ha realitzat més reserves durant el mes d’agost de l’any 2016. Les reserves a comptabilitzar són totes aquelles que en algun dia del seu període cau en el mes d’agost. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

16. Quin és el país que en tenim menys clients? </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

17. Quina és la mitjana de nits dels clients provinents d’‘HOLANDA’ per l’any 2016? </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

18. Digues el nom i cognoms dels clients que el seu cognom sigui ‘Bahi’. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

19. Quins clients (nom, cognoms) segueixen el patró de que el seu cognom comenci per la lletra ‘p’  i seguida d’una vocal. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

20. Quin és l’hotel de 4 estrelles amb més reserves durant tot el 2015 ( una reserva pertany el 2015 si alguna de les nits hi pertany). </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

21. Quin és l’hotel amb més reserves (tota la BD). </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

22. Quin és el país amb més reserves? (tots els anys) O sigui, quin és el país d’on han vingut més turistes. </br>

        SELECT nom, adreca
          FROM hotels
          WHERE categoria = 4;

