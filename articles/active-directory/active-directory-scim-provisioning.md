<properties
    pageTitle="SCIM használatának engedélyezése a felhasználók és csoportok Azure Active Directory-alkalmazások automatikus kiépítési |} Microsoft Azure"
    description="Azure Active Directory-felhasználók és csoportok, amelyeknek van fronted webszolgáltatás által meghatározott a SCIM protokoll specifikáció felülettel alkalmazás vagy identitás tároló automatikusan is kiépítése"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="using-scim-to-enable-automatic-provisioning-of-users-and-groups-from-azure-active-directory-to-applications"></a>Engedélyezése a felhasználók és csoportok az Azure Active Directory alkalmazások automatikus kiépítési SCIM használatával

##<a name="overview"></a>– Áttekintés

Azure Active Directory automatikusan is kiépítése a felhasználók és csoportok szerint webszolgáltatás fronted van definiálva az [SCIM 2.0-s protokoll specifikáció](https://tools.ietf.org/html/draft-ietf-scim-api-19)felülettel alkalmazás vagy identitás tároló. Azure Active Directory létrehozása, módosítása és törlése a hozzárendelt felhasználók és csoportok e majd lefordíthatja a kérések be a cél identitás tárolására, műveletek webszolgáltatásnak kérések küldhet. 

![][1]
*Ábra: Kiépítési az Azure Active Directory-identitás áruház webes szolgáltatáson keresztül*

Ez a képesség használható együtt "[jelenítse meg a saját alkalmazás](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)" lehetőséggel Azure Active Directory lehetővé teszi az egyszeri bejelentkezés és alkalmazásokhoz, adja meg, vagy egy SCIM web Service vannak fronted kiépítési automatikus felhasználói.

Vannak SCIM az Azure Active Directory-két használati eset:

* **Felhasználók és csoportok, amelyek támogatják a SCIM alkalmazások kiépítési** - alkalmazásokat, amelyek SCIM 2.0-s verzióját, és a hitelesítéshez használt OAuth bearer tokenek annak az Azure Active Directory működik.

* **A saját alkalmazások, amely támogatja a többi API-alapú kiépítési kiépítési megoldás** - alkalmazások nem SCIM, hozhat létre egy SCIM végpontot Azure Active Directory SCIM végpontot, és bármilyen fordíthat API az alkalmazás támogatja a felhasználó kiépítési.  Támogatás a SCIM végpont fejlesztésének, elvégezheti a szükséges CLI tárak mintakódok, amely megmutatja, hogyan adja meg a SCIM végpont és SCIM üzenetek fordítása együtt.  

##<a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Felhasználók és csoportok SCIM támogató alkalmazások kiépítése

Azure Active Directory beállítható úgy, hogy a felhasználók automatikusan rendelt létrehozására és a csoportok, hogy az weblapon [tartományok Identitáskezelés 2 (SCIM) rendszer](https://tools.ietf.org/html/draft-ietf-scim-api-19) megvalósítása alkalmazásokat szolgáltatás, és fogadja el a OAuth bearer tokenek hitelesítéshez. A SCIM 2.0-s specifikációja belül alkalmazások ezeknek a követelményeknek kell megfelelnie:

* Támogatja az létrehozását a felhasználóknak, illetve a csoportok szakasz 3.3 SCIM protokoll szerint.  

* Támogatja az módosítása a felhasználók, illetve a csoportok szakasz 3.5.2 SCIM protokoll szerint javítás kéréseivel.  

* Támogatja a retrieving ismert erőforrás szerinti SCIM protokollt 3.4.1 szakaszát.  

*  Támogatja a felhasználóknak, illetve a csoportok szakasz 3.4.2 SCIM protokoll szerint lekérdezése.  Alapértelmezés szerint externalId rendszer megkérdezi a felhasználókat, és a csoportok displayName szerint a rendszer megkérdezi.  

* Felhasználói azonosítója és manager szakaszában 3.4.2 a SCIM protokoll szerint lekérdezése támogatja.  

* Csoportok azonosító és a szakasz 3.4.2 SCIM protokoll szerint tag által lekérdezése támogatja.  

* Fogadja el a SCIM protokoll szerint szakaszban 2.1 hitelesítéshez OAuth bearer tokenek.

Ellenőrizze az alkalmazás-szolgáltató vagy alkalmazás szolgáltatójától dokumentáció ezeknek a követelményeknek való kompatibilitás érdekében a kimutatást.
 
###<a name="getting-started"></a>Első lépések

A fent leírt SCIM profil támogató alkalmazások Azure Active Directory, a "custom" alkalmazás funkció használata az Azure Active Directory-alkalmazás gyűjteményben kapcsolhat össze. Miután létrejött, a Azure Active Directory minden 5 percig tart, ahol azt kérdezi le az alkalmazás SCIM végpont hozzárendelt felhasználók és csoportok, és hoz létre vagy módosítja azokat a hozzárendelés részletek megfelelően fut szinkronizál.

**Csatlakozás egy alkalmazást, amely támogatja az SCIM:**

1.  Egy webböngészőben nyissa meg a Azure kezelőportálja https://manage.windowsazure.com címen.
2.  Tallózással keresse meg **az Active Directory > könyvtár > [a könyvtár] > alkalmazások**, és válassza a **hozzáadása > alkalmazás hozzáadása a gyűjteményből**.
3.  Jelölje ki az **egyéni** lapon, a bal oldalon, írjon be egy nevet az alkalmazáshoz, és kattintson a pipa ikonra, hozzon létre egy alkalmazás objektumot.

![][2]

4.  Az eredményül kapott képernyőn jelölje ki a második **konfigurálása fiók kiépítési** gombra.
5.  A **Kiépítési végpont URL-cím** mezőbe írja be az alkalmazás SCIM végpont URL-CÍMÉT.
6.  Ha a SCIM végpont igényel egy OAuth bearer jogkivonathoz az Azure Active Directory-től különböző kibocsátó, majd másolja a szükséges OAuth bearer jogkivonathoz a **Hitelesítési jogkivonat (nem kötelező)** mezőbe. Az Ez a mező üres, akkor az Azure AD-OAuth bearer jogkivonathoz minden kérésével Azure AD ki fogja tartalmazni. Alkalmazások által használt Azure AD, mint egy idenity szolgáltatóval ellenőrizheti a Azure AD-jogkivonat ki.
7.  Kattintson a **Tovább**gombra, és kattintson a **Teszt indítása** gombra, hogy az Azure Active Directory csatlakozni az SCIM végpontot. Ha a kísérletek sikertelenek, diagnosztikai adatok jelennek meg.  
8.  Ha az alkalmazás sikerült csatlakozni próbál kattintson a **Tovább gombra** a a fennmaradó képernyők, majd kattintson a **kész** való kilépéshez a párbeszédpanelen.
9.  Az eredményül kapott képernyőn jelölje ki a harmadik **Hozzárendelése fiókok** gombra. Az eredményül kapott felhasználók és csoportok szakaszban hozzárendelése a felhasználók vagy csoportok, amelyet az alkalmazás kiépítése.
10. A felhasználók és csoportok kapnak, amikor a **beállítás** lapján a képernyő felső részén.
11. A **Fiók kiépítési**győződjön meg arról, hogy az állapot beállítása a. 
12. A **eszközök**kick-start a kiépítési folyamat **Indítsa újra a fiók kiépítési** kattintással.

Figyelje meg, hogy 5-10 perc lehet eltelnie a kiépítési folyamat elkezdi kérést küld a SCIM végpontot.  Kapcsolatfelvételi összefoglalását megadva az alkalmazás az irányítópult lapon, és a kiépítési tevékenység jelentés és a kiépítési hibák letölthető a címtár-jelentések fülre.

##<a name="building-your-own-provisioning-solution-for-any-application"></a>A saját megoldás kiépítése tetszőleges-alkalmazás létrehozása

Az Azure Active Directory címtárral SCIM webszolgáltatás kapcsolatok létrehozásával engedélyezheti egyszeri bejelentkezéses és az automatikus felhasználói kiépítési gyakorlatilag bármilyen API kiépítési SOAP vagy a többi felhasználó biztosító alkalmazás.

Íme, hogy hogyan működik:

1.  Azure Active Directory [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)nevű közös nyelvi infrastruktúra tár biztosít. Rendszer kiegészítők és a fejlesztők segítségével a tár készíthetnek és helyezhetnek üzembe a SCIM-alapú webes szolgáltatás végpontjának Azure Active Directory csatlakoztatható bármely alkalmazásban identitás áruházból.
2.  Hozzárendelések alkalmazza az webszolgáltatás a szabványos felhasználói séma hozzárendelése a felhasználó séma és a protokoll követel meg az alkalmazást.
3.  Az endpoint URL-cím van regisztrálva az Azure Active Directory alkalmazás gyűjteményben lévő egyéni alkalmazás részeként.
4.  Ez az alkalmazás az Azure Active Directory-felhasználók és csoportok vannak rendelve. Hozzárendelés, frissítéskor bejelentésén szinkronizálható a célalkalmazás a várólista be. A szinkronizálás kezelése a várólista 5 percenként fut.

###<a name="code-samples"></a>Mintakódok

Egyszerűbbé teheti, hogy egy sor olyan [mintákat kód](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) vannak, feltéve, hogy hozzon létre egy SCIM webes szolgáltatás végpontjának és bemutatják, hogy az automatikus kiépítési. Egy minta van, a szolgáltató által fenntartott egy fájlt a felhasználók és csoportok vesszővel tagolt értékek sorát.  A másik pedig a szolgáltató, amely az Amazon Web Services-azonosító és a hozzáférés-kezelés szolgáltatása üzemel.  

**Előfeltételek**

* Visual Studio 2013-at vagy újabb verzió
* [Azure SDK a .NET rendszerhez](https://azure.microsoft.com/downloads/)
* A Windows gépi, amely támogatja az ASP.NET-keretrendszer 4.5 SCIM végpontjának használható. Ezen a számítógépen elérhető a felhőből kell lennie.
* [Az Azure Active Directory prémium a próba-előfizetésére vagy licencelt verziójával készült Azure előfizetéssel](https://azure.microsoft.com/services/active-directory/)
* Az Amazon AWS minta az [AWS eszközkészlet for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)tárai szükséges. Lásd: további információt a minta részét képező fontos fájl

###<a name="getting-started"></a>Első lépések

Az egy SCIM végpontot, ideiglenesen elfogadhatja az Azure Active Directory kiépítési kérései végrehajtásához legegyszerűbben létre és helyezhetnek üzembe a kód mintát, amely a kiépített felhasználók vesszővel tagolt (CSV) fájlba exportálja.

**Minta SCIM végpont létrehozása:**

1.  [Https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) -a kódot minta csomag letöltése
2.  Bontsa ki a csomagot, és helyezze el a Windows számítógépen C:\AzureAD-BYOA-Provisioning-Samples\ például egy helyen.
3.  Ebben a mappában nyissa meg a Visual Studio FileProvisioningAgent megoldás.
4.  Válassza a **Eszközök > tár csomag kezelő > csomag kezelője konzol**, majd hajtsa végre a FileProvisioningAgent projekt oldani a megoldást hivatkozásokat parancsai alatt:

    Telepítés-csomag Microsoft.SystemForCrossDomainIdentityManagement telepítés-csomag Microsoft.IdentityModel.Clients.ActiveDirectory telepítés-csomag Microsoft.Owin.Diagnostics telepítés-csomag Microsoft.Owin.Host.SystemWeb

5.  Építse fel a FileProvisioningAgent projektet.
6.  Indítsa el a parancssor alkalmazást a Windows (rendszergazdaként), és a **CD-re** paranccsal váltson a **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mappában arra a könyvtárra.
7.  Futtassa az alábbi IP-címének vagy tartomány nevét a Windows-gép < ip-cím > lecserélve a parancsot.

    FileAgnt.exe http://<ip-address>:9000 TargetFile.csv

8.  A Windows **Windows beállításai > hálózat és Internet beállítások**, jelölje be a **a Windows tűzfal > Speciális beállítások**, és hozzon létre egy **Bejövő szabályt** , amely lehetővé teszi a port 9000 bejövő elérését.
9.  Ha a Windows gép útválasztó mögött, az útválasztó kell beállítania, hogy végezze el a hálózati hozzáférés fordítási között a port 9000, amely az internethez, és port 9000 a Windows-gépen. Az Azure Active Directory fér hozzá a végpont a felhőben való szükséges.


**A minta SCIM végpont regisztrálása az Azure Active Directory:**

1.  Egy webböngészőben nyissa meg a Azure kezelőportálja https://manage.windowsazure.com címen.
2.  Tallózással keresse meg **az Active Directory > könyvtár > [a könyvtár] > alkalmazások**, és válassza a **hozzáadása > alkalmazás hozzáadása a gyűjteményből**.
3.  Jelölje ki az **egyéni** lapon, a bal oldalon, írjon be egy nevet, például "SCIM próba alkalmazást", és kattintson a pipa ikonra, hozzon létre egy alkalmazás objektumot. Figyelje meg, hogy az alkalmazás objektum létrehozott szeretné megjeleníteni az alkalmazásához az cél azt, akinek kell a kiépítési és az egyszeri bejelentkezés számára és nem csak a SCIM végpont végrehajtása.

![][2]

4.  Az eredményül kapott képernyőn jelölje ki a második **konfigurálása fiók kiépítési** gombra.
5.  A párbeszédpanelen adja meg az internet elérhetővé tett URL-cím és a SCIM végpont portot. Ez a következő lesz valami hasonló http://testmachine.contoso.com:9000 vagy http://<ip-address>:9000/, ahol < ip-cím >-e az interneten elérhetővé tett IP címet.  
6.  Kattintson a **Tovább**gombra, és kattintson a **Teszt indítása** gombra, hogy az Azure Active Directory csatlakozni az SCIM végpontot. Ha a kísérletek sikertelenek, diagnosztikai adatok jelennek meg.  
7.  Ha sikerül a kísérel meg való csatlakozáshoz a webes szolgáltatás, a hátralévő képernyőjén kattintson a **Tovább gombra** , és válassza a **kész** való kilépéshez a párbeszédpanelen.
8.  Az eredményül kapott képernyőn jelölje ki a harmadik **Hozzárendelése fiókok** gombra. Az eredményül kapott felhasználók és csoportok szakaszban hozzárendelése a felhasználók vagy csoportok, amelyet az alkalmazás kiépítése.
9.  A felhasználók és csoportok kapnak, amikor a **beállítás** lapján a képernyő felső részén.
10. A **Fiók kiépítési**győződjön meg arról, hogy az állapot beállítása a. 
11. A **eszközök**kick-start a kiépítési folyamat **Indítsa újra a fiók kiépítési** kattintással.

Figyelje meg, hogy 5-10 perc lehet eltelnie a kiépítési folyamat elkezdi kérést küld a SCIM végpontot.  Kapcsolatfelvételi összegzését megadva az alkalmazás az irányítópult lapon, és a kiépítési tevékenység jelentés és a kiépítési hibák letölthető a címtár-jelentések fülre.

A minta ellenőrzése utolsó lépéseként TargetFile.csv megnyitni a Windows-gépen \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mappában. A kiépítési folyamat futtatása után ezt a fájlt a részletek, az összes hozzárendelt és kiépítéstől a felhasználók és csoportok jeleníti meg.

###<a name="development-libraries"></a>Fejlesztési tárak

A saját webszolgáltatás, amely megfelel a SCIM specifikációja hozzanak először ismerkedjen meg a következő tárak segíti a fejlesztési folyamat felgyorsítása a Microsoft által nyújtott: 

**1:**  Közös nyelvi infrastruktúra-tárak használatra ajánlja fel, hogy infrastruktúra – például a C# alapján nyelven.  A tárak, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), közül deklarálása felületet, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, az alábbi ábrán látható.  A tárak használata a fejlesztők felülettel, amely lehet hivatkozni, általában szolgáltatóként osztály volna végrehajtása.  A tárak engedélyezése a fejlesztői egyszerűen webszolgáltatás, amely megfelel a SCIM specifikációja telepítéshez használni, vagy az Internet Information Services vagy az összes végrehajtható közös nyelvi infrastruktúra-összeállítás belül is.  Hívások átirányítása a szolgáltató, módszerek a fejlesztő által néhány identitás-tárolóban működtetéséhez volna programozott kéréseit az adott webszolgáltatás lesz átalakítva.    

![][3]

**2:** [Express útvonal kezelők](http://expressjs.com/guide/routing.html) érhetők el a node.js kérelem objektumok elemzése hívásokat (SCIM specifikációja által meghatározott), amely egy node.js webszolgáltatás módosításait.     

###<a name="building-a-custom-scim-endpoint"></a>Egy egyéni SCIM végpont létrehozása

A fentebb ismertetett tárak használata esetén a tárak használata fejlesztők üzemeltetheti bármelyik végrehajtható közös nyelvi infrastruktúra-összeállítás, vagy az Internet Information Services belül a szolgáltatások.  Az alábbiakban belül egy végrehajtható összeállítás, a következő címen: http://localhost:9000 szolgáltatás elhelyezésére minta kódot: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  
    
    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Fontos, hogy ne feledje, hogy ez a szolgáltatás kell egy HTTP címet és a kiszolgáló hitelesítési tanúsítványt, amelyek a legfelső szintű hitelesítésszolgáltató, az alábbiak egyikét: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Kiszolgálói hitelesítési tanúsítvány is kötött olyan portot egy Windows rendszerű állomás a hálózati rendszerhéj segédprogram használata a következőhöz hasonlóan: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  
 
A tanúsítványkivonat argumentumban megadott érték az alábbiakban az ujjlenyomatot a tanúsítvány közben a appid argumentumban megadott érték egy tetszőleges globálisan egyedi azonosítója.  

Tárolni a belül Internet Information Services szolgáltatás, a Fejlesztőeszközök volna hozzon létre egy közös nyelvi infrastruktúra kód tár összeállítás az indítási nevű az alapértelmezett névtér az összeállítás az osztály.  Íme egy mintája szerepel az ilyen osztály: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

###<a name="handling-endpoint-authentication"></a>Végpont hitelesítési kezelése

Azure Active Directory érkező kérések egy 2.0-s OAuth bearer jogkivonathoz tartalmazza.   Bármely felkért szolgáltatás kell hitelesíteni a kibocsátó, hogy az Azure Active Directory nevében a várható Azure Active Directory-bérlő Azure Active Directory Graph webszolgáltatás eléréséhez.  A jogkivonat, a kibocsátó egy iss állítást, például "iss" jelölt: "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".  Ebben a példában a felelős érték alapcímének https://sts.windows.net, amely azonosítja Azure Active Directory a kibocsátó a relatív cím szakaszában, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 ugyan a nevében, amely a token állította ki az Azure Active Directory-bérlő egyedi azonosítót kaphat.  Ha a token elérni az Azure Active Directory Graph webszolgáltatás bocsátotta, akkor szolgáltatás, 00000002-0000-0000-c000-000000000000 azonosítója a értéket a token és igény kell megadni.  

A fejlesztők használata a közös nyelvi infrastruktúra-tárak létrehozásának SCIM szolgáltatás a Microsoft által nyújtott hitelesíthet kérései Azure Active Directory, a Microsoft.Owin.Security.ActiveDirectory csomag használata az alábbi lépésekkel: 

**1:**  A szolgáltató végrehajtása a Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tulajdonságot úgy, hogy a szolgáltatás indulásakor hívható módszer visszaáll: 

    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }

**2:**  Ez a módszer, hogy az minden hitelesített szem előtt a megadott bérlői webhelyet, az Azure Active Directory Graph webszolgáltatás eléréséhez nevében Azure Active Directory által kibocsátott jogkivonat, a szolgáltatás végpontok kérésének hozzáadása a következő kódot: 

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }

##<a name="user-and-group-schema"></a>Felhasználók és a csoport séma

Azure Active Directory is kiépítése kétféle típusú erőforrás SCIM webes szolgáltatásokhoz.  Ilyen típusú erőforrás a felhasználók és csoportok ablakot.  

Felhasználói erőforrások azonosítjuk a séma azonosítóját, a protokoll specifikáció található urn: ietf:params:scim:schemas:extension:enterprise:2.0:User: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Az alapértelmezett leképezést, a felhasználók az Azure Active Directory urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrások tulajdonságainak attribútumai megadva 1, a táblázat alatt.  

Csoport erőforrások azonosítja a séma azonosító http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Táblázat 2, jeleníti meg az alábbi attribútumai közül csoportokat az Azure Active Directory attribútumok http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group erőforrások az alapértelmezett leképezést.  

###<a name="table-1-default-user-attribute-mapping"></a>Tartalomjegyzék 1: Alapértelmezett felhasználói attribútum hozzárendelésének

| Azure Active Directory-felhasználók | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| ------------- | ------------- |
| IsSoftDeleted | aktív |
| displayName | displayName |
| Telefax-TelephoneNumber | [típus eq "fax"] phoneNumbers .value |
| givenName | name.givenName |
| Munkakör | cím |
| levelek | e-mailek [típus eq "munka"] .value |
| mailnickname-beli | externalId |
| kezelő | kezelő |
| mobil | [típus eq "mobile"] phoneNumbers .value |
| Objektumazonosító | azonosító |
| Irányítószám | címek [típus eq "munka"] .postalCode |
| a proxykiszolgáló-címei | e-mailek [Írja be a "egyéb" eq]. Érték |
| fizikai-kézbesítési-OfficeName | a címek [Írja be a "egyéb" eq]. Formázott |
| streetAddress | címek [típus eq "munka"] .streetAddress |
| Vezetéknév | name.familyName |
| Telefonszám | [típus eq "munka"] phoneNumbers .value |
| felhasználó-PrincipalName | Felhasználónév |


###<a name="table-2-default-group-attribute-mapping"></a>Táblázat 2: Alapértelmezett csoport attribútum hozzárendelése

| Azure Active Directory-csoportnak | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| ------------- | ------------- |
| displayName | externalId |
| levelek | e-mailek [típus eq "munka"] .value |
| mailnickname-beli | displayName |
| a tagok | a tagok |
| Objektumazonosító | azonosító |
| proxyAddresses | e-mailek [Írja be a "egyéb" eq]. Érték |


##<a name="user-provisioning-and-de-provisioning"></a>Felhasználói kiépítési és vonja kiépítése

Az az alábbi ábrán az üzeneteket, hogy Azure Active Directory küld egy SCIM szolgáltatás kezelése az életciklus identitást egy másik felhasználó tárolni.  A diagram is látható, hogyan SCIM szolgáltatás használata a közös nyelvi infrastruktúra-tárak létrehozásának e szolgáltatások a Microsoft által nyújtott végrehajtott fog lefordítása azokat a kérelmeket hívások átirányítása egy szolgáltatót a módszereket.  

![][4]
*Ábra: Felhasználói kiépítési és vonja vissza a sorozat kiépítése*

**1:**  Azure Active Directory lekérdezéseit a szolgáltatást a felhasználó az Azure Active Directory-egy felhasználó mailnickname-beli attribútumérték egyező externalId attribútum értékkel.  A lekérdezés HTTP kérelem ehhez hasonló, itt megadhatja jyoung-e egy mintája szerepel egy mailnickname egy felhasználó az Azure Active Directory-beli fog kifejezhető: 

    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...

Ha a szolgáltatás készült használata a közös nyelvi infrastruktúra-tárak végrehajtási SCIM szolgáltatások a Microsoft által nyújtott, majd a kérelem fog értelmezhető hívást kezdeményez, a szolgáltató lekérdezésben metódusát.  Ez a módszer aláírását a következő: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);

Az alábbiakban a Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters felület meghatározása: 

    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }
    
    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }

