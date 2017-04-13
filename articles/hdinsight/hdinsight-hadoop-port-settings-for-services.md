<properties
pageTitle="HDInsight által használt portok |} Azure"
description="Futó HDInsight Hadoop-szolgáltatás által használt portok listája."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Portok és HDInsight által használt URL-címe

A dokumentum tartalmaz a által használt portok Linux-alapú HDInsight fürt futó Hadoop-szolgáltatások listája. Információ a fürthöz SSH eléréséhez használt portokra is tartalmaz.

## <a name="public-ports-vs-non-public-ports"></a>Nyilvános portok nem nyilvános portokat és összehasonlítása

HDInsight fürt Linux-alapú csak a nyilvánosan az interneten; három portok közzététele 22, 23 és 443-as. Ezek a SSH és a biztonságos HTTPS protokollon keresztül elérhetővé tett szolgáltatások használatával fürt keresztül biztonságosan hozzáférhet szolgálnak.

Belső HDInsight által több Azure virtuális gépeken futó (a fürt csomópontjainak) alkalmazása Azure virtuális hálózaton futó. Az a virtuális hálózaton belül is elérheti portok nem elérhetővé az interneten keresztül. Például ha egy központi csomópontok SSH használatával csatlakozni, a központi csomópont közvetlenül érheti el a fürt csomópontok futó services.

> [AZURE.IMPORTANT] Amikor egy HDInsight fürthöz hoz létre, ha nem adja meg az Azure virtuális hálózati konfigurációja lehetőségként, létrejön egy; más számítógépek (például más Azure virtuális gépeken futó vagy az ügyfélgép fejlesztési) azonban nem tud csatlakozni a automatikusan létrejön egy virtuális hálózati. 

