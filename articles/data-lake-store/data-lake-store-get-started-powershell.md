<properties
   pageTitle="Tó adattár – első lépések |} Azure"
   description="Tó adattár fiók létrehozásához, és végezze el az alapműveletek a Azure PowerShell használatával"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Első lépések az Azure tó adattár Azure PowerShell használatával

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

Megtudhatja, hogy miként Azure PowerShell-lel való tó adattár Azure-fiók létrehozása, és olyan mappában, létrehozásakor fájlok feltöltése és letöltése adatok alapvető műveleteket, törölje a fiókot, stb. Tó adattár kapcsolatos további tudnivalókért olvassa el a [Áttekintése a tó adattár](data-lake-store-overview.md)című témakört.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

* **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell-s vagy újabb 1,0**. [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)tájékozódhat.

## <a name="authentication"></a>Hitelesítés

Ez a cikk a egyszerűbb hitelesítési megközelítés megkezdésére, ahol a Azure-fiók hitelesítő adatok tó adattár használja. A hozzáférési szint fiók és a fájlrendszerben majd tevékenységére a bejelentkezett felhasználó hozzáférési szintjének tó adattár. Vannak azonban olyan is, hogy a hitelesítő adatok tó áruházzal más módszer, hogy mely **végfelhasználói** vagy **szolgáltatás hitelesítés használatával**. Utasításokat és hitelesítést végezni kapcsolatos tudnivalók című témakörben talál [tó áruházzal hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Azure tó adattár fiók létrehozása

1. Az asztalon nyissa meg a Windows PowerShell új ablakban, és írja be a következő kódtöredékének jelentkezzen be az Azure-fiók, az előfizetés beállítása és az adatok tó tár szolgáltató regisztrálni. Amikor a rendszer kéri, jelentkezzen be a, ügyeljen az előfizetés admininistrators/tulajdonosa egyikeként jelentkezzen be:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Azure tó adattár fiók társítva az Azure erőforráscsoport. Az Azure erőforráscsoport létrehozásával kell kezdenie.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure erőforrás-ös csoport létrehozása] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure erőforrás-ös csoport létrehozása")

2. Azure tó adattár fiókot létrehozni. Megadott név csak kell tartalmaznia a kisbetűket és számokat.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure tó adattár fiók létrehozása] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure tó adattár fiók létrehozása")

3. Győződjön meg arról, hogy a fiókot sikeresen jön létre.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Kell, hogy a ennek eredménye **Igaz**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Directory szerkezetek létrehozása az Azure tó adattárhoz

Könyvtárak kezeléséhez és adattárolásra Azure tó adattár fiókhoz tartozó hozhat létre.

1. Adja meg a legfelső szintű könyvtárában.

        $myrootdir = "/"

2. Hozzon létre egy új **mynewdirectory** csoportban megadott legfelső szintű nevű könyvtárat.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Győződjön meg arról, hogy az új könyvtár sikeresen jön létre.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Meg kell jelennie olyan eredménye az alábbihoz hasonló:

    ![Ellenőrizze a címtár] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Ellenőrizze a címtár")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Feltölteni az adatokat az Azure tó adattárhoz

Az adatok feltöltheti tó adattár közvetlenül a legfelső szinten, vagy a számla belüli létrehozott könyvtárában. Az alábbi kódrészletek bemutatják, hogy miként tölthet fel mintaadatokat tartalmaz a címtárban (**mynewdirectory**), az előző részben létrehozott.

Ha tölthet fel mintaadatokat is tartalmazó keres, a az [Azure adatok tó mely számjegy tárházba](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)kattint a **Mentővel adatok** mappát. Töltse le a fájlt, és tárolja a számítógépen, például C:\sampledata helyi könyvtárában található\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Nevezze át, töltse le és adatok törlése az adatok tó áruházból

Fájl átnevezése, használja az alábbi parancsot:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Egy fájl letöltésére, használja az alábbi parancsot:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Ha törölni szeretne egy fájlt, használja az alábbi parancsot:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Amikor a rendszer kéri, adja meg az **Y** az elem törlése. Ha egynél több fájl törlése, vesszővel elválasztva minden elérési út lehet nyújtani.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>A tó adattár Azure-fiók törlése

A következő paranccsal a tó adattár fiók törlése.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Amikor a rendszer kéri, adja meg az **Y** a fiók törléséhez.


## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
