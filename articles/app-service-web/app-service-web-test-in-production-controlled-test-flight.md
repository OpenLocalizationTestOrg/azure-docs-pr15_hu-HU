<properties
    pageTitle="Flighting telepítési (bétatesztelés) Azure App szolgáltatásban"
    description="Megtudhatja, hogy miként az alkalmazás új szolgáltatások légi vagy a frissítések, a végpontok közötti oktatóprogram beta tesztelése céljából. Ez az alkalmazás szolgáltatás szolgáltatásokat, például a folyamatos közzététel, a helyek, a forgalom Útválasztás és a alkalmazás háttérismeretek integráció egyetlen."
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
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting telepítési (bétatesztelés) Azure App szolgáltatásban

Ebből az oktatóanyagból megtudhatja, hogy hogyan megteheti, hogy a *flighting telepítések* integrálása az [Azure-alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) és az [Azure alkalmazás háttérismeretek](/services/application-insights/)különböző funkciók. 

*Flighting* egy telepítési folyamatot, amely ellenőrzi a egy új funkcióval vagy a valós ügyfelek korlátozott számú módosítása, és a fő gyártási helyzetben tesztelése. Mintha béta tesztelése és a "ellenőrzött próba nézetbeli" néha nevezik. Sok nagyvállalatok egy webes jelenlét segítségével ezt a megközelítést korai érvényességi meg az alkalmazás-frissítések a [Agilis](https://en.wikipedia.org/wiki/Agile_software_development)fejlesztési a gyakorlathoz. Azure alkalmazás szolgáltatás lehetővé teszi, hogy próba gyártási integrálása folyamatos közzététel, és az alkalmazás az összefüggéseket a DevOps ugyanolyan eseteket végrehajtásához. Többek között ezt a megközelítést előnyöket nyújtja:

- **Valós visszajelzés _előtt_ frissítések termelési nyereség** - nagyobb mértékben elveszti visszajelzést, amint felengedi beállítást van elveszti visszajelzést, felengedése előtt. Korai megfelel a életciklusáról a tesztelheti valós felhasználói forgalmat és viselkedések frissül.
- **Rámutatás [próba-alapú folyamatos fejlesztési (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** – által gyártási próba integrálása folyamatos integrációs- és műszerezettségi alkalmazás hírcsatornájában, a felhasználó-ellenőrzési történik korai, és automatikusan a termék életciklusának. Ezzel az elemzéssel próba kézi végrehajtás befektetések idő csökkentése.
- **Optimalizálás vizsgálat munkafolyamat** - folyamatos megfigyeléssel műszerezettségi, és munkakörnyezeti vizsgálat automatizálásával esetleg elvégezhető egyetlen folyamat, például [integráció](https://en.wikipedia.org/wiki/Integration_testing), [regressziós](https://en.wikipedia.org/wiki/Regression_testing), [használhatósági](https://en.wikipedia.org/wiki/Usability_testing), kisegítő, honosítási, [Teljesítmény](https://en.wikipedia.org/wiki/Software_performance_testing), [Biztonság](https://en.wikipedia.org/wiki/Security_testing)és [elfogadó](https://en.wikipedia.org/wiki/Acceptance_testing)teszteket a különféle céljainak.

Egy flighting telepítési szinte nem útválasztás élő forgalmat. Ilyen környezetben betekintést minél gyorsabban nyereség szeretné hogy akkor egy váratlan hiba, a teljesítmény csökkenés, a felhasználói élmény problémák. Ne feledje, hogy valós ügyfelek foglalkoznak. Így adunk jobbra, gondoskodnia kell arról, hogy állított be a flighting üzembe gyűjtése a következő lépésben tájékozott döntés szükséges összes adatot. Ebből az oktatóanyagból megtudhatja, hogy miként adatgyűjtés az alkalmazás hírcsatornájában, de új Relic és egyéb technológiákat igényektől illő is használhatja. 

## <a name="what-you-will-do"></a>Mit érhetek

Ebből az oktatóanyagból megtanulhatja hogyan jelenítse meg az alábbi esetekben közös gyártási az alkalmazás szolgáltatás alkalmazás vizsgálandó:

- A béta-alkalmazás [útvonal gyártási forgalom](app-service-web-test-in-production-get-start.md)
- [Az alkalmazás instrument](../application-insights/app-insights-web-track-usage.md) hasznos mértékek beszerzése
- Folyamatosan a béta-alkalmazások terjesztése és nyomon követheti az élő alkalmazás mérőszámok
- A gyártási és a béta-alkalmazást, tekintheti meg, hogyan kód módosításai Célnyelv listájában a találatok között mértékek összehasonlítása

## <a name="what-you-will-need"></a>Amire szüksége lesz

-   Azure-fiók
-   Egy [GitHub](https://github.com/) fiók
- Visual Studio 2015 - töltheti le a [közösségi edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Mely számjegy rendszerhéj (telepítve a [GitHub Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy a mely számjegy és a PowerShell parancsai futtathatók az adott munkamenetben
-   Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bittel
-   Egyszerű ismertetése a következőket:
    -   [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) -sablon telepítésének (lásd: a [Deploy egy összetett alkalmazás Azure-ban előre](app-service-deploy-complex-application-predictably.md))
    -   [Mely számjegy](http://git-scm.com/documentation)
    -   [A PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Az oktatóprogram elvégzéséhez Azure-fiók van szüksége:
> + [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/) is - credits kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a azokat megszokott után is választhatja, hogy a fiók, és használata ingyenes Azure szolgáltatások, például a Web Apps alkalmazások.
> + [Visual Studio előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/) is – a Visual Studio előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.
>
> Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="set-up-your-production-web-app"></a>A gyártási web app beállítása

>[AZURE.NOTE] Az ebben az oktatóanyagban használt parancsfájl automatikusan beállítja a GitHub tárházba folyamatos közzététel. Ehhez az, hogy a GitHub hitelesítő adatok már tárolják Azure-ban, egyéb esetben a parancsfájl telepítés meghiúsul, amikor megpróbál a web Apps alkalmazások adatforrás-vezérlő beállításainak konfigurálása.
>
>A GitHub hitelesítő adatainak tárolása Azure-ban, az [Azure-portál](https://portal.azure.com/) és - [GitHub telepítésének konfigurálásához](app-service-continuous-deployment.md#Step7)az hozzon létre egy web App alkalmazásban. Csak kell egyszer.

Egy tipikus DevOps esetben egy alkalmazást futtató élő Azure-ban van, és meg szeretné módosítani folyamatos közzétételi keresztül. Ebben az esetben telepíti a termelési egy sablont, amely fejlesztett, és tesztelje.

1.  Hozzon létre saját elágazás, a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárat. A elágazás létrehozásával kapcsolatos további tudnivalókért lásd: [Elágazás egy repó](https://help.github.com/articles/fork-a-repo/). Amikor létrejött a elágazás, megtekintheti a böngészőben.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Nyisson meg egy mely számjegy rendszerhéj munkamenetet. Ha még nincs mely számjegy rendszerhéj, telepítse a [GitHub a Windows](https://windows.github.com/) most már.
3.  Hozzon létre egy helyi adatfeliratsor a elágazás a hajtja végre a következő parancsot:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Ha befejezte a helyi adatfeliratsor, nyissa meg azt * &lt;repository_root >*\ARMTemplates, és a Futtatás a deploy.ps1 parancsfájl egyedi utótag beállítva, az alább látható módon:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Amikor a rendszer kéri, írja be a kívánt felhasználónév és jelszó-adatbázis eléréséhez. Ne feledje a adatbázis hitelesítő adatait, mert szüksége lesz az újra megadhatja őket az erőforráscsoport frissítésekor.

    Meg kell jelennie a kiépítési végrehajtásának különböző Azure erőforrásokhoz. Telepítés befejezésekor a parancsfájl indítsa el az alkalmazást a böngészőben, és egy felhasználóbarát hangjelzést ad.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Vissza a mely számjegy rendszerhéj munkamenetben futtatása:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Amikor befejezte a parancsfájlt, visszatérhet tallózással keresse meg a frontend cím (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) a gyártási futó alkalmazás megjelenítéséhez.
5.  Jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és nézze meg, hogy milyen létrehozott.

    Két web Apps alkalmazások azonos erőforrás csoportjában az egyik látja látnia kell a `Api` utótag nevét. Ha megnézi az erőforrás-csoport nézet is látni fogja az SQL-adatbázis és kiszolgáló, az alkalmazás szolgáltatáscsomagja és az átmeneti tárolásra szolgáló helyek a web Apps alkalmazások. Tallózással keresse meg a különböző forrásokból, és hasonlítsa össze őket a * &lt;repository_root >*\ARMTemplates\ProdAndStage.json kattintva megtekintheti, hogy hogyan konfigurálhatók a sablonban.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

A gyártási alkalmazás beállítása.  Most vegyük elképzelheti, hogy Ön visszajelzéseket, hogy az alkalmazás gyenge-e használhatósági. Így úgy dönt, hogy vizsgálja meg. Visszajelzés küldése, hogy az alkalmazás eszköz fogja.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Vizsgálja meg a: Az ügyfél-alkalmazás figyelése és mérőszámok az eszköz

5. Nyissa meg * &lt;repository_root >*a Visual Studio \src\MultiChannelToDo.sln.
6. Minden Nuget csomagon visszaállításához kattintson a jobb gombbal a megoldást > **NuGet csomagok kezelése megoldás** > **visszaállítása**.
6. Kattintson a jobb gombbal a **MultiChannelToDo.Web** > **Alkalmazás háttérismeretek Telemetriai hozzáadása** > **Beállításainak konfigurálása** > Módosítás erőforráscsoport ToDoApp*&lt;your_suffix >* > **Alkalmazás háttérismeretek hozzáadása a projekthez**.
7. Az Azure-portálon nyissa meg az alkalmazás betekintést **MultiChannelToDo.Web** erőforrás lap. Az **alkalmazás állapot** részen kattintson **megtudhatja, hogyan gyűjthetők össze az adatok a böngésző lap betöltése** > másolja a kódot.
7. Adja hozzá a másolt JS műszerezettségi kódot * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml közvetlenül a záró előtt `<heading>` címke. Az egyedi műszerezettségi billentyűt az alkalmazás betekintést erőforrás tartalmaznia kell.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Küldje el egyéni események alkalmazás háttérismeretek az egér kattintással törzsébe alján az alábbi kód hozzáadásával:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    A JavaScript kódtöredékének egyéni esemény küld az alkalmazás az összefüggéseket minden alkalommal, amikor a felhasználó tetszőleges kattint a web App alkalmazásban.

12. A mely számjegy felületén véglegesítése és küldeni a a módosításokat a elágazás GitHub a. Ezután várja meg, az ügyfelek, hogy frissítsék a böngészőben.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  A telepített alkalmazás módosítások gyártási felcserélése:

        .\swap –Name ToDoApp<your_suffix>

13. Tallózással keresse meg az alkalmazást az összefüggéseket erőforrás konfigurált. Kattintson az egyéni események.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Ha nem látja az egyéni események mértékek, várjon néhány percet, és kattintson a **frissítés**parancsra.

Tegyük fel, hogy látható diagram például az alábbi:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

És az esemény rács alatti:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

A ToDoApp alkalmazás kód megfelelően a **GOMB** esemény felel meg a Küldés gombra, és a **bemeneti** esemény felel meg a mezőben lévő értéket. Eddig dolog, amit stimmelnek. Jó helyen jár úgy tűnik, nincs a kattintással és a nagyon néhány kattintással a teendők ( **LI** események) a jó összege.

A hipotézis, amely az egyes felhasználók számára, űrlap ez alapján zavaros, mely a felhasználói felületének része kattintható, és mivel a kurzort a kijelölést a kijelölt szöveg stílussal, amikor azt mutat, a listaelemeket és ikonjaik.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Lehet, hogy egy contrived példa. Ennek ellenére fogjuk ellenőrizze, hogy az alkalmazás javítása, és hajtsa végre az flighting telepítés úgy juthat az élő ügyfelek használhatósági visszajelzést.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>A figyelés és mérőszámok server alkalmazás eszköz
Ez a egy tangens, mivel az alkalmazási példát, ebben az oktatóanyagban igazolni csak az ügyfélprogram alkalmazás foglalkozik. Azonban a teljesség fog beállítani a kiszolgálóoldali alkalmazást.

6. Kattintson a jobb gombbal a **MultiChannelToDo** > **Alkalmazás háttérismeretek Telemetriai hozzáadása** > **Beállításainak konfigurálása** > Módosítás erőforráscsoport ToDoApp*&lt;your_suffix >* > **Alkalmazás háttérismeretek hozzáadása a projekthez**.
12. A mely számjegy felületén véglegesítése és küldeni a a módosításokat a elágazás GitHub a. Ezután várja meg, az ügyfelek, hogy frissítsék a böngészőben.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  A telepített alkalmazás módosítások gyártási felcserélése:

        .\swap –Name ToDoApp<your_suffix>

Az egész!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Vizsgálja meg a: Tárolóhely-specifikus címkék hozzáadása az ügyfél alkalmazás mérőszámok

Ebben a részben beállítja a különböző telepítési helyek tárolóhely-specifikus telemetriai küldeni ugyanaz az erőforrás alkalmazás az összefüggéseket. Ezzel a módszerrel telemetriai adatokat másik helyek (telepítési környezetekben) érkező forgalmat közötti összehasonlíthatja egyszerűen tekintheti meg az app változtatások hatását. Egyszerre akkor is külön a termelési forgalom többi, továbbra is szükség szerint az gyártási-alkalmazás figyelése.

A ügyfél viselkedésének adatgyűjtés használja, mivel lehetővé teszi a [egy telemetriai inicializálója a JavaScript-kód hozzáadása](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) a index.cshtml. Kiszolgálóoldali teljesítmény vizsgálni kívánt, például azt is megteheti hasonlóan a kiszolgáló kódban (lásd: [Alkalmazás háttérismeretek API egyéni események és mérőszámok](../application-insights/app-insights-api-custom-events-metrics.md).

1. Először vegye fel a kód bewteen a két `//` Megjegyzések alatt a JavaScript blokkolása Önnek hozzáadni a `<heading>` nyomon követése a korábbi verziójában.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Inicializálója kód megjeleníti a `appInsights` objektum hozzáadása a egyéni tulajdonság neve `Environment` telemetriai küld, a minden részletéhez.

2. Ezután adja hozzá egyéni tulajdonság [tárolóhely beállítás](web-sites-staged-publishing.md#AboutConfiguration) a webalkalmazás az Azure-ban. Ehhez a következő parancsokat a mely számjegy rendszerhéj munkamenetben.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    A projektben a Web.config már határozza meg a `environment` app beállítása. Ezt a beállítást, a helyi meghajtóra, az alkalmazás tesztelésekor a mértékek fog címkézve van `VS Debugger`. Jó helyen jár, a változtatásokat az Azure megnyomása Azure megtalálja, és használja a `environment` beállítást használja a web app konfigurációs alkalmazást, és a mértékek címkével ellátott `Production`.

3. Véglegesítése és küldeni a a kód módosításokat a elágazás a GitHub, és majd várja meg a felhasználóknak, hogy az új alkalmazással (szükség frissítse a böngészőt). Az új tulajdonság megjeleníthető az alkalmazás meglátásait körülbelül 15 percet vesz igénybe `MultiChannelToDo.Web` erőforrás.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Ezután nyissa meg az **egyéni események** lap ismét, a mérőszámok szűrni `Environment=Production`. Most már láthatja a szűrő összes az új egyéni események a termelési tárolóhely.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Kattintson a **Kedvencek** gombra az aktuális mértékek Explorer beállításainak mentéséhez hasonló **egyéni események: gyártási**. Könnyedén válthat a nézet és a telepítés tárolóhely nézet között újabb.

    > [AZURE.TIP] Még hatékonyabb analytics érdemes megfontolni [integrálása a Power BI alkalmazás háttérismeretek erőforrás](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>A kiszolgáló alkalmazás mértékek tárolóhely-specifikus megcímkézése
Ismét a teljesség fog beállítani a kiszolgálóoldali alkalmazást. Eltérően a ügyfél alkalmazást, amely a JavaScript rendszereken van a kiszolgáló alkalmazáshoz tárolóhely-specifikus címkéket .NET kóddal van rendszereken.

1. Nyissa meg * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Adja hozzá a közvetlenül a záró előtt az alábbi kód Tiltás névtér kapcsos zárójelet.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Javítsa ki a név felbontás hibákat hozzáadásával a `using` kimutatásaiban alatt az elejére a fájlt:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Vegye fel a kódrészletet alatti elejére a `Application_Start()` metódus:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Véglegesítése és küldeni a a kód módosításokat a elágazás a GitHub, és majd várja meg a felhasználóknak, hogy az új alkalmazással (szükség frissítse a böngészőt). Az új tulajdonság megjeleníthető az alkalmazás meglátásait körülbelül 15 percet vesz igénybe `MultiChannelToDo` erőforrás.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Frissítés: A béta-fiók beállítása

2. Nyissa meg * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json és a Keresés a `appsettings` erőforrások (keresése `"name": "appsettings"`). Vannak olyan 4 egyik az egyes tárolóhely őket. 

2. Az egyes `appsettings` erőforrások hozzáadása egy `"environment": "[parameters('slotName')]"` végére app beállítása a `properties` tömb. Ne felejtse el egy vesszővel elválasztva az előző sor végére.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Csak hozzáadta a `environment` a sablon összes helyek alkalmazás beállítást.
    
2. Ugyanannak a fájlnak, keresse meg a `slotconfignames` erőforrások (keresése `"name": "slotconfignames"`). Vannak olyan 2 egyik minden alkalmazás őket.

2. Az egyes `slotconfignames` erőforrások hozzáadása `"environment"` végére a `appSettingNames` tömb. Ne felejtse el egy vesszővel elválasztva az előző sor végére.

    Csak saját a `environment` alkalmazás meghajtóra beállítást a megfelelő telepítési tárolóhely-mindkét alkalmazást.  

3. A mely számjegy rendszerhéj munkamenetben, a következő parancsokat utótaggal ugyanaz az erőforrás csoport korábban használt.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Amikor a rendszer kéri, adja meg a SQL adatbázis a hitelesítő adatait, mielőtt. Amikor a rendszer az erőforráscsoport frissítése, írja be `Y`, majd `ENTER`.

    Miután befejeződött a parancsfájl, őrződnek meg az eredeti erőforráscsoport az erőforrások, de egy új tárolóhely "béta" nevű akkor jön létre ugyanazt a konfigurációt, mint a "Átmeneti" tárolóhely az elején hozza létre.

    >[AZURE.NOTE] Ezt a módszert létrehozása különböző telepítési környezetekben eltér a módszerrel a [Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással](app-service-agile-software-development.md). Ebben az esetben telepítési környezetekben létrehozását telepítési helyek, amennyit kezdeményezik telepítési környezetekben erőforrás csoporttal. Telepítési környezetekben kezelése az erőforrás-csoportok lehetővé teszi az éles üzemi környezetben fejlesztők off-limits megtartani, de még nem egyszerűen gyártási, amely egyszerűen tehet helyek tesztelje.

Ha kívánja, is létrehozhat egy alfa app futtatásával

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Az ebben az oktatóanyagban meg fog csak folytassa a a béta-alkalmazást.

## <a name="update-push-your-updates-to-the-beta-app"></a>Frissítés: Leküldéses a frissítéseket a béta-alkalmazásba

Vissza a az alkalmazás javítása, amelyet.

1. Ellenőrizze, hogy Ön már a béta ág

        git checkout beta

2. A * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, keresse meg a `<li>` címkézéséhez, majd adja hozzá a `style="cursor:pointer"` attribútum, az alább látható módon.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. Véglegesítés és az Azure leküldéses.

4. Ellenőrizze, hogy a változtatások most tükröződik a béta tárolóhely http://todoapp való navigálással*&lt;your_suffix >*-beta.azurewebsites.net/. Ha még nem látható a módosítása, frissítse az új javascript-kód beolvasása a böngészőben.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Most, hogy a módosításokat a béta tárolóhely fut, készen áll a flighting telepítés végrehajtásához.

## <a name="validate-route-traffic-to-the-beta-app"></a>Ellenőrzése: A béta-alkalmazás útvonal forgalom

Ebben a szakaszban a béta-alkalmazásba irányítja a forgalmat. A bemutató áttekinthetőség for sake of fogjuk egy jelentős részét a felhasználó forgalom átirányítása. A valóságban kívánt irányítja a forgalmat mennyiségét megtervezésével függ. Például ha a webhely a microsoft.com beosztásának, majd szükség lehet a teljes forgalom kisebb, mint egy százalék hasznos adatok eléréséhez.

1. A mely számjegy Shell-munkamenetet futtassa a következő parancsokat, és a gyártási forgalmat a fele a béta tárolóhely irányítja:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  A `ReroutePercentage=50` tulajdonság adja meg, hogy a gyártási forgalom 50 %-os átirányítja a béta-app URL-címe (által megadott a `ActionHostName` tulajdonság).

2. Nyissa meg most http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. a forgalom 50 %-os célszerű most átirányítja a béta tárolóhely.

3. Az alkalmazás háttérismeretek az erőforrások, a mérőszámok szerinti szűrés környezet = "béta".

    > [AZURE.NOTE] Ha ez a szűrt nézet mentése másik felvétele a Kedvencek közé, majd, is egyszerűen tükrözése a metrikus explorer nézetek gyártási és a béta-nézetek között

Tegyük fel, hogy az alkalmazás Hírcsatornájában látni az alábbihoz hasonló:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nem csak a látható, hogy nincsenek-e sok plusz kattintással a a `<li>` címkék, de úgy tűnik, hogy a kattintással egy hullámzó jellegű a `<li>` címkék. Majd köt, hogy a felhasználók az új van talált `<li>` címkék kattintható, és most már vannak törölje a jelet az összes korábban szerzett feladatukat az alkalmazást.

A flighting üzembe adatok alapján, eldöntheti, hogy az új felhasználói felület készen áll a termelési.

## <a name="go-live-move-your-new-code-into-production"></a>Lépjen a élő: az új kód áthelyezése előállítása

Most már készen áll a frissítés áthelyezése gyártási. Mi az nagy, hogy most már tudja, hogy a frissítés már ellenőrzött _előtt_ azt tolódik gyártási. Most nyugodtan telepítheti azt. Óta elvégzett frissítés a AngularJS ügyfél alkalmazásba, csak érvényesíteni a ügyféloldali kódot. Ha módosítani szeretné a háttéradatbázis webes API-app volt, érvényesítés sikerült a módosításokat, hasonlóan és egyszerűen.

1. Mely számjegy rendszerhéj távolítsa el a forgalom útválasztási szabály a következő parancs futtatásával:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. A mely számjegy parancsokat:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Várjon néhány percet, amíg az új kódot telepítendő az átmeneti tárolásra szolgáló tárolóhely, majd indítsa el a http://ToDoApp*&lt;your_suffix >*-ellenőrzéséhez, hogy az új frissítést az átmeneti tárolásra szolgáló tárolóhely a bemelegíteni staging.azurewebsites.net. Ne feledje, hogy a fő ág a elágazás az átmeneti tárolásra szolgáló tárolóhely az alkalmazás kapcsolódik.

3. Az átmeneti tárolásra szolgáló tárolóhely éles felcserélése Now,

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Összefoglalás ##

Azure alkalmazás szolgáltatás egyszerűen a kis - és közepes méretű vállalkozások valami ellenőrzéséhez a ügyfélkapcsolati alkalmazások gyártási, hagyományos végrehajtott nagy vállalkozások. Remélhetőleg ebben az oktatóanyagban meghatalmazta a eredményét alkalmazás szolgáltatás és az alkalmazás elemzéseket, hogy a lehetséges flighting környezetben, és még a többi próba a termelési az esetek a DevOps világ szükséges ismereteket. 

## <a name="more-resources"></a>További források ##

-   [Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással](app-service-agile-software-development.md)
-   [Állítsa be az Azure-App szolgáltatásban webalkalmazások környezetekben átmeneti tárolására](web-sites-staged-publishing.md)
-   [Azure-ban előre egy összetett alkalmazások telepítése](app-service-deploy-complex-application-predictably.md)
-   [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md)
-   [JSONLint – a JSON-érvényesítő](http://jsonlint.com/)
-   [Mely számjegy elágaztatási – egyszerű elágaztatási és egyesítése](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