Ha a lekérdezés a felhasználó az egy adott értéket a externalId attribútum az előző példában a lekérdezés módszerrel átadott argumentumot értékek a következők szerint lesz: 

* a paraméterek. AlternateFilters.Count: 1
* a paraméterek. AlternateFilters.ElementAt(0). AttributePath: "externalId"
* a paraméterek. AlternateFilters.ElementAt(0). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
* a paraméterek. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"
* correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Kérelemazonosító"] 

**2:**  Ha a választ, amelyekkel a szolgáltatást az Azure Active Directory-egy felhasználó a mailnickname-beli attribútumérték egyező externalId attribútumérték rendelkező felhasználó azok a felhasználók nem ad vissza, majd Azure Active Directory kérni fogja, hogy a szolgáltatás kiépítése egy, az Azure Active Directory megfelelő felhasználó.  Íme egy példa megkeresést: 

    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}

A végrehajtási SCIM szolgáltatások a Microsoft által nyújtott közös nyelvi infrastruktúra-tárak volna kérelem lefordítása hívást kezdeményez, a szolgáltató létrehozása metódusát.  A létrehozás módszer az aláírás foglalja magában: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);

Egy felhasználó kiépítése kérelem esetén az erőforrás argumentum értéke lesz a Microsoft.SystemForCrossDomainIdentityManagement egy példánya. Core2EnterpriseUser kódokban, a Microsoft.SystemForCrossDomainIdentityManagement.Schemas tárban.  Ha az értekezlet-összehívást a felhasználó kiépítése sikerül, majd a módszerrel végrehajtásának várhatóan vissza egy példányának a a Microsoft.SystemForCrossDomainIdentityManagement. Az egyedi azonosító az újonnan kiépítéstől felhasználó azonosító tulajdonsága értékkel Core2EnterpriseUser osztály.  

