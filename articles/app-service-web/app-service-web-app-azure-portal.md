<properties
    pageTitle="Összefoglalás: a Navigálás az Azure-portálra"
    description="Ismerje meg, a más felhasználói élményt alkalmazás szolgáltatás webes az adatkezelési portál és az Azure-portálon között"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Összefoglalás: a Navigálás az Azure-portálra

Azure webhelyek [Alkalmazás szolgáltatás Web Apps alkalmazások](http://go.microsoft.com/fwlink/?LinkId=529714)most neve. Fejlesztjük, ezért érdemes az összes a dokumentáció megfelelően a név megváltoztatása, és útmutatást adunk az Azure-portálra. Mindaddig, amíg a folyamat befejezése után is használhatja a dokumentum egy segédvonalat a Web Apps alkalmazások használata az Azure-portálon.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>A tervek az Azure klasszikus portálra

Megfigyelheti, hogy az esetleges módosítások az Azure klasszikus portálon, a portálon is te000129565 helyettesíti az Azure-portálra. A klasszikus portál van folyamatban fokozatosan, mint a fókuszt az új fejlesztés eltolása van az Azure-portálra. Az összes várható új funkciók Web Apps alkalmazások érkezzenek az Azure-portálon. Indítsa el az Azure Portal segítségével kihasználhatja a legújabb és a legnagyobb, hogy van-e Web Apps alkalmazások kínálatát.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>A klasszikus Azure-portál és -Azure portál elrendezés eltérések

A klasszikus portál az Azure szolgáltatások szerepelnek a bal oldali. A klasszikus portálon navigációs fastruktúrában, ahol indítsa el a szolgáltatás, és nyissa meg az egyes elemek követi. Ez a struktúra jól használható, amikor független összetevők kezelése. Azonban Azure-ra épülő alkalmazások összekapcsolt szolgáltatások gyűjteménye, és a fastruktúra nem ideális a webhelycsoportok szolgáltatások használatához. 

Az Azure portál megkönnyíti az alkalmazások-végpont több szolgáltatásból elemekkel összeállítása. A portál *utakra*szerint vannak rendezve. *Utazás* *pengéit*, amelyek a különböző összetevők tárolók sorozata. Ha például beállítását Automatikus méretezés a webalkalmazást *utazás* , amely megnyitja több pengéit az alábbi példában látható módon: a **webhely** lap (, hogy a lap címét van még nem frissített használni az új terminológia), a **Beállítások** lap, és a **Méretezés meg** a lap. A példában az automatikus méretezés beállítása történik processzorhasználata, függ, ezért a **Processzor százalékos** lap. A *pengéit* összetevőit úgynevezett *részeket*, amelyek a következőhöz hasonlóan csempék. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Navigációs példa: egy webalkalmazás létrehozása

Új web Apps alkalmazások létrehozása akkor is ugyanolyan egyszerű, mint 1-2-3. Az alábbi képen látható a klasszikus portál és a portálon egymás mellett, hogy nem nagy lépéseket webalkalmazást kaphatná, és futtasson száma megváltozott bemutatására. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

A portálon a leggyakoribb web Apps alkalmazások, többek között a népszerű gyűjtemény alkalmazásokban, például a WordPress választhat. Egy teljes listát az elérhető alkalmazások keresse fel a [Microsoft Azure piactéren].

Webalkalmazást létrehozásakor megadott URL-CÍMÉT, alkalmazás szolgáltatáscsomagja és helyét a portálon csak, mint a klasszikus portálon. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Ezenkívül a portálon meghatározhatja, hogy más általános beállítások. Ha például [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) könnyítik funkcióval megnézheti és kezelheti a kapcsolódó Azure erőforrások. 

## <a name="navigation-example-settings-and-features"></a>Példa a navigációs: beállításainak és funkcióinak

A beállítások és szolgáltatások most logikailag csoportosítva vannak egy egyetlen lap, amelyből navigálhat.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Egyéni tartományok, például a **Beállítások** lap az **egyéni tartományok és az SSL** kattintva hozhat létre.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Egy ellenőrzési figyelmeztetés beállításához kattintson **kérelmek és a hibák** , majd **Értesítés hozzáadása**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Ahhoz, hogy a diagnosztika, kattintson a **Beállítások** lap a **diagnosztikai naplók** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Alkalmazás beállításainak megadásához kattintson a **Beállítások** lap **beállításai** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

A márka neve nem néhány dolog, amit a portálon már átnevezni vagy másképp csoportosítva, így azok könnyebben megtalálhassák őket a. Ha például az alábbi az alkalmazáshoz a megfelelő lap látható beállítások (**beállítás**) a klasszikus portálon.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>További források

[Azure Portal]: https://portal.azure.com
[Azure piactérről]: /marketplace/

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás című: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)
 
