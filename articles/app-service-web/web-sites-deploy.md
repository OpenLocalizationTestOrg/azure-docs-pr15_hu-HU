<properties
    pageTitle="Telepítse az alkalmazást az Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként telepítse az alkalmazást az Azure alkalmazás szolgáltatásba."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cephalin;dariac"/>
    
# <a name="deploy-your-app-to-azure-app-service"></a>Telepítse az alkalmazást az Azure alkalmazás szolgáltatás

Ez a cikk segít eldöntheti, hogy a legjobb megoldást [Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714)a fájlokat a web App alkalmazásban, mobilalkalmazás kódmentes vagy API-alkalmazás telepítéshez használni, és végigvezet megfelelő forrásokat találhat jellemző a kívánt beállítást.

## <a name="overview"></a>Azure alkalmazás szolgáltatás telepítési – áttekintés

Azure alkalmazás szolgáltatás az keretrendszer tárolja, Önnek (ASP.NET, PHP, Node.js, stb.). Néhány keretek például Java és Python míg mások, alapértelmezés szerint engedélyezve vannak, ahhoz, hogy egy egyszerű pipa konfiguráció szükséges. Ezeken kívül az alkalmazás keretrendszer, például a PHP verziót vagy a futtatókörnyezet bitszámértékének is testre szabhatja. További tudnivalókért lásd: a [Configure Azure alkalmazás szolgáltatás alkalmazását](web-sites-configure.md).

