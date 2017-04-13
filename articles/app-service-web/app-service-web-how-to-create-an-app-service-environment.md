<properties 
    pageTitle="Hogyan hozhat létre az alkalmazás-szolgáltatási környezetben" 
    description="Létrehozási folyamat leírás alkalmazás szolgáltatási környezetben" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Hogyan hozhat létre az alkalmazás-szolgáltatási környezetben #

### <a name="overview"></a>– Áttekintés ###

Alkalmazás szolgáltatási környezetben (SKB) a támogatási szolgáltatás lehetőséget Azure alkalmazás szolgáltatás olyan bővített konfigurációs lehetőséggel, amely nem érhető el a több elem bérlői bélyegzőket biztosítja.  A ASE funkció ügyfél virtuális hálózatba lényegében az Azure alkalmazás szolgáltatás üzembe helyezése.  Az alkalmazás szolgáltatás környezetekben, olvassa el a [Mi az, hogy az alkalmazás-szolgáltatási környezetben] által kínált lehetőségek megértése eléréséhez[ WhatisASE] dokumentációt.

### <a name="before-you-create-your-ase"></a>A ASE létrehozása előtt ###

Fontos tudnia, hogy a dolog, amit nem módosítható.  Azon szempontjait nem módosítható a ASE a létrehozása után a következők:

- Hely
- Előfizetés
- Erőforráscsoport
- Használt VNet
- Használt alhálózat 
- Alhálózat mérete

Ha egy VNet fejléccellát, és adja meg a alhálózat, győződjön meg arról, akkor elég nagy minden jövőbeni növekedés.  

### <a name="creating-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben létrehozása ###

Kétféleképpen ASE létrehozása felhasználói felület eléréséhez.  Ez lehet folytatott az Azure piactéren elérhető ***App szolgáltatási*** környezetben található vagy keresztül új webhely -> + Mobile-alkalmazás szolgáltatási környezetben >.  Egy ASE létrehozása:

1. Adja meg a ASE nevét.  A nevet, amelyet a ASE meg van adva a ASE készült alkalmazások használandó.  Ha a ASE neve appsvcenvdemo altartomány neve lenne. *appsvcenvdemo.p.azurewebsites.net*.  Ha az így létrehozott nevű *mytestapp* , akkor a *mytestapp.appsvcenvdemo.p.azurewebsites.net*címmel rendelkező alkalmazás.  Szóköz nem használható a ASE neve.  Nagybetűs karaktereket használ a nevét, ha a tartománynév lesz a név kisbetűs teljes verzióját.  Egy ILB használatakor majd a ÁZIS neve nem szerepel a altartomány, de inkább kifejezetten adni ASE létrehozása közben

    ![][1]

2. Jelölje ki azt az előfizetést.  A ASE használt is, hogy a minden alkalmazás az adott ASE jön létre.  Egy másik előfizetést tartalmazó VNet nem helyezhetők el a ASE

3. Jelölje ki vagy adja meg az új erőforráscsoport.  Az erőforráscsoport a ASE használt azonosnak kell lennie, amellyel a VNet.  Ha kijelöl egy már meglévő VNet majd az erőforrás csoport lehetőséget a ASE frissülnek a VNet tükrözi.

    ![][2]

4. Adja meg a helyét és virtuális hálózati beállításokat.  Megadhatja, hogy hozzon létre egy új VNet, vagy kattintson egy már meglévő VNet.  Ha egy új VNet adhatja meg a nevét és helyét, majd jelölje ki. Az új VNet lesz a cím tartomány 192.268.250.0/23 és **alapértelmezett** 192.168.250.0/24 definíciója nevű alhálózat.  Egyszerűen is kijelölhet egy már meglévő klasszikus vagy az erőforrás-kezelő VNet.  A virtuális típusának kiválasztása határozza meg, ha a ASE közvetlenül is elérhető az internetről (külső) vagy egy belső betöltés terheléselosztó (ILB) használ.  Ha többet szeretne róluk olvashatók [Egy belső terheléselosztó-alkalmazás szolgáltatás környezettel rendelkező][ILBASE].  Ha kijelöl egy külső virtuális típusú majd választhat hány külső IP-címek a rendszer IPSSL célokra jön létre.  Ha bejelöli a belső majd meg kell adja meg a altartomány, amelyekkel a ASE.  ASEs telepíthetők be virtuális hálózatokat, amelyek *mindkét* nyilvános címtartományok *vagy* RFC1918 cím szóközt (azaz használata a magánjellegű címeket).  Annak érdekében, hogy virtuális hálózatot használ egy nyilvános cím cellatartomány, szüksége lesz az VNet időszakokra létrehozásához.  Amikor kijelöl egy már meglévő VNet szüksége lesz hozzon létre egy új alhálózat ASE létrehozása során.  **A portál előre létrehozott alhálózathoz nem használhatók.  Ha az erőforrás-kezelő sablon használatával ASE hoz létre egy már meglévő alhálózat egy ASE hozhat létre.**  Egy ASE létrehozása a sablon használatát az információk ebben az esetben [létrehozása sablonból alkalmazás szolgáltatási környezetben] [ ILBAseTemplate] , és itt [létrehozása sablonból ILB alkalmazás szolgáltatási környezetben][ASEfromTemplate].

### <a name="details"></a>Részletek ###

Egy ASE 2 első befejeződik, és a 2 munkatársak jön létre.  Az első befejeződik használni kívánt a HTTP-/ HTTPS-végpont, és küldje el a forgalmat a dolgozók, amelyek az alkalmazások szolgáltató szerepköröket.   A mennyiség módosítása ASE létrehozását követően, és még akkor is létre lehet hozni Automatikus méretezéssel szabályok a következő erőforráskészletek.  További részletekért kézi méretezés köré, kezelése és az alkalmazás-szolgáltatási környezetben figyelése látogasson el ide: [az alkalmazás-szolgáltatási környezetben konfigurálása][ASEConfig] 

Csak az egyik ASE az alhálózathoz a ASE által használt is szerepel.  Az alhálózathoz nem használhatók az összes kívül a ASE

### <a name="after-app-service-environment-creation"></a>Alkalmazás-szolgáltatási környezetben létrehozása után ###

ASE létrehozását követően módosíthatja:

- Előtér mennyiségét (minimális: 2)
- Munkatársak mennyiségét (minimális: 2)
- Az IP-címek IP SSL elérhető mennyiség
- Az előtér és a munkatársak által használt erőforrás méretű számítja ki (előtér minimális méret: a P2)


Nincsenek körül kézi beosztását, kezelése és figyelemmel kísérése alkalmazás környezetek az alábbi részleteket: [az alkalmazás-szolgáltatási környezetben konfigurálása][ASEConfig] 

Autoscaling információt az alábbi útmutató: [az alkalmazás-szolgáltatási környezetben Automatikus méretezéssel konfigurálása][ASEAutoscale]

Nincsenek további függőségek, amelyek nem jelennek meg az adatbázist, és a tárhely például testreszabási.  Ezek Azure kezeli, és a rendszer beépített.  A rendszer tároló legfeljebb 500 GB támogatja a teljes alkalmazás-környezetben, és az adatbázis Azure úgy beállítani, szükség szerint a rendszer beosztásának.


## <a name="getting-started"></a>Első lépések
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Első lépések az alkalmazás-szolgáltatási környezetben, lásd: [alkalmazás szolgáltatási környezetben – bevezetés][WhatisASE]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
