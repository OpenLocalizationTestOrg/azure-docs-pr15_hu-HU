<properties
    pageTitle="Folytonos kézbesítési a Visual Studio Team Services szolgáltatással Azure-ban |} Microsoft Azure"
    description="További információ a Visual Studio Team Services csapata projektek automatikusan össze, és telepítse az Azure alkalmazás szolgáltatás vagy felhőalapú szolgáltatások Web App szolgáltatásának konfigurálása."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Folytonos kézbesítési Azure Visual Studio Team Services használatával

A Visual Studio Team Services csapata projektek automatikusan összeállítása és Azure web Apps alkalmazások terjesztése vagy cloud services konfigurálása  (Tájékoztatást arról, hogyan állíthatja be a folyamatos fejlesztése, és egy *helyszíni* a Team Foundation Server használatával rendszer üzembe helyezése, lásd: [Az Azure-ban Cloud Services folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md).)

Ebben az oktatóanyagban feltételezi, hogy a Visual Studio 2013 és az Azure SDK telepítve van. Ha még nem rendelkezik a Visual Studio 2013, töltse le az **első lépések az ingyenes** hivatkozásra [www.visualstudio.com](http://www.visualstudio.com)kiválasztásával. Telepítse az Azure SDK [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Ebben az oktatóanyagban befejezéséhez-Visual Studio Team Services-fiókra van szüksége: [Nyissa meg az ingyenes egy Visual Studio Team Services fiókot](http://go.microsoft.com/fwlink/p/?LinkId=512979)is.

Egy felhőalapú szolgáltatásba, automatikusan össze, és telepítse az Azure, a Visual Studio Team Services használatával történő beállításához kövesse az alábbi lépéseket.

## <a name="1-create-a-team-project"></a>1: csoportwebhely projekt létrehozása

Kövesse [az alábbi](http://go.microsoft.com/fwlink/?LinkId=512980) a csapata project létrehozása és csatolhatja a Visual Studio. Ez a forgatókönyv azt feltételezi, hogy a Team Foundation verzió vezérlő (TFVC) az adatforrás-vezérlő megoldásként használja. Ha szeretné használni a verziókövetés mely számjegy, [Ez a forgatókönyv mely számjegy verziójának](http://go.microsoft.com/fwlink/p/?LinkId=397358)megtekintését.

## <a name="2-check-in-a-project-to-source-control"></a>2: jelölje be az adatforrás-vezérlő projekt létrehozása

1. A Visual Studióban nyissa meg azt a megoldást szeretné telepíteni, vagy hozzon létre egy újat.
Az Útmutató lépéseit követve telepítheti a webalkalmazást vagy egy felhőalapú szolgáltatásba (Azure-alkalmazás).
Ha szeretne új megoldás létrehozása, hozzon létre új Azure Felhőszolgáltatásában projektekbe, vagy új ASP.NET MVC projekt. Győződjön meg arról, hogy a projekt célként .NET-keretrendszer 4 vagy 4.5 és egy felhőalapú szolgáltatás projektet hoz létre, ha ASP.NET MVC webes szerepet és dolgozó szerepkört, és válassza ki a webes szerep Internet alkalmazást. Amikor a rendszer kéri, válassza az **Internetes alkalmazás**.
Ha szeretne egy webalkalmazás létrehozása, ASP.NET webalkalmazás project-sablon kiválasztása, és válassza a MVC. Lásd: [az ASP.NET web app alkalmazás szolgáltatásban Azure létrehozása](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services csak most támogatja a Visual Studio webalkalmazások telepítéseknél ki. Webhely projektek hatókör ki vannak.

1. A helyi menü a megoldást, és válassza a **Megoldás hozzáadása a verziókövetés**.

    ![][5]

1. Fogadja el vagy megváltoztathatja az alapértelmezett beállításokat, és válassza az **OK** gombra. A folyamat befejeződik, miután **Solution Explorer**adatforrás-vezérlő ikonok jelennek meg.

    ![][6]

1. Nyissa meg a helyi menü a megoldás, és válassza a **Beadás**.

    ![][7]

1. **Csoportwebhely Explorer** **Függőben lévő változásokat** területén írja be a megjegyzést, és kattintson a **Beadás** gombra.

    ![][8]

    Megjegyzés: a belefoglalásához vagy kizárásához bizonyos módosítások, ha beadja a beállításokat. Ha tartoznak a kívánt módosításokat, válassza az **Összes olyan** hivatkozásra.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: Csatlakozás a project Azure

1. Most, hogy bizonyos forráskód VIEWBEN Team Services csapata projekt rajta, készen áll a csapata project csatlakozhat az Azure.  Válassza az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885), az a felhő szolgáltatás vagy a web app, vagy hozzon létre egy újat kiválasztása a **+** ikont a bal alsó sarokban, majd a **Felhőbeli szolgáltatástól** vagy **Web App** , majd **Gyors létrehozásához**. Válassza a **Közzététel a Visual Studio csapat-szolgáltatások beállítása** hivatkozásra.

    ![][10]

1. A varázslóban írja be a Visual Studio Team Services-fiókja nevét a szövegmezőbe, és kattintson a **Most már engedélyezni** hivatkozásra. Jelentkezzen be a kérheti.

    ![][11]

1. A **Csatlakozási kérelem** előugró párbeszédpanelen válassza az **Elfogadás** gombra kattintva engedélyezheti a csapata project VIEWBEN Team Services konfigurálása Azure.

    ![][12]

1. Engedély létrejött, jelenik a Visual Studio Team Services csapata Projektlista tartalmazó legördülő listában. Válassza a csapata project az előző lépésekben létrehozott nevét, és válassza a varázsló pipajeles gombra.

    ![][13]

1. A projekt van csatolva, miután megváltozik a Visual Studio Team Services csapata project-ellenőrzési néhány útmutatást találhat.  A következő beadás, a Visual Studio Team Services össze, és telepítse a projektet az Azure.  Próbálja ki most ikonjára kattintva **Jelölje be a Visual Studio** hivatkozás, és kattintson a **Visual Studio indítása** hivatkozásra (vagy a egyenértékű **Visual Studio** gombra a portál képernyő alján).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy Újraépítés elindítása és telepítsen újra a projekthez

1. A Visual Studio **Csapat Explorer**válassza az **Adatforrás-vezérlő Explorer** hivatkozásra.

    ![][15]

1. Nyissa meg azt a megoldást fájl, és nyissa meg.

    ![][16]

1. Kattintson a **Megoldás Explorer**nyissa meg a fájl, és módosítsa. Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\MVC webes szerepet megosztva mappájában.

    ![][17]

1. Az embléma a webhelyen szerkesztése, és válassza a **Ctrl + S billentyűkombinációt** a fájl mentéséhez.

    ![][18]

1. A **Csoportwebhely Explorer**a **Függőben lévő változásokat** hivatkozás kiválasztása

    ![][19]

1. Írja be a megjegyzést, és válassza a **Beadás** gombra.

    ![][20]

1. Válassza az **otthoni** gombra kattintva térjen vissza az **Explorer csoportwebhely** kezdőlapját.

    ![][21]

1. Kattintson a buildjeiben megtekintése a folyamatban lévő **épít** hivatkozására.

    ![][22]

    **Csoportwebhely Explorer** megjeleníti, hogy építés elindítva a beadás az.

    ![][23]

1. Kattintson duplán a folyamatban van a részletes naplójának megtekintéséhez build előrehaladtával az összeállítás nevét.

    ![][24]

1. Amikor összeállítása folyamatban van, nézze meg a Szerkesztés definícióját, amely létrehozásakor kapcsolta TFS Azure a varázslóval.  Nyissa meg a helyi menü Szerkesztés meghatározására, és válassza a **Szerkesztés Definition szerkesztése**.

    ![][25]

    Kattintson az **eseményindító** lap jelenik meg, hogy az összeállítás definition alapértelmezés szerint minden beadás épülnek értéke.

    ![][26]

    A **folyamatok** lapon megjelenik a telepítési környezet van beállítva a felhőből szolgáltatás vagy a webes alkalmazás nevére. Ha web Apps alkalmazások dolgozik, megjelenik a Tulajdonságok eltér a itt látható lesz.

    ![][27]

1. Adja meg a Tulajdonságok értékeket, ha azt szeretné, hogy a különböző értékek, mint az alapértelmezett beállításokat. A Tulajdonságok Azure közzétételi szerepelnek, a **telepítési** szakaszban.

    A következő táblázat mutatja az elérhető tulajdonságok, a **telepítési** szakaszban:

  	|A tulajdonság|Alapérték|
  	|---|---|
  	|Nem megbízható tanúsítványok engedélyezése|Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltató által SSL-tanúsítványokat kell bejelentkeznie.|
  	|Frissítés engedélyezése|Lehetővé teszi, hogy újat hozna létre helyett meglévő-telepítés frissítése történő telepítéséhez. Megőrzi az IP-címet.|
  	|Ne töröljön|IGAZ, ha nem írják felül a meglévő nem kapcsolódó telepítés (frissítés engedélyezett).|
  	|A telepítési beállítások elérési út|A fájl elérési útját a .pubxml egy webalkalmazás viszonyított a repó a legfelső szintű mappájára. Felhőszolgáltatások nem veszi figyelembe.|
  	|SharePoint-telepítési környezet|Az ugyanaz, mint a szolgáltatás neve.|
  	|Azure telepítési környezet|A web app vagy a felhőalapú szolgáltatás neve.|

1. Ha több szolgáltatás konfigurációk (.cscfg fájlok) használata esetén megadhatja a kívánt szolgáltatás beállításai a **összeállítása, a speciális MSBuild argumentumok** beállítással. Például ServiceConfiguration.Test.cscfg használatához megadása MSBuild argumentumokat sor `/p:TargetProfile=Test`.

    ![][38]

    Ebben az esetben által az összeállítás kell hajtható végre.

    ![][28]

1. Ha duplán kattint az összeállítás nevét, Visual Studio látható **Összeállítása összegzése**, beleértve a kapcsolódó egység próba projektekből bármely vizsgálati eredmények.

    ![][29]

1. Az [Azure klasszikus portál](http://go.microsoft.com/fwlink/?LinkID=213885)megtekintheti a társított példányban a **telepítés** lapon a fejlesztői környezet ki van jelölve.

    ![][30]

1.  Tallózással keresse meg a webhely URL-CÍMÉT. Webalkalmazás csak kattintson a **Tallózás** gombra a parancssávon. Egy felhőalapú szolgáltatásba válassza az URL-CÍMÉT az **Irányítópult** oldalát a fejlesztői környezet egy felhőalapú szolgáltatás megjelenítő **Quick Glance** szakaszában. A folyamatos integrációját felhőszolgáltatások telepítések alapértelmezés szerint a fejlesztői környezet vannak közzétéve. **Alternatív felhő szolgáltatási környezetben** tulajdonság beállításával **termelési**módosíthatja. Ez a képernyőkép mutatja meg, ha a webhely URL-CÍMÉT a felhőbeli szolgáltatástól Irányítópultlap nem.

    ![][31]

    Egy új böngészőlapon jelenítse meg a futó webhely nyílik meg.

    ![][32]

    Felhőszolgáltatások Ha más módosítást a projekthez, eseményindító további hoz létre, és fog gyűjteniük a több telepítések. A legújabb állapotúként aktív.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: egy korábbi verzióját újratelepítése

Ez a lépés felhőszolgáltatások vonatkozik, és nem kötelező. Az Azure klasszikus portálon válassza a korábbi telepítés, és válassza ki a visszatekerése a webhelyen a egy korábbi beadás **telepítsen újra** gombra.  Figyelje meg, hogy ezzel egy új build TFS az elindítani, és hozzon létre egy új bejegyzést telepítési előzményekben.

![][34]

## <a name="6-change-the-production-deployment"></a>6: az éles üzemi módosítása

Ebben a lépésben csak a felhőszolgáltatások, nem a web Apps alkalmazások vonatkozik. Ha készen áll, előléptetheti a fejlesztői környezet az éles üzemi környezetben, az Azure klasszikus portálon a **felcserélése** gomb kiválasztásával. Az imént telepített fejlesztői környezet megszerzése gyártási van, és az előző munkakörnyezetben, válik a fejlesztői környezet. Az aktív telepítési előállítására és a környezetek átmeneti eltérőek lehetnek, de a legutóbbi buildjeiben telepítési előzményeit megegyezik a környezet függetlenül.

![][35]

## <a name="7-run-unit-tests"></a>7: egység vizsgálatok futtatása

Ebben a lépésben csak a web Apps alkalmazások, a felhőszolgáltatásokba nem vonatkozik. A példányban minőség kaput elhelyezéséhez egység vizsgálatok futtatása, és ha nem sikerül, a telepítési megszüntetheti.

1.  A Visual Studióban adhatja hozzá egy egység próba-projektet.

    ![][39]

1.  A vizsgálni kívánt projektet a project hivatkozásokat adni.

    ![][40]

1.  Adja hozzá az egyes egység vizsgálatok. Első lépésként próbálja meg egy üres teszt mindig adja meg, hogy.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Összeállítás definíciójának szerkesztése, kattintson a **folyamat** fülre, és bontsa ki a **próba** csomópontot.

1.  **A teszt sikertelen Fail build** beállítás igaz értékű. Ez azt jelenti, hogy a telepítő nem fordul elő, ha a vizsgálatok adják át.

    ![][41]

1.  Egy új build várólista.

    ![][42]

    ![][43]

1. Miközben összeállítása halad, jelölje be a meghívási folyamat.

    ![][44]

    ![][45]

1. A Szerkesztés befejezése után ellenőrizze a vizsgálati eredmények.

    ![][46]

    ![][47]

1.  Próbáljon meg egy tesztelése, hogy a sikertelen lesz. Új vizsgálat hozzáadása a legelső másolásával, nevezze át és ki a sort, amely szerint NotImplementedException várható kivételt megjegyzést.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Ellenőrizze, hogy a változtatások várólista egy új build.

    ![][48]

1. Megtekintheti a vizsgálati eredmények szeretne tudni a hibát.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Következő lépések
A Visual Studio Team Services tesztelése egység, bővebben: [a verziójában egység vizsgálatok futtatása](http://go.microsoft.com/fwlink/p/?LinkId=510474). Ha mely számjegy használ, olvassa el a [megosztás mely számjegy a kód](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és a [folyamatos telepítési Azure alkalmazás szolgáltatásba](../app-service-web/app-service-continuous-deployment.md).  Visual Studio Team Services kapcsolatos további tudnivalókért olvassa el a [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861)című témakört.

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
