<properties
    pageTitle="Speciális konfigurációs és bővítmények Azure alkalmazás szolgáltatás webalkalmazás"
    description="XML-dokumentum Transformation(XDT) deklarációs használja, a következő fájl az Azure alkalmazás szolgáltatás webalkalmazásban átalakítása, és a személyes-bővítmények engedélyezése az egyéni felügyeleti műveleteket."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Speciális konfigurációs és bővítmények Azure alkalmazás szolgáltatás webalkalmazás

[XML-dokumentum átalakítása](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) deklarációs használatával átalakíthatja a [következő](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) fájlt az Azure-App szolgáltatásban webalkalmazásban. XDT deklarációs segítségével saját bővítmények engedélyezése az egyéni web app felügyeleti műveleteket. Ez a cikk a minta PHP kezelő web app bővítmény, amely lehetővé teszi, hogy egy webes felületén keresztül PHP-beállítások kezelése tartalmazza.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Speciális konfigurációs következő keresztül
Az alkalmazás szolgáltatás platform web app konfigurációs rugalmasság és a vezérlő biztosít. Bár a szabványos IIS következő konfigurációs fájl szerkesztésre közvetlen alkalmazás szolgáltatás nem érhető el, a platform támogatja az XML dokumentum átalakítása (XDT) alapú deklaráció következő átalakítás modell.

