<properties 
   pageTitle="Használja a Apache Phoenix és a HDInsight erdeimókus |} Microsoft Azure" 
   description="Útmutató: Apache Phoenix használata a hdinsight szolgáltatásból lehetőségre, és szeretné telepíteni és erdeimókus konfigurálja a HDInsight-HBase fürthöz való kapcsolódás a számítógépen." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>A HDInsight Linux-alapú HBase fürt Apache Phoenix használata  

Ismerje meg HDInsight [Apache Phoenix](http://phoenix.apache.org/) használatáról és a SQLLine használatáról. Phoenix kapcsolatos további tudnivalókért olvassa el a [15 perces vagy annál kisebb Phoenix](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html)című témakört. Phoenix nyelvhelyesség-ellenőrzés [Phoenix nyelvhelyesség-ellenőrzés](http://phoenix.apache.org/language/index.html)című témakör tartalmaz.

>[AZURE.NOTE] HDInsight Phoenix verzió információ [HDInsight által biztosított Hadoop fürt verziók újdonságai?] [hdinsight-versions].

##<a name="use-sqlline"></a>SQLLine használata
[SQLLine](http://sqlline.sourceforge.net/) SQL utasítás végrehajtása egy parancssori segédprogramot. 

###<a name="prerequisites"></a>Előfeltételek
SQLLine használata előtt az alábbiakat kell rendelkeznie:

- **A HBase fürt hdinsight szolgáltatásból lehetőségre**. Rendelkezést HBase információkat fürt, lásd: az [első lépések a HDInsight Apache HBase][hdinsight-hbase-get-started].
- **Csatlakozás a HBase fürthöz a távoli asztali protokollon keresztül**. Című cikkben olvashat [a HDInsight az Azure klasszikus portál használatával kezelése a Hadoop fürt][hdinsight-manage-portal].


Amikor csatlakozik egy HBase fürthöz, szüksége lesz az Zookeepers csatlakozni. Minden HDInsight fürt 3 Zookeepers tartalmaz. 

**Megtudhatja, hogy a Zookeeper állomásnév**

1. Nyissa meg a Tallózás gombra, és Ambari **https://<ClusterName>. azurehdinsight.net**.
2. Írja be a HTTP (fürthöz) felhasználónevével és jelszavával jelentkezzen be.
3. **ZooKeeper** kattintson a bal oldali menüben. Kell látni 3 **ZooKeeper kiszolgáló** szerepel a listában.
4. Kattintson a felsorolt **ZooKeeper kiszolgáló** . Keresse meg az összefoglalását tartalmazó munkaablakban a **Hostname (állomásnév)**. Hasonlít *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine használata**

1. Csatlakozás a fürt SSH használatával. Című cikkben olvashat [Használata SSH Linux-alapú Hadoop a HDInsight Linux rendszerhez, a Unix, vagy az OS X a](hdinsight-hadoop-linux-use-ssh-unix.md) vagy [Használata SSH Linux-alapú Hadoop a HDInsight a Windows,](hdinsight-hadoop-linux-use-ssh-windows.md) attól függően, hogy az ügyfélgépen beállított OS.

2. SSH futtassa a következő parancsokat SQLLine futtatásához:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. A következő parancsokat HBase táblázatot szeretne létrehozni, és bizonyos adatok beszúrása:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

További tudnivalókért lásd a [SQLLine kézi](http://sqlline.sourceforge.net/#manual) és a [Székesfehérvári nyelvhelyesség-ellenőrzés](http://phoenix.apache.org/language/index.html)elemre.


 
##<a name="next-steps"></a>Következő lépések
Ez a cikk Apache Phoenix haszná HDInsight rendelkezik megtanulta.  További tudnivalókért lásd:

- [HDInsight HBase áttekintése][hdinsight-hbase-overview]: HBase egy Apache, Megnyitás-forrást, NoSQL adatbázist, amelybe elérésű és erős konzisztencia semistructured strukturált és strukturálatlan adatokat nagy mennyiségű Hadoop-ra épülő.
- [Azure virtuális hálózati HBase fürt kiépítése][hdinsight-hbase-provision-vnet]: virtuális hálózati integrációval HBase fürt telepíthető az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül a HBase is kommunikálhatnak.
- [A HDInsight konfigurálása HBase replikációs](hdinsight-hbase-geo-replication.md): megtudhatja, hogy miként HBase replikáció konfigurálása két Azure adatközpontokkal keresztül. 
- [A HDInsight HBase a Twitteren sentiment elemzése][hbase-twitter-sentiment]: megtudhatja, hogy miként végezze el a valós idejű [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) nagy adatok HDInsight a Hadoop fürt HBase használatával.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
