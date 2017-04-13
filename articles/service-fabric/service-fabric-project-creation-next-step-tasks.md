<properties
   pageTitle="További lépések a szolgáltatás háló projekt létrehozása |} Microsoft Azure"
   description="Ez a cikk core fejlesztési feladatait hivatkozásokat tartalmaz az szolgáltatás háló"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>A szolgáltatás háló alkalmazás és a következő lépések
Az Azure Service háló alkalmazás létrehozva. Ez a cikk ismerteti a makeup a project és a néhány lehetséges következő lépések.

## <a name="your-application"></a>Az alkalmazás
Minden új alkalmazás egy alkalmazás projektet tartalmaz. Előfordulhat, hogy egy vagy két további projektek választott szolgáltatás típusától függően.

### <a name="the-application-project"></a>Az alkalmazás-projekt
Az alkalmazás projektet tartalmaz:

- Hivatkozás a szolgáltatásokhoz, az alkalmazás alkotó csoportja.

- Két tegye közzé a profilok (helyi és Felhőbeli), a különböző környezetekben – például alapértelmezés szerint egy fürt végpontot, illetve e frissítési telepítések végrehajtása kapcsolatos beállítások beállítások megőrzéséhez használható.

- Két alkalmazás paraméter fájlok (helyi és Felhőbeli) környezetfüggő alkalmazás beállításokat, például a partíciók szolgáltatás hozzon létre a megőrzéséhez használható.

- Olyan telepítési parancsfájlt, amely a parancssorból vagy automatizált folyamatos integráció és üzembe folyamat részeként a alkalmazás telepítéséhez is használhatja.

- Az alkalmazás található, amelyben ismerteti az alkalmazás. A jegyzék ApplicationPackageRoot mappájában található.

### <a name="stateless-service"></a>Állapot nélküli szolgáltatás
Ha új állapot nélküli szolgáltatás hozzáadása, a Visual Studio hozzáadja a szolgáltatási projekt a megoldás, amely tartalmazza a felvett típus `StatelessService`. A szolgáltatás egy helyi változót egy számláló megnöveli.

### <a name="stateful-service"></a>Állapot-nyilvántartó szolgáltatás
Ha új állapot-nyilvántartó szolgáltatás hozzáadása, a Visual Studio hozzáadja a szolgáltatási projekt a megoldás, amely tartalmazza a felvett típus `StatefulService`. A szolgáltatás a számláló megnöveli a `RunAsync` módszer, és tárolja az eredményt, a `ReliableDictionary`.

### <a name="actor-service"></a>Szereplő szolgáltatás
Amikor felvesz egy új megbízható szereplő, a Visual Studio két projektet hozzáadja a megoldás: projektben szereplő és felület projektben.

A szereplő projekt bemutat beállítás, és az első egy számláló értékét, hogy megbízható állandó belül a szereplő állam. A felület projekt más szolgáltatások segítségével a szereplő meghívása felületet nyújt.

### <a name="stateless-web-api"></a>Állapot nélküli webes API
Az állapot nélküli webes API a projectben egy egyszerű webszolgáltatás, nyissa meg az alkalmazás külső ügyfeleket is használhatja. További információt a projekt strukturált, [szolgáltatás háló webes API-szolgáltatások OWIN önálló szolgáltatója](service-fabric-reliable-services-communication-webapi.md)témakörben talál.

### <a name="aspnet-core"></a>ASP.NET core

A szolgáltatás háló SDK biztosít ugyanazok a ASP.NET Core ASP.NET Core projektek önálló rendelkezésre álló sablonok: üres, a [Webes API][aspnet-webapi], és a [Webes alkalmazás][aspnet-webapp].

## <a name="next-steps"></a>Következő lépések
### <a name="create-an-azure-cluster"></a>Hozzon létre egy Azure fürthöz
A szolgáltatás háló SDK fejlesztés és tesztelése a helyi fürtre biztosít. Fürt létrehozása az Azure-ban, látni [az Azure portálról szolgáltatás háló fürt beállításának][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Próbálja ki az Azure ingyenesen bevezetéshez fél fürt a

Ha azt szeretné, próbálja ki az Azure-ban a központi telepítés és kezelése alkalmazások saját fürt beállítása nélkül, az ingyenes [fél fürt szolgáltatás](http://aka.ms/tryservicefabric)is használhatja.

### <a name="publish-your-application-to-azure"></a>Az alkalmazás Azure közzététele
Az alkalmazás közvetlenül a Visual Studio Azure fürthöz közzéteheti. Megtudhatja, hogy című témakörben olvashat bővebben [az Azure alkalmazás közzétételi][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Szolgáltatás háló Intézővel a fürt ábrázolásához
Szolgáltatás háló Explorer számos a legegyszerűbben a fürt, beleértve a telepített alkalmazások és a tényleges elrendezés ábrázolásához. További tudnivalókért lásd: [a szolgáltatás háló Explorerrel fürt megjelenítése][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Verzió és a szolgáltatások frissítése
Háló szolgáltatás lehetővé teszi, hogy független verziószámozás és frissítése a szolgáltatások független az alkalmazásokban. További tudnivalókért lásd: [verziószámozás és a szolgáltatások frissítése][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Folytonos integráció a Visual Studio Team Services szolgáltatással
Hogyan állíthat be egy folyamatos integrációs folyamat a szolgáltatás háló alkalmazáshoz című témakörben talál [a Visual Studio Team Services konfigurálása folyamatos integrációja][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
