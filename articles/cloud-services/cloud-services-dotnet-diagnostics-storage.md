<properties
    pageTitle="Tár nézet diagnosztikai adatok és az Azure tároló |} Microsoft Azure"
    description="Azure diagnosztikai adatok beolvasása az Azure-tárhely és a megtekintéshez"
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor="tysonn" />
<tags
    ms.service="cloud-services"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="robb" />

# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Azure-tárolóban lévő diagnosztikai adatok megtekintése és tárolása

Diagnosztikai adatok nem végleges tárolja kivéve azt át a Microsoft Azure tároló irányító vagy Azure tárhelyet. Egyszer a tároló területen azt megtekintheti egy több használható eszközök.

## <a name="specify-a-storage-account"></a>Adja meg a tárterület-fiók

A tárterület-fiókot az ServiceConfiguration.cscfg fájl használni kívánt ad meg. A kapcsolati karakterlánc, egy konfigurációs beállítások meghatározása a fiók adatait. A következő példa bemutatja a Felhőbeli szolgáltatástól új projektet a Visual Studio által létrehozott alapértelmezett kapcsolati karakterláncot:


```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Módosíthatja a kapcsolati karakterláncot az Azure tároló ügyfélhez fiók adatokat.

Diagnosztikai adatokat gyűjtenek típusától függően a Azure diagnosztika vagy a Blob vagy a táblázat szolgáltatást használ. A következő táblázat mutatja a rendszer adatforrások és formátumukat.

|Adatforrás|Tárolási formátuma|
|---|---|
|Azure naplók|Táblázat|
|IIS 7.0 naplók|BLOB|
|Azure diagnosztika infrastruktúra naplók|Táblázat|
|Nem sikerült kérése a nyomkövetési naplók|BLOB|
|Windows-eseménynaplók|Táblázat|
|Teljesítmény számláló|Táblázat|
|Kiírása összeomlik|BLOB|
|Egyéni hibanaplók|BLOB|

## <a name="transfer-diagnostic-data"></a>Diagnosztikai adatok átvitele

Az SDK 2.5-ös és újabb verzióiban az értekezlet-összehívást diagnosztikai adatok átvitele akkor fordulhat elő, a konfigurációs fájl keresztül. Diagnosztikai adatok átviheti a konfigurációban megadott rendszeres időközönként.

SDK 2.4 és az előző kérhet át az diagnosztikai adatok között, valamint a konfigurációs fájl programozás útján is. A programozási megközelítés is lehetővé teszi, hogy végezze el az igény szerinti átvitelek.


>[AZURE.IMPORTANT] Diagnosztikai adatok átvitele Azure tárterület-fiókkal, ha a diagnosztikai adatok által használt tárterület erőforrások költségeinek merülnek fel.

## <a name="store-diagnostic-data"></a>Diagnosztikai adatok tárolására

Naplóadatok Blob vagy a táblázat tárterület az alábbi nevekkel tárolódnak:

**Táblák**

- **WadLogsTable** - megírt kód segítségével a nyomkövetés-figyelő naplók.

- **WADDiagnosticInfrastructureLogsTable** - diagnosztikai monitor és konfigurációs változik.

- **WADDirectoriesTable** – a diagnosztikai monitor figyeli könyvtárak.  Ide tartoznak a IIS naplók, az IIS nem sikerült a kérelem naplók és egyéni könyvtárak.  A tároló mezőben megadott a napló blob-fájl helyét és nevét a blob a RelativePath mezőben.  A AbsolutePath mező azt jelzi a helyét és nevét, a fájl, ahogy azt az Azure virtuális gépen létezik.

- **WADPerformanceCountersTable** – teljesítmény számláló.

- **WADWindowsEventLogsTable** – a Windows-eseménynaplók.

**BLOB**

- **a tároló vezérlő üvegvatta** – (csak az SDK 2.4 és az előző) tartalmaz, amely meghatározza az Azure diagnosztika XML-konfigurációs fájlokat.

- naplózza az **üvegvatta-iis-failedreqlogfiles** – IIS nem sikerült kérése adatait tartalmazza.

- **üvegvatta-iis-naplófájlok** – információt tartalmaz az IIS naplózza.

- **"custom"** – alapján, hogy a rendszer ellenőrzi a diagnosztikai monitor könyvtárak beállítása egyéni tároló.  WADDirectoriesTable fog kell megadni ebben a blob-tárolóban nevét.

## <a name="tools-to-view-diagnostic-data"></a>Diagnosztikai adatok megtekintéséhez eszközök
Számos eszközt után meg szeretné az adatokat tároló átkerül érhetők el. Példa:

- A Visual Studio - kiszolgáló Explorer Ha telepítve van az Azure-eszközök a Microsoft Visual Studio is használhatja az Azure tároló csomópontot a kiszolgáló Intézőben csak olvasható blob és a táblázat-adatok megtekintése az Azure tárterület-fiókból. A helyi tároló irányító fiókjából megjelenítheti az adatok és is tároló fiókokból létrehozott Azure. További tudnivalókért lásd: a [Böngészés és kezelése tároló kiszolgáló Intézővel](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).

- [Microsoft Azure tároló Explorerre](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy különálló alkalmazás, amely lehetővé teszi, hogy könnyen adatokkal végzett munkához Azure-tárhely Windows, OSX vagy Linux rendszerhez.

- [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) Azure diagnosztika Manager, amely lehetővé teszi, hogy megtekintése, töltse le és Azure az alkalmazások által gyűjtött diagnosztikai adatok kezelése tartalmazza.


## <a name="next-steps"></a>Következő lépések

[Nyomon követheti a folyamat az Azure diagnosztika Cloud Services alkalmazásban](cloud-services-dotnet-diagnostics-trace-flow.md)
