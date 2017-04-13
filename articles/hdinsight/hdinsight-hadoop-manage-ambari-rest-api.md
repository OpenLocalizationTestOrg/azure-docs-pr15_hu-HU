<properties
   pageTitle="Figyelheti és kezelheti a Apache Ambari REST API HDInsight fürt |} Microsoft Azure"
   description="Megtudhatja, hogy miként figyelheti és kezelheti a HDInsight Linux-alapú fürt Ambari használatával. A jelen dokumentum megtanulhatja a Ambari REST API-val HDInsight fürt részét képező használatáról."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>HDInsight fürt kezelése a Ambari REST API segítségével

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari egyszerűbbé teszi a kezelése és figyelése a Hadoop fürtre, mert a webhely felhasználói felület és a REST API-t egy könnyen. Ambari Linux-alapú HDInsight fürt szerepel, és a fürt figyelésére és konfigurációs változtatások szolgál. A dokumentumban, alapjai a gyakori feladatok cURL végrehajtásával Ambari REST API-val való használatáról.

##<a name="prerequisites"></a>Előfeltételek

* [cURL](http://curl.haxx.se/): cURL egy platformok segédprogram REST API-k használata a parancssorból használható. A jelen dokumentum használatos használatával kommunikál a Ambari REST API-t.
* [jq](https://stedolan.github.io/jq/): jq platformok parancssori eszköz JSON-dokumentumokkal dolgozik. A jelen dokumentum használatos elemzésére Ambari REST API-val – által eredményül adott JSON dokumentumokat.
* [Azure CLI](../xplat-cli-install.md): Azure szolgáltatások a platformok parancssori segédprogramot.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Mi az Ambari?

[Apache Ambari](http://ambari.apache.org) teszi kezelése a Hadoop egyszerűbb, mert egy könnyen használható webes felhasználói felület kiépítése, kezelése és figyelése Hadoop fürt használható. A fejlesztők integrálhatja e ezekre a lehetőségekre a alkalmazásokba a [Ambari REST API -khoz](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)használatával.

Ambari Linux-alapú HDInsight fürt alapértelmezés szerint megadva.

##<a name="rest-api"></a>REST API-VAL

Az alap URI a Ambari REST API-t a HDInsight érték https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, ahol __CLUSTERNAME__ a csoport nevére.

> [AZURE.IMPORTANT] Amikor a fürt nevét a URI (CLUSTERNAME.azurehdinsight.net) teljesen minősített tartománynév nevét (FQDN) részében nagybetűk, további előfordulásait az URI nagybetűk között. Ha a csoport neve en_furtom nevű fürt, az alábbiak mindegyike érvényes URL-címe:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Az alábbi URL-címe egy hibaüzenetet adja vissza, mivel a második előfordulás neve nem a megfelelő esetben.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

A HDInsight Ambari kapcsolódás szükséges HTTPS. A kapcsolat hitelesítő, amikor a rendszergazdai fiók nevére (az alapértelmezett érték __felügyeleti__), és a csoport létrehozásakor a megadott jelszót kell használnia.

Az alábbi példában cURL, hogy a GET kérésekben szemben a REST API-t. __Jelszó__ cserélje le a fürt rendszergazdai jelszavát. __CLUSTERNAME__ cserélje ki a fürt neve:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
A válasz a következőhöz hasonló adatokat kezdődő JSON dokumentum:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Mivel ez JSON, célszerűbb JSON-elemzővel használata az adatokat. Például az alábbi példában jq megjelenítéséhez csak a `health_report` elemet.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Példa: A teljesen minősített tartománynév fürt csomópontok beszerzése

HDInsight-használatakor előfordulhat, hogy csomópont teljes tartománynevét (FQDN). A teljesen minősített tartománynév a különböző csomópontok a fürt a következők használatával egyszerűen meghallgathatja:

* __Címsor csomópontok__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Dolgozó csomópontok__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper csomópontok__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Ezek a példák mindegyike kövesse az azonos típusúak Megjegyzés:

1. Tudjuk összetevő lekérdezést futtat, a azokat a csomópontokat.
2. Beolvasni a `host_name` elemeket, amelyek tartalmaznak, ezek csomópontok a teljesen minősített tartománynév.

A `host_components` elem a feladó dokumentum több elemet tartalmaz. Használatával `.host_components[]`, és majd adja meg az elemben elérési ismétlése mindegyik elemhez és kijjebb mezőbe írja be az adott elérési útja látható. Ha csak egy érték, például az első tételt FQDN gyűjtemény adja vissza az elemeket, és válassza az egy adott bejegyzést:

    jq '[.host_components[].HostRoles.host_name][0]'

Ez a gyűjteményben az első FQDN adja eredményül.

##<a name="example-get-the-default-storage-account-and-container"></a>Példa: Tároló és a alapértelmezett tárterület-fiók beszerzése

Amikor létrehoz egy HDInsight fürthöz, kell használnia Azure tárterület-fiók és egy blob-tárolóhoz az alapértelmezett tárolására a fürt. A csoport létrehozását követően ezeket az adatokat lekérdezni Ambari is használhatja. Például ha azt szeretné, hogy programozás útján írási adatokat közvetlenül a tároló.

A következő beolvassa az WASB URI a fürt alapértelmezett tároló:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Ez a parancs az első konfiguráció a kiszolgálón alkalmazott (`service_config_version=1`,) amely tartalmazza ezeket az adatokat. Ha beolvashatja egy érték, amely a csoport létrehozását követően módosult, szükség lehet a konfigurációs verziók listája, valamint a legújabb egy.

Ez értéket ad eredményül a következő példában, amelyben __tároló__ alapértelmezett tárolóját, __fióknév__ pedig az Azure tároló fióknév hasonló:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Fel-vagy adatok letöltése a tároló a az [Azure CLI](../xplat-cli-install.md) , majd használja ezt az információt.

1. Az erőforráscsoport beszerzése a tárterület-fiókot. __Fióknév__ cserélje ki a tárhely fióknév Ambari beolvasása:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Ez a az erőforráscsoport a fiók nevét adja eredményül.
    
    > [AZURE.NOTE] Ha semmi sem van ez a parancs, szükség lehet az Azure CLI átállítása Azure erőforrás-kezelő módba, és futtassa újra a parancsot. Erőforrás-kezelő Azure módba vált, használja az alábbi parancsot:
    >
    > `azure config mode arm`
    
2. A kulcs a tárterület-fiók beszerzése. Cserélje ki az előző lépésben az erőforráscsoport __CSOPORTNÉV__ . __Fióknév__ cserélje ki a tárterület-fiók neve:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Ebben a példában az elsődleges kulcs a fiók adja eredményül.
    
3. A feltöltés paranccsal fájlok tárolása a tároló:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    __Fióknév__ cserélje le a tárterület-fiók nevére. __ACCOUNTKEY__ cserélje le a korábban beolvasott billentyűt. __Fájl elérési ÚTJA__ a fájl elérési útját fel szeretné tölteni, az elérési utat a tároló __BLOBPATH__ állapotában.

    Ha például azt szeretné, hogy a HDInsight wasbs://example/data/filename.txt jelennek meg a fájlt, majd __BLOBPATH__ akkor `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Példa: Frissítés Ambari konfigurálása

1. A jelenlegi beállításokat, és a "a kívánt konfiguráció" Ambari tárolja kap:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Ebben a példában a szoftver jelenlegi konfigurációjának megfelelően (a _címke_ érték jelölt) tartalmazó JSON dokumentumot a telepítve van a fürt a összetevők adja eredményül. Az alábbi példában egy olyan külső fürt típusból visszaadott adatok kivonat.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Ebből a listából kell másolnia összetevő neve (például __külső\_thrift\_sparkconf__ és a __címke__ értéket.
    
2. Beolvashatja az adatokat a összetevő és a címkét a következő parancs használatával. __A külső-thrift-sparkconf__ és a __kezdeti__ cserélje összetevő, és az adatokat lekérdezni kívánt címkére.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Curl beolvassa a JSON-dokumentumot, majd jq használják, hogy az adatok módosítását, sablon létrehozásához. A sablon hozzáadása/módosítása konfigurációs értékek majd szolgál. Kifejezetten hajtja végre a következőket:
    
    * Létrehoz egy egyedi, amelyben a "verzió" karakterlánc és a dátumot, __newtag__tárolt.
    * Az új a kívánt konfiguráció a legfelső szintű dokumentum jön létre.
    * A tartalmát kapja a `.items[]` tömb, és hozzáadja a __desired_config__ elem alatt.
    * Törli a __href__, __verziószám__és __Config__ elemeket, ezeket az elemeket új konfiguráció küldhetik nem szükség szerint.
    * Felveszi az új __címke__ elemet, és az értékre állítja __verzió ###__. A numerikus rész az aktuális dátum alapul. Minden egyes kereséskonfigurációs egyedi címke kell rendelkeznie.
    
    Végül az adatok mentése a __newconfig.json__ dokumentumot. A dokumentum szerkezetét jelenjen meg a következőhöz hasonló:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Nyissa meg a __Tulajdonságok__ objektum __newconfig.json__ dokumentum és hozzáadása/módosítása értékeket. A következő példa __"spark.yarn.am.memory"__ értékének megváltoztatása __"" 1 "g"__ __"3__ g" és, és elhelyezi a __"spark.kryoserializer.buffer.max"__ , __"256 m"__érték az új elemet.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Ha projekten módosításokat végzett, mentse a fájlt.

4. A következő paranccsal a frissített konfiguráció Ambari nyújt.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Ez a parancs a curl kérelem elküldi az új a kívánt konfiguráció a fürt __newconfig.json__ fájl tartalmának pipák. A cURL kérelem JSON dokumentum adja eredményül. A dokumentum __versionTag__ elemének meg kell egyeznie az Ön által küldött verzióra, és a __konfigurációk__ objektum fogja tartalmazni a kért módosításokat.

###<a name="example-restart-a-service-component"></a>Példa: Indítsa újra a szolgáltatáshoz

Ezen a ponton akkor tekintse meg a Ambari webes felhasználói felület, ha a külső szolgáltatás jelzi, hogy szüksége van az új konfiguráció érvénybe léptetéséhez újra kell indítani. A szolgáltatás újraindításához kövesse az alábbi lépéseket.

1. A következő segítségével a külső szolgáltatás karbantartási mód engedélyezése:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Ez a parancs a JSON-dokumentum küldése a kiszolgáló (található a `echo` utasítás,) amely karbantartási mód bekapcsolása.
    Ellenőrizheti, hogy a szolgáltatás már a karbantartási módban használja a következő kérelmet:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Ezt az értéket ad eredményül `"ON"`.

3. Ezután a következő segítségével a szolgáltatás kikapcsolása:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Ez a parancs választ ad az alábbihoz hasonló.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    A `href` e URI által visszaadott érték használja a csomópont belső IP-címét. Ahhoz, hogy használhassa a kívül a fürt, cserélje le a "10.0.0.18:8080" rész a fürt teljes Tartománynevét. A következő parancs például a kérelem állapotának olvassa be.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Ha ezt az értéket ad eredményül `"COMPLETED"` , majd a kérelem befejeződött.

4. Ha az előző kérelem befejeződött, a következő használatával indítsa el a szolgáltatást.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    A szolgáltatás újraindítása után célszerű az új beállítások.

5. Végül használja az alábbi karbantartási mód kikapcsolása.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Következő lépések

A REST API teljes hivatkozást a [Ambari API hivatkozás V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)című témakör tartalmaz.

> [AZURE.NOTE] Néhány Ambari funkció le van tiltva, mint HDInsight felhőszolgáltatásba; kezeli például hozzáadása vagy eltávolítása a hosts a fürt, vagy új szolgáltatások hozzáadása.
