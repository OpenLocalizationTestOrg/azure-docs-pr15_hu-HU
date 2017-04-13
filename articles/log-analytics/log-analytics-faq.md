<properties
    pageTitle="Jelentkezzen be az analitikai – gyakori kérdések |} Microsoft Azure"
    description="A napló Analytics szolgáltatás kapcsolatos gyakori kérdésekre adott válaszok."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Log Analytics – gyakori kérdések

A Microsoft FAQ felsoroljuk a napló Analytics a Microsoft műveletek Management csomagja (MOBILE) kapcsolatos gyakori kérdésekre. Ha napló Analytics kapcsolatos további kérdésekre, nyissa meg a [vitafórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) , és a kérdéseiket. A közösségi valakinek segít a válaszokat. Ha gyakran ismételt a kérdést, azt ad hozzá, ez a cikk, hogy gyorsan és egyszerűen találhatók.

## <a name="general"></a>Általános

**Q. milyen ellenőrzéseket végzi az Active Directory és megoldások SQL értékelést?**

VÁLASZOK PARANCSRA. Az alábbi lekérdezés összes elvégzett jelenleg leírását jeleníti meg:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Az eredmények majd exportálhatók az Excelbe további véleményezésre.

* *Kérdés: Miért jelenik meg a valami mást, mint a *MOBILE* az SCOM Administration? **

A: függően a milyen frissítés összesítő a SCOM tart *System Center tanácsadó*, *Működési háttérismeretek*vagy *Napló Analytics*jelenhetnek meg egy csomópontot.

A szöveges karakterlánc módosítás *MOBILE* egy felügyeleti csomag, amelyekre szüksége van az importálandó manuálisan is tartalmazni fogja. A megjelenő útmutatást követve a legújabb SCOM frissítés összesítő tudásbáziscikk, és frissítse a MOBILE konzol a legújabb frissítések a *MOBILE* csomópontot a megtekintéséhez.

* *Kérdés: van-e egy *helyszíni* verziójának OMS? **

V: nem. Log Analytics dolgozza fel, és tárolja a nagyon nagy mennyiségű adatot. Egy felhőalapú szolgáltatásba, a napló Analytics el tudja skála szükség esetén és a teljesítmény igénybevételének a környezet elkerülése.

Is éppen egy felhőalapú szolgáltatás azt jelenti, nem kell a napló Analytics infrastruktúra nyilvántartása és működik, és fogadhat gyakori szolgáltatásfrissítések és javításokat.

## <a name="configuration"></a>Konfiguráció
**Q. lehet módosítani a nevét a táblázat/blob-tároló használva, olvassa el az Azure diagnosztika (ÜVEGVATTA)?**  

VÁLASZOK PARANCSRA.  Nem, ez nem lehetséges jelenleg, de az elkövetkező kiadásokban tervezett.

**Q. Mi IP-címek végezze el a MOBILE szolgáltatások használatára? Hogyan biztosítása, hogy a tűzfal csak lehetővé teszi, hogy a MOBILE Services-alapú forgalmat?**  

