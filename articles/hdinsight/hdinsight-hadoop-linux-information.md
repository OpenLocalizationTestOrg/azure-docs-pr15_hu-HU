<properties
   pageTitle="Tippek a Hadoop a Linux-alapú HDInsight |} Microsoft Azure"
   description="Végrehajtási tippeket Linux-alapú HDInsight (Hadoop) fürt az való használatához az Azure felhőben futó ismerős Linux környezetben."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>HDInsight Linux használatáról

Azure hdinsight szolgáltatáshoz fürt Linux-alapú Hadoop ismerős Linux környezetben, a Azure felhőben futó ad. Az elérhető összetevők többsége meg kell működniük megegyeznek a többi Linux rendszerhez Hadoop-telepítés. Ehhez a dokumentumhoz, vegye figyelembe a különbségekről hívások.

##<a name="prerequisites"></a>Előfeltételek

A lépéseket a jelen dokumentum számos használja az alábbi segédprogramok, és telepítve van a rendszer a szükséges.

* [cURL](https://curl.haxx.se/) – használatával kommunikál a szolgáltatások webes
* [jq](https://stedolan.github.io/jq/) - JSON dokumentumok elemzésére használható
* [Azure CLI](../xplat-cli-install.md) - használt távolról az Azure szolgáltatások kezelése

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>A tartománynevek

A teljes tartománynevét (FQDN) csatlakozáskor a fürthöz az internetről használatára van ** &lt;clustername >. azurehdinsight.net** vagy (csak SSH) ** &lt;clustername-ssh >. azurehdinsight.net**.

Belső az egyes a fürt csomópontjának van fürt konfigurálása közben rendelt neve. Keresse meg a csoport nevét, látogassa meg a __Hosts__ a Ambari webes Kezelőfelület azon, vagy használja az alábbi hosts listájának visszaadására Ambari REST API-t:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

__Jelszó__ cserélje le a fürt nevű és __CLUSTERNAME__ rendszergazdai fiók jelszavát. Ez ad vissza, amely tartalmazza a hosts a fürt listájának JSON dokumentum, majd a jq gyűjti össze a `host_name` minden állomás a fürt elem értéke.

Ha egy adott szolgáltatás a csomópont nevének megkereséséhez kell összetevő Ambari is lekérdezhetők. Például a Fájlrendszerhez neve csomópontot a hosts megkereséséhez használja a következő.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

A szolgáltatás leíró JSON dokumentumban visszaad, és majd jq gyűjti össze csak a `host_name` a hosts értékét.

## <a name="remote-access-to-services"></a>Távoli access Services szolgáltatásban

* **Ambari (web)** - https://&lt;clustername >. azurehdinsight.net

    Hitelesíteni a fürt rendszergazdaként és jelszava használatával, és jelentkezzen be Ambari. Ez is használ, a fürt rendszergazda felhasználó és a jelszavát.

    Hitelesítés egyszerű szöveges - mindig segítségével HTTPS győződjön meg arról, hogy a kapcsolat biztonságos.

    > [AZURE.IMPORTANT] A fürt Ambari közvetlenül az interneten keresztül érhető el, miközben a egyes funkciók támaszkodik csomópontok elérése a belső tartománynév a fürt használni. Mivel ez egy belső tartománynevet, és nem nyilvános kap "a kiszolgáló nem található" hibák meg az egyes funkciók eléréséhez az interneten keresztül.
    >
    > Az Ambari webhely felhasználói felület összes funkcióját használni, használja a proxy-alapú forgalmat webes központi csomópont-SSH alagutas. Lásd: a [Használatának SSH Tunneling Ambari webes felület, erőforrás-kezelő, JobHistory, NameNode, Oozie, és más webes Felhasználóifelület-féle eléréséhez](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Hitelesíteni a fürt rendszergazdaként és jelszavával.
    >
    > Hitelesítés egyszerű szöveges - mindig segítségével HTTPS győződjön meg arról, hogy a kapcsolat biztonságos.

* **WebHCat (Templeton)** - https://&lt;clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Hitelesíteni a fürt rendszergazdaként és jelszavával.
    >
    > Hitelesítés egyszerű szöveges - mindig segítségével HTTPS győződjön meg arról, hogy a kapcsolat biztonságos.

* **SSH** - &lt;clustername >-22-es vagy 23 port ssh.azurehdinsight.net. Az elsődleges headnode csatlakozni, miközben 23 használják, a másodlagos csatlakozhat port 22 használják. A központi csomópontok a további tudnivalókért lásd: [elérhetőségéről és az HDInsight Hadoop fürt megbízhatóságát](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Csak érheti el a fürt központi csomópontok SSH ügyfél számítógépről. Miután létrejött, majd hozzáférhet a dolgozó csomópontok egy headnode SSH használatával.

## <a name="file-locations"></a>Fájlok helye

Hadoop kapcsolódó fájlok találhatók a fürt csomópontok `/usr/hdp`. Ezt a címtárat a következő alkönyvtárak tartalmazza:

* __2.2.4.9-1__: ezt a címtárat a Hortonworks adatok platformot használja HDInsight, így a szám, a fürt lehet ugyanaz, mint az itt felsorolt egy verziójának neve.
* __aktuális__: ezt a címtárat a __2.2.4.9-1__ könyvtárban könyvtárak mutató hivatkozásokat tartalmaz, és létezik, hogy nem kell írja be a verziószám (Előfordulhat, hogy módosítani,), minden alkalommal, amikor azt szeretné, hogy a fájl elérésére.

Példa adatok és üveg fájlok találhatók Hadoop elosztott fájl rendszer (hdfs) lehetőségre, vagy az Azure Blob-tároló a "/ példa" vagy "wasbs: / / / példa".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>Fájlrendszerhez, Azure Blob-tárolóhoz és ajánlott eljárások a tárhely

A legtöbb Hadoop-terjesztését Fájlrendszerhez biztonsági helyi tároló a fürt gépeken. Bár ez hatékony, ez lehet költséges felhőalapú megoldás hol-előfizetést terhelő óránként vagy számítási erőforrások perc.

HDInsight Azure Blob-tárolóhoz használja az alapértelmezett tároló, amely a következő előnyöket nyújtja:

* Olcsó hosszú távú tárhely

* Külső szolgáltatásokból, például webhelyek, a fájl feltöltése és letöltése segédprogramok, a különböző nyelvi SDK és a böngészők kisegítő lehetőségek

Mivel ez az alapértelmezett áruházban HDInsight, általában nem kell tennie semmit sem kell alkalmazni. Például a következő parancsot a felsorolásban, amely megtalálható Azure Blob-tárolóhoz **/example/data** mappában fájlok:

    hdfs dfs -ls /example/data

Bizonyos parancsok megkövetelheti megadhatja, hogy Blob-tárolóhoz használja. Ezeknél a parancsot is előtag **wasb: / /**, vagy **wasbs: / /**.

HDInsight lehetővé teszi a fürt társíthatja Blob-tároló több fiókot is. Alapértelmezettől Blob-tároló fiókjában adatok eléréséhez is használhatja a formátum **wasbs: / /&lt;container-name>@&lt;fióknév >.blob.core.windows.net/**. Például az alábbi felsorolásban a **/example/data** könyvtár a megadott tárolóban és Blob-tároló fiók tartalma:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Milyen Blob-tárolóhoz a fürt használja?

Csoport létrehozása során kijelölt egy meglévő Azure tárterület-fiókot, és a tároló használja, vagy hozzon létre egy újat. Ezután lehet, mert elfelejtette vele. Az alapértelmezett tároló ügyfél és a tárolóban található, a Ambari REST API.

1. A következő paranccsal curl használatával Fájlrendszerhez konfigurációs adatok beolvasásához, és szűrése [jq](https://stedolan.github.io/jq/)használatával:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Ad eredményül, ez az első konfiguráció a kiszolgálón alkalmazott (`service_config_version=1`,) fogja tartalmazni, amelyek ezt az információt. Ha egy érték, amely a csoport létrehozását követően módosult keres, szükség lehet a konfigurációs verziók listája, valamint a legújabb egy.

    Ez értéket ad eredményül a következő, ahol __tároló__ alapértelmezett tárolóját, __fióknév__ pedig az Azure tároló fióknév hasonló:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Az erőforráscsoport beszerzése a tárterület-fiókot, akkor használja az [Azure CLI](../xplat-cli-install.md). A következő parancsot a tárhely fióknév Ambari beolvasása cserélje ki __fióknév__ :

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Ad vissza, ez az erőforrás-csoport nevét a fiókhoz.
    
    > [AZURE.NOTE] Ha semmi sem van ez a parancs, szükség lehet az Azure CLI átállítása Azure erőforrás-kezelő módba, és futtassa újra a parancsot. Erőforrás-kezelő Azure módba vált, használja a következő parancsot.
    >
    > `azure config mode arm`
    
2. A kulcs a tárterület-fiók beszerzése. Cserélje ki az előző lépésben az erőforráscsoport __CSOPORTNÉV__ . __Fióknév__ cserélje ki a tárterület-fiók neve:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Ad vissza, ez az elsődleges kulcs a fiókhoz.

A tároló információt az Azure-portálon is találhat:

1. Az [Azure-portálon](https://portal.azure.com/)válassza ki a HDInsight csoportját.

2. A __Essentials__ csoportban kattintson a __minden elérhető beállítás__.

3. __Beállítások__csoportban kattintson a __Azure tároló kulcsok__.

4. __Azure tároló kulcsok__, válassza a tárterület fiókjaihoz egyikét. Ekkor megjelenik a tárterület-fiókkal kapcsolatos információk.

5. Válassza a fő ikont. Ekkor megjelenik a tárterület-fiókhoz tartozó billentyűk.

### <a name="how-do-i-access-blob-storage"></a>Hogyan férhetek hozzá Blob-tárolóhoz

Más, mint a Hadoop parancs eltávolítása a keresztül vannak többféle módon BLOB eléréséhez:

* [A Mac, a Linux és a Windows Azure CLI](../xplat-cli-install.md): parancssor parancs a Azure. Telepítés után használhatja a `azure storage` tárolására, használatáról bővebben parancs vagy `azure blob` blob-specifikus parancsok.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): egy python parancsfájl-okkal Azure-tárolóban lévő használatához.

* SDK számos:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [NODE.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Fonetikus](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Tároló REST API-val](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>A fürt méretezése

A méretezés szolgáltatás fürt lehetővé teszi az Azure hdinsight szolgáltatáshoz a anélkül, hogy törli, és hozza létre újból a fürt futtató fürt által használt adatok csomópontok számának módosítása.

Más feladatok közben méretezési műveleteket végezheti el, vagy a fürthöz futnak folyamatok.

A különböző fürt típusú érinti a méretezés az alábbiak szerint:

* __Hadoop__: lefelé fürt csomópontok számának méretezésekor egyes szolgáltatások a fürt indítani. Emiatt feladatok fut, vagy a folyamatban lévő befejezésekor a méretezési művelet sikertelen lesz. A művelet befejezése után újra is elküldi a feladatokat.

* __HBase__: területi kiszolgálók automatikusan kiegyensúlyozott vannak a méretezési művelet befejezése után néhány percen belül. A kézi egyenleg területi kiszolgálók, kövesse az alábbi lépéseket:

    1. Csatlakozás a HDInsight fürt SSH használatával. A HDInsight SSH használja a további tudnivalókért lásd: a következő dokumentumokat:

        * [Az Linux rendszerhez, a Unix és a Mac OS X HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [A Windows HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. A következő segítségével a HBase rendszerhéj indítása:

            hbase shell

    2. Miután a HBase rendszerhéj betöltött, a következő segítségével manuálisan egyenleg a területi kiszolgálók:

            balancer

* __Vihar__: minden futó vihar topológiák kell visszaállás, egy méretezési művelet végrehajtása után. Ezzel a topológiát állítsa be újra a párhuzamos beállításokat, a fürt csomópontok új száma alapján. A futó topológiák visszaállás, használja az alábbi lehetőségek közül:

    * __SSH__: Csatlakozás a kiszolgálóhoz, és a következő parancs segítségével a topológia visszaállás:

            storm rebalance TOPOLOGYNAME

        A paramétert a párhuzamos útmutatók által a topológia eredetileg megadott felülbírálása. Ha például `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` fog konfigurálni a topológia 5 dolgozó folyamatok, a kék-spout összetevő végrendeleti 3 végrehajtó és a sárga-rögzített összetevő végrendeleti 10 végrehajtó.

    * __Vihar felhasználói felület__: kövesse az alábbi lépéseket a vihar felhasználói felületének használata a topológia visszaállás szeretne.

        1. Nyissa meg a webböngészőben CLUSTERNAME esetén a vihar fürt neve __https://CLUSTERNAME.azurehdinsight.net/stormui__ . Ha a rendszer kéri, adja meg a HDInsight fürt rendszergazda (rendszergazda) nevét és jelszavát, a csoport létrehozásakor meghatározott.

        3. Jelölje ki a kívánt visszaállás topológiát, majd jelölje be a __visszaállás__ gombra. Adja meg a késleltetés a rebalance művelet végrehajtása előtt.

A méretezés a HDInsight fürt adott tudnivalókért lásd:

* [A HDInsight Hadoop fürt kezelése az Azure portál használatával](hdinsight-administer-use-portal-linux.md#scaling)

* [Kezelése a Hadoop fürt HDinsight az Azure PowerShell használatával](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hogyan telepíthetem szín (vagy más Hadoop-összetevő)?

HDInsight felügyelt szolgáltatás, ami azt jelenti, hogy fürt csomópontjai semmisíteni is, és Azure, ha probléma merül fel automatikusan reprovisioned. Emiatt nem ajánlott webhelyen való manuális telepítéséhez dolog, amit közvetlenül a fürt csomópontok. Ehelyett használata a [HDInsight parancsfájl-műveletek](hdinsight-hadoop-customize-cluster.md) , ha telepítenie kell ezeket az alábbiakat:

* A szolgáltatás vagy a webhely, például a külső vagy szín.
* A módosításokat a fürt több csomóponton igénylő összetevőt. A szükséges környezetet változó például naplózás directory vagy a konfigurációs fájl létrehozása létrehozása.

Parancsfájl-műveletek Bash parancsfájlokat futtatta fürt kiépítési során, és telepítse és állítsa be a további összetevőit a fürt használható. Példa parancsfájlok áll rendelkezésre az alábbi összetevők telepítése:

* [Szín](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Egyéni parancsfájl-műveletek fejlesztésével kapcsolatos [parancsfájl műveletet fejlesztési HDInsight a](hdinsight-hadoop-script-actions-linux.md)témakörben talál.

###<a name="jar-files"></a>Üveg fájlok

Néhány Hadoop-technológiák részeként MapReduce feladatot, vagy a használt funkciók tartalmazó önálló üveg fájlokban találhatók malac vagy struktúra. Ezek telepíthető parancsfájl-műveletek, miközben azok gyakran bármelyik beállítás nincs szükség, és csak lehetnek kiépítése után a fürthöz feltöltött és használt közvetlenül. Ha szeretné, hogy a összetevő survives a fürt reimaging makle, üveg fájl tárolható WASB.

Például ha azt szeretné, [DataFu](http://datafu.incubator.apache.org/)legújabb verzióját szeretné használni, letöltheti a üveg, ahol az a projekt és töltse fel a HDInsight fürt. Malac vagy struktúra használatához kövesse a DataFu dokumentációt.

> [AZURE.IMPORTANT] Néhány különálló üveg fájlokban összetevők által biztosított HDInsight, de nem az elérési út. Ha egy adott összetevő keres, használhatja a követés szeretne keresni a fürt:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Ez ad vissza, üveg egyező fájlok elérési útját.

Ha a fürt már tartalmaz olyan összetevőt különálló üveg fájlként, de szeretne egy másik verzióját használja, töltse fel a összetevő új verziója a fürt, és próbáljon meg a feladatokat a.

> [AZURE.WARNING] A HDInsight fürt összetevői teljesen támogatott, és Microsoft Support segítséget nyújt azonosíthatók, és az alábbi összetevőket kapcsolatos problémák megoldásához.
>
> Egyéni összetevők kap, akár kereskedelmi célra is lehetővé teszi ügyfélszolgálatunkat, további elhárítani a problémát. Ez a probléma megoldása vagy kéri, hogy a hol található az adott technológia mély szakértelmét Megnyitás technológiák elérhető csatornák folytasson vezethet. Például vannak sok közösségi webhelyek használható, például: [HDInsight-fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projekteket is projektwebhelyek a [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [külső](http://spark.apache.org/).

## <a name="next-steps"></a>Következő lépések

* [Windows-alapú hdinsight áttelepítése Linux-alapú](hdinsight-migrate-from-windows-to-linux.md)
* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce feladatok használata](hdinsight-use-mapreduce.md)
