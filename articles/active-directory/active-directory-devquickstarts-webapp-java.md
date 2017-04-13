<properties
    pageTitle="Első lépések Azure Active Directory Java |} Microsoft Azure"
    description="Hogyan lehet, hogy a felhasználók be a munkahelyi vagy iskolai fiókjával bejelentkezik Java webalkalmazást összeállítása."
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


# <a name="java-web-app-sign-in--sign-out-with-azure-ad"></a>Java Web App bejelentkezés és kijelentkezés az Azure Active Directory

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


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisities-using-maven"></a>2. Állítsa be az alkalmazás ADAL4J tár és prerequisities használatával maven tesztelése
Ebben az esetben azt fogja állítsa be a hitelesítés OpenID csatlakozás protokoll használatára ADAL4J.  ADAL4J bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   A projekt legfelső szintű címtárban Megnyitás/létrehozás `pom.xml` , és keresse meg a `// TODO: provide dependencies for Maven` és a csere a következőre:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4jsample</artifactId>
    <packaging>war</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>adal4jsample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.1</version>
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
        <!-- Spring 3 dependencies -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>sample-for-adal4j</finalName>
        <plugins>
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
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <warName>${project.artifactId}</warName>
                    <source>${project.basedir}\src</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>utf-8</encoding>
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
        </plugins>
    </build>

</project>

```


## <a name="3-create-the-java-web-application-files-web-inf"></a>3. a Java webes alkalmazás fájlok létrehozása (webhely-információ)

Ebben az esetben azt fogja állítsa be a Java web app a csatlakozás OpenID hitelesítési protokoll használatára.  A ADAL4J tár bejelentkezési és kijelentkezési kérések hiba kezelése munkamenetet a felhasználó és a felhasználó számára, többek között adatainak használatával.

-   Első lépésként nyissa meg a `web.xml` csoportban található fájl `\webapp\WEB-INF\`, és az xml az alkalmazás-konfiguráció értékek megadása.

A fájl a következő hasonlóan kell kinéznie:

```xml
<?xml version="1.0"?>
<web-app id="WebApp_ID" version="2.4"
    xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
    http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
    <display-name>Archetype Created Web Application</display-name>
    <context-param>
        <param-name>authority</param-name>
        <param-value>https://login.windows.net/</param-value>
    </context-param>
    <context-param>
        <param-name>tenant</param-name>
        <param-value>YOUR_TENANT_NAME</param-value>
    </context-param>

    <filter>
        <filter-name>BasicFilter</filter-name>
        <filter-class>com.microsoft.aad.adal4jsample.BasicFilter</filter-class>
        <init-param>
            <param-name>client_id</param-name>
            <param-value>YOUR_CLIENT_ID</param-value>
        </init-param>
        <init-param>
            <param-name>secret_key</param-name>
            <param-value>YOUR_CLIENT_SECRET</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>BasicFilter</filter-name>
        <url-pattern>/secure/*</url-pattern>
    </filter-mapping>

    <servlet>
        <servlet-name>mvc-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>mvc-dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>

```


    -   A `YOUR_CLIENT_ID` az alkalmazás a regisztrációs portálon rendelt **Azonosítója** .
    -   A `YOUR_CLIENT_SECRET` a portálon létrehozott **Alkalmazás titkos kulcs** van.
    - A `YOUR_TENANT_NAME` az alkalmazást, például contoso.onmicrosoft.com **bérlői neve**

A többi konfigurációs paraméterét egyedül hagyja.

> [AZURE.NOTE]
Megjelenik az XML-fájlból, azt az éppen írt egy úgynevezett JSP/Servlet webappban `mvc-dispatcher` , amely fogja használni a `BasicFilter` jelenik meg, ha azt keresse fel a / biztonságos URL-CÍMÉT. Látni fogja a többi azonos azt írja, hogy azt fogja használni, és a hely, ahol a védett tartalmat él szerint biztonságos és Azure Active Directory authentication fog kényszerítése.

-   Ezután hozzon létre a `mvc-dispatcher-servlet.xml` csoportban található fájl `\webapp\WEB-INF\`, és írja be a következőt:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.0.xsd">

    <context:component-scan base-package="com.microsoft.aad.adal4jsample" />

    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>

</beans>
```

Ez a program tájékoztatja a webappban tavaszi és a hol találhatók a .jsp fájlt, amely azt írja alá.

## <a name="4-create-the-java-jsp-view-files-for-basicfilter-mvc"></a>4. a fájlok Java JSP megtekintése létrehozása (a BasicFilter MVC)

Jelenleg csak fele halászott a WEB-információ a saját webalkalmazás beállítása. Következő azt kell létrehozni a tényleges Java Server Pages fájlokat, amelyek a saját webalkalmazás hajtja végre, amely a tanácsolt azt a konfigurációban.

Ha emlékszik, hogy említettük Java az XML-konfigurációs fájlokat, amelyek van még egy `/` erőforrás, töltse be az .jsp fájlokat, és egy `/secure` erőforrás, amelynek szűrő kellene haladnia azt nevű `BasicFilter`. 

Vegyük hajtsa végre a azokat.

-   Első lépésként hozzon létre a `index.jsp` csoportban található fájl `\webapp\`, és a Kivágás és beillesztés a következőket:

```jsp
<html>
<body>
    <h2>Hello World!</h2>
    <ul>
    <li><a href="secure/aad">Secure Page</a></li>
    </ul>
</body>
</html>

```

Ez egyszerűen irányítja át a szűrő védik biztonságos weblapra.

- Ezután a azonos címtár segítségével létrehozása egy `error.jsp` tájékozódást segíti a hibák, amelyek akkor fordulhat elő, hogy a fájl:

```jsp
<html>
<body>
    <h2>ERROR PAGE!</h2>
    <p>
        Exception -
        <%=request.getAttribute("error")%></p>
    <ul>
        <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
    </ul>
</body>
</html>
```

- Végezetül: hozzon létre egy mappát a szeretnénk biztonságos weblapról nézzük győződjön `\webapp` nevű `\secure` , hogy a címtárban us most `\webapp\secure`. 

- Ezt a címtárat belül majd készítsünk egy `aad.jsp` fájl és a Kivágás és beillesztés a következő:

```jsp
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>AAD Secure Page</title>
</head>
<body>

    <h1>Directory - Users List</h1>
    <p>${users}</p>

    <ul>
        <li><a href="<%=request.getContextPath()%>/secure/aad?cc=1">Get
                new Access Token via Client Credentials</a></li>
    </ul>
    <ul>
        <li><a href="<%=request.getContextPath()%>/secure/aad?refresh=1">Get
                new Access Token via Refresh Token</a></li>
    </ul>
    <ul>
        <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
    </ul>
</body>
</html>
```

Láthatja, hogy a lapon úgy irányítja át a kérései, amely a BasicFilter servlet olvasni és hajthat végre használatáról a `ADAJ4J` tárat. Meglehetősen egyszerű huh?

Természetesen azt kell most be kell állítania a Java-fájlokat, hogy a servlet teheti munkáját.

## <a name="5-create-some-java-helper-files-for-basicfilter-mvc"></a>5. egyes Java segítő fájlok létrehozása (a BasicFilter MVC)

Célunk, hogy a program bizonyos Java-fájlok létrehozása:

1. Engedélyezze és kijelentkezési annak a felhasználónak bejelentkezés
2. Első néhány felhasználó adatait.

> [AZURE.NOTE] 
Ahhoz, hogy a felhasználó adatait, szükség az Azure Active Directory Graph API-t használja. A diagram API egy biztonságos webszolgáltatás ragadni a szervezetéhez, beleértve az egyes felhasználók adatainak használható. Ez célszerűbb, mint az engedélyezett előre kitöltése tokenek bizalmas adatokat, ahogy azt ellenőrzi, hogy a felhasználó megkérdezi, hogy az adatokat, és bárki fordulhat elő, a token (gyorsítótárból jailbroken telefon vagy a webhely böngészőben hozhat létre az asztalon) ragadni nem fog kapni a fontos tudnivalókat a felhasználó vagy a szervezete kezdetű.

Nézzük írjon nekünk a munka elvégzéséhez egyes Java-fájlok:

1. Hozzon létre egy mappát a legfelső szintű címtárában "adal4jsample" nevű összes a java-fájlt tárolja. 

Azt fogja használni a névtér `com.microsoft.aad.adal4jsample` a java-fájlokban. A legtöbb IDEs létrehozása egy olyan beágyazott mappastruktúra ahhoz, hogy ez (pl. `/com/microsoft/aad/adal4jsample`). Ehhez ingyenes, de már nem szükséges.

2. Ez a mappa belül nevű fájl létrehozása `JSONHelper.java` fogjuk használni szeretné, hogy segítse a szolgáltatás a tokenek JSON adatainak az elemezni, amely. Akkor is Kivágás és beillesztés a alulról:

```Java

package com.microsoft.aad.adal4jsample;

import java.lang.reflect.Field;
import java.util.Arrays;
import java.util.Enumeration;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.apache.commons.lang3.text.WordUtils;
import org.apache.log4j.Logger;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

/**
 * This class provides the methods to parse JSON Data from a JSON Formatted
 * String.
 * 
 * @author Azure Active Directory Contributor
 * 
 */
