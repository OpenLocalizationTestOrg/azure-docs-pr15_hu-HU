<properties 
    pageTitle="Egy webalkalmazás létrehozása az Azure alkalmazás szolgáltatás az Azure SDK Java" 
    description="Megtudhatja, hogy miként Azure alkalmazás szolgáltatás programozás útján az Azure SDK Java egy webalkalmazás létrehozása." 
    tags="azure-classic-portal"
    services="app-service\web" 
    documentationCenter="Java" 
    authors="donntrenton" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="02/25/2016" 
    ms.author="v-donntr"/>


# <a name="create-a-web-app-in-azure-app-service-using-the-azure-sdk-for-java"></a>Egy webalkalmazás létrehozása az Azure alkalmazás szolgáltatás az Azure SDK Java

<!-- Azure Active Directory workflow is not yet available on the Azure Portal -->

## <a name="overview"></a>– Áttekintés

Ez a forgatókönyv bemutatja, hogyan létrehozása az Azure SDK webalkalmazást létrehozása az [Azure alkalmazás szolgáltatás][]Java-alkalmazást, majd az alkalmazás telepítéséhez. Két részből áll:

- 1 rész bemutatja, hogyan létrehoz egy webalkalmazás Java-alkalmazások létrehozásához.
- Kijelző 2 bemutatja, hogyan hozhat létre egy egyszerű JSP "Helló, világ" alkalmazást, majd használata FTP-ügyfél alkalmazás szolgáltatás szeretne telepíteni, kódot.


## <a name="prerequisites"></a>Előfeltételek

### <a name="software-installations"></a>Szoftver telepítések

Ez a cikk a AzureWebDemo alkalmazás kód készült Azure Java SDK 0.7.0, amely telepíthető használata a [Webes Platform telepítő][] (WebPI) használatával. Ezenkívül ellenőrizze, hogy az [Azure eszközkészlete Holdas][]legújabb verzióját használja-e. Telepítése után a SDK csomagjában talál, a függőségek, a Holdas projekt frissítése **Frissítés parancsával** futtatásával **Maven tesztelése Tárházakban**található, majd vegye fel újból a **függőségek** ablakban minden csomag legújabb verzióját. Gombra kattintva ellenőrizheti a szoftvert a Holdas verziójának **Súgó > telepítéssel kapcsolatos részleteket**; rendelkeznie kell legalább az alábbiakat:

- Microsoft Azure-tárak Java 0.7.0.20150309 csomag
- Java 4.4.2.20150219 EE fejlesztőknek IDE Holdas


### <a name="create-and-configure-cloud-resources-in-azure"></a>Létrehozása és konfigurálása a felhőben erőforrások Azure-ban

Ez az eljárás megkezdése előtt kell aktív Azure verzióra szóló előfizetése, és állítsa be az Active Directory (Active Directory) a Azure alapértelmezett.


### <a name="create-an-active-directory-ad-in-azure"></a>Azure Active Directory (AD) létrehozása

Ha még nem rendelkezik az Active Directory (AD) az Azure-előfizetésben, jelentkezzen be az [Azure klasszikus portál][] Microsoft-fiókjával. Ha több előfizetéssel rendelkezik, kattintson az **előfizetések elemre** , és válassza ki az alapértelmezett könyvtár az ehhez a projekthez használni kívánt előfizetés. Kattintson az **Alkalmaz** , hogy előfizetése nézetre szeretne váltani.

1. Jelölje ki **Az Active Directory** , a bal oldali menüben. **Kattintson az új > könyvtár > egyéni létrehozása**.

2. **Címtár hozzáadása**jelölje be az **Új könyvtár létrehozása**.

3. A **név**mezőbe írja be a címtár nevét.

4. A **tartomány**adja meg a tartománynév. Egyszerű tartomány neve, amely alapértelmezés szerint a címtárában; együtt szerepel az űrlap van `<domain_name>.onmicrosoft.com`. Tetszés szerinti nevet adhat meg a könyvtár nevét vagy egy másik tartománynév tulajdonjogának alapján. Később a szervezete már használ egy másik tartománynév is hozzáadhat.

5. **Ország vagy régió**jelölje ki a területi beállítások.

AD a további tudnivalókért lásd: [Mi az, hogy az Azure Active directory][]?


### <a name="create-a-management-certificate-for-azure"></a>Azure Management tanúsítvány létrehozása

A Java Azure SDK management Tanúsítványok használja az Azure előfizetések hitelesítést végezni. Ezek a X.509 v3 tanúsítványok használatával előfizetés erőforrások kezelése az előfizetés tulajdonosa nevében eljáró a szolgáltatás felügyeleti API-t használó ügyfélalkalmazás hitelesítést végezni.

Az eljárás a kód önaláírt tanúsítványt használja az Azure hitelesítést végezni. Ez az eljárás szeretne tanúsítvány létrehozása és előre feltölteni az [Azure klasszikus portálon][] . Ez a következő lépésekből áll:

- Az ügyfél-tanúsítvány, amely PFX fájl készítése és a helyben menteni.
- Adatkezelési tanúsítvány (CER kiterjesztésű) készítése a PFX fájlból.
- Töltse fel a CER fájl Azure-előfizetéséhez.
- A PFX-fájl konvertálása JKS, mert Java használja ezt a formátumot hitelesíteni a tanúsítványok használatával.
- Írja be a helyi JKS fájlhoz az alkalmazás hitelesítési kódot.

