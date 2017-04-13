<properties
    pageTitle="Azure Active Directory B2C: A webes API hívhat fel az Android-alkalmazások |} Microsoft Azure"
    description="Ez a cikk bemutatja, hogyan hozhat létre a "Feladatlista" Android alkalmazást, amely felhívja Node.js webes API 2.0-s OAuth bearer tokenek használatával. Az Android alkalmazást mind a webes API segítségével Azure Active Directory B2C kezelése felhasználói identitások és a felhasználók hitelesítési."
    services="active-directory-b2c"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure Active Directory B2C: Hívás webes API-val Android alkalmazásból

> [AZURE.WARNING] Ebben az oktatóanyagban néhány fontos frissítések területen kifejezetten használata ADAL Android B2C eltávolítandó szükséges.  Friss utasításokat az Azure Active Directory B2C az Android-alkalmazásokban a következő hét közzététele fogjuk, és javasoljuk, hogy mindaddig lenyomva.  De csak szeretne kipróbálni dolog, amit meg, ha nyugodtan az alábbi cikkben folytatásához.



Azure Active Directory (Azure Active Directory) B2C használatával hatékony önkiszolgáló identitás kezelése szolgáltatások hozzáadása az Android-alkalmazások és a webes API-khoz néhány rövid lépéseket. Ez a cikk bemutatja, hogyan hozhat létre a "Feladatlista" Android alkalmazást, amely felhívja Node.js webes API 2.0-s OAuth bearer tokenek használatával. Az Android alkalmazást mind a webes API segítségével Azure Active Directory B2C kezelése felhasználói identitások és a felhasználók hitelesítési.

A quickstart útmutató igényel, hogy rendelkezik-e a webes API B2C teljesen használata az Azure Active Directory védett. Úgy alakítottuk van egy .NET és a Node.js szeretne használni. A segédlet feltételezi, hogy helyesek-e a Node.js webes API minta. További tudnivalókért lásd: az [Azure Active Directory B2C webes API a Node.js tanulásra](active-directory-b2c-devquickstarts-api-node.md).

Azure Active Directory védett forrásokat kell Android ügyfelek, az Active Directory hitelesítési tár (ADAL) biztosít. ADAL kizárólagos célja megkönnyítik az alkalmazás, hogy a hozzáférési jogkivonat. A bemutatják, hogy milyen könnyen kerül, ebből az útmutatóból azt fogja összeállítása Android Tennivaló lista alkalmazás, amely:

- Hozzáférési jogkivonat, amely az [OAuth 2.0-s hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)használatával hívja fel a feladatlistát API kap.
- A felhasználók a feladatlisták kap.
- A jelek felhasználók ki.

> [AZURE.NOTE] Ez a cikk nem tárgyalja a bejelentkezési, előfizetési megvalósításáról és profilok kezelése Azure Active Directory B2C használatával. Hívás a webes API-khoz után a felhasználó hitelesítése koncentrál. Ha még nem tette meg, érdemes együtt kezd [.NET web app bevezetés oktatóprogram](active-directory-b2c-devquickstarts-web-dotnet.md) Ha többet szeretne tudni az Azure Active Directory B2C alapjai.

## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői. A könyvtár (az összes a felhasználók, alkalmazások, csoportok és az egyéb tároló). Ha egy már nem rendelkezik, a [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt ebből az útmutatóból továbbra is.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Következő lépésként meg kell B2C címtárában-alkalmazás létrehozása. Ezzel kap, hogy szüksége van az alkalmazás biztonságos kommunikáció Azure AD-információkat. Az alkalmazás és a webes API jeleníti meg a egyetlen **Azonosítója** ebben az esetben, mert egy logikai alkalmazás tartalmazzák. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md). Győződjön meg arról, hogy:

- Egy **webalkalmazás**olyan/**webes API** -alkalmazásban.
- Adja meg `urn:ietf:wg:oauth:2.0:oob` **Válasz URL**-címként. Célszerű a kódot a következő példában az alapértelmezett URL-CÍMÉT.
- Hozzon létre egy **alkalmazás titkos** az alkalmazás, és másolja a vágólapra. Később lesz szüksége. Figyelje meg, hogy ezt az értéket kell lennie, [XML-escape](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használat előtt.
- Másolja az alkalmazás rendelt **Azonosítója** . Is szüksége lesz a később.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. Az alkalmazás három identitás élményt tartalmazza: regisztráció, jelentkezzen be, és jelentkezzen be a Facebook használatával.  Kell minden típusú több házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)leírt módon. Amikor a három házirendek hoz létre, ne felejtse el:

- A **megjelenítendő név** és egyéb előfizetési attribútumai válassza az előfizetés házirend.
- Válassza a **megjelenítendő név** és a **Objektumazonosító** alkalmazás jogcímalapú minden házirend. Megadhatja, hogy más igények is.
- Létrehozásuk után, másolja a vágólapra a minden szabály **nevét** . Az előtag rendelkeznie kell `b2c_1_`.  A házirend nevek újabb mobiltelefon szükséges.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a három házirendeket, készen áll az alkalmazás összeállítása.

Megjegyzés: Ez a cikk nem terjed ki az imént létrehozott házirendeket használatáról. Hogyan működnek a házirendek az Azure Active Directory B2C kapcsolatos további tudnivalókért kezdje az [oktatóprogram .NET web app az első lépések](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Töltse le a kódot.

Ezen oktatóprogram [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android)kódját. A minta létrehozásához a menet közben, akkor [Töltse le a skeleton projekt .zip fájlként](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-Android.git
```

> [AZURE.NOTE] **Az oktatóprogram elvégzéséhez skeleton letöltéséhez szükséges.** A teljes funkcionalitású Android-alkalmazások végrehajtási összetettsége miatt a skeleton rendelkezik UX kódot, amely ebből az oktatóanyagból befejezése után futtathatók. Ez azt méri időtakarékos fejlesztők számára. A UX kód nem a témakör bemutatja, hogyan B2C felvétele Androidos az alkalmazás a germane.

A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip) is érhető el vagy a `complete` ugyanazt a tárházba részlege.

Maven tesztelése készítéséhez használható `pom.xml` a legfelső szinten.

  1. Kövesse a [állíthatja be az Android-alapú maven tesztelése előfeltételekről](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)szakaszában.
  2. Állítsa be a SDK 21-irányító.
  3. Ugrás a legfelső szintű mappát, ahol klónozva-e a repó.
  4. A parancs futtatása `mvn clean install`.
  5. Váltson arra a quickstart útmutató minta `cd samples\hello`.
  6. A parancs futtatása `mvn android:deploy android:run`.

Meg kell jelennie a alkalmazás indítása. Írja be a próba felhasználói hitelesítő adatait, és próbálja ki.

Java archív (üveg) csomagok is nyújtanak az Android archív (AAR) csomag mellett.

## <a name="download-the-android-adal-and-add-it-to-your-android-studio-workspace"></a>Töltse le az Android ADAL, és vegye fel az Android Studio munkaterület

A tár a Android projektben használatára vonatkozó lehetőség közül választhat:

* A tár importálja Holdas és hivatkozás az alkalmazás a forráskód is használhatja.
* Android Studio használata esetén a AAR csomag formázással, és a bináris hivatkozást.

### <a name="option-1-binaries-via-gradle-recommended"></a>1 beállítást: Bináris keresztül Gradle (ajánlott)

A bináris amely letölthető a maven tesztelése központi repó. A projekt Android Studio beépíthetők a AAR csomag (például az `build.gradle`) ily módon:

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
    compile('com.microsoft.aad:adal:2.0.1-alpha') {
        exclude group: 'com.android.support'
    } // Recent version is 2.0.1-alpha
}
```

### <a name="option-2-aar-via-maven"></a>2 lehetőség: AAR keresztül maven tesztelése

Ha a `m2e` Holdas beépülő modult, adhatja meg a függőség a `pom.xml` fájl:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>2.0.1-alpha</version>
    <type>aar</type>
</dependency>
```

### <a name="option-3-source-via-git-last-resort"></a>3 beállítás: A forrás keresztül mely számjegy (legutóbbi megoldásként)

A via mely számjegy SDK forráskódot juthat, írja be:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

Használja a ág **Konvergencia.**

## <a name="set-up-your-configuration-file"></a>Állítsa be a konfigurációs fájl

A beállított konfiguráció használata az Android-projekt beállítása az B2C portálon korábbi.

Nyissa meg `helpes/Constants.java` , és töltse ki az értékeket, például az alábbiakat:

```

package com.microsoft.aad.taskapplication.helpers;

import com.microsoft.aad.adal.AuthenticationResult;

public class Constants {

    public static final String SDK_VERSION = "1.0";
    public static final String UTF8_ENCODING = "UTF-8";
    public static final String HEADER_AUTHORIZATION = "Authorization";
    public static final String HEADER_AUTHORIZATION_VALUE_PREFIX = "Bearer ";

    // -------------------------------AAD
    // PARAMETERS----------------------------------
    public static String AUTHORITY_URL = "https://login.microsoftonline.com/hypercubeb2c.onmicrosoft.com/";
    public static String CLIENT_ID = "<your application id>";
    public static String[] SCOPES = {"<your application id>"};
    public static String[] ADDITIONAL_SCOPES = {""};
    public static String REDIRECT_URL = "<redirect uri>";
    public static String CORRELATION_ID = "";
    public static String USER_HINT = "";
    public static String EXTRA_QP = "";
    public static String FB_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNIN_POLICY = "B2C_1_<your policy>";
    public static String EMAIL_SIGNUP_POLICY = "B2C_1_<your policy>";
    public static boolean FULL_SCREEN = true;
    public static AuthenticationResult CURRENT_RESULT = null;
    // Endpoint we are targeting for the deployed WebAPI service
    public static String SERVICE_URL = "http://localhost:3000/tasks";

    // ------------------------------------------------------------------------------------------

    static final String TABLE_WORKITEM = "WorkItem";
    public static final String SHARED_PREFERENCE_NAME = "com.example.com.test.settings";


}


```
- `SCOPES`: A hatókörök, adja meg a kiszolgálóra, amelyet szeretne kérése a kiszolgálón, amikor a felhasználó bejelentkezik. A sikeres B2C előzetes verziójának `client_id`. Azonban ez várhatóan átállítása `read scopes` a jövőben. A dokumentum, amely akkor következik be, amikor frissíti.
- `ADDITIONAL_SCOPES`: Érdemes az alkalmazás használata További hatókörök. A várható a jövőben használható.
- `CLIENT_ID`: Az alkalmazás azonosítója szerezte be a portálon.
- `REDIRECT_URL`: Az átirányítás, ahol a közzéteendő biztonsági jogkivonat várt.
- `EXTRA_QP`: Bármilyen további szeretné átadni a kiszolgáló URL-címként kódolt formátumban.
- `FB_POLICY`: A házirend hívott. A legfontosabb része-e segédlet.
- `EMAIL_SIGNIN_POLICY`: A házirend hívott. Az e segédlet legfontosabb kijelzőjét.
- `EMAIL_SIGNUP_POLICY`: A házirend hívott. A legfontosabb része-e segédlet.

## <a name="add-references-to-android-adal-to-your-project"></a>Android ADAL mutató hivatkozások hozzáadása a projekthez

> [AZURE.NOTE]  ADAL az Android-alapú hitelesítés meghívásához leképezés-alapú modellt használ. Módok "űrlapelrendezések" az alkalmazást a munkájához. Ez a teljes minta, és az Android, az összes ADAL központok módok – kezelése és közöttük át az információkat.

Első lépésként Android tájékoztatása az alkalmazást, például a használni kívánt leképezést elrendezését. Következő módok később az oktatóprogram részletesen részletesen.

A projekt frissítése `AndroidManifest.xml` fájl mentését a módok mindegyikét:

```
   <?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.microsoft.aad.taskapplication"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="11"
        android:targetSdkVersion="19" />

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name="com.microsoft.aad.adal.AuthenticationActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:exported="true"
            android:label="@string/title_login_hello_app" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.LoginActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.UsersListActivity"
            android:label="@string/title_activity_feed" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.SettingsActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.AddTaskActivity"
            android:label="@string/title_activity_settings" >
        </activity>
        <activity
            android:name="com.microsoft.aad.taskapplication.ToDoActivity"
            android:label="@string/app_name" >
        </activity>
    </application>

</manifest>    
```

Amint látható, meg kell határoznia öt tevékenységeket. Ezek közül az összes fogja használni.

- `AuthenticationActivity`: Ez az ADAL származik, és a bejelentkezési webes nézet biztosít.
- `LoginActivity`: Ez a bejelentkezési házirendek és az egyes házirend gombokra kattintva jeleníti meg.
- `SettingsActivity`: Segítségével a futásidőben alkalmazás beállításainak módosítása.
- `AddTaskActivity`: Segítségével a tevékenységek hozzáadása az Azure Active Directory eső REST API.
- `ToDoActivity`: Ez egy, a fő tevékenység feladatokat megjelenítő.

## <a name="create-the-sign-in-activity"></a>A bejelentkezési tevékenység létrehozása

Hozzon létre egy fő tevékenységet, és hívja meg `LoginActivity`.

Hozzon létre egy nevű fájlt `LoginActivity.java`.

Kell a tevékenység inicializálni és hozzáadása, amelyek a felhasználói felület fog néhány gombbal. Az ismert, ha Ön írt Android kód előtt.

```
import android.app.Activity;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;

/**
 * Created by brwerner on 9/17/15.
 */