public class JSONHelper {

    private static Logger logger = Logger.getLogger(JSONHelper.class);

    JSONHelper() {
        // PropertyConfigurator.configure("log4j.properties");
    }

    /**
     * This method parses an JSON Array out of a collection of JSON Objects
     * within a string.
     * 
     * @param jSonData
     *            The JSON String that holds the collection.
     * @return An JSON Array that would contains all the collection object.
     * @throws Exception
     */
    public static JSONArray fetchDirectoryObjectJSONArray(JSONObject jsonObject) throws Exception {
        JSONArray jsonArray = new JSONArray();
        jsonArray = jsonObject.optJSONObject("responseMsg").optJSONArray("value");
        return jsonArray;
    }

    /**
     * This method parses an JSON Object out of a collection of JSON Objects
     * within a string
     * 
     * @param jsonObject
     * @return An JSON Object that would contains the DirectoryObject.
     * @throws Exception
     */
    public static JSONObject fetchDirectoryObjectJSONObject(JSONObject jsonObject) throws Exception {
        JSONObject jObj = new JSONObject();
        jObj = jsonObject.optJSONObject("responseMsg");
        return jObj;
    }

    /**
     * This method parses the skip token from a json formatted string.
     * 
     * @param jsonData
     *            The JSON Formatted String.
     * @return The skipToken.
     * @throws Exception
     */
    public static String fetchNextSkiptoken(JSONObject jsonObject) throws Exception {
        String skipToken = "";
        // Parse the skip token out of the string.
        skipToken = jsonObject.optJSONObject("responseMsg").optString("odata.nextLink");

        if (!skipToken.equalsIgnoreCase("")) {
            // Remove the unnecessary prefix from the skip token.
            int index = skipToken.indexOf("$skiptoken=") + (new String("$skiptoken=")).length();
            skipToken = skipToken.substring(index);
        }
        return skipToken;
    }