VÁLASZOK PARANCSRA. A napló Analytics szolgáltatás épül Azure fölött, és a végpontok kap, amelyek a [Microsoft Azure adatközpont IP-tartományok](http://www.microsoft.com/download/details.aspx?id=41653)IP-címei.

Szolgáltatás telepítések történik, mint a MOBILE szolgáltatások tényleges IP-címének módosítása. A DNS-neveket a tűzfalon keresztül engedélyezni a [proxy és tűzfal beállításainak napló Analytics](log-analytics-proxy-firewall.md)ismerteti.

**Q. használni készült ExpressRoute Azure csatlakozhat. A napló Analytics-forgalmat a készült ExpressRoute kapcsolatot használja?**  

VÁLASZOK PARANCSRA. A különböző típusú készült ExpressRoute forgalom [készült ExpressRoute dokumentáció](./expressroute/expressroute-faqs.md#supported-services)ismertetjük.

Log Analytics-alapú forgalmat a nyilvános peering készült ExpressRoute áramkör használja.

**Q. van egy egyszerű, könnyen mód meglévő napló Analytics-munkaterülethez áthelyezése egy másik napló Analytics munkaterület/Azure előfizetést?**  Van még több ügyfél MOBILE munkaterületek, amely azt voltak tesztelése és trialing az Azure-előfizetésben, és azok áthelyezni termelési, helyezze át őket saját Azure/MOBILE előfizetés szeretnénk.  

VÁLASZOK PARANCSRA. A `Move-AzureRmResource` parancsmag szolgálat tájékoztatja Önt váltani egy napló Analytics-munkaterület, valamint az automatizálási fiók egy Azure előfizetés. További tudnivalókért lásd: az [Áthelyezés-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Ez a változás az Azure-portálon is tehetők.

Adatok áthelyezése egy napló Analytics-munkaterületről a másikra nem, vagy a régió tárolt napló Analytics adatainak módosítása.

**Kérdés: hogyan MOBILE hozzáadása SCOM?**

Válasz: a legújabb kumulatív frissítése és kezelése csomagok importálása lehetővé teszi, hogy csatlakozzon SCOM napló Analytics.

Ne feledje, hogy a napló Analytics SCOM kapcsolatot csak letölthető SCOM 2012 SP1 vagy újabb.

**Kérdés: hogyan lehet lehet erősítse meg, hogy ügynökszoftvert tud kommunikálni a napló Analytics?**

Megoldás: Győződjön meg arról, hogy a agent MOBILE kommunikálni, használja a: Vezérlőpult, biztonság és beállítások, **A Microsoft figyelése Agent**.

Az **Azure napló Analytics (MOBILE)** lapon keresse meg a zöld pipa jelzi. Egy zöld pipa ikonra megerősíti, hogy az ügynök nem tud kommunikálni a MOBILE szolgáltatást.

Egy sárga figyelmeztetés ikon azt jelzi, hogy a agent MOBILE kommunikáció problémák merülnek fel. Egy gyakori oka az, a Microsoft figyelése Agent szolgáltatás leállt, és újra kell indítani.

**Kérdés: Hogyan állíthatom le a napló Analytics kommunikáció ügynökszoftvert?**

SCOM, a válasz: a számítógép eltávolítása a MOBILE felügyelt listában. Ez leállítja az adott ügynök SCOM keresztül az összes kommunikációs. Ügynökök MOBILE közvetlenül csatlakozik, a következő módszerekkel is megakadályozhatja őket – MOBILE kommunikáció: Vezérlőpult, biztonság és beállítások, **A Microsoft figyelése Agent**.
**Azure napló Analytics (MOBILE)**, a felsorolt összes munkaterületek eltávolítása.

## <a name="agent-data"></a>Ügynök adatok

**Q. hogy mennyi adatot lehet küldeni a agent keresztül napló Analytics? Van-e a legnagyobb mennyiségű adat egy ügyfél?**  
VÁLASZOK PARANCSRA. Az ingyenes terv egy napi vonalvég 500 MB-os per munkaterület állítja be. A normál és premium tervek nincs korlátozva van feltöltött adatok mennyiségét. Egy felhőalapú szolgáltatásba, mint a MOBILE napló Analytics verziójához automatikus méretezés felfelé fogópont a mennyiségi érkező – ügyfél esetén is napi terabájt.

A napló Analytics agent készült még egy kis helyigénye, és néhány alapvető adatok tömörítés jelent biztosítása érdekében. Ügyfeleink egyik ténylegesen hozzászólás blog az elvégzett azokat a ügynök, és hogyan impressed voltak szemben. Az adatok mennyiségi lehetővé teszi, hogy ügyfelei megoldásokkal függően változik. Az adatok mennyiségi részletes információkat, és a MOBILE áttekintése lap a mozaik elrendezés csoportban a **szintaxis** megoldás darabolását látható.

További információ az alacsony helyigénye a MOBILE Agent kapcsolatos [ügyfél blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) erről.

**Q. mekkora sávszélesség használják a Microsoft adatkezelési ügynök (MMA), amikor adatokat küld a napló Analytics?**

VÁLASZOK PARANCSRA. Sávszélesség, akkor a függvény a küldött adatok mennyiségét. A hálózaton keresztül küldött adatok tömörítve.

**Q. ügynök per küldött mennyi adatok?**

VÁLASZOK PARANCSRA. Ez nagymértékben függ:

- engedélyezte a megoldásokkal
- naplókról és a teljesítmény számláló gyűjtenek száma
- a naplókban adatok mennyisége

A szabad árak réteg beépített több kiszolgálók és nyomtáv tipikus adatait hangerejének jó módszer. Általános használatát **felhasználása** lapon látható.
Futtatható az WireData ügynök számítógépeken láthatja, hogy mennyi adatot küldi a következő lekérdezés használatával:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Következő lépések

- A [napló Analytics – első lépések](log-analytics-get-started.md) további tudnivalók a napló Analytics és get felfelé és futó perc alatt.
