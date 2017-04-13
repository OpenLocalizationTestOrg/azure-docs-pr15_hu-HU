<properties
    pageTitle="502 – Hibás átjáró javítása, 503 szolgáltatás nem érhető el hibák |} Microsoft Azure"
    description="502 – Hibás átjáró hibák és 503 szolgáltatás nem érhető el az Azure-App szolgáltatásban tárolt webalkalmazást hibák elhárítása"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 – Hibás átjáró 503 szolgáltatás nem érhető el, hiba 503, hiba 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>A "502 – Hibás átjáró" és "503 – a szolgáltatás nem érhető el" a Azure web Apps alkalmazások a HTTP-hibák elhárítása

"502 – Hibás átjáró" és "503 – a szolgáltatás nem érhető el" a az [Azure](http://go.microsoft.com/fwlink/?LinkId=529714)-App szolgáltatásban tárolt webalkalmazásban gyakori hibák történnek. Ez a cikk segítséget nyújt a hibák elhárítása.

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet az Azure szakértői [az MSDN Azure és a túlcsordulás Papírhalom fórumokon](https://azure.microsoft.com/support/forums/). Másik lehetőségként az Azure támogatási kérelmeiket is küldhet. Nyissa meg a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , és a **Get-támogatást**gombra.

## <a name="symptom"></a>A jelenség

Keresse meg a web App alkalmazásban, ha a HTTP ad vissza egy HTTP vagy a "502 – Hibás átjáró" hiba "503 szolgáltatás nem érhető el" hibaüzenet.

## <a name="cause"></a>OK

Ez a probléma gyakran okozza alkalmazás szintű problémák, például:

-   túl hosszú ideig tart kérések
-   használó magas memória/Processzor
-   az alkalmazás összeomlik, kivétel következtében.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Hibaelhárítási lépések "502 – Hibás átjáró" és "503 – a szolgáltatás nem érhető el" hibák megoldására

Hibaelhárítás osztható három különböző tevékenységeket, egymás után következő sorrendben:

1.  [Megfigyelheti, és figyelemmel követheti az alkalmazás működése](#observe)
2.  [Adatgyűjtés](#collect)
3.  [A probléma csökkentésében](#mitigate)

[Alkalmazás szolgáltatás Web Apps alkalmazásokban](/services/app-service/web/) is megadhatja különböző lépésről lépésre.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. a megfigyelheti és figyelemmel követheti az alkalmazás működése

####    <a name="track-service-health"></a>Szolgáltatásállapot nyomon követése

Microsoft Azure publicizes minden alkalommal, amikor egy szolgáltatás megszakítása és a teljesítmény csökkenés van. A szolgáltatás állapotát jelző követheti nyomon az [Azure-portálon](https://portal.azure.com/). További tudnivalókért olvassa el a [szolgáltatásállapot nyomon követése](../monitoring-and-diagnostics/insights-service-health.md)című témakört.

####    <a name="monitor-your-web-app"></a>Figyelheti a web App alkalmazásban

Ez a beállítás lehetővé teszi annak megállapítása, ha az alkalmazás hibát tapasztal problémákat. Az a web App alkalmazásban a lap kattintson a **kérelem és hibák** csempére. A **metrikus** lap kell követnie a is hozzáadhat mérési módja miatt.

A mértékek, érdemes lehet figyelni a webalkalmazás vannak

-   Átlagos memória-munkakészlet
-   Átlagos válaszidő
-   Processzor idő
-   Memória-Munkakészlet
-   Kérések

![502 – Hibás átjáró és 503 – a szolgáltatás nem érhető el a HTTP-hibák megoldási felé monitor web App alkalmazásban](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

További tudnivalókért lásd:

-   [Webhely alkalmazásainak figyelése Azure App szolgáltatásban](web-sites-monitor.md)
-   [Értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. a adatok gyűjtése

####    <a name="use-the-azure-app-service-support-portal"></a>Az Azure alkalmazás szolgáltatás terméktámogatási portál használata

Web Apps alkalmazások lehetőséget nyújt a HTTP naplók, eseménynaplók, folyamat kiírása és az egyéb megjeleníti a web App alkalmazással kapcsolatos problémák megoldásához. Hozzáférhet az összes ezt az információt a támogatási portált a **http://&lt;az alkalmazás neve >.scm.azurewebsites.net/Support**

Az Azure alkalmazás szolgáltatás támogatja a portálon biztosít a három külön lapok a hibaelhárítási szokták három lépését támogatása:

1.  Tekintse át az aktuális viselkedése
2.  Diagnosztikai adatok összegyűjtése és fut a beépített gázelemzőknek elemzése
3.  Csökkentésében

Ha a probléma történik most, kattintson az **elemzés** > **Diagnosztika** > **Diagnosztizálása most** diagnosztikai munkamenet létrehozása, amelyek összegyűjti HTTP jelentkezik, az Eseménynapló, memória kiírása, PHP hibanaplók és PHP feldolgozni a jelentést.

Miután az adatgyűjtés, azt is elemzését futtatja az adatok és biztosítson, HTML-jelentésben.

Abban az esetben, ha szeretné az adatokat, töltse le a alapértelmezés szerint, azt a D:\home\data\DaaS mappában szeretné tárolni.

További információt az Azure alkalmazás szolgáltatás támogatása portálon olvassa el a [Támogatási webhely bővítmény a Azure webhelyeket új frissítések keresése](/blog/new-updates-to-support-site-extension-for-azure-websites)című témakört.

####    <a name="use-the-kudu-debug-console"></a>A Kudu hibakeresési konzol használata

Web Apps alkalmazások megtalálható a hibakeresési konzol hibakeresése során, felfedezése, tölt fel a fájlokat, valamint a JSON végpontokat adatainak megjelenítése a környezet használható. A _Kudu konzol_ vagy az _SCM irányítópult_ a webalkalmazás Link.

Nyissa meg a hivatkozás is elérheti az irányítópult **https://&lt;az alkalmazás neve >.scm.azurewebsites.net/**.

Az Kudu itt, amit a következők:

-   az alkalmazás környezeti beállítások
-   log adatfolyam
-   diagnosztikai kiírása
-   a hibakeresési konzol, amelyben Powershell-parancsmagok és egyszerű DOS parancsok futtathatja.


Egy másik hasznos Kudu jellemzője, hogy abban az esetben, ha az alkalmazás első-névjegyadatok kivételek van értesítő, Kudu és használhatja a SysInternals eszköz létrehozása memória Procdump kiírása. A memória kiírása pillanatfelvételek a folyamat, és gyakran segítséget nyújtanak az webalkalmazást bonyolultabb kapcsolatos problémák megoldása.

További információt a Kudu funkciót megtekintheti, [Azure webhelyek online kapcsolatos tudnivalók](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Indítsa újra az alkalmazást 502 – Hibás átjáró és 503 – a szolgáltatás nem érhető el a HTTP-hibák megoldására](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Az Azure Powershell használatával webalkalmazást is kezelheti. További tudnivalókért lásd: [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md).
