<properties
    pageTitle="Első lépések a szerepkörök, engedélyek és biztonsági Azure monitorral rendelkező |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure Monitor beépített szerepkörök és engedélyek használatával erőforrások figyelése korlátozhatja a hozzáférést."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Első lépések a szerepkörök, engedélyek és biztonsági Azure monitorral

Sok csapatok kell feltétlenül szabályozására figyelése adatai és beállításai eléréséhez. Például ha kizárólag a figyelése (támogatási szakemberei devops mérnökök) dolgozó csapattagok rendelkezik, vagy ha használja a szolgáltató felügyelt, érdemes hozzáférést őket csak nyomon követési adatok korlátozásával létrehozása, módosítása és törlése erőforrásokat. Ez a cikk bemutatja, hogyan gyorsan egy beépített felügyeleti RBAC szerepkör alkalmazása Azure-ban egy felhasználó, vagy a saját egyéni szerepkör egy felhasználóhoz, akinek van szüksége a megfigyeléssel korlátozott engedélyekkel készíthet. A cikk ismerteti az Azure Monitor kapcsolódó erőforrásokat, és hogyan korlátozhatja a hozzáférést az adatokat tartalmaznak biztonsági kérdései majd.

## <a name="built-in-monitoring-roles"></a>Beépített felügyeleti szerepkörök

Súgó készült Azure Monitor beépített szerepkörök korlátozhatja a hozzáférést az előfizetés továbbra is engedélyezése felelősöket beszerzése, és állítsa be a szükséges adatok infrastruktúra figyelemmel kísérésére során erőforrásokra. Azure Monitor nyújt két ki-be a szerepkörök: A figyelése olvasó és figyelése munkatársi.

### <a name="monitoring-reader"></a>A képernyőolvasók figyelése

Az ellenőrzés olvasó szerepkör tagjai előfizetés megtekintheti az összes felügyeleti adatokat, de nem minden erőforrás módosítása és bármely kapcsolódó erőforrásokat figyelése beállításainak szerkesztése. A szerepkör célszerű a szervezet, például a támogatási vagy műveletek mérnökök, akiknek engedélyezni szeretné a felhasználók számára:

