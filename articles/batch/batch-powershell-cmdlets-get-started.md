<properties
   pageTitle="Első lépések az Azure köteg PowerShell |} Microsoft Azure"
   description="Rövid ismertetés a Azure PowerShell-parancsmagok is használhatja az Azure köteg-szolgáltatás kezelése"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="10/20/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-powershell-cmdlets"></a>Első lépések az Azure köteg PowerShell-parancsmagok
Az Azure köteg PowerShell-parancsmagok hajtsa végre, és parancsfájl-sok feladatot azonos végrehajtja a köteg API-k, az Azure-portál és az Azure parancssor CLI (). Ez a rövid útmutatást talál az parancsmag használatával kezelheti a köteg fiókokat, és az a köteg erőforrások, például készletek, feladatok és feladatok kezelése.

Teljes listáját a köteg-parancsmagok és részletes parancsmag szintaxisát olvassa el a az [Azure köteg parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/mt125957.aspx)című témakört.

Ez a cikk az Azure PowerShell verzió 3.0.0 parancsmagok alapul. Azt javasoljuk, hogy az Azure PowerShell gyakran bővítve kihasználhatja az szolgáltatásfrissítések és továbbfejlesztett frissíti.

## <a name="prerequisites"></a>Előfeltételek

A köteg erőforrások Azure PowerShell használata az alábbi műveletek elvégzését.

* [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)

* Futtassa az előfizetés (az Azure köteg parancsmagok szállítási az Azure erőforrás-kezelő modul) csatlakozni a **Bejelentkezési-AzureRmAccount** parancsmagot:

    `Login-AzureRmAccount`

* **A köteg szolgáltató névtér regisztrálása**. Ez a művelet csak kell lennie az elvégzett **előfizetésenként egyszer**.

    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Köteg fiókok és a billentyűk kezelése

### <a name="create-a-batch-account"></a>Hozzon létre egy köteg fiókot

**Új-AzureRmBatchAccount** egy köteg-fiókból egy megadott erőforráscsoport hoz létre. Ha még nincs erőforráscsoport, hozzon létre egy újat az [Új-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) parancsmag futtatásával. Adja meg az Azure régiók közül a **hely** paraméter, például "USA-beli központi". Példa:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Ezután hozzon létre egy köteg fiók erőforrás csoportjában adja meg a fiók <*rendszer*> és helyét és nevét, az erőforrás-csoport nevét. A köteg fiók létrehozása befejezéséhez egy kis időt vehet igénybe. Példa:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [AZURE.NOTE] A köteg fióknév kell egyedi az erőforráscsoport Azure régió, között 3 és 24 karaktereket tartalmaznak, és használja a kisbetűket és számokat csak.

### <a name="get-account-access-keys"></a>Hívóbetűk fiók beszerzése
**Get-AzureRmBatchAccountKeys** a hívóbetűk Azure köteg fiókkal társított jeleníti meg. Ha például futtassa a következő, az elsődleges és másodlagos kulcsok létrehozott fiók eléréséhez.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Az access új kulcs generálása
**Új-AzureRmBatchAccountKey** kulcsot hoz létre egy új elsődleges és másodlagos fiókot az Azure köteg fiókkal. Új elsődleges kulcsot a köteg fiók létrehozásához írja be például a:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [AZURE.NOTE] Új másodlagos kulcs generálása, adja meg a "Másodlagos" **KeyType** paraméter. Ha az elsődleges és másodlagos kulcsok külön-külön újbóli.

### <a name="delete-a-batch-account"></a>A köteg fiók törlése
**Eltávolítás-AzureRmBatchAccount** töröl egy köteg fiókot. Példa:

    Remove-AzureRmBatchAccount -AccountName <account_name>

Amikor a rendszer kéri, erősítse meg a fiók eltávolítása. Fiók eltávolítása befejezéséhez egy kis időt vehet igénybe.

## <a name="create-a-batchaccountcontext-object"></a>BatchAccountContext-objektum létrehozása

Hitelesítést végezni, akkor létrehozhatja és kezelheti a köteg készletek, feladatok, feladatok és más erőforrások: a köteg PowerShell-parancsmagok használata, először a fiók nevét, és a billentyűk tárolása BatchAccountContext-objektum létrehozása:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

A BatchAccountContext objektum felkeresése parancsmagokról **BatchContext** paraméter használatával.

