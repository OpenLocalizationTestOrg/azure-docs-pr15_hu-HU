<properties
    pageTitle="Első lépések Azure Active Directory Android |} Microsoft Azure"
    description="Hogyan hozhat létre egy Android alkalmazást, amely integrálódik az Azure Active Directory való bejelentkezéshez és Azure Active Directory felhívja API-k használata OAuth védett."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Azure Active Directory integrálása az Android-alkalmazásokban

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Ha egy asztali alkalmazás fejleszt, Azure Active Directory teszi egyszerű és az, hogy a felhasználók hitelesítő az Active Directory-fiókok használatával egyszerű.  Azt is lehetővé teszi, hogy az alkalmazás bármely webes API védik az Azure Active Directory, például az Office 365 API-k és az Azure API biztonságos módon használhatnak.

A védett forrásokat kell Android ügyfelek esetén Azure AD az Active Directory Authentication Library vagy ADAL biztosít.  ADAL-féle kizárólagos életben célja, hogy az alkalmazás, hogy a hozzáférési jogkivonat könnyen.  Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre az Android teendőlista alkalmazás, amely:

-   Keresése access-tokenek a hívásához a teendősáv lista API [OAuth 2.0-s hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)használatával.
-   A felhasználó teendőlista módja
-   Jelek ki a felhasználókat.

Első lépésként szüksége lesz egy Azure AD-bérlő, ahol felhasználók létrehozása és az alkalmazás regisztrálása.  Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

