<properties
    pageTitle="Azure a diagnosztikai naplók archiválása |} Microsoft Azure"
    description="Megtudhatja, hogy miként archiválása az Azure a diagnosztikai naplók a hosszú távú adatmegőrzési tárterület-fiókjában."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure a diagnosztikai naplók archiválása
Ebben a cikkben bemutatjuk, hogyan használhatja az Azure portál PowerShell-parancsmagok, CLI vagy REST API-t a [Diagnosztikai naplók Azure](monitoring-overview-of-diagnostic-logs.md) tárterület-fiókjában archiválását. Ezt a lehetőséget akkor lehet hasznos, ha szeretné megőrizni a diagnosztikai naplók egy választható adatmegőrzési szabály naplózási, statikus elemzés vagy biztonsági másolatot a.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt elkezdené,, [Hozzon létre egy tárterület-fiókot](../storage/storage-create-storage-account.md#create-a-storage-account) , amelyhez archiválhatja a diagnosztikai naplók szükséges. Azt javasoljuk, hogy nem használjon más, nem figyelése, benne, hogy a jobb megadhatja, hogy az access-adatok figyelése tárolt adatokat tartalmazó meglévő tároló fiók. Jó helyen jár Ha is archiválás a tevékenységnapló és a diagnosztikai mértékek tárterület-fiókjába, előfordulhat, hogy értelme megőrizni az összes felügyeleti egy központi helyen, hogy a diagnosztikai naplók tárterület-fiók használatával. A használt tárterület-fiókkal általános célú tároló fiókkal, nem egy blob tárterület-fiókkal kell lennie.

## <a name="diagnostic-settings"></a>Diagnosztikai beállítások
A diagnosztikai naplók, az alábbi módszerek bármelyikével archiválja, állítsa az adott erőforrás egy **Diagnosztikai beállítást** . A diagnosztikai beállítása egy erőforrás meghatározza a kategóriába naplózza vannak tárolva vagy lejátszott online és a kimeneti értékeket – tárterület-fiók, illetve az esemény központi. Az egyes napló kategóriák tárterület-fiókjában tárolt események az adatmegőrzési (amely megőrzi a napok számát) is megadja. Adatmegőrzési szabály értéke nulla, az adott napló kategória események tárolódnak végtelen időre szóló (azaz, örökkévalóság). Adatmegőrzési szabály más módon lehet bármely 1 és 2147483647 közötti napok számát. [Olvashatók további tudnivalók az alábbi diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>A diagnosztikai naplók archív a portálon

1. A portálon kattintson az erőforrás lap, az erőforrás, amelyen szeretne engedélyezése a diagnosztikai naplók archiválás be.
2. Az erőforrás-beállítások menü **Figyelés** szakaszában jelölje be a **Diagnosztika**.

    ![Erőforrás-elemcsoportjából figyelése](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. **Tároló fiók exportálása**jelölje be a jelölőnégyzetet, majd jelölje ki a tárterület-fiókját. Tetszés szerint a megőrzése érdekében ezeket a naplókat használatával napok számának megadása a **adatmegőrzési (nap)** csúszkák. A nulla napok visszatartás határozatlan ideig tárolja a naplókat.

    ![A diagnosztikai naplók lap](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Kattintson a **Mentés**gombra.

A diagnosztikai naplók archivált e tárterület-fiókjába, amint az új esemény adatai jön létre.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Archiválás a diagnosztikai naplók keresztül PowerShell-parancsmagok

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| A tulajdonság         | Szükséges | Leírás                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | igen      | Erőforrás-azonosító az erőforrás, amelyen be szeretné állítani a diagnosztikai beállítást.                            |
| StorageAccountId | nem       | Erőforrás-azonosító a tárterület-fiók, amelyhez a diagnosztikai naplók legyen mentve.                          |
| A kategóriák       | nem       | Ahhoz, hogy a napló kategóriák vesszővel elválasztott listája.                                                     |
| Engedélyezve van          | igen      | Logikai érték, jelezve, hogy a diagnosztika engedélyezve vagy az erőforrás letiltása.                  |
| RetentionEnabled | nem       | Jelző, ha az adatmegőrzési szabály engedélyezve van-e az erőforrás logikai érték.                            |
| RetentionInDays  | nem       | Amelynek események kell megtartani, 1 és 2147483647 közötti napok száma. A nulla érték a naplókat végtelen időre szóló tárolja. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>A tevékenységnapló keresztül a platformok CLI archiválása

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| A tulajdonság         | Szükséges | Leírás                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | igen      | Erőforrás-azonosító az erőforrás, amelyen be szeretné állítani a diagnosztikai beállítást.                            |
| storageId        | nem       | Erőforrás-azonosító a tárterület-fiók, amelyhez a diagnosztikai naplók legyen mentve.                          |
| a kategóriák       | nem       | Ahhoz, hogy a napló kategóriák vesszővel elválasztott listája.                                                     |
| engedélyezve van          | igen      | Logikai érték, jelezve, hogy a diagnosztika engedélyezve vagy az erőforrás letiltása.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Archiválás a diagnosztikai naplók keresztül a REST API-val
Hogyan állíthat be egy a Azure Monitor REST API-diagnosztikai beállítás információt a [Lásd: ehhez a dokumentumhoz](https://msdn.microsoft.com/library/azure/dn931931.aspx) .

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>A diagnosztikai naplók tároló fiók séma
Miután állított be archiválás, tároló tároló jön létre a tárterület-fiókot, amint az esemény fordul elő, az egyik engedélyezte a napló kategóriát. A BLOB a tárolóban a diagnosztikai naplókban és a tevékenység naplója hajtsa végre az ugyanazt a formátumot. Ezeket a BLOB felépítése a következő:

> háttérismeretek – naplók-{napló kategórianevet} / resourceId = előfizetések ELEMRE / {előfizetés azonosítója} /RESOURCEGROUPS/ {erőforrásnév csoport} /PROVIDERS/ {provider erőforrásnév} / {írja be az erőforrás} / {erőforrásnév} / y = {négyjegyű numerikus év} / m = {kétjegyű numerikus hónap} / d {kétjegyű numerikus nap} = / h = {hour}/m=00/PT1H.json kétjegyű 24 órás formátumban

Vagy további egyszerűen

> háttérismeretek – naplók-{napló kategórianevet} / resourceId = / {erőforrás-azonosító} / y = {négyjegyű numerikus év} / m = {kétjegyű numerikus hónap} / d {kétjegyű numerikus nap} = / h = {hour}/m=00/PT1H.json kétjegyű 24 órás formátumban

Blob név lehet például:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Minden egyes PT1H.json blob belül az órát blob URL-címben megadott bekövetkezett esemény JSON blob tartalmazza (például h = 12). Bemutató órában események van ellátva a PT1H.json fájl megjelenésükkor. A perc értéket (m = 00) értéke mindig 00, mivel az egyes BLOB óránként megszakadnak a diagnosztikai naplókban események.

A PT1H.json fájlban egyes események tárolja a "rekordok" tömb, a következő formátumban:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elem neve  | Leírás                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| idő          | Ha az esemény hozta létre a megfelelő eseményt kérés végrehajtása az Azure szolgáltatás időbélyeg. |
| resourceId    | Erőforrás-azonosító a probléma által sújtott erőforrás.                                                                       |
| operationName | A művelet neve.                                                                                      |
| kategória      | Log kategória az esemény.                                                                                  |
| Tulajdonságok    | A Set `<Key, Value>` párban (azaz a szótár) leíró az esemény részleteit.                            |

> [AZURE.NOTE] A tulajdonságok és az a tulajdonságokat használatát az erőforrás függően változhat.

## <a name="next-steps"></a>Következő lépések
- [Töltse le a BLOB-analízis](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Esemény hubok adatfolyam diagnosztikai naplók](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [További információ: a diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
