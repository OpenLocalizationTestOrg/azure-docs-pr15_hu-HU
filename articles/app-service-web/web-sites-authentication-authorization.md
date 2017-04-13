<properties 
    pageTitle="A helyszíni Active Directoryval az Azure alkalmazásában hitelesítő |} Microsoft Azure" 
    description="A vállalati verziós alkalmazások Azure App szolgáltatásban a különböző lehetőségek hitelesíteni a helyszíni Active Directory ismertetése" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>A helyszíni Active Directory az Azure alkalmazásában a hitelesítéshez #

Ebből a cikkből megtudhatja, hogyan kívánja hitelesíteni a helyszíni Active Directory (AD) [Azure App](../app-service/app-service-value-prop-what-is.md)szolgáltatásban. Azure alkalmazás üzemelteti a felhőben, de módjai hitelesíteni a helyszíni Active Directory-felhasználók biztonságosan. 

## <a name="authenticate-through-azure-active-directory"></a>Azure Active Directory hitelesítő
Az Azure Active Directory-bérlői lehet a címtár-szinkronizálja a helyszíni Active Directory. Ezt a megközelítést lehetővé teszi, hogy az Active Directory-felhasználók az alkalmazást az internetről és hitelesíteni a helyszíni hitelesítő adataival. Ezenkívül a Azure alkalmazás szolgáltatás [bekapcsolása billentyűs megoldás, ha ezt a módszert](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)kínál. Néhány kattintással a gombra a címtár-szinkronizált bérlői hitelesítéssel engedélyezheti az Azure számára. Ez a módszer a következő előnyökkel jár:

-   Nincs szükség az alkalmazás bármely hitelesítési kódot. Tudassa az alkalmazás szolgáltatásra, végezze el a hitelesítést az időt az alkalmazás funkciók nyújtanak.
-   [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx) biztosítanak hozzáférést az Azure-alkalmazás címtár-adatokhoz.
-   [Azure Active Directory által támogatott összes alkalmazások](/marketplace/active-directory/), többek között az Office 365-ben, a Dynamics CRM Online, a Microsoft Intune és a nem Microsoft cloud-alkalmazások ezer SSO biztosít. 
-   Azure Active Directory szerepköralapú hozzáférés-vezérlés támogatja. Használhatja a [Authorize(Roles="X")] minta minimális módosításokkal a kódot.

A vállalati verzió Azure alkalmazás, amely az Azure Active Directory címtárral hitelesíti szövegalkotás című témakör tartalmazza [az Azure Active Directory authentication egy a vállalati verzió Azure alkalmazás létrehozása](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Hitelesítő keresztül egy helyszíni STS
Ha egy helyszíni biztonságos jogkivonat-szolgáltatás például az Active Directory összevonási szolgáltatások (AD FS), is használhatja, amely hitelesítési összevonására az Azure számára. Ezt a megközelítést a legjobb, ha a vállalat házirendjének megakadályozza, hogy a Active Directory-adatok Azure-ban nem tárolhatók. Ne feledje azonban, a következőket:

-   STS topológia telepített a helyszíni a költség és kezelés terhelést kell lennie.
-   Csak az AD FS-rendszergazdák konfigurálhatja [megbízó fél bizalmi kapcsolatok és állítást szabályok](http://technet.microsoft.com/library/dd807108.aspx), amelyek korlátozhatják a fejlesztői beállítások képernyőjét. Kézzel akkor lehet, kezelése és [követelések](http://technet.microsoft.com/library/ee913571.aspx) alkalmazásonként alapon testreszabása.
-   Az Access a helyszíni Active Directory-adatok szükséges a vállalati tűzfalon keresztül külön megoldást.

A vállalati verzió Azure alkalmazás meghívottnak egy helyszíni STS a hatékony szövegalkotás a [egy a vállalati verzió Azure Active Directory összevonási szolgáltatások hitelesítéssel alkalmazás létrehozása](web-sites-dotnet-lob-application-adfs.md)című témakör tartalmazza.
 