public class LoginActivity extends Activity {

    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_signin);
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        Button button = (Button) findViewById(R.id.signin_facebook);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.FB_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signin_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNIN_POLICY);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.signup_email);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(LoginActivity.this, ToDoActivity.class);
                intent.putExtra("thePolicy", Constants.EMAIL_SIGNUP_POLICY);
                startActivity(intent);

            }
        });

    }

}


```
Most már létrehozott gomb látható, hívja fel a `ToDoActivity` leképezés (amely felhívja ADAL, ha szüksége van egy token). Ez a tevékenység egy hivatkozást, valamint egy további paraméter használatával ugyanúgy működnek. A felesleges paramétert a `intent.putExtra()` módot. Definiálása `"thePolicy"` használatával a megadott `Constants.java`. Ez azt jelenti az szándékának milyen házirend-hitelesítés során meghívásához.

## <a name="create-the-settings-activity"></a>A beállítások tevékenység létrehozása

Ez az a felhasználói felület beállításait feltöltő tevékenységet.

Hozzon létre egy nevű fájlt `SettingsActivity.java` az egyszerű létrehozása, olvasása, módosítása és törlése (CRUD) műveletek.

```
 package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.content.SharedPreferences;
import android.content.SharedPreferences.Editor;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.Switch;
import android.widget.TextView;

