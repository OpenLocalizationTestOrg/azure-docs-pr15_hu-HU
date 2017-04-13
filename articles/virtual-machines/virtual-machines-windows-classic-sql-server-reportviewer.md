<properties 
    pageTitle="ReportViewer használata a webhelyen lévő |} Microsoft Azure"
    description="Ez a témakör ismerteti, hogyan hozhat létre a Microsoft Azure webhely és a Visual Studio ReportViewer vezérlő, amely a a Microsoft Azure virtuális gép tárolt jelentés jeleníti meg."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Azure-ban tárolt webhelyre ReportViewer használata

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


A Microsoft Azure webhely és a Visual Studio ReportViewer vezérlő, amely megjeleníti a a Microsoft Azure virtuális gép tárolt jelentés készíthet. A ReportViewer vezérlő szerepel a webalkalmazás az ASP.NET webes alkalmazás használata összeállítása.

>[AZURE.IMPORTANT] A ASP.NET MVC webalkalmazás-sablonok nem támogatják a ReportViewer vezérlő.

A Microsoft Azure webhely ReportViewer részévé tenni, meg kell az alábbi feladatok elvégzésére.

- **Hozzáadása** Szerelvények, hogy a telepítőcsomag

- **Konfigurálása** Hitelesítés és az engedélyezés

- **Közzététel** Azure ASP.NET webalkalmazás

## <a name="prerequisites"></a>Előfeltételek

