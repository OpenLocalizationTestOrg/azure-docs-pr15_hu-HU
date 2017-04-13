<properties 
    pageTitle="Hogyan lehet menteni, majd állítsa be a API-kezelés konfigurációjának, mely számjegy használatával" 
    description="Megtudhatja, hogyan lehet menteni, majd állítsa be a API-kezelés konfigurációjának, mely számjegy használatával." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Hogyan lehet menteni, majd állítsa be a API-kezelés konfigurációjának, mely számjegy használatával

>[AZURE.IMPORTANT] Mely számjegy konfigurációs API-kezelés jelenleg előzetes verzióban. Funkcionális teljes, de aktívan vár, akkor ez a funkció véleményezhessék oka előzetes verzióban. Akkor lehet, hogy egy szakítószilárdságának visszajelzései, változása, azt javasoljuk, nem attól függően, hogy a szolgáltatás éles környezetben használja, előfordulhat, hogy VÁLLALUNK. Ha bármelyik visszajelzés vagy kérdéseket, tudassa velünk a `apimgmt@microsoft.com`.

Minden egyes API Management szolgáltatás példányának konfigurációs adatbázis konfigurációs és szolgáltatás példány metaadataira vonatkozó információkat tartalmazó kezeli. A elvégzett módosításokat a szolgáltatás-példányhoz egy beállítást a publisher-portálon, egy PowerShell-parancsmag használatával és a Telefonhívás kezdeményezése REST API-t. Ezekkel az eljárásokkal mellett is kezelheti a példány konfigurációjának mely számjegy használatával, például: a szolgáltatás felügyeleti forgatókönyvek engedélyezése:

-   Konfigurációs verziószámozás - töltse le és tárolni a konfigurációjának különböző verziói
-   Beállítások módosítása tömeges - módosítja a szolgáltatás konfigurálása a helyi adattárban több részeit, és a módosítások a kiszolgáló integrálása egyetlen művelet
-   A jól ismert mely számjegy toolchain és a munkafolyamat - használja a mely számjegy szerszámok és a már jól ismert munkafolyamatok

A következő ábrán a különböző módokon állítható be a API Management szolgáltatás példányának áttekintése.

![Mely számjegy konfigurálása][api-management-git-configure]

Ha módosítja a szolgáltatás a publisher portál PowerShell-parancsmagok és a REST API-t, tartománya a szolgáltatás konfigurációs adatbázis használja a `https://{name}.management.azure-api.net` végpontot, a diagram jobb oldalán látható módon. Bal szélén látható a diagramon azt szemlélteti, hogyan kezelheti a konfigurációjának, mely számjegy használatával, és mely számjegy tárházba a szolgáltatásbeli található `https://{name}.scm.azure-api.net`.

Az alábbi lépéseket a API Management szolgáltatás példányának mely számjegy kezelése áttekintést nyújtanak.

1.  A szolgáltatás mely számjegy hozzáférés engedélyezése
2.  A szolgáltatás konfigurációs adatbázis mentése a mely számjegy tárházba
3.  A mely számjegy repó a helyi számítógépre másolása
4.  A helyi számítógép zónában, és a jóváhagyás és a leküldéses le a legújabb repó húzza vissza a repó
5.  A változások a repó üzembe a konfigurációs-adatbázisba

Ez a cikk azt ismerteti, hogyan engedélyezheti, és mely számjegy-lel való kezelése a konfigurációjának, és hivatkozás biztosít a fájlokat és mappákat a mely számjegy tárban tárolnak.

## <a name="to-enable-git-access"></a>Mely számjegy hozzáférés engedélyezése

Gyorsan megtekintheti a mely számjegy konfigurációs állapotának megtekintésével, a publisher portál jobb felső sarkában a mely számjegy ikonra. Ebben a példában az access mely számjegy még nincs engedélyezve.

![Mely számjegy állapota][api-management-git-icon-enable]

Megtekintése és a mely számjegy beállításait, vagy az mely számjegy ikonra, vagy kattintson a **Biztonság** menü majd a nyissa meg azt a **konfigurációs tárházba** fülre.

![Mely Számjegy engedélyezése][api-management-enable-git]

Ahhoz, hogy mely számjegy hozzáférést, a **hozzáférés engedélyezése mely számjegy** jelölőnégyzetet.

A módosítás mentése után egy kis időt, és a megerősítést kérő üzenet jelenik meg. Figyelje meg, hogy mely számjegy ikon színt jelezheti, hogy mely számjegy hozzáférés engedélyezve van, és állapotüzenete most jelzi, hogy nem mentett vannak változások a tárházba megváltozott. Ennek oka az, a API Management szolgáltatás konfigurációs adatbázis van még nem mentett a tárházba.

