# Pràctica 5

## Clustering amb Percona XtraDB Cluster

Abans de començar, haurem de desactivar el firewall (en el nostre cas per ser un entorn de prova) o permetre passar els paquets pels ports que necessitem, que són els següents: <b><i>3306, 4444, 4567, 4568</i></b>.
</br>
</br>
Utilitzarem 3 Clusters i 1 Master.

1. Master IP: 192.168.26.10
2. Slave Cluster 1: NodeID = 1; HostName: Percona1; IP: 192.168.26.71
3. Slave Cluster 2: NodeID = 2; HostName: Percona2; IP: 192.168.26.72
4. Slave Cluster 3: NodeID = 3; HostName: Percona3; IP: 192.168.26.73
