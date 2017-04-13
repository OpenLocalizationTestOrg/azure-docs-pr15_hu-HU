<properties
    pageTitle="Az Azure tevékenységnapló archiválása |} Microsoft Azure"
    description="Megtudhatja, hogy miként archiválhatja a tárterület-fiókjában hosszú távú adatmegőrzési Azure tevékenység naplót."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Az Azure tevékenységnapló archiválása
Ebben a cikkben bemutatjuk, hogyan használhatja az Azure portál PowerShell-parancsmagok vagy platformok CLI archiválja az [**Azure tevékenységnapló**](monitoring-overview-activity-logs.md) tárterület-fiókjában. Ezt a lehetőséget akkor lehet hasznos, ha szeretné megőrizni a naplózás, a statikus elemzés vagy a biztonsági másolat (ha benne az adatmegőrzési teljes hozzáféréssel) 90 napig hosszabb tevékenységnapló. Ha csak szeretne őrizni az eseményeket 90 napon vagy kisebb, akkor nem kell beállítása archiválás tárterület-fiókjába, mivel a tevékenység naplóbeli események megmaradnak az Azure platform 90 napig engedélyezése archiválás nélkül.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt elkezdené,, [Hozzon létre egy tárterület-fiókot](../storage/storage-create-storage-account.md#create-a-storage-account) , amelyhez archiválhatja a tevékenységnapló szükséges. Azt javasoljuk, hogy nem használjon más, nem figyelése, benne, hogy a jobb megadhatja, hogy az access-adatok figyelése tárolt adatokat tartalmazó meglévő tároló fiók. Jó helyen jár Ha is archiválás a diagnosztikai naplók és mérőszámok tárterület-fiókjába, előfordulhat, hogy értelme megőrizni az összes felügyeleti egy központi helyen, hogy a tevékenységnapló tárterület-fiók használatával. A használt tárterület-fiókkal általános célú tároló fiókkal, nem egy blob tárterület-fiókkal kell lennie.

## <a name="log-profile"></a>Log profil
A tevékenység naplója, az alábbi módszerek bármelyikével archiválja, állítsa a **Napló profil** előfizetéshez. A napló profillal határozza meg, milyen események tárolt vagy lejátszott online és a kimeneti értékeket – tárterület-fiók, illetve az esemény központi. Tárterület-fiókjában tárolt események az adatmegőrzési (amely megőrzi a napok számát) is megadja. Az adatmegőrzési házirend értéke nulla, események határozatlan ideig tárolódnak. Egyéb esetben e beállítás értéke 1 és 2147483647 között. [Olvashatók további napló ismertetése profilok itt](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>A tevékenység naplója a portálon archiválása
1. Az-portálon kattintson a bal oldali navigációs sávon a **Tevékenység naplója** hivatkozásra. Ha a hivatkozás nem látható a tevékenység napló, hivatkozásra **További szolgáltatások** először.

    ![Nyissa meg azt a tevékenységnapló lap](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. A lap tetején kattintson az **Exportálás**parancsra.

    ![Kattintson az Exportálás gombra.](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. A megjelenő lap **exportálása egy tároló fiókba** jelölje be a jelölőnégyzetet, és jelölje ki a tárterület-fiókját.

    ![A tároló fiókot beállítani](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Használja a csúszka vagy a szövegdobozt, meghatározása, amelyhez a tárterület-fiókjában tevékenység naplóbeli események őrizze napok számát. Ha szeretné, hogy inkább az adatok állandó tároló fiók határozatlan ideig, a szám nulla értékűre beállított.
5. Kattintson a **Mentés**gombra.

## <a name="archive-the-activity-log-via-powershell"></a>A tevékenységnapló PowerShell archiválása
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| A tulajdonság         | Szükséges | Leírás                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | nem       | Erőforrás-azonosító tevékenység naplók mentésére tárterület-fiók.                                                                                                                                                                                                                        |
| Helyek        | igen      | A régiók, amelynek kívánt tevékenység naplóbeli események összegyűjtése vesszővel elválasztott listája. Megtekintheti az összes terület [megtalálhatók az ezen az oldalon](https://azure.microsoft.com/en-us/regions) , vagy [Az Azure-kezelés REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)segítségével lista. |
| RetentionInDays  | igen      | Milyen események kell megtartani, 1 és 2147483647 közötti napok száma. A nulla érték tárolja a naplókat határozatlan ideig (örökkévalóság).                                                                                                                                                                                             |
| A kategóriák       | igen      | Esemény kategóriák, hogy be kell vesszővel tagolt listáját. Lehetséges értékei írási, törlés és művelet.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>A tevékenységnapló keresztül CLI archiválása
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| A tulajdonság        | Szükséges | Leírás                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| név            | igen      | A napló profil nevét.                                                                                                                                                                                                                                                                         |
| storageId       | nem       | Erőforrás-azonosító tevékenység naplók mentésére tárterület-fiók.                                                                                                                                                                                                                        |
| helyek       | igen      | A régiók, amelynek kívánt tevékenység naplóbeli események összegyűjtése vesszővel elválasztott listája. Megtekintheti az összes terület [megtalálhatók az ezen az oldalon](https://azure.microsoft.com/en-us/regions) , vagy [Az Azure-kezelés REST API](https://msdn.microsoft.com/library/azure/gg441293.aspx)segítségével lista. |
| retentionInDays | igen      | Milyen események kell megtartani, 1 és 2147483647 közötti napok száma. A nulla érték határozatlan ideig fog tárolni a naplókat (örökkévalóság).                                                                                                                                                                                             |
| a kategóriák      | igen      | Esemény kategóriák, hogy be kell vesszővel tagolt listáját. Lehetséges értékei írási, törlés és művelet.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>A tevékenységnapló tároló séma
Állított be archiválás, miután tároló tároló létrejön a tárterület-fiókjában, amint tevékenységnapló esemény fordul elő. A tárolóban a BLOB tevékenységnapló és a diagnosztikai naplók hajtsa végre az ugyanazt a formátumot. Ezeket a BLOB felépítése a következő:

> háttérismeretek-műveleti-naplók/neve alapértelmezett/resourceId = / ELŐFIZETÉSEK = / {előfizetés azonosítója} / y = {négyjegyű numerikus év} / m = {kétjegyű numerikus hónap} / d = {kétjegyű numerikus napja} / h = {hour}/m=00/PT1H.json kétjegyű 24 órás formátumban

Blob név lehet például:

> insights-Operational-Logs/Name=default/resourceId=/Subscriptions/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Minden egyes PT1H.json blob belül az órát blob URL-címben megadott bekövetkezett esemény JSON blob tartalmazza (például h = 12). Bemutató órában események van ellátva a PT1H.json fájl megjelenésükkor. A perc értéket (m = 00) értéke mindig 00, mivel az egyes BLOB óránként megszakadnak a tevékenység naplóbeli események.

A PT1H.json fájlban egyes események tárolja a "rekordok" tömb, a következő formátumban:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
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
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Elem neve    | Leírás                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| idő            | Ha az esemény hozta létre a megfelelő eseményt kérés végrehajtása az Azure szolgáltatás időbélyeg.    |
| resourceId      | Erőforrás-azonosító a probléma által sújtott erőforrás.                                                                          |
| operationName   | A művelet neve.                                                                                         |
| kategória        | A művelet kategória pl.. Írás olvasása, a műveletet.                                                               |
| resultType      | Az eredmény típusa pl.. Hiba sikeres indítása                                                            |
| resultSignature | Az erőforrás típusa függ.                                                                                  |
| durationMs      | A művelet ezredmásodpercben időtartama                                                                      |
| callerIpAddress | A felhasználó, aki elvégezte a műveletet, UPN igénylése vagy az elérhetőség alapján SPN igénylése IP-címe.         |
| correlationId   | Általában egy GUID a karakterlánc-formátumban. Esemény, amelyek egy correlationId megosztása ugyanazon uber művelet tartozik.         |
| azonosító        | A hitelesítés és követelések leíró JSON blob.                                                             |
| engedély   | Az esemény RBAC tulajdonságok BLOB. A "teendő", "szerepkör" és "tartomány" Tulajdonságok általában tartalmazza.            |
| szint           | Az esemény szintjét. Az alábbi értékek közül: "Kritikus", "Hiba", "Figyelmeztetés", "Tájékoztatások" és "Részletes" |
| hely        | Régió, a hely történt (vagy globális).                                                             |
| Tulajdonságok      | A Set `<Key, Value>` párban (azaz a szótár) leíró az esemény részleteit.                             |

> [AZURE.NOTE] A tulajdonságok és az a tulajdonságokat használatát az erőforrás függően változhat.

## <a name="next-steps"></a>Következő lépések
- [Töltse le a BLOB-analízis](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Adatfolyam-esemény hubok a tevékenység napló](monitoring-stream-activity-logs-event-hubs.md)
- [További információ: a tevékenység napló](monitoring-overview-activity-logs.md)
