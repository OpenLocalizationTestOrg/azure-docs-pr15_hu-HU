<properties
   pageTitle="Üzembe helyezéséhez és kezeléséhez, a Linux-alapú HDInsight Apache vihar topológiák |} Microsoft Azure"
   description="Megtudhatja, hogy miként telepíthető, figyelésére és kezelheti a Linux-alapú HDInsight vihar irányítópulttal Apache vihar topológiák. Visual Studio Hadoop eszközeivel."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Üzembe helyezéséhez és kezeléséhez, a Linux-alapú HDInsight Apache vihar topológiák

A jelen dokumentum alapjai kezelésére és a HDInsight fürt Linux-alapú vihar futó vihar topológiák figyelése.

> [AZURE.IMPORTANT] Az ebben a cikkben leírt lépéseket szükség egy Linux-alapú vihar fürt hdinsight szolgáltatásból lehetőségre. Információ üzembe helyezése, és a Windows-alapú HDInsight topológiák figyelése [Deploy és kezelése a Windows-alapú HDInsight Apache vihar topológiák](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Előfeltételek

* **A Linux-alapú vihar HDInsight fürt**: lásd: az [első lépések a HDInsight Apache vihar](hdinsight-apache-storm-tutorial-get-started-linux.md) fürt létrehozásának lépései

* **SSH és SCP ismerős**: a következő témakörben találhat további tájékoztatást a HDInsight SSH és SCP használata:

    * **Linux, Unix vagy OS X ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux, az OS X vagy a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Egy SCP ügyfél**: Ez az összes Linux, Unix és OS X rendszerben megadva. A Windows-ügyfelek esetén javasoljuk PSCP, amely érhető el a [gitt letöltési oldalon](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

## <a name="start-a-storm-topology"></a>Indítsa el a vihar topológiában

### <a name="using-ssh-and-the-storm-command"></a>SSH és a vihar parancs használatával

1. A HDInsight fürthöz kapcsolatfelvétel SSH használatával. **USERNAME** cseréje a a SSH bejelentkezési nevét. **CLUSTERNAME** cserélje ki a HDInsight fürt neve:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    SSH használatáról a HDInsight fürthöz való kapcsolódás további tudnivalókért lásd: a következő dokumentumokat:
    
    * **Linux, Unix vagy OS X ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux, az OS X vagy a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. A következő parancsot használja egy példa topológia indítása:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Ez a példa WordCount topológia indul a fürt. Azt véletlenszerűen mondatok készítése és az egyes szót a mondatok előfordulását megszámolása.

    > [AZURE.NOTE] A fürthöz topológia elküldésekor kell másolnia a fürt tartalmazó használata előtt, üveg fájlt a `storm` parancsot. A lehet elvégezni használata a `scp` az ügyféltől, ahol a fájl létezik parancsot. Ha például`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > A WordCount példa és más vihar starter példák már megtalálhatók a fürthöz `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programozás útján

Kommunikáció a fürt tárolt Nimbus szolgáltatás által programozás útján üzembe a topológia vihar a hdinsight szolgáltatásból lehetőségre. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) példa Java-alkalmazások, mely szemlélteti, hogy miként telepíthető, és indítsa el a topológia a Nimbus szolgáltatáson keresztül biztosít.

## <a name="monitor-and-manage-using-the-storm-command"></a>Figyelheti és kezelheti a vihar paranccsal

A `storm` segédprogram lehetővé teszi a topológiák fut a parancssorból végezhető. Az alábbiakban megtalálja a leggyakrabban használt parancsok listája. Használat `storm -h` parancsok teljes listáját.

### <a name="list-topologies"></a>Lista topológiák

Használja a következő parancsot a lista összes topológiák fut:

    storm list
    
Ez ad vissza információk az alábbihoz hasonló:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Inaktiválja, majd újraaktiválása

Inaktiválásával a topológia szüneteltetheti, amíg meg nem levágni, vagy kapcsolta be újra. A következő segítségével inaktiválása és újraaktiválása:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>A futó topológiában törlése

Továbbra is a vihar topológiák, miután elindult, leállítja futtatását. A topológia leállításához használja az alábbi parancsot:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Visszaállás

A topológia szakismeretekre lehetővé teszi, hogy a rendszer a párhuzamos a topológia módosítása. Például ha, további megjegyzések hozzáadásához a fürt átméretezte, szakismeretekre lehetővé teszi, hogy egy futó topológia új csomópontok használja.

> [AZURE.WARNING] A topológia először szakismeretekre inaktiválja a topológia végig a fürt egyenletesen újra elosztja a dolgozók, majd végül adja eredményül a topológia szakismeretekre történt előtti állapotát. Így a topológia aktív volt, akkor válnak aktív újra. Ha inaktiválva, inaktiválva marad.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Figyelheti és kezelheti a vihar felhasználói felületének használata

A felhasználói felület vihar webes felületet biztosít a topológiák fut, és a HDInsight fürt megtalálható. A felhasználói felület vihar megtekintéséhez segítségével egy webböngészőben nyissa meg __https://CLUSTERNAME.azurehdinsight.net/stormui__, hol __CLUSTERNAME__ -e a csoport nevére.

> [AZURE.NOTE] Ha a felhasználónév és jelszó megadására kéri, adja meg a csoport rendszergazdája (rendszergazda) és mikor használt jelszót a fürt létrehozása.


### <a name="main-page"></a>Fő lapja

A felhasználói felület vihar a főoldalra az alábbi információk találhatók:

* **Összefoglaló fürt**: a vihar fürt vonatkozó alapadatok.

* **Összefoglaló topológia**: topológiák futó listáját. Ebben a szakaszban a hivatkozások használatával megtekintheti az adott topológiák további információt.

* **Összefoglaló felügyelő**: a vihar felügyelő információt.

* **Nimbus konfigurációs**: a fürt Nimbus konfigurációs.

### <a name="topology-summary"></a>Összefoglaló topológiája

Hivatkozás kijelölése a a **összefoglaló topológiája** csoport jeleníti meg a topológia az alábbi adatokat:

* **Összefoglaló topológia**: a topológia vonatkozó alapadatok.

* **Topológia műveletek**: adatkezelési műveletek végezhetők el a topológia.

  * **Aktiválás**: a inaktiválva topológiában önéletrajzok feldolgozása.

  * **Inaktiválás**: felfüggeszti a futó topológiában.

  * **Visszaállás**: a topológia a párhuzamos igazítása. A futó topológiák kell visszaállás, miután módosította a fürt csomópontok számának. A párhuzamos-e a fürt csomópontok nagyobb vagy csökkent számának kompenzálja topológiát lehetővé teszi.

    További információ a <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">egy vihar topológiájának a párhuzamos ismertetése</a>című cikk nyújt.

  * **Leállítása**: a vihar topológiában megszakítja az megadott idő után.

* **Topológia stat**: a topológia statisztikájának. Az **ablak** oszlopban a hivatkozások segítségével a lapon a többieknek határideje állít be.

* **Spouts**: a topológia által használt a spouts. Ebben a szakaszban a hivatkozások használatával megtekintheti az adott spouts további információt.

* **Bolts**: a topológia által használt a csapszegek. Ebben a szakaszban a hivatkozások használatával megtekintheti az adott csapszegek további információt.

* **Topológiájának konfigurálása**: a kijelölt topológia beállításait.

### <a name="spout-and-bolt-summary"></a>Spout és a rögzített összegzése

A következő információkat a kijelölt elem egy spout kijelölése a **Spouts** vagy **Bolts** szakaszokat jeleníti meg:

* **Összetevő összefoglalása**: alapvető spout vagy rögzített adatokat.

* **Spout/rögzített stat**: a spout vagy rögzített statisztikájának. Az **ablak** oszlopban a hivatkozások segítségével a lapon a többieknek határideje állít be.

* **Beviteli stat** (csak szög): a bemeneti adatfolyamok a rögzített elfogyasztott információt.

* **Kimeneti stat**: a ez által kibocsátott adatfolyamok információt spout vagy szög.

* **Végrendeleti végrehajtó**: a spout vagy rögzített előfordulását információt. Jelölje ki a **Port** az egy adott executor erre az előfordulásra vonatkozóan előállított diagnosztikai adatok naplója megtekintéséhez.

* **Hiba**: bármilyen hiba adatait spout vagy szög.

## <a name="rest-api"></a>REST API-VAL

A felhasználói felület vihar hasonló kezelési és funkciók figyelése a REST API segítségével végezhet, fölött a REST API épül. Egyéni eszközök kezelésére és figyelése vihar topológiák létrehozását a REST API-t is használhatja.

További tudnivalókért lásd: [Vihar felhasználói felület REST API -t](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Az alábbi adatokat az Apache vihar a HDInsight a REST API-t használja.

> [AZURE.IMPORTANT] A vihar REST API-t nem nyilvánosan elérhető az interneten keresztül, és egy SSH alagutas HDInsight központi csomópont való használatával is elérhető. Létrehozásával és használatával egy SSH alagutas további tudnivalókért lásd [Használata SSH Tunneling Ambari webes felület, erőforrás-kezelő, JobHistory, NameNode, Oozie, és más webes Felhasználóifelület-féle eléréséhez](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Alap URI

A HDInsight Linux-alapú fürt REST API-t az alap URI érhető el a központi csomóponton **https://HEADNODEFQDN:8744/api/v1/**; azonban a tartománynév a központi csomópont fürt létrehozása során jön létre és nem statikus.

A központi csomópont teljes tartománynevét (FQDN) számos különböző módon találja meg:

* __Az egy SSH munkamenet__: a paranccsal `headnode -f` egy SSH munkamenetből a fürthöz.

* __A Ambari webről__: jelölje be a __szolgáltatások__ a lap tetején, majd jelölje ki a __vihar__. Az __Összegzés__ lapon jelölje ki a __Vihar Felhasználóifelület-kiszolgálót__. A csomópont futtató a vihar felhasználói felület és a REST API-t a teljes Tartománynevét lesz a lap tetején.

* __Ambari REST API__: a paranccsal `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` beolvasni a csomópontra a vihar felhasználói felület és a REST API-t a futó adatait. __Jelszó__ cserélje le a fürt rendszergazdai jelszavát. __CLUSTERNAME__ cserélje le a csoport nevét. A válasz a "gazdaszámítógép_neve" szöveg tartalmazza a csomópont teljes Tartománynevét.

### <a name="authentication"></a>Hitelesítés

Kérelmek a REST API-nak **Alapszintű hitelesítés**, kell használnia, hogy használni a HDInsight fürt rendszergazda nevét és jelszavát.

> [AZURE.NOTE] Alapszintű hitelesítés használatával egyszerű szöveges küldi el, mert meg kell **mindig** használata a biztonságos kommunikáció a fürthöz HTTPS.

### <a name="return-values"></a>Visszatérési érték

Lehet, hogy a REST API-t a visszaadott információk csak a fürt vagy a virtuális gépeken futó ugyanazon a hálózaton Azure virtuális a fürt használható. A teljes tartománynevét (FQDN) esetén a visszaadott Zookeeper kiszolgálók például nem lesz elérhető legyen az internetről.

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta üzembe topológiák figyelése vihar irányítópultja használatával, és megtudhatja, hogyan [kidolgozása Java-alapú topológiák maven tesztelése használatával](hdinsight-storm-develop-java-topology.md)hogyan.

További példa topológiák listáját olvassa el a [Példa topológiák vihar a HDInsight-](hdinsight-storm-example-topology.md)című témakört.
