<properties
    pageTitle="Azure alkalmazás szolgáltatás folyamatos telepítési |} Microsoft Azure"
    description="Megtudhatja, hogy miként folyamatos telepítési Azure alkalmazás szolgáltatás engedélyezéséhez."
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
    ms.date="10/28/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="continuous-deployment-to-azure-app-service"></a>Folytonos telepítési Azure alkalmazás szolgáltatás

Ebből az oktatóanyagból megtudhatja, hogy miként konfigurálható az [Azure alkalmazás szolgáltatás] számára folyamatos telepítési munkafolyamat. BitBucket GitHub és a Visual Studio csapat szolgáltatások (VSTS) integrációja alkalmazás szolgáltatás lehetővé teszi, hogy hol Azure gyűjti össze a projekt közzétett az alábbi szolgáltatások közül a legújabb frissítéseket a folyamatos telepítési munkafolyamat. Folytonos telepítési nagyszerű lehetőség projektekhez ahol több és gyakori adományok integrált.

## <a name="overview"></a>Folytonos telepítési engedélyezése

Ahhoz, hogy a folyamatos példányban 

1. Az alkalmazás tartalom közzététele a tárházba folyamatos telepítéshez használt.  
    Az alábbi szolgáltatások a projekt közzététele a további tudnivalókért lásd: [létrehozása egy repó (GitHub)], [a repó (BitBucket) létrehozása]és [VSTS – első lépések].

2. Kattintson az [Azure portal]menü fel az alkalmazást, **alkalmazások TELEPÍTÉSÉNEK > telepítési lehetőségek**. **Válassza a forrás**gombra, majd válassza ki a telepítési forrást.  

    ![](./media/app-service-continuous-deployment/cd_options.png)
    
    > [AZURE.NOTE] Állítsa be a VSTS alkalmazás szolgáltatás telepítési-fiók a következő témakörben ebben az [oktatóanyagban](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
    
3. Töltse ki a engedélyezési munkafolyamatot. 

4. A **telepítési forrást** a lap válassza ki a project és a beállításra, az üzembe. Amikor elkészült, kattintson az **OK gombra**.
  
    ![](./media/app-service-continuous-deployment/github_option.png)

    > [AZURE.NOTE] Folytonos telepítési GitHub vagy BitBucket engedélyezésekor nyilvános és magán projektek jelennek meg.

    Alkalmazás szolgáltatás a a kijelölt tárházba társítást hoz létre, gyűjti össze a fájljait a megadott ág és megőrzi az alkalmazás szolgáltatás számára a tárházba a átirattal. VSTS folyamatos telepítési az Azure portálról konfigurálásakor integrációja használja az alkalmazás szolgáltatás [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki), amellyel már automatizálja felépítési és a telepítési feladatok minden `git push`. Nem kell VSTS folyamatos bevezetésének külön-külön beállítása. Ez a folyamat befejezése után a **telepítési lehetőségek** alkalmazás lap jelenik meg, az aktív telepítési telepítési jelző sikeresen befejeződött.

5. Annak ellenőrzéséhez, hogy az alkalmazás telepítése sikerült, kattintson az **URL-CÍMÉT** az Azure-portálon az alkalmazást a lap tetején. 

6. Ha ellenőrizni szeretné, hogy a folyamatos telepítési választási tárházából mi, a tárházba leküldéses módosítás. Az alkalmazás frissíteni kell a változások megjelennek a leküldéses a tárházba befejezése után hamarosan. Ellenőrizheti, hogy rendelkezik-e húzni a frissítést, a **telepítési beállítások** lap, az alkalmazás a.

## <a name="VSsolution"></a>Folytonos megoldás üzembe helyezése a Visual Studio 

A Visual Studio megoldás terjesztése Azure alkalmazás szolgáltatás csak ugyanolyan egyszerű, mint egy egyszerű index.html fájl terjesztése. Az alkalmazás szolgáltatás telepítési folyamatot az összes az adatait, beleértve a helyreállítása NuGet függőségek, és az alkalmazás bináris épület egyszerűsítik. Kövesse a forrás vezérlő gyakorlati tanácsokat kód csak az a mely számjegy tárházba tartására, és lehetővé teszik gondoskodik a többi alkalmazást szolgáltatás telepítési.

A lépéseket a Visual Studio megoldás terjesztése alkalmazás szolgáltatás ugyanazok, mint az [előző szakaszban](#overview), feltéve, hogy a megoldás és beállítása tárházba az alábbi képlettel történik:

-   A Visual Studio adatforrás-vezérlő beállítást használatával hozza létre a `.gitignore` fájlban, például a kép alulra vagy manuális felvétele egy `.gitignore` a tárházba legfelső szintű ezt a [minta .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore)hasonló tartalommal fájlt. 

    ![](./media/app-service-continuous-deployment/VS_source_control.png)
 
-   A teljes megoldást directory fában hozzáadása a tárházba a tárházba legfelső szintű .sln-fájl segítségével.

Után állítsa be a tárházba leírt módon van, és be van állítva az alkalmazás Azure-ban az online tárhelyek mely számjegy a folyamatos közzététel, a helyi meghajtóra a Visual Studióban ASP.NET-alkalmazások fejlesztése, és folyamatosan üzembe a kód egyszerűen közvetítheti a módosításokat a online mely számjegy tárházba.

## <a name="disableCD"></a>Folytonos telepítési letiltása

Folytonos példányban letiltása 

1. Kattintson az [Azure portal]menü fel az alkalmazást, **alkalmazások TELEPÍTÉSÉNEK > telepítési lehetőségek**. Ezután kattintson a **telepítés beállításai** lap **kapcsolat bontása** .

    ![](./media/app-service-continuous-deployment/cd_disconnect.png)    

2. A megerősítést kérő üzenet megválaszolása az **Igen** , után térjen vissza a lap az alkalmazás és kattintson a **alkalmazások TELEPÍTÉSÉNEK > telepítési lehetőségek** Ha szeretné, hogy egy másik olyan forrásból történő közzététel beállítása.

## <a name="additional-resources"></a>További források

* [Hogyan folyamatos telepítési közös problémáinak vizsgálata](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Az Azure PowerShell használata]
* [Az Azure parancssori eszközök használatáról a Mac- és Linux]
* [Mely számjegy dokumentáció]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

[Azure alkalmazás szolgáltatás]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/ 
[Azure portál]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Az Azure PowerShell használata]: ../articles/powershell-install-configure.md
[Az Azure parancssori eszközök használatáról a Mac- és Linux]: ../articles/xplat-cli-install.md
[Mely számjegy dokumentáció]: http://git-scm.com/documentation

[Hozzon létre egy repó (GitHub)]: https://help.github.com/articles/create-a-repo
[Hozzon létre egy repó (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Első lépések a VSTS]: https://www.visualstudio.com/get-started/overview-of-get-started-tasks-vs
[Continuous delivery to Azure using Visual Studio Team Services]: ../articles/cloud-services/cloud-services-continuous-delivery-use-vso.md
