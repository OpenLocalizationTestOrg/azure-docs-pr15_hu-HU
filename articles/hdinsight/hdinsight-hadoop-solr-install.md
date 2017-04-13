<properties
    pageTitle="Parancsfájl művelettel Hadoop fürt Solr telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként szabhatja testre a HDInsight fürt Solr parancsfájl művelettel együtt."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Telepítése és HDInsight Hadoop fürt Solr használata

Megtudhatja, miként szabhatja testre a Windows-alapú HDInsight fürt Solr parancsfájl művelettel együtt, és hogyan Solr segítségével adatokat kereshet. A Linux-alapú fürt Solr használja a további tudnivalókért lásd [telepítése és használata a HDinsight Hadoop fürt (Linux) Solr](hdinsight-hadoop-solr-install-linux.md).
 
Azure hdinsight szolgáltatáshoz Solr fürt (Hadoop, vihar, HBase, külső) bármilyen típusú telepíthető *Művelet parancsfájl*használatával. A csak olvasható Azure tároló blob [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)a Solr telepítése egy HDInsight fürthöz mintaparancsfájl érhető el. 

A mintaparancsfájl csak 3.1-es HDInsight fürt működik. HDInsight fürt verzióján további tudnivalókért olvassa el a [HDInsight fürt verzióival](hdinsight-component-versioning.md)foglalkozó.

Az ebben a témakörben használt mintaparancsfájl a Windows-alapú Solr fürtre létrehoz egy adott konfigurációt. Ha szeretne a Solr fürt állítson be más webhelycsoportok shards, sémák, kópiák, stb, módosítania kell a parancsfájlt és Solr bináris lehetőséget.

**Kapcsolódó cikkek**