    /**
     * @param jsonObject
     * @return
     * @throws Exception
     */
    public static String fetchDeltaLink(JSONObject jsonObject) throws Exception {
        String deltaLink = "";
        // Parse the skip token out of the string.
        deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.deltaLink");
        if (deltaLink == null || deltaLink.length() == 0) {
            deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.nextLink");
            logger.info("deltaLink empty, nextLink ->" + deltaLink);

        }
        if (!deltaLink.equalsIgnoreCase("")) {
            // Remove the unnecessary prefix from the skip token.
            int index = deltaLink.indexOf("deltaLink=") + (new String("deltaLink=")).length();
            deltaLink = deltaLink.substring(index);
        }
        return deltaLink;
    }

    /**
     * This method would create a string consisting of a JSON document with all
     * the necessary elements set from the HttpServletRequest request.
     * 
     * @param request
     *            The HttpServletRequest
     * @return the string containing the JSON document.
     * @throws Exception
     *             If there is any error processing the request.
     */
    public static String createJSONString(HttpServletRequest request, String controller) throws Exception {
        JSONObject obj = new JSONObject();
        try {
            Field[] allFields = Class.forName(
                    "com.microsoft.windowsazure.activedirectory.sdk.graph.models." + controller).getDeclaredFields();
            String[] allFieldStr = new String[allFields.length];
            for (int i = 0; i < allFields.length; i++) {
                allFieldStr[i] = allFields[i].getName();
            }
            List<String> allFieldStringList = Arrays.asList(allFieldStr);
            Enumeration<String> fields = request.getParameterNames();

            while (fields.hasMoreElements()) {

                String fieldName = fields.nextElement();
                String param = request.getParameter(fieldName);
                if (allFieldStringList.contains(fieldName)) {
                    if (param == null || param.length() == 0) {
                        if (!fieldName.equalsIgnoreCase("password")) {
                            obj.put(fieldName, JSONObject.NULL);
                        }
                    } else {
                        if (fieldName.equalsIgnoreCase("password")) {
                            obj.put("passwordProfile", new JSONObject("{\"password\": \"" + param + "\"}"));
                        } else {
                            obj.put(fieldName, param);

                        }
                    }
                }
            }
        } catch (JSONException e) {
            e.printStackTrace();
        } catch (SecurityException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        return obj.toString();
    }

    /**
     * 
     * @param key
     * @param value
     * @return string format of this JSON obje
     * @throws Exception
     */
    public static String createJSONString(String key, String value) throws Exception {

        JSONObject obj = new JSONObject();
        try {
            obj.put(key, value);
        } catch (JSONException e) {
            e.printStackTrace();
        }

        return obj.toString();
    }

    /**
     * This is a generic method that copies the simple attribute values from an
     * argument jsonObject to an argument generic object.
     * 
     * @param jsonObject
     *            The jsonObject from where the attributes are to be copied.
     * @param destObject
     *            The object where the attributes should be copied into.
     * @throws Exception
     *             Throws a Exception when the operation are unsuccessful.
     */
    public static <T> void convertJSONObjectToDirectoryObject(JSONObject jsonObject, T destObject) throws Exception {

        // Get the list of all the field names.
        Field[] fieldList = destObject.getClass().getDeclaredFields();

        // For all the declared field.
        for (int i = 0; i < fieldList.length; i++) {
            // If the field is of type String, that is
            // if it is a simple attribute.
            if (fieldList[i].getType().equals(String.class)) {
                // Invoke the corresponding set method of the destObject using
                // the argument taken from the jsonObject.
                destObject
                        .getClass()
                        .getMethod(String.format("set%s", WordUtils.capitalize(fieldList[i].getName())),
                                new Class[] { String.class })
                        .invoke(destObject, new Object[] { jsonObject.optString(fieldList[i].getName()) });
            }
        }
    }

    public static JSONArray joinJSONArrays(JSONArray a, JSONArray b) {
        JSONArray comb = new JSONArray();
        for (int i = 0; i < a.length(); i++) {
            comb.put(a.optJSONObject(i));
        }
        for (int i = 0; i < b.length(); i++) {
            comb.put(b.optJSONObject(i));
        }
        return comb;
    }

}

```

3. Ezután hozzon létre egy nevű fájlt `HttpClientHelper.java` , amely a segítse a szolgáltatás a saját AAD végpont HTTP adatainak elemzése fogjuk használni. Akkor is Kivágás és beillesztés a alulról:

```Java

package com.microsoft.aad.adal4jsample;

import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;

import org.json.JSONException;
import org.json.JSONObject;

/**
 * This is Helper class for all RestClient class.
 * 
 * @author Azure Active Directory Contributor
 * 
 */
public class HttpClientHelper {

    public HttpClientHelper() {
        super();
    }

    public static String getResponseStringFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

        BufferedReader reader = null;
        if (isSuccess) {
            reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        } else {
            reader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
        }
        StringBuffer stringBuffer = new StringBuffer();
        String line = "";
        while ((line = reader.readLine()) != null) {
            stringBuffer.append(line);
        }

        return stringBuffer.toString();
    }

