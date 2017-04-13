<properties
    pageTitle="Azure diagnosztika korábbi verziók"
    description="Változások a különböző verzióiban Azure diagnosztika mint szállított Microsoft Azure SDK különböző verzióival magyarázata."
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Microsoft Azure diagnosztika korábbi verziók

Új Azure diagnosztika? [Azure diagnosztika áttekintése](azure-diagnostics.md)című témakörben találhat.

Minden verzió Azure SDK általában szállított Azure diagnosztika új verziója. Az alábbi táblázat ismerteti a SDK megjelenési társított Azure SDK és Azure diagnosztika verziók.



Azure SDK verziója | Azure diagnosztika verziója | Modell
--- | --- | ---
1.x      | 1.0 | a beépülő modul
2.0-2.4| 1.0 | "
2,5      | az 1.2 | bővítmény
2.6      | az 1.3 | "
2.7.      | 1.4 | "
2,8      | az 1,5 | "


A legújabb verziója 1,5, amely az Azure SDK 2,8 teljesült. Az Azure diagnosztika kiterjesztés a SDK képező verziója csak a helyi irányító esetek használatos. Bármely telepített alkalmazás automatikusan legújabb verzióját használja, az Azure-fut, függetlenül attól, amely a SDK csomagjában talál az alkalmazás verziójának helyére. Jó helyen jár, kivéve, ha telepíti a legújabb Azure SDK csomagjában talál, akkor valószínűleg nincs összes az eszközök, amelyek segítségével csatlakozást az új szolgáltatásokat.

Különböző szolgáltatások és a változások leírása alább.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure SDK 2,8 hozzá, az azt jelenti, hogy küldje el a diagnosztikai adatok egyszerűsítése problémáinak diagnosztizálása végig az alkalmazásokat, valamint a rendszer és infrastruktúra szint [Alkalmazás az összefüggéseket](./application-insights/app-insights-cloudservices.md) .

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnosztika módosítása

Azure SDK 2.6 Felhőszolgáltatásában projektekhez a Visual Studióban az alábbi módosításokat végzett. (A módosítások vonatkoznak az Azure SDK újabb verzióiban.)

- A helyi irányító diagnosztika támogatja. Ez azt jelenti, hogy diagnosztika adatgyűjtés, és győződjön meg róla, az alkalmazás hoz létre a jobb oldali halad, miközben Ön fejlesztése, és a Visual Studio alkalmazásban tesztelje. A kapcsolati karakterlánc `UseDevelopmentStorage=true` lehetővé teszi, hogy diagnosztika adatgyűjtés futtatása közben a felhőalapú szolgáltatás project a Visual Studióban az Azure tároló irányító használatával. Az összes diagnosztika adatgyűjtés tároló fiók (Development tároló).

- A szolgáltatás beállítása (.cscfg) fájlt a diagnosztika tároló fiók kapcsolati karakterlánc (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) még egyszer vannak tárolva. Az Azure SDK 2,5 megadva a diagnosztika tárterület-fiók a diagnostics.wadcfgx fájlt.

Hogyan a kapcsolati karakterlánc művelet sikerült, az Azure SDK 2.4 és a korábbi verziók, és hogy hogyan működik az Azure SDK 2,6 és újabb verzióiban között néhány főbb különbségek vannak.

- Az Azure SDK 2.4 és korábbi verzióiban a kapcsolati karakterlánc használtak, egy futtatókörnyezet a diagnosztika beépülő modul beszerzése a tárterület-fiók adatait a diagnosztikai naplók átviteléhez.

- Az Azure SDK 2.6 és újabb verzióiban a diagnosztikai bővítmény állítható be a megfelelő tárolási fiókadatok közzététel során az a diagnosztika kapcsolati karakterlánc használja a Visual Studio. A kapcsolati karakterlánc meghatározhatja, hogy más-más tárolási fiókok szerepeltethet konfigurációk, amelyekkel a Visual Studio közzétételekor. A diagnosztikai beépülő modul már nem érhető el (után Azure SDK 2,5), mivel azonban a .cscfg fájl saját maga által engedélyezése nem a diagnosztika bővítmény. Engedélyeznie kell a bővítmény külön-külön eszközök, például a Visual Studio vagy PowerShell keresztül.

- A diagnosztikai kiterjesztése a PowerShell egyszerűbbé a csomag kimeneti Visual Studio minden szerepkörhöz diagnosztika bővítményhez nyilvános konfigurációs XML is tartalmaz. Visual Studio a diagnosztika kapcsolati karakterláncot a tárhely fiók adatait a nyilvános konfigurációs bemutató feltöltéséhez használja. A nyilvános config fájlok létrehozásakor a bővítmények mappára a, és kövesse a mintázat PaaSDiagnostics. <RoleName>. PubConfig.xml. Bármely PowerShell-alapú telepítés segítségével a minta megfeleltetése egyes kereséskonfigurációs szerepkörbe.

