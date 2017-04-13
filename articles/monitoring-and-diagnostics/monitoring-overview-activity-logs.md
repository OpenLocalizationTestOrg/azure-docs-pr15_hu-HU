<properties
    pageTitle="Az Azure tevékenységnapló áttekintése |} Microsoft Azure"
    description="Megtudhatja, mi az Azure tevékenységnapló és hogyan használhatja azt az Azure előfizetés előforduló események megértéséhez."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Az Azure tevékenységnapló áttekintése
A **Tevékenységnapló Azure** , hogy az előfizetése erőforrásaira végrehajtott műveletek betekintést nyújt a naplózási. A tevékenységnapló volt korábban neve "Naplókat" vagy "Működési naplók," azt jelenti, hogy a előfizetésekhez vezérlő-sík események óta. A tevékenységnapló használ, eldöntheti, a "mi, ki, és mikor" vonatkozó bármelyik írni (helyezése, a BEJEGYZÉST, DELETE) az erőforrások az előfizetése készített. A művelet és az egyéb vonatkozó tulajdonság állapotának is érthető. A tevékenységnapló nem része a Olvasás (GET) műveleteket.

A tevékenységnapló eltér [a diagnosztikai naplók](./monitoring-overview-of-diagnostic-logs.md), amelyek az adott erőforrás által kibocsátott összes naplók. Ezek a naplók adatokkal kapcsolatos műveletek az adott erőforráson helyett a művelet az adott erőforrás.

Események beolvashatja a tevékenység naplója, az Azure portálon CLI, PowerShell-parancsmagok és Azure Monitor REST API-t.

Ez [a videó a tevékenységnapló bemutatása](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz)megtekintése  

## <a name="what-you-can-do-with-the-activity-log"></a>Mire alkalmas a a tevékenység napló
Íme néhány szolgáltatás, amit a tevékenységnapló kapcsolatban:

- A lekérdezés és megtekintéséhez kattintson az **Azure-portálon**.
- Lekérdezés REST API-t, PowerShell-parancsmag és CLI azt.
- [Hozzon létre e-mailbe vagy webhook figyelmeztetést kiváltó tevékenység naplója esemény ki.](./insights-auditlog-to-webhook-email.md)
- [Mentse egy **Tároló fiók** archiválás vagy manuális ellenőrzést](./monitoring-archive-activity-log.md). Az adatmegőrzési idő (nap) **Napló profilok**használatával adhatja meg.
- Elemzéséhez PowerBI a [**PowerBI tartalom csomagot**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/)használ.
- [Adatfolyam- **Esemény központi** meg](./monitoring-stream-activity-logs-event-hubs.md) egy külső szolgáltatásnak vagy egyéni analytics megoldást, például a PowerBI szempontjából.

## <a name="export-the-activity-log-with-log-profiles"></a>A tevékenység naplója napló profillal exportálása
A **Napló profil** szabályozza a tevékenység naplója exportálásának módját. A napló profillal adhatja meg:

- Ha a tevékenység naplója kell-e küldeni (tárterület-fiók vagy esemény hubok)
- Esemény kategóriák (írási, törlés, művelet) kell-e küldeni
- Exportálja azokat a területeket (helyek)
- Mennyi ideig kell megőrizni a tevékenységnapló tárterület-fiókjával – a nulla napok visszatartás azt jelenti, hogy naplók örökkévalóság maradnak. Egyéb esetben a értéke lehet bármely 1 és 2147483647 közötti napok számát. Adatmegőrzési házirendek vannak beállítva, de a naplók tárolása a tárterület-fiók le van tiltva, (például ha csak esemény hubok vagy MOBILE lehetőség ki van jelölve), az adatmegőrzési házirendek esetén nincs hatása.

Ezeket a beállításokat a "Exportálás" beállítás a tevékenységnapló fel a portálon keresztüli beállíthatók. Is lehet konfigurált programozás útján [a Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx)PowerShell-parancsmagok és CLI. Előfizetés csak lehet egy napló profilt.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Az Azure portálon napló profilok konfigurálása
Adatfolyam-esemény-hubon keresztül csatlakozott a tevékenység naplója, vagy a "Exportálás" lehetőséget az Azure-portálon használatával tárolja őket egy tárterület-fiókkal.