**3:**  A felhasználó által egy SCIM fronted identitás tárolóban található ismert frissítéséhez az Azure Active Directory igénylésével, hogy a felhasználó aktuális állapotát a szolgáltatásból az alábbihoz hasonló kérelmének folytatódik: 

    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...

A beépített használata a közös nyelvi infrastruktúra-tárak végrehajtási SCIM szolgáltatások a Microsoft által nyújtott szolgáltatás a kérelem számíthatók hívást kezdeményez, a szolgáltató lekérés metódusát.  Az alábbiakban az aláírást a lekérés módszer: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);
    
    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }

Az esetében a felhasználó aktuális állapotát beolvasásához kérelmének az előző példában az értékeket a paraméterek argumentum értéke az objektum tulajdonságai, a következők szerint lesz: 

* Azonosító: "54D382A4-2050-4C03-94D1-E769F1D15682"
* SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

**4:**  Ha egy hivatkozást attribútum frissíteni, és Azure Active Directory lekérdezéseit határozza meg, függetlenül attól aktuális értékét a hivatkozás attribútum, az Identitáskezelés áruházban fronted szolgáltatás már a szolgáltatást, hogy az Azure Active Directory-attribútum értéke megegyezik.  Ha a felhasználók a csak, amelyek az aktuális érték fog lekérdezett ily módon attribútum a kezelő attribútum.  Íme egy példa határozza meg, hogy a kezelő attribútum egy adott felhasználói objektum jelenleg értéke egy bizonyos kérelem: 

    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...