A virtuális hálózathoz további gépek csatlakozáshoz először létrehozása a virtuális hálózat, és adja meg azt a HDInsight fürt létrehozásakor. További tudnivalókért lásd: [az Azure virtuális hálózaton keresztül kiterjesztése HDInsight funkciók](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Nyilvános portok

Egy HDInsight fürthöz csomópontjait Azure virtuális hálózatban találhatók, és nem tudnak közvetlenül hozzáférni az internetről. Nyilvános az átjárók az alábbi portokat, amelyek különböző HDInsight fürt diagramtípusokat közös internet hozzáférést biztosít.

| Szolgáltatás | Port | Protocol (protokoll) | Leírás |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22-es hibakód | SSH | Kattintson az elsődleges headnode sshd ügyfelek csatlakozik. Lásd: [a Linux-alapú HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22-es hibakód | SSH | A szegély csomópont (csak a HDInsight prémium verzióban) sshd ügyfelek csatlakozik. Lásd: a [R Server HDInsight használatának első lépései](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Kattintson a másodlagos headnode sshd ügyfelek csatlakozik. Lásd: [a Linux-alapú HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443-as | HTTPS | Ambari webes felület. Lásd: [kezelése HDInsight Ambari webes felületének használata](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443-as | HTTPS | Ambari REST API-t. Lásd: [kezelése HDInsight Ambari REST API -t használata](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443-as | HTTPS | HCatalog REST API-t. Lásd: [Curl a struktúra](hdinsight-hadoop-use-pig-curl.md), [Curl malac használni](hdinsight-hadoop-use-pig-curl.md), [a Curl MapReduce](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443-as | ODBC | A struktúra ODBC használatával csatlakozik. Lásd: az [Excel csatlakozni a Microsoft ODBC-illesztőprogram hdinsight szolgáltatáshoz](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443-as | JDBC | Struktúra JDBC használatával csatlakozik. [A HDInsight-illesztőprogram struktúra JDBC használatával struktúra](hdinsight-connect-hive-jdbc-driver.md) csatlakoztatása |

A következő fióktípus esetén elérhetők adott fürt:

| Szolgáltatás | Port | Protocol (protokoll) |Fürt típusa | Leírás |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443-as | HTTPS | HBase | HBase REST API-t. Lásd: a [HBase használatának első lépései](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livius | 443-as | HTTPS |  A külső | A külső REST API-t. Lásd: [a külső elküldése feladatok Livius távolról használatával](hdinsight-apache-spark-livy-rest-interface.md) |
| Vihar | 443-as | HTTPS | Vihar | Vihar webes felület. Lásd: [Deploy és kezelheti a HDInsight vihar topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Hitelesítés

Az interneten nyilvánosan elérhető szolgáltatások hitelesíteni kell:

| Port | Hitelesítő adatok |
| ---- | ----------- |
| 22-es vagy 23 | Csoport létrehozása során megadott SSH felhasználói hitelesítő adatok |
| 443-as | A bejelentkezési név (alapértelmezett: rendszergazda,) és a csoport létrehozása során beállított jelszó |

## <a name="non-public-ports"></a>Nem nyilvános portok

> [AZURE.NOTE] Egyes szolgáltatások csak az adott fürt típusairól érhetők el. HBase például HBase fürt típusairól csak érhető el.

### <a name="hdfs-ports"></a>Fájlrendszerhez portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode webes felhasználói felület | Címsor csomópontok | 30070 | HTTPS | A webhely felhasználói felület aktuális állapot megtekintése |
| NameNode metaadatok szolgáltatás | központi csomópontok | 8020 | IPC | Fájl rendszer metaadatai 
| DataNode | Az összes dolgozó csomópontok | 30075 | HTTPS | Webes felület állapotának megtekintése és a naplókat, stb. |
| DataNode | Az összes dolgozó csomópontok | 30010 | &nbsp; | Adatátvitel |
| DataNode | Az összes dolgozó csomópontok | 30020 | IPC | Metaadat-műveletek |
| Másodlagos NameNode | Címsor csomópontok | 50090 | HTTP | Ellenőrzés NameNode metaadatok |

### <a name="yarn-ports"></a>FONAL portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| Erőforrás-kezelő webhely felhasználói felület | Címsor csomópontok | 8088 | HTTP | Webes az erőforrás-kezelő felület |
| Erőforrás-kezelő webhely felhasználói felület | Címsor csomópontok | 8090 | HTTPS | Webes az erőforrás-kezelő felület |
| Erőforrás-kezelő felületen | központi csomópontok | 8141 | IPC | Az alkalmazás beküldött elemek (struktúra, struktúra kiszolgáló malac, stb.) |
| Erőforrás-kezelő ütemező | központi csomópontok | 8030 | HTTP | Felügyeleti felület |
| Erőforrás-kezelő alkalmazás felület | központi csomópontok | 8050 | HTTP |Az alkalmazások kezelő felület címe |
| NodeManager | Az összes dolgozó csomópontok | 30050 | &nbsp; | A cím: a tároló kezelője |
| NodeManager webes felhasználói felület | Az összes dolgozó csomópontok | 30060 | HTTP | Erőforrás-kezelő felület |
| Ütemterv cím | Címsor csomópontok | 10200 | TÁVOLI ELJÁRÁSHÍVÁS | Az ütemterv RPC szolgáltatás. |
| Ütemterv webes felhasználói felület | Címsor csomópontok | 8181 | HTTP | Az ütemterv szolgáltatás webes felhasználói felület |

### <a name="hive-ports"></a>Portok struktúra

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Címsor csomópontok | 10001 | Thrift | A szolgáltatás programozás útján csatlakozhat struktúra (Thrift/JDBC) |
| HiveServer | Címsor csomópontok | 10000 | Thrift | A szolgáltatás programozás útján csatlakozhat struktúra (Thrift/JDBC) |
| Metastore struktúra | Címsor csomópontok | 9083 | Thrift | A szolgáltatás programozás útján csatlakozhat struktúra metaadat-alapú (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat kiszolgáló | Címsor csomópontok | 30111 | HTTP | Webes API HCatalog és más Hadoop-szolgáltatások felett |

### <a name="mapreduce-ports"></a>MapReduce portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Címsor csomópontok | 19888 | HTTP | MapReduce JobHistory webes felhasználói felület |
| JobHistory | Címsor csomópontok | 10020 | &nbsp; | MapReduce JobHistory kiszolgáló |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Köztes térkép átvitelek szűkítő igénylésével kimenet |

### <a name="oozie"></a>Oozie

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie kiszolgáló | Címsor csomópontok | 11000 | HTTP | Oozie szolgáltatás URL-címe |
| Oozie kiszolgáló | Címsor csomópontok | 11001 | HTTP | A port Oozie rendszergazdáknak |

### <a name="ambari-metrics"></a>Ambari mérőszámok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| Ütemterv (alkalmazás Előzmények) | Címsor csomópontok | 6188 | HTTP | Az ütemterv szolgáltatás webes felhasználói felület |
| Ütemterv (alkalmazás Előzmények) | Címsor csomópontok | 30200 | TÁVOLI ELJÁRÁSHÍVÁS | Az ütemterv szolgáltatás webes felhasználói felület |

### <a name="hbase-ports"></a>HBase portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Címsor csomópontok | 16000 | &nbsp; | &nbsp; |
| Webes felületének HMaster információ | Címsor csomópontok | 16010 | HTTP | A port HBase fő webes felhasználói felület |
| Régió kiszolgáló | Az összes dolgozó csomópontok | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | A port ZooKeeper csatlakozhat használó ügyfelek |

### <a name="kafka-ports"></a>Kafka portok

| Szolgáltatás | Csomópont | Port | Protocol (protokoll) | Leírás |
| ------- | ------- | ---- | -------- | ----------- |
| Ügynök  | Dolgozó csomópontok | 9092 | [Kafka protokoll](http://kafka.apache.org/protocol.html) | Az ügyfél kapcsolati használt |
| &nbsp; | Zookeeper csomópontok | 2181 | &nbsp; | A port Zookeeper csatlakozhat használó ügyfelek |
