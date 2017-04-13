<properties 
    pageTitle="Kibocsátási megjegyzések az alkalmazás az összefüggéseket a Windows" 
    description="A legújabb frissítéseit a Windows áruházból SDK csomagjában talál." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/12/2016" 
    ms.author="joshweb"/>
 
# <a name="release-notes-for-application-insights-sdk-for-windows-phone-and-store"></a>Kibocsátási megjegyzések az alkalmazás az összefüggéseket a Windows Phone SDK és tárolása

Az alkalmazás az összefüggéseket SDK telemetriai kapcsolatban az élő alkalmazás küld [Alkalmazás Hírcsatornájában](https://azure.microsoft.com/services/application-insights/), ahol használatát és a teljesítmény elemezheti.


## <a name="version-111"></a>1.1.1-es

### <a name="windows-sdk"></a>Windows SDK

- Javítás során összeomlik egy lefagy, a Windows Phone Silverlight SDK használata esetén. A módosítás után bármilyen összeomlik, hogy később ~ 2 másodperc után a hívást WindowsAppInitialier.InitializeAsync(...) történik maradnak a lemezre jelölőnégyzetből, és a következő küldi el az alkalmazás indításakor. Ha e az összeomlást ~ 2 másodperc után a hívás előtt történik, akkor figyelmen kívül hagyja.  
- Állítsa a NuGet függőségek egy adott verziójához Core és Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3).   

### <a name="core-sdk"></a>Alapvető SDK

- Alapvető github kezelése. Jövőbeli kibocsátási megjegyzések az alapvető SDK [github](http://github.com/Microsoft/ApplicationInsights-dotnet/releases) találhatók.

## <a name="version-11"></a>1.1-es verzió

### <a name="core-sdk"></a>Alapvető SDK

- SDK most vezet be új telemetriai típus ```DependencyTelemetry``` függőség hívás alkalmazásból információkat tartalmaz, amelyek
- Új módszer ```TelemetryClient.TrackDependency``` lehetővé teszi, hogy a hívások függőség információk küldése alkalmazásból

## <a name="version-100"></a>Verzió 1.0.0

### <a name="windows-app-sdk"></a>Windows SDK-alkalmazás

- Új inicializálni-Windows alkalmazást. Új `WindowsAppInitializer` az osztály `InitializeAsync()` metódus SDK webhelycsoport inicializálni rendszerindításáért teszi lehetővé. Ez a változás engedélyezze a pontosabban és jelentős alkalmazás inicializálni teljesítménybeli fejlesztések előző ApplicationInsights.config módszer szerint.
- DeveloperMode már nem automatikusan van beállítva. Meg kell adnia a kód DeveloperMode viselkedésének módosítása.
- NuGet csomag már nem beszúrhatja ApplicationInsights.config. Az új WindowsAppInitializer melyikkel manuális felvétele NuGet csomag javasoljuk.
- Csak felolvassa ApplicationInsights.config `<InstrumentationKey>`, minden egyéb beállításait nem veszi figyelembe a preferencia-sorrend WindowsAppInitializer beállítások.
- Tár piaci SDK által gyűjtött automatikus lesz.
- Számos hibajavítás stabilitás javítása és teljesítménnyel kapcsolatos fejlesztések.

### <a name="core-sdk"></a>Alapvető SDK

- ApplicationInsights.config fájl már nem requiered. És a NuGet csomag nem hozzáadott. Konfigurációs teljesen adható meg a kódot.
- NuGet csomag már nem fog célok fájl hozzáadása a megoldás. Ezzel a művelettel eltávolítja a DeveloperMode automatikus beállítás hibakeresési építés alatt. DeveloperMode manuálisan kell állítani a kódot.

## <a name="version-017"></a>Verzió 0.17

### <a name="windows-app-sdk"></a>Windows SDK-alkalmazás

- Windows SDK-alkalmazás létrehozása a Windows 10 technical preview ellen, és VIEWBEN 2015 RC univerzális Windows alkalmazásokat támogatja.

### <a name="core-sdk"></a>Alapvető SDK

- TelemetryClient inicializálni a InMemoryChannel, az alapértelmezett.
- Hozzáadott új API `TelemetryClient.Flush()`. Ez a módszer ürítése indítja összes telemetriai jelentkezik, hogy az ügyfél-azonnali blokkoló feltöltése. Feltöltés a folyamat leállítása előtt kézi kiváltó szolgáltatás lehetővé teszi.
- NuGet csomag .net 4.5 cél hozzá. Ez a cél egy külső csoporttól sem (eltávolított BCL és EventSource függőségek).

## <a name="version-016"></a>Verzió 0,16 

2015-04-28 előzetes verzió

- SDK DNX cél platform ahhoz, hogy az [alapvető .NET-keretrendszer](http://www.dotnetfoundation.org/NETCore5) alkalmazások figyelése támogatja.
- Példányai ```TelemetryClient``` gyorsítótárazásának műszerezettségi kulcs eltűnt. Ha műszerezettségi kulcs nem lett beállítva most ```TelemetryClient``` kifejezetten ```InstrumentationKey``` null ad eredményül. Megoldja a problémát beállításakor ```TelemetryConfiguration.Active.InstrumentationKey``` után néhány telemetriai már összegyűjtött, telemetriai modulok például függőség adatgyűjtő, webes kérések webhelycsoport és a teljesítmény Számláló adatgyűjtő új műszerezettségi kulcs fogja használni.

## <a name="version-015"></a>Verzió 0,15

- Új tulajdonság ```Operation.SyntheticSource``` már elérhető a ```TelemetryContext```. Most a telemetriai elemek megjelölése "nem valós felhasználói forgalom", és adja meg, hogy a forgalom létrehozásának módját. Ez a tulajdonság beállításával példaként a próba automatizálási a betöltés próba-forgalmat a különbséget a forgalmat.
- Csatorna logika a külön NuGet Microsoft.ApplicationInsights.PersistenceChannel nevű került. Alapértelmezett csatorna InMemoryChannel új neve
- Új módszer ```TelemetryClient.Flush``` lehetővé teszi, hogy telemetriai elemek kiüríteni a puffer a szinkron

## <a name="version-013"></a>Verzió 0,13

Nem érhető el a korábbi verzióival kibocsátási megjegyzések. 
