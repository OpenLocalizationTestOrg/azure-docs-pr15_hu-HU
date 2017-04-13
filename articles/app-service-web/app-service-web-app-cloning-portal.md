<properties
    pageTitle="Web App klónozhatja Azure portál használatával"
    description="Megtudhatja, hogy miként klónozhatja a Web Apps alkalmazások új Web Apps alkalmazások Azure-portálon."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure alkalmazás szolgáltatás alkalmazás klónozhatja Azure portál használatával#

Az [Azure alkalmazás szolgáltatás Web Apps alkalmazásokban](http://go.microsoft.com/fwlink/?LinkId=529714) a klónképző szolgáltatás lehetővé teszi, egyszerűen klónozhatja meglévő web Apps alkalmazások egy újonnan létrehozott egy másik régióbeli vagy ugyanabban a régióban alkalmazásba. Ezzel engedélyezi az ügyfelek számára üzembe alkalmazások számos különböző területei között, gyorsan és egyszerűen.

Alkalmazás klónozhatja jelenleg csak a prémium réteg app milyen szolgáltatáscsomagok támogatott. Az új szolgáltatás a korlátozásai használja, mint a Web Apps alkalmazások biztonsági szolgáltatás, című témakörben talál, [készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Meglévő alkalmazás klónozhatja ##

A web app rendszernek kell futnia, mielőtt hozhat létre a webalkalmazás átirattal **prémium** módban.

1. Az [Azure-portálon](https://portal.azure.com/)nyissa meg a webalkalmazás lap.
2. Kattintson az **eszközök**. Ezután kattintson az **eszközök** lap **Adatfeliratsor alkalmazást**.

    ![][1]

    > [AZURE.NOTE]
    > A web app még nincs megnyitva a **prémium** módja az, ha kap egy a támogatott módok alkalmazás klónozhatja jelző üzenet. Ezen a ponton válassza a **frissítés**lehetőség van.
    
3. Az **Adatfeliratsor alkalmazás** lap adjon meg egy nevet az új web App alkalmazásban, a erőforráscsoport és az alkalmazás szolgáltatás megtervezése adja meg. A felhasználó fog is válassza ki, hogy a forrás web app beállításai számos klónozhatja vagy sem.

    ![][2]

4. Kattintás a **létrehozása** után a platform fog kezd dolgozni átirattal a forrás webalkalmazás létrehozása.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben meglévő alkalmazás klónozhatja##

Az **Adatfeliratsor alkalmazás** lap az ügyfél-alkalmazáskészlet választhatja ki a meglévő alkalmazás-szolgáltatási környezetben fog rendelkezni.

## <a name="current-restrictions"></a>Aktuális korlátozások ##

Ez a funkció jelenleg előzetes verzióban, új funkciók hozzáadása időbeli dolgozunk, az alábbi lista az ismert korlátozásai a jelenlegi támogatás alkalmazás klónozhatja az Azure-portálon:

- Azure forgalom Manager beállítások vannak nem klónozva
- Automatikus méretezés beállításai vannak nem klónozva
- Ütemezett biztonsági mentés beállításait a rendszer nem klónozva
- VNET beállítások vannak nem klónozva
- Alkalmazás háttérismeretek nem beállítása automatikusan megtörténik a célként megadott web App
- Egyszerű Auth beállítások vannak nem klónozva
- Kudu bővítmény nem vannak klónozva
- Tipp: szabályok vannak nem klónozva
- Adatbázis-tartalom nem vannak klónozva


### <a name="references"></a>Hivatkozások ###
- [Web App klónozhatja PowerShell használatával](app-service-web-app-cloning.md)
- [Készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md)
- [Hogyan hozhat létre az alkalmazás-szolgáltatási környezetben](app-service-web-how-to-create-an-app-service-environment.md)
- [Egy webalkalmazás létrehozása az alkalmazás-szolgáltatási környezetben](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Alkalmazás-szolgáltatási környezetben – bevezetés](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png