Ez az eljárás befejezésekor a CER tanúsítvány elhelyezkednek az Azure-előfizetése, és a JKS tanúsítvány a helyi meghajtón elhelyezkednek. Adatkezelési tanúsítványok kapcsolatos további tudnivalókért olvassa el a [tölteni a Azure tartozó adatkezelési tanúsítvány][]című témakört.


#### <a name="create-a-certificate"></a>Tanúsítvány létrehozása

Létrehozhat saját önaláírt tanúsítványt, nyisson meg egy parancs konzolt az operációs rendszerre, és futtassa az alábbi parancsokat.

> **Megjegyzés:**  A számítógépen, amelyen a parancsot futtatja a JDK telepítve kell rendelkeznie. A keytool elérési útja is, a helyet, ahol a JDK telepítése függ. További tudnivalókért lásd: online Java-dokumentumokban [billentyű és a tanúsítvány kezelése eszközt (keytool)][] .

A .pfx fájl létrehozása:

    <java-install-dir>/bin/keytool -genkey -alias <keystore-id>
     -keystore <cert-store-dir>/<cert-file-name>.pfx -storepass <password>
     -validity 3650 -keyalg RSA -keysize 2048 -storetype pkcs12
     -dname "CN=Self Signed Certificate 20141118170652"

A .cer fájl létrehozása:

    <java-install-dir>/bin/keytool -export -alias <keystore-id>
     -storetype pkcs12 -keystore <cert-store-dir>/<cert-file-name>.pfx
     -storepass <password> -rfc -file <cert-store-dir>/<cert-file-name>.cer

Ha:

- `<java-install-dir>`a könyvtár elérési útját, amelyben Java telepítve van.
- `<keystore-id>`az adott keystore bejegyzés azonosítója (például `AzureRemoteAccess`).
- `<cert-store-dir>`a könyvtár elérési útját, amelyben a tanúsítványok tárolni szeretné van (például `C:/Certificates`).
- `<cert-file-name>`a tanúsítvány fájl neve (például `AzureWebDemoCert`).
- `<password>`a úgy dönt, hogy a tanúsítvány; védelme jelszóval legalább 6 karakterből kell lennie. Bár ez nem ajánlott nem a jelszót adhat meg.
- `<dname>`alias társítandó X.500 megkülönböztető név, és a kibocsátó és a tárgy mezőt az önaláírt tanúsítványt lesz.

További tudnivalókért lásd: [Azure tartozó adatkezelési tanúsítvány tölteni][].


#### <a name="upload-the-certificate"></a>Töltse fel a tanúsítvány

Önaláírt tanúsítvány feltöltése Azure, nyissa meg a **Beállítások** lap a klasszikus portálon, majd kattintson a **Kezelés tanúsítványok** fülre. Kattintson a **Feltöltés** elemre a lap alján, és keresse meg a létrehozott CER fájl helyét.


#### <a name="convert-the-pfx-file-into-jks"></a>A PFX-fájl konvertálása JKS

Az a Windows parancssort (futtatása rendszergazdaként), CD-hez, a címtárhoz tartalmazó a tanúsítványok, és futtassa a következő parancsot, ahol `<java-install-dir>` a címtárban, amelyben meg Java a számítógépen telepítve van:

    <java-install-dir>/bin/keytool.exe -importkeystore
     -srckeystore <cert-store-dir>/<cert-file-name>.pfx
     -destkeystore <cert-store-dir>/<cert-file-name>.jks
     -srcstoretype pkcs12 -deststoretype JKS

1. Amikor a rendszer kéri, írja be a cél keystore jelszót; Ez a jelszót az JKS fájl lesz.

2. Amikor a rendszer kéri, írja be a forrás keystore jelszót; Ez a PFX fájl a megadott jelszót.

A két jelszavak nem rendelkezik azonosnak kell lennie. Bár ez nem ajánlott nem a jelszót adhat meg.


## <a name="build-a-web-app-creation-application"></a>Egy webalkalmazás létrehozása alkalmazás összeállítása

### <a name="create-the-eclipse-workspace-and-maven-project"></a>Hozza létre a Holdas munkaterület és a Project maven tesztelése

Ebben a részben létrehozása a munkaterületen, és az alkalmazás létrehozási webalkalmazáshoz, AzureWebDemo nevű maven tesztelése projekt.

1. Hozzon létre egy új maven tesztelése projektet. Kattintson a **Fájl > Új > a Project maven tesztelése**. A **Projekt maven tesztelése**jelölje ki az **egyszerű projekt létrehozása** és **használata alapértelmezett munkaterületi hely**.

2. A második **Új maven tesztelése projekt**lapon adja meg a következőket:

    - Csoport azonosítója:`com.<username>.azure.webdemo`
    - Eltérés azonosító: AzureWebDemo
    - Verzió: 0.0.1-SNAPSHOT
    - Csomagolása: üveg
    - Név: AzureWebDemo

    Kattintson a **Befejezés gombra**.

3. Nyissa meg az új projekt pom.xml fájlt Projektböngésző. Jelölje ki a **függőségek** fülre. Mivel ez egy új projektet, nincs csomag még felsorolása.

4. Nyissa meg a maven tesztelése Tárházakban nézetben. **Ablakban kattintson a > nézet megjelenítése > más > maven tesztelése > maven tesztelése Tárházakban** , és kattintson az **OK gombra**. Kattintson az IDE alján a **Maven tesztelése Tárházakban** nézet jelenik meg.

