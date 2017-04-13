<properties
    pageTitle="Egyszerű alkalmazás telepítése és kezelése az Azure köteg |} Microsoft Azure"
    description="Használja az alkalmazás csomagok szolgáltatás Azure köteg egyszerűen kezelheti az alkalmazások és a verzió telepített példányának a köteg csomópontok számítja ki."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="application-deployment-with-azure-batch-application-packages"></a>Alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok

Az alkalmazás csomagok Azure köteg szolgáltatása könnyű kezelés tevékenység-alkalmazások és a készlet számítási csomópontjának a telepítést. Az alkalmazáscsomagok töltse fel, és az alkalmazások, a feladatok futtatni, beleértve a segédfájlok több verzióinak kezelése. Ezután automatikusan telepítheti egy vagy több ezeket az alkalmazásokat a számítási csomópontok a készletben.

Ebben a cikkben megtanulhatja, hogyan tölthet fel, és kezelheti a alkalmazáscsomagok az Azure-portálon. Ezután megtanulhatja, hogy miként telepítheti őket egy készlet számítási csomóponton és a [Köteg .NET] [ api_net] tárat.

> [AZURE.NOTE] Az itt leírt alkalmazás csomagok szolgáltatás levő adatbázisban felülírja a "Köteg alkalmazások" funkció elérhető a szolgáltatás korábbi verzióiban.

## <a name="application-package-requirements"></a>Alkalmazás csomag vonatkozó követelmények

