<properties
    pageTitle="Hogyan engedélyezhető az idegen-alkalmazás SSO iOS ADAL használata a |} Microsoft Azure"
    description="Hogyan lehet ahhoz, hogy egyszeri bejelentkezés az alkalmazások között a ADAL SDK funkcióját használni. "
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Idegen-alkalmazás SSO iOS ADAL használata a engedélyezése


Nyújtó, hogy a felhasználók csak kell megadnia a hitelesítő adatait egyszer, és a hitelesítő adatokat, hogy automatikusan belül tevékenységekhez egyszeri bejelentkezés (SSO) alkalmazások most várhatóan vevők szerint. A felhasználónév és jelszó megadására kis képernyőn, gyakran időpontok kombinálni hívni vagy texted kódot, például egy további tényező (2FA) nehézsége rövid kapcsolatos, ha egy felhasználó az eredmények ehhez a termék egynél több időt tartalmaz.

Ezenkívül ha egy, egyéb alkalmazások előfordulhat, hogy használják, például Microsoft Accounts vagy egy munkahelyi fiókkal az Office 365-ben identitás platform, elvárt, hogy azok hitelesítő adatok használata bárhol az szállítói mindegyik számára elérhetővé szeretné tenni.

A Microsoft Identity platform, a Microsoft identitás SDK együtt nem az összes e elkerülése meg, és felajánlja az azt jelenti, hogy a felhasználók egyszeri bejelentkezéssel delight vagy a saját alkalmazások csomagja belül vagy, mint a ügynök lehetőséget, és a hitelesítő alkalmazások között a teljes eszközre.

Ez a forgatókönyv megtudhatja, beállításáról a SDK ahhoz, hogy ez a szolgáltatás ügyfelei az alkalmazáson belül.

Ez a forgatókönyv vonatkozik:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory feltételes hozzáférés


