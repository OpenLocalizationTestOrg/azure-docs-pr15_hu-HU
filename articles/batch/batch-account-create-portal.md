<properties
    pageTitle="Hozzon létre egy köteg Azure fiókot |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure köteg fiók létrehozása az Azure-portálon nagyméretű párhuzamos munkaterhelésekből futtatásához a felhőben"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Az Azure portálon Azure köteg fiók létrehozása

> [AZURE.SELECTOR]
- [Azure portál](batch-account-create-portal.md)
- [.NET köteg kezelése](batch-management-dotnet.md)

Megtudhatja, hogy miként Azure köteg fiók létrehozása az [Azure portál][azure_portal], és a hol találhatók a fiók tulajdonságai, mint a hívóbillentyűk és URL-címek fiók fontos. Is ölelik árak, és Azure tárterület-fiókkal való köteg fiókját, hogy az [alkalmazáscsomagok](batch-application-packages.md) és a [probléma továbbra is fennáll a feladatot, és a tevékenység kimeneti](batch-task-output.md)köteget.

## <a name="create-a-batch-account"></a>Hozzon létre egy köteg fiókot

1. Jelentkezzen be az [Azure portál][azure_portal].

2. Kattintson az **Új** > **kiszámítania** > **Köteg szolgáltatás**.

    ![A piactér a köteg][marketplace_portal]