    public static String getResponseStringFromConn(HttpURLConnection conn, String payLoad) throws IOException {

        // Send the http message payload to the server.
        if (payLoad != null) {
            conn.setDoOutput(true);
            OutputStreamWriter osw = new OutputStreamWriter(conn.getOutputStream());
            osw.write(payLoad);
            osw.flush();
            osw.close();
        }

        // Get the message response from the server.
        BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String line = "";
        StringBuffer stringBuffer = new StringBuffer();
        while ((line = br.readLine()) != null) {
            stringBuffer.append(line);
        }

        br.close();

        return stringBuffer.toString();
    }

    public static byte[] getByteaArrayFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

        InputStream is = conn.getInputStream();
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] buff = new byte[1024];
        int bytesRead = 0;

        while ((bytesRead = is.read(buff, 0, buff.length)) != -1) {
            baos.write(buff, 0, bytesRead);
        }

        byte[] bytes = baos.toByteArray();
        baos.close();
        return bytes;
    }

    /**
     * for bad response, whose responseCode is not 200 level
     * 
     * @param responseCode
     * @param errorCode
     * @param errorMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processResponse(int responseCode, String errorCode, String errorMsg) throws JSONException {
        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        response.put("errorCode", errorCode);
        response.put("errorMsg", errorMsg);

        return response;
    }

    /**
     * for bad response, whose responseCode is not 200 level
     * 
     * @param responseCode
     * @param errorCode
     * @param errorMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processGoodRespStr(int responseCode, String goodRespStr) throws JSONException {
        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        if (goodRespStr.equalsIgnoreCase("")) {
            response.put("responseMsg", "");
        } else {
            response.put("responseMsg", new JSONObject(goodRespStr));
        }

        return response;
    }

    /**
     * for good response
     * 
     * @param responseCode
     * @param responseMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processBadRespStr(int responseCode, String responseMsg) throws JSONException {

        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        if (responseMsg.equalsIgnoreCase("")) { // good response is empty string
            response.put("responseMsg", "");
        } else { // bad response is json string
            JSONObject errorObject = new JSONObject(responseMsg).optJSONObject("odata.error");

            String errorCode = errorObject.optString("code");
            String errorMsg = errorObject.optJSONObject("message").optString("value");
            response.put("responseCode", responseCode);
            response.put("errorCode", errorCode);
            response.put("errorMsg", errorMsg);
        }

        return response;
    }

}

```

## <a name="6-create-the-java-graph-api-model-files-for-basicfilter-mvc"></a>6. létrehozása a java Graph API modellfájlt (a BasicFilter MVC)

A fentebb ismertetett módon azt fogja használni a diagram API beszerzése a bejelentkezett felhasználó adatait. Ez legyen az Amerikai egyszerűen azt kell hozzon létre egy fájlt a **Címtár-objektum** ábrázolására és a az egyes fájlja jelenítik meg a **felhasználót** , hogy Java OO szerkezet is használható.

1. Hozzon létre egy nevű fájlt `DirectoryObject.java` , amely bármely (akkor is nyugodtan ezzel a később más Graph lekérdezések is elvégezheti) DirectoryObject alapvető adatainak tárolására fogjuk használni. Akkor is Kivágás és beillesztés a alulról:

```Java

package com.microsoft.aad.adal4jsample;

/**
 * @author Azure Active Directory Contributor
 *
 */
public abstract class DirectoryObject {
    
    public DirectoryObject() {
        super();
    }
    
    /**
     * 
     * @return
     */
    public abstract String getObjectId();
    
    /**
     * @param objectId
     */
    public abstract void setObjectId(String objectId);

    /**
     * 
     * @return
     */
    public abstract String getObjectType();

    /**
     * 
     * @param objectType
     */
    public abstract void setObjectType(String objectType);
    
    /**
     * 
     * @return
     */
    public abstract String getDisplayName();

    /**
     * 
     * @param displayName
     */
    public abstract void setDisplayName(String displayName);

}

```

2. Hozzon létre egy nevű fájlt `User.java` pedig bármelyik felhasználó a címtárban egyszerű adatok fogjuk használni, amely. Ismét: az meglehetősen egyszerű Getterek/alkotóival címtár-adatok, akkor is Kivágás és beillesztés a alulról:

```Java

package com.microsoft.aad.adal4jsample;

import java.security.acl.Group;
import java.util.ArrayList;

import javax.xml.bind.annotation.XmlRootElement;

import org.json.JSONObject;

/**
 *  The User Class holds together all the members of a WAAD User entity and all the access methods and set methods
 *  @author Azure Active Directory Contributor
 */
@XmlRootElement
public class User extends DirectoryObject{
    
    // The following are the individual private members of a User object that holds
    // a particular simple attribute of an User object.
    protected String objectId;
    protected String objectType;
    protected String accountEnabled;
    protected String city;
    protected String country;
    protected String department;
    protected String dirSyncEnabled;
    protected String displayName;
    protected String facsimileTelephoneNumber;
    protected String givenName;
    protected String jobTitle;
    protected String lastDirSyncTime;
    protected String mail;
    protected String mailNickname;
    protected String mobile;
    protected String password;
    protected String passwordPolicies;
    protected String physicalDeliveryOfficeName;
    protected String postalCode;
    protected String preferredLanguage;
    protected String state;
    protected String streetAddress;
    protected String surname;
    protected String telephoneNumber;
    protected String usageLocation;
    protected String userPrincipalName;
    protected boolean isDeleted;  // this will move to dto

