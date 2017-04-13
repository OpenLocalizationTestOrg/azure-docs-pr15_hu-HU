<properties
   pageTitle="Gyakori Azure-telepítési hibák elhárítása |} Microsoft Azure"
   description="Gyakori hibák megoldásáról, amikor rendszerbe állítják erőforrások Azure erőforrás-kezelővel Azure ismerteti."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="azure telepítési hiba, azure példányban telepítése"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Hibaelhárítás közös Azure környezetben a Azure-kezelő eszközzel

Ez a témakör bemutatja, hogyan oldhatja meg néhány gyakori előfordulhatnak Azure-telepítési hibák. Ha további információt a Mi történt a telepítéssel, először tanulmányozza a [Nézet üzembe helyezési műveleteket](resource-manager-troubleshoot-deployments-portal.md) , és ezután térjen vissza az Ez a cikk a probléma megoldása érdekében. Ez a témakör a szakaszokban a hibakód: látható.

## <a name="invalid-template"></a>Érvénytelen sablon

Ezt a hibát okozhat a különböző típusú hibákat. 

### <a name="syntax-error"></a>Szintaktikai hiba

Ha olyan hibaüzenetet kap, amely jelzi a sablont nem sikerült érvényességi, előfordulhat szintaktikai hiba a sablon. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Ez a hiba akkor könnyen végezhet, mert a sablon a kifejezések bonyolult lehetnek. A következő nevet hozzárendelés tárterület-fiók például szögletes zárójelek között az egyik készlete, három függvények, három különböző zárójelek, aposztrófok az egyik készlete és egy tulajdonsággal tartalmazza:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Ha nem ad meg a megfelelő szintaxis, a sablon egy érték, amely nem a szándék hoz létre.

Az ilyen típusú hiba jelenik meg, amikor gondosan tekintse át a kifejezések szintaxisával. Fontolja meg egy JSON, például a [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) és a [Visual Studio Code](resource-manager-vs-code.md)szintaktikai hibák kapcsolatos figyelmeztetés, amely a szerkesztő. 

### <a name="incorrect-segment-lengths"></a>A szakasz helytelen hossza

Más érvénytelen sablon hiba lép fel, ha az erőforrás neve nem a megfelelő formátumban.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Legfelső szintű erőforrás egy kisebb szakasz rendelkeznie kell a nevet, mint a az erőforrás típusa. Minden szakasz perjellel különbözteti meg van. A következő példában a típus két szegmenst tartalmaz, és az nevét egy szakaszában, így **érvényes nevet**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

De a következő példa **nem egy érvényes nevet** mert ugyanannyi szakaszokat, mint a típus van.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Az erőforrásokhoz, a gyermek típusa és neve szegmensek ugyanannyi van. Ennyi szegmensek értelme, mert a teljes nevét, és írja be a gyermek tartalmaz, a szülő név és típus. Emiatt a teljes neve továbbra is tartalmaz, kisebb, mint a teljes típus egy szakasz. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Első szakasz megfelelő körlevelekben erőforrás-kezelő típusú erőforrás-szolgáltatók között alkalmazott lehet. Például az erőforrás lakat alkalmazása egy webhely szükséges négy szegmensek típus. A név ezért három szegmensek:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Nem várt másolás index

Ha a sablont, amely nem támogatja ezt az elemet egy részének alkalmazott az **másolása** elemet, bizonyos **InvalidTemplate** hibára. Csak a Másolás elem alkalmazhat egy erőforrás típusa. Másolás belül egy erőforrás típusa tulajdonság nem alkalmazható. Például másolás alkalmazása egy virtuális gép, de nem vonatkozik az operációs rendszer lemezt virtuális géphez. Bizonyos esetekben másolás ismétlődve létrehozása szülő erőforrás gyermek erőforrás alakíthatja. Másolás használatával kapcsolatos további tudnivalókért lásd: [erőforrások az Azure erőforrás-kezelő több példányának létrehozása](resource-group-create-multiple.md).

### <a name="parameter-is-not-valid"></a>Paraméter nem érvényes

Ha a sablon paraméter megengedett értékeket meghatározza, és megad egy értéket, amely nem szereplő értékek közül, az alábbihoz hasonló üzenet jelenhet meg:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Dupla, jelölje be a sablon a megengedett érték, és adja meg egy, a telepítés során.

## <a name="not-found-or-resource-not-found"></a>Nem található (vagy az erőforrás nem található)

