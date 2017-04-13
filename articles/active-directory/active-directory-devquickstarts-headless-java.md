<properties
    pageTitle="Első lépések Azure Active Directory Java |} Microsoft Azure"
    description="Egy Java parancssori alkalmazás, amely a felhasználó bejelentkezik egy API eléréséhez létrehozásának módját."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Java parancssori alkalmazással az Azure Active Directory-API eléréséhez

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure Active Directory teszi egyszerű és a webalkalmazás az Identitáskezelés, kihelyező egyszerű nyújtó egyetlen bejelentkezési és a csak néhány kódsorokat kijelentkezési.  Java-webalkalmazások esetében az Ez használatával elvégezhető a közösségi-alapú ADAL4J a Microsoft végrehajtását.

  Itt a ADAL4J be:
- Jelentkezzen be a felhasználó Azure AD a identitásszolgáltató használata az alkalmazásba.
- A felhasználó információkat megjeleníteni.
- Jelentkezzen be a felhasználó ki az alkalmazást.

Annak érdekében, hogy ehhez kell:

1. Azure AD-alkalmazás regisztrálása
2. Állítsa be az alkalmazás a ADAL4J tárat.
3. Használja a ADAL4J tár bejelentkezési és kijelentkezési kérések Azure AD ki.
4. Nyomtassa ki a felhasználó adatait.

Az első lépések, [Töltse le az alkalmazás skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) vagy [a kész minta letöltése](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Egy Azure AD-bérlő, amelyben az alkalmazás rögzítése is szüksége lesz.  Ha nincs telepítve egyik már, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. regisztrálja Azure AD-alkalmazás
Ahhoz, hogy a felhasználók hitelesítést végezni az alkalmazást, először kell egy új alkalmazás regisztrálhatja a bérlői webhelyén.

- Jelentkezzen be az Azure adatkezelési portálra.
- A bal oldali navigációs sávon kattintson **Az Active Directory**.
- Jelölje ki a bérlő, ha ki szeretne regisztrálni az alkalmazást.
- Kattintson az **alkalmazások** fülre, és kattintson a Hozzáadás a alsó fiókban.
- Az útmutatást követve hozzon létre egy új **webalkalmazás és/vagy WebAPI**.
    - Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    - A **Bejelentkezési URL** az alkalmazás alap URL-CÍMÉT.  A skeleton alapértelmezett `http://localhost:8080/adal4jsample/`.
    - Az **Alkalmazás azonosítója URI** az alkalmazás egy egyedi azonosítót.  A kiállítás környezetbe `https://<tenant-domain>/<app-name>`, például`http://localhost:8080/adal4jsample/`
- Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító.  Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a beállítás lapon.

Egyszer a portál az alkalmazás- **Alkalmazás titkos** az alkalmazás létrehozása, és másolja a vágólapra lefelé.  Hamarosan lesz szüksége.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. Állítsa be az alkalmazás ADAL4J tár és maven tesztelése használatával vonatkozó követelmények
Ebben az esetben azt fogja állítsa be a hitelesítés OpenID csatlakozás protokoll használatára ADAL4J.  ADAL4J bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   A projekt legfelső szintű címtárban Megnyitás/létrehozás `pom.xml` , és keresse meg a `// TODO: provide dependencies for Maven` és a csere a következőre:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. a java PublicClient fájl létrehozása

A fentebb ismertetett módon azt fogja használni a diagram API beszerzése a bejelentkezett felhasználó adatait. Ez legyen az Amerikai egyszerűen azt kell hozzon létre egy fájlt a **Címtár-objektum** ábrázolására és a az egyes fájlja jelenítik meg a **felhasználót** , hogy Java OO szerkezet is használható.

1. Hozzon létre egy nevű fájlt `DirectoryObject.java` , amely bármely (akkor is nyugodtan ezzel a később más Graph lekérdezések is elvégezheti) DirectoryObject alapvető adatainak tárolására fogjuk használni. Akkor is Kivágás és beillesztés a alulról:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Állíthat össze, és futtassa a minta

Váltson vissza ki a legfelső szintű könyvtárra, és futtassa a következő parancsot a minta létrehozásához csak helyezi együttes használatával `maven`. Ezt fogja használni a `pom.xml` függőségek írt fájlt.

`$ mvn package`

Most már rendelkeznie kell egy `adal4jsample.war` a fájl a `/targets` címtár. Előfordulhat, hogy telepíteni, amely a Tomcat tárolóban és látogasson el az URL-cím 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Akkor is nagyon könnyen egy HÁBORÚ legújabb Tomcat kiszolgálókkal való telepítéséhez. Egyszerűen navigáljon `http://localhost:8080/manager/` és a megjelenő útmutatást követve feltöltése a "adal4jsample.war" fájlt. Autodeploy, fogja meg a megfelelő végponttal.

##<a name="next-steps"></a>Következő lépések

Gratulálok! Most már rendelkezik egy, az azt jelenti, hogy biztonságosan hitelesíti a felhasználókat, amelynek Java-alkalmazások munka hívja fel a webes API-khoz OAuth 2.0-s verzióját használja, és a felhasználó alapvető adatainak.  Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez.

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)kész példa (nélkül a konfigurációs értékek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