import com.microsoft.aad.taskapplication.helpers.Constants;

/**
 * Settings page to try broker by options
 */
public class SettingsActivity extends Activity {

    //private CheckBox checkboxAskBroker, checkboxCheckBroker;
    private Switch fullScreenSwitch;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_settings);

        loadSettings();
//      checkboxAskBroker = (CheckBox) findViewById(R.id.askInstall);
//      checkboxCheckBroker = (CheckBox) findViewById(R.id.useBroker);

        Button save = (Button) findViewById(R.id.settingsSave);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                TextView textView = (TextView)findViewById(R.id.authority);
                Constants.AUTHORITY_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.resource);
            //    Constants.SCOPES = textView.getText().toString();

                textView = (TextView)findViewById(R.id.clientId);
                Constants.CLIENT_ID = textView.getText().toString();

                textView = (TextView)findViewById(R.id.extraQueryParameters);
                Constants.EXTRA_QP = textView.getText().toString();

                textView = (TextView)findViewById(R.id.redirectUri);
                Constants.REDIRECT_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                Constants.SERVICE_URL = textView.getText().toString();

                textView = (TextView)findViewById(R.id.serviceUrl);
                textView.setText(Constants.SERVICE_URL);

                textView = (TextView)findViewById(R.id.fb_signin);
                textView.setText(Constants.FB_POLICY);

                textView = (TextView)findViewById(R.id.email_signin);
                textView.setText(Constants.EMAIL_SIGNIN_POLICY);

                textView = (TextView)findViewById(R.id.email_signup);
                textView.setText(Constants.EMAIL_SIGNUP_POLICY);

                textView = (TextView)findViewById(R.id.correlationId);
                textView.setText(Constants.CORRELATION_ID);

                fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
                Constants.FULL_SCREEN = fullScreenSwitch.isChecked();

                finish();
            }
        });


    }

    private void loadSettings() {
        TextView textView = (TextView)findViewById(R.id.authority);
        textView.setText(Constants.AUTHORITY_URL);
        textView = (TextView)findViewById(R.id.resource);
        textView.setText(Constants.SCOPES[0]);
        textView = (TextView)findViewById(R.id.clientId);
        textView.setText(Constants.CLIENT_ID);
        textView = (TextView)findViewById(R.id.extraQueryParameters);
        textView.setText(Constants.EXTRA_QP);
        textView = (TextView)findViewById(R.id.redirectUri);
        textView.setText(Constants.REDIRECT_URL);
        textView = (TextView)findViewById(R.id.serviceUrl);
        textView.setText(Constants.SERVICE_URL);
        textView = (TextView)findViewById(R.id.fb_signin);
        textView.setText(Constants.FB_POLICY);
        textView = (TextView)findViewById(R.id.email_signin);
        textView.setText(Constants.EMAIL_SIGNIN_POLICY);
        textView = (TextView)findViewById(R.id.email_signup);
        textView.setText(Constants.EMAIL_SIGNUP_POLICY);
        textView = (TextView)findViewById(R.id.correlationId);
        textView.setText(Constants.CORRELATION_ID);

        fullScreenSwitch = (Switch)findViewById(R.id.fullScreen);
        fullScreenSwitch.setChecked(Constants.FULL_SCREEN);
    }

    private void saveSettings(String key, boolean value) {
        SharedPreferences prefs = SettingsActivity.this.getSharedPreferences(
                Constants.SHARED_PREFERENCE_NAME, Activity.MODE_PRIVATE);
        Editor prefsEditor = prefs.edit();
        prefsEditor.putBoolean(key, value);
        prefsEditor.commit();
    }
}
```

## <a name="create-the-add-task-activity"></a>A feladat hozzáadása tevékenység létrehozása

Ez a tevékenység segítségével tevékenység hozzáadása a REST API-végpontot.

Hozzon létre egy nevű fájlt `AddTaskActivity.java` , és írja be a következő.

```
package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;