- [Telepítése és használata Solr HDinsight Hadoop fürt (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [A HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md): HDInsight fürt általános információkat.
- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-cluster-customize]: általános tudnivalók a parancsprogram művelettel HDInsight fürt testreszabása.
- [Parancsfájl művelet kidolgozása parancsfájlok hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Mi az Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés. Hadoop lehetővé teszi, hogy tárolása és kezelése a nagy mennyiségű adatot, miközben a Apache Solr gyorsan az adatok beolvasásához a keresési lehetőségeket biztosít. 

## <a name="install-solr-using-portal"></a>Telepítse a Solr portál használatával

1. Indítsa el a fürt létrehozása **Egyéni létrehozása** beállítás használatával, [a HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md#portal)leírtak.
2. A varázsló **Parancsfájl-műveletek** lapon kattintson a **parancsprogram művelet hozzáadása** a parancsfájl műveletet részleteinek megadására, alább látható módon:

    ![Parancsfájl műveletek fürt testreszabása] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Parancsfájl műveletek fürt testreszabása")

    <table border='1'>
        <tr><th>A tulajdonság</th><th>Érték</th></tr>
        <tr><td>név</td>
            <td>Adja meg, hogy a parancsprogram művelet nevét. Ha például <b>Solr telepítése</b>.</td></tr>
        <tr><td>Parancsfájl URI</td>
            <td>Adja meg a parancsfájlt, ha testre szeretné szabni a fürt hív meg egységes erőforrás azonosító (URI). Ha például <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Csomópont típusa</td>
            <td>Adja meg a csomópontok a testreszabási parancsfájl futtatható. Megadhatja, hogy <b>csomópontjait</b>, <b>csak a központi csomópontok</b>vagy <b>csak a dolgozó csomópontok</b>.
        <tr><td>Paraméterek</td>
            <td>Szükség szerint a parancsfájlt, adja meg a paraméterek. A parancsfájl Solr telepítéséhez nincs szükség paramétereket, így ez akár üresen is hagyhat.</td></tr>
    </table>

    A fürt több összetevők telepítése egynél több parancsfájl műveletet is hozzáadhat. Miután hozzáadta a parancsfájlok, kattintson a csoport létrehozásához a bejelölést.


## <a name="use-solr"></a>Solr használata

Kell kezdődnie Solr indexelés az egyes adatfájlok használatával. Az indexelt adatok keresési lekérdezések futtatásához Solr használhatja. A következő lépésekkel egy HDInsight fürthöz Solr használata:

1. **Használata távoli asztali Protocol (RDP) távoli Solr telepítve van a HDInsight fürthöz be**. Az Azure portálról engedélyezése a távoli asztali a telepített Solr és majd remote be a fürt létrehozott fürt. Útmutatásért lásd: [Csatlakozás HDInsight fürt RDP segítségével](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Index Solr adatfájlok feltöltésével**. Ha indexelést állít be Solr, helyezi dokumentumokat, előfordulhat, hogy a keresés. Tárgymutató-Solr, használatával RDP távoli be a fürt, nyissa meg az asztali, nyissa meg a Hadoop parancssori, és **C:\apps\dist\solr-4.7.2\example\exampledocs**nyissa meg. A következő parancsot:

        java -jar post.jar solr.xml monitor.xml

    A következő kimenet a konzol jelenik meg:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    A post.jar segédprogram Solr indexek a két minta dokumentumokat, **solr.xml** és **monitor.xml**. A post.jar segédprogram és a minta dokumentumok Solr telepítés érhetők el.

3. **Használja a Solr irányítópult belül az indexelt dokumentumok kereséséhez**. Az a HDInsight fürthöz RDP-munkamenet, nyissa meg az Internet Explorerben, és indítsa el a Solr irányítópult **http://headnodehost:8983/solr / #/**. A bal oldali ablaktáblában, az **Alapvető Adatkijelölő** legördülő **collection1**jelölje ki, és belül, hogy a **lekérdezés**gombra. Példaként jelölje ki, és az összes dokumentumok szerepelniük Solr, adja meg az alábbi értékeket:

    * A **kérdések** mezőbe írja be a ** \*:**\*. Ez a dokumentumok indexelt szerepelniük Solr. Ha szeretne keresni egy adott karakterlánc belül a dokumentumok, karakterlánc Itt adhatja meg.
    
    * Válassza a kimeneti formátum **wt** mezőbe. Alapértelmezés szerint **json**. Kattintson a **lekérdezés**gombra.

    ![Parancsfájl műveletek fürt testreszabása] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr irányítópulton lekérdezések futtatása")
    
    A kimenet Solr indexelés használt két dokumentumok adja eredményül. A kimenet az alábbihoz hasonló:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Ajánlott: készítsen biztonsági másolatot a HDInsight fürt társított Azure Blob-tárolóhoz Solr indexelt adatainak**. Jó módszer készítsen biztonsági másolatot az indexelt adatokat a Solr fürt csomópontjának Azure Blob-tárolóhoz alakzatot. Végezze el ezt az alábbi lépéseket:

    1. Nyissa meg az Internet Explorer az RDP-munkamenet, és mutasson az alábbi URL-címe:

            http://localhost:8983/solr/replication?command=backup

        Meg kell jelennie egy válasz jelennek meg:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. A távoli munkamenetet, keresse meg azt a {SOLR_HOME}\{webhelycsoport} \data. A fürt a mintaparancsfájl keresztül létrehozott ezzel **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**kell lennie. Ezen a helyen, meg kell jelennie létrehozott *hasonló nevű mappa pillanatkép *pillanatkép.* időbélyeg***.

    3. Zip-a pillanatkép mappát, és töltse fel a Azure Blob-tárolóhoz. A Hadoop parancssorból nyissa meg a pillanatkép mappáját a következő parancs használatával:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Ez a parancs a pillanatkép másolja át, és /example/data az alapértelmezett fürthöz társított tároló számla belüli a tároló területen.

## <a name="install-solr-using-aure-powershell"></a>Telepítse a Solr Aure PowerShell használatával

Lásd: a [parancsfájl művelettel testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  A minta szemlélteti, hogyan telepítheti az Azure PowerShell használatával külső. Ha testre szeretné szabni a parancsfájlt [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)szüksége.

## <a name="install-solr-using-net-sdk"></a>.NET SDK használatával Solr telepítése

Lásd: a [parancsfájl művelettel testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). A minta szemlélteti, hogyan telepítheti a .NET SDK használatával külső. Ha testre szeretné szabni a parancsfájlt [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)szüksége.



## <a name="see-also"></a>Lásd még:

- [Telepítése és használata Solr HDinsight Hadoop fürt (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [A HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md): HDInsight fürt általános információkat.
- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-cluster-customize]: általános tudnivalók a parancsfájl művelettel HDInsight fürt testreszabása.
- [Parancsfájl művelet kidolgozása parancsfájlok hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-script-actions.md).
- [Telepítése és használata a külső HDInsight fürt][hdinsight-install-spark]: külső telepítéséről parancsfájl műveletet minta.
- [R telepítése HDInsight fürt][hdinsight-install-r]: j telepítéséről parancsfájl műveletet minta
- [A HDInsight fürt telepítése Giraph](hdinsight-hadoop-giraph-install.md): parancsfájl műveletet minta Giraph telepítéséről.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