Kihasználhatja az átalakítás ezt a funkciót, akkor hozzon létre egy ApplicationHost.xdt fájlt XDT tartalmak és a webhely a [Kudu konzol](https://github.com/projectkudu/kudu/wiki/Kudu-console)legfelső szintű (d:\home\site) mellett. Szükség lehet a Web App módosítások érvénybelépéséhez indítsa újra.

A következő applicationHost.xdt minta új egyéni környezeti változó hozzáadása egy PHP 5.4-es használó web app jeleníti meg.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


A naplófájl átalakítás állapota és részletes FTP legfelső szintjén a LogFiles\Transform érhető el.

További példák olvassa el a [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)című témakört.

**Megjegyzés:**<br />
Modulok csoportban a lista elemeit `system.webServer` nem lehet eltávolítani vagy rendelni, de kiegészítések az is lehetséges.


##<a id="extend"></a>A webalkalmazás kiterjesztése

###<a id="overview"></a>A magánjellegű web app bővítmények áttekintése

Alkalmazás szolgáltatás támogatja a web app bővítmények felügyeleti műveleteket bővítési pontként. Erre valójában alkalmazás szolgáltatás platform funkciókkal megvalósítása előre a telepített bővítmények. Nem lehet módosítani a platform előre a telepített bővítmények, miközben létrehozása, és saját bővítmények a saját webalkalmazás konfigurálása. Ez a funkció is XDT deklarációs támaszkodik. A fő lépéseket a magánjellegű web app kiterjesztés létrehozásához a következők:

1. Alkalmazás kiterjesztés **tartalom**kijelző: bármilyen szolgáltatás alkalmazás által támogatott webalkalmazás létrehozása
2. A Web app kiterjesztés **deklaráció**: hozzon létre egy ApplicationHost.xdt fájlt
3. A Web app bővítmény **telepítési**: tartalom elhelyezése a SiteExtensions mappában`root`

Belső hivatkozások a webalkalmazás az alkalmazás elérési útját a ApplicationHost.xdt fájl a megadott viszonyított elérési utat kell mutatnia. Bármilyen módosítást a ApplicationHost.xdt fájlt a web app Lomtár szükséges.

**Megjegyzés**: további információt az alábbi főbb elemeinek [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions)címen érhető el.

A részletes hozhat létre, és egy privát web app bővítmény engedélyezésével lépései bemutatják, hogy szerepel. A következő PHP-kezelő például forráskód letölthető [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Web app bővítmény példa: PHP-kezelő

PHP felügyelője egy webes alkalmazás bővítmény, amely lehetővé teszi, hogy a web app rendszergazdák megtekintése és a nem kell PHP .ini fájlok közvetlenül módosítása webes felületéről PHP-beállításainak konfigurálása. Közös konfigurációs fájlok PHP-programot a fájlok csoportban található php.ini fájl olyan, és a. user.ini fájl a web App alkalmazásban a legfelső szintű mappájában található. Php.ini fájl nem közvetlenül szerkeszthetők, az alkalmazás szolgáltatás platformon, mivel a PHP-kezelő bővítmény használja a. user.ini fájl beállítást a változtatásokat.

####<a id="PHPwebapp"></a>A PHP-kezelő webalkalmazás

Az alábbiakban látható a kezelő PHP telepítésének kezdőlapjára:

![TransformSitePHPUI][TransformSitePHPUI]

Amint látható, a web app kiterjesztéssel van egy normál webes alkalmazáshoz hasonlóan, de egy további ApplicationHost.xdt fájlt a legfelső szintű mappába helyezi a web App (többet szeretne tudni a ApplicationHost.xdt fájl érhetők el a következő szakaszban, a jelen cikk).

A PHP-kezelő bővítmény jött létre, a Visual Studio ASP.NET MVC 4 webalkalmazás sablonnal. A következő nézet megoldás Intézőből a PHP-kezelő bővítmény felépítésének jeleníti meg.

![TransformSiteSolEx][TransformSiteSolEx]

Fájl I/O szükséges egyetlen speciális logikát is jelezheti, ahol az a web app wwwroot könyvtárában található. Ahogy a következő példa látható, a környezet változó "KEZDŐLAP" jelzi a web app legfelső szintű elérési út, és wwwroot elérési hozzáfűzi az "site\wwwroot" által is alakítható ki:

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Után a könyvtár elérési útját, a hagyományos fájl bemeneti és kimeneti műveletet írási és olvasási fájlok is használhatja.

Amire érdemes ügyelni web app kiterjesztésű egy pont belső hivatkozások kezelése tekinti.  A HTML-fájlokban abszolút elérési út adni a web App belső hivatkozások, ha összes hivatkozást, győződjön meg ezeket a hivatkozásokat, a bővítmény nevét a legfelső szintű vannak $a. Ez szükséges, mert a legfelső szintű a bővítmény most "/`[your-extension-name]`és" nem csak "/", tehát minden belső hivatkozások frissíteni kell megfelelően. Tegyük fel, hogy a kód tartalmazza a következő hivatkozást:

`"<a href="/Home/Settings">PHP Settings</a>"`

A hivatkozás egy webes alkalmazás bővítmény része, ha a hivatkozást a következő formátumban kell:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Dolgozhat a követelmény körül vagy használatával csak relatív elérési utak, a webalkalmazásban, illetve ASP.NET-alkalmazások használata a `@Html.ActionLink` módszer, amely a megfelelő hivatkozásaira létrehoz.

####<a id="XDT"></a>A applicationHost.xdt fájl

A web app-bővítmény a kódot a %HOME%\SiteExtensions Ugrás\[a bővítmény név]. Azt fogja hívja fel a bővítmény legfelső szintű.  

A web app kiterjesztése a következő fájllal regisztrálásához kell helyezni a bővítmény legfelső szintű ApplicationHost.xdt nevű fájl. A ApplicationHost.xdt fájl tartalmát a következőképpen kell:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

Választotta a bővítmény nevét a név van, hogy a bővítmény legfelső szintű mappa ugyanazt a nevet.

Ez egy új alkalmazás elérési hozzáadása hatása a `system.applicationHost` webhelyek listájában a SCM webhelyet. A SCM webhely, azaz a webhely felügyelete végpont adott access hitelesítő adatokkal. Az URL-cím van `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>A webes alkalmazás bővítmény telepítése

A web app bővítmény telepítéséhez az FTP használatával a webalkalmazás az összes fájl másolása a `\SiteExtensions\[your-extension-name]` a web app, amelyen a bővítmény telepítése kívánt mappájában.  Győződjön meg arról, másolhatja a ApplicationHost.xdt ezen a helyen. Indítsa újra a web App alkalmazásban ahhoz, hogy a bővítmény.

Látnia kell a web app-bővítmény a megtekintéséhez:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Figyelje meg, hogy az URL-cím hasonlít csak az URL-címet a webalkalmazás azzal a különbséggel, hogy HTTPS használ, és a ".scm" tartalmazza.

Lehetséges a webalkalmazás az összes magánjellegű (nincs előre telepítve) bővítmények tiltását fejlesztés és nyomozásokban kulccsal-alkalmazás beállításainak hozzáadásával `WEBSITE_PRIVATE_EXTENSIONS` és egy értéket a `0`.

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
