<properties
    pageTitle="Csökkenti a web app alkalmazás szolgáltatás teljesítményét |} Microsoft Azure"
    description="Ez a cikk segít Azure alkalmazás szolgáltatás lassú web app teljesítményét érintő hibák elhárítása."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app teljesítmény elérése érdekében lassú alkalmazást, lassú alkalmazás"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Azure alkalmazás szolgáltatás lassú web app teljesítménnyel kapcsolatos problémák elhárítása

Ez a cikk segít [Azure alkalmazás](http://go.microsoft.com/fwlink/?LinkId=529714)szolgáltatás lassú web app teljesítménnyel kapcsolatos problémák elhárítása.

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet az Azure szakértői [az MSDN Azure és a túlcsordulás Papírhalom fórumokon](https://azure.microsoft.com/support/forums/). Másik lehetőségként az Azure támogatási kérelmeiket is küldhet. Nyissa meg a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , és a **Get-támogatást**gombra.

## <a name="symptom"></a>A jelenség

Amikor, tallózással keresse meg a web app, a lapok betöltés lassan és néha időtúllépési.

## <a name="cause"></a>OK

Ez a probléma gyakran okozza alkalmazás szintű problémák, például:

-   túl hosszú ideig tart kérések
-   használó magas memória/Processzor
-   az alkalmazás összeomlik, kivétel következtében.

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

Hibaelhárítás osztható három különböző tevékenységeket, egymás után következő sorrendben:

1.  [Megfigyelheti, és figyelemmel követheti az alkalmazás működése](#observe)
2.  [Adatgyűjtés](#collect)
3.  [A probléma csökkentésében](#mitigate)

[Alkalmazás szolgáltatás Web Apps alkalmazásokban](/services/app-service/web/) is megadhatja különböző lépésről lépésre.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. a megfigyelheti és figyelemmel követheti az alkalmazás működése

#### <a name="track-service-health"></a>Szolgáltatásállapot nyomon követése

Microsoft Azure publicizes minden alkalommal, amikor egy szolgáltatás megszakítása és a teljesítmény csökkenés van. A szolgáltatás állapotát jelző követheti nyomon az [Azure-portálon](https://portal.azure.com/). További tudnivalókért olvassa el a [szolgáltatásállapot nyomon követése](../monitoring-and-diagnostics/insights-service-health.md)című témakört.

#### <a name="monitor-your-web-app"></a>Figyelheti a web App alkalmazásban

Ez a beállítás lehetővé teszi annak megállapítása, ha az alkalmazás hibát tapasztal problémákat. Az a web App alkalmazásban a lap kattintson a **kérelem és hibák** csempére. A **metrikus** lap kell követnie a is hozzáadhat mérési módja miatt.

A mértékek, érdemes lehet figyelni a webalkalmazás vannak

-   Átlagos memória-munkakészlet
-   Átlagos válaszidő
-   Processzor idő
-   Memória-Munkakészlet
-   Kérések

![web app teljesítmény figyelését](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

További tudnivalókért lásd:

-   [Webhely alkalmazásainak figyelése Azure App szolgáltatásban](web-sites-monitor.md)
-   [Értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Webes végpont állapota figyelése

Ha a rendszerben a web App alkalmazásban a **szokásos** réteg árak Web Apps lehetővé teszi, hogy figyelheti a 2, 3 földrajzi helyek a végpontok.

Végpont figyelése konfigurálása webes azt vizsgálja, amely ellenőrzi a válasz a munkaidő és a webes URL-címek felmérést geo elosztott helyről. A tesztet a webes URL-címe határozza meg a válaszidő és üzemidőt minden olyan helyen, a HTTP GET műveletet hajt végre. Minden olyan helyen, konfigurált egy tesztcélú öt percenként fut.

Üzemidőt van ellenőrizni a HTTP-válasz kódokat használja, és a válasz idő ezredmásodpercben. A felügyeleti teszt sikertelen, ha a HTTP-válasz kód nagyobb vagy egyenlő 400, vagy ha a válasz legfeljebb 30 másodperc vesz igénybe. Zárólap érhető el, ha a megfigyeléssel teszteket a megadott helyekről sikeres számít.

A beállítás lásd: [hogyan: webes végpont állapot ellenőrzését](web-sites-monitor.md#webendpointstatus).

[Azure webhely megtartásáról be és végpont figyelés - lengyel Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) is meg egy videót arról, hogy végpont figyelése.

#### <a name="application-performance-monitoring-using-extensions"></a>Alkalmazás teljesítményét figyelve Extensions használata

Az alkalmazás teljesítményének is figyelheti a _webhely bővítmények_használata révén.

Minden alkalmazás szolgáltatás webalkalmazás egy bővíthető management végpont, amely lehetővé teszi, használhatja a webhely bővítmények telepítése egy hatékony eszközkészletet biztosít. Ezek eszközök tartomány a [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) hasonló kódot szerkesztők felügyeleti eszközök csatlakoztatott erőforrások, például a MySQL-adatbázishoz csatlakoztatva webalkalmazást.

[Azure alkalmazás Hírcsatornájában](/services/application-insights/) , és [Új Relic](/marketplace/partners/newrelic/newrelic/) két a teljesítmény figyelése a rendelkezésre álló bővítmények webhelyen. Új Relic használatához telepítenie egy ügynök futásidőben. Azure alkalmazás Hírcsatornájában használatához, a kód egy SDK újjáépítése, és további adatokat hozzáférést biztosító bővítménye is telepítheti. A SDK lehetővé teszi a Lync használatának és részletesebben az alkalmazás teljesítményének kódírás.

Alkalmazás mélyebb, olvassa el [a webalkalmazások teljesítményének figyelése](../application-insights/app-insights-web-monitor-performance.md).

Új Relic használatához olvassa el az [Új Relic teljesítmény Alkalmazáskezelés a Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. a adatok gyűjtése

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>A web App a diagnosztikai naplózás engedélyezése

A Web Apps alkalmazások környezet naplózási adatait az érintett webkiszolgálóra, mind a webalkalmazás diagnosztikai funkciókat tartalmaz. Ezek logikailag oszthatók webhely kiszolgálói diagnosztika és az alkalmazás diagnosztika.

##### <a name="web-server-diagnostics"></a>Webhely kiszolgálói diagnosztika

Is engedélyezése vagy letiltása a következő típusú naplók:

-   A **Hiba részletes naplózás** - HTTP állapot kódok (állapotkód 400-s vagy újabb) hibát jelző hiba részletes adatait. Határozza meg, miért a kiszolgáló hibakódot a segítő információk állhat.
-   **Kérés nyomkövetési sikertelen volt** - sikertelen kérelmek, beleértve a nyomkövetési segítségével dolgozza fel a kérést, és az egyes összetevőknek eltelt idő az IIS összetevő részletes tájékoztatást. Ez akkor lehet hasznos, ha a beadni kívánt web app teljesítmény javítása, illetve mi okozza a HTTP hibaüzenet elkülönítése.
-   **Webhely kiszolgálói naplózás** - HTTP tranzakciók a W3C bővített naplófájl formátumának használatával kapcsolatos információk. Ez akkor hasznos, általános web app mértékek, például a kezelt kéréseket vagy hány kéréseket az egy adott IP-címet a meghatározásakor.

##### <a name="application-diagnostics"></a>Alkalmazás diagnosztika

Alkalmazás diagnosztika webalkalmazás készített információk rögzítése teszi lehetővé. ASP.NET-alkalmazások használata a `System.Diagnostics.Trace` információk naplózása a alkalmazás diagnosztika napló osztály.

Állítson be a naplózás a részletes útmutatást talál [a diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése](web-sites-enable-diagnostic-log.md)

#### <a name="use-remote-profiling"></a>Használja a távoli adatainak összegyűjtése

Azure App szolgáltatásban Web Apps, API-alkalmazások és WebJobs is lehet távolról szánt. Ha a művelet végrehajtása a vártnál, lassabban vagy a HTTP-kérések várakozási nagyobb, mint normál és a folyamat processzorhasználata is magas, távolról a folyamathoz profile és beszerzése a folyamat tevékenység és kód meleg elérési út elemzése a Processzor mintavételnél hívás készleteket.

A további tudnivalókért olvassa el a [távoli adatainak összegyűjtése támogatás Azure App szolgáltatásban](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Az Azure alkalmazás szolgáltatás terméktámogatási portál használata

Web Apps alkalmazások lehetőséget nyújt a HTTP naplók, eseménynaplók, folyamat kiírása és az egyéb megjeleníti a web App alkalmazással kapcsolatos problémák megoldásához. Hozzáférhet az összes ezt az információt a támogatási portált a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**

Az Azure alkalmazás szolgáltatás támogatja a portálon biztosít a három külön lapok a hibaelhárítási szokták három lépését támogatása:

1.  Tekintse át az aktuális viselkedése
2.  Diagnosztikai adatok összegyűjtése és fut a beépített gázelemzőknek elemzése
3.  Csökkentésében

Ha a probléma történik most, kattintson az **elemzés** > **Diagnosztika** > **Diagnosztizálása most** diagnosztikai munkamenet létrehozása, amelyek összegyűjti HTTP jelentkezik, az Eseménynapló, memória kiírása, PHP hibanaplók és PHP feldolgozni a jelentést.

Miután az adatgyűjtés, azt is elemzését futtatja az adatok és biztosítson, HTML-jelentésben.

Abban az esetben, ha szeretné az adatokat, töltse le a alapértelmezés szerint, azt a D:\home\data\DaaS mappában szeretné tárolni.

További információt az Azure alkalmazás szolgáltatás támogatása portálon olvassa el a [Támogatási webhely bővítmény a Azure webhelyeket új frissítések keresése](/blog/new-updates-to-support-site-extension-for-azure-websites)című témakört.

#### <a name="use-the-kudu-debug-console"></a>A Kudu hibakeresési konzol használata

Web Apps alkalmazások megtalálható a hibakeresési konzol hibakeresése során, felfedezése, tölt fel a fájlokat, valamint a JSON végpontokat adatainak megjelenítése a környezet használható. A _Kudu konzol_ vagy az _SCM irányítópult_ a webalkalmazás Link.

Nyissa meg a hivatkozás is elérheti az irányítópult **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.

Az Kudu itt, amit a következők:

-   az alkalmazás környezeti beállítások
-   log adatfolyam
-   diagnosztikai kiírása
-   a hibakeresési konzol, amelyben Powershell-parancsmagok és egyszerű DOS parancsok futtathatja.


Egy másik hasznos Kudu jellemzője, hogy abban az esetben, ha az alkalmazás első-névjegyadatok kivételek van értesítő, Kudu és használhatja a SysInternals eszköz létrehozása memória Procdump kiírása. A memória kiírása pillanatfelvételek a folyamat, és gyakran segítséget nyújtanak az webalkalmazást bonyolultabb kapcsolatos problémák megoldása.

További információt a Kudu funkciót megtekintheti, [Azure webhelyek Team Services kapcsolatos tudnivalók](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. a probléma csökkentésében

####    <a name="scale-the-web-app"></a>A web app méretezése

Azure App szolgáltatásban nagyobb teljesítmény és a teljesítmény, beállíthatja, hogy a skála, amelynél az alkalmazást futtatja. Méretezés felfelé webalkalmazást magában foglalja a két kapcsolódó műveletek: az alkalmazás szolgáltatáscsomagja egy újabb árak réteg módosítása, és bizonyos konfigurálása után a magasabb árak réteg váltott.

Méretezés kapcsolatos további tudnivalókért olvassa el a [Méretarány webalkalmazást Azure App szolgáltatásban](web-sites-scale.md)című témakört.

Továbbá megadhatja az alkalmazás futtatásához egynél több példányon. Ez nem csak nyújt további feldolgozásra lehetőséggel, de is biztosít hibatűrést néhány összege. Ha egy példány megszakad a folyamatot, a másik példányában továbbra is kérések felszolgálásához.

Beállíthatja, hogy legyen a kézi és automatikus méretezés.

####    <a name="use-autoheal"></a>AutoHeal használata

AutoHeal rendszer akkor hasznosítja, újra a munkafolyamat, ha az alkalmazás, a beállítások (például beállítások módosítása kérelmeket, memória-alapú korlátai vagy egy kérés végrehajtásához szükséges időt) alapján. Legtöbbször, a Lomtár a folyamat probléma helyreállítása leggyorsabb módja. Bármikor újra elindíthatja a web app közvetlenül elérhető jelentését az Azure-portálra, mintha AutoHeal hajt végre, automatikusan meg. Kell tennie mindössze néhány indítók hozzáadása a legfelső szintű web.config a webalkalmazásban. Figyelje meg, hogy ezek a beállítások működő ugyanúgy még ha az alkalmazás nem egy .net.

További tudnivalókért lásd: [Azure webhelyek automatikus javító](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Indítsa újra a web App alkalmazásban

Az gyakran egyszeri problémák megoldására legegyszerűbb módja. Az [Azure-portált](https://portal.azure.com/), kattintson a web App alkalmazásban a lap, a lehetőség közül választhat leállítása vagy indítsa újra az alkalmazást.

 ![Indítsa újra a webalkalmazás teljesítménnyel kapcsolatos problémák megoldása](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Az Azure Powershell használatával webalkalmazást is kezelheti. További tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md).
