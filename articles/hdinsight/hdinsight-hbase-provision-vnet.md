<properties
    pageTitle="Hozzon létre egy virtuális hálózat HBase fürt |} Microsoft Azure"
    description="Az első lépések HBase használata Azure hdinsight szolgáltatásból lehetőségre. Megtudhatja, hogyan hozhat létre HDInsight HBase fürt Azure virtuális hálózaton."
    keywords=""
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
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>HBase fürt létrehozása az Azure virtuális hálózati 

Megtudhatja, hogy miként Azure hdinsight szolgáltatáshoz HBase fürt létrehozása az [Azure virtuális hálózati][1].

Virtuális hálózati integrációval HBase fürt telepíthető az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül a HBase is kommunikálhatnak. Előnyökkel jár a következők:

- A csomópontok a HBase fürt, amely lehetővé teszi, hogy a távoli eljárás HBase Java kommunikáció webalkalmazás közvetlen kapcsolat (RPC) API-khoz hívja.
- A forgalom nem kifejezi teljesítménynövekedés válassza a több átjáró és terheléselosztókkal.
- Az azt jelenti, hogy a bizalmas információkat folyamat további biztonságos módon anélkül, hogy egy nyilvános végpontot.

###<a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Azure PowerShell munkaállomás**. Lásd: [telepítése és Azure PowerShell használata](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Virtuális hálózatba HBase fürt létrehozása

Ebben a részben egy Azure virtuális hálózat egy [erőforrás-kezelő Azure-sablon](../resource-group-template-deploy.md)segítségével függő Azure tároló fiókkal Linux-alapú HBase fürt hoz létre. Más fürt létrehozási módszerek és bemutatása a beállításokat, témakörben [létrehozása HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md). HDInsight Hadoop fürt létrehozása sablon használatával kapcsolatos további tudnivalókért olvassa el a [Hadoop létrehozása fürt sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz a](hdinsight-hadoop-create-windows-clusters-arm-templates.md) című témakört.

> [AZURE.NOTE] Egyes tulajdonságai lett csomagolásukkor a sablonba. Példa:
>
> * __Hely__: Kelet-amerikai 2
> * __Cluster verzió: 3.4.
> * __Fürt dolgozó csomópont száma__: 4
> * __Alapértelmezett tároló fiók__: &lt;fürt neve > tárolása
> * __Virtuális nevet a hálózatnak__: &lt;fürt neve >-vnet
> * __Virtuális hálózati címterület__: 10.0.0.0/16
> * __Alhálózat neve__: alapértelmezett
> * __Alhálózat címtartományokat__: 10.0.0.0/24
>
> &lt;Fürt neve > cseréli a fürt nevét adja meg a sablon használata esetén.

1. Kattintson az alábbi képen a az Azure-portálon nyissa meg a sablont. A sablon egy nyilvános blob-tárolóban található. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Az **egyéni telepítési** lap az adja meg az alábbiakat:

    - **Előfizetés**: Válasszon egy a HDInsight fürt, a függő tárterület-fiók és az Azure virtuális hálózati létrehozásához használt Azure előfizetést.
    - **Erőforráscsoport**: jelölje be az **Új létrehozása**, majd adja meg a csoport egy új erőforrásnevet.
    - **Hely**: válassza ki az erőforráscsoport helyét.
    - **Fürtnév**: Adja meg a létrehozandó Hadoop fürt nevét.
    - **Fürt felhasználónév és jelszó**: az alapértelmezett bejelentkezési neve **felügyeleti**.
    - **SSH felhasználónév és jelszó**: az alapértelmezett felhasználónév szó **sshuser**.  Nevezze át. 
    - **A kifejezések és a fenti feltételek elfogadom**: (kiválasztása)
    

3. Kattintson a **vásárlás**gombra. Tart kapcsolatos körülbelül 20 perc fürt létrehozásához. Amikor létrejött a fürt, rákattintva a portálon nyissa meg a csoportját fel.