A sablon, amely nem oldható erőforrás nevét tartalmazza, a hasonló hibaüzenet jelenhet meg:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Ha megpróbálja telepíteni a sablonban a hiányzó erőforrást, ellenőrizze, hogy szükséges függőség hozzáadni. Erőforrás-kezelő optimalizálja a telepítési párhuzamosan, amikor csak lehetséges erőforrások létrehozásával. Egy erőforrás egy másik erőforrás után kell telepíthető, ha a sablon a **dependsOn** elem segítségével függőség létrehozása a erőforráson szeretne. Ha például webalkalmazást telepítésekor az alkalmazás szolgáltatáscsomagja léteznie kell. Nem adta meg, hogy a web app függ, hogy az alkalmazás szolgáltatáscsomagja, ha az erőforrás-kezelő egyszerre mindkét erőforrások hoz létre. Hibaüzenet jelenik meg arról, hogy az alkalmazás szolgáltatás terv erőforrás nem található, mert azt még nem létezik amikor megpróbál egy tulajdonságot állítsa be a web app Ha meg szeretné ezt a hibát a web App alkalmazásban a függőséget megadásával.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
Szintén meg ezt a hibát az erőforrás szerepel-e egy másik erőforráscsoport, mint az éppen felületek érdekében. Ebben az esetben a [resourceId függvény](resource-group-template-functions.md#resourceid) használatával el az erőforrás teljesen minősített neve.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Ha a [hivatkozásban](resource-group-template-functions.md#referenc) vagy [listKeys](resource-group-template-functions.md#listkeys) függvényt használhatja, amely nem oldható erőforráshoz megpróbálja, a következő hibaüzenet jelenik meg:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Keresse meg a kifejezést, amely tartalmazza a **hivatkozás** függvényt. Dupla ellenőrizze, hogy helyesek-e a paraméterértékeket.

## <a name="storage-account-already-exists-or-already-taken"></a>Már létezik tároló fiók (vagy már használatban)

Tárterület-fiókok esetén az Azure keresztül egyedi erőforrás nevét kell megadnia. Ha nem ad meg egy egyedi nevet, például egy hibaüzenet jelenik meg:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

Létrehozhat egy egyedi nevet, a [uniqueString](resource-group-template-functions.md#uniquestring) függvény eredményét az elnevezési összefűzésével.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Ha az előfizetés üzembe helyezni egy tároló fiók neve megegyezik egy meglévő tárterület-fiókot, de adja meg egy másik helyre, a tárhely fiók már létezik egy másik helyre a jelző hibaüzenetet kap. Törölje a meglévő tárterület-fiókkal, vagy adja meg a meglévő tárterület-fiókként ugyanazon a helyen.

## <a name="account-name-invalid"></a>A fióknév érvénytelen

**AccountNameInvalid** hibaüzenet jelenik meg, amikor megpróbálja nevezze el a tárterület-fiókot, amely tartalmazza tiltott karaktert. Tároló fióknevét 3 és 24 karakter hosszúságú között legyen, és használja a számokat és csak a kisbetűk betűket.

## <a name="no-registered-provider-found"></a>Nem található regisztrált szolgáltató

Erőforrás telepítésekor a következő hiba jelenik meg, és az üzenet jelenhet meg:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Ez a hiba három okok egyike jelenhet meg:

1. Az erőforrás típusa nem támogatott helye
2. API-verzió nem támogatja az erőforrás típusa
3. Az erőforrás-szolgáltató nem regisztrált az előfizetéshez

A hibaüzenet jelenik meg kell adnia a támogatott helyek és az API-verziók javaslatok. Módosíthatja a sablon egy javasolt értéket. A legtöbb szolgáltató regisztrált gombra kattintva automatikusan az Azure portálon vagy a parancssori felületet használja, de nem az összes olyan. Ha még nem használta az adott erőforráshoz szolgáltató előtt, szükség lehet regisztrálhatja adott szolgáltató. További információ az erőforrás-szolgáltatók PowerShell vagy Azure CLI keresztül felfedezheti.

### <a name="powershell"></a>A PowerShell

**Get-AzureRmResourceProvider**segítségével a regisztrációs állapotának megtekintéséhez.

    Get-AzureRmResourceProvider -ListAvailable

Regisztrálni a szolgáltató, **Külső.FÜGGV-AzureRmResourceProvider** használja, és adja meg a nevét, az erőforrás-szolgáltató szeretné rögzíteni.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

A támogatott helyeken egy adott típusú erőforrás, használja:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

A támogatott API-verziók egy adott típusú erőforrás, használja:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Szeretné látni, hogy a szolgáltató van regisztrálva, használja a `azure provider list` parancsot.

    azure provider list

Egy erőforrás-szolgáltató regisztrálásához használhatja a `azure provider register` parancsot, és adja meg a *névtér* regisztrálni.

    azure provider register Microsoft.Cdn

Erőforrás-szolgáltató a támogatott helyek és az API-verziók megtekintéséhez használja:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>A művelet nem engedélyezett

Lehet, hogy problémák Ha telepítési meghaladja lehet erőforráscsoport, az előfizetések, a partnerek és a más hatókörök használati kvóta. Az előfizetés például terület fúrásmintáit számát is beállítható. Ha egy további magmintákat, mint a megengedett összeg virtuális készülék üzembe megpróbálja, hibaüzenet arról, hogy a túllépte.
Teljes kvóta tudnivalók: [Azure előfizetés és szolgáltatás korlátozások, kvótákat, és korlátozások](azure-subscription-service-limits.md).

Az előfizetés kvóták magmintákat vizsgálatához használható a `azure vm list-usage` az Azure CLI parancsot. A következő példa bemutatja, hogy az alapvető kvóta ingyenes próba-fiók-e a 4:

    azure vm list-usage

Amely adja eredményül:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Ha egy sablont, amely legfeljebb négy magmintákat létrehozza a nyugati USA-beli tartományban lévő telepít, akkor egy telepítési hiba, amely a következőképpen néz:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Vagy a PowerShellben, használhatja a **Get-AzureRmVMUsage** parancsmag.

    Get-AzureRmVMUsage

Amely adja eredményül:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

Ebben az esetben nyissa meg a portál és a fájl egy ügyfélszolgálati probléma tipp a kvóta a régió, amelybe menteni szeretné a üzembe.

> [AZURE.NOTE] Ne feledje, hogy az erőforrás-csoportok kvóta nem az egész előfizetéshez tartozó minden egyes területhez tartozik. 30 magmintákat nyugati USA-beli telepíteni kell esetén lépjen kapcsolatba a nyugati USA-beli 30 erőforrás-kezelő magmintákat. Ha kell üzembe 30 magmintákat bármelyik azon részeire lépkedhet, amelyhez hozzáférése van, kérdezze meg az összes régióban 30 erőforrás-kezelő magmintákat.

## <a name="invalid-content-link"></a>Érvénytelen Tartalomhivatkozás

Ha a hibaüzenetet kap:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

Valószínűleg próbált hivatkozni szeretne egy beágyazott sablont, amely nem érhető el. A beágyazott sablon adni a URI duplán jelölőnégyzetet. Ha a sablon egy tároló fiók létezik, legyen az URI érhető el. Előfordulhat, hogy a biztonsági jogkivonat átadni. További tudnivalókért olvassa el a [Azure erőforrás-kezelővel csatolt sablonok használata](resource-group-linked-templates.md)című témakört.

## <a name="authorization-failed"></a>Nem sikerült engedély

Hibaüzenet jelenhet a telepítés során, mert a fiók vagy a fő megpróbálja telepíteni az erőforrások szolgáltatás nincs hozzáférésük hajtsa végre ezeket a műveleteket. Azure Active Directory lehetővé teszi, vagy a rendszergazdát, hogy melyik identitások szabályozhatja hozzáférhetnek a pontosság remek fokú erőforrásoktól. Ha például fiókját az Olvasó szerepkör van rendelve, ha Ön nem erőforrások létrehozhatnak. Ebben az esetben megjelenik egy hiba jelenik meg, miszerint engedélyezése nem sikerült.

Szerepköralapú hozzáférés-vezérlés kapcsolatos további tudnivalókért olvassa el a [Hozzáférés-vezérlés Azure Role-Based](./active-directory/role-based-access-control-configure.md)című témakört.

Szerepköralapú hozzáférés-vezérlés mellett a telepítési végrehajtott műveletek az előfizetés házirendek függvényében korlátozva lehet. Házirendek a rendszergazda telepítését az előfizetést az összes erőforrás szabályok is be. A rendszergazda megkövetelheti például, hogy egy adott címke érték egy erőforrás típusa megadva. Ha nem teljesíti a házirend követelményeket, hibaüzenetet kaphat a telepítés során. Házirendek kapcsolatos további tudnivalókért olvassa el a [Házirendek kezelheti az erőforrásokat, és a hozzáférés vezérléséhez használatával](resource-manager-policy.md)című témakört.

## <a name="troubleshooting-virtual-machines"></a>Virtuális gépeken futó – hibaelhárítás

| Hibaüzenet | Cikkek |
| -------- | ----------- |
| Egyéni parancsfájl bővítmény hibák | [A Windows virtuális bővítmény hibák](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />vagy<br />[Linux virtuális bővítmény hibák](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Áttelepítési hibákkal OS képe | [Új Windows virtuális hibák](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />vagy<br />[Új Linux virtuális hibák](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Felosztás hibák | [A Windows virtuális terhelés hibák](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />vagy<br />[Linux virtuális terhelés hibák](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Csatlakozás biztonságos rendszerhéj (SSH)-hibák | [Biztonságos Linux virtuális Shell-kapcsolatot](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Kapcsolódás virtuális alkalmazást hibák | [A Windows virtuális futó alkalmazás](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />vagy<br />[Egy Linux virtuális alkalmazást](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Távoli asztali kapcsolat hibák | [A Windows virtuális távoli asztali kapcsolatok](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| A rendszer által újbóli csatlakozási hibák | [Telepítsen újra virtuális gép új Azure csomóponthoz](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Felhőalapú szolgáltatás hibák | [Felhőalapú szolgáltatás telepítési problémák](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Más szolgáltatásokkal kapcsolatos hibaelhárítás

Az alábbi táblázatban nem témakörök hibaelhárítás az Azure listáját. Ehelyett, szolgáltatásaival kapcsolatos problémák megoldásához telepítése vagy az erőforrások beállítása. Ha a futási idejű problémák erőforrás segítségre van szüksége, olvassa el a Azure szolgáltatás dokumentációjában.

| Szolgáltatás | A cikk |
| -------- | -------- |
| Automatizálási | [Hibaelhárítási tippek az Azure automatizálás gyakori hibák](./automation/automation-troubleshooting-automation-errors.md) |
| Azure Papírhalom | [Microsoft Azure Papírhalom – hibaelhárítás](./azure-stack/azure-stack-troubleshooting.md) |
| Azure Papírhalom | [Webalkalmazások és Azure Papírhalom](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Adatok gyári | [Adatok gyári kapcsolatos problémák megoldása](./data-factory/data-factory-troubleshoot.md) |
| Szolgáltatás háló | [Amikor rendszerbe állítják Azure Service háló szolgáltatások a kapcsolatos gyakori problémák megoldása](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Webhely-helyreállítás | [Figyelésére és virtuális gépeken futó és fizikai kiszolgálók védelme – problémamegoldás](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Tárhely | [Figyelése, diagnosztizálása és a Microsoft Azure tároló – problémamegoldás](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [StorSimple eszköz telepítési problémák elhárítása](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-adatbázis | [Azure SQL-adatbázishoz kapcsolódási problémák elhárítása](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL-adatraktár | [Az SQL Azure-adatraktár – hibaelhárítás](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Amikor készen áll-e a telepítési ismertetése

Azure erőforrás-kezelő jelentések egy példányban sikeres, ha az összes szolgáltató sikeresen bevétel a telepítési. Jó helyen jár ez az üzenet, nem feltétlenül jelenti, hogy az erőforráscsoport "aktív, és készen áll a felhasználók a." A telepítés lehet például töltse le a frissítések, várjon nem sablon erőforrások vagy összetett parancsfájlok telepítése. Erőforrás-kezelő nem tudja, ha ezeket a feladatokat végrehajtani, mert nem, a szolgáltató nyomon követő tevékenységek. Ezekben az esetekben lehet egy kis időt, mielőtt az erőforrások használatra való életben. Eredményeként kell a várt, hogy a telepítés állapotának sikerül egy rövid idő múlva a üzembe is használható.

Megakadályozhatja, hogy Azure jelentéskészítés telepítési success, azonban a egyéni sablon egyéni parancsfájl létrehozásával. A parancsfájl tudja, hogyan kell figyelni a teljes telepítést, a rendszer szintű munkahelyi és intézményi, és csak akkor, amikor a felhasználók használhatják a teljes példányban sikeresen adja eredményül. Ahhoz, hogy a bővítmény szükség az utolsó futtatása, a sablon **dependsOn** tulajdonságot használja.

## <a name="next-steps"></a>Következő lépések

- Műveletek naplózási kapcsolatos további tudnivalókért olvassa el a [naplózási műveletek az erőforrás-kezelő](resource-group-audit.md)című témakört.
- A hibák meghatározása a telepítés során műveletek kapcsolatos további tudnivalókért lásd: [Nézet üzembe helyezési műveleteket](resource-manager-troubleshoot-deployments-portal.md).