5. **Globális Tárházakban**, kattintson a jobb gombbal a **központi** adattárban, és nyissa meg **Újraépítéséhez Index**.

    ![][1]
    
    Ezt a lépést is eltarthat néhány percig, amíg a kapcsolat sebességétől függően. Ha ismét létrehozza az indexet, meg kell jelennie a Microsoft Azure-csomagok a maven tesztelése **központi** tárban tárolnak.

6. **Függőségek**kattintson a **Hozzáadás**gombra. Adja meg **Az Enter csoport azonosító...** `azure-management`. Jelölje ki a csomagok dátumalapértékhez kezelése és alkalmazás szolgáltatás Web Apps alkalmazások kezelése:

        com.microsoft.azure  azure-management
        com.microsoft.azure  azure-management-websites

    > **Megjegyzés:** Ha egy új verzió kiadása után frissít a függőségeket, vegye fel újból a függőségeket a listában szereplő minden egyes szüksége.
    > Kattintson a **Hozzáadás** gombra, és válassza a Minden függőség, megjelenik az új verziószámmal **függőségek** listában.

Kattintson az **OK gombra**. Az Azure csomagok majd a **függőségek** listájában jelennek meg.


### <a name="writing-java-code-to-create-a-web-app-by-calling-the-azure-sdk"></a>Java kódírás hívja fel az Azure SDK webes alkalmazás létrehozása céljából

Ezután írja be a kódot, amely felhívja a Azure Java SDK az alkalmazás szolgáltatás webalkalmazás létrehozása API-khoz.

1. Hozzon létre egy Java osztály a bejegyzés fő pont kódot tartalmaznak. A Projektböngésző, kattintson a jobb gombbal a projekt csomópontra, és válassza ki **Új > osztály**.

2. **Új Java osztály**, nevezze el az osztály `WebCreator` és a **nyilvános statikus érvénytelenítése fő** jelölőnégyzetet. A kijelölés jelenjen meg az alábbi képlettel történik:

    ![][2]

3. Kattintson a **Befejezés gombra**. A WebCreator.java fájl Projektböngésző jelenik meg.


### <a name="calling-the-azure-api-to-create-an-app-service-web-app"></a>Hívja fel az Azure API-szolgáltatási alkalmazás webalkalmazás létrehozása


#### <a name="add-necessary-imports"></a>Szükséges import hozzáadása

Az WebCreator.java adja meg a következő import; Ezek az import osztályok az adatkezelési tárakban az Azure API-k használata más hozzáférést biztosít:

    // General imports
    import java.net.URI;
    import java.util.ArrayList;
    
    // Imports for Exceptions
    import java.io.IOException;
    import java.net.URISyntaxException;
    import javax.xml.parsers.ParserConfigurationException;
    import com.microsoft.windowsazure.exception.ServiceException;
    import org.xml.sax.SAXException;
    
    // Imports for Azure App Service management configuration
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.management.configuration.ManagementConfiguration;
    
    // Service management imports for App Service Web Apps creation
    import com.microsoft.windowsazure.management.websites.*;
    import com.microsoft.windowsazure.management.websites.models.*;
    
    // Imports for authentication
    import com.microsoft.windowsazure.core.utils.KeyStoreType;


#### <a name="define-the-main-entry-point-class"></a>A bejegyzés fő pont osztály meghatározása

Mivel az AzureWebDemo alkalmazás célját hozhat létre szolgáltatás Web App alkalmazást, ez az alkalmazás neve fő osztály `WebAppCreator`. Ez a bejegyzés fő pont kódot, amely felhívja az Azure szolgáltatás felügyeleti API hozhat létre a web app nyújt.

