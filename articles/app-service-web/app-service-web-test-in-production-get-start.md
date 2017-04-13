<properties
    pageTitle="Teszt gyártási Web Apps alkalmazások – első lépések"
    description="Tudjon meg többet a tesztet a termelési (TiP) funkcióval az Azure-alkalmazás szolgáltatás webalkalmazásokban."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Teszt gyártási Web Apps alkalmazások – első lépések

Gyártási tesztelésének vagy a web appba élő ügyfelek forgalmat, live-tesztelése, hogy az alkalmazás a fejlesztők egyre integrálhatja a [Agilis fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development) módszertan próba stratégiát is. Lehetővé teszi az alkalmazások az élő felhasználói forgalom minőségének tesztelhet az üzemi környezetben, nem pedig tesztkörnyezetben előállított adatokban. Által az új alkalmazás valós felhasználóknak ki, akkor is tájékoztatni az alkalmazás arcra is, ha telepítve van a valós problémák. Ellenőrizheti, hogy a használható funkciók körét, teljesítményre és alkalmazás módosításokat azokat a mennyiségi, a sebesség és a különböző típusú, akiket soha nem közelíteni, tesztkörnyezetben valós felhasználói forgalom értéke.

## <a name="traffic-routing-in-app-service-web-apps"></a>Az alkalmazás szolgáltatás webalkalmazásokban útválasztás forgalom

[Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714)forgalom továbbítás funkcióval közvetlenül egy vagy több [telepítési helyek](web-sites-staged-publishing.md)élő felhasználói forgalom egy részét, és majd elemezni az alkalmazás [Azure alkalmazás Hírcsatornájában](/services/application-insights/) [Azure hdinsight szolgáltatáshoz](/services/hdinsight/), vagy [Új Relic](/marketplace/partners/newrelic/newrelic/) például külső eszköz ellenőrzése lehetőségre a módosításokat. Például az alábbi esetekben is alkalmazhat alkalmazás szolgáltatással:

- Funkcionális hibák felfedezése vagy a webhely teljes szélességén telepítési előtt a frissítések teljesítményét szűk pinpoint
- Végezze el a módosításokat "ellenőrzött vizsgálat repülőjegyek" mérése usibility mértékek a béta-alkalmazásba
- Fokozatosan felfelé egy új frissítés emelkedő és biztonságosan biztonsági a legújabb verzióra hiba történik, ha 
- Az alkalmazás üzleti eredmény optimalizálása futtatásával [A és B vizsgálat](https://en.wikipedia.org/wiki/A/B_testing) vagy több telepítési helyek [multivariate vizsgálat](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing)

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>A Web Apps alkalmazások használatához a forgalom útválasztás vonatkozó követelmények

- A web app **normál** vagy a **támogatási** réteg kell futtatni, mivel több telepítési helyek szükséges az.
- Annak érdekében, hogy a sikeresek, cookie-kat, a felhasználók böngészőjében engedélyezésük forgalom útválasztás szükséges. Adatforgalom útválasztás egy telepítési tárolóhely élettartama ügyfél böngészőt az ügyfél-munkamenet rögzítése cookie-kat használ.
- Adatforgalom útválasztás speciális tipp lehetőséget támogat Azure PowerShell-parancsmagok között.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>A telepítési tárolóhely útvonal forgalom szegmens

Egyszerű szintre minden tipp helyzetben az élő forgalom előre megadott százalékos szeretne egy nem éles üzemi tárolóhely átirányítja. Ehhez hajtsa végre az alábbi lépéseket:

>[AZURE.NOTE] Az alábbi lépésekkel feltételezi, hogy már [nem éles üzemi tárolóhely](web-sites-staged-publishing.md) és a megfelelő web app tartalom már [rendszerbe](web-sites-deploy.md) rá.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. A web App alkalmazásban a lap, kattintson a **Beállítások** > **Útválasztás forgalmat**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Jelölje ki a tárolóhely kívánt irányítja a forgalmat és a teljes forgalmat kívánja, majd kattintson a **Mentés**gombra.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. A telepítési tárolóhely lap megnyitásához. Ekkor megjelennek azt érkezik élő forgalmat.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Miután forgalom útválasztás van konfigurálva, a megadott százalékos értéket az ügyfelek véletlenszerűen átirányítja a tesztkörnyezetben tárolóhely. Azonban fontos tudni, hogy amikor egy ügyfél automatikusan annak biztosítására, hogy egy adott tárolóhely, akkor fog kell "rögzített", hogy az ügyfél munkamenethez élettartama tárolóhely. Ezzel elkészült a cookie-k használata a felhasználó munkamenet rögzítése. Ha a HTTP-kérések nézze meg, látni fogja a `TipMix` cookie-k minden későbbi összehívásban.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Egy adott tárolóhely ügyfél kérések kényszerítése

Továbbítás automatikus forgalom, mellett alkalmazás szolgáltatás nem tudja egy adott tárolóhely kérelmek. Ez akkor hasznos, ha azt szeretné, hogy a felhasználók tudjanak kiválaszthatják be vagy a béta-alkalmazás letiltásra. Ehhez használja a `x-ms-routing-name` paraméteres lekérdezés.

A felhasználóknak, hogy egy adott tárolóhely használatával átirányíthatja `x-ms-routing-name`, meg kell győződnie arról, hogy a tárolóhely már hozzáadta a forgalom útválasztási listában. Szeretne egy tárolóhely kifejezetten irányítja, mivel a tényleges útválasztási százalék, beállíthatja, nem számít. Ha azt szeretné, egy "béta hivatkozás", amelyre kattintva a felhasználók a béta-app elérése akkor is állítson össze.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Csatlakozás a felhasználóknak ki béta letölthető

Ha felhasználónak szeretné engedélyezni a béta-alkalmazás lemondása, például írható erre a hivatkozásra az weblapon:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

A karakterlánc `x-ms-routing-name=self` adja meg a gyártási tárolóhely. Miután az ügyfél böngészője férni a hivatkozást, nem csak azt irányítja át a gyártási tárolóhely, de minden későbbi kérelem fogja tartalmazni a `x-ms-routing-name=self` cookie-k, amely a termelési tárolóhely a munkamenet rögzíti.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Csatlakozás a felhasználók béta alkalmazásba

Hogy a felhasználók letölthessék a béta-alkalmazást, például a tesztkörnyezetben tárolóhely nevét állítsa be ugyanazt a lekérdezési paraméter:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>További források ##

-   [Állítsa be az Azure-App szolgáltatásban webalkalmazások környezetekben átmeneti tárolására](web-sites-staged-publishing.md)
-   [Azure-ban előre egy összetett alkalmazások telepítése](app-service-deploy-complex-application-predictably.md)
-   [Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással](app-service-agile-software-development.md)
-   [A web Apps alkalmazások hatékony DevOps környezetekben használata](app-service-web-staged-publishing-realworld-scenarios.md)