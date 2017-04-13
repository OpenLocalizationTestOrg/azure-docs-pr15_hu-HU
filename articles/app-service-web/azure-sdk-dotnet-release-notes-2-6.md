<properties 
   pageTitle="A .NET rendszerhez 2.6 Azure SDK – kibocsátási megjegyzések" 
   description="A .NET rendszerhez 2,6 Azure SDK programra vonatkozó kibocsátási megjegyzések" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>A .NET rendszerhez 2.6 Azure SDK – kibocsátási megjegyzések

A dokumentum tartalmaz a kibocsátási megjegyzések az Azure SDK .NET 2.6 kiadását. 

Azure SDK 2.6 fejleszthet 4.5.2 .NET vagy .NET 4.6 kiválasztásával, feltéve, hogy a Felhőbeli szolgáltatások szerepkör a manuális telepítés a cél .NET-keretrendszer a felhőben szolgáltatásalkalmazások (PaaS). [Egy felhőalapú szolgáltatás szerepkörei .NET telepítése](http://go.microsoft.com/fwlink/?LinkID=309796)című témakörben tájékozódhat.


##<a name="service-bus-updates"></a>Szolgáltatás Bus frissítések

- Esemény hubok: 

    - Most már lehetővé teszi, hogy a célzott hozzáférés-vezérlés események által további publisher végpont közzéteszi esemény hubok küldésekor.
    - További stabilitás és fokozása esemény hubok szolgáltatás hozzáadni.
    - Támogatás Amqp protokoll hozzáadása fölé WebSocket üzenetkezelés és esemény.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>HDInsight Tools for Visual Studio frissítése

- **Az IntelliSense finomító**: távoli metaadatok javaslatok

    HDInsight Tools for Visual Studio támogatja keresztüli távoli metaadatok a struktúra parancsfájl szerkesztésekor. Például * *VÁLASSZA a* feladó*írhat * és a táblázatok neve meg fog jelenni. Az oszlopnevek is, miután megadta a táblázat fog megjelenni.

- **HDInsight irányító támogatás**

    Most HDInsight Tools for Visual Studio HDInsight irányító kapcsolódás támogatja-e, és a struktúra parancsfájlok helyileg sikerült fejleszt költséget, anélkül, hogy ezután hajtsa végre ezeket a HDInsight fürt parancsprogramok. 

    További tudnivalókért tekintse át [a kézi választógombot](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **Általános Hadoop fürt támogatása HDInsight Tools for Visual Studio** (Előzetes verzió)

    HDInsight Tools for Visual Studio mostantól általános Hadoop fürt, így HDInsight Tools for Visual Studio segítségével tegye a következőket:

    - Csatlakozzon a fürthöz 
    - Írja be az IntelliSense/automatikus-kiegészítési továbbfejlesztett-támogatással struktúra lekérdezés 
    - az összes feladatok megtekintése egy intuitív felhasználói felület és a fürt. 

    További tudnivalókért tekintse át [a kézi választógombot](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Szerepkör a gyorsítótár-frissítések

- **Szerepkör a gyorsítótár** frissült a **Microsoft Azure tároló SDK** 4.3 verzióját használja. Most, amíg a **Szerepkör a gyorsítótár** használta Azure tároló SDK verzió 1.7.

    Ügyfelek Azure SDK 2,5 vagy az alábbi kell Azure SDK 2.6 frissítése és áthelyezése az Azure tárolás SDK új verziója. 

    Most az Azure tároló verzió 2011-08-18 eltávolítjuk 2016 augusztus 1 van ütemezve. Azure SDK 2,5 vagy az alábbi szerepkör a gyorsítótár minden olyan áttelepítések 2.6 kell teljes ez idő szerint. Az Azure-tárterületét verzió 2011-08-18 elavulása kapcsolatos további tudnivalókért lásd [Microsoft Azure tároló szolgáltatás eltávolítása Verziófrissítés: 2016 bővítmény](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Azt is kihirdetése a 2016 November 30., elavulása Azure felügyelt gyorsítótár-szolgáltatás és Azure szerepkör a gyorsítótár. Azt javasoljuk, hogy áttelepítése a Azure vgx.dll gyorsítótárba a Felkészülés a elavulása. Dátumok és áttérési útmutató a további tudnivalókért lásd: [amely Azure-gyorsítótár ajánlja fel a megfelelő számomra?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Azure alkalmazás szolgáltatás eszközök

Az alábbi elemek lettek frissítve a Azure SDK 2.6 verzióban.

- Ha meg szeretné jeleníteni az Azure API-alkalmazások telepítési cél fokozott Azure közzététel.
- API alkalmazások funkciók kiépítési API-alkalmazás létrehozása és a kiépítési funkcionalitással való engedélyezése a felhasználóknak.
- Kiszolgáló Explorer megváltozott megfelelően az új alkalmazás szolgáltatás csomópontot, erőforráscsoport szerint csoportosított webhelyén, a Mobile és a API alkalmazással.
- Adja hozzá az Azure API-alkalmazás ügyfél kézmozdulat hozzáadni a legtöbb C# projektet, amely lehetővé teszi a automatikus létrehozása Swagger engedélyező API alkalmazások fut a felhasználó Azure-előfizetésben.
- API-alkalmazások szerszámok és Server Explorer csomópontok alkalmazás szolgáltatás csak állnak rendelkezésre a Visual Studio 2013-ban. 

##<a name="azure-resource-manager-tools-updates"></a>Azure erőforrás-kezelő eszközök frissítések

Az Azure erőforrás-kezelő eszközök a sablonok tartalmazzák a virtuális gépeken futó, hálózati és tárolási frissített. A JSON élmény szerkesztése amelyet fel szeretne venni egy új vázlat nézetben sablonok és szerkeszthetik a sablonok használatával JSON kódrészletek változásáról. Visual Studio telepített sablonok használata a project által biztosított, hogy a parancsprogram módosításai fogja használni a Visual Studio egy PowerShell-parancsprogramot.

##<a name="diagnostics-improvements-for-cloud-services"></a>A Cloud Services diagnosztikai javítása

Azure SDK 2,6 életre vissza az Azure számítási irányító a diagnosztikai naplók összegyűjtése és őket átvitele fejlesztési tároló támogatása. Bármely a diagnosztikai naplók (beleértve az alkalmazás nyomkövetési naplók, esemény (esemény-nyomkövetés) Windows-naplók pontra, a teljesítmény számláló, infrastruktúra nyomkövetési naplók és a windows-eseménynaplók) létrehozott amikor az alkalmazás fut a irányító átirányítható fejlesztési tároló ellenőrizze, hogy működik-e a diagnosztikai naplózás a helyi számítógépre. 

A diagnosztikai tároló fiók is a könnyebb olvashatóság érdekében különböző diagnosztika tároló fiókok használata különböző környezetekben (.cscfg) konfigurációs fájl megadott. Hogyan munka közben a kapcsolati karakterláncot az Azure SDK 2.4 és Azure SDK 2.6 között néhány főbb különbségek vannak. A diagnosztikai tároló kapcsolat használatáról további információt karakterláncot, és milyen hatással van a projektek talál [Az Azure Cloud Services konfigurálása diagnosztika](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Módosítások megszakítása

###<a name="azure-resource-manager-tools"></a>Azure erőforrás-kezelő eszközök 

- A **Felhőalapú környezetben projektek** projekttípus érhető el az Azure SDK 2,5 átnevezése **Azure erőforrás**csoporthoz.
- **Felhőalapú környezetben projektek** típusú az Azure SDK 2,5 készült projekteket is használható, 2.6, de üzembe helyezné a Visual Studio sablon sikertelen lesz. Azonban üzembe helyezése a PowerShell-parancsprogramot is működnek még mint korábban.  Használatáról bővebben **Felhőalapú környezetben projektek** 2.6 olvassa el a [tartalmakat](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Ismert problémák

- A 64 bites operációs rendszert gyűjtése a irányító a diagnosztikai naplók szükséges. Diagnosztikai naplók nem lesznek összegyűjtve, amikor a 32 bites operációs rendszeren futó. Ez bármilyen egyéb irányító funkciók nem befolyásolja. 

- Azure SDK 2.6 megjelent és a 4/29/2015 volna két problémákat: 

    - Univerzális alkalmazás nem tölthető Visual Studio 2015, amikor az Azure SDK 2.6 volt telepítve a számítógépen.
    - Hibakeresési egy felhőalapú szolgáltatás project 2013-ban Visual Studio és Visual Studio 2015, ahol Visual Studio nem válaszol, és az üzenettel "Konfigurálása diagnosztika irányító" párbeszédpanel megjelenítéséhez összeomlik meghiúsul.
    
    Azure SDK 2.6 frissítésére van szüksége a 5/18/2015 jelent meg. A frissített verziója nem 2.6.30508.1601; a fenti két problémák javításokat tartalmaz. A Vezérlőpultról SDK összeállítása megállapítható -> Programok és szolgáltatások-Microsoft Azure-eszközök > a Microsoft Visual Studio 2013 – v 2.6. Verzió oszlopban jelennek meg a Szerkesztés számot.

    Ha továbbra is a fenti problémákkal szemben lévő, [VIEWBEN 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [és 2013-ban](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy a [VIEWBEN 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)telepítése az Azure 2.6 SDK legújabb verzióját.
 
##<a name="see-also"></a>Lásd még:

[Támogatási és elavulása adatait az Azure SDK .NET és az API-hoz](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
