<properties 
    pageTitle="Alkalmazás szolgáltatás Linux – bevezetés |} Microsoft Azure" 
    description="Tudjon meg többet az alkalmazás szolgáltatás Linux." 
    keywords="Azure alkalmazás szolgáltatás, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Alkalmazás szolgáltatás Linux – bevezetés
Alkalmazás szolgáltatás Linux jelenleg nyilvános előzetes verzióban, és a futó web Apps alkalmazások támogatja natív módon Linux. 

## <a name="overview"></a>– Áttekintés ##
Ügyfelek szolgáltatással alkalmazás Linux host web Apps alkalmazások Linux a natív módon az alkalmazás által támogatott készleteket. A következő funkciók szakasz megjeleníti a jelenleg támogatott alkalmazásban készleteket.

## <a name="features"></a>Szolgáltatások ##
Alkalmazás szolgáltatás Linux jelenleg támogatja a következő alkalmazásban készleteket

- NODE.js
- PHP

Ügyfelek telepítheti a saját alkalmazások használata

- AZ FTP.
- Helyi mely számjegy.
- GitHub vagy BitBucket.

Az alkalmazás méretezése


- Ügyfelek felfelé és lefelé méretezheti a web App alkalmazásban a réteg az alkalmazás szolgáltatás tervben módosításával. 
- Ügyfelek méretezése ki az alkalmazások és az alkalmazás különböző több példányok futtatása belül az illető Termékváltozat.

A Kudu néhány alapvető funkcióit működnek

- Környezet.
- Telepítések.
- Egyszerű konzol.

## <a name="limitations"></a>Korlátozások ##

Az Azure adatkezelési portál csak az alkalmazás szolgáltatás Linux jelenleg támogatott szolgáltatások megjelenítése és elrejtése a többi. Engedélyezéséről további funkcióinak csoportunk, azt fog tartani tükröző Ez az adatkezelési portálon. Bizonyos szolgáltatások, mint a VNET integráció és AAD / külső hitelesítési vagy Kudu webhely bővítmények jelenleg nem működnek. De ezek a műveletek azt első azt frissül a dokumentáció és blog a változásokról.

A nyilvános előzetes jelenleg csak érhető el az alábbi régiókban

-   Nyugati USD.
-   Nyugati Europe.
-   Délkelet-ázsiai.

Webalkalmazás Linux csak támogatott dedikált alkalmazás szolgáltatás tervek, és nem rendelkezik egy ingyenes vagy megosztott réteg. App milyen szolgáltatáscsomagok rendszeres és Linux web Apps alkalmazások is egymást kölcsönösen kizáró, így egy alkalmazás nem Linux szolgáltatáscsomagja nem hozható létre Linux webalkalmazást.

Webalkalmazás Linux, amely nem tartalmaz nem Linux web Apps alkalmazások ugyanabban a régióban erőforráscsoport kell létrehozni.

Átfedő újrafelhasználás webalkalmazások hiánya miatt ügyfelek várt kell egy kis legrövidebb leállás megelőzve webalkalmazást újraindítása használ. 

## <a name="next-steps"></a>Következő lépések ##

Kövesse az alábbi hivatkozásokra kattintva Linux alkalmazás szolgáltatás – első lépések. Kérjük, tegye fel kérdéseket és problémáit, [a fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Alkalmazás szolgáltatás Linux Web Apps alkalmazások létrehozása](./app-service-linux-how-to-create-a-web-app.md)
* [A Web Apps alkalmazások Linux Node.js PM2 konfiguráció használata](./app-service-linux-using-nodejs-pm2.md)