public class AddTaskActivity extends Activity {

    EditText textField;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_task);
        textField = (EditText) findViewById(R.id.taskToAdd);
        Button button = (Button) findViewById(R.id.postTaskbutton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (textField.getText().toString() != null
                        && !textField.getText().toString().trim().isEmpty()
                        && Constants.CURRENT_RESULT != null) {

                    TodoListHttpService service = new TodoListHttpService();
                    try {
                        service.addItem(textField.getText().toString(), Constants.CURRENT_RESULT.getAccessToken());
                    } catch (Exception e) {
                        SimpleAlertDialog.showAlertDialog(getApplicationContext(), "Exception caught", e.getMessage());
                    }
                    finish();
                }
            }
        });
    }

}

```

## <a name="create-the-to-do-list-activity"></a>A teendősáv lista tevékenység létrehozása

A legfontosabb tevékenység: az. Úgy juthat az Azure Active Directory házirendhez jogkivonat, alkalmazhat, és adott jogkivonat segítségével hívja fel a tevékenység REST API-kiszolgáló.

Hozzon létre egy nevű fájlt `ToDoActivity.java` , és írja be a következő. (A hívások részletesen később.)

```

package com.microsoft.aad.taskapplication;

import android.app.Activity;
import android.app.AlertDialog;
import android.app.ProgressDialog;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.view.View;
import android.view.Window;
import android.webkit.CookieManager;
import android.webkit.CookieSyncManager;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import com.microsoft.aad.adal.AuthenticationCallback;
import com.microsoft.aad.adal.AuthenticationContext;
import com.microsoft.aad.adal.AuthenticationResult;
import com.microsoft.aad.adal.AuthenticationSettings;
import com.microsoft.aad.adal.UserIdentifier;
import com.microsoft.aad.adal.PromptBehavior;
import com.microsoft.aad.taskapplication.helpers.Constants;
import com.microsoft.aad.taskapplication.helpers.InMemoryCacheStore;
import com.microsoft.aad.taskapplication.helpers.TodoListHttpService;
import com.microsoft.aad.taskapplication.helpers.Utils;
import com.microsoft.aad.taskapplication.helpers.WorkItemAdapter;


