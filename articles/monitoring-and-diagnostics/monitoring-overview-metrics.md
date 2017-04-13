<properties
    pageTitle="A Microsoft Azure mértékek áttekintése |} Microsoft Azure"
    description="Mértékek és használatuk Microsoft Azure-ban – áttekintés"
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="ashwink"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Mértékek Microsoft Azure-ban – áttekintés 

Ez a cikk ismerteti a mértékek Mik a Microsoft Azure, juttatások, és hogyan kezdjek hozzá az őket.  

## <a name="what-are-metrics"></a>Mik azok a mértékek?

Azure Monitor lehetővé teszi, hogy felhasználása telemetriai betekintést kap abba, hogy a teljesítmény és a feladatok a Azure állapotának eléréséhez. A legfontosabb típusú Azure telemetriai adatokat a legtöbb Azure erőforrások által kibocsátott (más néven teljesítmény számláló) mértékek. Azure Monitor többféleképpen is állítsa be, és ezek mértékek figyelemmel kísérésére és hibaelhárítási felhasználása.


## <a name="what-can-you-do-with-metrics"></a>Mire használhatók a mértékek?

Mértékek telemetriai értékes forrást, és lehetővé teszi, hogy tegye a következőket:

- A portál diagram metrikus megrajzolásához és rögzítése az irányítópultra a diagram által (például egy virtuális, webhely vagy logikájának alkalmazást) az erőforrás **nyomon követése a teljesítmény** .
- A **probléma értesítést kaphat** az erőforrás teljesítményét érintő, amikor egy mérőszám metszéspontja egy bizonyos küszöbértéket.
- **Konfigurálása automatikus műveletek**, például egy erőforrás autoscaling vagy egy runbook alkalmaznak, amikor egy mérőszám metszéspontja egy bizonyos küszöbértéket.
- **Speciális analytics végrehajtandó** , illetve a erőforrás(ok) használatát és működésére alakulása jelentés.
- **Archiválás** **megfelelőség és naplózás az** adott célra erőforrás állapot és működésére előzményeit.

##  <a name="metric-characteristics"></a>Metrikus jellemzők
Mértékek a következő jellemzőkkel rendelkeznek:

- Az összes mértékek **1 perces gyakoriság**van. Kapott metrikus érték percenként az erőforrás, így valós idejű láthatóság be az állapot és az erőforrás állapotának közelében.
- Mértékek **érhető el, a – kész anélkül, hogy a csatlakozás** vagy beállításáról további diagnosztika.
- Minden egyes metrikus **előzmény 30 nap** érheti el. Gyorsan megtekintheti a legutóbbi és a havi trendek az erőforrás állapotának és működésére.

képes vagy:

- Egyszerűen felderítésére, az access és az **összes mértékek megtekintése** Ha jelöljön ki egy erőforrást, majd rajzolja meg őket a diagramon az Azure portálon keresztül. 
- A mérőszám metszéspontja a beállított küszöbértékét metrikus **szabályt, amely egy értesítést küld, vagy hajt végre műveletet automatikus** konfigurálása Automatikus méretezéssel egy speciális automatikus műveletet, amely lehetővé teszi, hogy méretezése felel meg a bejövő felkérést vagy töltse be a webhely vagy erőforrásokat kiszámítása az erőforrás. Beállíthatja egy Automatikus méretezéssel beállítás szabállyal méretezése blokkolás vagy metsző küszöbértéket metrikával alapján.
- **Archiválás** mértékek tovább, vagy használhatja a csoportokat offline tartalmazó jelentések készítéséhez. A mértékek, hogy az erőforrás diagnosztikai beállítások konfigurálásakor blob-tároló irányíthatja.
- **Adatfolyam** -mértékek egy esemény-hubon keresztül csatlakozott, majd Azure Értékáram-elemzés vagy egyéni alkalmazások közelében valós idejű elemzéshez irányítása úgy. A használatával végezheti el a diagnosztikai beállítások.
- **Útvonal** MOBILE napló Analytics zárolásának feloldásához azonnali analitikát, a szöveg.keres és a mértékek adatainak az erőforrások egyéni értesítés küldése az összes mértékek.
- **Felhasználás** a mérési módja miatt új Azure Monitor REST API-khoz keresztül.
- **Lekérdezés** mértékek a PowerShell-parancsmagok vagy platformok REST API-t.

 ![Az Azure monitoron mértékek továbbítását](./media/monitoring-overview-metrics/MetricsOverview0.png)

## <a name="access-metrics-via-portal"></a>Access-mértékek portálon keresztül
Íme a rövid útmutató az Azure portálon keresztül metrikus diagram létrehozása

### <a name="view-metrics-after-creating-a-resource"></a>Miután létrehozott egy erőforrás nézet mérőszámok
1. Nyissa meg az Azure portál
2. Hozzon létre egy új App - webhelyet.
3. Miután létrehozta a webhelyét, annak a webhelynek az Áttekintés lap megnyitásához.
4. Új mértékek, egy "Figyelés" csempe tekintheti meg. A csempe szerkesztése, és válassza a további metrikus

 ![Mértékek egy erőforrás az Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)    

### <a name="access-all-metrics-in-a-single-place"></a>Hozzáférés az összes mértékek egy helyen
1. Nyissa meg az Azure-portálra 
2. Új Monitor lapján keresse meg és jelölje ki a mértékek lehetőséget annak alapján 
3. A legördülő listából választhat az előfizetését, erőforráscsoport, illetve az erőforrás nevét. 
4. Megtekintheti a rendelkezésre álló mértékek listában. 
5. Jelölje ki a mérőszám érdeklik, és rajzolja meg. 
6. Akkor rögzítheti azt az irányítópult kattintson a jobb felső sarokban kattintson a PIN-kódot.

 ![Az Azure Monitor egy helyen minden mérőszám elérése](./media/monitoring-overview-metrics/MetricsOverview2.png) 