> [AZURE.NOTE] Alapértelmezés szerint a fiók elsődleges kulcs hitelesítéshez használt, de explicit módon választhatja ki a kulcs használatához az BatchAccountContext objektum **KeyInUse** tulajdonság módosításával: `$context.KeyInUse = "Secondary"`.

## <a name="create-and-modify-batch-resources"></a>Létrehozása és módosítása a köteg erőforrások
Például a **New-AzureBatchPool**, **Új-AzureBatchJob**és **Új-AzureBatchTask** parancsmagok használatával hozzon létre egy köteg fiókon erőforrásokat. Ott vannak megfelelő **Get -** és a **Set -** parancsmagok frissíteni a meglévő erőforrások tulajdonságai és **eltávolítása -** parancsmagok eltávolítása egy köteg fiókon erőforrásokat.

Sok átadása egy BatchContext objektum mellett parancsmagok használata esetén kell létrehozása vagy fázis, részletes erőforrás-beállításokat tartalmazó objektumok, az alábbi példában látható módon. A minden további példákat parancsmag részletes súgójában olvashatók.

### <a name="create-a-batch-pool"></a>A köteg készlet létrehozása

Létrehozásakor, vagy egy köteg erőforráskészlet frissítése, kijelöl egy felhőalapú szolgáltatás konfigurálása vagy egy virtuális gép konfigurációs az operációs rendszer, a számítási csomóponton (lásd: a [Köteg szolgáltatás áttekintése](batch-api-basics.md#pool)). A választási lehetőségek határozza meg, hogy a számítási csomópontok is telepíteni egy [Azure Vendég OS elengedi](../cloud-services/cloud-services-guestos-update-matrix.md#releases) vagy egy támogatott Linux vagy a Windows virtuális képek az Azure piactéren elérhető.

**Új-AzureBatchPool**futtatásakor adják át az operációs rendszer beállításainak PSCloudServiceConfiguration vagy PSVirtualMachineConfiguration objektum. Például a következő parancsmagot hoz létre az új köteget készlet méret kis számítási csomópontok a konfigurációban felhőalapú szolgáltatás, képes családi 3 (Windows Server 2012) operációs rendszer legfrissebb verziójához. Ebben az esetben a **CloudServiceConfiguration** paraméterrel a *$configuration* változó PSCloudServiceConfiguration objektumként. A **BatchContext** paraméter egy előzőleg definiált változó *$context* BatchAccountContext objektumként.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Az új készlet számítási csomópontok cél száma egy autoscaling képlet határozza meg. Ebben az esetben a képlet alapján számítható egyszerűen **$TargetDedicated = 4**, a készletben számítási csomópontok számát megadó legfeljebb 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Lekérdezés-készletek, feladatok, feladatokat és egyéb adatok

Lekérdezés például **Get-AzureBatchPool**, a **Get-AzureBatchJob**és a **Get-AzureBatchTask** -parancsmagok használata egy köteg fiókon létrehozott szervezetek.

### <a name="query-for-data"></a>Adatok lekérdezése

Példaként a **Get-AzureBatchPools** parancs segítségével megkeresheti a készletek. Alapértelmezés szerint ez a fiókját, feltéve hogy már minden készletek lekérdezések tárolja a BatchAccountContext objektum *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Az OData-szűrővel

Megadhat egy OData szűrő csak az Ön számára érdekes falakhoz objektum megkeresése **szűrő** paraméter használatával. Minden egyesített például "myPool" kezdetű azonosítójú is kereshet:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Ez a módszer nem áll, rugalmas, mint a "Where-objektum" egy helyi során. Jó helyen jár a lekérdezés elküldi, amint a köteg szolgáltatás közvetlenül, hogy az összes szűrő történik a kiszolgálóoldalon Internet-sávszélesség mentése.

### <a name="use-the-id-parameter"></a>Az azonosító paraméter használata

Az **azonosító** paraméter ahelyett, hogy egy OData-szűrő környezetbe. A lekérdezés egyedi azonosító "myPool" a készlet:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Az **azonosító** paraméter csak teljes-azonosító keresési, nem helyettesítő karaktereket vagy OData-stílus szűrők támogatja.

### <a name="use-the-maxcount-parameter"></a>MaxCount paraméter használatával

Alapértelmezés szerint minden parancsmag legfeljebb 1000 objektumok adja eredményül. Ha ezt a korlátot elér, finomítani a szűrőt ahhoz, hogy újra kevesebb objektumok, vagy explicit módon beállított legfeljebb **MaxCount** paraméter használatával. Példa:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

A felső határ eltávolításához beállítása **MaxCount** 0 vagy annál kisebb.

### <a name="use-the-powershell-pipeline"></a>A PowerShell folyamat használata

Köteg parancsmagok is kihasználhatja a PowerShell folyamat küldése adatok parancsmagok között. Paraméter megadása, ugyanazt az eredményt, de több entitás használata könnyebb.

Ha például keresse meg és a fiók csoportjában az összes feladat megjelenítése:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Indítsa újra (újraindítás) minden számítási csomópontjának erőforráskészlethez tartozik:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Alkalmazáskezelés csomag

A számítási csomópontok a készletek alkalmazások terjesztése egyszerűsített alkalmazáscsomagok kínál. A köteg PowerShell-parancsmagok az tölt fel és alkalmazáscsomagok kezelése a köteg fiókjában, és üzembe csomag verziók csomópontok számítja ki.

**Létrehozás** kérelmet:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Hozzáadás** az alkalmazás-csomagok:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Az **alapértelmezett verzióra** az alkalmazás beállítása:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Lista** -alkalmazáscsomagok

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Törölje** az alkalmazás-csomagok

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Törölje** az alkalmazás

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

>[AZURE.NOTE] Az alkalmazás törlése előtt kell törölje az összes az alkalmazások az alkalmazás csomag verziószáma. A "Ütközés" hiba értesítést kap, ha megpróbál, által jelzett alkalmazáscsomagok alkalmazás törlése.

### <a name="deploy-an-application-package"></a>Az alkalmazás-csomagok telepítése

Egy vagy több alkalmazáscsomagok telepítéshez is adja meg, ha a készlet létrehozása. Készlet létrehozása időben megadása egy csomag esetén telepítené minden csomópontot, a csomópont illesztések készletében. Csomagok is van telepítve, ha egy csomópont indítani, vagy reimaged.

Adja meg a `-ApplicationPackageReference` erőforráskészlethez tartozik egy alkalmazáscsomag a készlet csomópontok szeretne telepíteni, ahogy azok a csatlakozás a készlet létrehozása lehetőséget. Első lépésként hozzon létre egy **PSApplicationPackageReference** objektumot, és állítson be az alkalmazás azonosítója és csomag verziójával a készlet számítási csomópontok telepítéshez használni kívánt:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Most a készlet létrehozása, és adja meg a csomag hivatkozás objektum argumentumaként a `ApplicationPackageReferences` beállítást:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Alkalmazáscsomagok további információt az [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md)talál.

>[AZURE.IMPORTANT] A köteg fiókjába alkalmazáscsomagok használatához be kell [Azure tárterület-fiókkal hivatkozásra](#linked-storage-account-autostorage) .

### <a name="update-a-pools-application-packages"></a>Egy készlet alkalmazáscsomagok frissítése

Ha frissíteni szeretné az alkalmazások, kijelölt egy meglévő csoportba, először PSApplicationPackageReference objektum létrehozása a kívánt tulajdonságokkal (alkalmazás azonosítóját és csomag verziója):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Ezután a készlet beolvasása a köteget, bármelyik meglévő csomagok ürítse, az új csomag hivatkozás hozzáadása és frissítése a köteg szolgáltatás az új készlet beállításokkal:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

A köteg szolgáltatásban a készlet tulajdonságok most rendszer. Az új alkalmazáscsomag számítja ki a készlet található csomópontok ténylegesen üzembe helyezéséhez azonban kell újraindítása vagy azokat a csomópontokat reimage. Ezzel a paranccsal a készletben indítsa újra az összes csomópont:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

>[AZURE.TIP] A számítási csomópontokat egy készletben több alkalmazáscsomagok telepítheti. Ha meg szeretné *adni* az alkalmazáscsomag helyett a jelenleg telepített csomagok cseréje, hagyja a `$pool.ApplicationPackageReferences.Clear()` a felső sor.

## <a name="next-steps"></a>Következő lépések

* Részletes parancsmag szintaxis és példa című témakörben talál [Azure köteg parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Az alkalmazások és kötegben az alkalmazáscsomagok kapcsolatos további tudnivalókért lásd: [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md).
