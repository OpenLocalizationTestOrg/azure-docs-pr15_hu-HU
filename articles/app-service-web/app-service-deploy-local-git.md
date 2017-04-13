<properties
    pageTitle="Helyi mely számjegy telepítési Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogyan engedélyezhető a helyi mely számjegy telepítési Azure alkalmazás szolgáltatás."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="local-git-deployment-to-azure-app-service"></a>Helyi mely számjegy telepítési Azure alkalmazás szolgáltatás

Ebből az oktatóanyagból megtudhatja, hogy hogyan [Azure alkalmazás] szolgáltatás, a helyi számítógépen mely számjegy összegyűjti telepítse az alkalmazást. Alkalmazás szolgáltatás támogatja a ezt a megközelítést a **Helyi mely számjegy** telepítési lehetőség az [Azure-portálon].  
A jelen cikkben ismertetett mely számjegy parancsok számos automatikusan megtörténik, amikor az alkalmazás szolgáltatás-alkalmazások használata az [Azure parancssor] leírt [Itt](app-service-web-get-started.md)létrehozását.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez szükséges:

- Mely számjegy. A telepítés bináris [Itt](http://www.git-scm.com/downloads)letöltheti.  
- Mely számjegy alapismeretei.
- A Microsoft Azure-fiókjába. Ha nem rendelkeznek fiókkal, akkor [Jelentkezzen be az ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial) , vagy [a Visual Studio előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter alkalmazásban az alkalmazás szolgáltatás. Nem kötelező, hitelkártyák Nincs nyilatkozatát.  

## <a name="Step1"></a>Lépés: 1: Hozzon létre egy helyi tárházba

A következő feladatokat hozzon létre egy új mely számjegy tárban tárolnak.

1. Indítsa el a parancssori eszközt, például **GitBash** (Windows) vagy a **Bash** (Unix rendszerhéj). OS X rendszeren érheti el a parancssori a **terminált** alkalmazáson keresztül.

2. Nyissa meg a könyvtár, ahol telepíteni szeretné választani a tartalmat lesz található.

3. A következő parancs segítségével egy új mely számjegy tárházba inicializálni:

        git init

## <a name="Step2"></a>Lépés: 2: Véglegesítse a tartalom

Alkalmazás szolgáltatás lehetővé teszi a készült különböző programozási nyelven alkalmazások. 

1. Ha a tárházba már van ugyanerre tartalom kihagyása ez pont, és helyezze a mutasson az alábbi 2. Ha a tárházba már nem tartalmaz tartalom egyszerűen adataival statikus .html fájl az alábbi képlettel történik: 

    - Egy szövegszerkesztővel **index.html** a legfelső szintű a mely számjegy tárházba nevű új fájl létrehozása
    - Adja meg a következő szöveget a index.html tartalmának fájlba mentse: *helló mely számjegy!*
        
2. A parancssorból ellenőrizze, hogy a mely számjegy tárházba legfelső szintű csoportban. A következő parancs segítségével fájlok hozzáadása a tárházba:

        git add -A 

4. Ezután hajtsa végre a módosításokat a tárházba a következő parancs használatával:

        git commit -m "Hello Azure App Service"

## <a name="Step3"></a>3 lépés: Az alkalmazás szolgáltatás alkalmazás tárházba engedélyezése

A következő lépésekkel ahhoz, hogy mely számjegy összegyűjti az alkalmazás szolgáltatás számára.

1. Jelentkezzen be az [Azure-portálon].

2. Kattintson a lap az alkalmazás szolgáltatás alkalmazás **beállításai > telepítési forrás**. Kattintson a **Válassza ki a forrást**, majd kattintson a **Helyi mely számjegy tárházba**, és kattintson **az OK**gombra.  

    ![Helyi mely számjegy tárházba](./media/app-service-deploy-local-git/local_git_selection.png)

3. Ha egy tárházba Azure-ban a első alkalommal beállítandó, a bejelentkezési adatok létrehozása, szüksége. Jelentkezzen be az Azure tárházba és leküldéses módosításokat, a helyi mely számjegy tárházba való őket nyomtatáskor. Az alkalmazás a lap, kattintson a **beállításai > telepítési hitelesítő adatok**, majd állítsa be a telepítési felhasználónevével és jelszavával. Amikor elkészült, kattintson a **Mentés**gombra.

    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a name="Step4"></a>Lépés: 4: A projekt telepítése

Az alkalmazás közzététele helyi mely számjegy alkalmazás szolgáltatás kövesse az alábbi lépéseket.

1. Az alkalmazás fel az Azure-portált, kattintson **beállításai > Tulajdonságok** az **Mely számjegy URL-CÍMÉT**.

    ![](./media/app-service-deploy-local-git/git_url.png)

    **Mely számjegy URL-CÍMÉT** az a távoli hivatkozás, a helyi tárházba a telepítéshez használni. Ez az URL-CÍMÉT az alábbi lépéseket kell megadnia.

2. Használja a parancssori, győződjön meg arról, hogy a helyi mely számjegy tárházba legfelső szintű vannak-e.

3. Használat `git remote` hozzáadása a távoli hivatkozás szerepel **Mely számjegy URL-CÍMÉT** a lépés: 1. A parancs jelenik meg, az alábbihoz hasonló:

        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] A **távoli** parancs hozzáadása egy távoli tárházba elnevezett hivatkozást. Ebben a példában a web app tárházba "azure" nevű hivatkozást hoz létre.

4. A tartalom leküldéses alkalmazás szolgáltatás használata az új **azure** távoli éppen létrehozott.

        git push azure master

    A korábban létrehozott, a telepítési hitelesítő adatait az Azure-portálon alaphelyzetbe a jelszó kéri. Írja be a jelszót (figyelje meg, hogy Gitbash nem jeleníti meg a konzolhoz csillagok a jelszó beírásakor). 
       
5. Térjen vissza az alkalmazást az Azure-portálon. A **telepítés** lap, a legutóbbi leküldéses naplóbejegyzést jelenik meg. 

    ![](./media/app-service-deploy-local-git/deployment_history.png)

6. Kattintson a **Tallózás** gombra kattintva ellenőrizze, hogy a tartalom lett telepítve az alkalmazást a lap tetején. 
    
## <a name="Step5"></a>Hibaelhárítás

A hibák vagy problémáival gyakran mely számjegy közzététele Azure-szolgáltatási alkalmazás alkalmazás használatakor a következők:

****

**Probléma**: nem lehet access [siteURL]: nem sikerült csatlakozni [scmAddress]

**OK**: Ez a hiba akkor fordulhat elő, ha az alkalmazás nem lépéseket.

**Megoldás**: Indítsa el az alkalmazást az Azure-portálon. Mely számjegy telepítési hacsak nem működik, és az alkalmazás nem működik. 


****

**Probléma**: nem sikerült megoldani a host "hostname (állomásnév)"

**OK**: Ez a hiba akkor fordulhat elő, ha a megadott létrehozásakor a ": azure távoli címadatok helytelen volt.

**Megoldás**: használja a `git remote -v` parancs lista az összes távirányítók, és a kapcsolódó URL-címe. Győződjön meg arról, hogy helyesen-e a "azure" távoli URL-CÍMÉT. Ha szükséges, távolítsa el, és hozza létre a távoli URL-cím helyes használatáról.

****

**Probléma**: nem refs közös, és nincs megadva; semmi sem módon. Esetleg adjon meg egy ág, például "mesteralakzat".

**OK**: Ez a hiba akkor fordulhat elő, ha nem adja meg a fiók leküldéses művelet végrehajtása egy mely számjegy, és nem állította be a mely számjegy által használt push.default értéket.

**Megoldás**: a műveletet leküldéses, adja meg a fő ág. Példa:

    git push azure master

****

**Hibajelenség**: [branchname] src refspec sem felel meg.

**OK**: Ez a hiba akkor fordulhat elő, ha a tartalomtípusban szeretné egy minta eltérő ág "azure" a távoli.

**Megoldás**: a műveletet leküldéses, adja meg a fő ág. Példa:

    git push azure master

****

**A jelenség**: hiba - távoli tárházba változtatások, de a web App alkalmazásban nem frissülnek.

**OK**: Ez a hiba akkor fordulhat elő, a további szükséges modulokat megadó package.json fájlt tartalmazó Node.js alkalmazás telepítése.

**Megoldás**: további üzeneteket keres, amelyek a "hiba npm!" Ezt a hibát naplózott előtt kell lennie, és a hiba esetén további információkat tartalmaznak. Az alábbiakban néhány ismert ezt a hibát, és a megfelelő "npm hiba!" a okai üzenet:

* **Hibás package.json fájl**: npm hiba! Függőségek nem olvasható.

* **Natív modul, amely nincs a Windows bináris eloszlás**:

    * Hiba npm! \`cmd "/ c" "csomópont-gyp újraépítő"\` sikertelen 1

        VAGY

    * Hiba npm! [modulename@version]Telepítse elő: \`ellenőrizze. gmake\`


## <a name="additional-resources"></a>További források

* [Mely számjegy dokumentáció](http://git-scm.com/documentation)
* [Project Kudu dokumentációja](https://github.com/projectkudu/kudu/wiki)
* [Folyamatos telepítési Azure alkalmazás szolgáltatás](app-service-continuous-deployment.md)
* [Az Azure PowerShell használata](../powershell-install-configure.md)
* [Azure parancssori felületének használata](../xplat-cli-install.md)

[Azure alkalmazás szolgáltatás]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://www.windowsazure.com/en-us/develop/overview/
[Azure portál]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure parancssor]: https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart
