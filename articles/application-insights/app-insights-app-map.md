<properties 
    pageTitle="Az alkalmazás az összefüggéseket alkalmazás térkép |} Microsoft Azure" 
    description="Az alkalmazás-összetevők, a függőségeket bemutató vizuális címkézett KPI-k és az értesítések." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Az alkalmazás az összefüggéseket alkalmazás térkép

A [Visual Studio alkalmazás Hírcsatornájában](app-insights-overview.md)az alkalmazás-megfeleltetések még az alkalmazás-összetevők függőség kapcsolatok vizuális formában. Minden összetevő KPI-k például betöltése, teljesítmény, hibák és a riasztásokat, segít bármely összetevője egy teljesítményproblémát vagy a hibát okozó felfedezése jeleníti meg. Kattintással megnyitható keresztül minden összetevő részletesebb diagnosztika, mind az alkalmazás hírcsatornájában, és – ha az alkalmazás használja az Azure szolgáltatás – Azure diagnosztika, például az SQL-adatbázis Advisor ajánlások.

Egyéb diagramok, például egy alkalmazás megfeleltetést az Azure irányítópult, és hol található teljes funkcionalitású is rögzíthet. 

## <a name="open-the-application-map"></a>Nyissa meg az alkalmazás térképen

A térkép megnyitása az alkalmazás az Áttekintés lap:

![Nyissa meg az app térképen](./media/app-insights-app-map/01.png)

![alkalmazás térképen](./media/app-insights-app-map/02.png)

A térkép jeleníti meg:

* Elérhetőség vizsgálat
* Ügyfél egymás összetevőt (figyelni a JavaScript SDK)
* Kiszolgáló egymás-összetevő
* Az ügyfél- és kiszolgálóoldali összetevők függőségek

Bontsa ki, és függőség hivatkozás csoportok összecsukásához:

![összecsukás gomb](./media/app-insights-app-map/03.png)
 
Ha nagyszámú függőségek (SQL, HTTP stb.) azonos típusú, előfordulhat, hogy jelennek meg a csoportosított. 


![csoportosított függőségek](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Direkt problémák

Minden csomópont rendelkezik a megfelelő teljesítménymutatók, például a betöltés, a teljesítmény és a hiba díjak összetevő. 

Figyelmeztetés ikon kiemelése lehetséges problémákat. Narancsszínű figyelmeztetést azt jelenti, hogy hibák összehívásokban, oldal nézetek és a függőség hívásokat. Piros azt jelenti, hogy egy hiba ráta 5 %-nál.


![Hiba ikonok](./media/app-insights-app-map/04.png)

 
Aktív értesítések szintén megjelenítése be: 


![aktív értesítések](./media/app-insights-app-map/05.png)
 
Ha használja az SQL Azure, van-e egy ikon, amely mutatja, hogy mikor vannak arra vonatkozó javaslatokat, hogy miként javíthatja a teljesítményt. 


![Azure ajánlási](./media/app-insights-app-map/06.png)

Kattintson a bármely ikonra további részleteket:


![Azure ajánlási](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnosztikai kattintson keresztül

A csomópontok a térképen mindegyike kínál célzott kattintson ide keresztül diagnosztika. A beállításokat a csomópont típusától függően változnak.

![kiszolgáló beállításai](./media/app-insights-app-map/09.png)

 
Azure-ban tárolt összetevők a megadható többek között közvetlen rájuk mutató hivatkozást.


## <a name="filters-and-time-range"></a>Szűrők és időtartomány

Alapértelmezés szerint a térkép áttekintheti a választott időtartomány elérhető összes adatot. De csak az adott művelet nevek vagy a függőségek, szűrheti.

* Művelet neve: ilyenek lap nézetek és a egymás kérelem kiszolgálótípusok. Ezt a beállítást választja a térkép megjelenítése a KPI csak a kijelölt műveletek a kiszolgáló/ügyfél egymás csomópontot. A függőségeket, ezeket a műveleteket a környezetében nevű mutatja.
* Függőség alap neve: a AJAX böngésző egymás függőségek és a kiszolgálóoldali függések tartalmazó beállítás. Egyéni függőség telemetriai a TrackDependency API-val, jelentést, ha azokat is megjelennek az alábbi. Választhat a Függőségek megjelenítése a térképen. Felhívjuk a figyelmét arra, hogy jelenleg ezt fogja nem szűri a kiszolgáló egymás kérelmeket, vagy az ügyfél egymás lap nézetek.


![Szűrők beállítása](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Szűrők mentése

Ha menteni szeretné a szűrőket, rögzítés [Irányítópult](app-insights-dashboards.md)alakzatot az adatok szűrt nézetére.


![Irányítópult rögzítése](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Visszajelzés

Tájékoztassa a [Visszajelzés küldése – a portál visszajelzés lehetőséget](app-insights-get-dev-support.md).


![MapLink-1 képe](./media/app-insights-app-map/13.png)