- Ellenőrző irányítópultok megtekintése a portálon, és hozzon létre saját személyes felügyeleti irányítópultok.
- Az [Azure Monitor REST API -t](https://msdn.microsoft.com/library/azure/dn931930.aspx), [PowerShell-parancsmagok](insights-powershell-samples.md)vagy [platformok CLI](insights-cli-samples.md)mértékek lekérdezés.
- A tevékenység naplója a portálon, Azure Monitor REST API-t, PowerShell-parancsmagok vagy platformok CLI lekérdezés.
- Az adott erőforrás [Diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) megtekintése.
- Az előfizetés [napló profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) megtekintése.
- Automatikus méretezéssel beállítások megtekintése.
- Riasztási tevékenység megtekintése és beállításokat.
- Alkalmazás háttérismeretek adatok eléréséhez, és AI Analytics adatainak megtekintése.
- Log Analytics (MOBILE) munkaterület adatokat, többek között a munkaterület használati adatainak kereshet.
- Log Analytics (MOBILE) felügyeleti csoportok megtekintése hivatkozásra.
- A napló Analytics (MOBILE) keresési séma beolvasásához.
- Log Analytics (MOBILE) üzletiintelligencia-csomagok listája.
- Beolvasásához, majd hajtsa végre a napló Analytics (MOBILE) mentett keresések.
- Lekérdezi a napló Analytics (MOBILE) tárolási konfigurációt.

> [AZURE.NOTE] A szerepkör nem folyamatosan lejátszott esemény-hubon keresztül csatlakozott, vagy a tárterület-fiókjában tárolt naplóadatok olvasási hozzáférést adni. [Lásd az alábbi](#security-considerations-for-monitoring-data) konfigurálása access az alábbi forrásokban olvashat.

### <a name="monitoring-contributor"></a>Közös munka figyelése

A figyelés munkatársi szerepkörök társított személyek nyomon az összes adat megtekinthetők az előfizetés létrehozása vagy és figyelése beállítások módosítása, de minden más erőforrások: nem lehet módosítani. A szerepkör felülbírálja az ellenőrzés olvasó szerepkör, és egy szervezet felügyeleti csoport- vagy ki az engedélyeket a fenti mellett is kell tudja felügyelt szolgáltatók tagjai számára megfelelő:

- Közzététel egy megosztott irányítópult felügyeleti irányítópultok.
- Egy resource.* [Diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) megadása
- A [napló profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) egy subscription.* beállítása
- Riasztási tevékenység és beállítások megadásához.
- Hozzon létre alkalmazás háttérismeretek webes vizsgálatok és -összetevők.
- Log Analytics (MOBILE) megosztott munkaterület billentyűk sorolja fel.
- Engedélyezheti vagy letilthatja az üzletiintelligencia-csomagok napló Analytics (MOBILE).
- Hozzon létre, és törlése, majd hajtsa végre a napló Analytics (MOBILE) mentett keresések.
- Hozzon létre, és törölje a napló Analytics (MOBILE) tároló konfigurációt.

* felhasználói is külön-külön engedéllyel kell ListKeys a cél erőforráson (tárhely fiók vagy esemény központi névtér) napló profil vagy diagnosztikai beállítást kíván megadni.

> [AZURE.NOTE] A szerepkör nem folyamatosan lejátszott esemény-hubon keresztül csatlakozott, vagy a tárterület-fiókjában tárolt naplóadatok olvasási hozzáférést adni. [Lásd az alábbi](#security-considerations-for-monitoring-data) konfigurálása access az alábbi forrásokban olvashat.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Az engedélyek és egyéni RBAC szerepkörök figyelése
Ha a fenti beépített szerepkörök pontos igényeinek nem a csoporthoz, [Hozzon létre egy egyéni RBAC szerepkört](../active-directory/role-based-access-control-custom-roles.md) finomabb engedélyekkel rendelkező is. Az alábbiakban a gyakori Azure Monitor RBAC műveletek és azok leírását.

| Művelet                                                   | Leírás                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, írás, törlés]         | A figyelmeztetési szabályok olvasási és írási/törlése.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | A figyelmeztetési szabályok a lista, események (a szabályt, amelyen éppen előzmények). Ez csak a portálon vonatkozik.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, írás, törlés]  | Olvasási és írási/törlése Automatikus méretezéssel beállításai gombra.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, írás, törlés] | Diagnosztikai beállítások olvasási és írási/törlése.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | Ezzel az engedéllyel szükség a felhasználóknak, akik tevékenység naplók használni a portálon keresztül.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | A lista tevékenység naplóesemények (fiókkezelési események) az előfizetést. Ezzel az engedéllyel kell alkalmazni a tevékenységnapló programozott és a portál elérését. |
| Microsoft.Insights/LogDefinitions/Read                      | Ezzel az engedéllyel szükség a felhasználóknak, akik tevékenység naplók használni a portálon keresztül.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | Olvassa el a metrikus definíciók (a rendelkezésre álló metrikus típusok egy erőforrás lista).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | Olvassa el az adott erőforrás mértékek.                                                                                                                              |

> [AZURE.NOTE] Access értesítések, a diagnosztikai beállítások és a mértékek, az erőforrás van szükség az, hogy a felhasználó rendelkezik-e az erőforrás típusa és az adott erőforrás köre olvasási hozzáférést. ("Olvasás"), amely a tárterület-fiók vagy az esemény hubok adatfolyamok archívumok diagnosztikai beállítást, vagy jelentkezzen profil létrehozásához a felhasználó számára is ListKeys engedélye a cél erőforráson.

Például használja a fenti táblázatban, létrehozhat egy egyéni RBAC szerepkör-"tevékenység naplója olvasó" jelennek meg:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Nyomon követési adatok biztonsági kérdései
Nyomon követési adatok – különösen naplófájlok – bizalmas információkat, például az IP-címek vagy felhasználó neve tartalmazhat. Három egyszerű űrlapok szolgál az Azure-adatok figyelése:
1. A tevékenység naplót, amely az összes vezérlő-sík műveleteket ismerteti az Azure-előfizetésben.
2. A diagnosztikai naplók, amelyek egy erőforrás által kibocsátott naplók.
3. Mértékek, amely az erőforrások által kibocsátott vannak.

Ezek az adattípusok mindhárom lehet tárterület-fiókjában tárolt vagy esemény-hubon keresztül csatlakozott, amelyek mindkettő általános célú Azure erőforrások folyamatosan lejátszott. Ezek az erőforrások általános célú, létrehozása, törlése és elérése őket a rendszergazda általában fenntartott kiemelt művelet található. Javasoljuk, hogy figyelése kapcsolódó erőforrásokat a következő tanácsok segítségével megakadályozására:

- Használjon egyszeri, célorientált tárhely a nyomon követési adatok. Ha nyomon adatok bonthatók több tárterület-fiókot kell ne ossza meg a tárhely fiókok között az ellenőrzés használatát és -ellenőrző adatokat, mint ez véletlenül adhat meg azok, akik csak szükségük a nyomon követési adatok (pl.. egy külső SIEM) az access-ellenőrző adatokat.
- Egy egyetlen, célorientált szolgáltatás Bus vagy esemény központi névtér át az összes diagnosztikai beállítások használata a fentivel azonos okból.
- Korlátozhatja a hozzáférést a kapcsolódó figyelése tárhelyet fiókok vagy esemény hubok külön erőforráscsoport és [hatókört használja,](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) a felügyeleti szerepkörök csak az adott erőforrás csoport való hozzáférés korlátozása tartja.
- Soha nem engedélyt a ListKeys tároló fiókok vagy a esemény hubok előfizetés hatókörét, amikor a felhasználó csak a nyomon követési adatok hozzáférésre van szüksége. Ehelyett, engedélyezhetik ezeket a felhasználónak, erőforrás vagy erőforráscsoport (ha van egy dedikált felügyeleti erőforráscsoport) hatókörét.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Kapcsolódó figyelése tárhelyet fiókok való hozzáférés korlátozása
Ha egy felhasználó vagy az alkalmazás egy tároló fiókban-adatok figyelése hozzáférésre van szüksége, tegye a következőt: [a fiók Társítások létrehozása](https://msdn.microsoft.com/library/azure/mt584140.aspx) a tárterület-fiók blob-tárolóhoz szolgáltatói csak olvasható hozzáféréssel rendelkező felügyeleti adatokat tartalmazó. A PowerShell Ez a következőhöz hasonló lehet:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Ezután adhat a token személyhez, hogy szüksége van olvasható, hogy tárhelyről fiók, és is listára, és az adott tároló fiókban Valamennyi BLOB olvasni.

Azt is megteheti Ha kell ezzel az engedéllyel rendelkező RBAC szabályozhatja, meg lehet adni, hogy a szervezet a fiókhoz társított adott tárolási Microsoft.Storage/storageAccounts/listkeys/action engedéllyel. Ez a felhasználóknak, akiknek engedélyezni szeretné a diagnosztikai beállítás megadásához vagy archiválása profil bejelentkezni a tárterület-fiók szükséges. Ha például úgy lehetett létrehozni, a felhasználó vagy az alkalmazás csak a további tárterület-fiókból egy kell, hogy a következő egyéni RBAC szerepet:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] A ListKeys engedélyt lehetővé teszi, hogy a felhasználónak az elsődleges és másodlagos tároló fiók kulcsok megjelenítse. Billentyűk megadása a felhasználó engedélyeit az összes aláírt (olvasni, írja be, BLOB létrehozása, törlése BLOB, stb.) összes aláírt szolgáltatások (blob, várólista, tábla, fájl) az adott tárterület-fiókban. A fent leírt, amikor csak lehetséges fiók Társítások használata ajánlott.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Esemény figyelése kapcsolatos hubok való hozzáférés korlátozása
Az esemény hubok követheti hasonló mintát, de először egy dedikált meghallgatását engedélyezési szabály létrehozásához szükséges. Ha csak a figyelése kapcsolatos esemény hubok meghallgatása kell, hogy az alkalmazás hozzáférést szeretne, akkor tegye a következőket:

1. Az esemény hub(s) felügyeleti adatfolyam csak meghallgatását követelések létrehozott egy megosztott hozzáférési házirend létrehozása Ez történik a portálon. Ha például akkor lehet, hogy hívja meg "monitoringReadOnly." Ha lehetséges kívánt billentyűre közvetlenül adni a fogyasztói, és ugorja át a következő lépéssel.
2. Ha engedélyezni szeretné a fő alkalmi beszerzése a fogyasztói, adja meg a felhasználót az adott esemény elosztót ListKeys művelet. Ez akkor is szükséges felhasználóknak, akiknek engedélyezni szeretné a diagnosztikai beállítás megadásához vagy esemény hubok adatfolyam profil bejelentkezni. Ha például előfordulhat, hogy hozzon létre egy RBAC szabályt:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Következő lépések
- [További információ: RBAC és az engedélyeket az erőforrás-kezelő](../active-directory/role-based-access-control-what-is.md)
- [Olvassa el a figyelése Azure-ban – áttekintés](monitoring-overview.md)