Figyelje meg, hogy a dokumentumot, az alábbi feltételezi, hogyan kell [rendelkezni az alkalmazásokat a régi portál az Azure Active Directory](./develop/active-directory-how-to-integrate.md) ismeretek állnak rendelkezésre, valamint integrálva van az alkalmazás a [Microsoft identitás iOS SDK csomagjában talál](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Egyszeri bejelentkezés fogalmak a Microsoft identitás-platform

### <a name="microsoft-identity-brokers"></a>Microsoft személyes kereskedők

A Microsoft biztosít, amelyek lehetővé teszik a hitelesítő adatok összekötő számára más gyártóktól származó alkalmazások minden mobil platform, valamint lehetővé teszi, hogy honnan érvényesíteni a hitelesítő adatok biztonságos feljegyezhet igénylő különleges továbbfejlesztett funkciók. Ezek a **kereskedők**hívása. Az iOS és Android ezek szolgáltatáson keresztül letölthető alkalmazásokat, hogy ügyfelei vagy független telepítése vagy egy másik vállalatnál némelyikét vagy mindegyikét az eszköz a alkalmazott kezelő az eszközre is tolni. Ezek a kereskedők támogatása irányító biztonsági csak néhány alkalmazást vagy az informatikai rendszergazdák kívánja alapján teljes eszköz. Ez a funkció a Windows-fiók Nézetválasztó az operációs rendszer ismert értelemben webes hitelesítési közvetítő beépített által biztosított.

Megtudhatja, hogy ezek kereskedők használata és hogyan ügyfelei látható őket a saját bejelentkezési folyamat a Microsoft Identity platform, a cikkben olvashat bővebben.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>A naplózás a telefonok és táblagépek beállítása mintázatok

Hitelesítő adatok eszközön való hozzáférés két alapvető mintázatok, a Microsoft Identity platform hajtsa végre:

* Nem-ügynök fizetős bejelentkezések
* Támogatott bejelentkezések ügynök

#### <a name="non-broker-assisted-logins"></a>Nem-ügynök fizetős bejelentkezések

Nem-ügynök fizetős bejelentkezések, amely fordulhat elő, beágyazott az alkalmazással, és a helyi tároló használata az eszközön az alkalmazást a bejelentkezési szolgáltatások. A tároló osztozhat alkalmazások, de a hitelesítő adatok szorosan kötődnek az alkalmazást vagy az alkalmazások használata a hitelesítő adatok csomagja. Ez az valószínűleg tapasztal a sok mobilalkalmazások tapasztalatok hol meg egy felhasználónevet és jelszót magát az alkalmazásból.

Ezek a bejelentkezések van a következő előnyökkel jár:

-  Felhasználói élmény létezik teljesen az alkalmazáson belül.
-  Hitelesítő adatok megosztható ugyanazt a tanúsítványt egy egyszeri bejelentkezéses megoldást az alkalmazások programcsomag való kezeléséről által aláírt számára.
-  A naplózás a élményét körül vezérlő kapja meg alkalmazása előtt és után bejelentkezési.

Ezek a bejelentkezést a következő hátrányai van:

- Felhasználói nem élmény egyszeri bejelentkezés a használt összes-alkalmazások használata a Microsoft Identity csak azokat, amelyek az alkalmazás tulajdonában van, és beállították a Microsoft Identities.
- Az alkalmazás nem használható, például a feltételes Access speciális funkciókat, vagy használja a termékek az InTune csomagot.
- Az alkalmazás nem támogatja a tanúsítvány-alapú hitelesítés üzleti felhasználók számára.

Az alábbiakban a megadott a Microsoft identitás SDK működése a megosztott tároló az alkalmazás SSO engedélyezése:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Támogatott bejelentkezések ügynök

Ügynök által támogatott bejelentkezések bejelentkezési szolgáltatások, amelyek az ügynök alkalmazásban fordul elő, és a tárhely és a biztonsági közvetítő használatával megoszthatja a hitelesítő adatait, amelyek a Microsoft Identity platform kihasználhatja az eszközön mindegyik számára is. Ez azt jelenti, hogy az alkalmazások támaszkodik közvetítő annak érdekében, hogy a felhasználók jelentkezzen be. Az iOS és Android ezek szolgáltatáson keresztül letölthető alkalmazásokat, hogy ügyfelei vagy független telepítése vagy egy másik vállalatnál a felhasználónak az eszközt kezelő az eszközre is tolni. Az alkalmazás a következő típusú példája iOS az Azure hitelesítő alkalmazást. Ez a funkció a Windows-fiók kiválasztására az operációs rendszer ismert értelemben webes hitelesítési közvetítő beépített által biztosított.
A felület platform változó és esetenként is zavaró felhasználók Ha nem felügyelt megfelelően. Jártas valószínűleg leggyakrabban a mintázatot, ha a Facebook-alkalmazás telepítve van, és egy másik alkalmazásban Facebook bejelentkezési funkcióival. A Microsoft Identity platformot használja ugyanazt a minta.

Az iOS "áttűnés" mindezeket animációs, ha az alkalmazás a háttérben, miközben az Azure hitelesítő alkalmazásokat a rendszer elküldi a felhasználó számára, hogy melyik fiókkal való bejelentkezéshez szeretnének jelölje ki az előtérbe megtalálható.  

Android és Windows a fiók kiválasztására az alkalmazás, amely a felhasználó kevésbé zavaró felett jelenik meg.

#### <a name="how-the-broker-gets-invoked"></a>Hogyan közvetítő meghívott módja

Ha az eszközön, például az Azure hitelesítő alkalmazás telepítve van a kompatibilis ügynök a Microsoft identitás SDK automatikusan meghívása közvetítő meg, amikor a felhasználó azt jelzi, hogy jelentkezzen be minden olyan-fiókot a Microsoft Identity platform a kívánt munkájának hajt végre. Ennek oka lehet egy személyes Microsoft-Account, munkahelyi vagy iskolai fiókját, vagy egy fiókot, Ön által és host B2C és B2B termékei használatával Azure-ban. Rendkívül biztonságos algoritmusok és titkosítás akkor győződjön meg arról, hogy a hitelesítő adatokat kér, biztonságos módon vissza az alkalmazás kézbesítve. Ezek a mechanizmusok a pontos technikai részleteket a közzé nem tett, de Apple és a Google az együttműködési kifejlesztett.

**A Fejlesztőeszközök lehetősége van, ha a Microsoft identitás SDK közvetítő meghívja, vagy a nem ügynök fizetős folyamat használja.** Jó helyen jár, ha a Fejlesztőeszközök úgy dönt, hogy ne használja a ügynök által támogatott folyamat elveszti az egyszeri bejelentkezés hitelesítő adatokat, hogy a felhasználó előfordulhat, hogy már hozzáadta az eszközre, valamint megakadályozza, hogy a Microsoft ügyfeleinek, például a feltételes Access Intune szolgáltatásairól, itt funkciókat együtt alkalmazzuk a alkalmazást, és a tanúsítvány-alapú hitelesítés vizsgál előnyeit.

Ezek a bejelentkezések van a következő előnyökkel jár:

-  Felhasználói élmény a SSO függetlenül attól, hogy a szállító mindegyik számára.
-  Az alkalmazás speciális funkciókat például feltételes Access kihasználhatja vagy a termékek az InTune csomagot használja.
-  Az alkalmazás támogatniuk kell a tanúsítvány-alapú hitelesítés üzleti felhasználók számára.
- Sokkal több biztonságos bejelentkezés módja az alkalmazás és a felhasználó identitással ellenőrzése további algoritmusok és titkosítás ügynök alkalmazásával.

Ezek a bejelentkezést a következő hátrányai van:

- Az iOS a felhasználónak van-re áttelepített ki az alkalmazás élmény közben a hitelesítő adatok van kiválasztva.
- Elvesztése, az azt jelenti, hogy a bejelentkezési felület az alkalmazáson belül a vevők kezelése.



Az alábbiakban a megadott a Microsoft identitás SDK működése ahhoz, hogy egyszeri bejelentkezési ügynök alkalmazásokkal:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Szerelte és a háttér információ látnia kell, jobb és egyszeri bejelentkezés végrehajtja a a Microsoft Identity platform és SDK alkalmazáson belül.


## <a name="enabling-cross-app-sso-using-adal"></a>Idegen-alkalmazás SSO ADAL használatával engedélyezése

Itt használjuk az ADAL iOS SDK szeretne:

- Kapcsolja be az alkalmazás a csomagja nem ügynök fizetős egyszeri bejelentkezés
- Támogatás bekapcsolása ügynök által támogatott egyszeri bejelentkezés esetén

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Egyszeri bejelentkezés egyszeri bejelentkezés bekapcsolás nem ügynök segítik.

A Microsoft identitás SDK számára nem ügynök fizetős egyszeri bejelentkezés kezelése meg az egyszeri bejelentkezés komplexitását nagy. Ide tartoznak a találja a megfelelő felhasználó gyorsítótár és karbantartását jelenti, hogy a lekérdezés bejelentkezett felhasználók listájának.

Ahhoz, hogy egyszeri bejelentkezési tulajdonjogának, tegye a következőket kell számára:

1. Az azonos ügyfél-azonosító vagy alkalmazás azonosító, győződjön meg arról, hogy az alkalmazások az összes felhasználó
* Az Apple-től azonos tanúsítvány az alkalmazás az összes megosztása, hogy keychains megoszthatja biztosítása
* Az azonos kulcsláncában jogosultság minden, az alkalmazás kérése.
* A Microsoft identitás SDK tájékoztatása a megosztott kulcsláncból számunkra, hogy a használni kívánt.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Az azonos ügyfél-azonosító használatával / alkalmazást az alkalmazások az csomagja az összes alkalmazás azonosító

Ahhoz, hogy a Microsoft Identity platform tudni, hogy van engedélyezett tokenek megosztása az alkalmazások között az alkalmazás minden kell megosztani azonos ügyfél-azonosító vagy azonosítót. Ez a kapott, amikor az első alkalmazás regisztrálta a portálon egyedi azonosítója.

Akkor is kell tudni szeretné, hogy hogyan határozható meg a Microsoft Identity szolgáltatás különböző alkalmazások alkalmazás azonosító használ Válasz az **Átirányítás URL-címe**lesz. Minden egyes alkalmazást beállíthatja, hogy több átirányítás URL-címe regisztrált a bevezetési portálon. Minden alkalmazás, a másik alkalmazásból egy másik átirányítási URI lesz. Példa látható, hogyan ez nem éri el:

App1 átirányítási URI:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 átirányítási URI:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 átirányítási URI:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Ezek a egymásba ágyazott csoportban az azonos ügyfél-azonosító / azonosítója és a keresett az átirányítást, állítsa vissza szeretne velünk a SDK konfigurációban URI alapján.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Figyelje meg, hogy ezek átirányítása URL-címe formátumának ismertetése az alábbiakban olvasható. Előfordulhat, hogy használja bármely átirányítása URI kivéve, ha ki szeretne támogatja a ügynök, ebben az esetben azok kell valami hasonló a fenti*



#### <a name="create-keychain-sharing-between-applications"></a>A kulcslánc megosztása az alkalmazások között létrehozása

A kulcslánc megosztása a dokumentum túlra és Apple dokumentumhasználati [Funkciók felvétele](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html)a dokumentumban. Mi az fontos, hogy úgy dönt, hogy mi a kulcslánc hívható és ezzel a funkcióval hozzáadása az összes alkalmazás között.

Ha jogosultságok állítsa be megfelelően meg kell jelennie egy fájlt a projekt könyvtárában jogosult `entitlements.plist` , amely tartalmaz, amit az alábbiakhoz hasonlóan néz ki:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Miután a kulcslánc jogosultság engedélyezve van az alkalmazás minden van, és készen áll a egyszeri Bejelentkezést használja, tájékoztatása a Microsoft identitás SDK a kulcslánc az alábbi beállítás használatával a `ADAuthenticationSettings` az alábbi beállításokkal:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING]
Az alkalmazások között a kulcslánc megosztásakor minden alkalmazás felhasználó törlése, vagy rosszabb végig az alkalmazás az összes tokenek törlése. Ez az alkalmazás a háttérben munka a tokenek támaszkodó ha kifejezetten katasztrofális. A kulcslánc megosztása: azt jelenti, hogy meg kell ügyeljen arra, hogy nagyon az összes távolítsa el a Microsoft identitás SDK keresztül műveletek.

