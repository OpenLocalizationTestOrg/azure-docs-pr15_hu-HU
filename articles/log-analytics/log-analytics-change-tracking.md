<properties
    pageTitle="Változások követése a napló Analytics megoldás |} Microsoft Azure"
    description="Használhatja a konfigurációs változások követése megoldást napló Analytics Észreveheti, egyszerűen szoftverek és a Windows-szolgáltatások változásokról a környezetben – a módosításokat azonosító segíthetnek pinpoint működési problémák."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>A nyomon követés megoldás napló Analytics módosítása


Használhatja a konfigurációs változások követése megoldást napló Analytics Észreveheti, egyszerűen szoftver és a Windows-szolgáltatások és Linux démon változásokat a környezetben előforduló – konfigurációs módosítások azonosító segítséget nyújtanak a működési problémák pinpoint.

A megoldást is érintheti ügynök típusú frissítése telepítését. Módosításainak szoftver telepítése és Windows-szolgáltatások felügyelt kiszolgálókon olvasnak, és kattintson az adatokat a rendszer elküldi a napló Analytics szolgáltatás feldolgozásra a felhőben. Logika a kapott adatokon alkalmazott, és a felhőbeli szolgáltatástól rekordok az adatokat. Ha módosításokat talál, a változások követése irányítópulton kiszolgálók módosításokkal jelennek meg. A változások követése irányítópult információk segítségével egyszerűen megtekintheti a kiszolgáló infrastruktúra a módosításait.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- Operations Manager szükség a változások követése megoldás.
- A Windows vagy az Operations Manager ügynök összes olyan számítógépen, amelyre változások követésére kell rendelkeznie.
- A változások követése megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.


## <a name="change-tracking-data-collection-details"></a>Nyomon követési adatok gyűjtése részleteinek módosítása

A változások követésének gyűjt a szoftver készlet és a Windows-szolgáltatás metaadat-alapú az, hogy engedélyezte a ügynökök használatával.

A következő táblázat mutatja az adatgyűjtési módszerek és más adatait, hogy hogyan adatgyűjtés a változások nyomon követését.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![igen](./media/log-analytics-change-tracking/oms-bullet-green.png)|![igen](./media/log-analytics-change-tracking/oms-bullet-green.png)|![nem](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![nem](./media/log-analytics-change-tracking/oms-bullet-red.png)|![igen](./media/log-analytics-change-tracking/oms-bullet-green.png)| óránkénti|

## <a name="use-change-tracking"></a>Változások követésének használata

A megoldás telepítése után megtekintheti a módosítások összegzése a megfigyelt kiszolgálók a **Változások követése** csempére az **Áttekintés** lapon, a MOBILE használatával.

![Változások követése csempe képe](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Változások a infrastruktúra, és a következő kategóriákból-feltárás részletek tekintheti meg:

- Konfigurációs módosításait írja be a szoftverek és -szolgáltatások Windows
- Alkalmazások és az egyes kiszolgálókon frissítések szoftver módosítása
- Az egyes alkalmazások szoftver módosítások száma
- Az egyes kiszolgálókon Windows szolgáltatás módosítása

![Változások követése irányítópult képe](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Változások követése irányítópult képe](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Megtekintése bármely módosításokat a típus módosítása

1. Az **Áttekintés** oldalon kattintson a **Változások követése** csempére.
2. A **Követés módosítása** irányítópulton tekintse át az összegzett adatokat, az egyik típus módosítása rögzítéséhez, és kattintson az egyik részletes információt a **napló keresési** lap megtekintéséhez.
3. A napló keresési lapok bármelyik eredményeinek megtekintése időt, a részletes adatok és a napló keresési előzmények. Az eredmények szűkítéséhez metszettel szerint is szűrheti.

## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes változáskövetési adatok megtekintése.