> [AZURE.TIP] Az Előnézet az új [developer Portal segítségével](https://identity.microsoft.com/Docs/Android) , amely segít a kezdeti lépéseket az Azure Active Directory címtárral mindössze néhány perc múlva próbálkozzon!  A developer Portal segítségével végigvezeti a folyamaton, az alkalmazás rögzítése és Azure Active Directory integrálása a kódot.  Ha végzett, lehetősége van egy egyszerű alkalmazás, amely hitelesíthet a bérlő és egy kódmentes, amelyek képesek fogadja el a tokenek, és végezze el az érvényesítési felhasználókat. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Lépés: 1: Töltse le és a Node.js REST API teendők minta kiszolgáló futtatása

Ez a példa akkor írt kifejezetten szemben a meglévő minta egyetlen bérlő tennivaló REST API-t a Microsoft Azure Active Directory készítéséhez. Ez a rövid útmutató előzetesen szükséges.

Hogyan állíthatja ezt be információkért keresse fel a meglévő minták:

* [Microsoft Azure Active Directory minta REST API-val szolgáltatás Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Lépés: 2: A Microsoft Azure AD-bérlő regisztrálhatja a webes API

**Mit tegyek módon?**

*A Microsoft Active Directory támogatja a kétféle alkalmazások felvétele. Felhasználók és alkalmazások (akár a weben vagy-eszközön futó alkalmazás) e webes API-hoz elérhető szolgáltatásokat kínáló API-khoz webhelyet. Ebben a lépésben regisztrál az alábbi példa teszteléshez helyileg futtatja webes API-val. A szokásos módon a webes API lenne a többi szolgáltatás, amely azt szeretné, hogy egy app hozzáférésének funkciót kínál. Microsoft Azure Active Directory megvédheti bármely végpont!*

*Itt azt is feltételezve, hogy a teendők REST API-t a fentiekben regisztrál, de ez számos bármely webes API Azure Active Directory védelemmel ellátni kívánt.*

A webes API regisztrálhatja a Microsoft Azure Active Directory lépések

1. Jelentkezzen be az [Azure kezelőportálja segítségével](https://manage.windowsazure.com).
2. Kattintson az Active Directory a bal oldali navigációs funkcióját.
3. A címtár-bérlő hol szeretné rögzíteni a minta alkalmazása gombra.
4. Kattintson az alkalmazások fülre.
5. A fiók kattintson a Hozzáadás gombra.
6. Kattintson a "Szervezeten elkészítésének az alkalmazás hozzáadása".
7. Adja meg az alkalmazást, például "TodoListService", válassza a "Webes alkalmazás és/vagy webes API" rövid nevet, és kattintson a Tovább gombra.
8. Bejelentkezési URL-CÍMÉT, írja be az alap URL-címet a minta, amely alapértelmezés szerint be van `https://localhost:8080`.
9. Írja be az alkalmazás azonosítója URI `https://<your_tenant_name>/TodoListService`tagjára, `<your_tenant_name>` a Azure AD-bérlő a nevet.  Kattintson az OK gombra a regisztráció befejezéséhez.
10. Miközben továbbra is az Azure-portálon kattintson a beállítás lapon, az alkalmazás.
11. **Keresse meg az ügyfél-azonosító értékét, és másolja a vágólapra a következő**, szüksége lesz a később az alkalmazás beállításakor.

## <a name="step-3-register-the-sample-android-native-client-application"></a>3 lépés: A minta Android natív ügyfele alkalmazás regisztrálása

A webes alkalmazás regisztrálása első lépése. Ezután kell megállapítani, hogy a Azure Active Directory az alkalmazásról. Ez lehetővé teszi, hogy az alkalmazás az imént bejegyzett webes API kommunikáció

**Mit tegyek módon?**  

*Ahogy azt már említettük, a Microsoft Azure Active Directory alkalmazások hozzáadásával kétféle támogatja. Felhasználók és alkalmazások (akár a weben vagy-eszközön futó alkalmazás) e webes API-hoz elérhető szolgáltatásokat kínáló API-khoz webhelyet. Ebben a lépésben regisztrál az alkalmazás a következő példában. Amely ahhoz, hogy az alkalmazás engedélyezni szeretné a webes API eléréséhez kérelem imént bejegyzett kell végeznie. Azure Active Directory is engedélyezni az alkalmazás kérése bejelentkezési kivéve, ha van regisztrálva fog elutasítja! A modell biztonsága eleménél.*

*Itt azt is feltételezve, hogy a fentiekben minta alkalmazás regisztrál, de ez számos bármely alkalmazásban, ha fejlesztéséhez.*

**Miért tapasztalok lehet elhelyez az alkalmazások és a webes API-val egy bérlői webhelyemen?**

*Akkor előfordulhat, hogy rendelkezik kitalálhatják, sikerült generál hozzáférő külső API-t az Azure Active Directory regisztrált egy másik bérlői az alkalmazás. Az ehhez szükséges lépéseket, ha az alkalmazás az API használatának engedélyezése az ügyfelekkel jelenik meg. A szép része, a jóváhagyását adja meg az iOS az Active Directory Authentication Library gondoskodik! Azt eléréséhez kattintson speciális funkcióit, látni fog a része-fontos hozzáférhet az csomagot a Microsoft APIs Azure és az Office, valamint egyéb szolgáltató szükséges munkamennyiség. Most mert regisztrálta a webes API-val és a ugyanahhoz a bérlőhöz az alkalmazás nem látható minden felhasználó hozzájárul ahhoz kérő üzenet. Általában ez a helyzet, ha egy alkalmazás csak a saját cégen belül használandó számára.*

1. Jelentkezzen be az [Azure kezelőportálja segítségével](https://manage.windowsazure.com).
2. Kattintson az Active Directory a bal oldali navigációs funkcióját.
3. A címtár-bérlő hol szeretné rögzíteni a minta alkalmazása gombra.
4. Kattintson az alkalmazások fülre.
5. A fiók kattintson a Hozzáadás gombra.
6. Kattintson a "Szervezeten kialakítása az alkalmazás hozzáadása".
7. Adja meg az alkalmazást, például "TodoListClient Android rendszerű", jelölje be "natív ügyfélalkalmazás", rövid nevét, és kattintson a Tovább gombra.
8. Az átirányítás URI írja be a `http://TodoListClient`.  Kattintson a Befejezés gombra.
9. Kattintson az alkalmazás a beállítás fülre.
10. Keresse meg az ügyfél-azonosító értékét, és másolja a vágólapra különítsen, szüksége lesz a később az alkalmazás beállításakor.
11. Az "Engedélyek való egyéb alkalmazások" kattintson a "Alkalmazás hozzáadása".  Válassza a "Egyéb" a "Megjelenítése" tartalmazó legördülő listára, és kattintson a felső pipára.  Keresse meg és a TodoListService kattintson, és kattintson az alsó jelölőnégyzet be van jelölve az alkalmazás hozzáadása parancsra.  Jelölje ki a "A hozzáférési TodoListService" a "Meghatalmazott engedélyeit" legördülő listából, és mentse a konfigurációt.



Maven tesztelése készítéséhez használható a pom.xml felső szintjén

  * Ez egy tetszés szerinti mappába a repó klónozhatja:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Kövesse a lépéseket [Prerequests szakaszban a maven tesztelése az Android-alapú beállítása](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * A SDK 19 telepítő irányító
  * Ugrás a legfelső szintű mappát, ahol klónozva-e a repó
  * A következő parancs futtatásával: mvn tiszta telepítése
  * Váltson a rövid útmutató minta arra a könyvtárra: samples\hello CD-hez
  * A következő parancs futtatásával: mvn android: telepítése android: futtassa a
  * Meg kell jelennie alkalmazás indítása
  * Írja be a próba felhasználói hitelesítő adatokat, próbálja ki!

Üveg csomagok is nyújtanak a aar csomag mellett.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Lépés: 4: Töltse le az Android ADAL, és vegye fel a Holdas munkaterülete

Végeztünk, egyszerűen a tár a Android projektben használata több lehetőség közül választhat:

* A tár importálja Holdas és hivatkozás az alkalmazás a forráskód is használhatja.
* Android Studio használata esetén *aar* csomag formázással, és a bináris hivatkozást.

####<a name="option-1-source-zip"></a>1 beállítást: Zip-adatforrás

A forráskódot egy példányának letöltése, a lap jobb oldalán kattintson a "Letöltése ZIP-", vagy kattintson [ide](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>2 lehetőség: Forrás mely számjegy keresztül

Ismerkedés a via mely számjegy SDK forráskódot csak írja be:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Lehetőség 3: Bináris Gradle keresztül

A központi repó maven tesztelése a bináris elérheti. Az alábbi képlettel történik AAR csomagot is tartalmazza a projektben szereplő AndroidStudio:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>4-es lehetőség: aar keresztül maven tesztelése

A beépülő modul m2e használatakor az Holdas megadhatja a függőség a pom.xml fájl:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Választási lehetőség 5: üveg csomag könyvtárral lenne mappában
A üveg fájl beolvasása maven tesztelése a repó, és a projekt *könyvtárral lenne* mappába húzza. Szeretne másolni, valamint a projekthez a szükséges erőforrások, mivel a üveg csomagok nem veszi őket.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Lépés az 5: Android ADAL mutató hivatkozások hozzáadása a projekthez


2. A projekt mutató hivatkozás hozzáadása, és adja meg azt az Android-tárba. Ha nem biztos művelet című [Kattintson ide a további információt] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. A projekt függés hibakereséshez a a project-beállítások hozzáadása

4. A projekt AndroidManifest.xml fájl mentését frissítése:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Hozzon létre egy példányának AuthenticationContext elemre a fő tevékenységét. A hívás részleteit a fontos túlra, de egy jó kezdő megjeleníti az [Android natív ügyfél minta](https://github.com/AzureADSamples/NativeClient-Android)elérheti. Az alábbi képen:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Megjegyzés: mContext egy mezőt a tevékenység

8. Másolja a kódot következő AuthenticationActivity végén kezelje a felhasználói hitelesítő adatokat ad meg, és engedélyezési kód kap:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. A visszahívási definiálása kéréséhez jogkivonat,

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Végül kérje meg a jogkivonat, hogy a visszahívás használata:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

A paraméterek magyarázata:

  * Erőforrás szükség, és az elérni kívánt erőforrás.
  * ClientID szükség, és a AzureAD portál származik.
  * A telepítő redirectUri, a csomagnév. Még nem kell adni a acquireToken híváshoz szükséges.
  * Kérje meg a hitelesítő adatait, és ugorja át a gyorsítótár és a cookie-k segít PromptBehavior.
  * A visszahívás után engedélyezési kód cseréje a jogkivonat neve.

  A visszahívási AuthenticationResult accesstoken rendelkező objektum lesz, dátum lejárt, valamint idtoken adatait.

Nem kötelező: **acquireTokenSilent**

**AcquireTokenSilent** kezelheti a gyorsítótár és jogkivonat frissítés felhívhatja. Szinkronizálási verzióját is biztosít. Felhasználóazonosító paraméterként fogadja el.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Ügynök**: Microsoft Intune vállalati portál alkalmazás biztosítja az ügynök összetevőt. ADAL lesz a ügynök fiókot, ha egy felhasználói fiókhoz jön létre a hitelesítő és fejlesztői használata dönt, hogy nem ugorja át. Fejlesztőeszközök rendelkező ügynök felhasználó hagyhat ki:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Fejlesztőeszközök kell ügynök használati speciális redirectUri regisztrálni. RedirectUri msauth://packagename/Base64UrlencodedSignature formátumban van. A redirecturi beszerzése az alkalmazást, az "brokerRedirectPrint.ps1" parancsfájl használatával, vagy használja az API-hívás mContext.getBrokerRedirectUri. Az aláíró tanúsítvány aláírás kapcsolatban.

 Aktuális ügynök modell van egy felhasználó számára. AuthenticationContext API módszer a ügynök felhasználó megszerezni biztosít.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Ügynök felhasználói fog adja vissza, ha a fiókhoz érvényes.

 Az alkalmazás jegyzék AccountManager fiókok jogosultsággal kell rendelkeznie: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


Ez a forgatókönyv használ, van, hogy mit kell sikeresen integrálása az Azure Active Directory. Ha további példákat a munkát, látogasson el a AzureADSamples / GitHub tárházából.

## <a name="important-information"></a>Fontos tudnivalók

### <a name="customization"></a>Testreszabás

Tár projekterőforrások szerint az alkalmazás erőforrások felülírható. Ez történik, amikor az alkalmazás készíti. Emiatt testre szabhat hitelesítési tevékenység elrendezést a kívánt módon. Győződjön meg arról, hogy ADAL uses(Webview) a vezérlők azonosítója tartásához szüksége.

### <a name="broker"></a>Ügynök

Ügynök összetevő érkeznek meg a Microsoft Intune vállalati portál alkalmazást. Fiókot az Account Manager jön létre. Fióktípus "com.microsoft.workaccount". Csak egyetlen egyszeri bejelentkezési fiók teszi lehetővé. Az alkalmazások egyik eszköz kérdés elvégzése után az egyszeri bejelentkezés cookie-k ennél a felhasználónál hoz létre.

### <a name="authority-url-and-adfs"></a>Hitelesítésszolgáltató URL-címek és a ADFS

ADFS azonban nem ismeri fel gyártási STS, így kell kapcsolni a példány feltárás és AuthenticationContext konstruktor hamis át.

Hitelesítésszolgáltató URL-cím van szüksége, STS, amelyben és a bérlői webhely neve: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Gyorsítótár elemek lekérdezése

ADAL az alapértelmezett gyorsítótárat a SharedPreferences néhány egyszerű gyorsítótár lekérdezés funkciókat nyújt. Az aktuális gyorsítótár amely letölthető a AuthenticationContext:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Ha testre szeretné szabni, hogy a gyorsítótár végrehajtása is lehet nyújtani.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL itt meg kérdés viselkedése lehetőséget. PromptBehavior.Auto jelenik meg a felhasználói felület, ha a frissítés token érvénytelen és felhasználói hitelesítő adatok szükségesek. PromptBehavior.Always ugorja át a gyorsítótár használatát, és a mindig legyen látható a felhasználói felület.

### <a name="silent-token-request-from-cache-and-refresh"></a>Csendes jogkivonat kérése gyorsítótár és frissítése

Ez a módszer nem használja a felhasználói felület pop be, és nincs szükség a tevékenységet. Ad vissza jogkivonat gyorsítótárból, ha elérhető. Ha token lejárt, azt megpróbálja frissíteni az adatokat. Ha a frissítés token lejárt vagy nem sikerült, AuthenticationException ad vissza.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Szinkronizálási hívás ezzel a módszerrel is teheti. Beállíthatja a visszahívási null vagy acquireTokenSilentSync használja.

### <a name="diagnostics"></a>Diagnosztikai

Az elsődleges információforrások a problémák diagnosztizálása a következők:

+ A kivételek
+ Naplók
+ Hálózati nyomkövetés

Emellett látható, korrelációs azonosítót központi a diagnosztika a tárban. Beállíthatja, hogy a korrelációs azonosítók, ha azt szeretné, hogy egy ADAL összehangolására kérés / alapon az egyéb műveletek a kód kérése. Ha nem állít összefüggések azonosítóra, majd ADAL hoz létre a véletlen egyik and összes üzeneteket és a hálózati hívások fog megjelölni a Korrelációazonosító. A saját azonosító módosítások minden kérésre jön létre.

#### <a name="exceptions"></a>A kivételek

Ez a nyilvánvalóan a első diagnosztikai. Próbálja meg a szükséges hasznos hibaüzenetek jelennek meg. Ha talál, amely nem árt kérjük fájl a problémát, és tudassa velünk. Adjon meg is eszköz információt, például a modell és SDK #.

#### <a name="logs"></a>Naplók

Naplózás segítséget problémáinak diagnosztizálása használható létrehozása a tárban úgy is beállíthatja. Naplózás azáltal, hogy a következő hívás konfigurálása a visszahívás, amelyekkel a ADAL kéz ki az egyes naplózási üzenetet módon jön létre.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Üzenetek írható egy egyéni naplófájl alább látható módon. Sajnos nem nincs mód naplók ahhoz, hogy egy eszközt. Vannak bizonyos szolgáltatások, amelyek segíthetnek az adatokkal. Akkor is is készlet saját, például a fájl küldése a kiszolgálón.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Naplózási szintjének

+ Error(Exceptions)
+ Warn(WARNING)
+ Info (tájékoztatásra)
+ Részletes (További részletek)

Beállíthatja, hogy a naplózási szint jelennek meg:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Az összes naplózás logcat kívül minden egyéni napló visszahívást elküldi.
Egy fájl űrlap logcat napló mint megjelenített belog szerezheti be:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Adb parancsok kapcsolatos további példákat: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Hálózati nyomkövetés

Különböző eszközök segítségével rögzítheti a HTTP-forgalmat által generált ADAL.  Az a leghasznosabb, ha már jól ismert OAuth protokollt, vagy meg kell adnia a diagnosztikai adatokat a Microsoftnak vagy más támogatási csatornák.

Fiddler a legegyszerűbb HTTP nyomkövetési eszközt.  Az alábbi hivatkozások használatával megfelelően rekord ADAL hálózati forgalmának engedélyezésére felfelé a telepítő.  Annak érdekében, hogy a hasznos lehet fiddler vagy egy másik eszközre, például Charles rögzítése titkosítatlan SSL-forgalom konfigurálása szükség.  Megjegyzés: A nyomkövetési naplók ily módon generált is tartalmazhatnak kiemelt jogosultságokkal rendelkező információt, például a hozzáférési jogkivonat, felhasználónevét és jelszavát.  Gyártási fiókok használja, ha nem osztja meg ezeket a nyomkövetési naplók 3 személyekkel.  Ha valakinek a nyomkövetési adja meg annak érdekében, hogy a technikai támogatás kérése kell Reprodukálja a probléma ideiglenes fiókkal, a felhasználóneveket és jelszavakat, amely nem bánja, megosztása.

+ [Az Android-alapú Fiddler beállítása](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [ADAL Fiddler szabályok beállítása](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Párbeszédpanel mód
acquireToken módszert tevékenység nem támogatja a párbeszédpanel megjelenítése.

### <a name="encryption"></a>Titkosítás:

ADAL titkosítja a tokenek és alapértelmezés szerint a SharedPreferences áruházból. Megnézheti a StorageHelper osztály a részletek megtekintéséhez. Android-alapú AndroidKeyStore 4.3(API18) biztonságos tárolására szolgáló titkos kulcsokat jelent meg. ADAL használ, amely API18 és felett. Ha alsó SDK verziók ADAL használni szeretne, meg kell adnia a AuthenticationSettings.INSTANCE.setSecretKey titkos kulcs

### <a name="oauth2-bearer-challenge"></a>Oauth2 hitelesítési mód Bearer beavatkozás igazolására szolgáló eljárás

AuthenticationParameters úgy juthat az oauth2 hitelesítési mód bearer beavatkozás igazolására szolgáló eljárás a authorization_uri funkciókat nyújt.

### <a name="session-cookies-in-webview"></a>Munkamenet-webnézetben cookie-k használata

Android webnézet nem törli a munkamenet cookie-k alkalmazás bezárása után. Az alábbi példakódot kezelhető:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
További információ a cookie-k: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Erőforrás felülbírálása

A ADAL tár tartalmazza a következő két ProgressDialog üzeneteket angol nyelvű karakterláncok.

Az alkalmazás célszerű felülírni honosított karakterláncok van szükség.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM párbeszédpanel
Adal 1.1.0 verzióval keresztül WebViewClient onReceivedHttpAuthRequest eseményt feldolgozott NTLM párbeszédpanel. Testre szabható párbeszédpanel Elrendezés és a karakterláncok.

### <a name="cross-app-sso"></a>Idegen-alkalmazás SSO
Megtudhatja, [hogyan engedélyezhető az idegen-alkalmazás SSO ADAL használata Android-eszközön](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
