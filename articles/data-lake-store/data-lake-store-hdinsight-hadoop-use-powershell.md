<properties
   pageTitle="HDInsight fürt létrehozása az Azure tó adattár PowerShell használatával |} Azure"
   description="Azure PowerShell-lel való létrehozása és Azure adatok tó HDInsight fürt használata"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Hozzon létre egy HDInsight fürthöz Azure PowerShell használatával tó adattárhoz

> [AZURE.SELECTOR]
- [Portál használatával](data-lake-store-hdinsight-hadoop-use-portal.md)
- [A PowerShell használatával](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Erőforrás-kezelő használatával](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Megtudhatja, hogy miként Azure PowerShell-lel való beállítása egy HDInsight fürthöz (Hadoop, HBase vagy vihar) Azure tó adattár hozzáféréssel rendelkező. Ebben a kiadásban néhány fontos dolgot:

* **A külső fürt (Linux), és a Hadoop/vihar fürt (Windows és Linux)**, az adatok tó áruházból csak akkor használható, egy további tárterület-fiók. Az ilyen fürt alapértelmezett tároló fiókja továbbra is Azure tároló BLOB (WASB).

* Alapértelmezett tároló vagy további tárterület **-HBase fürt (Windows és Linux)**, az adatok tó áruházból használható.

> [AZURE.NOTE] Néhány fontos tudnivaló jegyezze fel. 
> 
> * HDInsight fürt létrehozásának tó adattár hozzáféréssel rendelkező csak 3,2 és 3.4 (a fürt Hadoop HBase és vihar Windows, valamint a Linux) HDInsight verzióiban érhető el. A külső fürt Linux ezt a beállítást megtalálható csak a HDInsight 3.4 fürt.
>
> * Fent említett tó adattár érhető el (Hadoop, külső, vihar) fürtre másfajta további tárterületet, valamint alapértelmezett tárterület néhány fürtre típusa (HBase). Adatok tó tárolót használó további tárterületet fiókként nincs hatással és az azt jelenti, hogy írható/olvasható a tárolóhoz eltávolítása a működésére. Abban az esetben, ahol tó adattár további tárterületet szolgál fürt kapcsolatos (például a naplókat, stb.) írja a fájlt, az alapértelmezett tároló (Azure BLOB), miközben feldolgozása, amelyet az adatok tárolható egy tó adattár fiókot.


Ebben a cikkben további tárterületet, hogy kiépítése tó áruházzal Hadoop fürt.

HDInsight tó adattár végezhető konfigurálása PowerShell használatával a következő lépésekből:

* Azure adatok tó tároló létrehozása
* Szerepköralapú hozzáférés-adatok tó áruház hitelesítés beállítása
* Hozzon létre HDInsight fürt tó adattár-hitelesítés
* A próba-feladat futtatása a fürt

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell-s vagy újabb 1,0**. [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)tájékozódhat.

- **Windows SDK csomagjában talál**. Az [alábbi](https://dev.windows.com/en-us/downloads)telepítheti. Ennek használatával hozhat létre olyan biztonsági tanúsítványt.

- **Azure Active Directory-szolgáltatás egyszerű**. Az oktatóprogram lépéseit a a szolgáltatás rendszerbiztonsági tag létrehozása az Azure AD útmutatást. Azonban engedélyezni szeretné, hogy hozzon létre egy egyszerű Azure Active Directory rendszergazdának kell lennie. Ha Ön az Azure Active Directory-rendszergazda, ugorja át a előfeltétel, és folytassa az oktatóprogram.
    
    **Ha Ön nem egy Azure AD-rendszergazda**, nem tudja végrehajtani a hozzon létre egy egyszerű lépéseket. Ebben az esetben az Azure Active Directory-rendszergazda először létre kell hoznia egy egyszerű egy HDInsight fürthöz tó áruházzal létrehozása előtt. Is a szolgáltatás egyszerű segítségével kell létrehozni egy tanúsítványt, a [fő tanúsítvány szolgáltatás hozzon létre](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate)leírt módon. 


## <a name="create-an-azure-data-lake-store"></a>Azure adatok tó tároló létrehozása

Kövesse ezeket a lépéseket követve hozzon létre egy adattár tó.

1. Az asztalon nyisson meg egy új Azure PowerShell-ablakot, és írja be a következő kódtöredékének. Amikor a rendszer kéri, jelentkezzen be a, ügyeljen az előfizetés admininistrators/tulajdonosa egyikeként jelentkezzen be:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Ha egy hasonló hibaüzenet jelenik meg `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` a tó adattár erőforrás szolgáltató regisztrálásakor akkor lehetséges, hogy a subsrcription nem jelenik meg az Azure tó adattár whitelisted. Ellenőrizze, hogy engedélyezi az Azure előfizetés előzetes verzió tó adattár nyilvános ezeket az [utasításokat](data-lake-store-get-started-portal.md#signup)követve.

3. Azure tó adattár fiók társítva az Azure erőforráscsoport. Az Azure erőforráscsoport létrehozásával kell kezdenie.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure erőforrás-ös csoport létrehozása] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Azure erőforrás-ös csoport létrehozása")

2. Azure tó adattár fiókot létrehozni. A fiók nevét adja meg, hogy csak kell tartalmaznia a kisbetűket és számokat.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure adatok tó fiók létrehozása] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Azure adatok tó fiók létrehozása")

3. Győződjön meg arról, hogy a fiókot sikeresen jön létre.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Kell, hogy a ennek eredménye **Igaz**.

4. Töltse fel mintaadatokat is tartalmazó Azure adatok tó. Használjuk Ez a cikk későbbi részében ellenőrizze, hogy az adatokat egy HDInsight fürthöz-ból elérheti. Ha tölthet fel mintaadatokat is tartalmazó keres, a az [Azure adatok tó mely számjegy tárházba](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)kattint a **Mentővel adatok** mappát.


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Szerepköralapú hozzáférés-adatok tó áruház hitelesítés beállítása

Minden Azure előfizetéshez társítva az Azure Active Directory. Felhasználók és az előfizetés az Azure klasszikus portálja vagy az Azure erőforrás-kezelő API segítségével forrásokat szolgáltatások kell először az adott Azure Active Directory hitelesítő. Hozzáférés az Azure előfizetések és szolgáltatásokhoz a megfelelő szerepkört egy Azure erőforráson társításával.  A szolgáltatás egyszerű szolgáltatások, a szolgáltatást az Azure Active Directory (AAD) a azonosítja. Ez a szakasz szemlélteti, hogyan kell adni egy alkalmazás szolgáltatás, például a HDInsight, az access az Azure-erőforrás (a korábban létrehozott Azure tó adattár fiók) hoz létre egy egyszerű alkalmazásához és szerepkörök, amely Azure PowerShell keresztül.

Active Directory-hitelesítést az Azure adatok tó beállításához hajtsa végre az alábbi műveleteket.

* Önaláírt tanúsítvány létrehozása
* Az alkalmazások létrehozása az Azure Active Directory és a szolgáltatás egyszerű

### <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása

Győződjön meg arról, hogy [Windows SDK](https://dev.windows.com/en-us/downloads) telepítve van a lépéseket, ebben a szakaszban a folytatás előtt. Akkor is létre kell könyvtárában, például **C:\mycertdir**, ahol a tanúsítvány létrejön.

1. A PowerShell ablakban nyissa meg azt a helyet, amelyen telepítette a Windows SDK (általában `C:\Program Files (x86)\Windows Kits\10\bin\x86` , és használja a [MakeCert] [ makecert] segédprogramot önaláírt tanúsítvány és a titkos kulcs létrehozásához. A következő parancsokat használhatja.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    A titkos kulcs jelszó megadását kéri. Miután sikeresen végrehajtja a parancsot, meg kell jelennie egy **CertFile.cer** és **mykey.pvk** a megadott tanúsítvány címtárban.

4. Használja a [Pvk2Pfx] [ pvk2pfx] MakeCert létrehozott .pvk és .cer-fájlok konvertálása .pfx fájl segédprogramot. A következő parancsot.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Amikor a rendszer kéri adja meg a korábban megadott titkos kulcs jelszót. A **megrendelés -** paraméterhez megadott érték van társítva a .pfx fájl jelszót. A parancsot sikerült befejeződése után egy CertFile.pfx a megadott tanúsítvány címtárban is meg kell jelennie.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Azure Active Directory és a szolgáltatás rendszerbiztonsági tag létrehozása

Ebben a részben hajtsa végre a lépéseket követve szolgáltatás fő az Azure Active Directory-alkalmazás létrehozása, a szerepkör hozzárendelése a legfontosabb szolgáltatás, és a hitelesítés, a szolgáltatás egyszerű, azáltal, hogy egy tanúsítványt. A következő parancsokat az Azure Active Directory-alkalmazás létrehozása.

1. A következő parancsmagok illessze be a PowerShell konzol ablakában. Ellenőrizze, hogy egyedi **- DisplayName** tulajdonság megadott értékkel. Az értékek **– Kezdőlap** és **- IdentiferUris** is, helyőrző értékeket, és nem ellenőrzi.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Hozzon létre egy egyszerű használja az alkalmazás azonosítója.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Hozzáférést a szolgáltatás fő a tó adattár fájl/mappa eltávolítása a HDInsight hozzáférő. Az alábbi kódtöredékének tó adattár fiók legfelső szintű hozzáférést biztosít.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    A parancssorba írja be az **Y** megerősítéséhez.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Hozzon létre egy HDInsight fürthöz tó adattár-hitelesítés

Ebben a részben hozzunk létre egy HDInsight Hadoop fürtre. Ebben a kiadásban a HDInsight fürt és az adatok tó áruházból ugyanazon a helyen (US kelet-2) kell lennie.

1. Kiindulás lekérdezése az előfizetés bérlői azonosítójával. Szüksége lesz, hogy később.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Ebben a kiadásban a Hadoop fürt tó adattár csak akkor használható, egy további tárterületet, de a fürt. Az alapértelmezett tároló továbbra is a tárhely Azure BLOB (WASB). Igen azt először létrehoznia a tárhely és a tároló a fürt szükséges.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. A HDInsight fürt létrehozása. A következő parancsmagok használata.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    A parancsmaggal sikeresen befejeződése után meg kell jelennie egy kimenet jelennek meg:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>A HDInsight fürtre az adatok tó áruház próba feladat futtatható

Után egy HDInsight fürthöz van beállítva, futtathatja a próba-feladatok a fürt tesztelése, hogy a HDInsight fürt hozzáférhet tó adattár. Kéri, hogy futtathatók egy minta struktúra feladat, amely a korábban a adattár tó feltöltött mintaadatokat tartalmazó táblázatot hoz létre.

### <a name="for-a-linux-cluster"></a>Linux fürt

Ebben a részben lehetővé teszi a SSH fürtre, és a Futtatás a példa struktúra lekérdezésre. A Windows nem nyújt a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Miután létrejött, először a struktúra CLI a következő parancsot:

        hive

2. A CLI használ, írja be a következő kimutatások **járműveket** nevű a mintaadatokat az adatok tó áruházban használatával új tábla létrehozása:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Meg kell jelennie a kibocsátás, az alábbihoz hasonló:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>A Windows fürthöz

Az alábbi parancsmag használatával a struktúra lekérdezés futtatása. Ebben a lekérdezésben hogy hozzon létre egy táblázatot az adatok tó áruházban az adatokat, és futtassa a létrehozott táblát a választó lekérdezés.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Ez a következő kimenet lesz. **ExitValue** kimeneti 0 felajánlja a feladat sikeresen befejeződött.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

A kimenet beolvashatja a feladat használatával az alábbi parancsmagot:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

A feladat eredménye az alábbihoz hasonló:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Az Access tó adattár Fájlrendszerhez parancsok használatával

A HDInsight fürtre adatok tó tároló használatára van beállítva, miután használhatja a Fájlrendszerhez rendszerhéj-parancsok az üzlet eléréséhez.

### <a name="for-a-linux-cluster"></a>Linux fürt

Ez a szakasz, SSH lesz a fürt be, és a Fájlrendszerhez parancsai futtathatók. A Windows nem nyújt a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Miután létrejött, a következő paranccsal Fájlrendszerhez formázáshoz a fájlokat az adatok tó áruházban listáját.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Megjelenik a fájl korábbi feltöltött az adatok tó áruházból.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Is használhatja a `hdfs dfs -put` parancs, amellyel az egyes fájlok feltöltése a adatok tó áruházba, és írja be `hdfs dfs -ls` ellenőrizze, hogy a fájlok feltöltése sikeresen.


### <a name="for-a-windows-cluster"></a>A Windows fürthöz

1. Jelentkezzen be az új [Azure-portálon](https://portal.azure.com).

2. Kattintson a **Tallózás gombra**, **HDInsight fürt**gombra, és válassza az Ön által létrehozott HDInsight fürt.

3. A fürt lap a **Távoli asztal**gombra, és a **Távoli asztali** lap a **Csatlakozás**gombra.

    ![Távoli HDI fürt be] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Azure erőforrás-ös csoport létrehozása")

    Amikor a rendszer kéri, adja meg a távoli asztali felhasználó megadott hitelesítő adatait.

4. Indítsa el a Windows PowerShell a távoli munkamenetet, és formázáshoz Fájlrendszerhez parancsaival az Azure adattár tó a fájlok listája.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Megjelenik a fájl korábbi feltöltött az adatok tó áruházból.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Is használhatja a `hdfs dfs -put` parancs, amellyel néhány fájlokat feltölteni az adatok tó áruházból, és írja be `hdfs dfs -ls` ellenőrizze, hogy a fájlok feltöltése sikeresen.

## <a name="see-also"></a>Lásd még:

* [Portálon: Hozzon létre egy HDInsight fürthöz adatok tó áruház](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