Az egész! A Microsoft identitás SDK most megosztja át az összes alkalmazás hitelesítő adatokat. A felhasználók listáját lesznek megosztva szolgáltatásalkalmazás-példányok között.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Egyszeri bejelentkezés egyszeri bejelentkezés bekapcsolás ügynök segítik.

A lehetőségét, hogy az alkalmazás bármely ügynök, amely az eszközre telepítve van az **alapértelmezés szerint ki van kapcsolva**. Az alkalmazás használatához a közvetítő néhány további beállítási lehetőségek és az alkalmazás hozzáadása a kód.

Az itt leírt lépések a következők:

1. Az alkalmazás kódja hívásban vesz részt az MS SDK ügynök mód engedélyezése.
2. Egy új átirányítási URI létrehozni, és arról, hogy az alkalmazás és az alkalmazás regisztrációs.
3. Egy URL-séma regisztrálása.
4. iOS9 támogatás: jogosultsági hozzáadása a info.plist fájlhoz.


#### <a name="step-1-enable-broker-mode-in-your-application"></a>Lépés: 1: Az alkalmazás ügynök mód engedélyezése
Az alkalmazás használatához közvetítő lehetősége van kapcsolva, a "helyi" vagy a kezdeti beállítás a hitelesítési objektum létrehozásakor. Ehhez a kódot a hitelesítő adatok típusának beállítása:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
A `AD_CREDENTIALS_AUTO` beállítás lehetővé teszi, hogy próbálja ki közvetítő, hogy a kimenő hívás kezdeményezése a Microsoft-identitás SDK `AD_CREDENTIALS_EMBEDDED` megakadályozza, hogy a Microsoft identitás SDK hívófél közvetítő szeretne.

