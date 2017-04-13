<properties
    pageTitle="A Microsoft Azure figyelése áttekintése |} Microsoft Azure"
    description="Felső szintű figyelő és áttekintése diagnosztika, beleértve a riasztások, webhooks, Automatikus méretezéssel és az egyéb Microsoft Azure-ban."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"l
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="robb"/>

# <a name="overview-of-monitoring-in-microsoft-azure"></a>A Microsoft Azure figyelése áttekintése

Ez a cikk megfigyelés Azure erőforrások fogalmi áttekintést nyújt. Adott típusú erőforrás mutat hivatkozás információt nyújt.  Az alkalmazás nem Azure szempontjából, figyelése magas szintű információkat lásd: a [Figyelés és diagnosztika útmutatást](../best-practices-monitoring.md).

Felhőalapú alkalmazások olyan sok mozgó részből áll a bonyolult. Figyelése annak érdekében, hogy az alkalmazás mentése maradjon adatok és a megfelelő állapotban futó biztosít. Segít, hogy ki a potenciális problémákról stave vagy korábbi szegélyei kapcsolatos hibák elhárítása. Ezenkívül mély háttérismeretek szerezhet a alkalmazásról felügyeleti adatokat is használhatja. Arra, hogy a könnyíthető meg az alkalmazás teljesítményének vagy karbantartási követelmények javítása, illetve a műveleteket, amelyeket egyébként beavatkozás automatizálása.

Az alábbi ábra mutatja sematikus ábrázolása Azure felügyeletet igényel, beleértve a naplók összegyűjtheti, és mit tehet az adatok típusát.   

![Logikai modell figyelemmel kísérésére és diagnosztika nem számítási erőforrások](./media/monitoring-overview/MonitoringAzureResources-non-compute_v3.png)

Ábra 1: Elvi modell figyelemmel kísérésére és diagnosztika nem számítási erőforrások

<br/>

![Logikai modell figyelemmel kísérésére és a számítási erőforrások diagnosztika](./media/monitoring-overview/MonitoringAzureResources-compute_v3.png)

Ábra 2: Elvi modell figyelemmel kísérésére és a számítási erőforrások diagnosztika


## <a name="monitoring-sources"></a>Adatforrások figyelése
### <a name="activity-logs"></a>Tevékenység naplók
A tevékenységnapló (korábbi nevén működési és naplókban) kereshet adatokat, az erőforrásról az Azure infrastruktúra naplójában. A napló időpontok erőforrások létrehozott vagy megsemmisíteni, ha például információkat tartalmazza.  

### <a name="host-vm"></a>A Host virtuális
**Csak a számítási**


Néhány kiszámítania erőforrások, mint a Felhőszolgáltatások virtuális gépeken futó, és szolgáltatás háló van egy dedikált Host virtuális őket másokkal. A Host virtuális megegyezik a legfelső szintű virtuális a Hyper-V hipervizor modell. Ebben az esetben mértékek összegyűjtheti csak a mellett a vendégként való bekapcsolódáshoz OS Host virtuális.  

Más Azure szolgáltatásokhoz nem áll feltétlenül között az erőforrás- és egy adott Host virtuális 1:1 megfeleltetés így host virtuális mértékek nem érhetők el.


### <a name="resource---metrics-and-diagnostics-logs"></a>Erőforrás - mértékek és a diagnosztikai naplók
Az erőforrás típusa inputra mértékek függ. Ha például virtuális gépeken futó statisztikai adatokkal szolgál a lemez IO és százalékos Processzor. De ezeket a stat nem léteznek, hogy a szolgáltatás Bus várólista, helyette a mértékek, például várólista méretét és az üzenet átviteli nyújt.

Számítási erőforrások szerezhet be mértékek a vendégként való bekapcsolódáshoz-OS és diagnosztika modulok Azure diagnosztika jelennek meg. Azure diagnosztika segít összegyűjtése és az útvonal diagnosztikai adatok más helyekre, többek között az Azure tároló gyűjtse össze.

Jelenleg inputra mértékek listáját a [támogatott mértékek](monitoring-supported-metrics.md)érhető el.

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Alkalmazás – a diagnosztikai naplók alkalmazás naplók és mérőszámok
**Csak a számítási**

Alkalmazások futtatását is lehetővé teszi, a számítási modell a vendégként való bekapcsolódáshoz OS fölött. Saját beállítása naplók és mérőszámok elhelyezése őket.

Mértékek típusai

- Teljesítmény számláló
- Alkalmazás naplók
- Windows-eseménynaplók
- .NET-forrás
- IIS naplók
- Jegyzék alapú esemény-nyomkövetés
- Kiírása összeomlik
- A hibanaplók ügyfél


## <a name="uses-for-monitoring-data"></a>Használja-e-adatok figyelése

### <a name="visualize"></a>Képi megjelenítés
Az ellenőrző adatok a képeket és a diagramok megjelenítése segít megtalálni trendek távol gyorsabban szeretne magának adatai között.  

Néhány képi megjelenítés módszerek a következők:

- Az Azure portál használata
- Azure alkalmazás mélyebb útvonal adatok
- A Microsoft PowerBI útvonal adatok
- Az adatok átirányítása az élő streaming használatával külső képi megjelenítés eszköz, vagy úgy, hogy az eszköz olvassa el az Azure tároló archívumból