3. Az **Új köteget fiók** lap jelenik meg. Látható elemek *egy* – *e* alatti minden lap elem leírását.

    ![Hozzon létre egy köteg fiókot][account_portal]

    egy. **Fióknév**: a köteg fiók egy egyedi nevet. Ez a név létrejön a fiók, (lásd az alábbi *helyen* ) az Azure terület belül egyedinek kell lennie. Tartalmazhat csak kisbetűs karakterek, számok, és a 3-24 karakter hosszúságú lehet.

    b. **Előfizetés**: előfizetés, amelyben a köteg fiók létrehozásához. Ha csak egy előfizetéssel rendelkezik, alapértelmezés szerint van jelölve.

    c billentyűkombinációt. **Erőforráscsoport**: meglévő erőforrás köteg-fiókja, és a csoport tetszés szerint hozzon létre egy újat.

    d. **Hely**:, amelyben a köteg fiók létrehozása az Azure régió. Az erőforráscsoport és előfizetési által támogatott régiók másként lehetőségek jelennek meg.

    e. **Tárterület-fiók** (nem kötelező): egy **Általános célú** tárterület-fiókkal (csatolás) társíthatja az új köteget fiókba. További részleteket lásd: a [csatolt Azure tároló fiók](#linked-azure-storage-account) alatt.

4. Kattintson a **Létrehozás** a fiók létrehozása gombra.

  A portálon azt jelzi, hogy **üzembe helyezése** a fiókot, és befejeztével **telepítések sikeres** értesítés jelenik meg az *értesítések*.

## <a name="view-batch-account-properties"></a>Köteg fiók tulajdonságainak megtekintése

A fiók létrehozása után nyissa meg a **Köteg fiók lap** eléréséhez a beállításokat, és a Tulajdonságok parancsot. A köteg fiók lap bal oldali menü segítségével hozzáférhet fiókbeállítások és a Tulajdonságok parancsot.

![Az Azure-portálon a köteg fiók lap][account_blade]

* **Köteg fiók URL-címe**: hoz létre a [Köteg fejlesztési API-khoz](batch-technical-overview.md#batch-development-apis) alkalmazások-fiók URL-CÍMÉT a kezelheti az erőforrásokat, és futtassa a feladatok fiók szükséges. Egy köteg fiók URL-címet a következő formátumban van:

    `https://<account_name>.<region>.batch.azure.com`

![A portál köteg fiók URL-címe][account_url]

* **Hívóbetűk**: az alkalmazások is szüksége hívóbetű, amikor a köteg fiókjában erőforrások használata. Írja be a megtekintéséhez, illetve a köteg fiók hívóbetűk újragenerálása `keys` **a köteg fiók lap bal oldali menü keresőmezőjébe,** és jelölje ki **billentyűk**.

    ![Az Azure-portálon a köteg fiók kulcsok][account_keys]

## <a name="pricing"></a>Árak

Csak egy "ingyenes réteg," ami azt jelenti, nem a fel a köteg fiók magát a köteg fiókok ajánlja fel. A köteg megoldások felhasználása mögöttes Azure számítási erőforrások, valamint az erőforrásokat, a feladatok futtatásakor más szolgáltatások elfogyasztott előfizetést terhelő. Például a készletek a számítási csomópontok terheli, és az adatok tárolhat Azure-tárolóban lévő mint a bemeneti és kimeneti a tevékenységekhez. Hasonlóképpen a köteg [alkalmazáscsomagok](batch-application-packages.md) szolgáltatását használja, ha az előfizetést terhelő az Azure tárolási erőforrásokat, az alkalmazáscsomagok tárolására szolgál. Lásd: a [Köteg árak] [ batch_pricing] további információt.

## <a name="linked-azure-storage-account"></a>Csatolt Azure tárterület-fiók

A korábban említett (nem kötelező) mutató hivatkozás létrehozásához egy **Általános célú** tárterület-fiókot a köteg fiók. A köteg [alkalmazáscsomagok](batch-application-packages.md) szolgáltatását használja blob-tárolóhoz egy csatolt általános célú tárterület-fiókkal, a a [Köteg fájl szabályok .NET](batch-task-output.md) tárat. Nem kötelező funkciókról segítséget nyújtanak az alkalmazások futtatásához a köteg feladatokat üzembe helyezése, és az adatokat azok konzerv pályától tartós.

A köteg jelenleg támogatja a *csak* az **Általános célú** tároló fióktípus az 5, [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)az [Azure kapcsolatos tároló fiókok](../storage/storage-create-storage-account.md)leírt módon. Azure tároló ügyfél csatolása köteg fiókját, amikor arról, hogy hivatkozás lehet *csak* egy **Általános célú** tárterület-fiókkal.

!["Általános célú" tároló fiók létrehozása][storage_account]

Azt javasoljuk, hogy a köteg fiókjába kizárólagos használatra tárterület-fiók létrehozása.

>[AZURE.WARNING] Amikor egy csatolt tárterület-fiók a hívóbetűk újragenerálása gondoskodik. Csak egy tároló fiókkulcs újragenerálása, majd kattintson **Szinkronizálási kulcsok** a csatolt tárhely a fiók lap. Várja meg, öt perc ahhoz billentyűk eltarthat, amíg megtörténik a készletek, a számítási csomópontok újragenerálása és, majd a más billentyűt szinkronizálni, ha szükséges. Ha egyszerre mindkét billentyűk követező létrehozásakor, a számítási csomópontok nem tud szinkronizálni bármelyik billentyűt, és azok elveszíti a hozzáférést a tárterület-fiókjába.

  ![Tárterület-fiók billentyűk újragenerálása][4]

## <a name="batch-service-quotas-and-limits"></a>Köteg szolgáltatás kvóták és korlátai

Ügyeljen arra, hogy az Azure-előfizetése, és egyéb Azure szolgáltatás, bizonyos [kvótáinak és korlátok](batch-quota-limit.md) köteg fiókra alkalmazhatja. A fiók **Tulajdonságok**portál köteg fiókot aktuális kvóták jelennek meg.

![Köteg fiók kvóták az Azure-portálon][quotas]

Ezek a kvóták érdemes szem előtt tartani tervezése és felfelé a köteg munkaterhelésekből méretezését. Például ha a készlet alkalmazáson keresztül nem korábban megadott számítási csomópontok cél számát, előfordulhat, hogy elérte az alapvető kvótakorlátja a köteg fiók.

Tartsa szem előtt, hogy vannak nem korlátozta a egy köteg számla Azure-előfizetéséhez. Több köteg munkaterhelésekből futtatása egy köteg egyetlen fiókban, vagy a köteg fiókok ugyanabban az előfizetésben, de más Azure régióban munkaterhelésének terjesztése.

Ezek a kvóták számos növelhető egyszerűen az elküldött az Azure-portálon termék ingyenes támogatási kérelmet. Lásd: [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md) kapcsolatos részleteket kérő kvóta nő.

## <a name="other-batch-account-management-options"></a>Más köteg Fiókbeállítások kezelése

Mellett az Azure portál használata esetén is létrehozása és kezelése a köteg fiókok a következő:

* [Köteg PowerShell-parancsmagok](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](../xplat-cli-install.md)
* [.NET köteg kezelése](batch-management-dotnet.md)

## <a name="next-steps"></a>Következő lépések

* A [Köteg Azure szolgáltatás áttekintése](batch-api-basics.md) további információt a köteg szolgáltatás – fogalmak és szolgáltatások című témakörben találhat. A cikk azt ismerteti, hogy az elsődleges köteg erőforrások, például készletek, a számítási csomópontok, a feladatok és a feladatok, és a nagyméretű számítási terhelést végrehajtásának engedélyezéséhez szolgáltatások áttekintése.

* Ismerje meg, hogy egy köteg engedélyező alkalmazást a [Köteg .NET ügyfél tár](batch-dotnet-get-started.md)elkészítésének alapjait. A [bevezető cikk](batch-dotnet-get-started.md) végigvezeti Önt a munkaidő-alkalmazások végrehajtani a terhelési több számítási csomópontok a köteg szolgáltatását használja, amely tartalmazza az Azure-tárhely használatának terhelést fájl fejlesztői és a lekérés.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Tárterület-fiók billentyűk újragenerálása"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