#### <a name="step-2-registering-a-url-scheme"></a>Lépés: 2: Egy URL-séma rögzítése
A Microsoft Identity platform URL-címek közvetítő meghívása, és térjen vissza az alkalmazást a control használja. Befejezéséhez az adott üzenetváltási szüksége van egy URL-séma regisztrált, amely a Microsoft Identity platform fogja tudni az alkalmazás. Ez lehet kívül más alkalmazást rendszerek előfordulhat, hogy korábban regisztrálta a alkalmazással.

> [AZURE.WARNING]
Azt javasoljuk, hogy az URL-CÍMÉT az Office-téma egy másik alkalmazásban az azonos URL-séma használata a csomagváltás minimalizálásához meglehetősen egyedi kezdeményezhet. Apple nem hivatkozási URL-sémát az app Store áruházban regisztrált egyediségét.

Az alábbi képen az példa ezt megjelenése a project konfigurálása. Előfordulhat, hogy is ehhez a XCode is látható:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Lépés 3: Létrehozni egy új átirányítási URI az URL-cím séma

Győződjön meg arról, hogy azt bármikor visszatérhet a hitelesítő adatok tokenek a megfelelő alkalmazást, hogy szükség győződjön meg arról, hogy visszahívhatja oly módon, hogy az iOS operációs rendszer ellenőrizheti az alkalmazás. Az iOS operációs rendszer a jelentést a Microsoft ügynök alkalmazások az alkalmazás, hívja fel, hogy az első lépésekhez azonosítója. Ez egy engedélyezetlen alkalmazás nem megtévesztésre. Emiatt azt kihasználhatja a saját ügynök alkalmazás biztosítja, hogy a megfelelő alkalmazása a tokenek kerülnek vissza az URI együtt. Azt csak az egyedi átirányítás mindkét URI létrehozni az alkalmazásban, és a developer Portal átirányítás URI beállítva.