Nem kell aggódnia amiatt, hogy a webes server vagy a program keretében, mivel az alkalmazás szolgáltatás telepítése az alkalmazás frissítését és üzembe helyezése a kód, bináris, tartalom fájlokat és a megfelelő címtár-struktúra a [ **/site/wwwroot** címtár](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure-ban (vagy a **/feladatok webhely/wwwroot/App_Data/** címtár-WebJobs). Alkalmazás szolgáltatás az alábbi telepítési lehetőségek használatát támogatja: 

- [FTP- vagy FTPS](https://en.wikipedia.org/wiki/File_Transfer_Protocol): a kedvenc FTP vagy FTPS használata engedélyezve van a fájlok áthelyezése az Azure- [FileZilla](https://filezilla-project.org) teljes szolgáltatáskészletet nyújtó IDEs [NetBeans](https://netbeans.org)például hogy az eszköz. Ez a szigorúan egy fájl feltöltése folyamat. Nincsenek további szolgáltatások alkalmazás szolgáltatás, által nyújtott például verziókövetést, struktúra Fájlkezelés stb. 

- [Kudu (mely számjegy/Mercurial vagy a OneDrive vagy Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): a [telepítés motor](https://github.com/projectkudu/kudu/wiki) alkalmazás szolgáltatás használata. A kód leküldéses közvetlenül a bármely tárházba való Kudu. Kudu hozzáadott szolgáltatásokat is nyújt, valahányszor kód tolódik, például verziókövetést, csomag visszaállítása, MSBuild és [webes horgok](https://github.com/projectkudu/kudu/wiki/Web-hooks) folyamatos telepítési és egyéb automatizálási feladatokat. A Kudu telepítési motor telepítési forrás 3 különböző típusú támogatja:   
    * Tartalom szinkronizálása a OneDrive és a Dropbox   
    * Folytonos telepítési tárházba-alapú automatikus szinkronizálása az GitHub, Bitbucket és a Visual Studio Team Services  
    * Tárházba-alapú környezet, amelyben a helyi mely számjegy manuális szinkronizálása  

- [Webes üzembe](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): Deploy kód alkalmazás szolgáltatás közvetlenül a Microsofttól a kedvenc eszközök, például azonos szerszámok, amely a Visual Studio automatizálja IIS-kiszolgálók történő telepítéséhez. Ez az eszköz csak összehasonlítása telepítési, az adatbázis létrehozása, kapcsolati karakterláncot, stb átalakítások támogatja. Webes Deploy Kudu különböznek egymástól annak, hogy az alkalmazás bináris kialakításának, mielőtt azokat a Azure telepítik. FTP hasonló, nincs további szolgáltatások által nyújtott szolgáltatás alkalmazás.

Népszerű webes Fejlesztőeszközök támogatja a telepítési folyamatok közül. A kiválasztott eszköz határozza meg, hogy a telepítési folyamatot is élvezheti, miközben a tényleges DevOps funkciókat rendelkezésére a telepítési folyamatot, ha a kiválasztott adott eszközök függ. Ha hajt végre webes üzembe [Azure SDK a Visual Studióban](#vspros), akkor is, ha nem kap automatizálási az Kudu, például kapja a Visual Studióban csomag visszaállítása telepített és automatizálási MSBuild. 

>[AZURE.NOTE] Ezek a folyamatok üzembe a ténylegesen [az Azure forrásokban kiépítése](../resource-group-template-deploy-portal.md) szüksége van az alkalmazás nem. Jó helyen jár, a csatolt útmutatók a legtöbb megtudhatja, hogy miként hozhatók létre az alkalmazás és helyezhetnek üzembe a kód azt végpontok közötti. Azure erőforrások [automatizálás telepítési parancssori eszközökkel](#automate) szakaszában kiépítési további beállításokat is megtalálhatók.
     
## <a name="ftp"></a>Fájlok másolása Azure manuális telepítéséhez FTP-n keresztül
A webes tartalom manuális másolása webkiszolgálóra használatakor, az [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) segédprogrammal fájlok, többek között a Windows Intéző vagy [FileZilla](https://filezilla-project.org/)másolja.

A fájlok manuális másolása a szakemberek a következők:

- Ismerete és az FTP szerszámok minimális összetettsége. 
- Ismeretében pontosan a fájlok vannak.
- A felvett értékpapír FTPS.

A fájlok manuális másolása a negatívumokat a következők:

- Hogy kíváncsi a fájlok üzembe a megfelelő könyvtárak App szolgáltatásban. 
- Nincs verziókövetés visszaállítás hiba esetén.
- Telepítési problémáinak nincs beépített telepítési előzményeit.
- Lehetséges hosszú telepítési látszódjon, mert sok FTP-eszközök nem adja meg, csak élőláb másolása és egyszerűen másolja át az összes fájlt.  

### <a name="howtoftp"></a>Fájlok másolása Azure kézzel történő telepítéséről
Fájlok másolása Azure néhány egyszerű lépésből áll:

1. Feltételezve, hogy a már létrehozott telepítési hitelesítő adatait, tudhatja meg az FTP kapcsolatadatainak **Beállítások** > **Tulajdonságok**, majd másolja az értékeket az **FTP/központi felhasználói**, **FTP állomásnév**és **FTPS Host Name**. Másolja az **FTP/központi felhasználói** felhasználói az Azure-portálra, többek között az alkalmazás nevét az FTP-kiszolgáló a megfelelő környezet biztosítása érdekében által megjelenített érték.
2. A kapcsolat adatait, az alkalmazás csatlakozhat összegyűjtötte az FTP-ügyfél használja.
3. Másolja a fájlok és a megfelelő címtár-struktúra a [ **/site/wwwroot** címtár](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) Azure-ban (vagy a **/feladatok webhely/wwwroot/App_Data/** címtár-WebJobs).
4. Tallózással keresse meg az app URL-címe, ellenőrizze, hogy az alkalmazás megfelelően működik-e. 

További tudnivalókért lásd: az alábbi forrást:

* A [megfelelő PHP-MySQL web App alkalmazásban készíthetnek és helyezhetnek üzembe, FTP használatával](web-sites-php-mysql-deploy-use-ftp.md).

## <a name="dropbox"></a>Egy-egy felhőalapú mappa szinkronizálását telepítéséhez
A helyes ahelyett, hogy a [fájlok manuális másolása](#ftp) szinkronizálódó fájlok és mappák alkalmazás szolgáltatás a egy felhőalapú tárhelyszolgáltatáshoz, például a OneDrive és a Dropbox. Egy-egy felhőalapú mappa szinkronizálását használja a Kudu folyamat telepítéshez (lásd: a [telepítési folyamatot áttekintése](#overview)).

A szakemberek a szinkronizált mappa létrehozása a felhőben vannak:

- Egyszerű a környezetben. Szolgáltatásai, például a OneDrive és a Dropbox asztali szinkronizálási ügyfelek, adja meg, tehát a helyi munkakönyvtárat is telepítési címtárában.
- Telepítési egyetlen kattintással.
- A Kudu telepítési motor az összes funkció áll rendelkezésre (például csomag visszaállítása, automatizálási).

A negatívumokat, egy-egy felhőalapú mappa szinkronizálását a következők:

- Nincs verziókövetés visszaállítás hiba esetén.
- Nincs automatikus telepítési manuális szinkronizálási szükség.

### <a name="howtodropbox"></a>Egy-egy felhőalapú mappa szinkronizálását telepítéséről
Az [Azure-portálon](https://portal.azure.com)kijelöli a tartalom szinkronizálása egy mappát a OneDrive vagy Dropbox felhő tárolóban lévő, az alkalmazás kódot és a tartalom abban a mappában, és alkalmazás szolgáltatás szinkronizálni a egyetlen kattintással.

* [Azure alkalmazás szolgáltatás felhő mappa szinkronizálása tartalmát](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>Felhőalapú adatforrás-vezérlő szolgáltatásból folyamatosan terjesztése
Ha a fejlesztőcsapatához egy felhőalapú forrás kód management (SCM) szolgáltatásba, például a [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com)vagy [BitBucket](https://bitbucket.org/)használ, alkalmazás szolgáltatást integrálása a tárházba, és helyezhetnek üzembe a folyamatosan konfigurálhatja. 

Szakemberek telepíti az adatforrás felhőalapú vezérlő szolgáltatáson keresztül, a következők:

- Ahhoz, hogy a visszaállítás verziókövetés.
- Azt jelenti, hogy mely számjegy (és szükség szerint Mercurial) folyamatos telepítésének konfigurálásához tárházakban. 
- Különböző [helyek](web-sites-staged-publishing.md)ággal ág-specifikus példányban telepítheti.
- A Kudu telepítési motor az összes funkció áll rendelkezésre (például telepítési verziószámozás, a visszaállítás, csomag visszaállítása, automatizálást).

A felhőalapú adatforrás-vezérlő szolgáltatásból üzembe helyezése con van:

- Néhány ismerete a megfelelő SCM szolgáltatás szükséges.

###<a name="vsts"></a>Telepítéséről folyamatosan felhőalapú adatforrás-vezérlő szolgáltatásból
Az [Azure-portálon](https://portal.azure.com)GitHub, a Bitbucket és a Visual Studio Team Services folyamatos telepítési is beállíthatja.

* [Folyamatos telepítési Azure alkalmazás szolgáltatásba](app-service-continuous-deployment.md). 

## <a name="localgitdeployment"></a>A helyi mely számjegy terjesztése
Ha a fejlesztőcsapatához használ egy helyszíni helyi forrás kód (SCM) alkalmazáskezelési szolgáltatás mely számjegy alapján, beállíthatja úgy a alkalmazás szolgáltatás telepítési adatforrásként. 

A helyi mely számjegy üzembe helyezése a szakemberek a következők:

- Ahhoz, hogy a visszaállítás verziókövetés.
- Ág-specifikus példányban ággal telepítheti a különböző [helyek](web-sites-staged-publishing.md).
- A Kudu telepítési motor az összes funkció áll rendelkezésre (például telepítési verziószámozás, a visszaállítás, csomag visszaállítása, automatizálási).

A helyi mely számjegy üzembe helyezése a con van:

- Néhány ismerete a megfelelő SCM rendszer szükséges.
- Nem folytonos telepítéshez bekapcsolása billentyűs megoldásokat. 

###<a name="vsts"></a>A helyi mely számjegy telepítéséről
Az [Azure-portálon](https://portal.azure.com)helyi mely számjegy telepítési is beállíthatja.

* [Helyi mely számjegy telepítési Azure alkalmazás szolgáltatásba](app-service-deploy-local-git.md). 
* [Bármely mely számjegy/hg repó a webalkalmazások közzétételéhez](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Egy IDE használatával terjesztése
Ha már használja [A Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) - [Azure SDK](https://azure.microsoft.com/downloads/)vagy más IDE programcsomagok, például a [Xcode](https://developer.apple.com/xcode/), [Holdas](https://www.eclipse.org)és [IntelliJ arról](https://www.jetbrains.com/idea/), telepítheti közvetlenül az Azure belül a IDE. Ez a beállítás egy egyéni Fejlesztőeszközök ideális.

Visual Studio támogatja minden három telepítési folyamatot (FTP, mely számjegy és webes üzembe), attól függően, hogy a beállításait, míg más IDEs telepítheti alkalmazás szolgáltatás, ha FTP vagy mely számjegy integrációs is (lásd: a [telepítési folyamatot áttekintése](#overview)).

A szakemberek telepíti az egy IDE használ, a következők:

- Esetleg állítható kis méretűre a szerszámok, a végpontok közötti alkalmazás életciklus-számára. Fejlesztése, hibakeresése, nyomon követheti és telepítse az alkalmazást az Azure minden kívül a IDE helyezés nélkül. 

A negatívumokat telepíti az egy IDE használ, a következők:

- A szerszámok hozzáadott összetettsége.
- Egy adatforrás-vezérlő rendszer továbbra is a csapata project igényel.

<a name="vspros"></a>Az üzembe helyezné a Visual Studio segítségével Azure SDK csomagjában talál további szakemberek a következők:

- Azure SDK Azure erőforrásokat teszi osztályú polgárok a Visual Studióban. Létrehozása, törlése, szerkesztése, indítsa el, és leállítása alkalmazások, a lekérdezés SQL, adatbázisszerű, kódmentes, live-hibakeresési az Azure alkalmazást, és még sok mással. 
- Élő szerkesztési Azure fájlok kódot.
- Az élő Azure a hibakeresési alkalmazások.
- Integrált Azure explorer.
- Ha csak összehasonlítása telepítési. 

###<a name="vs"></a>A Visual Studio közvetlenül telepítéséről

* [Első lépések a Azure és ASP.NET](web-sites-dotnet-get-started.md). Hogyan lehet készíthetnek és helyezhetnek üzembe a egy egyszerű ASP.NET MVC webes projekt Visual Studio és a webhely üzembe használatával.
* [Hogyan kell üzembe Azure WebJobs Visual Studio segítségével](websites-dotnet-deploy-webjobs.md). Hogyan lehet, hogy azok telepítése, mint WebJobs állítsa be a New projektek.  
* A [Biztonságos ASP.NET MVC 5 alkalmazások tagság, OAuth, és Web Apps alkalmazások SQL-adatbázis terjesztése](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Hogyan hozhat létre, és a Visual Studio, a webhely üzembe és a szervezet keretrendszer kód első áttelepítések egy ASP.NET MVC webes projekt SQL-adatbázishoz való telepítéséhez.
* [ASP.NET Web telepítésének Visual Studio segítségével](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 12 részből álló oktatóanyag sorozata, amely magában foglalja a telepítési feladatok teljesebb cellatartományt, mint a többi ebben a listában. Azure környezetben funkciókkal, mivel az oktatóprogram készült, de később jegyzetek ismertetik amik hiányoznak hozzáadva.
* [Üzembe helyezné a Visual Studio 2012 mely számjegy összegyűjti közvetlenül az Azure-ASP.NET webhelyére](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Megtudhatja, hogyan telepítse a Visual Studióban, mely a beépülő modul számjegy használatával véglegesítse a kód mely számjegy és a mely számjegy tárházba összekötő Azure-ASP.NET web-projektet. A Visual Studio 2013 kezdve, mely számjegy támogatási beépített és használatához nincs szükség a beépülő modul telepítésével.

###<a name="aztk"></a>Az Azure eszközök gazdag használatának Holdas és IntelliJ arról telepítéséről

Microsoft Azure Web Apps alkalmazások telepítéséhez közvetlenül a Holdas és a [Holdas Azure eszközkészlete](../azure-toolkit-for-eclipse.md) és [Azure eszközkészlete IntelliJ](../azure-toolkit-for-intellij.md)keresztül IntelliJ lehetővé teszi. Az alábbi oktatóanyagok játszik szerepet egyszerű a "Hello" nemzetközi webalkalmazás bevezetéshez vagy IDE használatával Azure lépések bemutatják:

*  [Hozzon létre Holdas az Azure helló világ webalkalmazást](./app-service-web-eclipse-create-hello-world-web-app.md). Ebből az oktatóanyagból megtudhatja, hogyan az Azure eszközkészlete Holdas segítségével készíthetnek és helyezhetnek üzembe a helló világ webalkalmazást az Azure.
*  [Hozzon létre IntelliJ az Azure helló világ webalkalmazást](./app-service-web-intellij-create-hello-world-web-app.md). Ebből az oktatóanyagból megtudhatja, hogyan az Azure eszközkészlete IntelliJ segítségével készíthetnek és helyezhetnek üzembe a helló világ webalkalmazás az Azure.


## <a name="automate"></a>Telepítési automatizálása parancssori eszközök használatával

* [Automatizálja alapú környezet, amelyben MSBuild](#msbuild)
* [Fájlok másolása FTP-eszközök és parancsfájlok](#ftp)
* [A Windows PowerShell telepítésének automatizálása](#powershell)
* [Környezet, amelyben a .NET-kezelés API automatizálása](#api)
* [Az Azure üzembe parancssori kezelőfelületről Azure](#cli)
* [Webes üzembe parancssorából Deploy](#webdeploy)
* A [Köteg FTP-parancsfájlokkal](http://support.microsoft.com/kb/96269).
 
Egy másik telepítési, hogy egy felhőalapú szolgáltatásba, például [Polip üzembe](http://en.wikipedia.org/wiki/Octopus_Deploy)használata. További tudnivalókért lásd: [üzembe ASP.NET-alkalmazásokat az Azure-webhelyeket](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

###<a name="msbuild"></a>Automatizálja alapú környezet, amelyben MSBuild

Ha a [Visual Studio IDE](#vs) fejlesztési használ, a [MSBuild](http://msbuildbook.com/) semmit a IDE végezhető műveletek automatizálása is használhatja. Fájlok másolása [Web üzembe](#webdeploy) vagy [FTP/FTPS](#ftp) használandó MSBuild is beállíthatja. Webes Deploy is automatizálhatja a sok egyéb telepítési feladatok, például a adatbázisok.

További információt a parancssori telepítési MSBuild használatával az alábbi forrásokban talál:

* [ASP.NET Web telepítésének használata a Visual Studio: parancssor telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). A Visual Studio segítségével Azure telepítésről oktatóanyagok a sorozat tizedik. Használatával a parancssorban után beállításának profilok közzététele a Visual Studióban mutatja.
* [Belül a Microsoft Build motor: MSBuild és a Team Foundation Build](http://msbuildbook.com/). A címjegyzék nyomtatott MSBuild használatát a telepítéshez fejezetekben tartalmazó.

###<a name="powershell"></a>A Windows PowerShell telepítésének automatizálása

A [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)MSBuild vagy FTP-telepítés függvények végezheti el. Az ehhez szükséges lépéseket, ha olyan gyűjteménye, amelyeknek könnyen hívja fel az Azure REST API kezelése Windows PowerShell-parancsmagok is használhatja.

További információt az alábbi forrásokban talál:

* [Csatolt egy GitHub tárházba webes alkalmazások terjesztése](app-service-web-arm-from-github-provision.md)
* [Az SQL-adatbázis webalkalmazást kiépítése](app-service-web-arm-with-sql-database-provision.md)
* [Építse és üzembe microservices előre Azure-ban](app-service-deploy-complex-application-predictably.md)
* Az [épület felhőben való életben alkalmazások az Azure - automatizálása mindent](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). Az E-címjegyzék fejezet, amelyből megtudhatja, hogyan a minta alkalmazás látható az e-címjegyzék használja a Windows PowerShell-parancsfájlokat létrehozása az Azure tesztkörnyezetben és azt telepítése. Az [erőforrások](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) dokumentációt Azure PowerShell mutató hivatkozások szakaszban olvashat.
* A [Windows PowerShell-parancsfájlokat közzététele fejlesztők és a próba-környezetben használja](../vs-azure-tools-publishing-using-powershell-scripts.md). A Windows PowerShell-lel telepítési parancsfájlok, hogy miként Visual Studio hoz létre.

###<a name="api"></a>Környezet, amelyben a .NET-kezelés API automatizálása

C# kód MSBuild vagy FTP-funkciók végrehajtása telepítésének is írhat. Az ehhez szükséges lépéseket, ha az Azure felügyeleti webhely-kezelési funkciók REST API-t is elérheti.

További tudnivalókért lásd: az alábbi forrást:

* [Minden, az Azure-kezelés tárak és a .NET automatizálása](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). A .NET-kezelés API-val és a további dokumentáció mutató hivatkozások bemutatása.

###<a name="cli"></a>Az Azure üzembe parancssori kezelőfelületről Azure

Használhatja a parancssorból Windows, Mac vagy Linux rendszerhez gépeken futó üzembe FTP használatával. Az ehhez szükséges lépéseket, ha Azure REST API segítségével az Azure CLI kezelése is elérheti.

További tudnivalókért lásd: az alábbi forrást:

* [Azure parancs sor eszközök](/downloads/#cmd-line-tools). Portáloldalon a Azure.com parancssori eszköz információt.

###<a name="webdeploy"></a>Webes üzembe parancssorából Deploy

[Webes üzembe](http://www.iis.net/downloads/microsoft/web-deploy) Microsoft-szoftverek, amely nem csak intelligens fájl szinkronizálási funkcióval, de is végrehajtása vagy koordinálása számos egyéb telepítési feladatok, FTP használatakor nem lehet automatikus IIS telepítéshez. Ha például webes üzembe telepítheti a új adatbázis vagy az adatbázis-frissítések együtt a web App alkalmazásban. Webes Deploy is méretűre frissítése egy meglévő webhelyet, mivel ezután másolhatja csak a módosított fájlok szükséges időt. Microsoft Visual Studio és a Team Foundation Server rendelkezik webhely üzembe beépített támogatása, de is használhatja webes üzembe közvetlenül a parancssorból automatizálhatja a telepítés gombra. Webes Deploy parancsok is, de lehet, hogy a tanulási görbe nagy ahhoz.

További tudnivalókért lásd: az alábbi forrást:

* [Egyszerű Web Apps alkalmazások: telepítési](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog által David Ebbo ő írt, így azok könnyebben használata a webes üzembe eszközről.
* [Webhely-telepítő eszközzel](http://technet.microsoft.com/library/dd568996). A Microsoft TechNet webhelyen hivatalos dokumentációt. De még egy jó kiindulási dátummal.
* [Webes telepítése](http://www.iis.net/learn/publish/using-web-deploy). A Microsoft IIS.NET webhelyen hivatalos dokumentációt. De az hasznos kiindulási is dátummal.
* [ASP.NET Web telepítésének használata a Visual Studio: parancssor telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild Visual Studio build motor, és azt is használható a parancssorból webalkalmazások Web Apps alkalmazások terjesztése. Ebben az oktatóanyagban, amely elsősorban a Visual Studio telepítésről sorozat része.

##<a name="nextsteps"></a>Következő lépések

Bizonyos esetekben célszerű engedélyezni szeretné, hogy könnyebben válthat másikba a fejlesztői és az alkalmazás egy gyártási között. További tudnivalókért lásd: [A Web Apps alkalmazások előkészített telepítési](web-sites-staged-publishing.md).

Biztonsági mentési és visszaállítási csomagra helyen, akkor az telepítési munkafolyamatok fontos része. Készítsen biztonsági másolatot az alkalmazás szolgáltatás információt és visszaállítási szolgáltatás, lásd: [Web Apps alkalmazások biztonsági másolatok](web-sites-backup.md).  

Információt arról, hogy miként alkalmazás szolgáltatás telepítési való hozzáférés kezelése a Azure-féle szerepköralapú hozzáférés-vezérlés használatával című témakörben talál [RBAC és Web App közzétételi](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).


 
