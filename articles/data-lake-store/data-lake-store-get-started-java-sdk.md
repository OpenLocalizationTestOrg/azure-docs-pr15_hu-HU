<properties
   pageTitle="Adatok tó áruházból Java SDK segítségével alkalmazások fejlesztéséhez |} Microsoft Azure"
   description="Alkalmazások fejlesztéséhez Azure adatok tó áruházból Java SDK használatával"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Azure tó adattár Java használata – első lépések

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

Megtudhatja, hogy miként használhatja az Azure adatok tó áruházból Java SDK alapvető műveleteket, például létrehozásakor mappák, töltse fel, és töltse le a adatfájlok stb. Az adatok tó kapcsolatos további tudnivalókért lásd: [Azure tó adattár](data-lake-store-overview.md).

A Java SDK API tó adattár Azure [Azure adatok tó áruházból Java API-dokumentumok](https://azure.github.io/azure-data-lake-store-java/javadoc/)a dokumentumok között is elérheti.

## <a name="prerequisites"></a>Előfeltételek

* Java Development Kit (JDK 7 vagy újabb, használata Java 1,7 vagy újabb verzió)
* Azure tó adattár fiók. Kövesse az utasításokat az [első lépések az Azure tó adattár az Azure-portálon](data-lake-store-get-started-portal.md).
* [Maven tesztelése](https://maven.apache.org/install.html). Ebben az oktatóanyagban maven tesztelése build és a project a függőségek használja. Lehetséges nélkül, például maven tesztelése vagy Gradle build rendszer létrehozásához, de ezek rendszerek helyezése sokkal egyszerűbb függőségek kezelése.
* (Nem kötelező) És például [IntelliJ arról](https://www.jetbrains.com/idea/download/) vagy [Holdas](https://www.eclipse.org/downloads/) IDE vagy hasonló.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hogyan hitelesíteni, használja az Azure Active Directory?

Ebben az oktatóanyagban használjuk egy Azure AD-alkalmazás ügyfél titkos lekérése az Azure Active Directory jogkivonat (szolgáltatás hitelesítés). Ez a token használatával hozzon létre egy tó adattár ügyfél objektumot műveletek fájl- és műveletek elvégzéséhez. Kapcsolatos tudnivalókat az Azure tó adattár használatával az ügyfél titkos hitelesítést végezni akkor a következő lépésekkel magas szintű:

1. Az Azure Active Directory webalkalmazás létrehozása
2. Az ügyfél-azonosító ügyfél titkos és az Azure Active Directory webalkalmazáshoz jogkivonat végpont beolvasásához.
3. A tó adattár fájl/mappa létrehozásakor Java alkalmazásból a kívánt hozzáférés az Azure Active Directory webalkalmazás konfigurálása

Hogyan kell végezniük ezeket a lépéseket, tanulmányozza [az Active Directory-alkalmazás létrehozása](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory jogkivonat lekérdezni, valamint egyéb lehetőségeket is biztosít. Választhat számos különböző hitelesítési mechanizmusok megfeleljen az Ön esetében, például az alkalmazások a böngészőben, az alkalmazások terjesztése egy asztali alkalmazás vagy egy helyszíni rendszert futtató kiszolgáló alkalmazás vagy egy Azure virtuális gépen futó. Is választhat a különböző típusú hitelesítő adatait, például jelszavak, tanúsítványok, 2 varianciaanalízis hitelesítési és stb. Ezenkívül Azure Active Directory lehetővé teszi, hogy a helyszíni Active Directory-felhasználók szinkronizálni a felhőben. Részletekért olvassa el [Az Azure Active Directory Authentication esetek](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Java-alkalmazás létrehozása

A kód minta érhető el [a GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) részletes információt tartalmaz az áruházban létrehozásának folyamata végig fájlok, szereplő fájlok, a fájl letöltése és a tárolóban lévő egyes fájlok törlése. Ebben a szakaszban a cikk ismerteti, hogy a kód fő részei között.

1. [Mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) a parancssorból vagy egy IDE használatával maven tesztelése projektet létrehozni. IntelliJ használatával Java projekt létrehozása, tanulmányozza [az alábbi](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Hogyan hozhat létre a projekt Holdas, tanulmányozza [az alábbi](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. A következő függőségeket hozzáadása a maven tesztelése **pom.xml** fájlhoz. A következő kódtöredékének közötti szöveg hozzáadása a ** \<version >** címke és a ** \</Projekt >** címke:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    Az első függőség azt javasoljuk, hogy az adatok tó áruházból SDK használata (`azure-datalake-store`) a a maven tesztelése tárat. A második függőség (`slf4j-nop`), megadhatja, melyik naplózás keretrendszer használni ehhez az alkalmazáshoz. Az adatok tó áruházból SDK [slf4j](http://www.slf4j.org/) naplózás homlokzati, amely lehetővé teszi számos népszerű naplózás keretek log4j, például a naplózás logback, stb Java használja, vagy nincs naplózás. Ebben a példában a naplózás letiltjuk, így használjuk a **slf4j-nop** kötés. Az alkalmazás a többi naplózási beállítások használatához olvassa el [az alábbi](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Az alkalmazás-kód hozzáadása

Vannak olyan három fő részből a kódot.

1. Az Azure Active Directory jogkivonathoz beszerzése

2. A token segítségével tó adattár ügyfél.

3. Az adatok tó áruházból ügyfél segítségével hajtanak végre.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Lépés: 1: Szerezze be az Azure Active Directory jogkivonat.

Az adatok tó áruházból SDK bemutat kényelmes, amelyekkel szerezze be a biztonsági tokenek felvegye a tó adattár fiók szükséges. Azonban a SDK nem határozza meg, hogy csak az alábbi módszerek használható. Bármely más módon megszerzése jogkivonat is, például az [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)vagy a saját egyéni kód használatával is használhatja.

Az adatok tó áruházból SDK jogkivonat, az Active Directory webalkalmazáshoz korábban létrehozott beszerzése használatához a statikus módszereinek `AzureADAuthenticator` osztály. Cserélje ki az Azure Active Directory webes alkalmazáshoz a tényleges értékek **FILL itt** .

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Lépés: 2: Azure tó adattár-ügyfél (ADLStoreClient) objektum létrehozása

Az objektum [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) létrehozásához adja meg a tó adattár fiók nevét és az Azure Active Directory jogkivonat, akkor az utolsó lépésben létrehozott. Figyelje meg, hogy a tó adattár fióknév kell lennie a tartománynevét. Ha például cserélje **FILL itt** **mydatalakestore.azuredatalakestore.net**hasonló.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Lépés 3: A fájl- és műveleteket ADLStoreClient használata

A kód, az alábbi példa kódrészletek néhány gyakori műveletek tartalmazza. Megnézheti a teljes [adatok tó áruházból Java SDK API-dokumentumok](https://azure.github.io/azure-data-lake-store-java/javadoc/) **ADLStoreClient** objektum egyéb műveletek megtekintéséhez.
 
Figyelje meg, hogy a fájlok olvassák és bejegyzi segítségével szabványos Java-adatfolyam megjelenítését. Ez azt jelenti, hogy layer-a tó adattár adatfolyamok összekapcsolhatók szabványos Java-funkciók (például nyomtatás adatfolyamok formázott kimeneti, vagy bármely a tömörítés és titkosítás adatfolyamok további funkciók a felső stb.) felvételével a Java adatfolyamok közül.

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Lépés: 4: Össze, és futtassa az alkalmazást

1. Futtathat egy IDE belül, keresse meg, és nyomja meg a **Futtatás** gombra. [Végrehajtási: végrehajtási](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html)segítségével maven tesztelése futtatásához.

2. Kapcsol egy különálló üveg, amely a parancssorból futtatását is lehetővé teszi a üveg az összes függőséget tartalmaz, a [maven tesztelése összeállítási beépülő modul](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html)használatával hozhat létre. A [Példa forráskód a github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) a pom.xml ehhez példa tartalmaz.


## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
