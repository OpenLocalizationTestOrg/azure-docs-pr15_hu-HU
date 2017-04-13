<properties
    pageTitle="Hadoop, HBase vagy vihar fürt létrehozása az Azure-platformok CLI használatával HDInsight a Linux |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre Linux-alapú HDInsight fürt az Azure-platformok CLI, erőforrás-kezelő Azure-sablonok és Azure REST API-t. Adja meg az fürt (Hadoop, HBase vagy vihar), vagy parancsfájlok használata egyéni összetevők telepítése."
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

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Az Azure CLI használatával HDInsight Linux-alapú fürt létrehozása

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Az Azure CLI, amely lehetővé teszi, hogy az Azure-szolgáltatások kezelése platformok parancssori segédprogramot. Akkor használható, együtt Azure erőforrás management sablonokat, hozzon létre egy HDInsight fürthöz együtt társított tároló fiókok és más szolgáltatásokhoz.

Azure az erőforrás-kezelés sablonok JSON dokumentumokat, amelyek bemutatják __erőforráscsoport__ és a benne lévő összes erőforrás, (például HDInsight.) Ez a sablon-alapú megközelítés lehetővé teszi, hogy egy sablonba HDInsight szükséges összes erőforrás. Is lehetővé teszi a csoport változások kezelése __telepítések__, amelyen alkalmazni a módosításokat a teljes csoporton keresztül egészének.

A dokumentum lépéseit az Azure CLI és a sablon használatával új HDInsight fürt létrehozásának folyamata végigvezetik.

> [AZURE.IMPORTANT] A lépéseket a dokumentum egy HDInsight fürthöz az alapértelmezett szám dolgozó csomópontok (4) használata. Ha több mint 32 dolgozó csomóponton (fürt létrehozása során, vagy a fürt méretezéssel), majd válasszon egy központi csomópont méretet legalább 8 magmintákat és 14 GB ram.
>
> Csomópont méretét és a kapcsolódó költségek a további tudnivalókért olvassa el a [HDInsight árak](https://azure.microsoft.com/pricing/details/hdinsight/)című témakört.

##<a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. A lépéseket a jelen dokumentum utolsó Azure CLI verzió 0.10.1 voltak ellenőrizni.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Jelentkezzen be az Azure előfizetés

Kövesse a [Microsoft Azure-előfizetésbe a az Azure parancssori kezelőfelületről Azure](../xplat-cli-connect.md) leírt lépéseket, és az előfizetés a __bejelentkezési__ módszerrel csatlakozzon.

##<a name="create-a-cluster"></a>Fürt létrehozása

Az alábbi lépéseket kell elvégezni a parancssor parancsot, rendszerhéj vagy munkamenet telepítése és konfigurálása az Azure CLI után.

1. A következő parancsot használja az Azure-előfizetéséhez hitelesítést végezni:

        azure login

    A név és jelszó megadására kéri. Ha több Azure előfizetéssel rendelkezik, `azure account set <subscriptionname>` beállítása az előfizetést, az Azure CLI parancsokat használhatja.

3. Váltás Azure erőforrás-kezelő módban használja a következő parancsot:

        azure config mode arm

4. Hozzon létre egy erőforrás csoportot. Az erőforráscsoport fogja tartalmazni a HDInsight fürt, és a kapcsolódó tárterület-fiókot.

        azure group create groupname location
        
    * __Csoportnév__ cserélje le egy egyedi nevet a csoportnak. 
    * Cserélje le a földrajzi területhez tartozik, hozza létre a csoportot a kívánt __helyre__ . 
    
        Érvényes helyek listáját, használja a `azure location list` parancsot, és írja be a __név__ oszlopban a helyek egyikére.

5. Tárterület-fiók létrehozása. Ez a tárterület-fiók a HDInsight fürt alapértelmezett tárolására lesz.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * __Csoportnév__ cserélje le a a csoport nevét, az előző lépésben létrehozott.
     * Cserélje ki az előző lépésben használt ugyanazon a helyen __helyét__ . 
     * __Storagename__ cserélje le egy egyedi nevet a tárolási fiókot.
     
     > [AZURE.NOTE] Ez a parancs használt paraméterek szeretne többet, `azure storage account create -h` Ez a parancs súgójának megtekintéséhez.

5. Lekérdezi a tárterület-fiók eléréséhez használt kulcs.

        azure storage account keys list -g groupname storagename
        
    * __Csoportnév__ cserélje ki az erőforrás-csoport nevét.
    * __Storagename__ cserélje le a tárterület-fiók nevére.
    
    A függvény által visszaadott adatok mentése __azonosító1__ __kulcs__ értékét.

6. Hozzon létre egy HDInsight fürthöz.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * __Csoportnév__ cserélje ki az erőforrás-csoport nevét.

    * Cserélje ki a létrehozni kívánt fürt típus __Hadoop__ . Ha például `Hadoop`, `HBase`, `Storm` vagy `Spark`.

        > [AZURE.IMPORTANT] HDInsight fürt fájltípusok, amelyek megfelelnek a terhelést vagy technológiát alkalmaz, amely a fürt van beállítva a különböző származnak. Nincs semmilyen módszerrel nem támogatott többféle, például vihar és egy fürt HBase kombináló fürt létrehozásához. 

    * Cserélje ki az előző lépéseket használt ugyanazon a helyen __helyét__ .

    * __Storagename__ cserélje le a tárterület-fiók nevére.

    * __StorageKey tulajdonságát__ cserélje le a kapott az előző lépésben billentyűt. 

    * Az a `--defaultStorageContainer` paraméter, a nevével megegyező, használja a fürt használható.

    * __Rendszergazdai__ és __httppassword__ cserélje a felhasználónevét és jelszavát, hogy a fürt keresztül HTTPS elérésekor használni kíván.

    * A felhasználónév és jelszó SSH használ a fürt elérésekor használni kívánt __sshuser__ és __sshuserpassword__ cseréje

    Eltarthat néhány percig, amíg a fürt létrehozási folyamat befejezéséhez. Általában körülbelül 15.

##<a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight fürthöz az Azure CLI használ, a következő segítségével megtudhatja, hogy miként dolgozhat a fürt:

###<a name="hadoop-clusters"></a>Hadoop fürt

* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase fürt

* [A HDInsight HBase – első lépések](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase az Java-alkalmazások fejlesztői számára](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Vihar fürt

* [Fejleszthet olyan Java topológiák vihar a hdinsight szolgáltatáshoz](hdinsight-storm-develop-java-topology.md)
* [A HDInsight vihar Python összetevők használata](hdinsight-storm-develop-python-topology.md)
* [Üzembe helyezéséhez és a HDInsight a vihar topológiák figyelése](hdinsight-storm-deploy-monitor-topology-linux.md)