import java.io.UnsupportedEncodingException;
import java.net.MalformedURLException;
import java.net.URL;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class ToDoActivity extends Activity {



    private final static String TAG = "ToDoActivity";

    private AuthenticationContext mAuthContext;
    private static AuthenticationResult sResult;

    /**
     * Adapter to sync the items list with the view
     */
    private WorkItemAdapter mAdapter = null;

    /**
     * Show this dialog when activity first launches to check if user has login
     * or not.
     */
    private ProgressDialog mLoginProgressDialog;


    /**
     * Initializes the activity
     */
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_todo_items);
        CookieSyncManager.createInstance(getApplicationContext());
        Toast.makeText(getApplicationContext(), TAG + "LifeCycle: OnCreate", Toast.LENGTH_SHORT)
                .show();

        // Clear previous sessions
        clearSessionCookie();
        try {
            // Provide key info for Encryption
            if (Build.VERSION.SDK_INT < 18) {
                Utils.setupKeyForSample();
            }

        Button button = (Button) findViewById(R.id.addTaskButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, AddTaskActivity.class);
                startActivity(intent);
            }
        });

        button = (Button) findViewById(R.id.appSettingsButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, SettingsActivity.class);
                startActivity(intent);

            }
        });

        button = (Button) findViewById(R.id.switchUserButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(ToDoActivity.this, LoginActivity.class);
                startActivity(intent);

            }
        });


        final TextView name = (TextView)findViewById(R.id.userLoggedIn);


        mLoginProgressDialog = new ProgressDialog(this);
        mLoginProgressDialog.requestWindowFeature(Window.FEATURE_NO_TITLE);
        mLoginProgressDialog.setMessage("Login in progress...");
        mLoginProgressDialog.show();
        // Ask for token and provide callback
        try {
            mAuthContext = new AuthenticationContext(ToDoActivity.this, Constants.AUTHORITY_URL,
                    false);
            String policy = getIntent().getStringExtra("thePolicy");

            if(Constants.CORRELATION_ID != null &&
                    Constants.CORRELATION_ID.trim().length() !=0){
                mAuthContext.setRequestCorrelationId(UUID.fromString(Constants.CORRELATION_ID));
            }

            AuthenticationSettings.INSTANCE.setSkipBroker(true);

            mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES, policy, Constants.CLIENT_ID,
                    Constants.REDIRECT_URL, getUserInfo(), PromptBehavior.Always,
                    "nux=1&" + Constants.EXTRA_QP,
                    new AuthenticationCallback<AuthenticationResult>() {

                        @Override
                        public void onError(Exception exc) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }
                            SimpleAlertDialog.showAlertDialog(ToDoActivity.this,
                                    "Failed to get token", exc.getMessage());
                        }

                        @Override
                        public void onSuccess(AuthenticationResult result) {
                            if (mLoginProgressDialog.isShowing()) {
                                mLoginProgressDialog.dismiss();
                            }

                            if (result != null && !result.getToken().isEmpty()) {
                                setLocalToken(result);
                                updateLoggedInUser();
                                getTasks();
                                ToDoActivity.sResult = result;
                                Toast.makeText(getApplicationContext(), "Token is returned", Toast.LENGTH_SHORT)
                                        .show();

                                if (sResult.getUserInfo() != null) {
                                    name.setText(result.getUserInfo().getDisplayableId());
                                    Toast.makeText(getApplicationContext(),
                                            "User:" + sResult.getUserInfo().getDisplayableId(), Toast.LENGTH_SHORT)
                                            .show();
                                }
                            } else {
                                //TODO: popup error alert
                            }
                        }
                    });
        } catch (Exception e) {
            SimpleAlertDialog.showAlertDialog(ToDoActivity.this, "Exception caught", e.getMessage());
        }
        Toast.makeText(ToDoActivity.this, TAG + "done", Toast.LENGTH_SHORT).show();
    } catch (InvalidKeySpecException e) {
            e.printStackTrace();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }}

```


 Akkor lehet, hogy láthatta, hogy ez támaszkodik egyelőre még nem írtak módszereket. Beállítások az alábbiak `updateLoggedInUser()`, `clearSessionCookie()`, és `getTasks()`. Ez az útmutató a később kell írni. Nyugodtan figyelmen kívül hagyhatja a hibák Android Studio most.

A paraméterek magyarázata:

  - `SCOPES`: Kötelező, a vonatkozó hozzáférési kérelmek próbál hatókörök. A B2C előzetes verzió: az ugyanaz, mint `client_id`, de ez a jövőben változhat várhatóan.
  - `POLICY`: A házirendet, amikor a felhasználók hitelesítését szeretné.
  - `CLIENT_ID`: Kötelező, az Azure Active Directory-portálon származik.
  - `redirectUri`: Beállíthatja a csomag nevével. Még nem szükséges kell adni a `acquireToken` hívja.
  - `getUserInfo()`: A úgy keresheti meg, hogy a felhasználó már szerepel a gyorsítótár. A paraméter is ismerteti, hogyan lehet a felhasználótól, ha nem található meg az adott felhasználó vagy a felhasználói jogkivonat argumentum valamelyike érvénytelen. Ez a módszer bekerül a később útmutatóban.
  - `PromptBehavior.always`: Kérje meg a hitelesítő adatait, és ugorja át a gyorsítótár és a cookie-k segítségével.
  - `Callback`: Neve után egy engedélyezési kód cseréje a jogkivonat. Objektum lesz `AuthenticationResult`, amely tartalmazza a hozzáférési jogkivonat, a lejárat dátuma és a azonosító jogkivonat adatai.

> [AZURE.NOTE]  A Microsoft Intune vállalati portál alkalmazás biztosítja a ügynök összetevő, és a felhasználó eszköze telepíthető. Az alkalmazás (egyszeri bejelentkezés SSO) hozzáférést biztosít a mindegyik számára az eszközön. A fejlesztők előkészített Intune lehetővé kell lennie. Az Android-alapú ADAL fogja használni a ügynök fiók, egy, a hitelesítő a létrehozott felhasználói fiókok esetén. Közvetítő használatához a fejlesztői kell egy speciális regisztrálni `redirectUri` közvetítő használatára vonatkozó. `redirectUri`msauth://packagename/Base64UrlencodedSignature formátumban van. Elérheti `redirectUri` az alkalmazás a parancsfájl használatával `brokerRedirectPrint.ps1` és az API-hívás használatával `mContext.getBrokerRedirectUri()`. Az aláírás kapcsolódik az aláíró tanúsítvány a Google Play áruházból.

 Kihagyhatja a ügynök felhasználó használatával:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```