Adja hozzá a következő paraméter definíciók webspace és web App alkalmazásban. Szüksége lesz a saját Azure előfizetés azonosító és a tanúsítvány adatokat.

    public class WebAppCreator {
    
        // Parameter definitions used for authentication.
        private static String uri = "https://management.core.windows.net/";
        private static String subscriptionId = "<subscription-id>";
        private static String keyStoreLocation = "<certificate-store-path>";
        private static String keyStorePassword = "<certificate-password>";
    
        // Define web app parameter values.
        private static String webAppName = "WebDemoWebApp";
        private static String domainName = ".azurewebsites.net";
        private static String webSpaceName = WebSpaceNames.WESTUSWEBSPACE;
        private static String appServicePlanName = "WebDemoAppServicePlan";

Ha:

- `<subscription-id>`az Azure előfizetés azonosítója, amelyben létre szeretne hozni az erőforrás van.
- `<certificate-store-path>`az elérési utat és a JKS fájlt tároló helyi tanúsítvány címtárában fájlnevet. Ha például `C:/Certificates/CertificateName.jks` az Linux és `C:\Certificates\CertificateName.jks` a Windows.
- `<certificate-password>`az a jelszó a JKS tanúsítvány létrehozása után a megadott.
- `webAppName`lehet egy tetszőleges nevet, válassza a; Ez az eljárás nevét használja `WebDemoWebApp`. A teljes tartományneve a `webAppName` együtt a `domainName` hozzáfűzött, így ebben az esetben a teljes tartománya `webdemowebapp.azurewebsites.net`.
- `domainName`meg kell adni, ahogy alább látható.
- `webSpaceName`az értékek közül a [WebSpaceNames][] osztály definiált kell lennie.
- `appServicePlanName`meg kell adni, ahogy alább látható.

> **Megjegyzés:** Minden alkalommal, amikor ez az alkalmazásnak a futtatására, át kell értékének `webAppName` és `appServicePlanName` (vagy törölje a web app az Azure-portálon) az alkalmazás ismételt futtatása előtt. Adatvégrehajtás meghiúsul, mert már létezik ugyanaz az erőforrás Azure.


#### <a name="define-the-web-creation-method"></a>A webhely létrehozási módszer megadása

Ezután adja meg a módszer a webalkalmazás létrehozása. Ez a módszer `createWebApp`, a web app és a webspace paramétereit határozza meg. Is hoz létre, és konfigurálja az adatkezelési határozzák meg a [WebSiteManagementClient][] objektum alkalmazás szolgáltatás Web Apps alkalmazások ügyfél. Az adatkezelési ügyfél Web Apps alkalmazások létrehozása kulcs. Biztosít, amelyek lehetővé teszik az alkalmazások kezelése (például létrehozása, frissítése és törlése, a műveletek elvégzéséhez) webalkalmazások hívja fel a Szolgáltatáskezelés API RESTful webszolgáltatásokhoz.

    private static void createWebApp() throws Exception {

        // Specify configuration settings for the App Service management client.
        Configuration config = ManagementConfiguration.configure(
            new URI(uri),
            subscriptionId,
            keyStoreLocation,  // Path to the JKS file
            keyStorePassword,  // Password for the JKS file
            KeyStoreType.jks   // Flag that you are using a JKS keystore
        );

        // Create the App Service Web Apps management client to call Azure APIs
        // and pass it the App Service management configuration object.
        WebSiteManagementClient webAppManagementClient = WebSiteManagementService.create(config);

        // Create an App Service plan for the web app with the specified parameters.
        WebHostingPlanCreateParameters appServicePlanParams = new WebHostingPlanCreateParameters();
        appServicePlanParams.setName(appServicePlanName);
        appServicePlanParams.setSKU(SkuOptions.Free);
        webAppManagementClient.getWebHostingPlansOperations().create(webSpaceName, appServicePlanParams);

        // Set webspace parameters.
        WebSiteCreateParameters.WebSpaceDetails webSpaceDetails = new WebSiteCreateParameters.WebSpaceDetails();
        webSpaceDetails.setGeoRegion(GeoRegionNames.WESTUS);
        webSpaceDetails.setPlan(WebSpacePlanNames.VIRTUALDEDICATEDPLAN);
        webSpaceDetails.setName(webSpaceName);

        // Set web app parameters.
        // Note that the server farm name takes the Azure App Service plan name.
        WebSiteCreateParameters webAppCreateParameters = new WebSiteCreateParameters();
        webAppCreateParameters.setName(webAppName);
        webAppCreateParameters.setServerFarm(appServicePlanName);
        webAppCreateParameters.setWebSpace(webSpaceDetails);

        // Set usage metrics attributes.
        WebSiteGetUsageMetricsResponse.UsageMetric usageMetric = new WebSiteGetUsageMetricsResponse.UsageMetric();
        usageMetric.setSiteMode(WebSiteMode.Basic);
        usageMetric.setComputeMode(WebSiteComputeMode.Shared);

        // Define the web app object.
        ArrayList<String> fullWebAppName = new ArrayList<String>();
        fullWebAppName.add(webAppName + domainName);
        WebSite webApp = new WebSite();
        webApp.setHostNames(fullWebAppName);

        // Create the web app.
        WebSiteCreateResponse webAppCreateResponse = webAppManagementClient.getWebSitesOperations().create(webSpaceName, webAppCreateParameters);

        // Output the HTTP status code of the response; 200 indicates the request succeeded; 4xx indicates failure.
        System.out.println("----------");
        System.out.println("Web app created - HTTP response " + webAppCreateResponse.getStatusCode() + "\n");

        // Output the name of the web app that this application created.
        String shinyNewWebAppName = webAppCreateResponse.getWebSite().getName();
        System.out.println("----------\n");
        System.out.println("Name of web app created: " + shinyNewWebAppName + "\n");
        System.out.println("----------\n");
    }

A kód fog kimeneti a választ, jelezve a megoldás sikeres vagy sikertelen a HTTP állapotát, és sikerrel jár, ha kimeneti lesz a létrehozott webes alkalmazás nevére.


#### <a name="define-the-main-method"></a>A main() módszer megadása

Adja meg a main() mód kódot, amely felhívja createWebApp() a webalkalmazás létrehozása.

Végezetül hívás `createWebApp` a `main`:

        public static void main(String[] args)
            throws IOException, URISyntaxException, ServiceException,
            ParserConfigurationException, SAXException, Exception {

            // Create web app
            createWebApp();

        }  // end of main()

    }  // end of WebAppCreator class


#### <a name="run-the-application-and-verify-web-app-creation"></a>Futtassa az alkalmazást, és ellenőrizze a webhely-alkalmazás létrehozása

Ha ellenőrizni szeretné, hogy fut-e az alkalmazást, kattintson a **Futtatás > Futtatás**. Az alkalmazás befejeztével meg kell jelennie a Holdas konzolban a következő kimenet:

    ----------
    Web app created - HTTP response 200
    
    ----------
    
    Name of web app created: WebDemoWebApp
    
    ----------

