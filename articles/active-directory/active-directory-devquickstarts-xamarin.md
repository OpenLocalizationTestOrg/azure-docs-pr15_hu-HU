<properties
    pageTitle="Első lépések Azure Active Directory Xamarin |} Microsoft Azure"
    description="Hogyan hozhat létre egy Xamarin alkalmazás, amely együttműködik a bejelentkezés az Azure Active Directory és Azure Active Directory felhívja API-k használata OAuth védett."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integráció a Azure Active Directory Xamarin-alkalmazásba

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin lehetővé teszi, hogy mobilalkalmazások írja be a C#, amely a iOS, Android és Windows (a mobileszközök és a PC-re) futtathatók. Ha az alkalmazás használatával Xamarin szeretne összeállítani, Azure Active Directory teszi egyszerű és egyszerű, az Active Directory-fiókokat azok a felhasználók azonosítania kell magát. Azt is lehetővé teszi, hogy az alkalmazás bármely webes API védi az Azure Active Directory, például az Office 365 API-k és az Azure API biztonságos módon használhatnak.

Védett forrásokat kell Xamarin alkalmazásai Azure AD az Active Directory Authentication Library vagy ADAL biztosít. ADAL-féle kizárólagos életben célja, hogy az alkalmazás, hogy a hozzáférési jogkivonat könnyen. Annak bemutatják, hogy milyen egyszerűen egyszerű, itt azt fogja hozza létre a "Címtár elutasíthat" alkalmazást, amely:

-   Az alábbiakon futtatható iOS, az Android, az a Windows asztal, a Windows Phone és a Windows áruházból.
- Egy egyetlen hordozható osztálytár (PCL) használja a tokenek hitelesíti a felhasználókat és az Azure Active Directory Graph API-val
-   Egy adott UPN rendelkező felhasználóknak könyvtárában keres.

A teljes munkaidő-alkalmazás létrehozására kell:

2. A Xamarin fejlesztői környezet beállítása
2. Az alkalmazás regisztrálása Azure AD.
3. Telepítse & konfigurálása ADAL.
5. ADAL segítségével tokenek lekérése az Azure Active Directory.

