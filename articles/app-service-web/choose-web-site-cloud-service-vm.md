<properties
    pageTitle="Azure alkalmazás szolgáltatás, virtuális gépeken futó, szolgáltatás háló és Cloud Services összehasonlító |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure alkalmazás szolgáltatás, a virtuális gépeken futó, a szolgáltatás háló és a Cloud Services webalkalmazások szolgáltatója közül választhat."
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure alkalmazás szolgáltatás, virtuális gépeken futó, szolgáltatás háló és Cloud Services összehasonlítása

## <a name="overview"></a>– Áttekintés

Azure host webhelyek több lehetőséget kínál a: [Azure alkalmazás szolgáltatás][], [virtuális gépeken futó][], [Szolgáltatás háló][]és [Cloud Services][]. Ez a cikk segít megérteni a beállításokat, és a webes alkalmazáshoz a megfelelő választás.

Azure alkalmazás szolgáltatása a legtöbb web Apps alkalmazások a legjobb választás. Telepítési és kezelés integrálni a platform, webhelyek kezelésére is méretezhető gyorsan forgalmi nagy terhelés és a beépített betöltése és a forgalom kezelő magas rendelkezésre állásának biztosítása. Ugrás a meglévő webhelyeken Azure alkalmazás szolgáltatás egyszerűen egy [online áttelepítési eszköz](https://www.migratetoazure.net/), Megnyitás-forrás alkalmazás használata a webes alkalmazás gyűjteményből vagy keretek és eszközök lehetőség használatával új webhely létrehozása. A [WebJobs][] funkció megkönnyíti a háttérben futó feladat az alkalmazás szolgáltatás webalkalmazást feldolgozás hozzáadása.

Szolgáltatás háló akkor hasznos, ha Ön új alkalmazás létrehozása, vagy újra írása egy meglévő alkalmazást egy microservice architektúra használatához. Alkalmazások, amely egy megosztott készletben gépek futtatásához elindíthatja a kisvállalati és több száz vagy szükség szerint gépek ezer tömeges méreteket nő. Állapot-nyilvántartó szolgáltatások könnyítse egységes, és biztos, hogy az alkalmazás állapot tárolási és szolgáltatás háló automatikusan kezeli a szolgáltatási szétválasztás méretezés és elérhetőség meg.  Szolgáltatás háló is támogatja a megnyitott webhely felülettel WebAPI .NET (OWIN) és az ASP.NET alapvető.  Alkalmazás szolgáltatás képest, szolgáltatás háló is tartalmaz további beállítási lehetőségekre vagy közvetlen hozzáférés az alapul szolgáló infrastruktúra. Azokat a kiszolgálókat távoli is, vagy állítsa be a kiszolgáló indítási feladatokat. Cloud Services szolgáltatás háló egyszerű használat, és a vezérlő fokú hasonlít, de most már egy régebbi szolgáltatás, és új fejlesztési szolgáltatás háló ajánlott.

Ha egy meglévő alkalmazást, amely jelentős módosításokat futtatását szolgáltatás alkalmazás vagy szolgáltatás háló lenne szükség, a felhőbe áttelepítése egyszerűsítése érdekében sikerült válassza ki a virtuális gépeken futó. Azonban megfelelően konfigurálása biztonságossá tétele és fenntartása VMs szükséges sokkal több időt és informatikai szakértelmét Azure alkalmazás szolgáltatás és szolgáltatás háló képest. Azure virtuális gépeken futó tervezi, győződjön meg arról, figyelembe véve folyamatos karbantartást javítás, frissítése és kezelése a környezettől virtuális szükséges munkamennyiség. Azure virtuális gépeken futó egy infrastruktúra-,-a-szolgáltatás (IaaS), míg alkalmazás szolgáltatás és a szolgáltatás háló Platform-mint-a-szolgáltatás (Paas). 

## <a name="features"></a>Webhelyfunkciók összehasonlítása

Az alábbi táblázat hasonlítja össze az eszköz képességeit alkalmazás szolgáltatás, a Cloud Services, a virtuális gépeken futó vagy a szolgáltatás háló annak érdekében, hogy a legjobb választás. Az egyes beállítások SLA legfrissebb információkért olvassa el a [Azure Service szint rendelkezést](/support/legal/sla/)című témakört.

A szolgáltatás|Alkalmazás Service (a web Apps alkalmazások)|Cloud Services (webes szerepkörök)|Virtuális gépeken futó|Szolgáltatás háló|Jegyzetek
---|---|---|---|---|---
Közelében azonnali telepítés|X|||X|Az alkalmazások és -alkalmazás frissítése bevezetéshez egy felhőalapú szolgáltatásba vagy egy virtuális gép létrehozása néhány percig tart legalább; a web app alkalmazás telepítése másodpercet vesz igénybe.
A nagyobb gépek újratelepítés nélkül szerkezetének kialakítása|X|||X|
Webhely kiszolgálói példány megosztása, tartalom és konfigurálása, ami azt jelenti, hogy nem kell telepítsen újra, és állítsa be újra a méretezni, mint.|X|||X|
Több telepítési környezetekben (a termelési és a fejlesztői)|X|X||X|Háló szolgáltatás lehetővé teszi, hogy az alkalmazás több környezetet vagy telepíteni az alkalmazást egymás mellé különböző verziói.
Az automatikus operációs rendszer frissítése kezelése|X|X|||A szolgáltatás jövőbeli háló engedje fel az automatikus OS frissítések tervezett.
Zökkenőmentes platform áttérést (32 bites és 64 bites között egyszerűen ugorhat)|X|X|||
Mely Számjegy, FTP kód terjesztése|X||X||
Webes üzembe üzembe helyezéséhez kódot.|X||X||Cloud Services támogatja a webes egyes szerepkör-példányok szeretne telepíteni, frissítések telepítése. Azonban nem használható a szerepkör a kezdeti telepítés, és egy frissítési használhatja a Web telepítése esetén minden példányában szerepkörbe külön-külön telepítéshez használni. Több példányával, hogy jogosult legyek az a felhő szolgáltatás SLA gyártási környezetben van szükség.
WebMatrix támogatás|X||X||
Szolgáltatásai, például a szolgáltatás Bus, tárterület, SQL-adatbázis eléréséhez|X|X|X|X|
A Host webhelyen vagy a web services réteg a több szálon-architektúra|X|X|X|X|
A Host középső a több szálon-architektúra|X|X|X|X|Alkalmazás szolgáltatás web Apps alkalmazások egyszerűen üzemeltetheti egy középső REST API-t, és a [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) szolgáltatást üzemeltetheti háttér feladatok feldolgozása. A réteg számára független méretezhetőség eléréséhez dedikált webhelyen WebJobs futtatását is lehetővé teszi. A betekintő [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md) szolgáltatása még több funkció többi szolgáltatások elhelyezésére.
Integrált MySQL-mint-a-szolgáltatás támogatása|X|X|X||Cloud Services integrálhatja a MySQL-mint-a-szolgáltatás keresztül ClearDB meg szeretne rendelni, de nem az Azure-portálon munkafolyamat részeként.
ASP.NET klasszikus ASP, Node.js, PHP, Python támogatja|X|X|X|X|Szolgáltatás háló támogatja az weblapon előtér-kibocsátása [ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md) vagy, terjeszthet alkalmazás (Node.js, Java stb.) bármilyen típusú [végrehajtható Vendég](../service-fabric/service-fabric-deploy-existing-app.md)felhasználóként.
Több példányaiban újratelepítés nélkül méretezése|X|X|X|X|Virtuális gépeken futó méretezheti ki több példányban, de a futtatja rajtuk szolgáltatások kezelése a méretezés-ki kell írni. Konfigurálása kérések irányítja át a gép és, hogy az összes előfordulását karbantartási vagy hardverfrissítés hibák miatt egyidejű újraindul affinitás csoport létrehozása egy terheléselosztó szükséges.
Az SSL támogatása|X|X|X|X|Az alkalmazás szolgáltatás webalkalmazások esetében az Basic és szabványos üzemmódban funkció egyéni tartományneveket az SSL használható. Web Apps alkalmazások az SSL használatával kapcsolatos további tudnivalókért olvassa el a [az Azure webhely SSL-tanúsítvány beállítása](../app-service-web/web-sites-configure-ssl-certificate.md)című témakört.
Visual Studio-integráció|X|X|X|X|
Távoli hibakeresés|X|X|X||
Kód TFS terjesztése|X|X|X|X|
Hálózati elkülönítési [Azure virtuális](/services/virtual-network/) hálózattal|X|X|X|X|Lásd még: [a webhelyek Azure virtuális hálózati integrációval](/blog/2014/09/15/azure-websites-virtual-network-integration/)
[Azure forgalom Manager](/services/traffic-manager/) támogatása|X|X|X|X|
Integrált végpont figyelése|X|X|X||
Kiszolgálók távoli asztali elérése||X|X|X|
Minden olyan egyéni MSI telepítése||X|X|X|Szolgáltatás háló lehetővé teszi, hogy minden olyan végrehajtható fájl [végrehajtható Vendég](../service-fabric/service-fabric-deploy-existing-app.md) felhasználóként tárolni, vagy bármely alkalmazás telepítése a VMs.
Azt jelenti, hogy indítási feladatok definiálása/végrehajtása||X|X|X|
Esemény-nyomkövetés események hallgathatnak||X|X|X|

## <a name="scenarios"></a>Alkalmazási helyzetek és javaslatok

Az alábbiakban néhány gyakori alkalmazás esetek a javaslatok, hogy melyik Azure webes üzemeltetésű lehetőséget lehet, hogy az egyes leginkább megfelelő.

- [Előtér háttér feldolgozás és az adatbázis-fájlok integrálva van eszközein üzleti alkalmazások futtatásához van szükség.](#onprem)
- [Méretezze át a jól vállalati webhelyemre tárolni megbízható módszerre van szükség, és a globális ajánlatok is elérheti.](#corp)
- [Egy IIS6 alkalmazást a Windows Server 2003-e.](#iis6)
- [A kisvállalati verzió tulajdonosa vagyok, és tárolni a webhely-olcsó módszerre van szükség, de a későbbi növekedési szem előtt.](#smallbusiness)
- [Egy webhelyre vagy grafikus designer vagyok, és szeretném azt tervezhet és készíthet webhelyeket az ügyfeleknek.](#designer)
- [A több szálon alkalmazás az weblapon előtér-e vagyok áttelepítése a felhőbe.](#multitier)
- [A Windows magas testre szabott függ, hogy az alkalmazás, vagy Linux környezetben, és újat szeretne helyezze át a felhőben.](#custom)
- [Megnyitás szoftvert használ a saját webhelyen, és szeretném azt az Azure tárolni.](#oss)
- [Van egy kell a vállalati hálózathoz csatlakozik, hogy a vállalati verzió alkalmazás.](#lob)
- [Szeretném tárolni a REST API-t vagy a webszolgáltatás mobiltelefonos ügyfélalkalmazások.](#mobile)


### <a id="onprem"></a>Előtér háttér feldolgozás és az adatbázis-fájlok integrálva van eszközein üzleti alkalmazások futtatásához van szükség.

Azure alkalmazás szolgáltatása összetett üzleti alkalmazások kiváló megoldás. Lehetővé teszi, hogy automatikusan átméretezheti a betöltés kiegyensúlyozott platformon, az Active Directoryval biztosított és a helyszíni erőforrásokhoz alkalmazások fejlesztése. Lehetővé teszi az adott egyszerűen keresztül egy világszínvonalú portál és az API-alkalmazások kezelése, és lehetővé teszi, hogy hogyan felhasználók, akik használják őket az alkalmazás betekintést eszközök bepillantást. A [Webjobs][] szolgáltatás lehetővé teszi a háttérfolyamatok futtatása és a webes réteg hibrid kapcsolódási és VNET szolgáltatások közben részeként feladatok könnyítse való csatlakozáshoz a helyszíni erőforrások. Azure alkalmazás szolgáltatás három 9 SLA web Apps alkalmazások, és lehetővé teszi, hogy:

* Az alkalmazások futtatásához biztos, hogy egy önálló öngyógyító, az automatikus javítási felhő platformon.
* Automatikusan méretezze át a adatközpontokkal globális hálózaton keresztül.
* Biztonsági mentése és visszaállítása vészhelyreállítás.
* Lesz ISO SOC2 és PCI kompatibilis.
* Az Active Directory integrálása

### <a id="corp"></a>Méretezze át a jól vállalati webhelyemre tárolni megbízható módszerre van szükség, és a globális ajánlatok is elérheti.

Azure alkalmazás szolgáltatás a vállalati webhelyek elhelyezésére kiváló megoldás. Ha át kívánja méretezni gyorsan és egyszerűen igényeknek adatközpontokkal a globális hálózaton keresztül web apps lehetővé teszi. Helyi vannak, hibatűrést és intelligens forgalom kezelésére nyújt. Az összes platformon világszínvonalú eszközeit tartalmazza amellyel bepillantást webhely állapotát és a webhely forgalom gyorsan és egyszerűen. Azure alkalmazás szolgáltatás három 9 SLA web Apps alkalmazások, és lehetővé teszi, hogy:

* Önálló öngyógyító, az automatikus javítási felhő platformon biztos, hogy futtassa a-webhelyek.
* Automatikusan méretezze át a adatközpontokkal globális hálózaton keresztül.
* Biztonsági mentése és visszaállítása vészhelyreállítás.
* Adatforgalom integrált eszközökkel és kezelni naplók.
* Lesz ISO SOC2 és PCI kompatibilis.
* Az Active Directory integrálása

### <a id="iis6"></a>Egy IIS6 alkalmazást a Windows Server 2003-e.

Azure alkalmazás szolgáltatás egyszerűen áttelepítése régebbi IIS6 alkalmazások társított infrastruktúra költségek elkerülésére. Microsoft hozott létre, [könnyen használható áttelepítési eszközök és részletes áttelepítési útmutató](https://www.movemetowebsites.net/) , amely lehetővé teszi a kompatibilitás ellenőrzése és kiválogathatja azokat a módosításokat, kell tenni. Integráció a Visual Studióban, TFS és közös CMS eszközök egyszerűen IIS6 alkalmazások közvetlenül a felhőben való telepítéséhez. Miután telepített, az Azur portál robusztus eszközök, amelyek lehetővé teszik, hogy a költségek kezelésére és az értekezlet felfelé igény szükség szerint átméretezheti. Az áttelepítési eszköz van lehetősége:

* Gyorsan és egyszerűen át a régi Windows Server 2003 webalkalmazás a felhőben.
* Csatlakozás a csatolt SQL adatbázis helyszíni hibrid-alkalmazás létrehozása a hagyja.
* Automatikus áthelyezése mellett a régi alkalmazás az SQL-adatbázis.

### <a id="smallbusiness"></a>A kisvállalati verzió tulajdonosa vagyok, és tárolni a webhely-olcsó módszerre van szükség, de a későbbi növekedési szem előtt.

Azure alkalmazás szolgáltatás oka az, ebben az esetben a remek megoldást használatbavételéhez az ingyenes, és további funkciók majd hozzáadása a kellő időben. Azure által megadott tartomány minden egyes ingyenes web App alkalmazásban megtalálható (*your_company*. azurewebsites.net), és a platform integrált telepítési és felügyeleti eszközök, valamint egy alkalmazás gyűjtemény, amelyek megkönnyítik az első lépések. Vannak számos más szolgáltatások és méretezési beállításokat, amelyek lehetővé teszik a is fokozott felhasználói igényű alapkoncepciójára webhelyet. Azure alkalmazás szolgáltatással a következőkre van lehetősége:

- A szabad réteg kezdődik, és majd méretezni, szükség szerint.
- Az alkalmazás gyűjtemény segítségével gyorsan népszerű webalkalmazások, például WordPress beállítása.
- Adja hozzá további Azure szolgáltatások és funkciók az alkalmazás szükség szerint.
- A web app HTTPS biztonságos.

### <a id="designer"></a>Egy webhelyre vagy grafikus tervező vagyok, és szeretném tervezhet és készíthet a webhelyeken a vevők

Webes fejlesztők és -tervezők, az Azure alkalmazás szolgáltatás egyesíti a könnyen keretek és eszközök különféle, telepítési támogatja mely számjegy és FTP és eszközök és szolgáltatások, például a Visual Studio és SQL-adatbázis szoros integrációja kínál. Az alkalmazás szolgáltatás a következőkre van lehetősége:

- Parancssori eszközök segítségével [automatizált feladatok][scripting].
- [.Net]például népszerű nyelvek használata[dotnet], [PHP][], [Node.js][nodejs], és [Python][].
- Kattintson a nagyon magas kapacitás méretezés három különböző méretezési szintek gombra.
- Más Azure szolgáltatások, például [SQL-adatbázis]integrálása[sqldatabase], [Szolgáltatás Bus] [ servicebus] és [tárhely][], vagy az [Azure áruházból]partner ajánlataiban[azurestore], például a MySQL- és MongoDB.
- Integráció az eszközöket, például a Visual Studióban, mely számjegy, WebMatrix, WebDeploy, TFS és FTP.

### <a id="multitier"></a>Vagyok áttelepítése a több szálon alkalmazás az weblapon előtér-az Outlook a felhőbe

Ha futtatja a több szálon alkalmazást, például adatbázis-csatlakozó webkiszolgálóra Azure alkalmazás szolgáltatás beállítás jó, amely felajánlja a szoros integrálása az Azure SQL-adatbázis. És WebJobs funkció kódmentes folyamatok működtetéséhez használható.

Ha több szabályozási lehetőséget biztosít a kiszolgálói környezetben, például az azt jelenti, hogy távoli be a kiszolgáló vagy szükséges kiszolgáló indítási feladatok konfigurálása válassza a szolgáltatás háló egy vagy több a réteg.

Válassza a meghatározási lehetnek virtuális gépeken futó, ha azt szeretné, saját gépi kép használni, vagy futtassa a kiszolgáló szoftver vagy a szolgáltatások, amelyek nem állíthatja be a szolgáltatás háló.

### <a id="custom"></a>A Windows magas testre szabott függ, hogy az alkalmazás, vagy Linux környezetben, és újat szeretne helyezze át a felhőben.

Ha az alkalmazás összetett telepítési vagy a szoftverek és az operációs rendszer konfigurációja, virtuális gépeken futó valószínűleg a legjobb megoldás. A virtuális gépeken futó a következőkre van lehetősége:

- A virtuális gépek galéria használatba az operációs rendszer, például a Windows vagy Linux rendszerhez, és kattintson az alkalmazás igényeinek megfelelően testre szabhatja.
- Hozzon létre, és töltse fel egy egyéni képe egy meglévő helyszíni kiszolgálót az Azure virtuális gépen futtatásához.

### <a id="oss"></a>A saját hely Megnyitás szoftvert használ, és szeretném tárolni, Azure-ban

Ha a Megnyitás keretrendszer támogatott alkalmazás szolgáltatása, a nyelv, és szükség szerint az alkalmazás keretek ki automatikusan megtörténik. Alkalmazás szolgáltatás lehetővé teszi, hogy:

- Számos népszerű megnyitott forrás nyelv használata, például a [.NET][dotnet], [PHP][], [Node.js][nodejs], és [Python][].
- Állítsa be a WordPress, Drupal, Umbraco, DNN és sok más külső webalkalmazások.
- Egy létező alkalmazás áttelepíteni, vagy hozzon létre egy újat az alkalmazás gyűjteményből.

A Megnyitás keretrendszer alkalmazás szolgáltatása nem támogatja, a többi Azure webes beállítások szolgáltatója egyik futtathatja. A virtuális gépeken futó, telepítése, és állítsa be a szoftver a gép képre, amely lehet a Windows vagy Linux-alapú.

### <a id="lob"></a>Van egy kell a vállalati hálózathoz csatlakozik, hogy a vállalati verzió alkalmazás

Ha szeretné a vállalati verzió alkalmazás létrehozása, a webhely előfordulhat közvetlen hozzáférés services vagy az adatokat a vállalati hálózaton. Ez az alkalmazás Service, szolgáltatás háló és virtuális gépeken futó az [Azure virtuális hálózati szolgáltatás](/services/virtual-network/)használatával lehetséges. Alkalmazás szolgáltatása az [VNET integrációs szolgáltatás](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/), amely lehetővé teszi, hogy az Azure alkalmazások futtatásához, mintha a vállalati hálózaton is használhatja.

### <a id="mobile"></a>Szeretném tárolni a REST API-t vagy a webszolgáltatás mobiltelefonos ügyfélalkalmazások

HTTP-alapú webes szolgáltatások lehetővé teszi ügyféleszközre is kiterjedhet, ideértve a mobil ügyfélalkalmazások számos különböző támogatja. ASP.NET webes API például keretek integrálása a Visual Studióban, így azok könnyebben hozhat létre, és a többi szolgáltatásait.  Ezek a szolgáltatások egy webes végpontot, a szerepelnek, így támogatják ezt a helyzetet bármely webes szolgáltatója a Azure technikák használatával lehetséges. Alkalmazás szolgáltatás viszont REST API-khoz elhelyezésére kiváló választás. Az alkalmazás szolgáltatás a következőkre van lehetősége:

- Gyorsan létrehozhat egy [mobilalkalmazás](../app-service-mobile/app-service-mobile-value-prop.md) vagy [API-alkalmazás](../app-service-api/app-service-api-apps-why-best-platform.md) globálisan tárolni a HTTP webszolgáltatás az Azure-féle egyik elosztott adatközpontokkal.
- Meglévő szolgáltatások áttelepíteni, vagy hozzon létre új dokumentumokat.
- SLA elérése az elérhetőség az egy példányát, vagy több kijelölt gépek méretezése.
- Használja a közzétett webhelyet ahhoz, hogy REST API-khoz bármely HTTP-ügyfelek, beleértve a mobil ügyfélalkalmazások.

> [AZURE.NOTE]
> Ha azt szeretné, mielőtt feliratkozna az ügyfélhez használatbavételéhez Azure alkalmazás szolgáltatás, nyissa meg <a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>, ahol azonnal létrehozhat egy rövid életű starter alkalmazásban az Azure alkalmazás szolgáltatás ingyenesen. Nem kötelező hitelkártya, nincs nyilatkozatát.

## <a id="nextsteps"></a>Következő lépések

További információt a három webes üzemeltetési beállításai című témakör [Azure bemutatása](../fundamentals-introduction-to-azure.md).

Az első lépések a kapcsolók úgy dönt, hogy az alkalmazás, a következő forrásokban talál:

* [Azure alkalmazás szolgáltatás](/documentation/services/app-service/)
* [Azure Cloud Services](/documentation/services/cloud-services/)
* [Azure virtuális gépeken futó](/documentation/services/virtual-machines/)
* [Szolgáltatás háló](/documentation/services/service-fabric)

<!-- URL List -->

[Azure alkalmazás szolgáltatás]: /services/app-service/
[Cloud Services]: http://go.microsoft.com/fwlink/?LinkId=306052
[Virtuális gépeken futó]: http://go.microsoft.com/fwlink/?LinkID=306053
[Szolgáltatás háló]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[Tárhely]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