    /**
     * below 4 properties are for future use
     */
    // managerDisplayname of this user
    protected String managerDisplayname;
    
    // The directReports holds a list of directReports
    private ArrayList<User> directReports;
    
    // The groups holds a list of group entity this user belongs to. 
    private ArrayList<Group> groups;
    
    // The roles holds a list of role entity this user belongs to. 
    private ArrayList<Group> roles;
    
    
    /**
     * The constructor for the User class. Initializes the dynamic lists and managerDisplayname variables.
     */
    public User(){
        directReports = null;
        groups = new ArrayList<Group>();
        roles = new ArrayList<Group>();
        managerDisplayname = null;
    }
//  
//  public User(String displayName, String objectId){
//      setDisplayName(displayName);
//      setObjectId(objectId);
//  }
//  
//  public User(String displayName, String objectId, String userPrincipalName, String accountEnabled){
//      setDisplayName(displayName);
//      setObjectId(objectId);
//      setUserPrincipalName(userPrincipalName);
//      setAccountEnabled(accountEnabled);
//  }
//  

    /**
     * @return The objectId of this user.
     */
    public String getObjectId() {
        return objectId;
    }
    
    /**
     * @param objectId The objectId to set to this User object.
     */
    public void setObjectId(String objectId) {
        this.objectId = objectId;
    }


    /**
     * @return The objectType of this User.
     */
    public String getObjectType() {
        return objectType;
    }

    /**
     * @param objectType The objectType to set to this User object.
     */
    public void setObjectType(String objectType) {
        this.objectType = objectType;
    }

    /**
     * @return The userPrincipalName of this User.
     */
    public String getUserPrincipalName() {
        return userPrincipalName;
    }

    /**
     * @param userPrincipalName The userPrincipalName to set to this User object.
     */
    public void setUserPrincipalName(String userPrincipalName) {
        this.userPrincipalName = userPrincipalName;
    }

    
    /**
     * @return The usageLocation of this User.
     */
    public String getUsageLocation() {
        return usageLocation;
    }

    /**
     * @param usageLocation The usageLocation to set to this User object.
     */
    public void setUsageLocation(String usageLocation) {
        this.usageLocation = usageLocation;
    }

    /**
     * @return The telephoneNumber of this User.
     */
    public String getTelephoneNumber() {
        return telephoneNumber;
    }

    /**
     * @param telephoneNumber The telephoneNumber to set to this User object.
     */
    public void setTelephoneNumber(String telephoneNumber) {
        this.telephoneNumber = telephoneNumber;
    }

    /**
     * @return The surname of this User.
     */
    public String getSurname() {
        return surname;
    }

    /**
     * @param surname The surname to set to this User Object.
     */
    public void setSurname(String surname) {
        this.surname = surname;
    }

    /**
     * @return The streetAddress of this User.
     */
    public String getStreetAddress() {
        return streetAddress;
    }

    /**
     * @param streetAddress The streetAddress to set to this User.
     */
    public void setStreetAddress(String streetAddress) {
        this.streetAddress = streetAddress;
    }

    /**
     * @return The state of this User.
     */
    public String getState() {
        return state;
    }

    /**
     * @param state The state to set to this User object.
     */
    public void setState(String state) {
        this.state = state;
    }

    /**
     * @return The preferredLanguage of this User.
     */
    public String getPreferredLanguage() {
        return preferredLanguage;
    }

    /**
     * @param preferredLanguage The preferredLanguage to set to this User.
     */
    public void setPreferredLanguage(String preferredLanguage) {
        this.preferredLanguage = preferredLanguage;
    }

    /**
     * @return The postalCode of this User.
     */
    public String getPostalCode() {
        return postalCode;
    }

    /**
     * @param postalCode The postalCode to set to this User.
     */
    public void setPostalCode(String postalCode) {
        this.postalCode = postalCode;
    }

    /**
     * @return The physicalDeliveryOfficeName of this User.
     */
    public String getPhysicalDeliveryOfficeName() {
        return physicalDeliveryOfficeName;
    }

    /**
     * @param physicalDeliveryOfficeName The physicalDeliveryOfficeName to set to this User Object.
     */
    public void setPhysicalDeliveryOfficeName(String physicalDeliveryOfficeName) {
        this.physicalDeliveryOfficeName = physicalDeliveryOfficeName;
    }

    /**
     * @return The passwordPolicies of this User.
     */
    public String getPasswordPolicies() {
        return passwordPolicies;
    }

    /**
     * @param passwordPolicies The passwordPolicies to set to this User object.
     */
    public void setPasswordPolicies(String passwordPolicies) {
        this.passwordPolicies = passwordPolicies;
    }

    /**
     * @return The mobile of this User.
     */
    public String getMobile() {
        return mobile;
    }

    /**
     * @param mobile The mobile to set to this User object.
     */
    public void setMobile(String mobile) {
        this.mobile = mobile;
    }
    
    /**
     * @return The Password of this User.
     */
    public String getPassword() {
        return password;
    }

    /**
     * @param password The mobile to set to this User object.
     */
    public void setPassword(String password) {
        this.password = password;
    }

    /**
     * @return The mail of this User.
     */
    public String getMail() {
        return mail;
    }

    /**
     * @param mail The mail to set to this User object.
     */
    public void setMail(String mail) {
        this.mail = mail;
    }
    
    /**
     * @return The MailNickname of this User.
     */
    public String getMailNickname() {
        return mailNickname;
    }

