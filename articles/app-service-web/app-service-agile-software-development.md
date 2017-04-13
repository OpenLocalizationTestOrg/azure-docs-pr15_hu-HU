<properties
    pageTitle="Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással"
    description="Megtudhatja, hogyan hozhat létre a magas szintű összetett alkalmazások Azure alkalmazás szolgáltatással oly módon, amely támogatja az Agilis szoftverfejlesztési."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>


# <a name="agile-software-development-with-azure-app-service"></a>Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással #

Ebből az oktatóanyagból megismerheti a magas szintű összetett alkalmazások létrehozása az [Azure](/services/app-service/) -alkalmazás szolgáltatással oly módon, amely támogatja az [Agilis szoftverfejlesztési](https://en.wikipedia.org/wiki/Agile_software_development)fog. Azt feltételezi, hogy már tudja, hogyan [Azure-ban előre összetett alkalmazások](app-service-deploy-complex-application-predictably.md)telepítéséhez.

Műszaki folyamatokban korlátozások gyakran is útjában Agilis módszerek sikeres végrehajtása. Azure alkalmazás szolgáltatás szolgáltatásokkal, például a [folyamatos közzététel](app-service-continuous-deployment.md), [átmeneti tárolására környezetben](web-sites-staged-publishing.md) (helyek) és [figyelése](web-sites-monitor.md), amikor az üzletifolyamat-tervezési és kezelése az [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md)-telepítés bölcs párosított lehet a fejlesztők számára, akik Agilis szoftverfejlesztési kibővül kiváló megoldás részét.

Az alábbi táblázat rövid felsoroljuk Agilis fejlesztése, és hogyan az Azure szolgáltatás lehetővé teszi, hogy minden közülük társított követelményeinek.

| Követelmények | Hogyan Azure lehetővé teszi, hogy |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Összeállítása a minden jóváhagyás<br>-Automatikusan összeállítása és gyors | Folytonos telepítési konfigurálásakor Azure alkalmazás szolgáltatás működhetnek live futó buildjeiben a fejlesztők ág alapján. Minden alkalommal, amikor a kódot a ág tolódik, érdemes automatikusan beépített és Azure-ban futó élő.|
| – Ellenőrizze hozza létre saját tesztelése | Betöltése, vizsgálatok, webes azt vizsgálja, stb, az erőforrás-kezelő Azure-sablon telepíthető.|
| -Ellenőrzések végrehajtásához a üzemi környezetben, átirattal | Azure erőforrás-kezelő sablonok létrehozása az Azure munkakörnyezetben (beleértve a alkalmazás beállításainak, a kapcsolati karakterlánc sablonok, a méretezés, a stb.) az teszteléséhez gyorsan és előre a klónok használható.|
| -Megtekintése legújabb verziójában eredményét egyszerűen | Azure összegyűjti a folyamatos telepítést, az azt jelenti, hogy tesztelheti új kód egy élő alkalmazásban, a módosítások végrehajtása után azonnal. |
| -Érvényesíteni a fő ág minden nap<br>-Telepítés automatizálása | Folytonos integráció a tárházba fő ág egy gyártási alkalmazás minden jóváhagyás egyesítés automatikusan a fő gyártási beállításra, hogy üzembe helyezése. |

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-do"></a>Mit érhetek ##

Egy tipikus fejlesztők-próba-szakasz-gyártási munkafolyamat annak érdekében, hogy új módosításainak közzététele a minta két [web Apps alkalmazások](/services/app-service/web/), a frontend (FE), a másik a webes API kódmentes (BE), és az [SQL-adatbázis](/services/sql-database/)egyik áll [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) alkalmazás fog végigvezetik. A telepítési architektúra alább látható módon működnek:

![](./media/app-service-agile-software-development/what-1-architecture.png)

A kép üzembe szavak:

-   A telepítési architektúra van osztva három egyedi környezetek (vagy [az erőforrás csoportok](../azure-resource-manager/resource-group-overview.md) Azure-ban), a saját [alkalmazás szolgáltatáscsomagja](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), [méretezési](web-sites-scale.md) beállításokat és SQL-adatbázishoz. 
-   Az egyes környezetekben külön-külön kezelhető. Vannak még ilyenek, a különböző előfizetések.
-   Az azonos alkalmazás szolgáltatás alkalmazás két helyek átmeneti és üzemi megvalósítása. A fő ág beállítva az átmeneti tárolásra szolgáló tárolóhely folyamatos integrációja.
-   Az átmeneti tárolásra szolgáló tárolóhely (a termelési adatokkal) a fő ág egy véglegesítés ellenőrzését követően a program az igazolt átmeneti tárolásra szolgáló alkalmazást be a gyártási tárolóhely [nincs legrövidebb leállás és](web-sites-staged-publishing.md)cserélni.

A gyártási és fejlesztői környezet: a sablon a által meghatározott [ * &lt;repository_root >*/ARMTemplates/ProdandStage.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/ProdAndStage.json).

A fejlesztők és próba környezetekben határozzák meg a sablon a [ * &lt;repository_root >*/ARMTemplates/Dev.json](https://github.com/azure-appservice-samples/ToDoApp/blob/master/ARMTemplates/Dev.json).

A szokásos elágaztatási stratégia használandó helyez át a a fejlesztők ág felfelé a próba ág, majd a fő ág (feljebb helyezi minőség, így az speak) kóddal is.

![](./media/app-service-agile-software-development/what-2-branches.png) 

## <a name="what-you-will-need"></a>Amire szüksége lesz ##

-   Azure-fiók
-   Egy [GitHub](https://github.com/) fiók
-   Mely számjegy rendszerhéj (telepítve a [GitHub Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy a mely számjegy és a PowerShell parancsai futtathatók az adott munkamenetben 
-   Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/0.9.4-June2015/azure-powershell.0.9.4.msi) bittel
-   Egyszerű ismertetése a következőket:
    -   [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) sablon telepítésének (lásd még: [Deploy egy összetett alkalmazás Azure-ban előre](app-service-deploy-complex-application-predictably.md))
    -   [Mely számjegy](http://git-scm.com/documentation)
    -   [A PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Az oktatóprogram elvégzéséhez Azure-fiók van szüksége:
> + [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/) is - credits kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a azokat megszokott után is választhatja, hogy a fiók, és használata ingyenes Azure szolgáltatások, például a Web Apps alkalmazások.
> + [Visual Studio előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/) is – a Visual Studio előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.
>
> Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="set-up-your-production-environment"></a>A gyártási környezet beállítása ##

>[AZURE.NOTE] Az ebben az oktatóanyagban használt parancsfájl automatikusan beállítja a GitHub tárházba folyamatos közzététel. Ehhez az, hogy a GitHub hitelesítő adatok már tárolják Azure-ban, egyéb esetben a parancsfájl telepítés meghiúsul, amikor megpróbál a web Apps alkalmazások adatforrás-vezérlő beállításainak konfigurálása. 
>
>A GitHub hitelesítő adatainak tárolása Azure-ban, az [Azure-portál](https://portal.azure.com/) és - [GitHub telepítésének konfigurálásához](app-service-continuous-deployment.md)az hozzon létre egy web App alkalmazásban. Csak kell egyszer. 

Egy tipikus DevOps esetben egy alkalmazást futtató élő Azure-ban van, és meg szeretné módosítani folyamatos közzétételi keresztül. Ebben az esetben, ha egy sablont, amely fejlesztett, tesztelése és az éles üzemi környezetben üzembe használt. Fog beállítani, ebben a szakaszban.

1.  Hozzon létre saját elágazás, a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárat. A elágazás létrehozásával kapcsolatos további tudnivalókért lásd: [Elágazás egy repó](https://help.github.com/articles/fork-a-repo/). Amikor létrejött a elágazás, megtekintheti a böngészőben.
 
    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Nyisson meg egy mely számjegy rendszerhéj munkamenetet. Ha még nincs mely számjegy rendszerhéj, telepítse a [GitHub a Windows](https://windows.github.com/) most már.

3.  Hozzon létre egy helyi adatfeliratsor a elágazás a hajtja végre a következő parancsot:

        git clone https://github.com/<your_fork>/ToDoApp.git 

4.  Ha befejezte a helyi adatfeliratsor, nyissa meg azt * &lt;repository_root >*\ARMTemplates, és a Futtatás a deploy.ps1 parancsfájl az alábbi képlettel történik:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git

4.  Amikor a rendszer kéri, írja be a kívánt felhasználónév és jelszó-adatbázis eléréséhez.

    Meg kell jelennie a kiépítési végrehajtásának különböző Azure erőforrásokhoz. Telepítés befejezésekor a parancsfájl indítsa el az alkalmazást a böngészőben, és egy felhasználóbarát hangjelzést ad.

    ![](./media/app-service-agile-software-development/production-2-app-in-browser.png)
 
    >[AZURE.TIP] Ajánljuk figyelmébe az * &lt;repository_root >*\ARMTemplates\Deploy.ps1, hogyan hoz létre egyedi azonosítói a források megjelenítéséhez. Ugyanezt a megközelítést is használhatja ugyanazt a telepítési klónok anélkül, hogy nem ütközik-e erőforrásneveket létrehozásához.
 
6.  Vissza a mely számjegy rendszerhéj munkamenetben futtatása:

        .\swap –Name ToDoApp<unique_string>master

    ![](./media/app-service-agile-software-development/production-4-swap.png)

7.  Amikor befejezte a parancsfájlt, visszatérhet tallózással keresse meg a frontend cím (http://ToDoApp*&lt;unique_string >*master.azurewebsites.net/) a gyártási futó alkalmazás megjelenítéséhez.
 
5.  Jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és nézze meg, hogy milyen létrehozott.

    Lásd: a két web Apps alkalmazások azonos erőforrás csoportjában az egyik láthatja a `Api` utótag nevét. Ha megnézi az erőforrás-csoport nézet is látni fogja az SQL-adatbázis és kiszolgáló, az alkalmazás szolgáltatáscsomagja és az átmeneti tárolásra szolgáló helyek a web Apps alkalmazások. Tallózással keresse meg a különböző forrásokból, és hasonlítsa össze őket a * &lt;repository_root >*\ARMTemplates\ProdAndStage.json kattintva megtekintheti, hogy hogyan konfigurálhatók a sablonban.

    ![](./media/app-service-agile-software-development/production-3-resource-group-view.png)

Most már beállított az éles üzemi környezetben. Ezután a egy új frissítés az alkalmazás fog elindításához.

## <a name="create-dev-and-test-branches"></a>A fejlesztői létrehozása és ágak tesztelése ##

Most, hogy van egy gyártási Azure-ban futó összetett alkalmazások, teheti frissítés Agilis módszer szerint az alkalmazás. Ebben a szakaszban a fejlesztők létrehozása, és tesztelése, hogy meg kell a szükséges frissítéseket használja.

1.  Először hozzon létre a próba-környezetet. A mely számjegy rendszerhéj munkamenetben a következő parancsokat hozhat létre egy új ág **NewUpdate**nevű a környezetet. 

        git checkout -b NewUpdate
        git push origin NewUpdate 
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch NewUpdate

1.  Amikor a rendszer kéri, írja be a kívánt felhasználónév és jelszó-adatbázis eléréséhez. 

    Telepítés befejezésekor a parancsfájl indítsa el az alkalmazást a böngészőben, és egy felhasználóbarát hangjelzést ad. És csak például hogy most már rendelkezik egy új elágazás saját tesztkörnyezetben. Szánjon egy kis időt néhány dolog, amit tudni e tesztkörnyezetben áttekintése:

    -   Létrehozhat olyan Azure-előfizetésben. Ez azt jelenti, hogy az éles üzemi környezetben külön-külön a próba-környezetből kezelhető.
    -   A próba-környezet fut élő Azure-ban.
    -   A próba-környezet megegyezik a munkakörnyezetben, kivéve az átmeneti tárolásra szolgáló helyek és a méretezési beállításokat. Ez lehet tudja, mert ezek csak különbségeiről ProdandStage.json Dev.json.
    -   A próba-környezet kezelheti a saját alkalmazás szolgáltatás tervet, egy másik ár szint (például **ingyenes**).
    -   Ebben a tesztkörnyezetben törlése, egyszerűen törlése az erőforráscsoport lesz. Találja meg, hogyan végezze el a [később](#delete).

2.  Nyissa a fejlesztők ág létrehozása a következő parancs futtatásával:

        git checkout -b Dev
        git push origin Dev
        .\deploy.ps1 -TemplateFile .\Dev.json -RepoUrl https://github.com/<your_fork>/ToDoApp.git -Branch Dev

3.  Amikor a rendszer kéri, írja be a kívánt felhasználónév és jelszó-adatbázis eléréséhez. 

    Szánjon egy kis időt tekintse át a következőkkel kapcsolatban a fejlesztői környezet: 

    -   A fejlesztői környezet azonos a tesztkörnyezetben konfiguráció tartalmaz, mert a telepítik használ, ugyanazt a sablont.
    -   A Fejlesztőeszközök saját Azure-előfizetésben, külön-külön kell kezelni a tesztkörnyezetben elhagyása minden fejlesztői környezet hozhat létre.
    -   A fejlesztői környezet fut élő Azure-ban.
    -   A fejlesztői környezet törlése nem csupán az erőforráscsoport törlése. A [későbbi](#delete)való találja.

>[AZURE.NOTE] Ha több fejlesztők az új frissítést dolgozik, mindegyiket egyszerűen készíthet egy ág és a dedikált fejlesztői környezet az alábbiak szerint:
>
>1. Hozzon létre saját elágazás a tárat a GitHub (lásd: [a repó elágazás](https://help.github.com/articles/fork-a-repo/)).
>2. A helyi számítógépen a elágazás másolása
>3. A saját fejlesztők ág és környezet létrehozása azonos parancsai futtathatók.

Amikor elkészült, a GitHub elágazás kell három használja:

![](./media/app-service-agile-software-development/test-1-github-view.png)

És hat web Apps alkalmazások (a két három készletek) rendelkeznie kell a három külön az erőforrás-csoportok:

![](./media/app-service-agile-software-development/test-2-all-webapps.png)
 
>[AZURE.NOTE] Figyelje meg, hogy ProdandStage.json Itt adhatja meg a termelési környezetén használni a **szabványos** réteg, amely megfelel a gyártási alkalmazásának méretezhetőség árak.

## <a name="build-and-test-every-commit"></a>Szerkesztés és tesztelje a minden jóváhagyás ##

A sablonfájlokat ProdAndStage.json és Dev.json már paramétert a forrás vezérlő, amely alapértelmezés szerint beállítja folyamatos közzététele a web app. Ennélfogva minden GitHub ág véglegesítés eseményindítók egy automatikus telepítési az Azure az adott ág. Lássuk, hogyan működik a telepítés gombra.

1.  Győződjön meg arról, hogy Ön a fejlesztők részlege a helyi tárházba. Ehhez a mely számjegy felületén futtassa a következő parancsot:

        git checkout Dev

2.  Módosítás az egyszerű az alkalmazás felhasználói felület réteghez: módosítsa a kódot az [betöltő](http://getbootstrap.com/components/) listák használata. Nyissa meg * &lt;repository_root >*\src\MultiChannelToDo.Web\index.cshtml és a módosítások kiemelt az alábbi:

    ![](./media/app-service-agile-software-development/commit-1-changes.png)

    >[AZURE.NOTE] Ha a fenti képen nem tudja olvasni: 
    >
    >- A 18 sorban módosítása `check-list` való `list-group`.
    >- A vonal 19, módosíthatja `class="check-list-item"` való `class="list-group-item"`.

3.  A módosítás mentéséhez. Vissza a mely számjegy felületén a következő parancsokat:

        cd <repository_root>
        git add .
        git commit -m "changed to bootstrap style"
        git push origin Dev
 
    Ezek a parancsok mely számjegy hasonlóak "beadása a kód" TFS például egy másik forrás vezérlő rendszerben. Futtatásakor `git push`, az új jóváhagyás eseményindítók-kód automatikus leküldéses az Azure, amelyek ezután ismét létrehozza az alkalmazás a fejlesztői környezet változásának megfelelően.

4.  Ha ellenőrizni szeretné, hogy a kód leküldéses a fejlesztői környezet történt, nyissa meg a fejlesztői környezet web app lap, és tekintse meg a **telepítési** rész. Látnia kell a vannak a legújabb jóváhagyás üzenet jelenik meg.

    ![](./media/app-service-agile-software-development/commit-2-deployed.png)

5.  Itt Azure-ban, az élő alkalmazásban új állapotúként jelenik meg a **Tallózás** gombra.

    ![](./media/app-service-agile-software-development/commit-3-webapp-in-browser.png)

    Ez az alkalmazás igazán kisebb módosításának. Azonban van számos összetett webalkalmazás egyesíthető változtatás időpontok várt és nemkívánatos hatásai. Nem tesztelheti egyszerűen minden jóváhagyás élő buildjeiben lehetővé teszi, hogy elfog ezeket a problémákat, mielőtt az ügyfelekkel megtekintheti őket.

Most, amelyet az, hogy a **NewUpdate** projekten fejlesztők fogja tudni egyszerűen hozzon létre egy fejlesztői környezet beállítása önmaga számára, majd minden jóváhagyás összeállítása és tesztelése minden build megvalósítási gondot kell lennie.

## <a name="merge-code-into-test-environment"></a>Kód egyesíteni tesztkörnyezetben ##

Amikor készen áll a kód leküldéses a fejlesztők ág NewUpdate ág felfelé, a normál mely számjegy folyamat:

1.  A fejlesztők ág GitHub, például véglegesítése más fejlesztők által létrehozott minden új véglegesítése, a NewUpdate egyesíteni. Minden új jóváhagyás GitHub a kód leküldéses elindítása, és a a fejlesztői környezet összeállítása. Ezután teheti meg arról, hogy a fejlesztők ág a kód továbbra is működik a NewUpdate ág legfrissebb bit.

2.  Az új kiírása a fejlesztők ág egyesítheti a GitHub NewUpdate ág. Ez a művelet a kód leküldéses és a tesztkörnyezetben build elindítja. 

Megjegyzés: ismét, hogy folyamatos telepítési már be állítva az alábbi mely számjegy ágak, mert nem kell tennie más semmit, például az integration hoz létre. Egyszerűen kell elvégeznie, mely számjegy segítségével szabványos forrás vezérlő eljárások, és Azure végez build folyamatok meg.

Most vegyük leküldéses a kód **NewUpdate** ág szeretne. A mely számjegy felületén a következő parancsokat:

    git checkout NewUpdate
    git pull origin NewUpdate
    git merge Dev
    git push origin NewUpdate

Az egész! 

Nyissa meg a web app lap az új jóváhagyás (NewUpdate ág egyesítve) most tolni a tesztkörnyezetben megtekintéséhez próba-környezetben. Kattintson a **Tallózás gombra** kattintva megtekintheti, hogy a stílus módosítása most már fut élő Azure-ban.

## <a name="deploy-update-to-production"></a>Frissítés ügyfélszámítógépekre előállítása ##

Kód terjesztése a átmeneti és üzemi környezetben kell úgy érzi nem ugyanaz, mint a már már elvégzett esetén, a tesztkörnyezetben tolódik kódot. Ez nagyon egyszerű. 

A mely számjegy felületén a következő parancsokat:

    git checkout master
    git pull origin master
    git merge NewUpdate
    git push origin master

Ne feledje, hogy a átmeneti és üzemi környezetben, amelyet a telepítő a ProdandStage.json módjának alapján, az új kódot a **átmeneti** tárolóhely tolódik, és hogy fut-e. Nyissa meg az átmeneti tárolásra szolgáló tárolóhely URL-címét, ha látni fogja az új kódot futó van. Ehhez futtassa a `Show-AzureWebsite` mely számjegy rendszerhéj parancsmag.

    Show-AzureWebsite -Name ToDoApp<unique_string>master -Slot Staging
 
És most az átmeneti tárolásra szolgáló tárolóhely frissítés ellenőrzése után beállítást a hátralévő megjelenítését a éles. A mely számjegy felületén csak a következő parancsokat:

    cd <repository_root>\ARMTemplates
    .\swap.ps1 -Name ToDoApp<unique_string>master

Gratulálok! A gyártási webalkalmazás sikeresen már tett közzé egy új frissítés. Mi a teendő ennél, hogy már csak akkor által elvégzett egyszerűen a fejlesztők létrehozása, és tesztelje a környezetben, és a létrehozásához, és minden jóváhagyás teszteléséhez. Ezek az Agilis szoftverfejlesztési döntő építőelemek.

<a name="delete"></a>
## <a name="delete-dev-and-test-enviroments"></a>Törölje a fejlesztők és enviroments tesztelése ##

A fejlesztők és próba környezetekben önálló erőforráscsoport kell tervezett konfigurálták, van, mert az nagyon egyszerűen törölje őket. Törölheti azokat létrehozott ebben az oktatóprogramban az GitHub ágak és az Azure eltéréseket, csak a következő parancsokat a mely számjegy felületén:

    git branch -d Dev
    git push origin :Dev
    git branch -d NewUpdate
    git push origin :NewUpdate
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>dev-group -Force -Verbose
    Remove-AzureRmResourceGroup -Name ToDoApp<unique_string>newupdate-group -Force -Verbose

## <a name="summary"></a>Összefoglalás ##

Agilis szoftverfejlesztési sok olyan vállalatok, akiket szeretne elfogadni, az alkalmazás platform Azure egy kell – már. Ebben az oktatóanyagban hogyan hozhat létre és pontos kópiák lefelé vagy annak közelében az éles üzemi környezetben replikái tear könnyen, akár az összetett alkalmazások megtanulta azt van. Emellett megtanulta azt hogyan használhatja a Ez azt jelenti, hogy hozzon létre, amelyek képesek összeállítása és minden egyetlen jóváhagyás tesztelje az Azure fejlesztési folyamat. Ebben az oktatóanyagban azt remélhetőleg mutatják, akkor legjobb használatát Azure alkalmazás szolgáltatás és Azure erőforrás-kezelő együtt az Agilis módszertan caters DevOps megoldás létrehozása. Ezután készíthet ebben az esetben a speciális DevOps módszerrel, például [a termelési vizsgálat](app-service-web-test-in-production-get-start.md)hajt végre. Gyakori tesztelése gyártási példa [Flighting telepítési (bétatesztelés) Azure App szolgáltatásban](app-service-web-test-in-production-controlled-test-flight.md)című témakör tartalmaz.

## <a name="more-resources"></a>További források ##

-   [Azure-ban előre egy összetett alkalmazások telepítése](app-service-deploy-complex-application-predictably.md)
-   [A gyakorlatban Agilis fejlesztési: tippek és trükkök korszerűsített fejlesztési ciklus](http://channel9.msdn.com/Events/Ignite/2015/BRK3707)
-   [A speciális telepítési stratégiák Azure Web Apps alkalmazások erőforrás-kezelő sablonok használata](http://channel9.msdn.com/Events/Build/2015/2-620)
-   [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md)
-   [JSONLint – a JSON-érvényesítő](http://jsonlint.com/)
-   [ARMClient – GitHub közzétételi webhelyet beállítása](https://github.com/projectKudu/ARMClient/wiki/Setup-GitHub-publishing-to-Site)
-   [Mely számjegy elágaztatási – egyszerű elágaztatási és egyesítése](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Belinszki Ebbo Blog](http://blog.davidebbo.com/)
-   [Azure PowerShell](../powershell-install-configure.md)
-   [Azure-platformok parancssori eszközök](../xplat-cli-install.md)
-   [Felhasználók létrehozása és szerkesztése az Azure Active Directory](https://msdn.microsoft.com/library/azure/hh967632.aspx#BKMK_1)
-   [Projekt Kudu Wiki](https://github.com/projectkudu/kudu/wiki)