![Mely számjegy engedélyezve][api-management-git-enabled]

>[AZURE.IMPORTANT] Bármely titkos kulcsok tulajdonságok szeretne tárolni a tárat, és az előzmények marad, amíg meg nem definiált letiltása és ismételt engedélyezése az access mely számjegy. Tulajdonságok biztonságos helyen állandó karakterlánc-értékek, többek között a titkos kulcsok, az összes API konfigurációs és házirendeket, így nem kell tárolja őket közvetlenül a házirendi kezeléséhez nyújt. További tudnivalókért lásd: [tulajdonságai az Azure API adatkezelési házirendek használatáról](api-management-howto-properties.md).

Engedélyezése vagy letiltása a REST API mely számjegy hozzáférést a további tudnivalókért lásd [engedélyezheti vagy letilthatja a REST API mely számjegy hozzáférést](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>A szolgáltatás beállításai a mely számjegy tár mentése

Az első lépés a tárházba klónozhatja előtt a szolgáltatás beállításai aktuális állapotát a tár mentése. **A tárházba konfigurációs a Mentés**gombra.

![Konfiguráció mentése][api-management-save-configuration]

Végezze el a kívánt módosításokat a a megerősítési képernyőn, és kattintson az **Ok** mentéséhez.

![Konfiguráció mentése][api-management-save-configuration-confirm]

A konfiguráció mentése után néhány percet, és a tárházba konfigurációs állapotának jelenik meg, beleértve a dátum- és konfigurációs utolsó módosítás és az utolsó szinkronizálás között a szolgáltatás beállításai és a tárat.

![Konfiguráció állapota][api-management-configuration-status]

Miután a konfigurációs mentésekor a tárházba is klónozható.

A REST API-e a művelet a további tudnivalókért lásd [a REST API konfigurációs pillanatfelvétel véglegesítése](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>A helyi számítógépre a tárházba klónozhatja

Tár klónozhatja, az URL-címet a tárházba, felhasználónév és jelszó szükséges. A felhasználó nevét és URL-CÍMÉT a **konfigurációs tárházba** lap tetején jelenik meg.

![Mely számjegy adatfeliratsor][api-management-configuration-git-clone]

A jelszó jön létre a **konfigurációs tárházba** lap alján.

![Jelszó létrehozása][api-management-generate-password]

Jelszót szeretne létrehozni, először győződjön meg arról, hogy a **lejárati** értéke kívánt lejárati dátumát és időpontját, és kattintson a **Token készítése**.

![Jelszó][api-management-password]

>[AZURE.IMPORTANT] Jegyezze fel a jelszót. Miután a lap elhagyása a jelszó nem jelennek meg újból.

Az alábbi példák a eszközzel mely számjegy Bash [mely számjegy a](http://www.git-scm.com/downloads) Windows-ban, de minden mely számjegy eszközzel, amely már jól ismert.

Nyissa meg a mely számjegy eszköz a kívánt mappát, és futtassa a következő parancsot a helyi számítógépre, a publisher portálja által megadott paranccsal a mely számjegy tárházba klónozhatja.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Adja meg a felhasználónevet és jelszót a megjelenő párbeszédpanelen.

Ha megjelenik a hibák, próbálja meg módosítása a `git clone` parancsot, amelyet fel szeretne venni a felhasználónevet és jelszót, az alábbi példában látható módon.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Ha ez a hiba biztosít, próbálja meg kódolása a parancs jelszó része URL-CÍMÉT. Ehhez egy gyorsan, hogy nyissa meg a Visual Studióban, hiba a következő parancs az **Immediate ablak**. Az **Immediate ablak**megnyitásához, nyissa meg bármelyik megoldás vagy a projekt Visual Studio (vagy hozzon létre egy új üres konzol alkalmazást), és válassza a **Windows**és az **közvetlen** **hibakeresési** menüből.

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

A felhasználó nevét, és a tárházba helyét együtt kódolt jelszót használja a mely számjegy parancs összeállításához.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Miután klónozva van, a tár megtekintése, és a helyi fájlrendszerben használata. További tudnivalókért lásd: a [fájl és mappa helyi mely számjegy tárházba struktúra hivatkozást](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>A helyi tárházba frissítheti a legfrissebb szolgáltatás példány beállításai

Ha a módosítást végez az API Management szolgáltatás példány a publisher-portálon vagy a REST API-t használ, mentenie kell a módosítások a tárházba a legfrissebb módosítások a helyi tárházba frissítése előtt. Ehhez **a tárházba konfiguráció mentése** lapon kattintson a **konfiguráció tárházba** a publisher-portálon, és ezután hiba a következő parancsot a helyi adattárban.

    git pull

Futtatása előtt `git pull` ellenőrizze, hogy a mappa a helyi tárhoz. Ha csak a `git clone` parancsot, majd a módosítania kell a címtárban a repó a következő parancs futtatásával.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>Szeretne küldeni a módosításokat a a helyi repó a kiszolgáló repó

Küldeni a módosításokat a helyi tárházba való a kiszolgáló tárat, hajtsa végre a módosításokat, és kattintson a kiszolgáló tárházba leküldéses őket. A módosítások véglegesítése, nyissa meg a mely számjegy parancs eszközt, lépjen a mappába, a helyi tárházba és hiba az alábbi parancsokat.

    git add --all
    git commit -m "Description of your changes"

Az összes a véglegesítése leküldéses a kiszolgálóra, futtassa a következő parancsot.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>A szolgáltatás konfigurációs módosításokat üzembe API Management szolgáltatás előfordulásáig

A helyi módosítások véglegesítése és a kiszolgáló tárházba tolni, miután a API Management szolgáltatás példányának telepítheti őket.

![Terjesztése][api-management-configuration-deploy]

A REST API-e a művelet a további tudnivalókért lásd [a REST API konfigurációs adatbázis terjesztése mely számjegy módosításait](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Fájlok és mappák helyi mely számjegy tárházba struktúra hivatkozása

A fájlokat és mappákat a helyi mely számjegy adattárban tartalmazzák a konfigurációs információkat a szolgáltatás példánya.

| Elem                       | Leírás                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| gyökérmappa api-kezelés | Legfelső szintű konfigurációs szolgáltatás példány tartalmazza.                                  |
| API-khoz mappa                | Az adatokat az API-hoz a szolgáltatás példány tartalmazza.                            |
| Csoportok mappára              | A csoportok a szolgáltatás példány konfigurációját tartalmazza                          |
| házirendek mappára            | A szolgáltatás példány a házirendek tartalmazza.                                              |
| portalStyles mappában        | Az adatokat a developer portal testre szabott funkciók a szolgáltatás példány tartalmazza. |
| Products mappa            | Az adatokat a termékek a szolgáltatás példány tartalmazza.                        |
| sablonok mappa           | Az e-mail sablonok a szolgáltatás példány az adatokat tartalmazza.                 |

Minden mappa tartalmazhat egy vagy több fájlt, és sőt bizonyos esetekben egy vagy több mappa, például az egyes API-t, a termék vagy a csoport egy mappát. Minden mappában lévő fájlok kifejezetten az a személy típusnak a mappa nevét.

| Fájltípus | Cél                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | A megfelelő entitás konfigurációs adatait                  |
| a HTML      | Leírások entitás, gyakran jelenik meg a developer Portal segítségével |
| XML       | Házirend-kimutatásokban                                                      |
| stíluslap       | Stíluslapok developer portal testreszabáshoz                        |

Ezek a fájlok létrehozott, törölt, szerkeszthető, és kattintson a helyi fájlrendszerben kezelt, végül a módosítások rendszerbe vissza az a API Management szolgáltatás példánya.

>[AZURE.NOTE] A következő vállalkozások nem található a mely számjegy tárat, és mely számjegy nem konfigurálhatók.
>
>-    Felhasználók
>-    Az előfizetések
>-    Tulajdonságok
>-    Developer portal szervezetek eltérő stílusok

### <a name="root-api-management-folder"></a>Gyökérmappa api-kezelés

A legfelső szintű `api-management` mappa tartalmaz egy `configuration.json` legfelső szintű információkat a szolgáltatás példánya, az alábbi formátumban tartalmazó fájlt.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Az első négy beállítások (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, és `UserRegistrationTermsConsentRequired`) térképre a **identitások** lap **Biztonság** szakaszában az alábbi beállításokat.

| Azonosító beállítása                     | Térképek                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Átirányítás a bejelentkezési lapja névtelen felhasználók** jelölőnégyzet |
| UserRegistrationTerms                | Szövegdoboz **a felhasználói azonosító létrehozása használati feltételei**               |
| UserRegistrationTermsEnabled         | **Regisztrációs lapján használati feltételei megjelenítése** jelölőnégyzet         |
| UserRegistrationTermsConsentRequired | **Felhasználó hozzájárul ahhoz szükséges** jelölőnégyzet                          |

![Személyes beállítások][api-management-identity-settings]

A következő négy beállítások (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, és `DelegationValidationKey`) az alábbi beállítások a **Meghatalmazás** lap **Biztonság** szakaszában a térképre.

| Meghatalmazás beállítása           | Térképek                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | **Meghatalmazott bejelentkezési és -előfizetési** jelölőnégyzet    |
| DelegationUrl                | Szövegdoboz **Meghatalmazás végpont URL-címe**        |
| DelegatedSubscriptionEnabled | **Meghatalmazott termék előfizetés** jelölőnégyzet |
| DelegationValidationKey      | **Meghatalmazott érvényesítési kulcs** szövegdoboz        |

![Meghatalmazás beállításai][api-management-delegation-settings]

Az utolsó beállítás `$ref-policy`, megfelelteti a globális házirend kimutatások fájl szolgáltatás példány.

### <a name="apis-folder"></a>API-khoz mappa

A `apis` mappa tartalmaz egy mappát az egyes API a szolgáltatás példány, amely tartalmazza az alábbiakat.

-   `apis\<api name>\configuration.json`– Ez az adatokat az API-t, és információt tartalmaz a kódmentes szolgáltatás URL-CÍMÉT és a műveletek. Ez a ugyanazokat az adatokat, mintha [első egy adott API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) hívás vissza `export=true` a `application/json` formátum.
-   `apis\<api name>\api.description.html`– Ez az API leírását és felel meg a `description` az [API entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties)tulajdonsága.
-   `apis\<api name>\operations\`– Ez a mappa `<operation name>.description.html` , amelyek a műveleteket a API megfeleltetése. Fájlonként a API, amely egyetlen művelet leírását tartalmazza a `description` tulajdonság a [művelet entitás](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) a REST API-t.

### <a name="groups-folder"></a>Csoportok mappára

A `groups` mappa tartalmaz az egyes csoportok a szolgáltatás példány definiált egy mappát.

-   `groups\<group name>\configuration.json`– Ez a beállítás a a csoport szimbólumára. Ez a ugyanazokat az adatokat, hívja fel az [adott csoport első](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) művelet mintha vissza.
-   `groups\<group name>\description.html`– Ez a csoport leírása és felel meg a `description` a [csoport entitás](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties)tulajdonsága.

### <a name="policies-folder"></a>házirendek mappára

A `policies` mappa a házirend-utasításait a szolgáltatás példány a tartalmazza.

-   `policies\global.xml`-a globális hatókör a szolgáltatást az előfordulásra vonatkozóan definiált házirendek tartalmazza.
-   `policies\apis\<api name>\`– Ha bármelyik API hatókör definiált házirendek, azok a mappában található.
-   `policies\apis\<api name>\<operation name>\`mappa – Ha bármelyik művelet hatókör definiált házirendek, azok helyezkednek ebben a mappában lévő `<operation name>.xml` , amelyek a házirend-utasításait a az egyes műveletek megfeleltetése.
-   `policies\products\`– Ha bármelyik a termék hatókör definiált házirendek, azok helyezkednek ezt a mappát, amelybe `<product name>.xml` , amelyek feleltesse meg a házirend-utasításait a két termékhez.

### <a name="portalstyles-folder"></a>portalStyles mappa

A `portalStyles` mappa developer portal testreszabások szolgáltatás példány konfigurálása és a stílus lap tartalmazza.

-   `portalStyles\configuration.json`-a stíluslapok, a developer portal által használt nevét tartalmazza.
-   `portalStyles\<style name>.css`-minden `<style name>.css` fájl tartalmaz a developer Portal stílusok (`Preview.css` és `Production.css` alapértelmezés szerint).

### <a name="products-folder"></a>Products mappa

A `products` mappa tartalmaz egy mappát a szolgáltatás példány definiált termékenként.

-   `products\<product name>\configuration.json`– Ez a beállítás a termékért. Ez a ugyanazokat az adatokat, hívja fel az [első egy adott termék](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) művelet mintha vissza.
-   `products\<product name>\product.description.html`– Ez a termék leírása és felel meg a `description` tulajdonság a [termék entitás](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) a REST API-t.

### <a name="templates"></a>sablonok

A `templates` mappa konfiguráció az [e-mail sablonok](api-management-howto-configure-notifications.md) a szolgáltatás-példány tartalmazza.

-   `<template name>\configuration.json`– Ez a beállítás az e-mailek sablon.
-   `<template name>\body.html`-e-mailek sablon szövege.

## <a name="next-steps"></a>Következő lépések

A szolgáltatás példányának kezelésének egyéb részleteivel a további tudnivalókért lásd:

-   A szolgáltatás példánya, az alábbi PowerShell-parancsmagok használata kezelése
    -   [Szolgáltatás telepítési PowerShell parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Szolgáltatás kezelése a PowerShell parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   A szolgáltatás példány a publisher portál kezelése
    -   [Az első API kezelése](api-management-get-started.md)
-   A szolgáltatás példány a REST API kezelése
    -   [API-kezelés REST API-hivatkozás](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Megtekintés az egy videohívások – áttekintés

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