    /**
     * @param mail The MailNickname to set to this User object.
     */
    public void setMailNickname(String mailNickname) {
        this.mailNickname = mailNickname;
    }


    /**
     * @return The jobTitle of this User.
     */
    public String getJobTitle() {
        return jobTitle;
    }

    /**
     * @param jobTitle The jobTitle to set to this User Object.
     */
    public void setJobTitle(String jobTitle) {
        this.jobTitle = jobTitle;
    }

    /**
     * @return The givenName of this User.
     */
    public String getGivenName() {
        return givenName;
    }

    /**
     * @param givenName The givenName to set to this User.
     */
    public void setGivenName(String givenName) {
        this.givenName = givenName;
    }

    /**
     * @return The facsimileTelephoneNumber of this User.
     */
    public String getFacsimileTelephoneNumber() {
        return facsimileTelephoneNumber;
    }

    /**
     * @param facsimileTelephoneNumber The facsimileTelephoneNumber to set to this User Object.
     */
    public void setFacsimileTelephoneNumber(String facsimileTelephoneNumber) {
        this.facsimileTelephoneNumber = facsimileTelephoneNumber;
    }

    /**
     * @return The displayName of this User.
     */
    public String getDisplayName() {
        return displayName;
    }

    /**
     * @param displayName The displayName to set to this User Object.
     */
    public void setDisplayName(String displayName) {
        this.displayName = displayName;
    }

    /**
     * @return The dirSyncEnabled of this User.
     */
    public String getDirSyncEnabled() {
        return dirSyncEnabled;
    }

    /**
     * @param dirSyncEnabled The dirSyncEnabled to set to this User.
     */
    public void setDirSyncEnabled(String dirSyncEnabled) {
        this.dirSyncEnabled = dirSyncEnabled;
    }

    /**
     * @return The department of this User.
     */
    public String getDepartment() {
        return department;
    }

    /**
     * @param department The department to set to this User.
     */
    public void setDepartment(String department) {
        this.department = department;
    }

    /**
     * @return The lastDirSyncTime of this User.
     */
    public String getLastDirSyncTime() {
        return lastDirSyncTime;
    }

    /**
     * @param lastDirSyncTime The lastDirSyncTime to set to this User.
     */
    public void setLastDirSyncTime(String lastDirSyncTime) {
        this.lastDirSyncTime = lastDirSyncTime;
    }

    /**
     * @return The country of this User.
     */
    public String getCountry() {
        return country;
    }

    /**
     * @param country The country to set to this User.
     */
    public void setCountry(String country) {
        this.country = country;
    }

    /**
     * @return The city of this User.
     */
    public String getCity() {
        return city;
    }

    /**
     * @param city The city to set to this User.
     */
    public void setCity(String city) {
        this.city = city;
    }

    /**
     * @return The accountEnabled attribute of this User.
     */
    public String getAccountEnabled() {
        return accountEnabled;
    }

    /**
     * @param accountEnabled The accountEnabled to set to this User.
     */
    public void setAccountEnabled(String accountEnabled) {
        this.accountEnabled = accountEnabled;
    }
    
    public boolean isIsDeleted() {
        return this.isDeleted;
    }

    public void setIsDeleted(boolean isDeleted) {
        this.isDeleted = isDeleted;
    }

    @Override
    public String toString() {
        return new JSONObject(this).toString();
    }
    
    public String getManagerDisplayname(){
        return managerDisplayname;
    }
    
    public void setManagerDisplayname(String managerDisplayname){
        this.managerDisplayname = managerDisplayname;
    }
}


/**
 * The Class DirectReports Holds the essential data for a single DirectReport entry. Namely,
 * it holds the displayName and the objectId of the direct entry. Furthermore, it provides the
 * access methods to set or get the displayName and the ObjectId of this entry.
 */
//class DirectReport extends User{
//
//  private String displayName;
//  private String objectId;
//   
//  /**
//   * Two arguments Constructor for the DirectReport Class.
//   * @param displayName
//   * @param objectId
//   */
//  public DirectReport(String displayName, String objectId){
//      this.displayName = displayName;
//      this.objectId = objectId;
//  }
//
//  /**
//   * @return The diaplayName of this direct report entry.
//   */
//  public String getDisplayName() {
//      return displayName;
//  }
//
//  
//  /**
//   *  @return The objectId of this direct report entry. 
//   */
//  public String getObjectId() {
//      return objectId;
//  }
//
//}

```

## <a name="7-create-the-authentication-modelcontroller-files-for-basicfilter"></a>7. a hitelesítés Model-vezérlőt fájlok létrehozása (BasicFilter)

Igen, Java inkább részletes, de azt már majdnem elkészült. Melletti utolsó, mielőtt azt írja be a BasicFilter servlet a kérések kezelésére, nézzük írása néhány további segítő-fájlok a `ADAL4J` tárban van szüksége. 

1. Hozzon létre egy nevű fájlt `AuthHelper.java` amelyek fog közölje velünk használjuk, az állapot bejelentkezett használatának meghatározásához módszereket. Ezek a következők:

- `isAuthenticated()`módszer, amely adja eredményül, ha a felhasználó be van jelentkezve, vagy nem
- `containsAuthenticationData()`amely fog közli velünk a Ha a token vagy nem tartalmaz adatokat
- `isAuthenticationSuccessful()`amely fog mondja el a hitelesítése sikerült-e a felhasználó számára.

Kivágás és beillesztés az alábbi kódot:

```Java
package com.microsoft.aad.adal4jsample;

