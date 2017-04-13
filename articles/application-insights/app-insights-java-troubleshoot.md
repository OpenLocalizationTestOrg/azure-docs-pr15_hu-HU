<properties 
    pageTitle="Java webes projekt alkalmazás háttérismeretek kapcsolatos hibák elhárítása" 
    description="Hibaelhárítási útmutatójának – az alkalmazás mélyebb élő Java-alkalmazások figyelése." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Hibaelhárítás és a kérdések és A az alkalmazás az összefüggéseket Java

Kérdések vagy problémák a [Visual Studio alkalmazás háttérismeretek az Java][java]? Az alábbiakban néhány tippet.


## <a name="build-errors"></a>Hibák összeállítása

*Holdas fel az alkalmazást az összefüggéseket SDK maven tesztelése vagy Gradle keresztül, a kapok build vagy ellenőrző érvényesítési hibákat.*

* Ha a függőséget <version> elem mintát által használt helyettesítő karakterekkel (pl. (maven tesztelése) `<version>[1.0,)</version>` vagy (Gradle) `version:'1.0.+'`), próbálkozzon egy adott verziójához inkább megadása például `1.0.2`. Lásd: a [kibocsátási megjegyzések](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) a legújabb verzióját.

## <a name="no-data"></a>Adatok nélkül 

*E alkalmazás háttérismeretek sikeresen hozzáadva, és az alkalmazás futtatta, de lehet soha nem láthatta, adatok a portálon.*

* Várjon egy percet, és kattintson a frissítés gombra. A diagramok rendszeres időközönként frissítse maguk, de manuálisan is frissíthet. Frissítési időköz attól függ, hogy a diagram időtartomány.
* Ellenőrizze, hogy van-e egy (a mappában erőforrásokat a projekt) ApplicationInsights.xml fájlban definiált műszerezettségi kulcs
* Győződjön meg arról, hogy nincs `<DisableTelemetry>true</DisableTelemetry>` csomópont az XML-fájlban.
* A tűzfal lehet, ha TCP-portok 80 és 443-as kimenő forgalmához dc.services.visualstudio.com történő megnyitásához. A [tűzfal teljes listája](app-insights-ip-addresses.md)
* A Microsoft Azure-ban indítsa el a falat, tekintse meg a szolgáltatás állapota térképen. Néhány riasztási jelei vannak, ha várja meg azok OK visszaküldött, és ezután zárja be és nyissa meg újra az alkalmazást az összefüggéseket a alkalmazás lap.
* Az IDE konzol ablak naplózás bekapcsolása a hozzáadásával egy `<SDKLogger />` elem alatt a legfelső szintű csomópontot a ApplicationInsights.xml-fájlt (az erőforrások mappára a projekt), és tartalmazni [hibával] Naplóbejegyzéshez tartozó jelölőnégyzetet.
* Győződjön meg arról, hogy a megfelelő ApplicationInsights.xml fájl tartalmaz sikeresen töltötte a Java SDK megjeleníti a konzol kimenő üzenetek "konfigurációs fájl sikeresen talált" utasítás.
* Ha a konfigurációs fájl nem található, jelölje be a kimenő üzenetek megtekintéséhez, ahol keresett a konfigurációs fájl, és győződjön meg arról, hogy a ApplicationInsights.xml található adott keresési helyek közül. Egy szabály a legjobb megoldás, mint a konfigurációs fájl is elhelyezhet a alkalmazás háttérismeretek SDK JARs közelében. Példa: Tomcat, az Ez a webhely-információ/tár mappát szeretné jelenti.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Használt alkalmazva meghatározott adatokat, de le van állítva

