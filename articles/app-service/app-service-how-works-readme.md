<properties 
    pageTitle="Hogyan Azure alkalmazás szolgáltatás működése" 
    description="Ismerje meg az App szolgáltatás működése" 
    keywords="alkalmazás, azure alkalmazás szolgáltatás, a méretarány méretezhető, alkalmazás szolgáltatáscsomagja, alkalmazás szolgáltatás költség"
    services="app-service" 
    documentationCenter="" 
    authors="yochay" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="02/10/2016" 
    ms.author="yochay"/>

# <a name="how-app-service-works"></a>Alkalmazás-szolgáltatás működése

Azure alkalmazás szolgáltatása egy felhőalapú szolgáltatásba, szemben lévő ma mérnökök gyakorlati problémamegoldó verziójához. Alkalmazás szolgáltatás összpontosít kiváló Fejlesztőeszközök productivity kezeléséről alkalmazások előadása a felhőben skála szükséges a kompromisszumok nélkül. Alkalmazás szolgáltatást is tartalmaz, a szolgáltatások és a szükséges vállalati a vállalati verziós alkalmazások írása közben fejlesztők számára (.NET, Java, PHP, Node.JS és Python) legnépszerűbb fejlesztési nyelvek támogatására keretek.
Az alkalmazás szolgáltatás fejlesztők lehetőségei vannak:

* Nagyon méretezhető Web Apps alkalmazások készítése
* Gyors létrehozása mobilalkalmazás vissza részek könnyen használható mobil lehetőségek, például adatok vissza részek tartalmazó, a felhasználói hitelesítés és a leküldéses értesítések a Mobile-alkalmazások használatát. 
* Megvalósítása telepítése és az API-alkalmazások API-khoz Közzététel gombra.
* Üzleti alkalmazások közös kötik munkafolyamatok az adatok és átalakítása logika alkalmazással.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

Diagramtípusokat alkalmazást használja arra, amely lehetővé teszi, hogy a fejlesztők egy optimalizált teljes életciklus élmény alkalmazást tervező alkalmazás karbantartási méretezhető és rugalmas Web App platformon. Az életciklus-funkciók engedélyezése:

* Rövid alkalmazás létrehozása - indulás az alapoktól, vagy válasszon egy OSS csomagot a Microsoft Azure piactéren lévő. 
* Folytonos telepítés – automatikusan üzembe a Népszerű elemek source vezérlő megoldások, például az online tárhelyek szolgáltatásokból, például a OneDrive és a DropBox TFS, GitHub és BitBucket és szinkronizálási tartalom új kódot.
* Tesztelje a gyártási - zökkenőmentesen létrehozása előtti gyártási környezetekben, és kezelheti a forgalmat, látogasson el a őket részét. Hibakeresési a felhőben, ha szükséges, és állítsa vissza, ha problémák találhatók.
* Futó aszinkron tevékenységek és a köteg feladatok - kód végrehajtása a háttérben folyamat, vagy aktiválni a kód alapján események (például az üzenetek jelenjenek meg az Azure tároló várólista) és a munkaidőt (CRON).
* A méretezés az alkalmazás - közül számos beállítást, ha át kívánja méretezni a vízszintesen és függőlegesen automatikusan alapján forgalmat és erőforrás-kihasználtság szolgáltatás használatát. Állítsa be az alkalmazásait dedikált saját környezetben   
* Az alkalmazás - emelés fenntartása számos hibakeresési és diagnosztika szolgáltatásait, akik előre problémák kapcsolatának és hatékonyan Megoldásukhoz vagy valós idejű (például az automatikus javítás és élő hibakeresési szolgáltatásokkal) vagy naplókról és a memóriában elemzésével utólag listázása
 
Összeállított, az alkalmazás szolgáltatás funkciók engedélyezése fejlesztők kódjukat kiemelése és a stabil, nagyon méretezhető gyártási állapot érni. API-val és a logika alkalmazás funkcióival fejlesztők hidat korlátok üzleti megoldások, valamint a helyszíni környezetbe való integráció a felhőben való életben vállalati alkalmazások hozhat létre.  

[AZURE.INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]
