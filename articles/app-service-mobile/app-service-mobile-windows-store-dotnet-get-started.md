<properties
    pageTitle="Hozzon létre egy egyetemes Windows platformon (UWP) használó Mobile-alkalmazások |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban Azure mobilalkalmazás háttérkiszolgálókon verzióval univerzális Windows platformon (UWP) alkalmazások fejlesztéséhez a C#, Visual Basic vagy a JavaScript használatbavételéhez."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>A Windows-alkalmazás létrehozása

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan kódmentes felhőalapú szolgáltatás hozzáadása egy univerzális Windows platformon (UWP) alkalmazásba. További tudnivalókért olvassa el a [Mik azok a Mobile-alkalmazások](app-service-mobile-value-prop.md)című témakört. A kész alkalmazásból képernyőképek a következők:

![Befejezett asztali alkalmazás](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Futó hozhat létre az asztalon. 

![Befejezett phone alkalmazása](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Operációs rendszert futtató telefonon

Ebben az oktatóanyagban befejezése rendszer minden más mobilalkalmazás oktatóanyagok-UWP alkalmazást. 

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következőkre lesz szüksége:

* Azure active fiók. Nem rendelkeznek fiókkal, ha egy Azure próbaverzió regisztrálhat, és legfeljebb 10 ingyenes mobilalkalmazásainak beszerzése, amelyet még a próbaidőszak lejártakor megtartása használ. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio közösségi 2015] vagy újabb verziója.

>[AZURE.NOTE] Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg](https://tryappservice.azure.com/?appServiceName=mobile). Azonnal létrehozhat egy rövid életű starter mobilalkalmazás az alkalmazás szolgáltatás – nem kötelező hitelkártya, és nincs nyilatkozatát.

##<a name="create-a-new-azure-mobile-app-backend"></a>Hozzon létre egy új Azure mobilalkalmazás kódmentes

Kövesse ezeket a lépéseket követve hozzon létre egy új mobilalkalmazás kódmentes.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Most már van, kiépítéstől a az Azure mobilalkalmazás kódmentes a mobil ügyfélalkalmazások használható. Ezután töltheti le egy egyszerű "teendőlista" kiszolgáló projektté kódmentes és Azure közzéteheti.

## <a name="configure-the-server-project"></a>A kiszolgáló-projekt beállítása

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Töltse le és futtassa az ügyfél-projekt

Miután beállította a mobilalkalmazás kódmentes, hozzon létre egy új ügyfél alkalmazás, vagy csatlakozhat az Azure-meglévő alkalmazás módosítása. Ebben a részben töltse le egy UWP alkalmazás sablon projekt való csatlakozáshoz a mobilalkalmazás kódmentes testreszabott.

1. Vissza a mobilalkalmazás kódmentes az **első lépések** lap, kattintson az **Új alkalmazás létrehozása** > **letöltése**, majd a helyi számítógépre tömörített projektfájlok venni.

    ![Töltse le a Windows quickstart útmutató projekt](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Nem kötelező) Adja hozzá a UWP app a project a kiszolgáló projektként ugyanahhoz a megoldáshoz. Így könnyebben hibakeresési, és tesztelje az alkalmazás és a kódmentes ugyanabban a Visual Studio megoldásban, ha úgy dönt, hogy erre. Projekt UWP alkalmazás hozzáadása a megoldást, kell használnia a Visual Studio 2015 vagy újabb verziója.

4. Az indítási projekt UWP alkalmazás nyomja le a telepíthető, és futtassa az alkalmazást az F5 billentyűt.

5. Abban az alkalmazásban értelmes szöveget, például *az oktatóprogram kész*, írja be a szövegdoboz **beszúrása egy TodoItem** , és kattintson a **Mentés**gombra.

    ![Windows quickstart útmutató teljes asztali](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    POST kérelem küld az új mobilalkalmazás kódmentes Azure-ban tárolt.

6. (Nem kötelező) Állítsa le az alkalmazást, majd indítsa újra a mobil irányító vagy másik eszközt.

    ![A Windows quickstart útmutató teljes telefonon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Figyelje meg, hogy az előző lépésben mentett adatok betöltése az Azure, a UWP alkalmazás indítása után. 

##<a name="next-steps"></a>Következő lépések

* [Hitelesítés felvétele az alkalmazásba](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Megtudhatja, hogy miként tel az alkalmazás felhasználóinak hitelesítést végezni.

* [Leküldéses értesítések felvétele az alkalmazásba](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Megtudhatja, hogy miként vehet fel a leküldéses értesítések támogatása az alkalmazás és a mobilalkalmazás kódmentes Azure értesítés hubok segítségével küldje el a leküldéses értesítések konfigurálása.

* [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio közösségi 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