Az érték az attribútumok lekérdezési paraméter id, azt jelzi, hogy, ha létezik a user objektumban, amely eleget tesz a kifejezést a Szűrés lekérdezési paraméter értéke, majd a szolgáltatás várhatóan urn: ietf:params:scim:schemas:core:2.0:User vagy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrással, többek között csak az adott erőforrás-azonosító attribútum értékének válaszolni.  Természetesen a azonosító attribútum értékét a lekérdező ismert – a Szűrés lekérdezési paraméter; értéke szerepel. az ezt kérő célja ténylegesen a minimális egy erőforráshoz megadott kéri, hogy a filter kifejezésére, függetlenül attól, például olyan objektumokat létezik feltüntetése felel meg.   

A szolgáltatás készült, a végrehajtási SCIM szolgáltatások a Microsoft által nyújtott közös nyelvi infrastruktúra-tárak használata, ha kattintson a kérelem fog értelmezhető hívást kezdeményez, a szolgáltató lekérdezésben metódusát.  A paraméterek argumentum értéke a megadott objektum tulajdonságainak értékét a következők: 

* a paraméterek. AlternateFilters.Count: 2
* a paraméterek. AlternateFilters.ElementAt(x). AttributePath: "azonosító"
* a paraméterek. AlternateFilters.ElementAt(x). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
* a paraméterek. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
* a paraméterek. AlternateFilters.ElementAt(y). AttributePath: "manager"
* a paraméterek. AlternateFilters.ElementAt(y). ÖsszehasonlítóOperátor: ComparisonOperator.Equals
* a paraméterek. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
* a paraméterek. RequestedAttributePaths.ElementAt(0): "azonosító"
* a paraméterek. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

