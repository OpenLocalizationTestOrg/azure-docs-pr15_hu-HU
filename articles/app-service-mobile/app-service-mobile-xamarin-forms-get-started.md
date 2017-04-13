<properties
    pageTitle="Ismerkedés a Mobile-alkalmazások Xamarin.Forms használatával"
    description="Ezen oktatóprogram lépéseiből elkezdeni használni az Azure Mobile-alkalmazások fejlesztése Xamarin.Forms"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Xamarin.Forms alkalmazás létrehozása

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan kódmentes felhőalapú szolgáltatás hozzáadása az Azure mobilalkalmazás kódmentes használatával Xamarin.Forms mobilalkalmazásban. Létrehoz egy új mobilalkalmazás kódmentes és egy egyszerű _Teendőlista_ Azure-ban alkalmazás adatokat tároló Xamarin.Forms alkalmazás is.

Ebben az oktatóanyagban befejezése rendszer minden más Mobile-alkalmazások oktatóanyagok az Xamarin.Forms.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következőkre lesz szüksége:

* Azure active fiók. Ha az Azure próbaverzió regisztrálhat, és akár a get-fiókkal rendelkezik, nincs a 10 ingyenes, akár a próbaidőszak lejártakor folytassa a Mobile-alkalmazások. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio Xamarin együtt. [A telepítő és a Visual Studio és Xamarin telepítés](https://msdn.microsoft.com/library/mt613162.aspx) talál utasításokat. 

* A Mac vagy újabb verziójával Xcode 7.0 és Xamarin Studio közösségi telepítve. Lásd: [a telepítő for Visual Studio és Xamarin telepítés](https://msdn.microsoft.com/library/mt613162.aspx) és [beállítás, telepítése, és a Mac-felhasználók számára ellenőrzések](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja alkalmazás szolgáltatás](https://tryappservice.azure.com/?appServiceName=mobile), ahol azonnal létrehozhat egy rövid életű starter Mobile alkalmazásban az alkalmazás szolgáltatás megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="create-a-new-azure-mobile-app-backend"></a>Hozzon létre egy új Azure mobilalkalmazás kódmentes

Kövesse ezeket a lépéseket követve hozzon létre egy új mobilalkalmazás kódmentes.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Most már van, kiépítéstől a az Azure mobilalkalmazás kódmentes a mobil ügyfélalkalmazások használható. Ezután töltheti le egy egyszerű "teendőlista" kiszolgáló projektté kódmentes és Azure közzéteheti.

## <a name="configure-the-server-project"></a>A kiszolgáló-projekt beállítása

Hajtsa végre az alábbi lépéseket követve állítsa be a kiszolgáló projekt Node.js vagy .NET kódmentes használni.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Töltse le és futtassa a Xamarin.Forms megoldás

Itt van néhány választási lehetőséget. Mac töltse le a megoldást, és nyissa meg az Xamarin Studio, vagy töltse le a megoldás egy Windows rendszerű számítógépről, és nyissa meg a Visual Studióban, hálózati Mac használata az iOS-alkalmazás készítéséhez. Ha az Xamarin beállítás jelenik meg a részletes útmutatás van szüksége, nézze át a [beállítása és a Visual Studio és Xamarin telepítése](https://msdn.microsoft.com/library/mt613162.aspx) .

Vegyük folytassa:

 1. A Mac vagy Windows rendszerű számítógépén nyissa meg a böngészőablakban az [Azure-portálon] .
 2. Kattintson a beállítások lap, a Mobile alkalmazáshoz, kattintson az **Első lépések** (a mobil) > **Xamarin.Forms**. Lépés a 3-as kattintson **egy új alkalmazás létrehozása** , ha nem az az aktív.  Ezután kattintson a **Letöltés** gombra.

    Egy projekt, amely tartalmazza a mobilalkalmazásban csatlakoztatott ügyfélalkalmazás letölti ezt. A tömörített projektfájlok mentésére a helyi számítógépre, és jegyezze fel a mentési helyét.

 3. Bontsa ki a projektet, amely letöltötte, és nyissa meg az Xamarin Studio vagy Visual Studio.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Nem kötelező) Futtassa az iOS-projekt

Ez a szakasz nem fut az iOS-eszközökhöz Xamarin iOS projekt. Ez a szakasz átugorhatja, ha nem működik az iOS-eszközök.

####<a name="in-xamarin-studio"></a>A Xamarin Studio

1. Kattintson a jobb gombbal az iOS-projektet, és kattintson a **Beállítása, indítása projekt**.
2. Kattintson a **Futtatás** menü **Indítása hibakeresési** készítése a projekthez, és indítsa el az alkalmazást az iPhone-alapú irányító.

####<a name="in-visual-studio"></a>A Visual Studio
1. Kattintson a jobb gombbal az iOS-projektet, és kattintson a **indítási projektként beállítása**gombra.
2. A **Szerkesztés** menüben kattintson a **Configuration Manager**parancsra.
3. **Configuration Manager** párbeszédpanelen jelölje ki a iOS projekt **készítése** és **Deploy** jelölőnégyzetek.
4. Nyomja le az **F5** billentyűvel készítése a projekthez, és indítsa el az alkalmazást az iPhone-alapú irányító.

    >[AZURE.NOTE] Ha a probléma összeállítását, NuGet futtatása csomag manager és a Xamarin a legújabb frissítéseket támogatja a csomagok. Előfordul, hogy a quickstart útmutató projekteket is időbeli eltérés a frissítést a legkésőbbiig.    

Abban az alkalmazásban, írja be a értelmezhető szöveget, például _Xamarin ismerje meg,_ és kattintson a **+** gombra.

![][10]

POST kérelem küld az új mobilalkalmazás kódmentes Azure-ban is. A kért adatokat a program beszúrja a TodoItem táblázatba. A táblában tárolt elemek szerepeljenek eredményként a mobilalkalmazásban kódmentes szerint, és az adatok jelennek meg a listában.

>[AZURE.NOTE]
> A kód, a fájlban TodoItemManager.cs C# a hordozható osztály tár projekt a megoldás a mobilalkalmazásban kódmentes hozzáférő találhatók.

##<a name="optional-run-the-android-project"></a>(Nem kötelező) Az Android projekt futtatása

Ez a szakasz nem fut a Xamarin droid projekt az Android-alapú. Ebben a szakaszban kihagyhatja, ha nem használata Android-eszközön.

####<a name="in-xamarin-studio"></a>A Xamarin Studio

1. Kattintson a jobb gombbal az Android projekt, és kattintson a **Beállítása, indítása projekt**.
2. Kattintson a **Futtatás** menü **Indítása hibakeresési** készítése a projekthez, és indítsa el az alkalmazás az Android irányító.

####<a name="in-visual-studio"></a>A Visual Studio
1. Kattintson a jobb gombbal az Android-alapú (Droid)-projektet, és kattintson a **indítási projektként beállítása**gombra.
4. A **Szerkesztés** menüben kattintson a **Configuration Manager**parancsra.
5. **Configuration Manager** párbeszédpanelen jelölje ki a Android projekt **készítése** és **Deploy** jelölőnégyzetek.
6. Nyomja le az **F5** billentyűvel készítése a projekthez, és indítsa el az alkalmazás az Android irányító.

    >[AZURE.NOTE] Ha probléma összeállítását, futtassa a NuGet csomag manager és a Xamarin a legújabb frissítéseket támogatja a csomagok. Előfordul, hogy a quickstart útmutató projekteket is időbeli eltérés a frissítést a legkésőbbiig.    


Abban az alkalmazásban, írja be a értelmezhető szöveget, például _Xamarin ismerje meg,_ és kattintson a **+** gombra.

![][11]

POST kérelem küld az új mobilalkalmazás kódmentes Azure-ban is. A kért adatokat a program beszúrja a TodoItem táblázatba. A táblában tárolt elemek szerepeljenek eredményként a mobilalkalmazásban kódmentes szerint, és az adatok jelennek meg a listában.

> [AZURE.NOTE]
> A kód, a fájlban TodoItemManager.cs C# a hordozható osztály tár projekt a megoldás a mobilalkalmazásban kódmentes hozzáférő találhatók.


##<a name="optional-run-the-windows-project"></a>(Nem kötelező) Futtassa a Windows-projekt


Ez a szakasz nem fut a Xamarin WinApp projekt Windows mobileszközökön. Ez a szakasz kihagyhatja, ha nem működik Windows eszközökkel.


####<a name="in-visual-studio"></a>A Visual Studio
1. Kattintson a jobb gombbal a Windows projektek közül, és kattintson a **beállítás indítása projektként**.
4. A **Szerkesztés** menüben kattintson a **Configuration Manager**parancsra.
5. **Configuration Manager** párbeszédpanelen jelölje be a **készítése** és **Deploy** jelölőnégyzetét, amelyhez a Windows-projekt.
6. Nyomja le az **F5** billentyűvel készítése a projekthez, és indítsa el az alkalmazást egy Windows irányító.

    >[AZURE.NOTE] Ha probléma összeállítását, futtassa a NuGet csomag manager és a Xamarin a legújabb frissítéseket támogatja a csomagok. Előfordul, hogy a quickstart útmutató projekteket is időbeli eltérés a frissítést, hogy a legújabb.    


Abban az alkalmazásban, írja be a értelmezhető szöveget, például _Xamarin ismerje meg,_ és kattintson a **+** gombra.

POST kérelem küld az új mobilalkalmazás kódmentes Azure-ban is. A kért adatokat a program beszúrja a TodoItem táblázatba. A táblában tárolt elemek szerepeljenek eredményként a mobilalkalmazásban kódmentes szerint, és az adatok jelennek meg a listában.

![][12]

> [AZURE.NOTE]
> A kód, a fájlban TodoItemManager.cs C# a hordozható osztály tár projekt a megoldás a mobilalkalmazásban kódmentes hozzáférő találhatók.

##<a name="next-steps"></a>Következő lépések

* [Hitelesítés felvétele az alkalmazásba](app-service-mobile-xamarin-forms-get-started-users.md)  
Megtudhatja, hogy miként tel az alkalmazás felhasználóinak hitelesítést végezni.

* [Leküldéses értesítések felvétele az alkalmazásba](app-service-mobile-xamarin-forms-get-started-push.md)  
Megtudhatja, hogy miként vehet fel a leküldéses értesítések támogatása az alkalmazás és a Mobile alkalmazásban kódmentes Azure értesítés hubok segítségével küldje el a leküldéses értesítések konfigurálása.

* [Az alkalmazás offline szinkronizálás engedélyezése](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Megtudhatja, hogy miként hozzáadása az offline munka támogatása az alkalmazás-mobilalkalmazás kódmentes használatával. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

* [A felügyelt ügyfél használata Azure Mobile-appokról](app-service-mobile-dotnet-how-to-use-client-library.md)  
Megtudhatja, hogy miként dolgozhat az Xamarin alkalmazásban kezelt ügyfél SDK csomagjában talál. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portál]: https://portal.azure.com/

