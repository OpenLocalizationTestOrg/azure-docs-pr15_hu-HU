<properties
    pageTitle="Folytonos kézbesítési mely számjegy és a Visual Studio Team Services Azure-ban |} Microsoft Azure"
    description="További információ a Visual Studio Team Services csapata projektek mely számjegy használatával automatikusan össze, és telepítse az Azure alkalmazás szolgáltatás vagy felhőalapú szolgáltatások Web App szolgáltatásának konfigurálása."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>A Visual Studio Team Services és mely számjegy Azure folyamatos kézbesítési

Visual Studio Team Services csapata projektek mely számjegy összegyűjti állomásnév számára a forráskód, és automatikusan összeállítása és Azure web Apps alkalmazások terjesztése vagy cloud services, amikor a tárházba leküldéses, a jóváhagyás is használhatja.

Visual Studio 2013 és az Azure SDK telepítve kell. Ha még nem rendelkezik a Visual Studio 2013, töltse le az **első lépések az ingyenes** hivatkozásra [www.visualstudio.com](http://www.visualstudio.com)kiválasztásával. Telepítse az Azure SDK [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Az oktatóprogram elvégzéséhez Visual Studio Team Services-fiókra van szüksége: [Nyissa meg az ingyenes egy Visual Studio Team Services fiókot](http://go.microsoft.com/fwlink/p/?LinkId=512979)is.

Egy felhőalapú szolgáltatásba, automatikusan össze, és telepítse az Azure, a Visual Studio Team Services használatával történő beállításához kövesse az alábbi lépéseket.

## <a name="1-create-a-git-repository"></a>1: mely számjegy tár létrehozása

1. Ha még nincs a Visual Studio Team Services-fiókot, kattint egy [ide](http://go.microsoft.com/fwlink/?LinkId=397665). A csoportwebhely projektet hoz létre, ha az adatforrás-vezérlő rendszer válassza a mely számjegy. Kövesse az utasításokat követve csatlakoztassa a Visual Studio a csapat projekthez.

2. A **Csoportwebhely Explorer**a **klónozhatja a tárházba** hivatkozás kiválasztása

    ![][3]

3. Adja meg a helyi példányban helyét, és válassza az **Adatfeliratsor** gombra.

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: projekt létrehozása, és hajtsa végre, akkor a tárházba

1. **Csapat Explorer** **megoldások** csoportban válassza az **Új** hivatkozást hozhat létre új projektet a helyi adattárban.

    ![][4]

2. Az Útmutató lépéseit követve telepítheti a webalkalmazást vagy egy felhőalapú szolgáltatásba (Azure-alkalmazás). Hozzon létre új Azure Felhőszolgáltatásba projektekbe, vagy új ASP.NET MVC projekt. Győződjön meg arról, hogy a projekt célként a .NET-keretrendszer 4-es vagy újabb verziója. Ha egy felhőalapú szolgáltatás projektet szeretne létrehozni, adja hozzá a ASP.NET MVC webes szerepet és a dolgozó szerepkörbe.
Ha szeretne egy webalkalmazás létrehozása, **ASP.NET webalkalmazás** project-sablon kiválasztása, és válassza a **MVC**. További információt talál [az Azure-App szolgáltatásban ASP.NET-webes alkalmazás létrehozása](../app-service-web/web-sites-dotnet-get-started.md) .

3. Nyissa meg a helyi menü a megoldás, és válassza a **véglegesítése**.

    ![][7]

4. Ha az első alkalommal a Visual Studio Team Services mely számjegy használtuk, kell néhány információval mutatkozzon be mely számjegy. **Csoportwebhely Explorer** **Függőben lévő változásokat** területen adja meg a felhasználónevét és e-mail címét. Írja be a Megjegyzés a jóváhagyás, és kattintson a **Jóváhagyás** gombra.

    ![][8]

5. Megjegyzés: a belefoglalásához vagy kizárásához bizonyos módosítások, ha beadja a beállításokat. Ha a kívánt módosításokat tartoznak, válassza az **Összes olyan**.

6. Most már elkövetett a módosításokat a helyi példányt a tárat. Ezután a **szinkronizálási** hivatkozást választva szinkronizálja a módosításokat a kiszolgálóval.

## <a name="3-connect-the-project-to-azure"></a>3: Csatlakozás a project Azure

1. Most, hogy az egyes forráskód, akkor a Visual Studio Team Services mely számjegy tárházba, készen áll a mely számjegy tárházba csatlakozhat az Azure.  Válassza az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885), az a felhő szolgáltatás vagy a web app, vagy hozzon létre egy újat kiválasztása a + ikont a bal alsó sarokban, majd a **Felhőbeli szolgáltatástól** vagy **Web App** , majd **Gyors létrehozásához**.

    ![][9]

3. Felhőszolgáltatások kattintson a **Közzététel a Visual Studio Team Services állíthatja be** hivatkozására. Web Apps alkalmazások válassza az **adatforrás-vezérlő telepítési beállítása** hivatkozásra.

    ![][10]

2. A varázslóban írja be a Visual Studio Team Services fiók nevét a szövegdoboz és a **Most már engedélyezni** hivatkozást. Jelentkezzen be a kérheti.

    ![][11]

3. A **Csatlakozási kérelem** előugró párbeszédpanelen válassza az **Elfogadás** Azure a csapata project konfigurálása a Visual Studio Team Services engedélyezése lehetőséget.

    ![][12]

4. Miután létrejött a hitelesítés, megjelenik egy legördülő listán, amely tartalmazza a Visual Studio Team Services csapata projektek.  Jelölje ki a nevét a csapata project az előző lépésekben létrehozott, és válassza a varázsló pipajeles gombra.

    ![][13]

    A következő alkalommal, amikor a tárházba leküldéses, a jóváhagyás Visual Studio Team Services fog létre és helyezhetnek üzembe a projekt az Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy Újraépítés elindítása és telepítsen újra a projekthez

1. A Visual Studióban nyissa meg a fájl, és módosításáról. Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\MVC webes szerepet megosztva mappájában.

    ![][17]

2. Élőláb szerkesztése a webhelyet, és mentse a fájlt.

    ![][18]

3. A **Megoldás Intéző**nyissa meg a helyi menü a megoldást csomópontot, a projekt csomópontját vagy a módosított fájlt, és válassza a **véglegesítése**gombra.

4. Írja be a megjegyzést, és válassza a **véglegesítése**.

    ![][20]

5. Válassza a **szinkronizálás** hivatkozásra.

    ![][38]

6. Válassza a Jóváhagyás leküldéses a Visual Studio Team Services tárházba a **leküldéses** hivatkozásra. (Használhatja a **szinkronizálás** gombra a véglegesítése másolása a tárat. A különbség a, hogy **szinkronizálási** is gyűjti össze a legfrissebb módosítások a tárat a.)

    ![][39]

7. Válassza az **otthoni** gombra kattintva térjen vissza az **Explorer csoportwebhely** kezdőlapját.

    ![][21]

8. Válassza a **hozza létre** a buildjeiben megtekintése a folyamatban lévő.

    ![][22]

    **Csoportwebhely Explorer** megjeleníti, hogy építés elindítva a beadás az.

    ![][23]

9. A Szerkesztés előrehaladtával részletes naplózási megtekintéséhez kattintson duplán a folyamatban lévő építés.

10. Amikor összeállítása folyamatban van, nézze meg a csatolni kívánt Azure a varázsló használatakor létrehozott build definícióját.  Nyissa meg a helyi menü Szerkesztés meghatározására, és válassza a **Szerkesztés Definition szerkesztése**.

    ![][25]

11. Kattintson az **eseményindító** lap jelenik meg, hogy az összeállítás definition alapértelmezés szerint minden beadás épülnek értéke. (Egy felhőalapú szolgáltatásba, Visual Studio Team Services professzionális és automatikusan a fejlesztői környezet üzembe helyezése a fő ág. Továbbra is a kézi lépés azt a webhelyet, amely élő terjesztése. Webalkalmazás átmeneti tárolására környezeti nincs akkor közvetlenül azt a webhelyet, amely élő a fő ág üzembe helyezése.

    ![][26]

1. A **folyamatok** lapon megjelenik a telepítési környezet van beállítva a felhőből szolgáltatás vagy a webes alkalmazás nevére.

    ![][27]

1. Adja meg a Tulajdonságok értékeket, ha azt szeretné, hogy a különböző értékek, mint az alapértelmezett beállításokat. Azure közzététel tulajdonságainak **telepítési** szakaszában, és előfordulhat, hogy is kell MSBuild paramétereket állíthat. Például egy felhőalapú szolgáltatás project megadhatja a szolgáltatás konfiguráció eltérő "Felhő", állítsa be a MSbuild paraméterek `/p:TargetProfile=[YourProfile]` Ha a *[YourProfile]* ServiceConfiguration hasonló nevű szolgáltatás konfigurációs fájl megegyezik. *YourProfile*.cscfg.

    A következő táblázat mutatja az elérhető tulajdonságok, a **telepítési** szakaszban:

  	|A tulajdonság|Alapérték|
  	|---|---|
  	|Nem megbízható tanúsítványok engedélyezése|Hamis, ha az SSL-tanúsítványok a legfelső szintű hitelesítésszolgáltató alá kell írnia.|
  	|Frissítés engedélyezése|Lehetővé teszi, hogy újat hozna létre helyett meglévő-telepítés frissítése történő telepítéséhez. Megőrzi az IP-címet.|
  	|Ne töröljön|IGAZ, ha nem írják felül a meglévő nem kapcsolódó telepítés (frissítés engedélyezett).|
  	|A telepítési beállítások elérési út|A fájl elérési útját a .pubxml egy webalkalmazás viszonyított a repó a legfelső szintű mappájára. Felhőszolgáltatások nem veszi figyelembe.|
  	|SharePoint-telepítési környezet|Az ugyanaz, mint a szolgáltatás neve.|
  	|Azure telepítési környezet|A web app vagy a felhőalapú szolgáltatás neve.|

1. Ebben az esetben által az összeállítás kell hajtható végre.

    ![][28]

1. Ha duplán kattint az összeállítás nevét, Visual Studio látható **Összeállítása összegzése**, beleértve a kapcsolódó egység próba projektekből bármely vizsgálati eredmények.

    ![][29]

1. Az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)megtekintheti a társított példányban a **telepítés** lapon a fejlesztői környezet ki van jelölve.

    ![][30]

1.  Tallózással keresse meg a webhely URL-CÍMÉT. Webalkalmazás egyszerűen válassza a **Tallózás** gombra a portálon. Egy felhőalapú szolgáltatásba válassza az URL-CÍMÉT az **Irányítópult** oldalát a fejlesztői környezet megjelenítő **Quick Glance** szakaszában.

    A folyamatos integrációját felhőszolgáltatások telepítések alapértelmezés szerint a fejlesztői környezet vannak közzétéve. A **Másodlagos felhőalapú szolgáltatás környezet** tulajdonság beállításával **gyártás**módosíthatja. Az alábbiakban a webhely URL-címe esetén a felhőbeli szolgáltatástól Irányítópultlap.

    ![][31]

    Egy új böngészőlapon jelenítse meg a futó webhely nyílik meg.

    ![][32]

1.  Ha más módosítást a projekthez, eseményindító további hoz létre, és fog gyűjteniük a több telepítések. A legújabb egy aktív van megjelölve.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: egy korábbi verzióját újratelepítése

Ez a lépés nem kötelező. Az Azure klasszikus portálon válassza a korábbi telepítés, és válassza a **telepítsen újra** a webhelyen a egy korábbi beadás visszatekerése. Figyelje meg, hogy ezzel egy új build TFS az elindítani, és hozzon létre egy új bejegyzést telepítési előzményekben.

![][34]

## <a name="6-change-the-production-deployment"></a>6: az éles üzemi módosítása

Ha készen áll, ha kiválasztja az Azure klasszikus portál **felcserélése** alakíthat a fejlesztői környezet az éles üzemi környezetben. Az imént telepített fejlesztői környezet megszerzése gyártási van, és az előző munkakörnyezetben, válik a fejlesztői környezet. Az aktív telepítési előállítására és a környezetek átmeneti eltérőek lehetnek, de a legutóbbi buildjeiben telepítési előzményeit megegyezik a környezet függetlenül.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: a munka ág az üzembe.

Mely számjegy használatakor, általában módosítások elvégzése egy munka ág, és a fő ág integrálhatja a fejlesztés elérésekor egy állapotba. Projekt fejlesztési fázisban érdemes létre és helyezhetnek üzembe a munka ág az Azure.

1. A **Csapat Intéző**válassza a **Kezdőlap** gombot, és válassza ki a a **fiókok** gombra.

    ![][40]

2. Válassza az **Új fiók** hivatkozásra.

    ![][41]

3. Adja meg, például a "dolgozunk," a fiók nevét, és válassza a **Fiók létrehozása**. Ezzel létrehoz egy új helyi ág.

    ![][42]

4. Tegye közzé a ág. Válassza a fióknév **közzé nem tett használja**, és válassza a **Közzététel**lehetőséget.

    ![][44]

6. Alapértelmezés szerint csak a fő ág módosításainak folyamatos építés indítja el. A munka ág folyamatos build beállítani, a a **professzionális** lapot a **Csapat Intézőben**, és válassza a **Összeállítása Definition szerkesztése**.

7. Az **Adatforrás-beállítások** lap megnyitásához. A **folyamatos integráció és a Szerkesztés fióktelepekbe figyelt**, csoportban adja meg **egy új sor hozzáadásához kattintson ide**.

    ![][47]

8. Adja meg a ág hozta létre, például refs/előzetes/munka.

    ![][48]

9. A kód módosítás, nyissa meg a helyi menü megváltozott a fájl elemre, és válassza a **véglegesítése**lehetőséget.

    ![][43]

10. Válassza a **Szinkronizálatlan véglegesítése** hivatkozásra, és válassza a **szinkronizálás** gombra vagy a **leküldéses** hivatkozás másolása a módosításokat a munka ág a Visual Studio Team Services példányát.

    ![][45]

11. Nyissa meg a **professzionális** nézetet, és keresse meg a létrehozás, amelyen éppen használ a munka ág.

## <a name="next-steps"></a>Következő lépések

Mely számjegy használja a Visual Studio Team Services szolgáltatással kapcsolatos további tippek című témakörben talál [elektronikus oktatóanyagok elkészítése és megosztása a kódot a Visual Studio segítségével mely számjegy](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és Azure közzététele Visual Studio csapat szolgáltatásai nem kezelt mely számjegy összegyűjti használatával kapcsolatos információkért olvassa el a [Folyamatos telepítési Azure alkalmazás szolgáltatás](../app-service-web/app-service-continuous-deployment.md)című témakört.. Visual Studio Team Services kapcsolatos további tudnivalókért olvassa el a [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861)című témakört.

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