### <a name="archive"></a>Archiválás
Nyomon követési adatok általában Azure tárolóhoz írt, és ott tartott, amíg nem törli azokat.

Néhány módszert, amellyel az adatok:

- Miután írt, akkor is egyéb eszközök belüli vagy kívüli Azure elolvasni és dolgozza fel.
- Letöltése helyi meghajtóra egy helyi archiválás adatait, vagy módosíthatja az adatmegőrzési szabály adatok megtartása hosszabb ideig a felhőben.  
- Akkor határozatlan ideig az adatok Azure tároló, hogy ki kell tartsa adatok mennyisége alapján Azure tárterületet vásárolni.
-

### <a name="query"></a>Lekérdezés
Az Azure Monitor REST API kereszt platform parancssori felület (CLI) parancsokat, PowerShell-parancsmagok vagy a .NET SDK segítségével elérni az adatokat a rendszer vagy Azure tárhely

Példák:

-  Adatok beolvasása írt egyéni felügyeleti alkalmazáshoz
-  Egyéni lekérdezések létrehozásával és adatokat küld egy harmadik fél alkalmazást.

### <a name="route"></a>Továbbítására
Valós idejű más helyekre felügyeleti adatok is adatfolyamként.

Példák:

- Küldése alkalmazás mélyebb, hogy használni tudja a képi megjelenítések eszközök van.
- Küldje el az esemény hubok, hogy a külső eszközök valós idejű elemzés elvégzéséhez irányíthatja.

### <a name="automate"></a>Automatizálása
Használhatja a kiváltó események nyomon adatokat, vagy akár teljes folyamatok többek között:

- Adatok használata Automatikus méretezéssel számítási példányaiban felfelé vagy lefelé alkalmazás betöltés alapján.
- Küldje el e-mailben, ha a egy mérőszám metszéspontja egy előre megadott küszöbértékét.
- Egy webhely URL-CÍMÉT (webhook) művelet végrehajtása az Azure-Ön kívüli rendszerben hívása
- Nyissa meg a runbook Azure automatizálási bármely különböző műveletek elvégzéséhez a

## <a name="methods-of-use"></a>Használati módjai
Az általános adatok nyomon követése, továbbítás és az alábbi módszerek egyikével lekérés is kezelheti. Nem minden módszerek állnak rendelkezésre minden műveletek és adattípusok.

- [Azure portál](https://portal.azure.com)
- [A PowerShell](insights-powershell-samples.md)  
- [Platformok parancssor (CLI)](insights-cli-samples.md)
- [REST API-VAL](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Azure-féle figyelése ajánlataiban
Azure tartalmaz a gépi infrastruktúrájának szolgáltatásai az alkalmazás telemetriai kíséréséhez használható szeretne rendelni. A legjobb felügyeleti stratégia egyesíti mindhárom a szolgáltatások állapotát jelző átfogó bepillantást való használatát.

- [Azure Monitor](http://aka.ms/azmondocs) – ajánlatok képi megjelenítés, lekérdezés, továbbítása, riasztási, Automatikus méretezéssel és mind az Azure infrastruktúra (tevékenység naplója), és minden egyes Azure erőforrás (a diagnosztikai naplók) adatokon automatizálási. Ez a cikk az Azure Monitor dokumentáció része. Azure Monitor neve szeptember 25, Ignite 2016 jelent meg.  A korábbi név lett "Azure háttérismeretek."  
- [Alkalmazás háttérismeretek](https://azure.microsoft.com/documentation/services/application-insights/) – az alkalmazás rétegben a szolgáltatás fölött Azure figyelése származó adatok integrálhatók problémák gazdag észlelése és diagnosztika biztosít. Az alapértelmezett diagnosztika platform alkalmazás szolgáltatás Web Apps alkalmazások.  Adatait más szolgáltatások irányíthatja.  
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) részét [Műveletek Management csomagja](https://www.microsoft.com/cloud-platform/operations-management-suite) – egy holisztikus management levelezőprogramból nyújt a helyszíni és a harmadik fél felhőalapú infrastruktúrájának (például AWS) Azure erőforrások mellett.  Azure Monitor adatainak továbbíthatók közvetlenül napló Analytics úgy is látható a mértékek és a naplókat egy helyen teljes környezetben.     


## <a name="next-steps"></a>Következő lépések
További tudnivalók:

- [Azure Monitor videókban elhelyezett Ignite 2016-ban](https://myignite.microsoft.com/videos/4977)
- [Azure Monitor – első lépések](monitoring-get-started.md)
- [Azure diagnosztika](../azure-diagnostics.md) , ha a Felhőbeli szolgáltatástól, virtuális gép vagy szolgáltatás háló alkalmazás problémáinak diagnosztizálása kísérel meg.
- [Alkalmazás Hírcsatornájában](https://azure.microsoft.com/documentation/services/application-insights/) , ha az alkalmazás szolgáltatás webalkalmazást próbál diagnosztikai problémákat.
- [Azure tároló hibaelhárítási](../storage/storage-e2e-troubleshooting.md) tároló BLOB, táblázatot vagy sorban várakozó használata esetén
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és a [Műveletek Management programcsomagban](https://www.microsoft.com/cloud-platform/operations-management-suite)
