<properties
    pageTitle="Log Analytics Azure hálózati Analytics megoldás |} Microsoft Azure"
    description="Az Azure hálózati Analytics megoldás használhatja napló Analytics Azure hálózati biztonsági csoport naplók és Azure alkalmazás átjáró naplók áttekintése."
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
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>A napló Analytics Azure hálózati Analytics (előzetes verzió) megoldás

>[AZURE.NOTE] Ez az [előnézeti megoldás](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Az Azure hálózati Analytics megoldás használhatja napló Analytics Azure alkalmazás átjáró naplók és Azure hálózati biztonsági csoport naplók áttekintése.

Lehetősége van engedélyezni a naplózás az Azure alkalmazás átjáró naplók és Azure hálózati biztonsági csoportokat. Ezek a naplók Blob-tároló, ahol azok majd lehet indexelni napló Analytics keresésre és elemzése kerülnek.

A következő naplók alkalmazás átjárók támogatottak:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

A következő naplók hálózati biztonsági csoportok támogatottak:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Telepítse és állítsa be a megoldás

Kövesse az alábbi lépéseket telepítése és konfigurálása az Azure hálózati Analytics megoldást:

1.  Az erőforrások figyelni kívánt diagnosztikai naplózás engedélyezése:
  + [Alkalmazás átjáró](../application-gateway/application-gateway-diagnostics.md)
  + [Hálózati biztonsági csoport](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Állítsa be a napló Analytics értelmezése a naplókat Blob-tárolóból [JSON fájlok blob-tárolóban lévő](../log-analytics/log-analytics-azure-storage-json.md)leírt eljárással.
3.  Az Azure hálózati Analytics megoldás engedélyezése a folyamat [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)ismertetett használatával.  

Ha nem engedélyezi a diagnosztikai naplózás az adott erőforrástípus, az adott erőforrás irányítópult rögzítéséhez üres lesz.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Tekintse át az Azure hálózati Analytics adatainak webhelycsoport részletei

Az Azure hálózati Analytics megoldás gyűjti össze a diagnosztikai naplók Azure Blob-tárolóhoz Azure alkalmazás átjárók és a hálózati biztonsági csoportokat.
Nincs ügynök nem szükséges adatgyűjtés.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés Azure hálózati elemzéséhez más adatait.

| Platform | Közvetlen agent | Rendszerek központ műveletek Manager (SCOM) agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | A webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|Azure|![nem](./media/log-analytics-azure-networking/oms-bullet-red.png)|![nem](./media/log-analytics-azure-networking/oms-bullet-red.png)|![igen](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![nem](./media/log-analytics-azure-networking/oms-bullet-red.png)|![nem](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 perc|

## <a name="use-azure-networking-analytics"></a>Azure hálózati Analytics használata

Telepítése után a megoldást, megtekintheti az összefoglaló ügyfél, és a megfigyelt alkalmazás átjárók az **Azure hálózati Analytics** használatával kiszolgálói hibák csempe a napló Analytics **áttekintése** lapon.

![Kép: Azure hálózati Analytics csempe](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Az **Áttekintés** mozaik gombra kattintás után a naplók összegzéseinek megjelenítése, és kattintson a részletek, a következő kategóriák részletezést:

+ Alkalmazás átjáró elérése napló
  - Az alkalmazás átjáró access naplók ügyfél- és kiszolgálóoldali hibák
  - Egyes alkalmazások átjáró óránként kérések
  - Nem sikerült kérelmek óránkénti minden alkalmazás átjáró
  - Felhasználói ügynököt alkalmazás átjárók szerinti Hibalista
+ Alkalmazások átjáró teljesítménye
  - A Host állapot alkalmazás átjáró
  - A legnagyobb és 95 PERCENTILIS alkalmazás átjáró sikertelen kérelmek
+ A letiltott flow hálózati biztonsági csoport
  - Letiltott flow hálózati biztonsági csoport szabályokkal
  - A letiltott flow MAC-címek
+ Engedélyezett flow hálózati biztonsági csoport
  - Biztonsági csoport szabályokkal engedélyezett flow hálózati
  - Az engedélyezett flow MAC-címek


![Azure hálózati statisztika irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Azure hálózati statisztika irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Azure hálózati statisztika irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Azure hálózati statisztika irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Bármely napló összefoglalása részleteinek megtekintése

1. Az **Áttekintés** oldalon kattintson az **Azure hálózati Analytics** csempére.
2. Az **Azure hálózati statisztika** irányítópultján tekintse át az összegzett adatokat, az egyik rögzítéséhez, és kattintson az egyik megtekintéséhez részletes információt a napló keresés lapon.

    A napló keresési lapok bármelyik eredményeinek megtekintése időt, a részletes adatok és a napló keresési előzmények. Az eredmények szűkítéséhez metszettel szerint is szűrheti.

## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes Azure hálózati Analytics adatainak megtekintése.