> [AZURE.NOTE] Annak érdekében, hogy ez B2C quickstart útmutató egyszerűsítheti, hogy döntött, hogy ugorja át a mintában közvetítő.

Ezután hozzon létre a token egyedül beolvasása közben a tevékenységhez API hitelesítési hívások segítő módszereket.

Ugyanazon `ToDoActivity.java` fájlt, írja be a következő.

```
    private void getToken(final AuthenticationCallback callback) {

        String policy = getIntent().getStringExtra("thePolicy");

        // one of the acquireToken overloads
        mAuthContext.acquireToken(ToDoActivity.this, Constants.SCOPES, Constants.ADDITIONAL_SCOPES,
                policy, Constants.CLIENT_ID, Constants.REDIRECT_URL, getUserInfo(),
                PromptBehavior.Always, "nux=1&" + Constants.EXTRA_QP, callback);
    }
```

Hozzáadhat, amely "beállítása" és "beolvasása" módszerek `AuthenticationResult` (amely rendelkezik a token) a globális `Constants`. Habár `ToDoActivity.java` használja `sResult` a saját flow, fel kell vennie ezeket a módszereket. Ha nem, a másik tevékenységeket nem hozzáférni a munkát a token (például a tevékenység hozzáadásával `AddTaskActivity.java`).

```

    private AuthenticationResult getLocalToken() {
        return Constants.CURRENT_RESULT;
    }

    private void setLocalToken(AuthenticationResult newToken) {


        Constants.CURRENT_RESULT = newToken;
    }


```
## <a name="create-a-method-to-return-a-user-identifier"></a>A módszer való visszatéréshez a felhasználói azonosító létrehozása

Az Android-alapú jelöli a felhasználó formájában ADAL egy `UserIdentifier` objektumot. A felhasználó kezeli. Az objektum segítségével adja meg, hogy az adott felhasználó használja az Ön hívásait. Ezek az információk segítségével meg is a gyorsítótár támaszkodhat helyett új hívás kezdeményezése a kiszolgálóra. A könnyebb létrehozott a `getUserInfo()` módszer, amely visszaadja a `UserIdentifier`. Ezzel a `acquireToken()`. Is létrehozott egy `getUniqueId()` módszer, amely visszaadja a azonosítója `UserIdentifier` gyorsítótár.

```
  private String getUniqueId() {
        if (sResult != null && sResult.getUserInfo() != null
                && sResult.getUserInfo().getUniqueId() != null) {
            return sResult.getUserInfo().getUniqueId();
        }

        return null;
    }

    private UserIdentifier getUserInfo() {

        final TextView names = (TextView)findViewById(R.id.userLoggedIn);
        String name = names.getText().toString();
        return new UserIdentifier(name, UserIdentifier.UserIdentifierType.OptionalDisplayableId);
    }

```

### <a name="write-helper-methods"></a>Írás a segítő módszerek

Ezután írja be néhány segítő módszerek segít a cookie-k törlése, és adja meg `AuthenticationCallback`. Ezeket a módszereket segítségével teljesen a minta kattintva győződjön meg arról, hogy Ön egy könnyen áttekinthető állapotban hívásakor megadhat a `ToDo` tevékenységet.

Ugyanannak a fájlnak nevű `ToDoActivity.java`, írja be a következő.

```

    private void clearSessionCookie() {

        CookieManager cookieManager = CookieManager.getInstance();
        cookieManager.removeSessionCookie();
        CookieSyncManager.getInstance().sync();
    }
```

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        mAuthContext.onActivityResult(requestCode, resultCode, data);
    }

```   

## <a name="call-the-task-api"></a>Hívja fel a tevékenység API

Után készen áll a tokenek ragadja meg a tevékenységét, írhat a API-t a tevékenység kiszolgáló elérésére.

`getTasks`a kiszolgáló a feladatokat megjelenítő tömb biztosít.

Kiindulás `getTasks`.

Ugyanannak a fájlnak nevű `ToDoActivity.java`, írja be a következő.

```
    private void getTasks() {
        if (sResult == null || sResult.getToken().isEmpty())
            return;

        List<String> items = new ArrayList<>();
        try {
            items = new TodoListHttpService().getAllItems(sResult.getToken());
        } catch (Exception e) {
            items = new ArrayList<>();
        }

        ListView listview = (ListView) findViewById(R.id.listViewToDo);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1, android.R.id.text1, items);
        listview.setAdapter(adapter);
    }

