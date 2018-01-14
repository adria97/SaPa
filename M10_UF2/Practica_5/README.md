# Pràctica 5

## Clustering amb Percona XtraDB Cluster


Per fer aquesta pràctca, utilitzarem 4 Clusters.

1. Slave Cluster 1: NodeID = 1; HostName: Percona1; IP: 192.168.26.71
2. Slave Cluster 2: NodeID = 2; HostName: Percona2; IP: 192.168.26.72
3. Slave Cluster 3: NodeID = 3; HostName: Percona3; IP: 192.168.26.73
4. Slave Cluster 4: NodeID = 4; HostName: Percona4; IP: 192.168.26.74

</br>

Abans de començar, haurem de desactivar el firewall (en el nostre cas per ser un entorn de prova) o permetre passar els paquets pels ports que necessitem, que són els següents: <b><i>3306, 4444, 4567, 4568</i></b>. Si volem permetre passar els paquets pels ports en concret requerits, utilitzarem la comanda <b>ufw allow <i>[número de port]</i></b>. Per desctivar el firewall, utilitzarem al comanda <b>ufw disable</b>. </br>

![*_XtraDB](img/0_XtraDB.png) </br>

Un cop deshabilitat el firewall, procedim a instal·lar l'XtraDB. </br>
Per poder-lo instal·lar, primer hem de descarregar i instal·lar els repositoris, o bé actualitzar-los en cas que els tinguem descarregats. </br>

També hem de configurar les interfícies, que en el cas de CentOS, és un pèl diferent a altres sistemes Linux. Podeu trobar el tutorial aquí. http://ask.xmodulo.com/configure-static-ip-address-centos7.html. La IP del node base és la 192.168.26.71, i aquí la tenim configurada. </br>

![*_XtraDB](img/8_XtraDB.png)

</hr>

![*_XtraDB](img/1_XtraDB.png) </br>

Un cop descarregat, desempaquetem i l'istal·lem/actualitzem la llista dels repositoris. </br>

![*_XtraDB](img/2_XtraDB.png) </br>

![*_XtraDB](img/3_XtraDB.png) </br>

Podem confirmar que s'han atualitzat els repositoris buscant el paquet que volem instal·lar. </br>

![*_XtraDB](img/4_XtraDB.png) </br>

Al confirmar que si tenim els repositoris dels paquets, procedim a instal·lar el nostre paquet XtraDB. </br>

![*_XtraDB](img/5_XtraDB.png) </br>

I per acavar ens demanarà una contrasenya. </br>

![*_XtraDB](img/6_XtraDB.png) </br>