Ebben az esetben lehet, hogy az index x értéke 0 és 1, lehet, az index y értékének vagy lehet, hogy az x értéke 1 és lehet, hogy az az y érték 0, attól függően, hogy a Szűrés lekérdezési paraméter kifejezések sorrendjének.   

**5:**  Az alábbiakban a példa egy kérelmet Azure Active Directory-SCIM szolgáltatás felhasználó frissítése: 

    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}

A Microsoft közös nyelvi infrastruktúra tárak SCIM szolgáltatások végrehajtásához szeretné a kérelem lefordítása hívást kezdeményez, a szolgáltató a frissítés módja.  Ez a módszer aláírását a következő: 

    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }
    
    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }
      
      public string Value
      { get; set; }
    }



Kérelem frissítéséhez egy felhasználó az előző példában, ha a javítás argumentum értéke a megadott objektum lesz tulajdonság ezeket az értékeket: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"
* (Mint PatchRequest2 PatchRequest). Operations.Count: 1
* (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). OperationName: OperationName.Add
* (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Path.AttributePath: "manager"
* (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.Count: 1
* (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Hivatkozás: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
* (Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Érték: 2819c223-7f76-453a-919d-413861904646

**6:**  Vonja kiépítése egy felhasználó az identitás-SCIM szolgáltatás által fronted áruházból, az Azure Active Directory küld egy kérelmet az alábbihoz hasonló: 

    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    
Ha a szolgáltatás készült használata a közös nyelvi infrastruktúra-tárak végrehajtási SCIM szolgáltatások a Microsoft által nyújtott, majd a kérést fog értelmezhető hívást kezdeményez, a szolgáltató törlés metódusát.   Ez a módszer az aláírás foglalja magában: 

    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
 
Az objektumot adni a resourceIdentifier argumentum értéke lesz a következő tulajdonság értékeket, a fenti példa vonja vissza a felhasználó kiépítése kérés esetén: 

* ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
* ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"

##<a name="group-provisioning-and-de-provisioning"></a>Csoport kiépítési és vonja kiépítése

Az az alábbi ábrán az üzeneteket, hogy Azure Active Directory küld egy SCIM szolgáltatás kezelése az életciklus egy másik identitást csoport tárolni.  Azokat az üzeneteket a felhasználók háromféleképpen vonatkozó üzeneteket különböznek: 

* A csoport erőforrás séma mint http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group azonosítania kell magát.  
* Csoportok beolvasásához kérések fog kötni, hogy a tagok attribútum bármely válasz arra a megadott erőforrás alól.  
* Határozza meg, hogy rendelkezik-e egy adott értéket egy hivatkozás attribútum kérések kapcsolatos a tagok attribútum kérések lesz.  

![][5]
*Ábra: Csoport kiépítési és vonja vissza a sorozat kiépítése*

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Automatizálhatja a felhasználói kiépítési/megszüntetés szoftver-alkalmazás](active-directory-saas-app-provisioning.md)
- [A felhasználó kiépítéséhez attribútum hozzárendelések testreszabása](active-directory-saas-customizing-attribute-mappings.md)
- [Az attribútum hozzárendelések kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Hatókör meghatározása felhasználói kiépítési szűrői](active-directory-saas-scoping-filters.md)
- [Fiókbeállítások a kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
- [Integrálhatja a szoftver alkalmazások ismertető oktatóanyagok listája](active-directory-saas-tutorial-list.md)


    
<!--Image references-->
[1]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
