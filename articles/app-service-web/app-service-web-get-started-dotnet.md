<properties 
    pageTitle="Az első .NET-webalkalmazást beállítaná Azure öt perc |} Microsoft Azure" 
    description="Megtudhatja, hogy milyen könnyen web Apps alkalmazások futtatásához alkalmazás szolgáltatás üzembe helyezése a minta-at. Indítsa el a valódi fejlesztéséhez gyorsan módon, és azonnal eredmények megtekintéséhez." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-net-web-app-to-azure-in-five-minutes"></a>Az első .NET-webalkalmazást beállítaná Azure öt perc

Ebben az oktatóanyagban segít egyszerű .NET webes alkalmazások terjesztése [Azure alkalmazás](../app-service/app-service-value-prop-what-is.md)szolgáltatásba.
Alkalmazás szolgáltatással web Apps alkalmazások, [mobilalkalmazás biztonsági végű](/documentation/learning-paths/appservice-mobileapps/)és az [API-alkalmazások](../app-service-api/app-service-api-apps-why-best-platform.md)létrehozásához.

Teendők: 

- Azure alkalmazás szolgáltatás hozzon létre webalkalmazást.
- ASP.NET példakódot üzembe.
- Lásd: a kód futó élő gyártási.
- Frissítse a web app [mely számjegy véglegesíti leküldéses](https://git-scm.com/docs/git-push)ugyanúgy.

## <a name="prerequisites"></a>Előfeltételek

- [Mely számjegy](http://www.git-scm.com/downloads).
- [Azure CLI](../xplat-cli-install.md).
- A Microsoft Azure-fiókjába. Ha nem rendelkeznek fiókkal, akkor [Jelentkezzen be az ingyenes próbaverzióra](/pricing/free-trial/?WT.mc_id=A261C142F) , vagy [a Visual Studio előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751) Azure-fiók nélkül is. Alapszintű alkalmazás létrehozása, és nem szükséges hitelkártya, nincs kötelezettségek akár egy óra – vele az lejátszása.

## <a name="deploy-an-net-web-app"></a>.NET webes alkalmazás telepítése

1. Nyissa meg egy új Windows parancssor, PowerShell ablakában, Linux rendszerhéj vagy OS X Terminálszolgáltatások. Futtatása `git --version` és `azure --version` ellenőrizze, hogy mely számjegy és Azure CLI telepítve van a számítógépre.

    ![Az első webalkalmazás CLI eszközök-telepítés tesztelése Azure-ban](./media/app-service-web-get-started/1-test-tools.png)

    Ha még nem telepítette az eszközök, olvassa el a [vonatkozó követelmények](#Prerequisites) letöltési hivatkozások.

3. Jelentkezzen be az Azure jelennek meg:

        azure login

    Kövesse a Súgó üzenet, folytassa a bejelentkezési folyamatot.

    ![Jelentkezzen be az első webalkalmazás létrehozása az Azure](./media/app-service-web-get-started/3-azure-login.png)

4. Azure CLI módosítása ASM módba, majd állítsa a központi felhasználói alkalmazás szolgáltatás. Telepíti a kód később a hitelesítő adataival.

        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

1. Munkakönyvtárat módosítása (`CD`) és a minta alkalmazás klónozhatja.

        git clone https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git

2. Állítsa be a minta alkalmazás tárházából. 

        cd app-service-web-dotnet-get-started

4. Hozzon létre az alkalmazás szolgáltatás alkalmazás erőforrás Azure-ban egy egyedi alkalmazás nevére, és a központi felhasználói, a korábban beállított. Amikor a rendszer kéri, adja meg a kívánt terület számát.

        azure site create <app_name> --git --gitusername <username>

    ![Az Azure erőforrás az első webalkalmazás létrehozása az Azure-ban](./media/app-service-web-get-started-languages/dotnet-site-create.png)

    Az alkalmazás letöltése létrehozott Azure-ban. Az aktuális könyvtár is mely számjegy inicializált és az új alkalmazás szolgáltatás alkalmazás csatlakozik egy távoli mely számjegy szerint.
    Az alkalmazás URL-címre tallózhat (http://&lt;alkalmazásnév >. azurewebsites.net) gyönyörű alapértelmezett HTML-lap, de vegyük tudjanak lépni a kód most már van.

4. A minta kód üzembe az Azure-alkalmazást, mely számjegy bármilyen kódot szeretne leküldéses, mint. Amikor a rendszer kéri, a korábban beállított jelszót használja.

        git push azure master

    ![Leküldéses kódot az első webalkalmazást Azure-ban](./media/app-service-web-get-started-languages/dotnet-git-push.png)

    `git push`nemcsak az Azure-kód helyezi, de is visszaállítja a szükséges csomagokat, és hozza létre az ASP.NET bináris. 

Gratulálunk, Azure alkalmazás szolgáltatás telepítette az alkalmazást.

## <a name="see-your-app-running-live"></a>Lásd: az alkalmazás élő fut

Ha látni szeretné az alkalmazást futtató élő Azure-ban, parancsot a bármely címtárból a tárban tárolnak:

    azure site browse

## <a name="make-updates-to-your-app"></a>Végezze el az alkalmazás frissítések

Mely számjegy segítségével most, hogy a frissítés az élő webhelyen a project (tárházba) legfelső szintű bármikor leküldéses. Megteheti azt ugyanúgy, mint amikor első alkalommal a kód telepítette. Ha például minden olyan alkalommal, amikor azt szeretné, hogy tesztelt új módosítás helyben leküldéses, csak a következő parancsokat a project (tárházba) legfelső szintű:

    git add .
    git commit -m "<your_message>"
    git push azure master


## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan hozhat létre, kialakítása és Azure közvetlenül a [Deploy Azure alkalmazás szolgáltatás Visual Studio-ASP.NET web App alkalmazásban](web-sites-dotnet-get-started.md)a Visual Studio .NET web Apps alkalmazások telepítéséhez.

Másik lehetőségként további műveletek az első webalkalmazást. Példa:

- Próbálja ki az [egyéb módjai az Azure kód telepítése](../app-service-web/web-sites-deploy.md). Például szeretné üzembe helyezni a GitHub tárházakban közül, egyszerűen jelölje ki **GitHub** **Helyi mely számjegy tárházba** helyett a **telepítési lehetőségek**.
- A következő szintre, hogy az Azure-alkalmazást. A felhasználók hitelesítő. A méretezés igény alapján. Bizonyos teljesítmény értesítések beállítása. Mindezt néhány kattintással. Lásd: [az első webalkalmazást hozzáadása funkciókat](app-service-web-get-started-2.md).

