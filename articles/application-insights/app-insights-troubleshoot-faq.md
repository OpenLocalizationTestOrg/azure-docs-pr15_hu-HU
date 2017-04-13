<properties 
    pageTitle="Hibaelhárítás és alkalmazás háttérismeretek kapcsolatos kérdések" 
    description="Valami a Visual Studio alkalmazás háttérismeretek világos vagy nem működik? Próbálkozzon az alábbi." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Kérdések - alkalmazás háttérismeretek ASP.NET

## <a name="configuration-problems"></a>Konfigurációs problémák

*Problémái vannak a beállítás lépett a:*

* [.NET-alkalmazás](app-insights-asp-net-troubleshoot-no-data.md)
* [Már futó alkalmazás figyelése](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure diagnosztika](app-insights-azure-diagnostics.md)
* [Java web App alkalmazásban](app-insights-java-troubleshoot.md)
* [Más platformokhoz készült Worddel](app-insights-platforms.md)

*Adatokat nem tudom bekapcsolni a kiszolgálója*

* [Tűzfal a kivételek megadása](app-insights-ip-addresses.md)
* [ASP.NET-kiszolgáló beállítása](app-insights-monitor-performance-live-website-now.md)
* [Java-kiszolgáló beállítása](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Használhatom-e az alkalmazást az összefüggéseket...?

[Lásd: platformok][platforms]


## <a name="is-it-free"></a>Az ingyenes?

* Igen, ha úgy dönt, hogy a [réteg árak](app-insights-pricing.md)ingyenes. A legtöbb funkció és az adatok egyéb kvóta kap. 
* Meg kell adnia a Microsoft Azure regisztrálása hitelkártya adatait, de nincs díjak történik, kivéve, ha egy másik kifizetett az Azure szolgáltatás használja, vagy explicit módon frissítenie a kifizető réteg.
* Ha az alkalmazás több adatot, mint a havi kvóta küld a szabad réteg számára, megszakítja a naplózott. Ebben az esetben, ha választva elindíthatja a fizet, vagy várja meg, amíg a hónap végén kvóta visszaállítása.
* Egyszerű használat és a munkamenet adat nem fizetnie kvóta van.
* Egy 30 napig ingyenes próbaverziója, amely alatt a ingyenesen a kifizetett-funkciók beszerzése is van.
* Egyes alkalmazások erőforrásnak megvan-e egy külön kvóta, és beállíthatja, hogy a többi függetlenül árak réteg.

#### <a name="what-do-i-get-if-i-pay"></a>Mi a kapok, ha e fizetni?

* Nagyobb, [az adatok havi kvóta](https://azure.microsoft.com/pricing/details/application-insights/).
* A fizetés "többlet" továbbra is az adatok gyűjtése a havi kvóta fölé lehetőséget. Ha az adatok kvóta fölé kerül, akkor egy Mb esetén fel.
* A [folyamatos exportálja](app-insights-export-telemetry.md).


## <a name="q14"></a>Mi nem alkalmazás háttérismeretek módosíthatják a projekt?

A részletek attól függenek, hogy milyen típusú projektet. Webes alkalmazáshoz:


+ Ezek a fájlok hozzáadása a projekthez:

 + ApplicationInsights.config. 
 + AI.js


+ Ezekről a csomagokról NuGet telepítése:

 -  *Alkalmazás háttérismeretek API* - az alapvető API

 -  *Alkalmazás háttérismeretek API a webalkalmazások* - telemetriai küldi a kiszolgálóról

 -  *JavaScript-alkalmazásokhoz alkalmazás háttérismeretek API* - telemetriai küldi a ügyfélprogramból

    A csomagok többek között az alábbi összeállítások:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Az elemek beillesztése:

 - Web.config

 - Packages.config

+ (Új projektek csak – ha [alkalmazás háttérismeretek egy meglévő projekten szeretne hozzáadni][start], kövesse az alábbi lépéseket manuálisan kell.) Kódrészletek szúrja be az ügyfél- és kiszolgálóoldali kódot őket az alkalmazás mélyebb erőforrás azonosítójával. Például egy MVC alkalmazásban, a kód beilleszti a mesterlap Views/Shared/_Layout.cshtml


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Hogyan frissíthetem régebbi verziójával csatlakoznak SDK?

Lásd: a [kibocsátási megjegyzések](app-insights-release-notes.md) az alkalmazás a típusú megfelelő SDK. 



## <a name="update"></a>Hogyan változtathatom meg a projekt adatokat küld Azure erőforrás?

A megoldás Intézőben kattintson a jobb gombbal `ApplicationInsights.config` , és válassza a **Frissítés alkalmazása az összefüggéseket**. Az adatok küldhet egy meglévő vagy új erőforrás Azure-ban. A frissítés varázsló módosítja a ApplicationInsights.config, amely meghatározza, ahol a kiszolgáló SDK küld, az adatok műszerezettségi kulcsot. Ha "Összes frissítése" kijelölésének megszüntetése is megváltozik a kulcsot, amelyben a weblapok jelenik meg.


#### <a name="data"></a>Mennyi ideig tárolja a portálon az adatok? Az biztonságos?

Ajánljuk figyelmébe az [adatmegőrzés és adatvédelmi][data].

## <a name="logging"></a>Naplózás

#### <a name="post"></a>Hogyan tekinthetem meg a diagnosztikai keresés a bejegyzés adatok?

Automatikusan azt nem jelentkezik a bejegyzés adatokat, de használható TrackTrace hívást: az üzenet paraméter elhelyezni az adatokat. Ez méretkorlátja hosszabb, mint a karakterlánc-tulajdonságok vonatkozó korlátok azonban nem szűrhet rajta. 

## <a name="security"></a>Biztonsági

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Az adataim biztonságos a portálon? Mennyi ideig tárolja azt?

Lásd: [adatok adatmegőrzési és adatvédelmi][data].


## <a name="q17"></a>E engedélyezve van az alkalmazás az összefüggéseket a szolgáltatás?

<table border="1">
<tr><th>Érdemes megjelenő elemek</th><th>Hogyan szerezhető be?</th><th>Miért kívánt</th></tr>
<tr><td>Elérhetőség diagramok</td><td><a href="../app-insights-monitor-web-app-availability/">Webes vizsgálatok</a></td><td>Tudja, hogy a webalkalmazás felfelé</td></tr>
<tr><td>Kiszolgáló alkalmazás perf: válaszidő,...:
</td><td><a href="../app-insights-asp-net/">Alkalmazás háttérismeretek hozzáadása a projekthez</a><br/>vagy <br/><a href="../app-insights-monitor-performance-live-website-now/">AI állapot Monitor kiszolgálón telepítenie</a> (vagy saját <a href="../app-insights-api-custom-events-metrics/#track-dependency">függőségek nyomon</a>követéséhez kódírás)</td><td>Észleli perf kapcsolatos problémák</td></tr>
<tr><td>Függőség telemetriai</td><td><a href="../app-insights-monitor-performance-live-website-now/">AI állapot Monitor telepíteni a kiszolgálón</a></td><td>Adatbázisok vagy más külső összetevők problémáinak diagnosztizálása</td></tr>
<tr><td>Egymást fedő halad beolvasása a kivételek</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">A kód beszúrása TrackException hívások</a> (de néhány automatikusan jelentett)</td><td>Észleli és kivételek diagnosztizálása</td></tr>
<tr><td>Keresés napló nyomkövetési naplók</td><td><a href="../app-insights-search-diagnostic-logs/">A naplózás kártya hozzáadása</a></td><td>A kivételek perf problémák diagnosztizálása</td></tr>
<tr><td>Ügyfél használatát alapjai: lap nézetek, a munkamenetek,...:</td><td><a href="../app-insights-javascript/">A JavaScript inicializálója a weblapokon</a></td><td>Lekérdezéshasználati analitika</td></tr>
<tr><td>Ügyfél egyéni mérőszámok</td><td><a href="../app-insights-api-custom-events-metrics/">A weblapok hívások nyomon követése</a></td><td>Felhasználói élmény fokozása</td></tr>
<tr><td>Egyéni mértékek kiszolgáló</td><td><a href="../app-insights-api-custom-events-metrics/">Kiszolgáló kód hívások nyomon követése</a></td><td>Üzleti intelligenciával kapcsolatos funkciók</td></tr>
</table>


## <a name="automation"></a>Automatizálási

Azt is megteheti, hogy [PowerShell-parancsfájlokat](app-insights-powershell.md) létrehozása és frissítése az alkalmazás az összefüggéseket erőforrásokat.

## <a name="more-answers"></a>Ha további válaszokat

* [Alkalmazás háttérismeretek fórum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 