Az átirányítási URI megfelelő formájában kell:

`<app-scheme>://<your.bundle.id>`

utólagos: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Ez az átirányítási URI kell a az [Azure klasszikus portál](https://manage.windowsazure.com/)alkalmazás regisztrációs kell megadni. Azure AD-alkalmazás regisztrációs kapcsolatos további tudnivalókért olvassa el a [integrálása az Azure Active Directory címtárral](./develop/active-directory-how-to-integrate.md)című témakört.


##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>3a lépés: Adja hozzá egy átirányítási URI az alkalmazás és a fejlesztők portálján tanúsítvány-alapú hitelesítés támogatására

A tanúsítvány támogatási második "msauth" regisztrálva kell lennie az alkalmazás és az [Azure klasszikus portálon](https://manage.windowsazure.com/) kezelheti a hitelesítés, ha szeretné hozzáadni, amely támogatja az alkalmazásban a hitelesítési alapján.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

utólagos: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>Lépés: 4: iOS9: az alkalmazás konfigurációs paraméter felvétele

ADAL használ – canOpenURL: ellenőrizni, hogy telepítve van-e közvetítő az eszközön. Az iOS 9 Apple zárolva mi alkalmazás rendszerek is lekérdezhetők. Meg kell "msauth" hozzáadása a LSApplicationQueriesSchemes szakaszában a `info.plist file`.

<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauth</string>
</array>

### <a name="youve-configured-sso"></a>Beállította az egyszeri bejelentkezés!

Most a Microsoft identitás SDK fog automatikusan is megoszthatja az alkalmazás hitelesítő adatokat, és meghívása közvetítő, ha meg-e az eszköze a.