Az első lépések, [skeleton projekt letölthetők](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) , és [Töltse le a bejegyzett minta](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Minden egyes a Visual Studio 2013 megoldást. Az Azure Active Directory bérlői, ahol felhasználók létrehozása és az alkalmazások regisztráljon is szüksége lesz. Ha még nem rendelkezik a bérlői, [megtudhatja, hogy miként beszerzéséhez](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. a Xamarin fejlesztői környezet beállítása*
Ebben az oktatóprogramban az iOS, Android és Windows projektet tartalmaz, mert szüksége lesz a Visual Studio és Xamarin együtt. A szükséges környezet létrehozásához a teljes megjelenő útmutatást követve [beállítása és a Visual Studio és Xamarin telepítése az](https://msdn.microsoft.com/library/mt613162.aspx) MSDN webhelyen. Ezeket az utasításokat, akkor ellenőrizheti, ha többet szeretne tudni a Xamarin, a telepítőfájlokat elvégzéséhez az várakozás közben anyagokra tartalmazzák. 

Egyszer végzett a szükséges beállítást, és nyissa meg azt a megoldást a Visual Studióban kezdéshez. Hat projektek találja: öt platform-specifikus projektek és egy hordozható osztálytár minden platformon lesz megosztva`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. a címtár-elutasíthat alkalmazás regisztrálása*
Ahhoz, hogy az alkalmazás tokenek megszerezni, először kell regisztrálni a Azure AD-bérlő, és adja meg azt az Azure Active Directory Graph API eléréséhez szükséges engedély:

-   Jelentkezzen be az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlői webhelyet, ahol az alkalmazás regisztrálni.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **Natív ügyfélalkalmazás**.
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Az **Átirányítás Uri** , amelyekkel a Azure AD vissza jogkivonat válaszok az Office-téma és karakterlánc együttes. Írja be a érték, például `http://DirectorySearcher`.
-   Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító. Ez az érték van szüksége a következő szakaszok, így másolja a vágólapra a **beállítás** lapon.
- Is **konfigurálása** lapon keresse meg a "Engedélyeket szeretne egyéb alkalmazások" szakaszt. A "Azure Active Directory" alkalmazás hozzáadása a **hozzáférést a szervezet címtárára** engedélyt a **Meghatalmazott engedélyeit**. Ezzel engedélyezi az alkalmazás a Graph API-felhasználóknak lekérdezéséhez.

## <a name="2-install--configure-adal"></a>*2. a telepítés & konfigurálása ADAL*
Most, hogy az alkalmazások az Azure Active Directory, ADAL telepítése, és írja be a kapcsolódó azonosító kódot. Az Azure Active Directory kommunikálni tudjanak ADAL sorrendben kell, küldje el az alkalmazás regisztrációs információkat.
-   Kezdje el a projekteket a megoldást a csomag Manager konzolon az egyes ADAL hozzáadásával.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Meg kell figyelje meg, hogy minden projekt - két tár hivatkozás hozzáadódnak ADAL PCL részét, és a platform-specifikus része.

-   Nyissa meg a DirectorySearcherLib projekt `DirectorySearcher.cs`. A osztály tagság értékek megfelelően, az Azure-portálra a bemeneti értékek módosítása. A kód minden alkalommal ADAL hivatkoznak ezeket az értékeket.
    -   A `tenant` a Azure AD-bérlő, például contoso.onmicrosoft.com tartománya
    -   A `clientId` van az alkalmazás a portálról másolta a clientId.
    - A `returnUri` van a beírt a portálon, például redirectUri `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Kattintson ADAL úgy juthat az AAD tokenek*
Minden, az alkalmazás hitelesítési logikájának esik, a *Almost* `DirectorySearcher.SearchByAlias(...)`. Minden szükséges platform-specifikus projektekben történő átadásakor környezetfüggő paramétere a `DirectorySearcher` PCL.

- Első lépésként nyissa meg a `DirectorySearcher.cs` és vegyen fel egy új paramétere a `SearchByAlias(...)` módot. `IPlatformParameters`környezetfüggő paraméter platform-specifikus objektumokat tartalmazó ADAL kell hitelesítéshez ágyazza van.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Ezután inicializálni a `AuthenticationContext` -ADAL adatait a elsődleges osztály. Most, hogy hol ADAL adják át a koordináták kommunikálni az Azure Active Directory szüksége van. Ezután felhívni `AcquireTokenAsync(...)`, amely fogadja el a `IPlatformParameters` objektum és a hitelesítési folyamat egy token visszatérhet az alkalmazás szükséges meghívják.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`először próbálja a kért erőforráshoz (a Graph API ebben az esetben) token visszatérési megadnia a hitelesítő adatait (keresztül gyorsítótárazás vagy frissítése a régi tokenekre) a felhasználó értesítése nélkül. Csak ha szükséges, akkor jelennek meg a felhasználó az Azure Active Directory bejelentkezési lapon a kért jogkivonat megszerzése előtt.


- A hozzáférési jogkivonat majd csatolása a Graph API kérelem az engedélyezés fejlécében:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Ez minden, az a `DirectorySearcher` PCL és az alkalmazás által identitás kapcsolatos kódot.  Hogy továbbra is-e hívni a `SearchByAlias(...)` módszer az egyes platform nézeteket, és ha szükséges hozzáadása a megfelelően kezeli a felhasználói felület életciklusáról kódját.

####<a name="android"></a>Android:
- A `MainActivity.cs`, adja hozzá a hívást kezdeményez `SearchByAlias(...)` kattintson a gombra a kezelő:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Is kell felülbírálása az `OnActivityResult` életciklus módszer minden hitelesítés továbbítani átirányítja visszatérhet a megfelelő módszert.  ADAL nyújt a segítő módszert Ez az Android-alapú:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows asztali:
- A `MainWindow.xaml.cs`, egyszerűen hívás `SearchByAlias(...)` átadása egy `WindowInteropHelper` az asztalon `PlatformParameters` objektum:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- A `DirSearchClient_iOSViewController.cs`, az iOS `PlatformParameters` objektum egyszerűen megnyitja a nézet vezérlő hivatkozás:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>A Windows univerzális:
- Nyissa meg az Windows univerzális `MainPage.xaml.cs` és megvalósítása a `Search` módszer, amely megosztott projekt segítő módszer segítségével szükség szerint a felhasználói felület frissítése.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Gratulálok! Most már Xamarin alkalmazás, amely tartalmazza a felhasználók hitelesítő és biztonságos módon hívja fel a webes API-khoz OAuth 2.0 használatával öt platformon működik. Ha még nem tette meg, ezt követően az idő az egyes felhasználók a bérlő kitöltéséhez. Futtassa a DirectorySearcher alkalmazást, és jelentkezzen be egy azoknak a felhasználóknak. Más felhasználók UPN név alapján kereshet.

ADAL megkönnyíti a közös identitás szolgáltatások részévé tenni az alkalmazást. Azt gondoskodik a dirty munkáját vonatkozó - gyorsítótár-kezelés, a OAuth protokoll támogatása, a felhasználó bemutató tartása a bejelentkezési felhasználói felülete frissítése lejárt tokenek, és így tovább. Valójában kell tudnia, mindössze egyetlen API-hívás `authContext.AcquireToken*(…)`.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Most már áthelyezheti további identitás esetek. Előfordulhat, hogy szeretne kipróbálni:

[.NET webes API az Azure Active Directory biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
