<properties
   pageTitle="Szolgáltatás háló áttekintése |} Microsoft Azure"
   description="Szolgáltatás háló, ahol alkalmazások számos microservices méretezése és címtárfrissítések megadására tevődnek áttekintése. Szolgáltatás háló elosztott rendszerek platformot méretezhető, megbízható létrehozásához használt, de egyszerűen kezelt az alkalmazások számára a felhőbe"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="mfussell"/>

# <a name="overview-of-service-fabric"></a>Szolgáltatás háló áttekintése
Szolgáltatás háló, amely megkönnyíti az csomag, üzembe helyezéséhez és kezeléséhez méretezhető és megbízható microservices elosztott rendszerek platformot. Szolgáltatás háló is oldja meg a jelentős kihívásokkal fejlesztése, felhőalapú alkalmazások kezelése. A fejlesztők és a rendszergazdák elkerülhetők a megoldási összetett infrastruktúra problémák és a fókusz inkább arra, hogy azok méretezhető, megbízható és kezelhető kulcsfontosságú, nagy terhelés megvalósításáról. Szolgáltatás háló létrehozásához és a vállalati szintű, a réteg-1 felhő nagyméretű alkalmazások kezelése a következő generációs köztes platform képviseli.

Ebből a [rövid videó](https://aka.ms/servicefabricvideo) szolgáltatás háló és a microservices biztosít.


## <a name="applications-composed-of-microservices"></a>Alkalmazások tevődik össze microservices
Háló szolgáltatás lehetővé teszi, hogy a készítése és tevődik össze (néven a fürtre) gépek megosztott erőforráskészlethez tartozik a nagyon magas sűrűség futó microservices méretezhető és megbízható alkalmazások kezeléséhez. Az elosztott, méretezhető állapot nélküli és állapot-nyilvántartó microservices készítéséhez egy összetett futtatókörnyezet biztosít. Teljes alkalmazás szolgáltatásairól kiépítési, telepítése, figyelése, frissítése és javítása is tartalmaz, és törlése a telepített alkalmazások.

Miért fontos a microservices megközelítés? Két fő oka a következők:

1. Lehetővé teszik az alkalmazásokat, attól függően, hogy a szükségleteinek különböző részeire méretezni.

2. A fejlesztői csoport képesek további Agilis a módosítások helyezése, és így adja meg szolgáltatások ügyfelei gyorsabban és gyakrabban.

Szolgáltatás háló hatásköröket sok Microsoft szolgáltatást ma, többek között a Azure SQL-adatbázissal, Azure DocumentDB, Cortana, a Power BI, Microsoft Intune, Azure esemény hubok, Azure IoT, a Skype vállalati verzió és számos alapvető Azure szolgáltatások.

Szolgáltatás háló hozhat létre a felhőben elindítható kicsi, szükség szerint, és a száz vagy gépek ezer tömeges méreteket nő natív szolgáltatások üzleti intelligenciára szabott.

Az aktuális nagyméretű internetes szolgáltatások, microservices készültek. Microservices többek között protokoll átjárók, a felhasználói profilok vásárlásra kocsik, készlet feldolgozása, sorok, és a gyorsítótárban. Szolgáltatás háló, amellyel minden microservice egy egyedi nevet, amely lehet állapot nélküli vagy állapot-nyilvántartó microservices platformot.

Szolgáltatás háló átfogó futtatókörnyezet és életciklus management szolgáltatásokat nyújt alkalmazások ezeket microservices tevődik össze. Tárolók rendszerbe és végig a szolgáltatás háló fürthöz aktiválása belül microservices tárolja azt. Áttérés a VMs tárolók tesz lehetséges növekedését sorrendben, a bal sűrűségfüggvény eredménye az. Hasonlóképpen egy másik sorrend az nagyságát sűrűségfüggvény eredménye az válik lehetséges helyez át a tárolók microservices. Egy egyetlen Azure SQL-adatbázis fürt például adatbázisok ezer több száz összesen szolgáltatója tárolók ezer tízesre futtató számítógépeken több száz magába foglalja. Adatbázisonként a szolgáltatás háló állapot-nyilvántartó microservice. Ugyanez igaz a más szolgáltatások már említettük, vagyis az oka annak, hogy a kifejezéskészlet "hyperscale" szolgáltatás háló funkciók leírására szolgál. Ha a tárolók magas sűrűség adni, majd microservices ad hyperscale.

További tudnivalók a microservices megközelítés, olvassa el a [oka annak, hogy a microservices megközelítés alkalmazások létrehozásába?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>A tároló telepítési és üzletifolyamat-tervező
Szolgáltatás háló egy [orchestrator](service-fabric-cluster-resource-manager-introduction.md) microservices az, a fürtre gépek között. Microservices használja a [szolgáltatás háló programozási modellek](service-fabric-choose-framework.md) [Vendég végrehajtható](service-fabric-deploy-existing-app.md)üzembe többféle módon is készüljön. Szolgáltatás háló telepítheti a szolgáltatások tároló képeket, és a fontosabb akkor is keverjen ki a folyamatok services és a tárolók közös ugyanabban az alkalmazásban a services. Csak az [üzembe helyezéséhez és kezeléséhez tároló képek](service-fabric-containers-overview.md) fürt gépek között, szolgáltatás háló szükség a tökéletes választási lehetőséget.


## <a name="create-service-fabric-clusters-anywhere"></a>Tetszőleges szolgáltatás háló fürt létrehozása
Szolgáltatás háló fürt hozhat létre, sok környezetben, beleértve az Azure vagy a helyszíni, Windows Server vagy Linux rendszerhez. A fejlesztői környezet a SDK ezenkívül megegyezik az érintett nincs emulátor az éles üzemi környezetben. Más szóval ha a helyi fejlesztési fürt futása azt üzembe helyezése a azonos fürthöz más környezetben.

Fürt helyszíni, olvassa el a [Windows Server vagy Linux fürt létrehozása](service-fabric-deploy-anywhere.md) létrehozásával kapcsolatban további információt, vagy az Azure létrehozása egy fürt [az Azure portálon keresztül](service-fabric-cluster-creation-via-portal.md).

![Szolgáltatás háló platform][Image1]

## <a name="stateless-and-stateful-service-fabric-microservices"></a>Állapot nélküli és állapot-nyilvántartó szolgáltatás háló microservices

Szolgáltatás háló a microservices álló alkalmazások létrehozását teszi lehetővé. Állapot nélküli microservices (protokoll átjárók, webes proxyk stb.) nem őrzik meg egy adott kérésének és a válasz, a szolgáltatásból kívüli változtatható állam. Azure Cloud Services dolgozó szerepkörök szolgáltatás állapot nélküli példa. Állapot-nyilvántartó microservices (felhasználói fiókokat, adatbázisok eszközök, vásárlás kártyák, sorok, stb.) állapotban egy változtatható, mérvadó túl a kérést, és a választ. Az aktuális Internet-skála alkalmazások állnia állapot nélküli és állapot-nyilvántartó microservices kombinációi.

Miért van az állapot-nyilvántartó microservices állapot nélküli melyeket együtt? Két fő oka a következők:

1. Az azt jelenti, hogy összeállítása nagy átviteli, alacsony-késés, hiba tervezett online tranzakció (OLTP) szolgáltatások feldolgozása, hogy tartja a kódot, és zárja be az adatok ugyanazon a gépen. Néhány példa, hogy interaktív kirakatokkal, keresés, dolog Internet (IoT) rendszerek, kereskedési rendszerek, hitelkártya feldolgozás és csalás rendszerek, és személyes record management.

2. Alkalmazás tervezés egyszerűsítése. Állapot-nyilvántartó microservices meg további sorok eltávolítása és gyorsítótárát, a hagyományos szükséges cím alkalmazást tisztán állapot nélküli elérhetőség és a késés követelményeknek. Állapot-nyilvántartó azok a szolgáltatások természetesen magas-elérhetőségéről és az alacsony késés, az alkalmazás egészének kezelése mozgó részek számának csökkentése.

További információt a szolgáltatás háló, olvassa el az [alkalmazás forgatókönyvek](service-fabric-application-scenarios.md) , majd a [modell programozási keretet](service-fabric-choose-framework.md) a szolgáltatásbeli alkalmazás mintázatok

## <a name="application-lifecycle-management"></a>Alkalmazás életciklus-kezelése
Szolgáltatás háló osztályú támogatást nyújt a teljes alkalmazás életciklus-kezelése (ALM) a felhőben alkalmazások – telepítés, napi kezelési és karbantartás fejlesztését esetleges leszerelje.

A szolgáltatás háló ALM funkciók engedélyezése rendszergazdái informatikai operátorokat használhatja a simple, Min érintéssel munkafolyamatokat kiépítése, üzembe helyezéséhez javítás, és használt alkalmazások figyelése. Ezek beépített munkafolyamatok nagyban csökkentheti az informatikai operátorok folyamatosan elérhető alkalmazások megtartása terhet.

A legtöbb alkalmazások állapot nélküli és állapot-nyilvántartó microservices és más végrehajtható/szükséges közös rendszerbe állnak. Erős típusai csomagolt microservices és alkalmazások, az alábbiakat háló szolgáltatás lehetővé teszi, hogy több szolgáltatásalkalmazás-példányok telepítését. Egyes példányok felügyelt, és önállóan frissíteni. Fontosabb szolgáltatás háló alkalmas *bármilyen* végrehajtható vagy futtatókörnyezet telepítése, és végezze el őket a megbízható. Például háló szolgáltatás üzembe helyezése ASP.NET Core 1, Node.js, Java VMs, parancsfájlok vagy bármely más jelenjenek meg az alkalmazást.

Alkalmazás életciklus-kezelése kapcsolatos további tudnivalókért olvassa el az [alkalmazás életciklusa](service-fabric-application-lifecycle.md) és minden olyan programkódot telepít nézze meg [a Deploy végrehajtható vendégként](service-fabric-deploy-existing-app.md)

## <a name="key-capabilities"></a>Főbb funkciók
Szolgáltatás háló használatával a következőkre van lehetősége:

- Önálló öngyógyító nagymértékben méretezhető alkalmazások fejlesztése.

- Használja a szolgáltatás háló programozási modell microservices álló alkalmazások fejlesztéséhez. Vagy egyszerűen host Vendég végrehajtható és más alkalmazások keretek lehetőség, például ASP.NET Core 1 vagy Node.js.

- Fejleszthet olyan nagymértékben megbízható állapot nélküli és állapot-nyilvántartó microservices.

- Üzembe helyezéséhez és téve közé tartozik a Windows tárolók és Docker tárolók tárolók fürt keresztül. Ezek a tárolók tároló Vendég végrehajtható vagy megbízható állapot nélküli és állapot-nyilvántartó microservices is.  Bármelyik lehetőséget választja, akkor tároló port host port hozzárendelése, tároló feltárást és automatizált feladatátvevő.

- A tervezés, az alkalmazás egyszerűbbé és a sorok helyett állapot-nyilvántartó microservices használatával.

- Telepítse az Azure vagy a helyszíni felhőket fut a Windows Server vagy Linux nulla kód módosításokkal. Egyszer írható és bármely szolgáltatás háló fürt tetszőleges telepítését.

- Fejleszthet olyan, a "adatközponthoz a számítógépen" megközelítés. A helyi fejlesztői környezet az Azure adatközpont esetén futó ugyanazt a kódot.

- Alkalmazások telepítése a másodpercben megadott.

- A virtuális gépeken futó üzembe helyezése a száz vagy gépi / alkalmazások ezer-nál nagyobb sűrűség alkalmazások terjesztése.

- Különböző verzióit egymás mellett, egymástól függetlenül minden bővíthető az azonos alkalmazás telepítéséhez.

- Kezelése az életciklus az állapot-nyilvántartó alkalmazás bármely legrövidebb leállás, többek között a megszakítása és a nem törhető frissítések nélkül.

- Alkalmazások használata a .NET API-k kezelése, Java (Linux), a PowerShell, az Azure CLI (Linux) vagy a többi felületek.

- Frissítése, és önállóan javítás microservices-alkalmazásokból.

- Figyelheti és állapota az alkalmazások diagnosztizálása és automatikus javítás elvégzésére vonatkozó szabályok beállítása.

- Azt jelenti, hogy a méretezési vagy skála egy fürt, valamint a méretezés-felfelé csomópontok számának vagy skála lefelé minden csomópont arra, hogy az alkalmazások automatikus méretezése és a rendelkezésre álló erőforrásokat megfelelően kerülnek terjesztésre méretét.

- Megtekintés az önálló öngyógyító erőforrás terheléselosztó a terjesztés céljából, az alkalmazások téve a végig a fürt. Szolgáltatás helyreállítása a hibák és az eloszlás alapján a rendelkezésre álló erőforrásokat terhelés optimalizálja.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

* További információ:
    * [Miért egy microservices alkalmazások létrehozásába megközelíthető?](service-fabric-overview-microservices.md)
    * [Kifejezések áttekintése](service-fabric-technical-overview.md)
* A szolgáltatás háló [fejlesztői környezet](service-fabric-get-started.md) beállítása  
* A szolgáltatás [kiválasztása egy programozási modell keretrendszer](service-fabric-choose-framework.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