- A kapcsolati karakterláncot az .cscfg fájlban is használják az Azure-portálra, is jelenjenek meg az **ellenőrzési** lap diagnosztikai adatok eléréséhez. A részletes felügyeleti adatok megjelenítéséhez a portálon szolgáltatás konfigurálása a kapcsolati karakterlánc van szükség.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Azure SDK 2.6 és újabb verzióiban áttelepítése projektek

Fiókneveket Azure SDK 2,5 Azure SDK 2,6 vagy újabb, ha egy .wadcfgx fájl a megadott diagnosztika tárterület-fiókot, majd marad van. Más-más tárolási fiókok különböző tároló konfigurációk rugalmasan kihasználhatja meg kell a kapcsolati karakterlánc manuális hozzáadása a projekthez. Ha a projekt éppen áttelepítette, Azure SDK 2.4 vagy korábbi verzióról Azure SDK 2.6, a diagnosztikai a csatlakozási_karakterlánc megőrződnek. Jó helyen jár tartsa szem előtt a változások hogyan csatlakozási_karakterlánc számít Azure SDK 2.6 adott az előző szakaszban.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hogyan Visual Studio határozza meg, hogy a diagnosztika tárterület-fiók

- Ha a .cscfg fájlban diagnosztika kapcsolati karakterlánc megadott, Visual Studio használja konfigurálása a diagnosztika bővítmény, közzététele, valamint a nyilvános konfigurációs XML-fájlok létrehozása során összecsomagolása esetén.

- Ha nincs diagnosztika kapcsolati karakterláncot az .cscfg fájlban megadva, majd a Visual Studio visszavált fiókkal a tárterület a .wadcfgx fájl a megadott konfigurálása a diagnosztika bővítmény, amikor közzététele, valamint a nyilvános konfigurációs XML-fájlok létrehozása, amikor csomagolása.

- A diagnosztikai kapcsolati karakterláncot az .cscfg fájlban bontásakor a tárterület-fiók a .wadcfgx fájlt. Ha diagnosztika kapcsolati karakterlánc megadva a .cscfg fájlban, Visual Studio jeleníti meg, és figyelmen kívül hagyja a .wadcfgx a tárterület-fiók.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Mit jelent a "frissítés fejlesztési tárhely a csatlakozási_karakterlánc..." jelölőnégyzet tenni?

A **frissítés fejlesztési tároló kapcsolat** karakterláncok diagnosztika és a Microsoft Azure tároló fiók hitelesítő adatait, a Microsoft Azure való közzétételkor gyorsítótárazás jelölőnégyzetet bármely fejlesztési tároló fiók csatlakozási_karakterlánc frissítse a közzététel során megadott Azure tároló fiók kényelmesen ad.

Tegyük fel például, hogy, jelölje be ezt a jelölőnégyzetet, és a diagnosztikai kapcsolati karakterlánc megadja `UseDevelopmentStorage=true`. Azure tegye közzé a projektet, amikor a Visual Studio automatikusan frissíti a diagnosztika kapcsolati karakterláncot a tárterület-fióknak a közzététel varázslóban megadott. Azonban, ha egy valós tárhely a diagnosztika csatlakozási karakterlánc lett megadva, majd a fiók használja helyette.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnosztikai funkciók eltérések Azure SDK 2.4 és korábbi és Azure SDK 2.5-ös és újabb verziók

Ha frissít a projekt az Azure SDK 2.4 Azure SDK 2.5-ös vagy újabb verziójával, akkor tudatában kell lennie, az alábbi diagnosztikai funkciók különbségeket.

- **Konfigurációs API -khoz is elavult** – diagnosztika programozott konfigurációs Azure SDK 2.4 és korábbi verzióiban érhető el, de az Azure SDK 2.5-ös és újabb verzióiban elavult. Ha a diagnosztikai konfigurációja kód jelenleg definiált kell újrakonfigurálni ezeket a beállításokat az alapoktól az áttelepített projektben ahhoz, hogy diagnosztika folytathatja a munkát a. A diagnosztikai konfigurációs fájl az Azure SDK 2.4 diagnostics.wadcfg, de az Azure SDK 2.5-ös és újabb verzióiban diagnostics.wadcfgx.

- **Az felhő szolgáltatásalkalmazások diagnosztika csak szerepkör szintre, de a példány szintjén nem állítható be.**

- **Minden alkalommal telepíti az alkalmazást, a diagnosztikai konfigurációja frissül** – eltérés áll fenn problémák léphetnek fel, ha a diagnosztikai konfigurációja megváltoztatja a kiszolgáló Intézőből és majd telepítsen újra az alkalmazást.

- **Az Azure SDK 2.5-ös és újabb, összeomlik kiírása van konfigurálva, a diagnosztikai konfigurációs fájl nem található a** – ha van konfigurálva a kód összeomlik kiírása, is be kell manuálisan át a konfigurációs kódot, hogy a konfigurációs fájl, mert az összeomlást kiírása Azure SDK 2.6 az áttelepítés során nem át.