Tekintse át az "általános ajánlási és a gyakorlati tanácsok" szakasz [SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] ReportViewer vezérlők Visual Studióban, Standard Edition vagy újabb operációs rendszer teljesült. A webes Developer Express Edition használja, ha telepítenie kell a [MICROSOFT jelentés MEGJELENÍTŐ 2012 FUTTATÓKÖRNYEZET](https://www.microsoft.com/download/details.aspx?id=35747) az ReportViewer futtatókörnyezet szolgáltatások használatához.
>
>Helyi feldolgozás módban konfigurált ReportViewer nem támogatott Microsoft Azure-ban.

Tekintse át a [Reporting Services-jelentés megjelenítő vezérlő és a Microsoft Azure virtuális gép alapú jelentéskészítő kiszolgálókon](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)követelményeinek.

## <a name="adding-assemblies-to-the-deployment-package"></a>Hozzáadás szerelvények, hogy a telepítőcsomag

A ASP.NET alkalmazás helyszíni üzemelteti, amikor a ReportViewer szerelvények telepítéskor rendszerint közvetlenül az IIS-kiszolgáló (GAC) globális összeállítási gyorsítótár Visual Studio a telepítés során, és közvetlenül az alkalmazás által is elérhető. Jó helyen jár Ha Ön üzemelteti a ASP.NET-alkalmazások a felhőben, Microsoft Azure nem engedi semmit, meg kell győződnie arról, hogy a ReportViewer szerelvények érhetők el az alkalmazáshoz helyileg a GAC, be kell telepíteni. Ehhez a projekt rájuk mutató hivatkozások hozzáadásával, és konfigurálhatók másolni helyileg.

Távoli feldolgozás módban a ReportViewer vezérlő használja a következő szerelvények:

- **Microsoft.ReportViewer.WebForms.dll**: akkor használja a ReportViewer az weblapon ReportViewer kódot tartalmazza. Hivatkozás a összeállítás hozzá a projekthez, a projekt ASP.NET-ról ReportViewer vezérlőelem elhelyezésekor.

- **Microsoft.ReportViewer.Common.dll**: futásidőben a ReportViewer vezérlő által használt osztályok tartalmazza. Nem automatikusan felkerül a projekthez.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Microsoft.ReportViewer.Common mutató hivatkozás hozzáadása

- Kattintson a jobb gombbal a projekt **hivatkozások** csomópontot, és válassza a **Hivatkozás hozzáadása**a .NET rendszerhez lapon jelölje be az összeállítás, és kattintson az **OK gombra**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>A szerelvények helyileg számára hozzáférhetővé szeretné tenni a ASP.NET-alkalmazás

1. A **hivatkozások** mappában kattintson a Microsoft.ReportViewer.Common összeállítás, annak tulajdonságait a Tulajdonságok ablaktáblában jelennek meg.

1. A Tulajdonságok ablaktáblában **Helyi példányt** igaz értékre állítás.

1. Microsoft.ReportViewer.WebForms esetében ismételje meg az 1-2.

### <a name="to-get-reportviewer-language-pack"></a>ReportViewer Language Pack csomag beszerzésére

1. A megfelelő Microsoft jelentés megjelenítő 2012 futtatókörnyezet terjeszthető telepítse a [Microsoft](http://go.microsoft.com/fwlink/?LinkId=317386)letöltőközpontból.

1. Jelölje ki a nyelvet a legördülő listából, és a lapon a megfelelő letöltési központ lapjára átirányítását.

1. Kattintson a **Letöltés** ReportViewerLP.exe letöltésének elindításához.

1. Miután letöltötte ReportViewerLP.exe, kattintson a **Futtatás** azonnal, és mentse a számítógépre a **Mentés** gombra. Ha a **Mentés**gombra ne felejtse el a mappát, ahová a fájlt menteni nevét.

1. Keresse meg azt a mappát, amelybe mentette a fájlt. Kattintson a jobb gombbal a ReportViewerLP.exe, kattintson a **Futtatás rendszergazdaként**parancsra, és kattintson az **Igen gombra**.

1. Miután ReportViewerLP.exe fut, látni fogja a c:\windows\assembly az erőforrás van fájlok **Microsoft.ReportViewer.Webforms.Resources** és **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>A honosított ReportViewer vezérlő szolgáltatáshoz való konfigurálása

1. Töltse le és telepítse a Microsoft jelentés megjelenítő 2012 futtatókörnyezet terjeszthető a fenti megadott utasításokat követve.

1. Hozzon létre <language> a project és a Másolás mappába a hozzárendelt erőforrás összeállítás fájlok van. Az erőforrás összeállítás fájlok másolandó: **Microsoft.ReportViewer.Webforms.Resources.dll** és **Microsoft.ReportViewer.Common.Resources.dll**. Jelölje ki az erőforrás-összeállítás fájlokat, és a Tulajdonságok ablaktáblában adja a **kimeneti könyvtár másolás** **"Mindig másolása**.

1. A kulturális környezet és UICulture beállítása a webes projektet. Állítsa be a kulturális környezet és a felhasználói felület nyelve egy ASP.NET-weblap kapcsolatos további tudnivalókért lásd: [hogyan: állítsa be a kulturális környezet és a felhasználói felület nyelve ASP.NET-weblap globalizációs](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Hitelesítés és az engedélyezés konfigurálása

A ReportViewer kell használni a megfelelő hitelesítő adatokkal hitelesíteni a jelentéskészítő kiszolgálót, és a hitelesítő adatok engedélyezni kell a jelentés kiszolgáló által a jelentések kívánt eléréséhez. Hitelesítés a további tudnivalókért lásd a [Reporting Services-jelentés megjelenítő vezérlő és a Microsoft Azure virtuális gép alapú jelentéskészítő kiszolgálókon](https://msdn.microsoft.com/library/azure/dn753698.aspx)követelményeinek.

## <a name="publish-the-aspnet-web-application-to-azure"></a>Azure ASP.NET webalkalmazás közzététele

A közzétételi Azure-ASP.NET webalkalmazás című cikkben olvashat [hogyan: áttelepítése és a Visual Studio Azure webalkalmazás közzététele](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) és [első lépések a Web Apps alkalmazások és az ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Ha az Azure környezetben projekt hozzáadása vagy Azure felhőalapú szolgáltatás projekt hozzáadása parancs a helyi menüben a megoldást Intézőben nem jelenik meg, előfordulhat, .NET-keretrendszer 4 a projekt cél keretét módosítása.
>
>A két parancs lényegében ugyanazokat a szolgáltatásokat nyújt. Egy- vagy az egyéb parancs megjelenik a helyi menü attól függően, hogy a Microsoft Azure SDK melyik verziója van telepítve.

## <a name="resources"></a>Erőforrások

[A Microsoft-jelentések](http://go.microsoft.com/fwlink/?LinkId=205399)

[Az SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó](virtual-machines-windows-classic-ps-sql-bi.md)

[Hozzon létre egy Azure virtuális natív mód jelentés-kiszolgálóval a PowerShell használatával](virtual-machines-windows-classic-ps-sql-report.md)

[A Reporting Services-jelentés megjelenítő vezérlő és a Microsoft Azure virtuális gép alapú jelentéskészítő kiszolgálón](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
