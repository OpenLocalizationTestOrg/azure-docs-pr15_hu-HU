<properties 
    pageTitle="Ha át kívánja méretezni, az alkalmazás-alkalmazás szolgáltatási környezetben hogyan" 
    description="Az alkalmazás méretezés-alkalmazás szolgáltatási környezetben" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben méretezési alkalmazások #

Az Azure-App szolgáltatásban dolgot általában három méretezheti:

- árak terv
- dolgozó mérete 
- példányainak száma.

Egy ASE nem megadása vagy módosítása a árak terv nincs szükség.  Értelmez funkciók már képesség szint árak támogatás elemre.  

Részletez dolgozó méretét a ASE felügyeleti rendelhet a számítási erőforrás minden dolgozó készlet használandó méretét.  Ez azt jelenti futtatható dolgozó készlet 1 P4 számítási erőforrásokkal és P1 készlet 2 dolgozó erőforrások számítja ki, ha szükségesnek látja.  Nem rendelkeznek a méret sorrendben kell.  A részletek a méret és a árak lásd: a dokumentum itt körül [Azure alkalmazás szolgáltatás árak][AppServicePricing].  Az alkalmazás-szolgáltatási környezetben kell ez elhagyja a web Apps alkalmazások és az alkalmazás szolgáltatás tervek méretezési beállításokat:

- dolgozó készlet kiválasztása
- példányainak száma

A megfelelő felhasználói felületének látható esetében a ASE üzemeltetett App szolgáltatás csomagok között vagy az elem módosításának történik.  

![][1]

Nem lehet méretezni, felfelé a ASP túl a rendelkezésre álló számítási erőforrások szerepel-e a ASP dolgozó készletben számát.  Ha a kell kiszámítania a készlethez tartozó dolgozó erőforrások meg kell szereznie a ASE-rendszergazdától, adja hozzá őket.  Az információk újra konfigurálása a ASE körül, olvassa el az alábbi információkat: [az alkalmazás-szolgáltatási környezetben konfigurálása][HowtoConfigureASE].  Érdemes azt is, kihasználhatja a ASE Automatikus méretezéssel szolgáltatások hozzáadása a kapacitás ütemezése és mértékek alapján.  További részleteket a Automatikus méretezéssel beállítása a ASE magát környezet megtudhatja, [hogy miként állítsa be az alkalmazás-szolgáltatási környezetben Automatikus méretezéssel][ASEAutoscale].

Létrehozhat több app milyen szolgáltatáscsomagok számítási erőforrások használata a különböző dolgozó készletek, vagy azonos dolgozó készletben is használhatja.  Ha például dolgozó készlet 1 (10) elérhető számítási erőforrások van választva alkalmazás szolgáltatás csomagot használ (6) számítási erőforrások létrehozása és egy második alkalmazás szolgáltatás megtervezése (4) használó erőforrások kiszámítására.

### <a name="scaling-the-number-of-instances"></a>Méretezés példányainak száma ###

Első létrehozásakor a web app alkalmazás szolgáltatási környezetben 1 példány kezdődik.  Ezután méretezheti ki további számítási erőforrások az alkalmazás számára a további előfordulásait.   

Ha a ASE elegendő kapacitása van, akkor ez a meglehetősen egyszerű.  Megnyitja az alkalmazás szolgáltatás tervet, amely a webhelyek szeretné méretezni, és válassza ki a méretezés.  Ekkor megnyílik a felhasználói felület, ahol manuálisan a skála megadása a ASP és állítsa be az Automatikus méretezéssel szabályokat a ASP.  Manuális méretarányra az alkalmazás egyszerűen ***méretarányainak*** beállítása szerint ***-példányok száma, amely a kézi megadása***.  További lehetőségek húzza a csúszkát a kívánt mennyiség, vagy írja be a csúszkák melletti mezőkben.  

![][2] 

Egy-egy ASE a ASP Automatikus méretezéssel szabályairól ugyanúgy működik, mint a szokásos módon is.  ***Processzor százalék*** ***szerint a méretarány*** csoportban jelölje be, és a Processzor százalékos alapján ASP Automatikus méretezéssel a szabályok létrehozása vagy összetettebb szabályokat ***ütemterv és a teljesítmény szabályok***használatával hozhat létre.  További részletes adatainak megjelenítéséhez beállításával Automatikus méretezéssel használja az útmutató [Azure App szolgáltatásban alkalmazás méretezheti]Itt[AppScale]. 


### <a name="worker-pool-selection"></a>Dolgozó készlet kiválasztása ###

Amint korábbi verziójában, akkor a dolgozó készlet kijelölés a felhasználói felület ASP érhető el.  Nyissa meg a használni kívánt méretezni, és válassza a dolgozó készlet ASP a lap.  Megjelenik a dolgozó készletek amely beállította az alkalmazás-szolgáltatási környezetben.  Ha csak egy dolgozó készlet majd csak akkor láthatja az egyik készletben szerepel a listában.  Milyen dolgozó készlet szerepel-e a ASP módosításához, egyszerűen jelölje ki a szeretné, hogy az alkalmazás szolgáltatás megtervezése áthelyezése dolgozó készletre.  

![][3]

Mielőtt a ASP egy dolgozói készletből között fontos, hogy a ASP elegendő kapacitása lesz.  Dolgozó készletek a listában nem csak a dolgozó készlet neve szerepel, de azt is megtekintheti, hogy hány dolgozók érhetők el az adott dolgozó készletre.  Győződjön meg arról, hogy nincsenek-e elegendő példányok érhető el, amely tartalmazhat, az alkalmazás szolgáltatás megtervezése.  Szükséges további számítja ki az áthelyezni kívánt dolgozó készletben erőforrásokat, ha akkor vegye fel az illetőt a ASE rendszergazda vissza.  

> [AZURE.NOTE] Az adott ASP alkalmazások áthelyezése egy-egy dolgozói készletből ASP okoz hideg kezdődik.  Valaki lassan, az alkalmazás hideg lépések az új számítási erőforrások léphetnek fel.  Hideg indítása elkerülhetők legyenek az [alkalmazás meleg lehetőség be] használatával[ AppWarmup] Azure App szolgáltatásban.  Az alkalmazás inicializálni modulban, a cikkben ismertetett hideg indított is használható, mert a inicializálni folyamat is hív meg alkalmazások hideg új számítási erőforrások lépések. 

## <a name="getting-started"></a>Első lépések

Első lépések az alkalmazás-szolgáltatási környezetben, lásd: [Hogyan kattintva hozzon létre egy alkalmazás szolgáltatási környezetben][HowtoCreateASE]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
