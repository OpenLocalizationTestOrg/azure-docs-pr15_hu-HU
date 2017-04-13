<properties
    pageTitle="Mi az a Azure .NET SDK"
    description="Megtudhatja, hogy mi minden található az Azure .NET SDK."
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"
    services=""/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="what-is-the-azure-sdk-for-net"></a>Mi az a Azure SDK csomagjában talál a .NET rendszerhez?

## <a name="overview"></a>– Áttekintés

A .NET rendszerhez az Azure SDK Visual Studio eszközök, parancssori eszközök, futtatókörnyezet bináris és ügyfél tárakat, amelyek segítségével fejlesztése, tesztelése és az Azure-ban futó apps telepítése. Ez a cikk részletes mi kap a SDK telepítésekor. A SDK letöltheti a [Azure letöltési lapját](https://azure.microsoft.com/downloads/).

A .NET rendszerhez az Azure SDK is magában foglalja a [igénybe Azure szolgáltatások ügyfél-tárakban](http://go.microsoft.com/fwlink/?LinkId=510472). A tárak telepített NuGet külön-külön használatával.

##<a id="included"></a>Mit tartalmaz az az Azure SDK a .NET rendszerhez

A .NET rendszerhez az Azure SDK telepíti, az alábbi termékek:

- [Visual Studio közösségi Edition 2015](#vwd)
- [Microsoft Azure tároló irányító](#stgemulator)
- [Microsoft Azure tároló eszközök](#stgtools)
- [Microsoft Azure létrehozáshoz használható eszközök](#auth)
- [Microsoft Azure irányító](#emulator)
- [HDInsight Tools for Visual Studio és a Microsoft-struktúra ODBC-illesztőprogram](#hdinsight)
- [Microsoft Azure-tárak a .NET rendszerhez](#libraries)
- [Microsoft Azure mobil alkalmazás SDK](#mobile)
- [Microsoft Azure PowerShell](#ps)
- [Microsoft Azure-eszközök a Microsoft Visual Studio](#tools)
- [A Microsoft ASP.NET- és webes Tools for Visual Studio](#wte)
- [Microsoft Azure adatok tó Tools for Visual Studio](#datalake)

###<a id="vwd"></a>Visual Studio közösségi Edition 2015

Visual Studio nincs a számítógépen, a SDK telepíti a [Visual Studio közösségi Edition 2015](https://www.visualstudio.com/products/visual-studio-community-vs).

###<a id="stgemulator"></a>Microsoft Azure tároló irányító

Az [Azure tároló irányító](http://msdn.microsoft.com/library/hh403989.aspx) Azure tároló (sorban várakozó, táblázatokat, BLOB), hasonlóan az, hogy a helyi meghajtóra tesztelheti használja az SQL Server-példányt, és a helyi fájlrendszerben.

###<a id="stgtools"></a>Microsoft Azure tároló eszközök

Ez a telepítést [AzCopy](http://aka.ms/AzCopy)és az adatátviteli-ellenőrzés be- és kijelentkezés a Azure tároló fiók segítségével parancssori eszköz.

###<a id="auth"></a>Microsoft Azure létrehozáshoz használható eszközök

Ide tartoznak az alábbiak:

* A [CSPack parancssori eszköz](http://msdn.microsoft.com/library/gg432988.aspx) a telepítőcsomag.
* a [CSEncrypt parancssori eszköz](http://msdn.microsoft.com/library/hh404001.aspx) titkosítására távoli asztali kapcsolaton keresztül felhőalapú szolgáltatás szerepkör példányainak eléréséhez használt jelszót.
* A felhő szolgáltatás projektek futtatókörnyezet bináris azok futtatókörnyezet kommunikáció és a diagnosztikai szükség. Ezek a bináris NuGet csomagokban nem érhetők el.

###<a id="emulator"></a>Microsoft Azure irányító

Az [Azure irányító](http://msdn.microsoft.com/library/dn339018.aspx) keltő szűrő megőrzi a felhőben szolgáltatási környezetben, hogy tesztelheti felhőalapú szolgáltatás projektek helyi számítógépen mielőtt beállítaná őket Azure.

###<a id="hdinsight"></a>HDInsight Tools for Visual Studio és a Microsoft-struktúra ODBC-illesztő

A kiszolgáló Intézőben HDInsight eszközök lehetővé teszi nyissa meg a struktúra adatbázisok és csatolt tároló fiókok HDInsight fürtre vonatkozóan, táblázatok, létrehozása és létrehozása és elküldése struktúra lekérdezések. További tudnivalókért olvassa el a [HDInsight Hadoop Tools for Visual Studio használatának első lépései](hdinsight/hdinsight-hadoop-visual-studio-tools-get-started.md)című témakört.

###<a id="libraries"></a>Microsoft Azure-tárak a .NET rendszerhez

Ide tartoznak az alábbiak:

* NuGet csomagjai Azure tárolására, a szolgáltatás Bus és a gyorsítótár, hogy a Visual Studio hozhat létre a új felhőalapú szolgáltatás projektek, miközben a számítógépen tárolt offline.
* A Visual Studio beépülő modul, amely lehetővé teszi, hogy a [Szerepkör a gyorsítótár](http://msdn.microsoft.com/library/dn386103.aspx) projektek Visual Studio helyi futtatásához.

###<a id="mobile"></a>Microsoft Azure mobil alkalmazás SDK

Eszközök [Azure alkalmazás szolgáltatás Mobile-alkalmazások](app-service-mobile/app-service-mobile-value-prop.md)használata a.

###<a id="ps"></a>Microsoft Azure PowerShell

Azure PowerShell automatizálhatja a [Azure környezetben létrehozásának és telepítési](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)lehetővé teszi.

###<a id="tools"></a>Microsoft Azure-eszközök a Microsoft Visual Studio

Ez lehetővé teszi, hogy az Azure erőforrások, elsősorban a Felhőszolgáltatások és a virtuális gépeken futó kezelése:

* [Jelentések létrehozása, megnyitása, és felhőalapú szolgáltatás projektek közzététele](cloud-services/cloud-services-dotnet-get-started.md).
* [Létrehozás telepítőcsomag felhőalapú szolgáltatás projektekhez](http://msdn.microsoft.com/library/ff683672.aspx).
* [Hozzon létre Azure virtuális gépeken futó új webes projektek létrehozása során](virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md).
* [Új virtuális gépeken futó létrehozása közben használható létrehozása PowerShell-parancsprogramok](http://msdn.microsoft.com/library/dn642480.aspx).
* [Megtekintése és kezelése a felhőalapú szolgáltatás a project beállításai a Visual Studio projekt tulajdonságai windows](http://msdn.microsoft.com/library/ee405486.aspx).
* Megtekintheti és kezelheti a [felhőszolgáltatások](http://msdn.microsoft.com/library/ff683675.aspx), [virtuális gépeken futó](http://msdn.microsoft.com/library/jj131259.aspx)és [Szolgáltatás Bus](http://msdn.microsoft.com/library/jj149828.aspx) a kiszolgáló Intézőben.
* [Távolról a felhőszolgáltatások és a virtuális gépeken futó hibakeresési módra](http://msdn.microsoft.com/library/ff683670.aspx).
* [Erőforrás kiépítési Azure erőforrás csoport telepítési projektek automatizálása](https://msdn.microsoft.com/library/dn872471.aspx)

###<a id="wte"></a>A Microsoft alkalmazás szolgáltatás Tools for Visual Studio

Lehetővé teszi, hogy az Azure-webhelyek használata:

* [Közzétételi webhely projektek Azure webhelyekre](app-service-web/web-sites-dotnet-get-started.md).
* [Közzététel konzol alkalmazás projektek Azure WebJobs](app-service-web/websites-dotnet-deploy-webjobs.md).
* [Azure-webhely létrehozása és SQL-adatbázis erőforrások webes új projekt létrehozása során, vagy egy webes projekt közzététele közben](app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
* [Hozzon létre PowerShell telepítésének parancsfájlok új webhelyek létrehozásakor](http://msdn.microsoft.com/library/dn642480.aspx).
* [Kezelése és hibaelhárítása – Azure webhelyek, a kiszolgáló Explorer](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#sitemanagement).
* [Távolról a webhelyekre és WebJobs hibakeresési módra](app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).

>[AZURE.NOTE] Nem kell telepíteni az Azure SDK a .NET rendszerhez szeretne ezekkel a funkciókkal; Visual Studio frissítések azok is szerepelnek.

##<a id="datalake"></a>Microsoft Azure adatok tó Tools for Visual Studio

További tudnivalókért lásd: [oktatóanyag: adatok tó eszközeivel for Visual Studio U – SQL-parancsfájlok kidolgozása](data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

##<a id="notincluded"></a>Mit nem tartalmaz az Azure SDK telepítésekor a .NET rendszerhez

Van néhány dolgot, amelyeket érdemes Azure fejlesztési, amely nem része, ha telepíti a SDK csomagjában talál. A legfontosabb a következők:

* [Ügyfél-tárakban](http://go.microsoft.com/fwlink/?LinkId=510472).

    Az Azure SDK tartalmaz az Azure szolgáltatások használata más ügyfél tárak, de nem az összes telepített a SDK telepítésekor. Ha az alkalmazás nem telepíthető a SDK csomagjában talál az ügyfél tárról, a [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)kattint. Ha az alkalmazás a SDK telepítő ügyfél tárról, tanácsos egy frissítheti a NuGet.org jelenlegi verziójával.

    **Ügyfél-tárak helyi példányainak.** Az Azure SDK a .NET rendszerhez néhány Azure ügyfél tárakat, például tárhely, a szolgáltatás Bus és a gyorsítótár NuGet csomagját másolja a számítógépre. A ügyfél-tárak automatikusan bekerülnek új felhőalapú szolgáltatás projekteket, a helyi NuGet csomagok engedélyezése a Visual Studio projekteket létrehozni, akkor is, ha nem kapcsolódik az internethez. Ügyfél-tárak általában frissülnek, mint a megjelenik az új SDK verziókat, gyakran úgy, hogy az ügyfél-tárak a NuGet.org gyakran több mint beszerzése a SDK csomagjában talál az aktuális.

    **Project-sablonok, amely tartalmazza az ügyfél-tárakban.** Csak a [Azure Felhőbeli szolgáltatástól](cloud-services/cloud-services-dotnet-get-started.md) és Azure alkalmazás szolgáltatás [Mobile-alkalmazások](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) projektsablonok a néhány ügyfél-tárak automatikusan tartalmazni. Más tárak és egyéb sablonokat telepítse az [ügyfél tár NuGet csomagok](http://go.microsoft.com/fwlink/?LinkId=510472) van szüksége.

* [A project sablonok Mobile-alkalmazások](./app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

    Mobile-alkalmazások használhat, amelyek csak a Visual Studio 2013 frissítés 2 vagy újabb verzió. Nem érhető el, Visual Studio 2012 és korábbi verzióiban, nem pedig a Visual Studio 2013 frissítés 1 vagy korábbi, még akkor is, ha a .NET telepítse az Azure SDK csomagjában talál.

##<a id="faq"></a>Gyakori kérdések

- [Sok Azure szolgáltatásokkal már a Visual Studióban. Szükség van telepítse az Azure SDK a .NET rendszerhez?](#azinvs)
- [Szeretném ügyfél tárban. Van az Azure SDK a .NET rendszerhez el telepíteni?](#clientlib)
- [Hol találhatók az Azure SDK régebbi verziói a .NET rendszerhez?](#olderversions)
- [Mi az a életciklusra vonatkozó szabályzata az Azure SDK a .NET rendszerhez verziójához?](#lifecycle)
- [Melyik vendég operációs rendszer verziója van az Azure SDK a .NET rendszerhez kompatibilis?](#guestos)
- [Hogyan tudom eltávolítani az Azure SDK a .NET rendszerhez?](#uninstall)

###<a id="azinvs"></a>Sok Azure szolgáltatásokkal már a Visual Studióban. Szükség van telepítse az Azure SDK a .NET rendszerhez?

Tanácsos telepítse a SDK csomagjában talál, ha a legújabb eszközeivel Azure fejleszthet olyan szeretne. Ha szeretné inkább nem telepíthető a SDK csomagjában talál, ehhez az alábbi feltételek teljesülése esetén:

* A legújabb [Visual Studio Update](http://www.visualstudio.com/downloads/download-visual-studio-vs#DownloadFamilies_5)telepítette.
* Csak az Azure-webhelyek és a Mobile szolgáltatásokat, nem Cloud services vagy a virtuális gépeken futó fejleszt.
* Az alkalmazás nem használható tárterületet, vagy tárterületet használ, de nincs szükség a tárhely irányító vagy a AzCopy eszközt.

###<a id="clientlib"></a>Szeretném ügyfél tárban. Van az Azure SDK a .NET rendszerhez el telepíteni?

A SDK ügyfél tárak telepíti, csak így létrehozhat felhőalapú szolgáltatás projektek még akkor is, ha nem kapcsolódik az internethez. Aktuális ügyfél-tárak a [NuGet.org](http://go.microsoft.com/fwlink/?LinkId=510472)NuGet csomagok érhetők el. További tudnivalókért lásd [Mit nem tartalmaz az Azure SDK a .NET rendszerhez telepítésekor](#notincluded) a dokumentum korábbi.

###<a id="olderversions"></a>Hol találhatók az Azure SDK régebbi verziói a .NET rendszerhez?

Régebbi verziójú című témakör tartalmaz, az [Azure SDK a .NET rendszerhez](https://azure.microsoft.com/downloads/archive-net-downloads/) letöltések oldalt.

###<a id="lifecycle"></a>Mi az a életciklusra vonatkozó szabályzata az Azure SDK a .NET rendszerhez verziójához?

Lásd: [Microsoft Azure Cloud Services támogatási életciklusra vonatkozó szabályzata](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq).

###<a id="guestos"></a>Melyik vendég operációs rendszer verziója van az Azure SDK a .NET rendszerhez kompatibilis?

Lásd: [Azure Vendég OS kiadásairól és SDK kompatibilitási mátrix](http://msdn.microsoft.com/library/ee924680.aspx).

###<a id="uninstall"></a>Hogyan tudom eltávolítani az Azure SDK a .NET rendszerhez?

Minden egyes [az Azure SDK a .NET rendszerhez található](#included) a jelen cikkben felsorolt elem egy külön programot a telepített programok listájában a Windows Vezérlőpult **Programok és szolgáltatások**.  Nincs mód a javításukhoz csoportként; Ha egyes programok külön-külön eltávolítása.

Amikor az Azure SDK a .NET rendszerhez már telepítve van, és új verzió telepítése, akkor általában távolítsa el a régit nincs szükség. A legtöbb esetben a SDK telepítő frissíti a meglévő programok, hanem egy új hozzáadásával, és hagyja a régit.

Azonban el szeretné távolítani egy korábbi verzióját maradványait nem hosszabb-szükség, ha csak az adja meg a korábbi verziószám programok távolítsa el, és csak távolítsa el őket, ha telepítve ugyanazon a program, egy újabb verziójával. Ha például kezelőfelületének a 2,5 2.6 "Microsoft Azure-eszközök a Microsoft Visual Studio 2013" 2,5 és a 2.6 verziója is látható, és a 2.5-ös verziójának. De előfordulhat, hogy csak megjelenik a "Microsoft Azure létrehozáshoz használható eszközök" 2.5-ös verzióját, és nem lehet eltávolítani, hogy megbízható.

Figyelje meg, hogy a program címek **Programok és szolgáltatások** látható verziószámai félrevezető lehet.  Ha például SDK 2.6 verzióját tartalmaz-e "A Microsoft Azure Mobile alkalmazást SDK 1.0" Ha telepíti a SDK csomagjában talál a Visual Studio 2013 és a "Microsoft Azure Mobile alkalmazást SDK 2.0-s" for Visual Studio 2015; a verzió ebben az esetben nem a SDK verziót, de azt jelzi, amelynek a program a Visual Studio verzió vonatkozik.

Ez a cikk nem szerepel a Minden program, amely tartalmazza az Azure SDK minden korábbi verziójának; régebbi SDK előfordul, hogy szerepel a program, hogy azok lapokról újabb verziók ezért, vannak más programok korábbi SDK is eltávolítja. Például 2.5-ös verziójának telepítése "Azure erőforrás kezelő eszközök for Visual Studio", de ez nem ez a cikk listában, mert azt már nem jelenik meg a **Programok és szolgáltatások**különálló alkalmazásként.  Ez a cikk csak a .NET rendszerhez 2.6 verzióját az Azure SDK az szereplő programok sorolja fel.

> **Megjegyzés:** Néhány, amely tartalmazza a SDK programmal is telepíthetők külön-külön más környezetekben, és szükség lehet a még akkor is, ha nincs szükség a teljes SDK csomagjában talál. Ugyanez igaz a programok korábbi SDK által telepített, de azok elhagyják a későbbi SDK verzióiból lehet. Amikor a program eltávolítja, ügyeljen arra, hogy elkerülése érdekében, eltávolítása, amit továbbra is szükség van a számítógépen.



##<a id="versions"></a>Verziók

Megtudhatja, milyen verziójú aktuális vagy régebbi verzióiban letöltése, lásd: a [.NET-verziók Azure SDK](https://azure.microsoft.com/downloads/archive-net-downloads/) lapra.

##<a id="resources"></a>Erőforrások

Az aktuális Azure SDK .NET vagy ügyfél lévő letöltéséhez látogasson el az [Azure letöltési lapját](https://azure.microsoft.com/downloads/).

A .NET forráskód ügyfél tárak, beleértve az Azure SDK [GitHub.com/Azure](https://github.com/azure/)című témakör tartalmaz.

Azure ügyfél tár hivatkozási dokumentáció [Azure .NET – segédlet](https://azure.microsoft.com/documentation/api/)című cikkben.
