<properties
    pageTitle="Figyelheti a Ambari API HDInsight a Hadoop fürt |} Microsoft Azure"
    description="A Apache Ambari API-k létrehozása, kezelése és figyelése Hadoop fürt használata. Operátor használható eszközök és az API-elrejtése Hadoop komplexitását."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>A HDInsight a Ambari API Hadoop fürt figyelése

Megtudhatja, hogy miként figyelheti a HDInsight fürt Ambari API-khoz használatával.

> [AZURE.NOTE] Ez a cikk adatai elsősorban a Windows-alapú HDInsight fürt, amely biztosítja a Ambari REST API csak olvasható verzióját. Linux-alapú fürt olvassa el a [kezelése a Hadoop fürt Ambari használatával](hdinsight-hadoop-manage-ambari.md)című témakört.

## <a name="what-is-ambari"></a>Mi az Ambari?

[Apache Ambari] [ ambari-home] kiépítési, kezelése és Apache Hadoop fürt ellenőrzésére szolgál. Egy ötletes webhelycsoport operátor eszközök és Hadoop, fürt működésének egyszerűsítése komplexitását elrejtése API-k robusztus halmazából tartalmazza. [Ambari API – segédlet]című cikkben olvashat az API-k[ambari-api-reference]. 

HDInsight jelenleg csak a Ambari felügyeleti funkció támogatja. Ambari API 1.0 HDInsight 3.0-s és 2.1-es verziójú fürt által támogatott. Ez a cikk bemutatja a HDInsight 3.1-es és 2.1-es verziójú fürt elérésekor Ambari API-k. Kulcs a kettő közötti különbség, hogy egyes összetevői (például a feladat előzmények Server) az új funkciók bevezetésével megváltozott. 

**Előfeltételek**

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Nem kötelező) [cURL] [curl]. Telepítheti azt, lásd: a [cURL kiadásairól és -letöltések][curl-download].

    >[AZURE.NOTE] Ha a paranccsal cURL a Windows rendszerben használata idézőjelbe szimpla idézőjelek lehetőséget az értékek helyett.

- **Az Azure hdinsight szolgáltatáshoz fürt**. Fürt kiépítési kapcsolatos útmutatásért lásd: a [HDInsight használatának első lépései] [ hdinsight-get-started] vagy [rendelkezést HDInsight fürt][hdinsight-provision]. Szüksége lesz az oktatóprogram folyamatát, az alábbi adatokat:

    Fürt tulajdonság|Azure PowerShell változó neve|Érték|Leírás
    ---|---|---|---
    HDInsight fürt neve|$clusterName||A HDInsight fürt neve.
    Fürt felhasználónév|$clusterUsername||A megadott fürt felhasználó nevét a fürt létrehozásának ideje.
    Fürt jelszó|$clusterPassword||Fürt felhasználó jelszavát.

    >[AZURE.NOTE] Fill-in az értékeket a táblázatban. Ez lesz hasznos ebben az oktatóanyagban keresztül.

## <a name="jump-start"></a>Ugrás a indítása

Többféle módon Ambari használata a Lync-HDInsight fürt.

**Azure PowerShell használata**

A következő egy Azure PowerShell-parancsprogramot MapReduce feladat nyilvántartása információkért *HDInsight 3.1 fürt.*  A fő különbség, hogy azt lekérés ezeket a fonal szolgáltatás (helyett MapReduce) adatait.

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Az alábbiakban az Azure PowerShell-parancsprogramot, a MapReduce feladat nyilvántartása adatok *HDInsight-2.1 fürt*ismertető:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

A kimenet van:

![Jobtracker kimeneti][img-jobtracker-output]

**CURL használata**

Az alábbi képen fürt lehetőségekről cURL használatával:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

A kimenet van:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**A 10/8/2014-es kiadás**:

A Ambari végpont használatakor "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", a *gazdaszámítógép_neve* mezőben adja eredményül teljes tartománynevét (FQDN) az állomásnév helyett a csomópontot. A 10/8/2014-es verzió, mielőtt ez a példa egyszerűen "**headnode0**" adja vissza. Után a 10/8/2014-es verzió, akkor a teljesen minősített tartománynév "**headnode0. { ClusterDNS} .azurehdinsight .net**", az előző példában látható módon. Ez a változás esetek, amikor egy virtuális hálózati (VNET) (például HBase és Hadoop) többféle fürt telepíthető megkönnyítésére volt szükség. Ez történik, ha például HBase a háttéradatbázist platformot, Hadoop-használatakor.

## <a name="ambari-monitoring-apis"></a>Ambari API-khoz figyelése

Az alábbi táblázat néhány a leggyakoribb Ambari figyelése API-hívások. Az API-val kapcsolatos további tudnivalókért lásd [Ambari API][ambari-api-reference].

Hívás monitor API|URI|Leírás
---|---|---
Fürt beszerzése|`/api/v1/clusters`|
Fürt Infó.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|fürt, a szolgáltatások, a hosts
Szolgáltatások|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Szolgáltatások tartalmazzák a: fájlrendszerhez, mapreduce
Szolgáltatások Infó.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Szolgáltatás-összetevők beszerzése|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|Fájlrendszerhez: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Infó összetevőt.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, host-összetevők, a mérőszámok
Hosts beszerzése|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
A host Infó.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
A host összetevők beszerzése|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, erőforrás-kezelő
Infó host összetevőt.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles, összetevők, a host, a mérőszámok
Konfigurációk beszerzése|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config típusok: alapvető webhelyeken, fájlrendszerhez webhelyeken, mapred-webhely, struktúra-webhely
Konfigurációs Infó.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config típusok: core webhelyeken, fájlrendszerhez webhelyeken, mapred webhelyeken, struktúra-webhely


##<a name="next-steps"></a>Következő lépések

Most már megtanulta azt is van API-hívások figyelése Ambari használatáról. További tudnivalókért lásd:

- [Az Azure-portálon HDInsight fürt kezelése][hdinsight-admin-portal]
- [Azure PowerShell használatával HDInsight fürt kezelése][hdinsight-admin-powershell]
- [Parancssori felhasználói felületén HDInsight fürt kezelése][hdinsight-admin-cli]
- [Dokumentáció hdinsight szolgáltatáshoz][hdinsight-documentation]
- [Első lépések a hdinsight szolgáltatáshoz][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
