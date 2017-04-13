<properties
   pageTitle="Első lépések az Azure napló integrációs |} Microsoft Azure"
   description="Megtudhatja, hogy miként telepítse az Azure napló integrációs szolgáltatást, és integrálása az Azure tárhely, az Azure naplókat és Azure biztonság otthon riasztások naplók."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Első lépések az Azure napló integrációs (előzetes verzió)

Azure napló integráció lehetővé teszi az Azure erőforrásoktól nyers naplók integrálása a helyszíni biztonsági információk és esemény-kezelés (SIEM) rendszerek. Ez az integráció biztosítja egységesített irányítópult az összes eszközeit, a helyszíni, vagy a felhőben, hogy az összesítő, összehangolására, elemezheti, és a felhasználó-alkalmazásokkal kapcsolatos biztonsági események.

Ebben az oktatóanyagban végigvezeti az Azure napló integrációs telepítése, és integrálása az Azure tárhely, az Azure naplókat és Azure biztonság otthon riasztások naplók. Ebben az oktatóanyagban befejezéséhez becsült ideje egy órával.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez az alábbiakat kell rendelkeznie:

- A gép (helyszíni, vagy a felhőben) az Azure napló integrációs szolgáltatás telepítése. Ezen a számítógépen egy 64 bites Windows operációs rendszer a .net telepített 4.5.1 rendszernek kell futnia. Ez a gép az **Azlog integrátor**neve.
- Azure előfizetés. Ha nem rendelkezik egy, jelentkezzen [ingyenes fiókot](https://azure.microsoft.com/free/).
- Azure diagnosztika engedélyezve van az Azure virtuális gépeken futó (VMs). Diagnosztikai Cloud Services engedélyezése, olvassa el a [Azure diagnosztika segítségével, így az Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md)című témakört. Egy Windows rendszerű Azure virtuális diagnosztika engedélyezéséhez a [PowerShell használatá engedélyezése egy virtuális gépen futó Windows Azure diagnosztika](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)című.
- Kapcsolat a Azlog integrátor Azure tárhely és a hitelesítheti és engedélyezheti az Azure-előfizetéséhez.
- Azure virtuális naplók esetén a SIEM ügynök (például Splunk univerzális továbbító, HP ArcSight Windows esemény adatgyűjtő ügynök vagy IBM QRadar WinCollect) telepítenie kell az Azlog integrátor.

## <a name="deployment-considerations"></a>Telepítéssel kapcsolatos szempontok

Ha az esemény hangja fel hangosítva futtatását is lehetővé teszi az Azlog integrátor több példányát. Terheléselosztás *(ÜVEGVATTA)* Windows Azure diagnosztika tároló fiókok és előfizetések ahhoz, hogy a példányok száma a kapacitás alapuljon.

8-processzor (core) számítógépen egy példányát és Azlog integrátor is dolgozható körülbelül 24 millió napi (~1M/hour).

A 4-processzor (core) gépen Azlog integrátor egyetlen példánya is dolgozható körülbelül 1,5 millió napi (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Telepítse az Azure napló-integráció

Töltse le a [Azure jelentkezzen integráció](https://www.microsoft.com/download/details.aspx?id=53324).

Az Azure naplózási integrációs szolgáltatás telemetriai adatokat gyűjt a a számítógépről, amelyen telepítve van.  Összegyűjtött telemetriai adatokat a következő:

- Azure végrehajtása során fellépő kivételek jelentkezzen integrációja
- Tudnivalók a lekérdezések és feldolgozása események száma mérőszámok
- Melyik Azlog.exe kapcsolatos parancssori kapcsolói használják statisztika

> [AZURE.NOTE] Adatgyűjtés telemetriai kikapcsolhatja ezt a beállítást jelölésének törlése.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integrálása az Azure diagnosztika tároló fiókjaiból Azure virtuális naplók

1. Jelölje be a fent felsorolt, annak érdekében, hogy ÜVEGVATTA tárterület-fiókját az Azure napló integráció a továbblépés előtt gyűjt naplók előfeltételek. Hajtsa végre az alábbi lépéseket, ha a ÜVEGVATTA tárterület-fiók nem gyűjt naplók.

2. Nyissa meg a parancssor és **CD-re** **c:\Program Files\Microsoft Azure napló integrációs**be.

3. A parancs futtatása

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      StorageAccountName cserélje le a virtuális diagnosztika események fogadására Azure tárterület-fiók nevére.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Ha szeretné, hogy az előfizetés azonosítója jelenjen meg az Eseménynapló XML, a felhasználóbarát név Hozzáfűzés az előfizetés azonosítója:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Csak a 30-60 perccel (is eltelhet mindaddig óránként), majd a tárterület-fiók kikerülnek eseményeket megtekintése. Megtekintéséhez nyissa meg **Eseménynapló > a Windows-naplók > továbbított események** az Azlog integrátor a.

5. Győződjön meg arról, hogy a szabványos SIEM összekötő a számítógépen telepítve van-e beállítva események kiválasztása a **Továbbított események** mappából, és a SIEM példányához pipe őket. Tekintse át a SIEM konkrét konfigurációja konfigurálása és a naplókat integrálása.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Mi történik, ha adat, nem jelenik meg a továbbított események mappa?

Ha egy óra után adatokat, nem jelenik meg a **Továbbított események** mappába, majd:

1. Jelölje be a számítógépre, és győződjön meg arról, hogy elérhető-e az Azure. Kapcsolat teszteléséhez próbál megnyitni az [Azure portált](http://portal.azure.com) a böngészőből.

2. Győződjön meg arról, hogy a felhasználói fiók **azlog** írási engedélye van a mappa **users\azlog**.

3. Ellenőrizze, hogy szerepel a listában a tárhely fiók hozzáadva az **azlog forrás hozzáadása** parancs a parancs **azlog forráslista**futtatásakor.
4. Nyissa meg a **Eseménynapló > a Windows-naplók > alkalmazás** kattintva megtekintheti, hogy vannak-e a hibák jelentett az Azure napló integráció.

Ha még nem látható az események, majd:

1. Töltse le a [Microsoft Azure tároló Explorer](http://storageexplorer.com/).

2. Csatlakozás a tárhely fiók hozzáadott **azlog forrás hozzáadása**parancsot.

3. A Microsoft Azure tároló Intézőben keresse meg táblázat **WADWindowsEventLogsTable** megtudhatja, ha semmilyen adatot. Ha nem, majd a virtuális a diagnosztika nincs megfelelően beállítva.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrálása az Azure naplókat és a biztonság otthon értesítések

1. Nyissa meg a parancssor és **CD-re** **c:\Program Files\Microsoft Azure napló integrációs**be.

2. A parancs futtatása

        azlog createazureid

      Ez a parancs kéri az Azure jelentkezzen be. A parancs majd hoz létre az [Azure Active Directory szolgáltatás egyszerű](../active-directory/active-directory-application-objects.md) az Azure AD-bérlők, amely az Azure előfizetések, amelyben a bejelentkezett felhasználó, a rendszergazda, a közös rendszergazdája vagy tulajdonos szolgáltató. A parancs sikertelen lesz a bejelentkezett felhasználó csak az Azure Active Directory-ös bérlői Vendég felhasználó esetén. Azure-hitelesítés történik át Azure Active Directory (AD).  Az Azure Active Directory identitás Azure előfizetésekről olvasási hozzáférést biztosítandó létrehozása egy egyszerű integráció Azlog hoz létre.

3. A parancs futtatása

        azlog authorize <SubscriptionID>

      Ez a olvasó elérésének az előfizetést, hogy a szolgáltatás fő lépés: 2 létrehozott rendeli hozzá. Ha egy SubscriptionID nem adja meg, majd megkísérli hozzárendelése a szolgáltatás fő olvasó szerepkört, összes előfizetést, amelyhez bármely hozzáférése van.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Figyelmeztetések jelenhetnek meg a **engedélyezheti** parancs futtatása közvetlenül azután, hogy a **createazureid** parancsot. Van néhány késés az Azure Active Directory-fiók létrehozásakor, és a fiók esetén használható között. Ha a **createazureid** parancs futtatása a **engedélyezheti** parancs futtatása után körülbelül 10 másodperc várakozás, majd nem kellene látnia ilyen figyelmeztetések.

4. Jelölje be a következő mappák győződjön meg arról, hogy a JSON naplófájlok van:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Jelölje be az alábbi mappák kattintva erősítse meg, hogy biztonság otthon riasztások megtalálható őket:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. A szokásos SIEM továbbító összekötő mutasson az adatokat az SIEM példány pipe a megfelelő mappát. Előfordulhat, hogy bizonyos mező megfeleltetésének a SIEM termék alapján.

Ha Azure Log integrációs kérdéseket, küldjön e-mailben [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban telepítse az Azure napló integráció és integrálása az Azure tárhelyről naplók megtanulta azt. További információért olvassa el az alábbiakat:

- [Microsoft Azure napló integrálása az Azure naplók (előzetes verzió)](https://www.microsoft.com/download/details.aspx?id=53324) – a letöltőközpontból információ, rendszerkövetelmények és Azure napló integráció a telepítési utasításokat.
- [Bevezetés az Azure napló integrációba](security-azure-log-integration-overview.md) – ehhez a dokumentumhoz bemutatja a Azure napló integrációs főbb funkciói és működéséről.
- [Partner konfigurálási lépéseket](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – blogbejegyzésben bemutatja, hogyan Azure napló integráció a partnerek megoldásaival Splunk, HP ArcSight és IBM QRadar végezhető.
- [Azure napló integrációs gyakran ismételt kérdések](security-azure-log-integration-faq.md) – a Gyakori kérdésekre ad választ Azure napló integrálása.
- [Az Azure integrálása biztonság otthon riasztások jelentkezzen integrációs](../security-center/security-center-integrating-alerts-with-log-integration.md) – ehhez a dokumentumhoz megtudhatja, hogy miként szinkronizálhatja a biztonság otthon riasztások virtuális gép biztonsági események gyűjti össze a Azure diagnosztikai és a naplókat Azure, SIEM megoldás vagy a napló analytics együtt.
- [Azure diagnosztikai és Azure naplókat új szolgáltatásai](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – blogbejegyzésben megismerkedhet Azure naplókat, és más szolgáltatásokat nyújt szerezhet be az Azure erőforrások műveletek az összefüggéseket.