```

Írja be egy módszer, amely elindítja a táblázat első futtatásakor is.

Ugyanannak a fájlnak nevű `ToDoActivity.java`, írja be a következő.

```
    private void initAppTables() {
        try {
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

        } catch (Exception e) {
            createAndShowDialog(new Exception(
                    "There was an error creating the Mobile Service. Verify the URL"), "Error");
        }
    }

```

Láthatja, hogy a kód néhány további módszerek a munka elvégzéséhez szükséges. Írja be azokat a Tovább gombra.

### <a name="create-an-endpoint-url-generator"></a>Hozzon létre egy végpont URL-cím nyilvántartás-készítő alkalmazás

Szeretne létrehozni, amely meg fog csatlakozni végpont URL-CÍMÉT. Hajtsa végre, amely ugyanannak a fájlnak osztály.

**Ugyanannak a fájlnak a** nevű `ToDoActivity.java`, írja be a következő.

 ```
    private URL getEndpointUrl() {
        URL endpoint = null;
        try {
            endpoint = new URL(Constants.SERVICE_URL);
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        return endpoint;
    }

 ```


Megjegyzés: a hozzáférési jogkivonat hozzáadása a kérelmet, a kód, a következő szakaszban ismertetett.

## <a name="write-some-ux-methods"></a>Írja be néhány UX módszerek

Android-alapú szükséges, hogy az alkalmazás működéséhez néhány visszahívást kezelni. Ezek a `createAndShowDialog` és `onResume()`. Az ismert, ha Ön írt Android kód előtt.

Ugyanannak a fájlnak nevű `ToDoActivity.java`, írja be a következő.

```
    @Override
    public void onResume() {
        super.onResume(); // Always call the superclass method first

        updateLoggedInUser();
        // User can click logout, it will come back here
        // It should refresh list again
        getTasks();
    }


```

Ezután kezelése a párbeszédpanel visszahívást.

Ugyanannak a fájlnak nevű `ToDoActivity.java`, írja be a következő.

```
    /**
     * Creates a dialog and shows it
     *
     * @param exception The exception to show in the dialog
     * @param title     The dialog title
     */
    private void createAndShowDialog(Exception exception, String title) {
        createAndShowDialog(exception.toString(), title);
    }

    /**
     * Creates a dialog and shows it
     *
     * @param message The dialog message
     * @param title   The dialog title
     */
    private void createAndShowDialog(String message, String title) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);

        builder.setMessage(message);
        builder.setTitle(title);
        builder.create().show();
    }

```

Most már rendelkeznie kell egy `ToDoActivity.java` , amely a fájlt. A teljes projekt is szerkesszenek ezen a ponton.

## <a name="run-the-sample-app"></a>A minta alkalmazás futtatása

Végezetül össze, és futtassa az alkalmazást az Android Studio vagy Holdas. Jelentkezzen, vagy jelentkezzen be az alkalmazás. A bejelentkezett felhasználó létrehozása. Jelentkezzen ki, és jelentkezzen be újra a különböző felhasználóként, majd a felhasználó számára-feladatok létrehozása.

Figyelje meg, hogy a tevékenységek is tárolt felhasználónkénti az API, mivel az API-t a felhasználói azonosító olvas a hozzáférési jogkivonat kap.

Kész példa [.zip fájlként megadva](https://github.com/AzureADQuickStarts/B2C-NativeClient-Android/archive/complete.zip)hivatkozás. Is átmásolhatja azt a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-Android```


## <a name="important-information"></a>Fontos tudnivalók


### <a name="encryption"></a>Titkosítás:

ADAL titkosítja a tokenek és a tár `SharedPreferences` alapértelmezés szerint. Megnézheti a `StorageHelper` osztály a részletek megtekintéséhez. Android-alapú **AndroidKeyStore 4.3(API18) a** titkos kulcsokat biztonságos tárolása jelent meg. ADAL használ, amely API18 és felett. Alsó SDK verziók ADAL használni szeretné, ha meg kell adnia a titkos kulccsal `AuthenticationSettings.INSTANCE.setSecretKey`.

### <a name="session-cookies-in-web-view"></a>Webes nézetben munkamenet cookie-k használata

Android webes nézet nem törli a munkamenet cookie-k, az alkalmazás bezárása után. Ez a példa a képes kezelni.
```
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
[További tudnivalók a cookie-k](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).
