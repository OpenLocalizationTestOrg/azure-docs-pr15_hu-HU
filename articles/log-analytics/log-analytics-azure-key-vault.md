<properties
    pageTitle="A napló Analytics Azure kulcs tárolóból elemre megoldás |} Microsoft Azure"
    description="Használhatja a billentyű-tárolóra Azure megoldást napló Analytics, tekintse át a naplók Azure kulcs tárolóból elemre."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>A napló Analytics Azure kulcs tárolóból elemre (előzetes verzió) megoldás

>[AZURE.NOTE] Ez az [előnézeti megoldás](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Tekintse át az Azure kulcs tárolóra AuditEvent naplók használhatja a napló Analytics a billentyű-tárolóra Azure megoldást.

Azure kulcs tárolóból elemre az események naplózásának engedélyezése Ezek a naplók kerülnek, ahol azok majd lehet indexelni napló Analytics keresésre és elemzése az Azure Blob-tárolóhoz.

## <a name="install-and-configure-the-solution"></a>Telepítse és állítsa be a megoldás

Telepítse és állítsa be a billentyű-tárolóra Azure megoldást használata az alábbi lépéseket:

1.  [Diagnosztikai naplózás kulcs tárolóból elemre a](../key-vault/key-vault-logging.md) információforrások, amelyek a figyelni kívánt engedélyezése
2.  Állítsa be a napló Analytics értelmezése a naplókat blob-tárolóból [JSON fájlok blob-tárolóban lévő](../log-analytics/log-analytics-azure-storage-json.md)leírt eljárással.
3.  Engedélyezze a billentyű-tárolóra Azure megoldást a folyamat [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)ismertetett használatával.  

## <a name="review-azure-key-vault-data-collection-details"></a>Tekintse át a webhelycsoport Adatrészletek Azure kulcs tárolóból elemre

Azure kulcs tárolóra megoldás gyűjti össze a diagnosztikai naplók Azure blob-tárolóhoz az Azure kulcs tárolóból elemre.
Nincs ügynök nem szükséges adatgyűjtés.

A következő táblázat mutatja az adatgyűjtési módszerek és más adatait, hogy hogyan adatgyűjtés az Azure kulcs tárolóból elemre.

| Platform | Közvetlen agent | Rendszerek központ műveletek Manager (SCOM) agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | A webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|Azure|![nem](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![nem](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![igen](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![nem](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![nem](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 perc|

## <a name="use-azure-key-vault"></a>Azure kulcs tárolóra használata

Telepítése után a megoldást, megtekintheti a megfigyelt kulcs tárolókban használatával a **Azure kulcs tárolóból elemre** az idő múlásával állapotok csempe kérelem összefoglalása napló Analytics az **Áttekintés** oldalon.

![Kép: Azure kulcs tárolóra csempe](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Az **Áttekintés** mozaik gombra kattintás után a naplók összegzéseinek megjelenítése, és kattintson a részletek, a következő kategóriák részletezést:

- Adott idő alatt összes kulcs tárolóra művelet mennyisége
- Nem sikerült a művelet kötet adott idő alatt
- Átlagos műveleti várakozási művelet
- A szolgáltatás műveletekhez több, mint 1000 ms viselő műveletek száma és a műveleteket, amelyek több, mint 1000 ms listával minősége

![Azure kulcs tárolóból elemre az irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Azure kulcs tárolóból elemre az irányítópult képe](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Bármely művelethez részletes adatainak megjelenítéséhez

1. Az **Áttekintés** oldalon kattintson az **Azure kulcs tárolóra** csempére.
2. Az **Azure kulcs tárolóra** irányítópulton tekintse át az összegzett adatokat, az egyik rögzítéséhez, és kattintson az egyik megtekintéséhez részletes információt a napló keresés lapon.

    A napló keresési lapok bármelyik eredményeinek megtekintése időt, a részletes adatok és a napló keresési előzmények. Az eredmények szűkítéséhez metszettel szerint is szűrheti.

## <a name="log-analytics-records"></a>Naplóbejegyzések Analytics

A kulcs-tárolóra Azure megoldást elemzi a rekordok, amelyek egy adott típusú **KeyVaults** , amely az Azure diagnosztika [AuditEvent naplók](../key-vault/key-vault-logging.md) begyűjtési.  Ezeket a rekordokat tulajdonságainak szerepelnek, az alábbi táblázatban.  

| A tulajdonság | Leírás |
|:--|:--|
| Típus | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | A kérelmet, aki az ügyfél IP-címe |
| Kategória      | A kulcs tárolóból elemre a naplók AuditEvent értéke egyetlen, érhető el.|
| CorrelationId | Az ügyfél továbbíthatja (kulcs tárolóra) szolgáltatás mellett naplók az ügyféloldali naplókban összehangolására választható GUID. |
| DurationMs | A mennyi ideig tartott a REST API-összehívási ablak ezredmásodperc szolgáltatás. Ez nem tartalmazza a hálózati késés, így előfordulhat, hogy az ügyféloldali mérése időt nem egyezik meg most. |
| HttpStatusCode_d | A kérelem által visszaadott HTTP állapotkód |
| Id_s       | A kérés egyedi azonosító |
| Identity_o | A token lett mutatják be, amikor a REST API-kérés, az Identitáskezelés. Ez általában a "felhasználó", "szolgáltatás egyszerű" vagy "felhasználó + appId" ahogy a kis-és az Azure PowerShell-parancsmag eredő kérelem kombinációja. |
| OperationName      | A művelet [Azure kulcs tárolóra naplózás](../key-vault/key-vault-logging.md) ismertetett módon neve|
| OperationVersion      | Az ügyfél által kért REST API-verzió|
| RemoteIPLatitude | Az ügyfél, aki a kérelmet szélesség |
| RemoteIPLongitude | Az ügyfél, aki a kérelmet hosszúság |
| RemoteIPCountry | Az ügyfél, aki a kérelmet ország  |
| RequestUri_s | A kérés URI |
| Erőforrás   | A fő tárolóra neve |
| ResourceGroup | A fő tárolóra erőforrás csoportja |
| ResourceId | Azure erőforrás-kezelő erőforrás-azonosító. Kulcs tárolóból elemre a naplók ez nem mindig kulcs tárolóra erőforrás-azonosító. |
| ResourceProvider | *MICROSOFT. KEYVAULT* |
| ResultSignature  | HTTP-állapot|
| ResultType      | Eredmény REST API-összehívás|
| SubscriptionId | Az előfizetés, ahol az a billentyűt tárolóból elemre Azure előfizetés azonosítója |


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes Azure kulcs tárolóra adatok megtekintéséhez.