import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import com.microsoft.aad.adal4j.AuthenticationResult;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public final class AuthHelper {

    public static final String PRINCIPAL_SESSION_NAME = "principal";

    private AuthHelper() {
    }

    public static boolean isAuthenticated(HttpServletRequest request) {
        return request.getSession().getAttribute(PRINCIPAL_SESSION_NAME) != null;
    }

    public static AuthenticationResult getAuthSessionObject(
            HttpServletRequest request) {
        return (AuthenticationResult) request.getSession().getAttribute(
                PRINCIPAL_SESSION_NAME);
    }

    public static boolean containsAuthenticationData(
            HttpServletRequest httpRequest) {
        Map<String, String[]> map = httpRequest.getParameterMap();
        return httpRequest.getMethod().equalsIgnoreCase("POST") && (httpRequest.getParameterMap().containsKey(
                        AuthParameterNames.ERROR)
                        || httpRequest.getParameterMap().containsKey(
                                AuthParameterNames.ID_TOKEN) || httpRequest
                        .getParameterMap().containsKey(AuthParameterNames.CODE));
    }

    public static boolean isAuthenticationSuccessful(
            AuthenticationResponse authResponse) {
        return authResponse instanceof AuthenticationSuccessResponse;
    }
}
```

2. Hozzon létre egy nevű fájlt `AuthParameterNames.java` amelyek fog közölje velünk néhány megváltoztatható változók `ADAL4J` van szükség. Kivágás és pate a következőket:

```Java
package com.microsoft.aad.adal4jsample;

public final class AuthParameterNames {

    private AuthParameterNames() {
    }

    public static String ERROR = "error";
    public static String ERROR_DESCRIPTION = "error_description";
    public static String ERROR_URI = "error_uri";
    public static String ID_TOKEN = "id_token";
    public static String CODE = "code";
}
```

3. Végül a nevű fájl létrehozása `AadController.java` amely olyan, a vezérlő a MVC minta, amely közölje velünk a JSP vezérlő és elérhetővé tenni a `secure/aad` URL-cím végpontot, az alkalmazás. Ezeken kívül azt helyezni a graph lekérdezés ezt a fájlt.

Kivágás és beillesztés, a következőket:

```Java
package com.microsoft.aad.adal4jsample;

import java.net.HttpURLConnection;
import java.net.URL;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.json.JSONArray;
import org.json.JSONObject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.microsoft.aad.adal4j.AuthenticationResult;

@Controller
@RequestMapping("/secure/aad")
public class AadController {

    @RequestMapping(method = { RequestMethod.GET, RequestMethod.POST })
    public String getDirectoryObjects(ModelMap model, HttpServletRequest httpRequest) {
        HttpSession session = httpRequest.getSession();
        AuthenticationResult result = (AuthenticationResult) session.getAttribute(AuthHelper.PRINCIPAL_SESSION_NAME);
        if (result == null) {
            model.addAttribute("error", new Exception("AuthenticationResult not found in session."));
            return "/error";
        } else {
            String data;
            try {
                data = this.getUsernamesFromGraph(result.getAccessToken(), session.getServletContext()
                        .getInitParameter("tenant"));
                model.addAttribute("users", data);
            } catch (Exception e) {
                model.addAttribute("error", e);
                return "/error";
            }
        }
        return "/secure/aad";
    }

    private String getUsernamesFromGraph(String accessToken, String tenant) throws Exception {
        URL url = new URL(String.format("https://graph.windows.net/%s/users?api-version=2013-04-05", tenant,
                accessToken));

        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        // Set the appropriate header fields in the request header.
        conn.setRequestProperty("api-version", "2013-04-05");
        conn.setRequestProperty("Authorization", accessToken);
        conn.setRequestProperty("Accept", "application/json;odata=minimalmetadata");
        String goodRespStr = HttpClientHelper.getResponseStringFromConn(conn, true);
        // logger.info("goodRespStr ->" + goodRespStr);
        int responseCode = conn.getResponseCode();
        JSONObject response = HttpClientHelper.processGoodRespStr(responseCode, goodRespStr);
        JSONArray users = new JSONArray();

        users = JSONHelper.fetchDirectoryObjectJSONArray(response);

        StringBuilder builder = new StringBuilder();
        User user = null;
        for (int i = 0; i < users.length(); i++) {
            JSONObject thisUserJSONObject = users.optJSONObject(i);
            user = new User();
            JSONHelper.convertJSONObjectToDirectoryObject(thisUserJSONObject, user);
            builder.append(user.getUserPrincipalName() + "<br/>");
        }
        return builder.toString();
    }

}

```

## <a name="8-create-the-basicfilter-file-for-basicfilter-mvc"></a>8. hozza létre a BasicFilter fájlt (a BasicFilter MVC)

Hozza létre a BasicFilter fájlt a kérések a nézetből (JSP fájlok) kezelését végző végül készen áll azt.

Hozzon létre egy nevű fájlt `BasicFilter.java` , amely tartalmazza az alábbiakat:

```Java