Jelentkezzen be az Azure klasszikus portált, és kattintson a **Web Apps alkalmazások**. Az új webalkalmazás néhány percen belül megjelennek a Web Apps alkalmazások listájában.


## <a name="deploying-an-application-to-the-web-app"></a>A Web App alkalmazás telepítése

AzureWebDemo futtatása és az új webhely-alkalmazás létrehozása, jelentkezzen be a klasszikus portál, kattintson a **Web Apps alkalmazások**és **Web Apps alkalmazások** listában válassza az **WebDemoWebApp** . A web app-Irányítópultlap, kattintson a **Tallózás** gombra (vagy kattintson az URL-cím `webdemowebapp.azurewebsites.net`) segítségével keresse meg azt. Mivel nincs tartalom közzétételét követően a web App még egy üres helyőrző lap jelenik meg.

Ezután fog "Helló, világ"-alkalmazás létrehozása és beállítaná őket a web App alkalmazásban.


### <a name="create-a-jsp-hello-world-application"></a>JSP Helló, világ-alkalmazás létrehozása

#### <a name="create-the-application"></a>Az alkalmazás létrehozása

Annak érdekében, hogy a weben alkalmazások telepítése szemléltetik, az alábbi eljárás bemutatja, hogyan hozhat létre egyszerű "Helló, világ" Java-alkalmazások, és töltse fel a alkalmazás szolgáltatás Web App alkalmazás által létrehozott.

1. Kattintson a **Fájl > Új > a Project Web dinamikus**. Nevezze el `JSPHello`. Nem kell módosítani ezt a párbeszédpanelt a többi beállítást. Kattintson a **Befejezés gombra**.

    ![][3]

2. A Project Explorer bontsa ki a **JSPHello** projektet, kattintson a jobb gombbal a **Web tartalma**, majd kattintson a **Új > JSP fájl**. Az új JSP fájlt párbeszédpanelen nevezze el az új fájl `index.jsp`. Kattintson a **Tovább**gombra.

3. A **JSP sablon kiválasztása** párbeszédpanelen jelölje ki az **Új JSP fájl (html)** , és kattintson a **Befejezés gombra**.

4. Az index.jsp, adja meg a következő kódot a `<head>` és `<body>` szakaszok nyomon követése:

        <head>
          ...
          java.util.Date date = new java.util.Date();
        </head>
    
        <body>
          Hello, the time is <%= date %> 
        </body>


#### <a name="run-the-hello-world-application-in-localhost"></a>A Helló, világ alkalmazás futtatásához a localhost

Ez az alkalmazás futtatásához előtt kell néhány tulajdonságainak konfigurálása.

1. Kattintson a jobb gombbal a **JSPHello** projekt, és válassza a **Tulajdonságok parancsot**.

2. Az a **tulajdonságokat** tartalmazó párbeszédpanel: válassza a **Java összeállítása elérési út**, válassza ki a **sorrend és exportálás** lapot, **JRE rendszer tár**jelölőnégyzetét, majd **be** a lista elejére áthelyezheti kattintson.

    ![][4]

3. Is az a **tulajdonságokat** tartalmazó párbeszédpanel: Jelölje ki a **Futásidőben célként használt** , és kattintson az **Új**gombra.

4. Az **Új kiszolgáló futtatókörnyezet** párbeszédpanelen válassza ki a kiszolgálót, például **Apache Tomcat 7.0** , és kattintson a **Tovább**gombra. **Tomcat kiszolgáló** párbeszédpanelen állítsa be **nevet** `Apache Tomcat v7.0`, és adja meg a címtárhoz használni kívánt Tomcat kiszolgáló verziója van telepítve **Tomcat telepítési könyvtár** .

    ![][5]

    Kattintson a **Befejezés gombra**.

5. Majd térjen vissza az a **tulajdonságokat** tartalmazó párbeszédpanel **Futásidőben célként használt** oldalát. Jelölje ki a **Apache Tomcat 7.0**, majd kattintson az **OK gombra**.

    ![][6]

6. Menü Holdas **futtatni** kattintson a **Futtatás**gombra. A **Futtatás másként** párbeszédpanelen jelölje ki, **futtassa a kiszolgálón**. **Futtassa a kiszolgálón** párbeszédpanelen jelölje be a **kiszolgáló Tomcat 7.0**:

    ![][7]

    Kattintson a **Befejezés gombra**.

7. Ha az alkalmazás fut, meg kell jelennie a **JSPHello** lap jelenik meg a Holdas localhost ablakban (`http://localhost:8080/JSPHello/`), a következő üzenetben:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="export-the-application-as-a-war"></a>Az alkalmazás egy HÁBORÚ exportálása

A webes projektfájlok exportálása webes archív (HÁBORÚ) fájlként, így a web App telepítése. A következő webes projektfájlok a Web tartalma mappában találhatók:

    META-INF
    WEB-INF
    index.jsp

1. Kattintson a jobb gombbal a Web tartalma mappát, és válassza ki az **exportálni**.

2. Kattintson az **Exportálás kiválasztása** párbeszédpanelen **webes > HÁBORÚ** fájl, majd kattintson a **Tovább**gombra.

3. A **Háborús exportálása** párbeszédpanelen jelölje be a forrás könyvtár az aktuális projektben, és a végén a HÁBORÚ fájl nevét. Példa:

    `<project-path>/JSPHello/src/JSPHello.war`