Miután az oktatóanyagot, érdemes lehet a fürt törléséhez. HDInsight az adatok Azure-tárolóban lévő tárolja, törölheti a fürtre biztonságosan, ha még nem használja. Is az előfizetést terhelő egy HDInsight fürthöz, akkor is, ha még nem használja. Mivel a fürt díjai sokszor több, mint a költségek tárolására, célszerű economic fürt törlése, ha nem használja. A fürt törlése című témakör nyújt tájékoztatást [kezelése a Hadoop fürt a HDInsight az Azure portál használatával](hdinsight-administer-use-management-portal.md#delete-clusters).

Az új HBase fürt használatának megkezdése előtt, használhatja a [HBase Hadoop HDInsight a az első lépések](hdinsight-hbase-tutorial-get-started.md)a található eljárásokat.

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Csatlakozzon a HBase fürthöz HBase Java RPC API-k használata

1.  Service (IaaS) virtuális gép infrastruktúrát létrehozása az Azure virtuális hálózatához és ugyanahhoz az alhálózathoz. Egy új IaaS virtuális gép létrehozásával kapcsolatos útmutatásért lásd: [Hozzon létre egy virtuális gépen futó Windows kiszolgálót](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Című témakör lépései a dokumentumban, amikor a hálózati konfigurálásról kell használnia a következőket:

    - __Virtuális hálózati__: &lt;fürt neve >-vnet
    - __Alhálózat__: alapértelmezett

    > [AZURE.IMPORTANT] Csere &lt;fürt neve > használta az előző lépésben a HDInsight fürt létrehozásakor nevű.

    Használja ezeket az értékeket konfigurálja a HDInsight fürt a virtuális hálózatához és használni a virtuális gépen. Ez lehetővé teszi őket közvetlenül egymással.

2.  Java-alkalmazások használatával csatlakozik a HBase távolról, kell használnia a teljes tartománynevét (FQDN). Ennek ellenőrzéséhez a kapcsolat-specifikus DNS-utótag a HBase fürt kell beszereznie. Amely akkor is használja az alábbi lehetőségek közül:

    * A webböngésző használatával Ambari kezdeményezése:
    
        Tallózással keresse meg azt a https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hosts?minimal_response = true. Kikapcsolja a JSON fájl az a DNS-mértékegységeket.

    * Válassza a Ambari webhelyet:

        1. Tallózással keresse meg azt a https://&lt;ClusterName >. azurehdinsight.net.
        2. A felső menüben kattintson a **Hosts** .

    * További hívásokat Curl használata:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        A JavaScript objektum jelölés (JSON) visszaadott adatok keresse a "gazdaszámítógép_neve" tételt. Ez a teljesen minősített tartománynév a csomópontok a fürt tartalmaz. Példa:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        A tartomány neve elején fürt nevű része a DNS-utótag. Ha például mycluster.b1.cloudapp.net.

    * Azure PowerShell használata
    
        Az alábbi Azure PowerShell parancsprogramot segítségével regisztrálhatja a **Get-ClusterDetail** függvény, amely a való visszatéréshez a DNS-utótag használható:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Után az Azure PowerShell parancsprogramot fut, használja a következő parancsot a DNS-utótag a **Get-ClusterDetail** függvénnyel való visszatéréshez. Adja meg a HDInsight HBase fürt neve, a rendszergazda nevét, és a rendszergazdai jelszavát, ha ezzel a paranccsal.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Ez a DNS-utótag vissza. Ha például **yourclustername.b4.internal.cloudapp.net**.

    * RDP használata
    
        Távoli asztali is használhatja a HBase fürt (fog is kapcsolatban kell a fő csomópont) való kapcsolódáshoz és **ipconfig** futtatása a parancssorból a DNS-utótag juthat. Engedélyezése a távoli asztali Protocol (RDP), és a fürthöz RDP, a kapcsolódó témaköröket olvassa el [a HDInsight az Azure portálon kezelése a Hadoop fürt][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Ha ellenőrizni szeretné, hogy a virtuális gép kommunikálni tudjanak a HBase fürt, paranccsal `ping headnode0.<dns suffix>` a virtuális számítógépről. Ha például a ping headnode0.mycluster.b1.cloudapp.net.

Java-alkalmazások használni ezt az információt, kövesse az [Maven tesztelése használja a HDInsight (Hadoop) HBase használó Java-alkalmazások létrehozásához](hdinsight-hbase-build-java-maven.md) alkalmazás létrehozása című témakör lépéseit. Ahhoz, hogy a távoli HBase kiszolgálóhoz való kapcsolódás alkalmazás, a **hbase-site.xml** fájl módosítását ebben a példában a teljesen minősített tartománynév használható Zookeeper. Példa:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Kapcsolatban további információk a névfeloldás az Azure virtuális hálózatok, beleértve a saját DNS-kiszolgáló használata [Name felbontás (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan hozhat létre egy HBase fürthöz. További tudnivalókért lásd:

- [Első lépések a hdinsight szolgáltatáshoz](hdinsight-hadoop-linux-tutorial-get-started.md)
- [A HDInsight HBase a replikáció konfigurálása](hdinsight-hbase-geo-replication.md)
- [Hozzon létre Hadoop fürt hdinsight szolgáltatáshoz](hdinsight-provision-clusters.md)
- [Elkezdeni használni a HDInsight Hadoop HBase](hdinsight-hbase-tutorial-get-started.md)
- [A HDInsight HBase a Twitteren sentiment elemzése](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Virtuális hálózati – áttekintés][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Az új HBase fürt rendelkezést részletei"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Parancsfájl műveletet használja egy HBase fürthöz testreszabása"

[azure-preview-portal]: https://portal.azure.com
