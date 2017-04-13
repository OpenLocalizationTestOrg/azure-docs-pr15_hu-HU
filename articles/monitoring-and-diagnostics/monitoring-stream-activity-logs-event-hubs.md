<properties
    pageTitle="Adatfolyam-esemény hubok az Azure tevékenység napló |} Microsoft Azure"
    description="Megtudhatja, hogy miként adatfolyam-esemény hubok az Azure tevékenység telefonnaplót."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Adatfolyam-esemény hubok az Azure tevékenység napló
A [**Tevékenységnapló Azure**](./monitoring-overview-activity-logs.md) a is közeli valós időben való a beépített "Exportálása" beállítást használó, a portálon vagy a szolgáltatás Bus szabály azonosítója naplót-profilban az Azure PowerShell-parancsmagok vagy a Azure CLI engedélyezésével alkalmazást folyamatosan lejátszott.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Mire alkalmas a tevékenységnapló és esemény hubok
Íme néhány módszer a továbbított képesség használhatja a tevékenység napló:

- **Naplózás és telemetriai rendszerek külső adatfolyam** – az idő múlásával a folyamatos átvitelű esemény hubok válik az eljárás a tevékenységnapló pipe külső SIEMs be, és jelentkezzen be a analytics-megoldások.
- **Egy egyéni telemetriai és naplózás platform** – Ha akkor már van-e egy custom-built telemetriai platform vagy csak egy épület gondolat, nagyon méretezhető közzététel előfizetés jellegét esemény hubok lehetővé teszi a tevékenységnapló rugalmasan ingest. [Esemény hubok használatával az alábbi globális skála telemetriai platformot Dan Rosanova útmutató című témakörben.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>A tevékenység naplója streaming engedélyezése
Engedélyezheti a folyamatos átvitelű tevékenység naplója programozás útján vagy a portálon keresztül. Mindkét esetben választhat egy szolgáltatás Bus Namespace és a megosztott hozzáférési házirendet, hogy a névtér, és egy esemény hubhoz jön létre, hogy a névtér az első új tevékenységnapló esemény esetén. Ha nem rendelkezik a szolgáltatás Bus Namespace, először hozzon létre egy újat. Ha korábban folyamatosan lejátszott tevékenységet a szolgáltatás Bus Namespace eseményeket, az esemény-központban, a korábban létrehozott használja fel újra. A megosztott access házirend azt határozza meg az adatfolyam mechanizmusa jogosultságai. Ma a folyamatos átvitelű azon egy esemény hubokhoz **kezelése**, **olvasása**és **küldése** engedélyek szükségesek. Hozzon létre, vagy módosíthatja a szolgáltatás Bus Namespace szolgáltatás Bus Namespace megosztott access házirendek az "Konfigurálása" lapon a klasszikus portálon. A folyamatos átvitelű felvenni, a tevékenységnapló napló profil frissítése, a felhasználó a módosítások végrehajtása a ListKey engedéllyel kell rendelkeznie a szolgáltatás Bus engedélyezési szabály.

### <a name="via-azure-portal"></a>Azure portálon keresztül 
1. Nyissa meg a **Tevékenység naplója** lap a portál bal oldalán lévő menü használatával.

    ![Nyissa meg a tevékenység naplója azt a portálon](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kattintson a lap tetején az **Exportálás** gombra.

    ![Exportálás a portál gomb](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. A megjelenő lap a régiók, amelynek meg szeretné adatfolyam-események és a szolgáltatás Bus Namespace, amelyben meg szeretné létrehozni a folyamatos átvitelű ezeket az eseményeket egy esemény központi közül választhat.

    ![Exportálás a tevékenységnapló lap](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Ezek a beállítások mentéséhez a **Mentés** gombra. A beállítások azonnal érvényesek-előfizetéséhez.


### <a name="via-powershell-cmdlets"></a>VIA PowerShell-parancsmagok
Ha már létezik egy napló profilt, először kell, hogy a profil eltávolítása.

1. Használat `Get-AzureRmLogProfile` azonosíthatja, ha létezik napló profil
2. Ha igen, `Remove-AzureRmLogProfile` törölheti.
3. Használat `Set-AzureRmLogProfile` profil létrehozása:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

A szolgáltatás Bus szabály azonosítója egy olyan karakterlánc, a következő formátumban: {szolgáltatás bus erőforrás-azonosító} /authorizationrules/ {kulcs neve}, például 

### <a name="via-azure-cli"></a>Azure CLI keresztül
Ha már létezik egy napló profilt, először kell, hogy a profil eltávolítása.

1. Használat `azure insights logprofile list` azonosíthatja, ha létezik napló profil
2. Ha igen, `azure insights logprofile delete` törölheti.
3. Használat `azure insights logprofile add` profil létrehozása:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

A szolgáltatás Bus szabály azonosítója egy olyan karakterlánc, a következő formátumban: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hogyan használja fel a naplóadatokat származó esemény hubok?
[A tevékenységnapló sémájának érhető el az alábbi](./monitoring-overview-activity-logs.md). Egyes események "rekordok." nevű JSON BLOB tömbje szerepel.

## <a name="next-steps"></a>Következő lépések
- [A tevékenység naplója tárterület-fiókhoz archiválása](./monitoring-archive-activity-log.md)
- [Olvassa el az Azure tevékenységnapló áttekintése](./monitoring-overview-activity-logs.md)
- [A tevékenységnapló esemény alapján értesítés beállítása](./insights-auditlog-to-webhook-email.md)