HÁBORÚ fájlok üzembe helyezése a további tudnivalókért lásd: [Azure alkalmazás szolgáltatás Web Apps alkalmazások Java-alkalmazások felvétele](web-sites-java-add-app.md).


### <a name="deploying-the-hello-world-application-using-ftp"></a>Az FTP használatával helló világ alkalmazás telepítése

Jelölje be a külső FTP-ügyfél közzétenni az alkalmazást. Ez az eljárás bemutatja a két lehetőség közül választhat: a Kudu konzol épített Azure; és FileZilla, a népszerű eszközben a kényelmes, grafikus felhasználói Felületet.

> **Megjegyzés:** Az Azure eszközkészlete Holdas támogatja a telepítési tárolás fiókok és a felhőszolgáltatások, de jelenleg nem támogatja a web Apps alkalmazások telepítési. Tárterület-fiókok és Azure környezetben projekt segítségével [egy helló világ alkalmazás létrehozása az Azure Holdas a](http://msdn.microsoft.com/library/azure/hh690944.aspx)ismertetett módon felhőszolgáltatások, de nem web Apps alkalmazások telepítheti. Más módszerek, például az FTP- vagy GitHub segítségével fájlok továbbítása a web App alkalmazásban.

> **Megjegyzés:** FTP használata a Windows parancssorból (a parancssori FTP.EXE segédprogram Windows részét képező) nem ajánlott. Az aktív, FTP.EXE, például az FTP használó FTP-ügyfelek gyakran nem működik a tűzfalon keresztül. Aktív FTP adja meg a belső LAN-alapú címet, amelyre az FTP-kiszolgáló valószínűleg meghiúsul való csatlakozáshoz.

Az alkalmazás szolgáltatás web app FTP használatával történő telepítéséhez olvashat a következő témakörökben olvashatók:

- [Az FTP segédprogrammal terjesztése](web-sites-deploy.md)


#### <a name="set-up-deployment-credentials"></a>Telepítési hitelesítő adatainak beállítása

Ellenőrizze, hogy a **AzureWebDemo** alkalmazás hozzon létre egy webalkalmazás futtatta. Ezen a helyen lesz fájlok átvitele.

1. Jelentkezzen be a klasszikus portál, majd kattintson a **Web Apps alkalmazások**. Ellenőrizze, hogy **WebDemoWebApp** megjelenik a web Apps alkalmazások listájában, és győződjön meg arról, hogy fut. Kattintson a **WebDemoWebApp** az **Irányítópult** lap megnyitásához.

2. Az **Irányítópult** lapon a **Quick Glance**, kattintson **a telepítés hitelesítő adatok beállítása** (már telepítési hitelesítő adatait, ez beolvassa **a telepítési hitelesítő adatok alaphelyzetbe állítása**).

    Telepítési hitelesítő adatok társítva egy Microsoft-fiókkal. Adja meg a felhasználónevet és jelszót, mely számjegy és FTP üzembe használható szüksége. Ezek a hitelesítő adatok bármely web App alkalmazásba a Microsoft-fiókjához tartozó Azure foglalt üzembe is használhatja. Mely számjegy és FTP-telepítés hitelesítő adatok párbeszédpanelen adja meg, és rögzítse a felhasználónév és jelszó későbbi felhasználás céljából.


#### <a name="get-ftp-connection-information"></a>FTP-kapcsolat beállításai

Az újonnan létrehozott web App alkalmazás telepítéshez FTP használatához kell szerezze be a kapcsolat adatait. Két módon szerezze be a kapcsolat adatait. Egy úgy, hogy keresse fel **a web app Irányítópultlap;** a más módon van töltheti le a webes alkalmazásra profil közzététele. A közzététel profilt az XML-fájlban, például az FTP host nevét és a bejelentkezési hitelesítő adatait találja a web Apps alkalmazások Azure App szolgáltatásban. Felhasználónév és jelszó használatával az Azure-fiók, nem csak ezt az egy társított összes foglalt bármely web App telepítését.

A web app fel az [Azure-portálon][]lekérdezni FTP-kapcsolat adatait:

1. A **Essentials**keresse meg és másolja a vágólapra az **FTP hostname (állomásnév)**. Ez a hasonló URI `ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net`.

2. A **Essentials**keresse meg és **FTP/telepítési felhasználónév**másolása. Ez lesz az űrlap *webappname\deployment-username*; Ha például `WebDemoWebApp\deployer77`.

FTP-kapcsolat információkhoz juthat a közzététel profilból:

1. Kattintson a web app lap, a **Get-profil közzététele**. .Publishsettings fájl letöltése helyi meghajtóra.

2. Nyissa meg a .publishsettings fájlt egy XML-szerkesztő vagy szövegszerkesztőben, és keresse meg a `<publishProfile>` elemet tartalmazó `publishMethod="FTP"`. Meg kell kinéznie:

        <publishProfile
            profileName="WebDemoWebApp - FTP"
            publishMethod="FTP"
            publishUrl="ftp://waws-prod-bay-NNN.ftp.azurewebsites.windows.net/site/wwwroot"
            ftpPassiveMode="True"
            userName="WebDemoWebApp\$WebDemoWebApp"
            userPWD="<deployment-password>"
            ...
        </publishProfile>

3. Megjegyzendő, hogy a web app `publishProfile` beállítások térkép a FileZilla webhelykezelő beállítások az alábbiak szerint:

- `publishUrl`megegyezik az **FTP állomásnév**, **fogadó**értéket.
- `publishMethod="FTP"`azt jelenti, **FTP - fájl Transfer Protocol**, majd a **titkosítás** használatára **egyszerű FTP** **protokoll** beállítása.
- `userName`és `userPWD` kulcsok a tényleges felhasználónév és jelszó értékek megadott, amikor a telepítő hitelesítő adatok alaphelyzetbe állítása. `userName`ugyanaz, mint **telepítési / FTP-felhasználó**. **Felhasználók** és a **jelszót** a FileZilla rendeli.
- `ftpPassiveMode="True"`azt jelenti, hogy az FTP-helyre használja-e a passzív FTP továbbításhoz; Jelölje ki a **passzív** **Átadása beállítások** lapján.


#### <a name="configure-the-web-app-to-host-a-java-application"></a>Java-alkalmazások tárolni a Web App konfigurálása

Mielőtt közzétenne az alkalmazást, néhány beállítások módosításáról, így a web app üzemeltetheti Java-alkalmazások szüksége.

1. A klasszikus portálon nyissa meg **a web app-Irányítópultlap** , és kattintson a **Konfigurálás**gombra. **A lap** adja meg a következő beállításokat.

2. Az alapértelmezett érték **Java** verziójában **ki**; Jelölje ki a Java verzióját az alkalmazás cél; Ha például 1.7.0_51. Ezt követően is ellenőrizze, hogy a **webes tároló** Tomcat Server verziója van-e beállítva.

3. Az **Alapértelmezett dokumentumok**hozzáadása a index.jsp, és helyezze át a lista tetejére. (Az alapértelmezett fájl web Apps alkalmazások a hostingstart.html.)

4. Kattintson a **Mentés**gombra.


#### <a name="publish-your-application-using-kudu"></a>Az alkalmazás használatával Kudu közzététele

Az alkalmazás közzététele egy úgy, hogy az Azure épített Kudu hibakeresési konzolról. Kudu stabil és konzisztensek legyenek, az alkalmazás szolgáltatás Web Apps alkalmazások és az Tomcat Server ismert. A konzol a web App URL-címre az alábbi képernyő között érheti el:

`https://<webappname>.scm.azurewebsites.net/DebugConsole`

1. Ebben a példában a Kudu konzol található; a következő URL-címen Tallózás ezen a helyen:

    `https://webdemowebapp.scm.azurewebsites.net/DebugConsole`

2. Kattintson a felső menü **hibakeresési konzol > CMD**.

3. A konzol parancssorban nyissa meg `/site/wwwroot` (vagy kattintson a `site`, majd `wwwroot` a lap tetején a címtár nézetben):

    `cd /site/wwwroot`

4. **Java-verzió**adja meg, miután Tomcat kiszolgáló hozzon létre egy webalkalmazás könyvtár. A konzol parancssorban nyissa meg a webalkalmazás könyvtár:

    `mkdir webapps`

    `cd webapps`

5. Húzza a JSPHello.war `<project-path>/JSPHello/src/` , és engedje el a Kudu címtár nézetbe `/site/wwwroot/webapps`. Nem húzza a "Húzása ide felirat látható tölthet fel, és zip-" területre, mert Tomcat fog csomagolja ki.

  ![][8]

Első JSPHello.war, megjelenik a címtár-területen, saját maga által:

  ![][9]

A rövid idő (valószínűleg kisebb, mint 5 perc) Tomcat kiszolgáló fog bontsa ki a HÁBORÚ fájlt egy kicsomagolt JSPHello könyvtárban. Kattintson a legfelső szintű könyvtár, hogy index.jsp kibontott és másolt van. Ha igen, lépjen vissza a webalkalmazás könyvtár látható, hogy a kicsomagolt JSPHello könyvtár készült. Ha ezeket az elemeket nem látható, várjon, és ismételje meg.

  ![][10]


#### <a name="publish-your-application-using-filezilla-optional"></a>Tegye közzé a alkalmazást FileZilla (nem kötelező)

Egy másik is használhatja az alkalmazás közzététele eszköze FileZilla, a népszerű külső FTP-ügyfél kényelmes, grafikus felhasználói Felületet. Töltse le, és telepítse a FileZilla [http://filezilla-project.org/](http://filezilla-project.org/) az, ha már nincs rá. Az ügyfél használatával kapcsolatos további tudnivalókért lásd: a [FileZilla dokumentációt](https://wiki.filezilla-project.org/Documentation) , valamint a blogbejegyzés a [FTP-ügyfél - kijelző 4: FileZilla](http://blogs.msdn.com/b/robert_mcmurray/archive/2008/12/17/ftp-clients-part-4-filezilla.aspx).

1. Kattintson az FileZilla, **Fájl > webhelykezelő**.
2. A **Webhely kezelője** párbeszédpanelen kattintson az **Új helyre**. Egy új, üres FTP-hely megjelenik a kéri egy név megadására, **Jelölje ki a bejegyzést** . Ebben a példában nevezze el `AzureWebDemo-FTP`.

    Az **Általános** lapon adja meg a következő beállításokat:
    - **Host:** Adja meg az irányítópult másolt **FTP állomásnév** .
    - **Port:** (Ez üresen hagyja, passzív áthelyezés és meghatározza, hogy a kiszolgáló használata a port.)
    - **Protocol (protokoll):** FTP-fájl Transfer Protocol
    - **Titkosítás:** Egyszerű FTP használata
    - **Bejelentkezési típusa:** Normál
    - **Felhasználói:** Adja meg a telepítési / FTP-felhasználót, hogy az irányítópult másolta. Ez a teljes FTP felhasználónév, amelynek az űrlap *webappname\username*.
    - **Jelszó:** Adja meg a hitelesítő adatok telepítési beállításakor megadott jelszót.

    A **Átviteli beállítások** lapon válassza a **passzív**.

3. Kattintson a **Csatlakozás**gombra. Ha sikeres, FileZilla's konzolban megjelenik egy `Status: Connected` üzenetet, és a probléma a `LIST` parancs, amellyel a címtár-tartalom lista.

4. Válassza a **helyi** webhely ablaktábla, amelyben a JSPHello.war fájl található; a forrás-címtár az elérési utat a következőhöz hasonló lesz:

    `<project-path>/JSPHello/src/`

5. A **távoli** webhely panelen jelölje ki a célmappát. A háborús fájl telepíti a `webapps` könyvtár csoportban a web app legfelső szintű. Nyissa meg azt `/site/wwwroot`, kattintson a jobb gombbal a `wwwroot`, és válassza a **könyvtár létrehozása**. A könyvtár nevet `webapps` , és adja meg a könyvtár.

6. A JSPHello.war átadása `/site/wwwroot/webapps`. A **helyi** listából válassza ki a JSPHello.war, kattintson rá a jobb gombbal, és válassza a **Feltöltés**. Meg kell jelennie jelennek meg, hogy `/site/wwwroot/webapps`.

7. Miután JSPHello.war a webalkalmazás címtárhoz másolta, Tomcat kiszolgáló automatikusan csomagolja ki (csomagolja ki) az HÁBORÚ fájlban a fájlokat. Bár Tomcat kiszolgáló elkezdi szinte azonnal kicsomagolás, igénybe vehet egy hosszú idő (valószínűleg óra) a fájlok jelennek meg az FTP-ügyfél.


#### <a name="run-the-hello-world-application-on-the-web-app"></a>Az Helló, világ alkalmazás futtatásához a Web App

1. A háborús fájl feltöltése és ellenőrzött Tomcat kiszolgáló hozott egy kicsomagolt `JSPHello` directory, a Tallózás gombra, és `http://webdemowebapp.azurewebsites.net/JSPHello` az alkalmazásnak a futtatására.

    > **Megjegyzés:** Ha a klasszikus portálról a **Tallózás** gombra kattint, is javulhat az alapértelmezett weblapra kapja meg: "Ez alapján Java webalkalmazás létrehozása sikeresen befejeződött." Előfordulhat, hogy az weblapon frissítheti az alkalmazás kimeneti helyett az alapértelmezett weblap megtekintéséhez.

2. Ha az alkalmazás fut, meg kell jelennie a következő eredményt az weblapon:

    `Hello World, the time is Tue Mar 24 23:21:10 GMT 2015`


#### <a name="clean-up-azure-resources"></a>Azure erőforrások karbantartása

Ez az eljárás-szolgáltatási alkalmazás web App alkalmazásban hoz létre. Akkor fognak számlát kapni az erőforrás mindaddig, amíg ki van. Ha szeretné, hogy továbbra is használni a web app vizsgálatára vagy fejlesztése, figyelembe leállítása vagy a törlést. Leállítva webalkalmazást továbbra is merülnek fel egy kis díjat, de bármikor újraindítása. Feltöltött hozzá az összes adat törlése a webalkalmazást törli.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

  [1]: ./media/java-create-azure-website-using-java-sdk/eclipse-maven-repositories-rebuild-index.png
  [2]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-java-class.png
  [3]: ./media/java-create-azure-website-using-java-sdk/eclipse-new-dynamic-web-project.png
  [4]: ./media/java-create-azure-website-using-java-sdk/eclipse-java-build-path.png
  [5]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-tomcat-server.png
  [6]: ./media/java-create-azure-website-using-java-sdk/eclipse-targeted-runtimes-properties-page.png
  [7]: ./media/java-create-azure-website-using-java-sdk/eclipse-run-on-server.png
  [8]: ./media/java-create-azure-website-using-java-sdk/kudu-console-drag-drop.png
  [9]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-1.png
  [10]: ./media/java-create-azure-website-using-java-sdk/kudu-console-jsphello-war-2.png
 

[Azure alkalmazás szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=529714
[Webes Platform telepítő]: http://go.microsoft.com/fwlink/?LinkID=252838
[Azure eszközkészlete Holdas]: https://msdn.microsoft.com/library/azure/hh690946.aspx
[Azure klasszikus portál]: https://manage.windowsazure.com
[Mi az Azure Active Directory címtárhoz]: http://technet.microsoft.com/library/jj573650.aspx
[Adatkezelési tanúsítvány tölteni az Azure]: ../cloud-services/cloud-services-certs-create.md
[Kulcs és a tanúsítvány kezelése eszköz (keytool)]: http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html
[WebSiteManagementClient]: http://azure.github.io/azure-sdk-for-java/com/microsoft/azure/management/websites/WebSiteManagementClient.html
[WebSpaceNames]: http://dl.windowsazure.com/javadoc/com/microsoft/windowsazure/management/websites/models/WebSpaceNames.html
[Azure portál]: https://portal.azure.com
