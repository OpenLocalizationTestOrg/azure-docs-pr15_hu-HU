<properties
    pageTitle="A HDInsight fürtök Hadoop elérhetősége |} Microsoft Azure"
    description="HDInsight-további központi csomópont erősen elérhető és megbízható fürt üzembe helyezése."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Elérhetőség és megbízhatóságára HDInsight a Windows-alapú Hadoop fürt


>[AZURE.NOTE] A lépéseket a dokumentumban használt Windows-alapú HDInsight fürt vonatkoznak. Linux-alapú fürt használatakor talál [elérhetőségének és a HDInsight Linux-alapú Hadoop fürt megbízhatóságának](hdinsight-high-availability-linux.md) Linux adatait.

HDInsight lehetővé teszi a felhasználóknak a fürt adattípusokra különböző analytics munkaterhelésekből az üzembe. Ma felajánlott fürt típusok Hadoop fürt lekérdezés és elemzési munkaterhelésekből, HBase fürt NoSQL munkaterhelésekből a és a valós idejű feldolgozás-esemény munkaterhelésekből vihar fürt. Belül egy adott fürt típus különféle szerepkör létezik a különböző csomópontok. Példa:



- A HDInsight Hadoop fürt két szerepkörök van telepítve:
    - Címsor csomópont (2 csomópontok)
    - Adatok csomópont (legalább 1 csomópont)

- A HDInsight HBase fürt telepítik három szerepkörök:
    - Címsor servers (2 csomópontok)
    - Régió servers (legalább 1 csomópont)
    - Főadat/Zookeeper csomópontok (3 csomópontok)

- A HDInsight vihar fürt telepítik három szerepkörök:
    - Nimbus csomópontok (2 csomópontok)
    - Felügyelő servers (legalább 1 csomópont)
    - Zookeeper csomópontok (3 csomópontok)

Szabványos példányait Hadoop fürt általában egy központi csomópontra rendelkeznek. HDInsight eltávolítja a hiba egyetlen pont egy másodlagos fő csomópont/HEAD hozzáadásával növelheti a rendelkezésre állásának és megbízhatóságára a szolgáltatás munkaterhelésekből kezeléséhez szükséges kiszolgáló/Nimbus csomópontot. A központi csomópontokat/vezetője kiszolgálók/Nimbus csomópontok célja, hogy a hibát dolgozó csomópontokat zökkenőmentesen kezelése, de a központi csomóponton futó fő szolgáltatások bármely kimaradások okozhat a fürt munkavégzés.


[ZooKeeper](http://zookeeper.apache.org/ ) csomópontokat (ZKs) felvett és központi csomópontokat vezető megválasztása segítségével, és annak biztosítására, hogy dolgozó csomópontokat és átjárók (GWs), hogy mikor kell átadni a másodlagos fő csomópont (vezetője Node1) Ha az aktív központi csomópont (vezetője Node0) inaktívvá válik.

![Diagram nagymértékben megbízható központi csomópontok HDInsight Hadoop végrehajtása.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Aktív fő csomópont szolgáltatás állapotának ellenőrzése
Annak megállapításához, hogy melyik fő csomópont aktív, és a központi csomóponton fut a szolgáltatások állapotának ellenőrzése, csatlakoznia kell a Hadoop fürt a távoli asztali Protocol (RDP) használatával. A RDP című cikkben olvashat [kezelése a Hadoop fürt a HDInsight az Azure portál használatával](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Ha csatlakozott a fürthöz, kattintson duplán a **Hadoop szolgáltatás elérhető** ikonra az asztalon, mely fő csomópont állapotát a Namenode juthat, Jobtracker, Templeton, Oozieservice, Metastore és Hiveserver2 szolgáltatások fut vagy HDI 3.0-s, a Namenode, erőforrás-kezelő, előzmények Server, Templeton, Oozieservice, Metastore és Hiveserver2 szolgáltatásokat.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

A képernyőképet az az aktív fő csomópont *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Access-naplófájlokat a másodlagos központi csomópontra

Feladat eléréséhez a másodlagos központi csomópontra naplók abban az esetben, ha vált, az aktív központi csomópontra, a felhasználói felület JobTracker böngészési továbbra is működik, ahogyan ezt az elsődleges aktív csomóponthoz. JobTracker eléréséhez csatolnia kell a Hadoop fürthöz RDP használatával, az előző szakaszban leírtak szerint. Ha befejezte a fürt be távolról, kattintson duplán a **Hadoop név csomópont állapot** ikonra az asztalon, és kattintson a **NameNode naplók** naplók a másodlagos központi csomópontra a címtárban eléréséhez a.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Fő csomópont méret beállítása
Alapértelmezés szerint a központi csomópontok nagy virtuális gépeken futó (VMs) szerint van rendelve. Ez a fürt futtassa a legtöbb Hadoop feladatok kezelésére vonatkozó megfelelő mérete. De forgatókönyv a központi csomópontok létre nagyon sok résztvevős VMs megkövetelheti. Egy példa, amikor a fürt kezelése a kisvállalati Oozie feladatok nagyszámú rendelkezik.

Nagyméretű VMs Azure PowerShell-parancsmagok vagy a HDInsight SDK beállíthatók.

A létrehozás és Azure PowerShell használatával fürt kiépítési ismertetett [HDInsight kezelése a PowerShell használatával](hdinsight-administer-use-powershell.md). Egy nagyméretű központi csomópont a konfiguráció szükséges hozzáadása a `-HeadNodeVMSize ExtraLarge` paramétert a `New-AzureRmHDInsightcluster` parancsmag használja ezt a kódot.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

A SDK hasonlít a a szövegegység. A létrehozás és a SDK használatával fürt kiépítési [HDInsight .NET SDK használatával](hdinsight-provision-clusters.md#sdk)ismertetését. A konfiguráció egy nagyméretű központi csomópont igényel hozzáadása a `HeadNodeSize = NodeVMSize.ExtraLarge` paramétere a `ClusterCreateParameters()` kód használt módszer.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Következő lépések

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Csatlakozás HDInsight fürt RDP segítségével](hdinsight-administer-use-management-portal.md#rdp)
- [HDInsight .NET SDK használatával](hdinsight-provision-clusters.md#sdk)
