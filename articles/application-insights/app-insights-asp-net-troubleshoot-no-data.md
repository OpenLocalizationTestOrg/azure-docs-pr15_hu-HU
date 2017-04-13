<properties 
    pageTitle="Hibaelhárítás nincs adat - alkalmazás az összefüggéseket a .NET rendszerhez" 
    description="Nem jelennek meg a Visual Studio alkalmazás mélyebb adatok? Próbálkozzon az alábbi." 
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
    ms.date="10/24/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Hibaelhárítás nincs adat - alkalmazás az összefüggéseket a .NET rendszerhez

## <a name="some-of-my-telemetry-is-missing"></a>A telemetriai közül hiányzik

*Az alkalmazás Hírcsatornájában az alkalmazásom által létrehozott események tört csak látható.*

* Ha a azonos tört egységes láthatók, annak oka valószínűleg adaptív [mintavételnél](app-insights-sampling.md)miatt. Ennek ellenőrzéséhez nyissa meg a keresés (a az Áttekintés lap), és tekintse meg egy példányának kérelmének vagy egyéb esemény. A Tulajdonságok szakasz alján kattintson a (...), teljes tulajdonság részleteket. Ha kérése Count > 1, akkor mintavételnél művelet. 
* Egyéb esetben célszerű lehet, hogy akkor is szerezze meg a [adatsebesség korlátozni](app-insights-pricing.md#limits-summary) árak csomagja. Ezek a korlátok percenkénti veszi.

## <a name="no-data-from-my-server"></a>A kiszolgálóról adatok nélkül

*Az alkalmazás telepítése a webkiszolgálón, és be van kapcsolva, nem látható minden telemetriai belőle. A fejlesztők gépen működött az OK gombra.*

* Tűzfal probléma valószínűleg. [Az alkalmazás az összefüggéseket adatok küldése a tűzfal kivételek megadása](app-insights-ip-addresses.md).

*E [állapot Monitor telepítve](app-insights-monitor-performance-live-website-now.md) a Lync-meglévő alkalmazások webkiszolgáló. Nem látom a találatok között.*

* Lásd: [állapot Monitor hibaelhárítás](app-insights-monitor-performance-live-website-now.md#troubleshooting). 


## <a name="q01"></a>A Visual Studióban nincs hozzáadása alkalmazás háttérismeretek beállítás

*Amikor e új projekt létrehozása a Visual Studióban, vagy lehet kattintson a jobb gombbal a megoldást Intézőben egy meglévő projekten, alkalmazás háttérismeretek beállításokat nem látható.*

+ .NET projekt nem az összes típusú a eszközök támogatottak. Webes és a WCF projektek támogatottak. Más projekttípusok, például az asztali vagy szolgáltatási alkalmazást akkor is [adhat az alkalmazás az összefüggéseket SDK csomagjában talál a projekthez manuálisan](app-insights-windows-desktop.md).
+ Győződjön meg arról, hogy a [Visual Studio 2013 frissítés 3 és újabb verziók](http://go.microsoft.com/fwlink/?LinkId=397827). Előre telepítve van az összefüggéseket eszközökkel származik.
+ Válassza az **eszközök**, **bővítmények és frissítések** , és ellenőrizze, hogy a **Alkalmazás háttérismeretek eszközök** telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** megjelenítéséhez, ha van frissítés gombra.
+ Nyissa meg az új projekt párbeszédpanelt, és válassza a ASP.NET webalkalmazást. Ha van az alkalmazás mélyebb beállítás jelenik meg, az eszköz van telepítve. Ha nem, próbálja meg eltávolítása és újbóli telepítése az alkalmazás az összefüggéseket eszközök.


## <a name="q02"></a>Alkalmazás mélyebb hozzáadása nem sikerült

*Hozhatok létre egy új webes projekt vagy jelenik meg az összefüggéseket alkalmazás hozzáadása egy meglévő projekten esetén megjelenő hibaüzenet látható.*

Lehetséges okai:

* Nem sikerült az alkalmazás az összefüggéseket portál való kommunikáció; vagy
* Az Azure-fiók; valamilyen probléma van
* Csak akkor [olvasási hozzáférést az előfizetést, vagy a csoport megpróbált, ahol az új erőforrás létrehozásához](app-insights-resources-roles-access-control.md).

Javítás:

+ Jelölje be a bejelentkezési hitelesítő adataimat előírt a jobb oldali Azure-fiók. 
+ Jelölje be a böngészőben, hogy az [Azure portál](https://portal.azure.com)hozzáférése van. Nyissa meg a beállításokat, és látható, ha nincs korlátozás.
+ [Alkalmazás háttérismeretek hozzáadása egy már meglévő projektbe](app-insights-asp-net.md): megoldás Intéző, kattintson a jobb gombbal a projekt, és válassza a "Hozzáadása az alkalmazás az összefüggéseket."
+ Azt még mindig nem működik, ha végezze el az erőforrások hozzáadása a portál és az SDK hozzáadása a projekthez [kézi eljárást](app-insights-windows-services.md) . 

## <a name="emptykey"></a>Hibaüzenet "műszerezettségi kulcs nem lehet üres"

Tűnik valami hiba történt, miközben alkalmazás háttérismeretek vagy esetleg a naplózás kártya telepíti.

A megoldás Intézőben kattintson a jobb gombbal `ApplicationInsights.config` , és válassza az **Alkalmazás az összefüggéseket konfigurálása**. Szerezze be a párbeszédpanelt, amely felajánlja a jelentkezzen be az Azure, és hozza létre az alkalmazás mélyebb erőforrás, vagy újra felhasználhat egy meglévőt.


##<a name="NuGetBuild"></a>"NuGet csomagok fognak hiányozni" saját build kiszolgálón

*Az OK gombra a szolgáltatás hoz létre, fejlesztési számítógépemre vagyok hibakeresés lehet, de NuGet hibaüzenetet kapok build kiszolgálói.*

Tekintse át a [NuGet csomag visszaállítása](http://docs.nuget.org/Consume/Package-Restore) , és [Az automatikus csomag visszaállítása](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Hiányzó menüparancs kattintva nyissa meg az alkalmazást az összefüggéseket a Visual Studio

*Az egér jobb gombjával a project Explorer megoldást, alkalmazás háttérismeretek parancsok nem látható, vagy egy megnyitott alkalmazás háttérismeretek parancs nem látható.*

Lehetséges okai:

* Ha az alkalmazás az összefüggéseket erőforrás manuálisan létrehozott, vagy ha a projekt az alkalmazás az összefüggéseket eszközök által nem támogatott típusú.
* Az alkalmazás az összefüggéseket eszközök le vannak tiltva a Visual Studio.
* A Visual Studio 2013 frissítés 3-nél korábbi verzióval készült.

Javítás:

* Ellenőrizze, hogy a Visual Studio verzió 2013 frissítés 3 és újabb verziók.
* Válassza az **eszközök**, **bővítmények és frissítések** , és ellenőrizze, hogy a **Alkalmazás háttérismeretek eszközök** telepítve és engedélyezve van. Ha igen, kattintson a **frissítések** megjelenítéséhez, ha van frissítés gombra.
* Kattintson a jobb gombbal a projekt a megoldást Intézőben. Ha az **Alkalmazás az összefüggéseket konfigurálása**parancs látható, használja a projekt csatlakozhat az alkalmazás az összefüggéseket szolgáltatásban az erőforrás.


A projekt típusa ellenkező esetben közvetlenül nem támogatott a alkalmazás háttérismeretek eszközeivel. A telemetriai megtekintéséhez jelentkezzen be az [Azure portál](https://portal.azure.com), válassza az alkalmazás az összefüggéseket a bal oldali navigációs sávon, és jelölje ki azt az alkalmazást.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>"Hozzáférés megtagadva" a alkalmazás háttérismeretek nyitja meg a Visual Studio

*A "Megnyitás alkalmazás háttérismeretek" parancs megnyitja, az Azure-portálra, de hibaüzenet "hozzáférés megtagadva".*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

A bejelentkezés a Microsoft az alapértelmezett böngésző legutóbb használt nincs hozzáférése [az erőforrás, amely jött létre, amikor az alkalmazás háttérismeretek az alkalmazás hozzá lett adva](app-insights-asp-net.md). Két okból valószínű: 

* Ha egynél több Microsoft - fiókjából, esetleg a munkahelyi telefonszámán és a személyes Microsoft-fiók? A legutóbb használt az alapértelmezett böngésző a bejelentkezés egy másik fiókhoz, mint a nézet hozzáadása [A Project alkalmazás háttérismeretek](app-insights-asp-net.md)hozzáféréssel rendelkező volt. 

 * Javítás: Kattintson a nevére a böngésző ablakának jobb felső, és jelentkezzen ki. Jelentkezzen hozzáféréssel rendelkező fiók. A bal oldali navigációs sávon kattintson az alkalmazás mélyebb, majd válassza ki az alkalmazást.

* Valaki más alkalmazást az összefüggéseket a projekthez való hozzáadásának, és azok elfelejtette [erőforráscsoport való hozzáférést](app-insights-resources-roles-access-control.md) , amelyben létrehozták. 

 * Javítás: Segítségükkel szervezeti fiókkal, ha azokat is fel kell vennie Önt a csapat; vagy azok is hozzáférést, egyes erőforráscsoport.



## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>"Eszköz nem található" a alkalmazás háttérismeretek nyitja meg a Visual Studio

*A "Megnyitás alkalmazás háttérismeretek" parancs megnyitja, az Azure-portálra, de hibaüzenet "eszköz nem található".*

Lehetséges okai:

* Az alkalmazás az alkalmazás az összefüggéseket erőforrás törölve lett; vagy
* A műszerezettségi kulcs lett beállítása és módosítása a ApplicationInsights.config való szerkesztésével, közvetlenül a projektfájl frissítése nélkül 

A hol küldi el a telemetriai ApplicationInsights.config vezérlők műszerezettségi billentyűt. A project-fájlban lévő vonal szabályozza, hogy melyik erőforrás nyitja meg a Visual Studióban a parancs használatakor. 

Javítás:

* A megoldás Intézőben kattintson a jobb gombbal a projekt, és válassza az alkalmazás mélyebb, állítsa be az alkalmazás összefüggéseket. A párbeszédpanel választhat telemetriai küldhet egy meglévő erőforrás, vagy hozzon létre egy újat. Vagy:
* Nyissa meg közvetlenül az erőforrás. Jelentkezzen be [az Azure](https://portal.azure.com)-portálra, és kattintson az alkalmazás az összefüggéseket a bal oldali navigációs sávon válassza az alkalmazás.



## <a name="where-do-i-find-my-telemetry"></a>Hol találom meg a telemetriai?

*Lehet bejelentkezni a [Microsoft Azure-portálon](https://portal.azure.com), és az Azure otthoni irányítópulton a keresek. Hogy hol találhatók az alkalmazás az összefüggéseket adataimat?*

* A bal oldali navigációs sávon kattintson az alkalmazás hírcsatornájában, majd az alkalmazás neve. Ha van olyan projektek nincs, kell [hozzáadása és konfigurálása az alkalmazás az összefüggéseket a webes projekt](app-insights-asp-net.md).

    Vannak bizonyos összefoglaló diagramok megjelenik. Kattinthat végig őket a részletek megtekintéséhez.

* A Visual Studióban közben az alkalmazás Hibakeresés gombra az alkalmazás az összefüggéseket.


## <a name="q03"></a>Nincs kiszolgálóadatok (vagy egyáltalán nincs adat)

*Lehet az alkalmazás és a majd megnyitott az alkalmazás az összefüggéseket szolgáltatás a Microsoft Azure-ban, de minden diagram megjelenítéséhez "Megtudhatja, hogyan gyűjthetők össze az..." vagy "Nincs beállítva."* Másik lehetőségként *csak lap nézetben, és a felhasználói adatok, de nincs kiszolgálóadatok.*

+ Az alkalmazás futtatásához hibakeresési módban a Visual Studióban (F5). Annak érdekében, hogy bizonyos telemetriai készítése alkalmazással. Ellenőrizze, hogy látható-e jelentkezve a Visual Studio kimeneti ablakban események. 

    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)

+ Az alkalmazás az összefüggéseket portálon nyissa meg a [Diagnosztikai keresés](app-insights-diagnostic-search.md). Adatok általában itt jelenik meg először.
+ Kattintson a frissítés gombra. A lap rendszeres frissíti magát, de manuálisan is megteheti. Frissítési időköz hosszabb ideig nagyobb idő tartományait.
+ Jelölje be a műszerezettségi billentyűk megfelelően. Az alkalmazás az alkalmazás az összefüggéseket portálon a **Essentials** legördülő listában a fő lap ellenőrizze **műszerezettségi billentyűt**. Ezután a projektben a Visual Studióban, nyissa meg a ApplicationInsights.config, és keresse meg a `<instrumentationkey>`. Ellenőrizze, hogy a két billentyűk egyenlő. Ha nem:
 + A portálon kattintson az alkalmazás az összefüggéseket, és keresse meg a megfelelő kulccsal; az alkalmazás erőforrás vagy
 + A Visual Studio megoldás Intézőben kattintson a jobb gombbal a projekt, és válassza az alkalmazás hírcsatornájában, konfigurálása. Állítsa alaphelyzetbe az alkalmazás telemetriai küldése a megfelelő erőforrásnézethez.
 + Ha nem találja a megfelelő kulcsok, ellenőrizze, hogy esetén a bejelentkezési hitelesítő adatait a Visual Studióban, mint a portálra.

    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
    
+ A [Microsoft Azure otthoni irányítópult](https://portal.azure.com)tekintse meg a szolgáltatás állapota térképen. Néhány riasztási jelei vannak, ha várja meg azok OK visszaküldött, és ezután zárja be és nyissa meg újra az alkalmazást az összefüggéseket a alkalmazás lap.
+ [Állapot blogbejegyzés](http://blogs.msdn.com/b/applicationinsights-status/)is ellenőrizni.
+ Jelent, kódírás bármely a gyakran változó a műszerezettségi kulcsot a [kiszolgálóoldali SDK](app-insights-api-custom-events-metrics.md) `TelemetryClient` példányok vagy a `TelemetryContext`? Jelent, a [szűrő vagy mintavételnél konfigurációs](app-insights-api-filtering-sampling.md) előfordulhat, hogy szűr beírása túl nagynak találja meg?
+ Ha korábban szerkesztett ApplicationInsights.config, gondosan ellenőrizze [TelemetryInitializers és TelemetryProcessors](app-insights-api-filtering-sampling.md)beállításait. Egy helytelenül nevű típusa vagy a paraméter nincs adatok küldése a SDK okozhat.

## <a name="q04"></a>A lap nézetek, böngészőkkel, a használati adatok nélkül

*Látható adatok kiszolgáló válaszidő-és kiszolgáló kérések, de adatok nélkül nézet Lapbetöltési idő vagy a böngészőben vagy használatát rögzítéséhez.*

A weblapok parancsfájlok származnak az adatok. 

+ Ha adott alkalmazás mélyebb webes projekt, [a parancsfájlok hozzáadása kézzel kell](app-insights-javascript.md).
+ Győződjön meg arról, hogy az Internet Explorer kompatibilitási módban nem jelennek meg a webhelyen.
+ Funkcióval a böngésző hibakeresési (egyes böngészőkben F12 válassza a hálózat) ellenőrizze, hogy küldött adatok `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Függőség vagy a kivétel adatok nélkül

Lásd: [függőség telemetriai](app-insights-asp-net-dependencies.md) és a [kivétel telemetriai](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>A teljesítmény adatok nélkül

A teljesítményadatok (Processzor, IO ráta, és így tovább) [Java webszolgáltatásokhoz](app-insights-java-collectd.md), [a Windows asztali alkalmazások](app-insights-windows-desktop.md), [az IIS webes alkalmazások és -szolgáltatások, ha telepíti az állapot monitor](app-insights-monitor-performance-live-website-now.md)és [Azure Cloud Services](app-insights-azure.md)érhető el. Kiszolgálók a beállítások csoportban találhatók.

Nem érhető el a Azure webhelyeket.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>Nincsenek (kiszolgáló) adatok, mivel az alkalmazás a kiszolgálón közzétett

+ Ellenőrizze, hogy valójában másolta a a Microsoft. A kiszolgálón üzemelő Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll ApplicationInsights dll-EK
+ A tűzfalat akkor előfordulhat, hogy [Nyissa meg az egyes TCP-portok](app-insights-ip-addresses.md#data-access-api).
+ Ha a proxy használatát a vállalati hálózathoz ki küldhet, állítsa [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) Web.config
+ Windows Server 2008: Győződjön meg arról, hogy telepítve van az alábbi frissítéseket: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).


## <a name="i-used-to-see-data-but-it-has-stopped"></a>Használt alkalmazva meghatározott adatokat, de le van állítva

* A [blog állapot](http://blogs.msdn.com/b/applicationinsights-status/)ellenőrzése
* Van találat a havi kvóta adatpontok? Nyissa meg a beállítások/kvóta és árak, megtudhatja, hogy. Ha igen, frissítse a csomagja, vagy kiegészítő kapacitás vásárolni. Lásd: az [Office-téma árak](https://azure.microsoft.com/pricing/details/application-insights/).


## <a name="i-dont-see-all-the-data-im-expecting"></a>Nem tudok vagyok meglepő összes adatot látható

Ha az alkalmazás küld az adatokat, és használja az alkalmazás az összefüggéseket SDK ASP.NET verzió 2.0.0-beta3 vagy újabb, a [Adaptív mintavételnél](app-insights-sampling.md) funkció működjön, és küldése csak a telemetriai százalékában. 

Letilthatja, de ez nem ajánlott. Példákat talál arra, hogy a kapcsolódó telemetriai megfelelően továbbított, diagnosztikai célokra lett tervezve. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Felhasználói telemetriai a megfelelő földrajzi adattal

A város, a terület és a ország méretek IP-címek származik, és nem mindig pontos.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Kivétel "metódus nem található" a az Azure Cloud Services fut.

DID összeállítása a .NET 4.6? 4.6 automatikusan nem támogatott Azure Cloud Services szerepkörök. [A szerepkörökhöz telepítése 4.6](../cloud-services/cloud-services-dotnet-install-dotnet.md) az alkalmazás futtatása előtt.

## <a name="still-not-working"></a>Továbbra sem működik.

* [Alkalmazás háttérismeretek fórum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

