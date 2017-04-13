<properties
    pageTitle="Azure Active Directory 2.0-s verzió – Android alkalmazás |} Microsoft Azure"
    description="Hogyan lehet, hogy a felhasználók személyes Microsoft-fiók és munkahelyi vagy iskolai partnereket és a diagram API hívások bejelentkezik a harmadik féltől származó tárak használatával Android alkalmazás összeállítása."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Külső tár használata a 2.0-s verzió végpont használatával Graph API-val Android alkalmazás bejelentkezési hozzáadása

A Microsoft identitás platform használja, például oauth2 hitelesítési mód és a csatlakozás OpenID megnyitott szabványoknak. A fejlesztők használhatják a szolgáltatások integrálása szeretnének tártípusból. Hogy a fejlesztők az tárakat a platformot használja, néhány forgatókönyvek konfigurálása a Microsoft identitás platform csatlakozhat külső tárak igazolni az alábbihoz hasonló írt azt. A legtöbb tárakat, amely [a RFC6749 oauth2 hitelesítési mód specifikáció](https://tools.ietf.org/html/rfc6749) végrehajtja a Microsoft identitás platform csatlakozhat.

Ez a forgatókönyv hoz létre az alkalmazással felhasználók jelentkezzen be szervezete, és keresse meg a saját maga a szervezet a Graph API segítségével.

Ha most használja először a oauth2 hitelesítési mód vagy a OpenID csatlakozni, a minta konfigurálási előfordulhat, hogy nem hozzárendelésével. Javasoljuk, hogy [2.0-s protokollok - OAuth 2.0-s engedélyezési kód Flow](active-directory-v2-protocols-oauth-code.md) háttér olvassa el.

> [AZURE.NOTE] Egyes szolgáltatások a platform, hogy van-e egy kifejezést az oauth2 hitelesítési mód vagy OpenID csatlakozás előírásoknak, feltételes Access és az Intune házirendkezelő, például csak a Megnyitás a Microsoft Azure identitás-tárak használata.

A 2.0-s verzió végpont esetek Azure Active Directory és a szolgáltatások nem támogatja.

> [AZURE.NOTE] Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.


## <a name="download-the-code-from-github"></a>Töltse le a kód GitHub
Ebben az oktatóanyagban kódját [a GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Is egyszerűen letöltheti a minta, és azonnal első lépések:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Regisztráljon az alkalmazás
Új alkalmazás létrehozása, az [alkalmazás regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy a részletes lépéseket, [hogy miként regisztrálhatja egy alkalmazást a 2.0-s verzió végponttal](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolja az alkalmazás van rendelve, mert hamarosan kell **Azonosítója** .
- A **mobil** platform az alkalmazás hozzáadása

> Megjegyzés: Az alkalmazás regisztrációs portál értéket **Irányítani URI** . Jó helyen jár, ebben a példában kell használnia az alapértelmezett értéket `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Töltse le a NXOAuth2 külső tárat, és a munkaterület létrehozása

Ez a forgatókönyv a OIDCAndroidLib GitHub, amely az oauth2 hitelesítési mód tárba, a Google OpenID csatlakozás kódját a fogja használni. Alkalmazza a natív alkalmazás profil és a támogatja az engedélyezés végpontot, annak a felhasználónak. Az alábbiakban az összes, amely a Microsoft identitás platform integrálása kell.

A számítógépen a OIDCAndroidLib repó klónozhatja.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Az Android Studio környezet beállítása

1. Android Studio új projekt létrehozása, és fogadja el az alapértelmezett beállításokat a varázslóban.

    ![Az Android Studio új projekt létrehozása](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Cél Android-eszközön](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Tevékenység hozzáadása a mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. A projekt helye a klónozott repó áthelyezése a project-modulok beállításához. A projekt is létrehozhat, és majd klónozhatja, közvetlenül a projekt helye.

    ![A Project-modulok](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Nyissa meg a projekt modulok beállítások, a helyi menüjének használatával vagy a Ctrl + Alt + Maj + S helyi használatával.

    ![Projekt-modulok beállítások](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Mivel a projektet tároló beállítások csak szeretne, távolítsa el az alapértelmezett alkalmazás modul.

    ![Az alapértelmezett alkalmazás modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. A klónozott repó modulok importálása a jelenlegi projektből.

    ![Projekt importálása a gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![új lap létrehozása a modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Ismételje meg ezeket a lépéseket a `oidlib-sample` modulra.

7. Jelölje be a oidclib függőségeket a a `oidlib-sample` modulra.

    ![Kattintson a minta oidlib modul oidclib függőségek](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Kattintson az **OK gombra** , és várja meg gradle szinkronizálása.

    A settings.gradle hasonlóan kell kinéznie:

    ![Képernyőkép a settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Győződjön meg róla, hogy a minta alkalmazás összeállítása a minta megfelelően működik.

    Használhatja ezt az Azure Active Directoryval, még nem. Azt kell először állítsa be az egyes végpontok. Ez a annak érdekében, hogy nem rendelkezik az Android Studio problémák azt a testreszabása a minta alkalmazás indítása előtt.

10. Szerkesztés és futtatása `oidlib-sample` Android Studio a célként.

    ![Oidlib kétmintás build végrehajtásának](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Törölje a `app ` könyvtár maradt, amikor a modul a projektből eltávolított, mert biztonsági Android Studio nem törli.

    ![A alkalmazástárához tartalmazó fájlszerkezet](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Ha el szeretné távolítani a Futtatás beállításokat, amelyek a modul a projektből eltávolításakor is balra **Konfigurációk szerkesztése** menü megnyitása.

    ![Beállítások menü szerkesztése](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![futtatása app konfigurálása](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>A minta a végpontok konfigurálása

Most, hogy a `oidlib-sample` sikeresen fut, a munka megkezdése az Azure Active Directory címtárral néhány végpontok szerkesztése.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Az ügyfélprogram konfigurálását a oidc_clientconf.xml fájl szerkesztésével

1. Mivel oauth2 hitelesítési mód flow csak az első jogkivonat, és hívja fel a Graph API használ, állítsa az ügyfelet oauth2 hitelesítési mód csak. Egy újabb példában OIDC fog érkezni.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Állítsa be az ügyfél-azonosító a regisztrációs portálról kapott.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Állítsa be az átirányítási URI az egyik alábbi.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Állítsa be a tartományok, amely a diagram API elérése szükséges.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

A `User.Read` érték `oidc_scopes` lehetővé teszi, hogy az alap profilt az aláírt teszik a felhasználó.
További tudnivalók a [Microsoft Graph jogosultsági hatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes)elérhető hatóköreit talál.

Magyarázatok mezőkódokkal kapcsolatban `openid` vagy `offline_access` hatókörök a OpenID csatlakozni, mint [2.0-s protokollok - OAuth 2.0-s engedélyezési kód Flow](active-directory-v2-protocols-oauth-code.md)látható.

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Állítsa be az ügyfél végpontok a oidc_endpoints.xml fájl szerkesztése

- Nyissa meg a `oidc_endpoints.xml` fájl, és hajtsa végre a következő módosításokat:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Ezeket a végpontokat soha ne változtassa meg, ha a protokolljaként oauth2 hitelesítési mód használja.

> [AZURE.NOTE]
A végpontok `userInfoEndpoint` és `revocationEndpoint` Azure Active Directory jelenleg nem támogatott. Ha ezek az alapértelmezett example.com értékkel hagyja, fog emlékezteti, hogy azok nem érhetők el a mintában :-)


## <a name="configure-a-graph-api-call"></a>Diagram API hívás konfigurálása

- Nyissa meg a `HomeActivity.java` fájl, és hajtsa végre a következő módosításokat:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Egy egyszerű diagram API-hívás itt az adatokat ad eredményül.

Láthatóvá válnak a módosítások kell tennie. Futtassa a `oidlib-sample` alkalmazást, és kattintson a **Bejelentkezés**gombra.

Miután sikeresen hitelesítése megtörtént, jelölje ki a hívás a Graph API teszteléséhez a **Védett erőforrás kérése** gombra.

## <a name="get-security-updates-for-our-product"></a>A termék biztonsági frissítések beszerzése

Javasoljuk, ha értesítést szeretne kapni kapcsolatos biztonsági események megtalálhatók a [Biztonsági, TechCenter](https://technet.microsoft.com/security/dd252948) és előfizetés a biztonsági tanácsadó riasztások.