* A [blog állapot](http://blogs.msdn.com/b/applicationinsights-status/)ellenőrzése
* Van találat a havi kvóta adatpontok? Nyissa meg a beállítások/kvóta és árak megtudhatja, hogy. Ha igen, frissítse a csomagja, vagy kiegészítő kapacitás vásárolni. Lásd: az [Office-téma árak](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Nem tudok kapcsolódok meglepő összes adatot látható

* Nyissa meg a kvótákat és árak lap és a jelölőnégyzet, hogy [mintavételnél](app-insights-sampling.md) művelet. (100 %-os átviteli azt jelenti, hogy nincs-e mintavételnél művelet a.) Az alkalmazás az összefüggéseket szolgáltatást beállítható, hogy csak egy részét a telemetriai, hogy az alkalmazás megérkezik, fogadja el. Ezzel az elemzéssel tisztábban megőrzése a havi kvótáját telemetriai belül. 

## <a name="no-usage-data"></a>Látogatottsági adatok nélkül

*Adatok kérések és válaszidő, de nincs lap nézetben, a böngészőben vagy felhasználói látható.*

Sikeresen beállította az alkalmazás telemetriai küldése a kiszolgálóról. Most a következő lépésként [állítsa be a weblapokhoz küldése a böngészőn telemetriai][usage].

Másik lehetőségként az ügyfelek esetén az alkalmazás, a [telefon vagy más eszköz][platforms], elküldhet telemetriai onnan. 

Az azonos műszerezettségi billentyűvel állíthatja be az ügyfél- és kiszolgálóoldali telemetriai. Az adatokat az alkalmazás mélyebb ugyanaz az erőforrás fog megjelenni, és is ügyfél- és kiszolgálóoldali eseményeinek összehangolására.



## <a name="disabling-telemetry"></a>Telemetriai letiltása

*Hogyan tilthatom telemetriai webhelycsoport?*

A kód:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Vagy** 

Frissítse a ApplicationInsights.xml (a mappában erőforrásokat a projekt). Adja hozzá a legfelső szintű csomópont csoportban a következő:

    <DisableTelemetry>true</DisableTelemetry>

Az XML módszerrel, akkor indítsa újra az alkalmazást, amikor módosítja az értéket.

## <a name="changing-the-target"></a>A target módosítása

*Hogyan változtathatom meg a projekt adatokat küld Azure erőforrás?*

* [Ismerkedés a műszerezettségi billentyűt az új erőforrás.][java]
* Alkalmazás háttérismeretek hozzá a projekthez, az Azure eszközkészlet használata Holdas, kattintson a jobb gombbal a webes projekt, ha **Azure**, **Állítsa be az alkalmazás összefüggéseket**, jelölje be, és változtassa meg a kulcsot.
* Egyéb esetben frissítése a kulcsot a ApplicationInsights.xml a Projekt erőforrások mappájában.


## <a name="the-azure-start-screen"></a>Azure kezdőképernyőjén

*[Az Azure-portálra](https://portal.azure.com)a keresek. Nem a térkép jelenjen meg egy üzenetet saját alkalmazás?*

Azure kiszolgálók állapotának nem, a világ mutatja.

*Azure kezdő tábláról (kezdőképernyőjén) Hogyan tudhatom meg, adatok megtudni az alkalmazásról?*

Feltételezve, [állítsa be az alkalmazás az alkalmazás az összefüggéseket][java], kattintson a Tallózás gombra, jelölje be az alkalmazás az összefüggéseket, és jelölje ki az alkalmazás erőforrást hoz létre az alkalmazást. Megszerezni van gyorsabb későbbi, akkor rögzítheti az alkalmazás, a kezdő fal megnyitásához.

## <a name="intranet-servers"></a>Intranet kiszolgálók

*Miként kiszolgáló is figyelheti a intraneten?*

Igen, feltéve, a kiszolgáló telemetriai küldhet az alkalmazás az összefüggéseket portált a nyilvános interneten keresztül. 

A tűzfalat akkor előfordulhat, hogy nyissa meg a TCP-portok 80 és 443-as dc.services.visualstudio.com és f5.services.visualstudio.com kimenő forgalmához.

## <a name="data-retention"></a>Adatmegőrzés 

*Mennyi ideig tárolja a portálon az adatok? Az biztonságos?*

Lásd:, [adatmegőrzés és adatvédelmi][data].

## <a name="next-steps"></a>Következő lépések

*Tudom beállítani a alkalmazás az összefüggéseket a Java server alkalmazáshoz. Mit lehet tenni?*

* [A weblapok elérhetősége figyelése][availability]
* [Weblap használat figyelése][usage]
* [Nyomon követheti a használatát, és a eszköz alkalmazás problémáinak diagnosztizálása][platforms]
* [Az alkalmazás használatát nyomon követéséhez kódírás][track]
* [A diagnosztikai naplók rögzítése][javalogs]


## <a name="get-help"></a>Segítség kérése

* [Egymást fedő túlcsordulás](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 