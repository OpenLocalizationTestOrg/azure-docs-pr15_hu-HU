<properties 
   pageTitle="HBase replikáció között két adatközpontokkal beállítása |} Microsoft Azure" 
   description="Megtudhatja, hogy miként HBase replikáció konfigurálása a két adatközpontokban keresztül, és hogyan használati eset fürt replikáció." 
   services="hdinsight,virtual-network" 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>HDInsight HBase geo replikációs beállítása

> [AZURE.SELECTOR]
- [Virtuális Magánhálózati kapcsolatok konfigurálása](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [A DNS konfigurálása](hdinsight-hbase-geo-replication-configure-dns.md)
- [HBase replikáció konfigurálása](hdinsight-hbase-geo-replication.md) 
 
Megtudhatja, hogy miként HBase replikáció konfigurálása két adatközpontokban keresztül. Néhány használatával esetek fürt replikációs tartalmazza:

- Biztonsági mentési és katasztrófa helyreállítás
- Adatok összesítése
- Földrajzi adatokat ki.
- Kapcsolat nélküli adatok analytics kombinálni online adatok bevitel

Fürt replikációs egy adatforrás-leküldéses módszertan használja. Egy HBase fürthöz forrást, a cél vagy teljesíteni tudja egyszerre mindkét szerepkörhöz. A replikáció aszinkron, és a cél replikációs esetleges konzisztencia. Amikor a forrás való replikáció engedélyezett kap a szerkesztési egy oszlopot az operációs rendszere, minden cél fürt propagálja, hogy a szerkesztési. Adatok között van replikált eltávolítása egy, a forrás fürt és minden fürt, amely már rendelkezik felhasznált adatok való replikáció hurkok megakadályozása nyomon követése. További információ ebben az oktatóanyagban fog beállíthatja a forrás replikáció.  Más fürt topológiák útmutatójában [Apache HBase hivatkozást](http://hbase.apache.org/book.html#_cluster_replication).

Ez a harmadik rész az adatsor:

- [Virtuális hálózatok között egy virtuális Magánhálózati kapcsolatok konfigurálása][hdinsight-hbase-replication-vnet]
- [A virtuális hálózatok DNS konfigurálása][hdinsight-hbase-replication-dns]
- HBase geo replikáció (ebben az oktatóanyagban) konfigurálása

Az alábbi ábra mutatja be, a két virtuális hálózat és a hálózati kapcsolat [beállítása a virtuális Magánhálózati kapcsolat két virtuális hálózatok között] a létrehozott[ hdinsight-hbase-geo-replication-vnet] és [DNS konfigurálása a virtuális hálózathoz][hdinsight-hbase-replication-dns]: 

![HDInsight HBase replikációs virtuális hálózatdiagram][img-vnet-diagram]

## <a id="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Azure PowerShell munkaállomás**.

    Végrehajtása a PowerShell-parancsfájlokat, Azure PowerShell Futtatás rendszergazdaként, és állítsa az adatvégrehajtás házirendet *RemoteSigned*. Lásd: a Set-végrehajtási házirend-parancsmag használata.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Két Azure virtuális hálózati virtuális Magánhálózati kapcsolatot, és a DNS beállítva**.  Útmutatásért lásd: [a virtuális Magánhálózati kapcsolat két Azure virtuális hálózatok között konfigurálása][hdinsight-hbase-replication-vnet], és a [DNS konfigurálása Azure virtuális hálózatok között][hdinsight-hbase-replication-dns].


    PowerShell-parancsfájlokat futtatásához győződjön meg arról, hogy csatlakozva van az Azure előfizetés az alábbi parancsmag használatával:

        Add-AzureAccount

    Ha több Azure előfizetéssel rendelkezik, használja a következő parancsmagot az aktuális előfizetés beállítása:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>A HDInsight HBase fürt kiépítése

[Azure virtuális hálózatok között a virtuális Magánhálózati kapcsolat beállítása]a[hdinsight-hbase-replication-vnet], létrehozott egy virtuális hálózati egy Europe adatközpont, és egy USA-beli adatok központban virtuális hálózat. A két virtuális hálózat VPN keresztül csatlakozik. Ebben a munkamenetben, egy HBase fürthöz mindegyik virtuális hálózathoz fog rendelkezni. Ebben az oktatóanyagban belül teheti a HBase fürt való replikáció a HBase fürt közül.

Az Azure klasszikus portálon nem támogatja az egyéni beállítások kiépítési HDInsight fürt. Ha például *hbase.replication* *true*értékűre. Ha az érték a konfigurációs fájl után fürt már kiépítve, el fog veszni a beállítás után a fürt van folyamatban reimaged. További információt a témakörben [a HDInsight rendelkezés Hadoop fürt][hdinsight-provision]. Azure PowerShell használ egyéni beállításokkal HDInsight fürt kiépítése a lehetőségek közül.


**A Contoso-VNet-Európai Unió egy HBase fürthöz kiépítése** 

1. A munkaállomásról nyissa meg a Windows PowerShell ISE.
2. Adja hozzá a változót a parancsfájl elején, és futtassa a parancsfájl.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**A Contoso-VNet – hu-HBase fürt kiépítése** 

- Az azonos parancsfájl használata az alábbi értékeket:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Mivel már csatlakozott az Azure-fiókjába, nem kell többé futtassa a következő comlets:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Állítsa be a DNS-feltételes továbbító

A [DNS konfigurálása a virtuális hálózathoz][hdinsight-hbase-replication-dns], beállította a DNS-kiszolgálóiról az két hálózathoz. A HBase fürt utótag másik tartományt kell. Úgy kell konfigurálni további feltételes továbbítókat.

Feltételes továbbító konfigurálásához kell tudnia a két HBase fürt, a tartomány mértékegységeket. 

**A tartomány mértékegységeket, a két HBase fürt keresése**

1. A **Contoso-HBase-Európai Unió**RDP.  Című cikkben olvashat [a HDInsight az Azure klasszikus portál használatával kezelése a Hadoop fürt][hdinsight-manage-portal]. A fürt ténylegesen headnode0.
2. Nyissa meg a Windows PowerShell-konzol vagy a parancssor parancsot.
3. Futtassa az **ipconfig**, és jegyezze fel a **kapcsolatot-specifikus DNS-utótag**.
4. Kérjük, ne zárja be az RDP-munkamenet.  Szüksége lesz, hogy később tartomány névfeloldás tesztelése.
5. Ismételje meg ezeket a lépéseket megtudhatja, hogy a **Contoso-HBase-hu**a **kapcsolat-specifikus DNS-utótag** .


**DNS-továbbítókat konfigurálása**
 
1.  A **Contoso-DNS-Európai Unió**RDP. 
2.  Kattintson a Windows-billentyűt, a bal alsó sarokban.
2.  Kattintson a **Felügyeleti eszközök**.
3.  Kattintson a **DNS**elemre.
4.  A bal oldali ablaktáblában bontsa ki a **DSN**, **Contoso-DNS-Európai Unió**.
5.  Kattintson a jobb gombbal a **Feltételes továbbítókat**, és kattintson az **Új feltételes továbbító**. 
5.  Írja be az alábbi adatokat:
    - **DNS-tartomány**: Adja meg a DNS-utótag a Contoso-HBase-amerikai. Példa: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **A fő kiszolgálók IP-címe**: Adja meg a 10.2.0.4, amely a Contoso-DNS-hu 's IP-címe. Ellenőrizze, hogy a IP. A DNS-kiszolgáló beállíthatja, hogy egy másik IP-címet.
6.  Nyomja le az **ENTER BILLENTYŰT**, és kattintson **az OK**gombra.  Most már lesz a Contoso-DNS-hu 's IP-címet a Contoso-DNS-Európai Unió megoldható.
7.  Ismételje meg a DNS-feltételes továbbító hozzáadása a DNS-szolgáltatás az alábbi értékeket a Contoso-DNS-hu virtuális gépen:
    - **DNS-tartomány**: Adja meg a DNS-utótag Contoso-HBase-Unió. 
    - **A fő kiszolgálók IP-címe**: Adja meg a 10.2.0.4, amely a Contoso-DNS-Európai Unió 's IP-címe.

**Tartomány névfeloldás tesztelése**

1. Váltás a Contoso-HBase-Európai Unió RDP-ablakba.
2. Nyisson meg egy parancssort.
3. A ping parancsot:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    A ICM protokoll be van kapcsolva a HBase fürt dolgozó csomópontjai

4. Ne zárja be az RDP-munkamenet. Továbbra is szüksége lesz rá az oktatóprogram belül.
5. Ismételje meg ezeket a lépéseket a Contoso-HBase-Amerikai Egyesült Államok a Contoso-HBase-Unió headnode0 pingelése.

>[AZURE.IMPORTANT] DNS kell dolgozniuk, a folytatás előtt a következő lépésekkel.

## <a name="enable-replication-between-hbase-tables"></a>Engedélyezze a replikáció HBase tábla között

Most hozzon létre egy minta HBase táblát, replikáció engedélyezése, és tesztelje az adatokat. A mintatáblázat használni kívánt van két oszlop család: személyes és az Office. 

Ebben az oktatóanyagban láthatóvá válik a Europe HBase fürt, valamint a forrás fürt, az Amerikai Egyesült Államok HBase fürt a cél fürt.

HBase táblázatok létrehozása a ugyanaz a neve és a forrás- és fürt a oszlop családok, hogy a cél fürt tudja, hogy hol tárolja az adatokat fog fogadni. A HBase rendszerhéj használatával kapcsolatos további tudnivalókért lásd: a [Apache HBase HDInsight az első lépések][hdinsight-hbase-get-started].

**A Contoso-HBase-Európai Unió egy HBase táblázat létrehozásához**

1. Váltás a **Contoso-HBase-Európai Unió** RDP-ablakba.
2. Az asztalról **Hadoop parancssor**parancsára.
2. Váltson a mappa HBase otthoni:

        cd %HBASE_HOME%\bin
3. Nyissa meg a HBase rendszerhéj:

        hbase shell
4. Hozzon létre egy HBase táblázatot:

        create 'Contacts', 'Personal', 'Office'
5. Ne zárja be az RDP-munkamenet sem a Hadoop parancssorablakban. Továbbra is szüksége lesz rájuk belül az oktatóprogram.
    
**A Contoso-HBase-hu-HBase táblázat létrehozásához**

- Ismételje meg ezeket a lépéseket a táblázatból létrehozni a Contoso-HBase-hu.


**A replikáció peer Contoso-HBase-hu hozzáadni kívánt**

1. Váltás a **Contso-HBase_EU** RDP-ablakba.
2. A HBase rendszerhéj ablakban adja hozzá a cél fürt (Contoso-HBase-hu) a peer, mint például:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    A mintában a tartomány képz_jel *contoso-hbase-us.d4.internal.cloudapp.net*. Frissítenie kell, hogy a tartomány utótag a Kapcsolatfelvételi HBase fürt megfelelően. Győződjön meg arról, hogy ne legyen szóköz a állomásnevekké között.

**A forrás fürt replikált kell minden oszlop családtagok konfigurálása**

1. Az **Contso-HBase-Európai Unió** RDP-munkamenet HBase rendszerhéj ablakban minden oszlop családtagok kell replikált beállítása:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Tömeges feltöltés adatok a HBase táblázatra**

Adatok mintafájl nyilvános Azure Blob-tárolóhoz az alábbi URL-címmel feltöltötte:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

A fájl tartalma:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Adatok ugyanannak a fájlnak a HBase fürt feltölteni, és az adatok importálása a tárból.

1. Váltás a **Contoso-HBase-Európai Unió** RDP-ablakba.
2. Az asztalról **Hadoop parancssor**parancsára.
3. Váltson a mappa HBase otthoni:

        cd %HBASE_HOME%\bin

4. Töltse fel az adatokat:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Ellenőrizze, hogy adatokat a replikáció folyik

Ellenőrizheti, hogy a replikáció folyik által végzett vizsgálatot a következő HBase rendszerhéj parancsokkal mindkét fürt tábláinak:

        Scan 'Contacts'


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta van HBase replikációs konfigurálásáról két adatközpontokkal keresztül. További információ a HDInsight és HBase című témakörben talál:

- [HDInsight szolgáltatás lapon](https://azure.microsoft.com/services/hdinsight/)
- [Dokumentáció hdinsight szolgáltatáshoz](https://azure.microsoft.com/documentation/services/hdinsight/)
- [A HDInsight Apache HBase – első lépések][hdinsight-hbase-get-started]
- [HDInsight HBase – áttekintés][hdinsight-hbase-overview]
- [Azure virtuális hálózati HBase fürt kiépítése][hdinsight-hbase-provision-vnet]
- [Valós idejű Twitter sentiment HBase és elemzése][hdinsight-hbase-twitter-sentiment]
- [Vihar és a HDInsight (Hadoop) HBase érzékelő adatok elemzése][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