A köteg fiókjába alkalmazáscsomagok használatához be kell [Azure tárterület-fiókkal hivatkozásra](#link-a-storage-account) .

Az alkalmazás csomagok a jelen cikkben tárgyalt funkció kompatibilis *csak* a köteg készletek 10 március 2016 után létrehozott. A dátum előtt létrehozott készletek található csomópontok kiszámítására az alkalmazáscsomagok nem lehet telepíteni.

Ez a funkció a [Köteg REST API] jelent meg[ api_rest] 2015-12-01.2.2 verzió és a megfelelő [Köteg .NET] [ api_net] tár verzió 3.1.0. Azt javasoljuk, hogy mindig a legújabb API történő használt köteg használata.

> [AZURE.IMPORTANT] Jelenleg csak *CloudServiceConfiguration* készletek támogatja az alkalmazáscsomagok. VirtualMachineConfiguration képek használatával létrehozott készletek alkalmazáscsomagok nem használhatók. Olvassa el a [virtuális gép konfigurációs](batch-linux-nodes.md#virtual-machine-configuration) szakaszát [rendelkezést Linux Azure köteg készletek található csomópontok számítja ki](batch-linux-nodes.md) további információt a két különböző beállításokat.

## <a name="about-applications-and-application-packages"></a>Alkalmazások és az alkalmazás csomagok

A készletben számítási csomópontok automatikusan letölti verziószámmal bináris halmazának belül Azure köteget, az *alkalmazás* hivatkozik. Egy *Alkalmazáscsomag* ezeket a bináris *adott* hivatkozik, és egy adott *verzió* , az alkalmazás jelöli.

![Az alkalmazások és az alkalmazáscsomagok magas szintű diagram][1]

### <a name="applications"></a>Alkalmazások

Egy köteg alkalmazást tartalmaz egy vagy több alkalmazás csomagolja, és adja meg a beállítások alkalmazásához. Az alkalmazások megadhatja például, az alapértelmezett alkalmazás csomag verzió számítási csomópontok és a e frissített vagy törlése a saját csomagok telepítése.

### <a name="application-packages"></a>Alkalmazáscsomagok

Az alkalmazás-csomagok az alkalmazás bináris tartalmazó .zip fájlok és a feladatok végrehajtásához szükséges segédfájlok. Minden egyes alkalmazáscsomag az alkalmazás adott verzióját jeleníti meg.

Megadhatja, hogy a készlet és a tevékenység szintjén alkalmazáscsomagok. Egy vagy több ezekről a csomagokról, és olyan verziójú (nem kötelező) egy készlet vagy a tevékenység létrehozásakor is megadhat.

* **Készlet alkalmazáscsomagok** telepítik *minden* készletben csomópontra. Alkalmazások telepítik egy csomópont csatlakozik egy készlet, valamint esetén indítani, vagy reimaged.

    Készlet alkalmazáscsomagok megfelelőek, amikor egy készletben csomópontjait, hajtsa végre a projektfeladatok. Egy vagy több alkalmazáscsomagok készletet hoz létre, és felvehet és módosítása meglévő készlet csomagok is megadhat. Ha egy meglévő készlet alkalmazáscsomagok frissít, újra kell indítani a csomópontok az új csomag telepítését.

* **Tevékenység alkalmazáscsomagok** csak egy számítási csomópontot, csak a parancssorból a feladat futtatása előtt a tevékenységhez ütemezve vannak telepítve. Ha a megadott alkalmazáscsomag és a verziója már a csomópontra, nem üzlethez van, és a meglévő csomag használják.

    Tevékenység alkalmazáscsomagok hasznosak megosztott készlet környezetekben, ahol egy készlet különböző feladat futtatása, és pedig a program nem törli a feladat befejezésekor. Ha a feladat kisebb, mint a csomópontok feladatok a készletben, tevékenység alkalmazáscsomagok adatátvitel méretűre, mivel az alkalmazás csak a feladatok futtatása csomópontok telepítik.

    Más tevékenység alkalmazáscsomagok előnyeivel eseteket, amely elsősorban nagy alkalmazással, feladatok, de csak kisszámú feladatokat. Például egy előre feldolgozása szakaszban vagy egyesítés tevékenység, a előtti feldolgozás vagy egyesítés alkalmazás esetén nehéz.

> [AZURE.IMPORTANT] Nincsenek korlátozások az alkalmazások és az alkalmazáscsomagok belül egy köteg fiókot, valamint a maximális mérete csomag számát. Lásd: [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md) ezek a korlátok kapcsolatban további tájékoztatást.

### <a name="benefits-of-application-packages"></a>Alkalmazáscsomagok előnyei

Alkalmazáscsomagok leegyszerűsíti a kódot a köteg megoldás és a csökkentse a terhelést a tevékenységek futó alkalmazások kezeléséhez szükséges.

A készlet kezdő tevékenység nem kell az adott erőforrás fájl csomópontok telepítése hosszú listáját adja meg. Nem kell manuálisan kezelése az Azure-tárolóban lévő vagy a csomóponton alkalmazás fájl több verziója. És nem kell aggódnia amiatt, hogy generálni [Társítások URL-ek](../storage/storage-dotnet-shared-access-signature-part-1.md) a fájlokat a tárterület-fiókjában hozzáférést biztosít. Köteg működik, a háttérben Azure adathordozós alkalmazáscsomagok mentheti, és üzembe helyezése csomópontok számítja ki.

## <a name="upload-and-manage-applications"></a>Töltse fel és az alkalmazások kezelése

Az [Azure portálon] használható[ portal] vagy a [Köteg Management .NET](batch-management-dotnet.md) tár kezelése a köteg fiókjában alkalmazáscsomagok. A következő néhány szakaszok azt először hivatkozás: a tárterület-fiók, majd megvitatása alkalmazások hozzáadásával és a csomagok és kezelése őket a portálra.

### <a name="link-a-storage-account"></a>Hivatkozás: a tárterület-fiók

Alkalmazáscsomagok használatához először hivatkozás Azure tárterület-fiók a köteg-fiókjába. Még nem állított a tárterület-fiók a köteg fiókjához, ha az Azure portál először kattintson az **alkalmazások** csempe a **Köteg fiók** lap a figyelmeztetés jelenik meg.

> [AZURE.IMPORTANT] A köteg jelenleg támogatja a *csak* az **Általános célú** tároló fióktípus az 5, [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)az [Azure kapcsolatos tároló fiókok](../storage/storage-create-storage-account.md)leírt módon. Amikor Azure tároló ügyfél csatolása a köteg fiók hivatkozás *csak* egy **Általános célú** tárterület-fiókkal.

![Nincs tároló beállított fiók figyelmeztetés az Azure-portálon][9]

A köteg szolgáltatás a társított tárterület-fiókot használnak tárolására és alkalmazáscsomagok beolvasása. Hivatkozás a két fiókot, miután köteg automatikusan telepítheti a csomagok, a számítási csomópontok tároló csatolt ügyfél tárolt. **Tárterület Fiókbeállítások** gombra a **figyelmeztető** lap a, és kattintson az ügyfélhez csatolni kívánt a tárterület-fiók a köteg **Tároló fiók** lap a **Tárterület-fiókot** .

![Válassza a tárterület a fiók lap az Azure-portálon][10]

Azt javasoljuk, hogy hozzon létre egy tároló fiók *kifejezetten* használatra a köteg fiókjával, és jelölje be az alábbi. További információ arról, hogy miként tárterület-fiók létrehozása "Tárterület-fiók létrehozása" az [Azure kapcsolatos tároló fiókok](../storage/storage-create-storage-account.md)látható. Miután létrehozta a tárterület-fiókot, majd azt a köteg fiókjába használatával csatolhatók a **Tárterület-fiók** a lap.

> [AZURE.WARNING] Köteg Azure tárterületet használ az alkalmazáscsomagok tárolására, mert Ön [szokásos terhére] [ storage_pricing] blokk blob-adatokhoz. Győződjön meg arról, fontolja meg a méret és a az alkalmazáscsomagok és rendszeres a költség minimalizálásához elavult csomagok eltávolítása.

### <a name="view-current-applications"></a>Aktuális alkalmazások megtekintése

A köteg fiókjában az alkalmazások megtekintéséhez kattintson az **alkalmazások** menüpont, a bal oldali menüben a **Köteg fiók** lap megtekintése közben.

![Alkalmazás csempéje][2]

Ekkor megnyílik az **alkalmazások** lap:

![Lista-alkalmazások][3]

Az **alkalmazások** lap minden alkalmazás azonosítója jeleníti meg a fiókja és a következő tulajdonságokat:

* **Csomagok**– Ez az alkalmazás társított verziók száma.
* Az **alapértelmezett verziója**– a verziót, ha nem ad meg olyan verziójú beállíthatja, hogy az alkalmazás készlet telepített. Ez a beállítás nem kötelező.
* **Frissítések engedélyezése**– az érték, amely meghatározza, hogy csomag frissíti, törléssé és kiegészítések engedélyezett. Ha **Ez nem értékű,**csomag frissítések és törlések le vannak tiltva az alkalmazáshoz. Csak új alkalmazás csomag verziók felvehetők. Az alapértelmezett érték **Igen**.

### <a name="view-application-details"></a>Alkalmazás részleteinek megtekintése

Kattintson az **alkalmazások** lap alkalmazásként az alkalmazás részleteit tartalmazó lap megnyitásához.

![Alkalmazás részletei][4]

Az alkalmazás Részletek lap konfigurálnia az alkalmazás a következő beállításokat.

* **Frissítések engedélyezése**– Megadja, hogy az az alkalmazáscsomagok frissítése vagy törölt. A jelen cikk lásd: a "Módosítása vagy törlése az alkalmazáscsomag".
* Az **alapértelmezett verzióra**– adja meg egy alapértelmezett alkalmazáscsomag üzembe csomópontok számítja ki.
* **Megjelenítendő név**– adja meg a "rövid" nevet, amely a köteg megoldás használhatja, ha adatait jeleníti meg az alkalmazást, például a felhasználói felületen egy köteg keresztül ügyfelei nyújtó szolgáltatás.

### <a name="add-a-new-application"></a>Új alkalmazás hozzáadása

Hozzon létre egy új alkalmazást, egy alkalmazáscsomag hozzáadása, és adjon meg egy új, egyedi alkalmazás azonosítót. Az első alkalmazáscsomag fel az új alkalmazás azonosító is létrehozza az új alkalmazás.

Kattintson a **Hozzáadás** gombra kattintva nyissa meg az **Új alkalmazás** lap az **alkalmazások** lap.

![Új alkalmazás lap az Azure-portálon][5]

Az **Új alkalmazás** lap Itt adhatja meg az új alkalmazás és a alkalmazáscsomag beállításait az alábbi mezőket.

**Azonosítója**

Ebben a mezőben adja meg az új alkalmazást, és a szokásos Azure köteg azonosító érvényességi szabályok vonatkoznak-e olyan azonosítója:

* Tetszőleges alfanumerikus karaktert, beleértve a kötőjeleket és aláhúzás karakterek találhatók kombinációját is tartalmazhat.
* Nem tartalmazhat legfeljebb 64 karaktert.
* A köteg fiók belül egyedinek kell lennie.
* Az nagybetűk megőrzése és eset nincs.

**Verzió**

Itt adhatja meg az alkalmazáscsomag feltöltése, a verziója. Verzió karakterláncokat a következő adatérvényesítési szabályok vonatkoznak:

* Tetszőleges alfanumerikus karaktereket, például kötőjeleket, aláhúzás és időszakok kombinációját is tartalmazhat.
* Nem tartalmazhat legfeljebb 64 karaktert.
* Az alkalmazáson belül egyedinek kell lennie.
* Eset megőrzése és a nagybetűk között.

**Alkalmazás csomag**

Ezt a mezőt, amely tartalmazza az alkalmazás bináris fájl .zip és az alkalmazás végrehajtásához szükséges segédfájlok adja meg. Kattintson a **fájl kiválasztása** mezőben vagy tallózással keresse meg és jelöljön ki egy .zip fájlt az alkalmazás-fájlokat tartalmazó mappát ábrázoló ikonra.

Fájl kiválasztása után kattintson az **OK gombra** a feltöltés Azure tárolóhoz megkezdéséhez. Amikor a feltöltés művelet befejeződött, értesítést kap, és bezárul a lap. Attól függően, hogy a fájl feltöltése, méretét és a hálózati kapcsolat sebességét Ez a művelet bizonyos időt vehet igénybe.

> [AZURE.WARNING] Ne zárja be az **Új alkalmazás** lap, amíg a feltöltés művelet befejeződik. Ezzel leállítja a feltöltés folyamat.

### <a name="add-a-new-application-package"></a>Új alkalmazás csomag hozzáadása

Felvenni egy meglévő alkalmazás új alkalmazás csomag verzióját, jelölje ki az alkalmazások az **alkalmazások** lap, kattintson a **csomagok**, majd kattintson a **Hozzáadás gombra** kattintva nyissa meg a **Hozzáadás csomag** lap.

![Alkalmazás csomag lap hozzáadása az Azure-portálon][8]

Amint látható, a mezők megegyeznek az **Új alkalmazás** lap, de a **azonosítója** mező le van tiltva. Ahogy az új alkalmazás adja meg az új csomag **verzió** keresse meg az **Alkalmazáscsomag** .zip fájlt, majd kattintson az **OK gombra** a csomag feltöltése.

### <a name="update-or-delete-an-application-package"></a>Módosítani vagy törölni egy alkalmazáscsomag

Frissítése, vagy töröl egy meglévő alkalmazáscsomag, a Részletek lap az alkalmazás megnyitásához, majd kattintson a **csomagok** a **csomagok** lap, kattintson a módosítani kívánt alkalmazáscsomag sorában lévő **három pontra** , és nyissa meg a végrehajtani kívánt műveletet.

![Módosítani vagy törölni az Azure-portálon csomag][7]

**Frissítés**

Ha a **frissítés**gombra kattint, a *frissítés* lap jelenik meg. Ez a lap hasonlít az *Új alkalmazáscsomag* lap azonban csak a csomag kiválasztása mező engedélyezve van, amely lehetővé teszi, hogy adja meg az új ZIP-fájl feltöltése.

![Frissítés a csomag lap az Azure-portálon][11]

**Törlése**

Amikor a **Törlés**gombra a csomag verziójának a törlés megerősítését kéri, hogy, és a Köteg törlése a csomag Azure-tárhelyről. Ha töröl egy alkalmazás az alapértelmezett verzióra, az **alapértelmezett verzióra** beállítás törlődik, az alkalmazás.

![Alkalmazás törlése][12]

## <a name="install-applications-on-compute-nodes"></a>Alkalmazások telepítése a számítási csomópontok

Most, hogy az Azure portálján alkalmazáscsomagok kezelése láthatta, ölelik is telepítéséről csomópontok kiszámítása és mutassa meg a köteg műveleteket.

### <a name="install-pool-application-packages"></a>Készlet alkalmazáscsomagok telepítése

Az alkalmazás-csomagok telepítése számítási csomópontjait erőforráskészlethez tartozik, adja meg, hogy egy vagy több alkalmazás csomag *hivatkozások* pedig. Akkor adja meg a készlet alkalmazáscsomagok telepítve van egyes számítási csomópont csomópontra csatlakozik a készlet, valamint a csomópont indítani, vagy reimaged esetén.

A köteg .NET, adjon meg egy vagy több [CloudPool][net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref] új készlet létrehozásakor vagy egy meglévő készlet. A [ApplicationPackageReference] [ net_pkgref] osztály adja meg, hogy egy azonosítója és verzió telepítése egy készlet kiszámítania csomópontot.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicated: "1",
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package will be installed on each.
await myCloudPool.CommitAsync();
```

>[AZURE.IMPORTANT] Ha bármilyen okból-alkalmazás csomag telepítése sikertelen, a köteg szolgáltatás megjelöli a csomópont [használhatatlanná][net_nodestate], és nincsenek feladatok végrehajtása a csomóponton kell ütemeznie. Ebben az esetben célszerű, **Indítsa újra** az újabb kezdeményezése csomag telepítését csomópontot. A csomópont újraindítását is engedélyezheti a tevékenységütemezést ismét a csomópontra.

### <a name="install-task-application-packages"></a>Tevékenység alkalmazáscsomagok telepítése

Egy készlet hasonló, megadhatja alkalmazás csomag *hivatkozások* feladat. Ha egy tevékenységhez ütemezve van csomópontra, a csomag letöltése és kibontott csak a tevékenység parancssori végrehajtása előtt. Ha egy megadott csomag és a verziója már telepítve van a csomópontra, a csomag esetén nem töltődik le, és a meglévő csomagot használ.

Egy feladat alkalmazáscsomag, állítsa be a tevékenység [CloudTask][net_cloudtask]. [ApplicationPackageReferences] [net_cloudtask_pkgref] tulajdonság:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>A telepített alkalmazások végrehajtása

A megadott készlet vagy feladat csomagok letöltési és elnevezett könyvtár belüli kibontása a `AZ_BATCH_ROOT_DIR` a csomópont. Köteg is, amely tartalmazza az elnevezett könyvtár elérési útját környezeti változó hoz létre. A tevékenység parancssorokat használja ezt a környezeti változót, az alkalmazás a csomóponton hivatkozik. A változó a következő formátumban:

`AZ_BATCH_APP_PACKAGE_APPLICATIONID#version`

`APPLICATIONID`és `version` olyan értékek, amelyek megfelelnek a telepítéshez korábban megadott alkalmazás és a csomag verzióra. Ha például a megadott kell, hogy az alkalmazás *keverőgép* 2.7 verzióját telepítette, a tevékenység parancssorokat használható a környezeti változó férhetnek hozzá a fájlokhoz:

`AZ_BATCH_APP_PACKAGE_BLENDER#2.7`

Ha megad egy alapértelmezett verziójára, az alkalmazás, a verzió utótag elhagyható. Ha "2.7", az alapértelmezett verzióra, az alkalmazás *keverőgép*, például a feladatok hivatkozhat a következő környezeti változó, és hajtja végre, azok verzió 2.7:

`AZ_BATCH_APP_PACKAGE_BLENDER`

A következő kódrészletet egy példa tevékenység parancssori, amely az alapértelmezett verzióra *keverőgép* -alkalmazás elindul jeleníti meg:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [AZURE.TIP] Tekintse meg a [tevékenységek környezeti beállítások](batch-api-basics.md#environment-settings-for-tasks) számítási csomópont környezet beállításairól további információt a [Köteg szolgáltatás áttekintése](batch-api-basics.md) .

## <a name="update-a-pools-application-packages"></a>Egy készlet alkalmazáscsomagok frissítése

Ha egy meglévő alkalmazáskészlet-alkalmazáscsomag már van beállítva, pedig az új csomag is megadhat. Ha adja meg a készlet, az alábbi alkalmazás új csomag hivatkozást:

* Új csomópontjait, amely a készlet csatlakozás és a bármelyik meglévő csomópont indítani vagy reimaged telepíti az imént megadott csomag.
* Kiszámítania csomópontok, amikor frissíti a csomag hivatkozások már pedig nem telepíti automatikusan az új alkalmazás csomagot. Ezek kiszámítania csomópontok el kell indítani, vagy az új csomag fogadásához reimaged.
* Amikor új csomag telepítve van, a létrehozott környezeti változók tükrözni a az új alkalmazás csomag hivatkozásokat.

Ebben a példában a meglévő készlet van konfigurálva a [CloudPool]egyikeként *keverőgép* alkalmazása 2.7 verziójának[net_cloudpool]. [ApplicationPackageReferences] [net_cloudpool_pkgref]. Ha frissíteni szeretné a készlet csomópontok 2.76b verzióval, adja meg egy új [ApplicationPackageReference] [ net_pkgref] az új verzió és a jóváhagyás módosítása.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Most, hogy az új verzió van konfigurálva, minden *Új* csomópontot, hogy az erőforráskészlet illesztő lesz rá rendszerbe 2.76b verziója. A csomópontok *már* a készletben 2.76b telepítéséhez indítsa újra vagy reimage őket. Figyelje meg, hogy újraindított csomópontok megőrzi az előző csomag telepítések fájljait.

## <a name="list-the-applications-in-a-batch-account"></a>Az alkalmazások egy köteg fiókban lista

Az alkalmazások és a csomagok egy köteg fiókban jeleníthet meg a [ApplicationOperations]használatával[net_appops]. [ListApplicationSummaries] [net_appops_listappsummaries] módot.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Tördelése

Az alkalmazáscsomagok segítséget nyújthat az ügyfelekkel, jelölje be a saját feladatok alkalmazások, és adja meg a pontos verziót használja, ha a feladatokat a köteg engedélyező szolgáltatásával feldolgozása. A vevők tölthet fel, és nyomon követheti a szolgáltatás a saját alkalmazások lehetősége is nyújthatnak.

## <a name="next-steps"></a>Következő lépések

* A [Köteg REST API] [ api_rest] is támogatja az alkalmazáscsomagok való használatra. Például jelenik meg a [applicationPackageReferences] [ rest_add_pool_with_packages] elem hozzáadása [egy fiókot egy készlet] [ rest_add_pool] adja meg, telepítse a REST API-csomagok olvashat. Lásd: [alkalmazások] [ rest_applications] alkalmazás információk beszerzése a köteg REST API használatával kapcsolatban további tájékoztatást.

* Megtudhatja, hogyan programozás útján az [Azure köteg fiókok és a köteg Management .NET kvóták kezelése](batch-management-dotnet.md). A [Köteg Management .NET] [ api_net_mgmt] tár engedélyezheti a fiók létrehozásakor vagy törlésekor funkciók a köteg alkalmazás vagy szolgáltatás.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Alkalmazás csomagok magas szintű diagram"
[2]: ./media/batch-application-packages/app_pkg_02.png "Az Azure-portálon alkalmazás csempéje"
[3]: ./media/batch-application-packages/app_pkg_03.png "Az Azure-portálon alkalmazások lap"
[4]: ./media/batch-application-packages/app_pkg_04.png "Az Azure-portálon alkalmazás a Részletek lap"
[5]: ./media/batch-application-packages/app_pkg_05.png "Új alkalmazás lap az Azure-portálon"
[7]: ./media/batch-application-packages/app_pkg_07.png "Módosítani vagy törölni a legördülő menü az Azure-portálon csomagok"
[8]: ./media/batch-application-packages/app_pkg_08.png "Új alkalmazás csomag lap az Azure-portálon"
[9]: ./media/batch-application-packages/app_pkg_09.png "Nincs csatolt tároló fiók értesítés"
[10]: ./media/batch-application-packages/app_pkg_10.png "Válassza a tárterület a fiók lap az Azure-portálon"
[11]: ./media/batch-application-packages/app_pkg_11.png "Frissítés a csomag lap az Azure-portálon"
[12]: ./media/batch-application-packages/app_pkg_12.png "Törölje az Azure-portálon csomag törlésének megerősítését kérő párbeszédpanel"