1. Nyissa meg a **Tevékenység naplója** lap a portál bal oldalán lévő menü használatával.

    ![Nyissa meg a tevékenység naplója azt a portálon](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kattintson a lap tetején az **Exportálás** gombra.

    ![Exportálás a portál gomb](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. A megjelenő lap közül választhat:  
    - a régiók, amelynek szeretné exportálni események
    - a tárterület-fiókot, amelyhez szeretné menteni a események
    - meg szeretné őrizni az események tárolóban lévő napok száma. Egy beállítást a 0 napok örökkévalóság megőrzi a naplókat.
    - a szolgáltatás Bus Namespace, amelyben meg szeretné létrehozni a folyamatos átvitelű ezeket az eseményeket egy esemény központi.

    ![Exportálás a tevékenységnapló lap](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Ezek a beállítások mentéséhez a **Mentés** gombra. A beállítások azonnal érvényesek-előfizetéséhez.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Azure PowerShell-parancsmagok használata napló profilok konfigurálása
#### <a name="get-existing-log-profile"></a>Napló profillal beszerzése
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Log profil hozzáadása
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| A tulajdonság         | Szükséges | Leírás   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| név             | igen      | A napló profil nevét.                                 |
| StorageAccountId | nem       | Erőforrás-azonosító a tárterület-fiók, amelyhez a tevékenységnapló menti.                         |
| serviceBusRuleId | nem       | A szolgáltatás Bus névtér szolgáltatás Bus szabály azonosítója szeretné, hogy létre esemény hubok van. Egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`. |
| Helyek        | igen      | A régiók, amelynek kívánt tevékenység naplóbeli események összegyűjtése vesszővel elválasztott listája.              |
| RetentionInDays  | igen      | Milyen események kell megtartani, 1 és 2147483647 közötti napok száma. A nulla érték tárolja a naplókat határozatlan ideig (örökkévalóság). |
| A kategóriák       | nem       | Esemény kategóriák, hogy be kell vesszővel tagolt listáját. Lehetséges értékei írási, törlés és művelet.                                 |

#### <a name="remove-a-log-profile"></a>Log profil eltávolítása
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Log profilok használatával az Azure-platformok CLI konfigurálása
#### <a name="get-existing-log-profile"></a>Napló profillal beszerzése
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
A `name` tulajdonság kell lennie a napló profil nevét.

#### <a name="add-a-log-profile"></a>Log profil hozzáadása
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| A tulajdonság         | Szükséges | Leírás   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| név             | igen      | A napló profil nevét.                                 |
| storageId        | nem       | Erőforrás-azonosító a tárterület-fiók, amelyhez a tevékenységnapló menti.                         |
| serviceBusRuleId | nem       | A szolgáltatás Bus névtér szolgáltatás Bus szabály azonosítója szeretné, hogy létre esemény hubok van. Egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`. |
| helyek        | igen      | A régiók, amelynek kívánt tevékenység naplóbeli események összegyűjtése vesszővel elválasztott listája.              |
| retentionInDays  | igen      | Milyen események kell megtartani, 1 és 2147483647 közötti napok száma. A nulla érték tárolja a naplókat határozatlan ideig (örökkévalóság).     |
| a kategóriák       | nem       | Esemény kategóriák, hogy be kell vesszővel tagolt listáját. Lehetséges értékei írási, törlés és művelet.                                 |

#### <a name="remove-a-log-profile"></a>Log profil eltávolítása
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Esemény séma
A tevékenységnapló az egyes események Ez a példa hasonló JSON blob foglalja magában:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Elem neve         | Leírás             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| engedély        | Az esemény RBAC tulajdonságok BLOB. A "teendő", "szerepkör" és "tartomány" Tulajdonságok általában tartalmazza.|
| hívóazonosító               | A felhasználó, aki elvégezte a műveletet, UPN igénylése vagy az elérhetőség alapján SPN igénylése e-mail címe.|
| csatornák             | Az alábbi értékek közül: "Felügyeleti", "Művelet"|
| correlationId        | Általában egy GUID a karakterlánc-formátumban. Esemény, amelyek egy correlationId megosztása ugyanazon uber művelet tartozik.   |
| Leírás          | Statikus szöveggé az esemény leírását.                              |
| eventDataId          | Esemény egyedi azonosítója.    |
| eventSource          | Azure szolgáltatás vagy infrastruktúra által generált az esemény nevét.    |
| httpRequest          | A HTTP-kérés leíró BLOB. Általában tartalmazza a "clientRequestId", "clientIpAddress" és "módszer" (HTTP módszer. For example, ELHELYEZNI).                            |
| szint                | Az esemény szintjét. Az alábbi értékek közül: "Kritikus", "Hiba", "Figyelmeztetés", "Tájékoztatások" és "Részletes"  |
| resourceGroupName    | Az erőforráscsoport a probléma által sújtott erőforrás nevét.               |
| resourceProviderName | A probléma által sújtott erőforrás nevét az erőforrás-szolgáltató             |
| resourceUri          | Erőforrás-azonosító a probléma által sújtott erőforrás.                               |
| operationId          | Egy globálisan egyedi azonosítója, amely megfelel egy lépésben az események között.         |
| operationName        | A művelet neve.  |
| Tulajdonságok           | A Set `<Key, Value>` párban (Ez azt jelenti, hogy egy szótár) leíró az esemény részleteit.                                |
| állapot               | A művelet állapotának leíró karakterlánc. Néhány gyakori értékei a következők: haladás, sikerült, nem sikerült, aktív, a lépések feloldva.                                |
| Részállapot            | Általában a HTTP-állapotkódot a megfelelő többi hívás, de más karakterláncok, például a közös ezeket az értékeket a részállapot leíró is használható: az OK gombra (HTTP állapotkód: 200), létrehozott (HTTP állapotkód: 201), elfogadott (HTTP állapotkód: 202), nem tartalom (HTTP állapotkód: 204), hibás kérés (HTTP állapotkód: 400), nem található (HTTP állapotkód: 404), ütközés (HTTP állapotkód : 409), belső kiszolgálóhiba (HTTP állapotkód: 500), a szolgáltatás nem érhető el (HTTP állapotkód: 503), átjáró időtúllépése (HTTP állapotkód: 504). |
| eventTimestamp       | Ha az esemény hozta létre a megfelelő eseményt kérés végrehajtása az Azure szolgáltatás időbélyeg.     |
| submissionTimestamp  | Időbélyeg, amikor az esemény vált lekérdezésére használható.             |
| subscriptionId       | Azure előfizetés azonosítójával.  |
| nextLink             | Folytatását jelző jogkivonat-ból eredménye a következő készlete, amikor azok megszakadnak több válasz be. A szokásos van szükség, ha több mint 200 rekordok is meg vannak.     |

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók a tevékenységnapló (korábbi nevén naplókat)](../resource-group-audit.md)
- [Adatfolyam-esemény hubok az Azure tevékenység napló](./monitoring-stream-activity-logs-event-hubs.md)