>[AZURE.NOTE] Bármilyen további diagnosztikai beállítások nélkül elérheti host szintű mértékek a VMs (Azure erőforrás-kezelő alapuló) és a virtuális skála beállítása. A host szintű új mértékek Windows és Linux előfordulását érhetők el. Ezek a mértékek nem kell a vendégként való bekapcsolódáshoz-OS szintű mérési módja miatt, ha bekapcsolja az Azure diagnosztika VMs vagy VMSS van hozzáférése az összezavarodik olyan. Azure diagnosztika beállításával kapcsolatos további információért olvassa el a [Mi az Microsoft Azure diagnosztika](../azure-diagnostics.md)című témakört.

## <a name="access-metrics-via-rest-api"></a>Az Access mértékek REST API keresztül
Azure mértékek keresztül Azure Monitor API-khoz is elérhető. Két API-hoz, amelyek segítségével Fedezze fel, és mérőszámok elérése. Használja a: 

- [Azure Monitor metrikus definíciók REST API -t](https://msdn.microsoft.com/library/mt743621.aspx) a rendelkezésre álló szolgáltatáshoz mértékek listáját eléréséhez.
- [Azure Monitor mértékek REST API -t](https://msdn.microsoft.com/library/mt743622.aspx) a mértékek tényleges adatok eléréséhez

>[AZURE.NOTE] Ez a cikk bemutatja az [új API, a mérőszámok](https://msdn.microsoft.com/library/dn931930.aspx) Azure erőforrások keresztül a mérési módja miatt. Az új metrikus definíciók API API verziója 2016-03-01 és mérőszámok API Ez a verzió 2016-09-01. A régi metrikus definíciók és mérőszámok api-ös verziójával 2014-04-01 is elérhető.

További részletes ismertetését megtalálja az Azure Monitor REST API-k használata olvassa el az [Azure Monitor REST API-útmutató](monitoring-rest-api-walkthrough.md)című témakört.

## <a name="export-options-for-metrics"></a>Mértékek exportálási beállításai
Nyissa meg a Monitor lapon a diagnosztikai naplók lap és a mértékek az exportálási beállítások megtekintése. Használati eset korábban a jelen cikkben említett választhat az mértékek (és a diagnosztikai naplók) Blob-tárolóhoz, esemény hubok vagy MOBILE napló Analytics átirányítását. 

 ![Az Azure Monitor mértékek exportálási beállításai](./media/monitoring-overview-metrics/MetricsOverview3.png)   

Erőforrás-kezelő sablonok, [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) vagy [REST API -khoz](https://msdn.microsoft.com/library/dn931943.aspx)keresztül konfigurálhatja. 

## <a name="take-action-on-metrics"></a>Művelet végrehajtása a mértékek
Értesítéseket, vagy az automatikus műveletekhez metrikus adatokat. A figyelmeztetési szabályok vagy Automatikus méretezéssel beállítások ehhez is beállíthatja.

### <a name="alert-rules"></a>A figyelmeztetési szabályok
Beállíthatja a riasztási szabályok mérési módja miatt. Ha egy mérőszám van Metszéspont egy bizonyos küszöbértéket és mailben értesítést szeretne, vagy egy webhook, majd rendszerben olyan egyéni parancsfájl használható fire riasztási szabályok ellenőrizni. A webhook 3 termék integrációs konfigurálása is használhatja.

 ![Mértékek és Azure Monitor riasztási szabályok](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale"></a>Automatikus méretezéssel
Azure források támogatja a több példányával, ha át kívánja méretezni fel- vagy a feladatok kezelésére. Automatikus méretezéssel alkalmazás szolgáltatások (Web apps), a virtuális gép skála beállítása (VMSS) és a klasszikus Cloud Services vonatkozik. Ha át kívánja méretezni fel- vagy, ha egy bizonyos mérőszám érintő a terhelést a megadott küszöbértékét metszéspontja Automatikus méretezéssel szabályok konfigurálása További tudnivalókért olvassa el a [autoscaling áttekintése](monitoring-overview-autoscale.md)című témakört.

 ![Mértékek és Azure monitoron Automatikus méretezéssel](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="supported-services-and-metrics"></a>Támogatott szolgáltatások és mérőszámok
Azure monitorra egy új mértékek infrastruktúra. Támogatást nyújt az Azure-portálra, és az Azure Monitor API az új verzió a következő Azure szolgáltatások:

- VMs (Azure erőforrás-kezelő alapján)
- Virtuális skála beállítása
- Köteg
- Esemény központi névtere 
- Szolgáltatás Bus névtér (prémium verzióban Termékváltozat csak)
- SQL (12 verzió)
- Rugalmas SQL készlet
- Webhelyek
- Webes kiszolgálófarm
- Logika alkalmazások
- IoT hubok
- Gyorsítótár vgx.dll
- Hálózati: Alkalmazás átjárók
- Keresés

Megtekintheti egy részletes listáját a támogatott szolgáltatások és [Azure Monitor mértékek - erőforrás típusonként támogatott mértékek](monitoring-supported-metrics.md), a mérőszámok. 


## <a name="next-steps"></a>Következő lépések

Keresse meg az egész Ez a cikk hivatkozásokat. Ezenkívül további:  

- tudnivalók [a autoscaling közös mértékek](insights-autoscale-common-metrics.md)
- [a figyelmeztetési szabályok](insights-alerts-portal.md) létrehozása




