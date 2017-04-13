<properties
    pageTitle="Egy webalkalmazás létrehozása az alkalmazás-szolgáltatási környezetben"
    description="Megtudhatja, hogy miként hozhat létre web Apps alkalmazások és az app milyen szolgáltatáscsomagok az alkalmazás-szolgáltatási környezetben"
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
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Egy webalkalmazás létrehozása az alkalmazás-szolgáltatási környezetben

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy miként web Apps alkalmazások és az alkalmazás milyen szolgáltatáscsomagok létrehozása [Alkalmazás szolgáltatási környezetben](app-service-app-service-environment-intro.md) (SKB). 

> [AZURE.NOTE] Ha szeretné megtudhatja, hogy miként hozhat létre a megfelelő web App alkalmazásban, de nem kell tennie, az alkalmazás-szolgáltatási környezetben, olvassa el a [Létrehozás .NET webalkalmazást](web-sites-dotnet-get-started.md) vagy más nyelvek és keretek kapcsolódó oktatóanyagok egyik.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban feltételezi, hogy az alkalmazás-szolgáltatási környezetben hozott létre. Ha a, amely még nem kész, olvassa el az [létrehozása az alkalmazás-szolgáltatási környezetben](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Egy webalkalmazás létrehozása

1. Az [Azure-portálon](https://portal.azure.com/)kattintson a **Új > webes + mobil > Web App**. 

    ![][1]

2. Jelölje ki azt az előfizetést.  

    Ha több előfizetéssel rendelkezik ügyeljen arra, hogy hozhat létre az alkalmazást az alkalmazás-szolgáltatási környezetben, akkor használja a környezet létrehozásakor használt ugyanabban az előfizetésben. 

3. Jelölje be, vagy hozzon létre egy erőforrás csoportot.

    *Erőforrás csoportokkal* kezelheti a kapcsolódó Azure erőforrások egy egységként, és akkor hasznos, ha az alkalmazások *szerepköralapú hozzáférés-vezérlés* (RBAC) szabályokat létrehozásáról. További tudnivalókért lásd: [az Azure erőforrások kezelése][ResourceGroups]. 

4. Jelölje be, vagy hozzon létre egy alkalmazás szolgáltatáscsomagja.

    *App milyen szolgáltatáscsomagok* kezelhetők a web Apps alkalmazások beállítása.  A szokásos módon kijelölésekor árak, az ár érvényesül az alkalmazás szolgáltatáscsomagja helyett egyéni alkalmazás. Az egy ASE fizet a ASE rendelt számítási-példányok helyett a ASP van szerepel.  Ha át kívánja méretezni, méretezheti be az alkalmazás szolgáltatás példányainak webalkalmazást előfordulását számú tervet, és hatással van az összes webalkalmazások, hogy a csomagban.  Bizonyos szolgáltatások, például a webhely helyek vagy VNET integrációs is mennyiség korlátozások a terv belül.  További információ a [Azure alkalmazás szolgáltatás csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) című témakörben találhat.

    Az App milyen szolgáltatáscsomagok megjeleníti a helyet, a terv neve alatt kell jegyezni azonosíthatja a ASE a.  

    ![][5]

    Ha szeretne egy alkalmazás szolgáltatáscsomagja, amely már létezik az alkalmazás-szolgáltatási környezetben használja, jelölje ki azt. Ha létrehoz egy új alkalmazás szolgáltatás tervet, a következő részben: Ebben az oktatóanyagban [létrehozása egy alkalmazás szolgáltatáscsomagja az alkalmazás-szolgáltatási környezetben](#createplan).

5. Írja be a nevet, a webalkalmazásban, és kattintson a **Létrehozás**gombra. 

    Ha a ASE használ egy külső virtuális van-e egy ASE-alkalmazás URL-címe: [*sitename*]. [*az alkalmazás-szolgáltatási környezetben neve*]. [*sitename*] helyett p.azurewebsites.net. azurewebsites.net
    
    Ha a ASE használ egy belső virtuális, majd az alkalmazás URL-CÍMÉT, hogy a ASE: [*sitename*]. [*altartományt megadott ASE létrehozása során*]   
    Ekkor megjelenik a **neve** alatti frissítése altartomány ASE létrehozása során a ASP kijelölése után

## <a name="createplan"></a>Alkalmazás szolgáltatás terv létrehozása

Amikor az alkalmazás-szolgáltatási környezetben alkalmazás szolgáltatás csomagjával hoz létre, a dolgozók lehetőségeket eltérőek, ahány nem megosztott dolgozók egy ASE.  A dolgozók kell használnia, lépnek szeretne a ASE osztottak meg a rendszergazda  Ez azt jelenti, hogy új csomagot létrehozásához van szükség kiosztott összes a dolgozó készlethez-ös csomagok között a ASE dolgozó készlet a példányok száma-nél több dolgozók.  Ha nincs elég dolgozók a ASE dolgozó készlet létrehozása a csomag, szüksége használata a ASE rendszergazdának el őket hozzá.

Az alkalmazás-szolgáltatási környezetben által üzemeltetett App szolgáltatás csomagok másik különbség hiányoznak a kijelölés árak.  Amikor az alkalmazás-szolgáltatási környezetben is fizet, a rendszer által használt számítási erőforrások, és nem rendelkezik a tervek hozzáadott díjai, hogy a környezetben.  A szokásos módon alkalmazás szolgáltatás csomagjával létrehozásakor kijelöl egy árak tervet, amely meghatározza, hogy a számlázás.  Az alkalmazás-szolgáltatási környezetben lényegében hol tartalom hozhat létre saját hely.  A környezet, nem pedig a tartalom szolgáltató fizet.

Az alábbi utasításokat bemutatják, hogyan hozhat létre egy alkalmazás szolgáltatáscsomagja, az oktatóprogram korábbi részében című cikkben ismertetett módon webalkalmazást létrehozása közben.

1. A terv kijelölésbe felhasználói felületének **Új létrehozása** gombjára, és adja meg a csomag nevét, csak a szokásos módon egy ASE kívül.

2. Jelölje ki a használni kívánt a hely segítségével ASE.

    Mivel az alkalmazás-szolgáltatási környezetben lényegében egy privát telepítési helye, a hely csoportban látható. 

    ![][2]

    Után egy ASE a hely kiválasztása a listáját az alkalmazás szolgáltatás terv létrehozása felhasználói felület frissítése.  A hely megjelenik a ASE rendszer és a régió nevét, és a árak terv választó helyére egy dolgozó készlet választó.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Jelölje ki a dolgozó készletben

A szokásos módon Azure App szolgáltatásban, és az alkalmazás-szolgáltatási környezetben kívül vannak 3 számítási méretét, hogy a kijelölést ki egy dedikált ár terv találhatók.  Hasonló módon számára egy ASE azt dolgozók legfeljebb 3 forrásanyagok megadása, és adja meg, hogy dolgozó készlet használt számítási méretét.  Mi ez azt jelenti, a bérlők a hajlamosnak áll, hogy inkább számítási méretű alkalmazás szolgáltatás csomagja árak terv kiválasztása, a *dolgozók készlet*úgynevezett lehetőséget választja.  

A dolgozó készlet kijelölés felhasználói felület neve alatti dolgozó készlethez használt számítási méretét jeleníti meg.  A rendelkezésre álló mennyiség hivatkozik példányok érhetők el, hogy az erőforráskészlet használata hány számítja ki.  A teljes készletbe tulajdonképpen ennél több további előfordulásait, de ez az érték hivatkozik egyszerűen hány ki van kapcsolva.  Ha ki kell igazítania az alkalmazás-környezetből további kiszámítania erőforrások hozzáadása nézze meg [az alkalmazás-szolgáltatási környezetben konfigurálása](app-service-web-configure-an-app-service-environment.md).

![][4]

Ebben a példában a rendelkezésre álló csak két dolgozó készletek láthatók. Ennek oka az, a ASE rendszergazda csak kiosztott hosts e két dolgozó készletek be.  A harmadik volna jelenik meg, a mikrofonba kiosztott VMs esetén.  

## <a name="after-web-app-creation"></a>Web app létrehozása után

Néhány dolgot web Apps alkalmazások fut, és App milyen szolgáltatáscsomagok egy ASE kell figyelembe kell venni a kezeléséhez.  

Korábbi amint a ASE tulajdonosának felelős a rendszer méretének és eredményt ezek egyben biztosítva, hogy van elegendő kapacitása ahhoz, hogy a kívánt App milyen szolgáltatáscsomagok szolgáltató felelős. Ha nem érhető el dolgozók, nem tudja a alkalmazás díjcsomagjától létrehozása.  Ez akkor is igaz méretezés felfelé a web App alkalmazásban.  Ha további előfordulásait majd meg szeretné, hogy az alkalmazás-szolgáltatási környezetben-rendszergazda hozzáadása több dolgozók megszerezni.

A web app és az alkalmazás szolgáltatáscsomagja létrehozása után célszerű az átméretezi, hogy.  Az egy ASE mindig van szükség a alkalmazás díjcsomagjától hibatűrést nyújt az alkalmazások legalább 2 példányát.  Egy alkalmazás szolgáltatáscsomagja méretezési egy ASE megegyezik a felhasználói felület alkalmazás szolgáltatás tervét normál.  További információt a beosztását, [Ha át kívánja méretezni, az alkalmazás-szolgáltatási környezetben webalkalmazást hogyan](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
