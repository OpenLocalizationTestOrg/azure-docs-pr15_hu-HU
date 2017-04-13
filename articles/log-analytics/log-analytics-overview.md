<properties
   pageTitle="Mi az napló Analytics? | Microsoft Azure"
   description="Log analitikai szolgáltatása a tevékenységek kezelése csomagja (MOBILE), amelyek segítségével összegyűjtése és a felhőben az erőforrások által generált műveleti adatok elemzése és a helyszíni környezetben.  Ebben a cikkben rövid áttekintését, a naplófájl analitikai és a részletes tartalmakra mutató hivatkozásokat különböző összetevői."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Mi az napló Analytics?
Log Analytics a szolgáltatás [Műveletek Management csomagja \(MOBILE\) ](../operations-management-suite/operations-management-suite-overview.md) , amely segít összegyűjtése és a felhőben az erőforrások által generált adatok elemzése és a helyszíni környezetben. Beépített keresési lehetőség és egyéni irányítópultok használatával könnyen elemzése a rekordok milliónyi munkaterhelésekből és fizikai helyük függetlenül kiszolgálók közötti valós idejű háttérismeretek kínál fel.


## <a name="log-analytics-components"></a>Jelentkezzen be a Analytics-összetevők
A napló Analytics központ tárháza a MOBILE, amely a Azure felhőben üzemelteti.  Adatgyűjtés történő tárházba csatlakoztatott adatforrásból konfigurálása adatforrások és megoldások hozzáadása az előfizetéshez.  Adatforrások és megoldások minden létrehozza a különböző bejegyzéstípusok vannak olyan saját tulajdonságait, de továbbra is elemezhető együtt a tárházba lekérdezések.  Ez lehetővé teszi, hogy az azonos eszközök és módszerek használata készült különböző forrásokból által gyűjtött adatokat a különböző típusú.


![MOBILE tárházba](media/log-analytics-overview/overview.png)


Csatlakoztatott adatforrásból, hogy a számítógép és egyéb információforrások, amelyek készítése a napló Analytics által gyűjtött adatokat.  Ez is elhelyezhet a [csatlakoztatott System Center Operations Manager-kezelés csoport](log-analytics-om-agents.md)számítógépekre telepített [Windows](log-analytics-windows-agents.md) és [Linux](log-analytics-linux-agents.md) közvetlenül csatlakozó ügynökök vagy ügynökök miatt.  Log Analytics adatainak [Azure](log-analytics-azure-storage.md)tárhelyről összegyűjtheti.

[Adatforrások](log-analytics-data-sources.md) egyes csatlakoztatott forrásból származó gyűjtött adatok különböző típusú.  Ide tartoznak a események és az [ablakok](log-analytics-data-sources-windows-events.md) és Linux ügynökök kívül forrásokhoz, például [az IIS naplók](log-analytics-data-sources-iis-logs.md)és [egyéni szöveg naplók](log-analytics-data-sources-custom-logs.md) [teljesítményadatokat](log-analytics-data-sources-performance-counters.md) .  Minden egyes adatforrás gyűjtendő, és a konfigurációs automatikusan érkeznek minden csatlakoztatott adatforrás konfigurálása


## <a name="analyzing-log-analytics-data"></a>Log Analytics adatainak elemzése
A napló Analytics interakció többsége a MOBILE portáljába, amely bármilyen böngészőben elindul, és a konfigurációs beállítások hozzáféréssel rendelkező, és több eszközöket kínál elemzése és működjön a gyűjtött adatok keresztül lesz.  [Log keresések](log-analytics-log-searches.md) hol gyűjtött adatok, testre szabhatja a nézetekkel grafikus legértékesebb keresések és [megoldások](log-analytics-add-solutions.md) , amelyek további funkciókat és elemzőeszközök [irányítópultok](log-analytics-dashboards.md) lekérdezések állítja össze a portálon is élvezheti.

![MOBILE portál](media/log-analytics-overview/portal.png)


Log Analytics gyorsan beolvasásához, és a tárat az adatok összesítése lekérdezés szintaxist tartalmaz.  Létrehozhat és mentse a [Napló keresések](log-analytics-log-searches.md) közvetlenül a a MOBILE portálon található adatok elemzése céljából vagy napló keresések futtatta automatikusan hozhat létre az értesítés, ha a lekérdezés eredményét jelzi az fontos feltételt.

![Log keresés](media/log-analytics-overview/log-search.png)

A teljes környezet állapotának grafikus gyors áttekintést ad, mentett naplót keresés megjelenítések is adhat az [Irányítópult](log-analytics-dashboards.md).   

![Irányítópult](media/log-analytics-overview/dashboard.png)

Annak érdekében, hogy a napló Analytics-Ön kívüli adatok elemzése, az adatokat exportálhatja a MOBILE tárat a az eszközöket, például a [Power BI](log-analytics-powerbi.md) és az Excel alkalmazásban.  Egyéni megoldások, amelyek a napló Analytics kihasználhatja vagy más rendszerekből integrálása a [Napló keresési API](log-analytics-log-search-api.md) is is élvezheti.

## <a name="solutions"></a>Megoldások
Megoldások funkciókkal napló Analytics.  Elsősorban a felhőben futtatása és a MOBILE adattárban gyűjtött adatok elemzése a szükséges. Előfordulhat, hogy is meghatározzák új rekordtípusokat összegyűjtve a napló keresések vagy a további felhasználói felületén a megoldást a MOBILE irányítópulton által biztosított elemezheti.  

![A nyomon követés megoldás módosítása](media/log-analytics-overview/change-tracking.png)


Megoldások érhetők el függvényeket számos, és könnyen Tallózás megoldás áll rendelkezésre, és [vegye fel őket a MOBILE munkaterületre](log-analytics-add-solutions.md) a megoldástárból.  Sok fog automatikusan telepíthető, és kezdjen el dolgozni rajta azonnal, míg másokat előfordulhat, hogy az egyes kereséskonfigurációs.

![Megoldástár](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Jelentkezzen be a Analytics-architektúra
Log Analytics telepítésének követelményei minimális, mivel a központi összetevők Azure a felhőben tárolt.  Ide tartoznak a mellett a szolgáltatásokat, amelyek lehetővé teszik egyeztetéséhez és gyűjtött adatok elemzése a tárat.  A portálon, nem szükséges ügyfélprogram nem érhetők el egy tetszőleges böngészővel.

Telepíteni kell az ügynökök beállítása [a Windows](log-analytics-windows-agents.md) és [Linux](log-analytics-linux-agents.md) számítógépeken, de nincs már egy [csatlakoztatott SCOM kezelése csoport](log-analytics-om-agents.md)tagjai számítógépek működéséhez szükséges további ügynök nem.  Adatkezelési kiszolgálók, amelyek továbbítja az adataikat napló Analytics kommunikáció SCOM ügynökök továbbra is.  Néhány megoldás bár igényel, közvetlenül a napló Analytics kommunikálni ügynökök beállítása  A kapcsolati követelmények betartását dokumentációjában találhat minden megoldást határozza meg.

Amikor [napló Analytics regisztrálhat](log-analytics-get-started.md), létrehoz egy MOBILE munkaterülethez.  A saját adatok tárházba, adatforrások és megoldások egy egyedi MOBILE környezet, érdemes elképzelnie a munkaterület. Több munkaterületek hozhatja létre, például gyártási több környezetek támogatására, és tesztelje az előfizetés.

![Jelentkezzen be a Analytics-architektúra](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Következő lépések

- A [napló Analytics ingyenes fiókot regisztrálni](log-analytics-get-started.md) tesztelje a saját környezetben.
- Megtekintheti a különböző [Adatforrásokat](log-analytics-data-sources.md) adatokat szeretne gyűjteni történő MOBILE tárházba érhető el.
- [Tallózással keresse meg a megoldástárban érhető el megoldásokkal](log-analytics-add-solutions.md) napló Analytics funkciókat felvenni.