package com.microsoft.aad.adal4jsample;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URI;
import java.net.URLEncoder;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;
import com.nimbusds.oauth2.sdk.AuthorizationCode;
import com.nimbusds.openid.connect.sdk.AuthenticationErrorResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public class BasicFilter implements Filter {

    private String clientId = "";
    private String clientSecret = "";
    private String tenant = "";
    private String authority;

    public void destroy() {

    }

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {

        if (request instanceof HttpServletRequest) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            try {

                String currentUri = request.getScheme()
                        + "://"
                        + request.getServerName()
                        + ("http".equals(request.getScheme())
                                && request.getServerPort() == 80
                                || "https".equals(request.getScheme())
                                && request.getServerPort() == 443 ? "" : ":"
                                + request.getServerPort())
                        + httpRequest.getRequestURI();
                String fullUrl = currentUri
                        + (httpRequest.getQueryString() != null ? "?"
                                + httpRequest.getQueryString() : "");
                // check if user has a session
                if (!AuthHelper.isAuthenticated(httpRequest)) {
                    if (AuthHelper.containsAuthenticationData(httpRequest)) {
                        Map<String, String> params = new HashMap<String, String>();
                        for (String key : request.getParameterMap().keySet()) {
                            params.put(key,
                                    request.getParameterMap().get(key)[0]);
                        }
                        AuthenticationResponse authResponse = AuthenticationResponseParser
                                .parse(new URI(fullUrl), params);
                        if (AuthHelper.isAuthenticationSuccessful(authResponse)) {

                            AuthenticationSuccessResponse oidcResponse = (AuthenticationSuccessResponse) authResponse;
                            AuthenticationResult result = getAccessToken(
                                    oidcResponse.getAuthorizationCode(),
                                    currentUri);
                            createSessionPrincipal(httpRequest, result);
                        } else {
                            AuthenticationErrorResponse oidcResponse = (AuthenticationErrorResponse) authResponse;
                            throw new Exception(String.format(
                                    "Request for auth code failed: %s - %s",
                                    oidcResponse.getErrorObject().getCode(),
                                    oidcResponse.getErrorObject()
                                            .getDescription()));
                        }
                    } else {
                            // not authenticated
                            httpResponse.setStatus(302);
                            httpResponse
                                    .sendRedirect(getRedirectUrl(currentUri));
                            return;
                    }
                } else {
                    // if authenticated, how to check for valid session?
                    AuthenticationResult result = AuthHelper
                            .getAuthSessionObject(httpRequest);

                    if (httpRequest.getParameter("refresh") != null) {
                        result = getAccessTokenFromRefreshToken(
                                result.getRefreshToken(), currentUri);
                    } else {
                        if (httpRequest.getParameter("cc") != null) {
                            result = getAccessTokenFromClientCredentials();
                        } else {
                            if (result.getExpiresOnDate().before(new Date())) {
                                result = getAccessTokenFromRefreshToken(
                                        result.getRefreshToken(), currentUri);
                            }
                        }
                    }
                    createSessionPrincipal(httpRequest, result);
                }
            } catch (Throwable exc) {
                httpResponse.setStatus(500);
                request.setAttribute("error", exc.getMessage());
                httpResponse.sendRedirect(((HttpServletRequest) request)
                        .getContextPath() + "/error.jsp");
            }
        }
        chain.doFilter(request, response);
    }

    private AuthenticationResult getAccessTokenFromClientCredentials()
            throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", new ClientCredential(clientId,
                            clientSecret), null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private AuthenticationResult getAccessTokenFromRefreshToken(
            String refreshToken, String currentUri) throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByRefreshToken(refreshToken,
                            new ClientCredential(clientId, clientSecret), null,
                            null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;

    }

    private AuthenticationResult getAccessToken(
            AuthorizationCode authorizationCode, String currentUri)
            throws Throwable {
        String authCode = authorizationCode.getValue();
        ClientCredential credential = new ClientCredential(clientId,
                clientSecret);
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByAuthorizationCode(authCode, new URI(
                            currentUri), credential, null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private void createSessionPrincipal(HttpServletRequest httpRequest,
            AuthenticationResult result) throws Exception {
        httpRequest.getSession().setAttribute(
                AuthHelper.PRINCIPAL_SESSION_NAME, result);
    }

    private String getRedirectUrl(String currentUri)
            throws UnsupportedEncodingException {
        String redirectUrl = authority
                + this.tenant
                + "/oauth2/authorize?response_type=code%20id_token&scope=openid&response_mode=form_post&redirect_uri="
                + URLEncoder.encode(currentUri, "UTF-8") + "&client_id="
                + clientId + "&resource=https%3a%2f%2fgraph.windows.net"
                + "&nonce=" + UUID.randomUUID() + "&site_id=500879";
        return redirectUrl;
    }

    public void init(FilterConfig config) throws ServletException {
        clientId = config.getInitParameter("client_id");
        authority = config.getServletContext().getInitParameter("authority");
        tenant = config.getServletContext().getInitParameter("tenant");
        clientSecret = config.getInitParameter("secret_key");
    }

}
```

A servlet közzététele minden mód, amely `ADAL4J` az alkalmazás futtatásához a várt lesz. Ide tartoznak:

- `getAccessTokenFromClientCredentials()`-hozzáférési jogkivonat kapja a titkos kulcs
- `getAccessTokenFromRefreshToken()`-hozzáférési jogkivonat kapja a frissítés jogkivonat
- `getAccessToken()`-megszerzi hozzáférési jogkivonat-OpenID csatlakozás flow (amely használjuk)
- `createSessionPrincipal()`-hoz létre egy egyszerű diagram API access használjuk
- `getRedirectUrl()`-kapja a redirectURL való hasonlítsa össze az portálra a megadott érték.

##<a name="compile-and-run-the-sample-in-tomcat"></a>Állíthat össze, és futtassa a minta Tomcat

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

