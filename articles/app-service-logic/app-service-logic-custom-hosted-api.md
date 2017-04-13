<properties
    pageTitle="Hívja fel egy egyéni API összefüggés-alkalmazásokban"
    description="Az alkalmazás szolgáltatás logika alkalmazással is egyéni API segítségével"
    authors="stepsic-microsoft-com"
    manager="dwrede"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>

# <a name="using-your-custom-api-hosted-on-app-service-with-logic-apps"></a>Az alkalmazás szolgáltatás logika alkalmazással is egyéni API segítségével

Habár logika alkalmazások összekötők 40 + szolgáltatások sokféle széles körű, érdemes felhívása saját egyéni API-val, amely a saját kód futtatható. A legegyszerűbb és a legtöbb méretezhető módjai üzemelteti saját egyéni webes API egyik alkalmazás szolgáltatás használatát. Ez a cikk bemutatja, hogy miként szeretne csatlakozni az bármely webes API-alkalmazás szolgáltatáshoz tartozó API alkalmazás, webalkalmazás vagy mobilalkalmazásban is.

A API-k létrehozása az eseményindító vagy művelet összefüggés-alkalmazásokból, olvassa el [Ez a cikk](app-service-logic-create-api-app.md).

## <a name="deploy-your-web-app"></a>A webes alkalmazások terjesztése

Első lépésként kell telepíteni az API a Web App alkalmazás szolgáltatásban. Az útmutatást egyszerű telepítési terjed ki: [az ASP.NET web-alkalmazás létrehozása](../app-service-web/web-sites-dotnet-get-started.md).  Logika alkalmazásból felhívhatja be minden olyan API, miközben az optimális használat javasoljuk, hogy könnyen integrálása logika alkalmazások műveletek Swagger metaadat-alapú hozzáadása.  [Swagger](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)megadásáról olvashat.

### <a name="api-settings"></a>API-beállítások

Ahhoz, hogy a logika alkalmazások Tervező a Swagger elemzésére fontos, hogy CORS engedélyezése, és a webalkalmazás APIDefinition tulajdonságainak beállítása.  Ez az egyszerű belül az Azure-portálra.  Egyszerűen nyissa meg a beállítások lap a Web App és az API című szakaszt területen adja meg a "API-definíció", az URL-cím a swagger.json fájl (Ez általában https://{name}.azurewebsites.net/swagger/docs/v1), és a CORS házirend hozzáadása "*" Designer logika alkalmazásokból kérések engedélyezése.

## <a name="calling-into-the-api"></a>Az API közölt

A logikai alkalmazások portál belül, ha CORS van állítva, és az API Definition tulajdonságai látnia kell a könnyen hozzáadása egyéni API műveletek a munkafolyamat belül.  A Tervező választhat a webhelyek listáját definiált swagger URL-előfizetés webhelyek tallózással.  Is használhatja a HTTP + Swagger művelet mutasson egy swagger és listában elérhető műveletekről és a bemeneti adatok alapján.  Végezetül mindig hozhat létre a HTTP-művelet használata bármely API, azok is, amelyek nem vagy elérhetővé tenni a dokumentum swagger felhívására kérést.

Ha szeretne a API biztonságos, majd többféleképpen néhány megtennie:

1. Kód ugyanakkora kötelező - Azure Active Directory védelme a API kód módosításokat és újratelepítés anélkül is használható.
1. Alapszintű hitelesítés, AAD Auth vagy tanúsítvány Auth hivatkozási be a kódot a API.

## <a name="securing-calls-to-your-api-without-a-code-change"></a>Hívások átirányítása a kód megváltoztatása nélkül API biztonságossá tétele

Ebben a részben kell létrehoznia a két Azure Active Directory alkalmazások – egy, az logika számára és egy, a webalkalmazásban.  Hívások átirányítása a Web App használatával (ügyfél-azonosító és titkos) a szolgáltatás egyszerű, társított AAD alkalmazása az logika számára fogja hitelesítést végezni. Végezetül is tartalmazni fogja az alkalmazást az app logika definíciójának azonosítók.

### <a name="part-1-setting-up-an-application-identity-for-your-logic-app"></a>Rész 1: Az logika alkalmazás alkalmazás identitás beállítása

Ez az a logika alkalmazást használja az active directory hitelesítő. Egyetlen *szükséges* egyszer a könyvtár. Például megadhatja az azonos identitás használható összes logika alkalmazás, bár is létrehozhat egy logikai alkalmazás egyedi identitások sorból. Ehhez a felhasználói felület, vagy PowerShell-lel.

#### <a name="create-the-application-identity-using-the-azure-classic-portal"></a>Az alkalmazás-azonosítóját az Azure klasszikus portálon létrehozása

1. Nyissa meg azt [az klasszikus portálon Azure Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) , és válassza a könyvtárra, használhatja a Web App alkalmazásban
2. Kattintson az **alkalmazások** fülre
3. Kattintson a **Hozzáadás** gombra a lap alján a parancssáv
4. Nevezze el az identitás, kattintson a következő nyíl
5. Elhelyezése, formázása: a két szövegdobozok tartomány egyedi karakterláncot, és kattintson a pipa
6. Kattintson a **beállítás** fülre, ehhez az alkalmazáshoz
7. Másolja az **Ügyfél-azonosító**, a rendszer ezt használja a logika alkalmazásban
8. A **billentyűk** szakaszban kattintson a **Jelölje ki az időtartam** , és válassza a 1 év és a 2 évben
9. Kattintson a **Mentés** gombra a képernyő alján a képernyő (is Várjon néhány másodpercet)
10. Most már meg róla, hogy másolja a vágólapra a kulcsot a mezőben. Ez lesz is a logika alkalmazásban

#### <a name="create-the-application-identity-using-powershell"></a>A PowerShell használatá alkalmazás azonosító létrehozása
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Ellenőrizze, hogy a **Bérlői azonosító**, az **Alkalmazás azonosítója** és a jelszó használatával

### <a name="part-2-protect-your-web-app-with-an-aad-app-identity"></a>2 rész: A Web App AAD alkalmazás identitással védelme

Ha már telepítette az a Web app csak engedélyezheti újra a portálon. Egyéb esetben teheti hitelesítés engedélyezése a Azure erőforrás-kezelő üzembe része.

#### <a name="enable-authorization-in-the-azure-portal"></a>Az Azure-portálon engedélyezése

1. Nyissa meg a Web app, majd kattintson a parancssávon a **Beállítások** gombra.
2. Kattintson az **Engedély/hitelesítési**.
3. Kapcsolja **be**.

Ezen a ponton az alkalmazás automatikusan létrejön. Van szüksége az alkalmazás ügyfél-azonosító rész 3, így kell:

1. Nyissa meg [az klasszikus portálon Azure Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) , és válassza ki a címtárban.
2. A Keresés mezőbe az alkalmazás keresése
3. Kattintson rá a listában
4. Kattintson a **beállítás** lapon
5. Meg kell jelennie a **Ügyfél-azonosító**

#### <a name="deploying-your-web-app-using-an-arm-template"></a>A Web App-ARM sablonnal üzembe helyezése

Első lépésként meg kell a Web App alkalmazás létrehozása. Ez az alkalmazás, amellyel a logika alkalmazás eltér kell lennie. Indítsa el a fenti lépéseket követően rész 1, de most **Kezdőlap** és **IdentifierUris** használatára vonatkozó tényleges https://**URL-CÍMÉT** a Web app.

>[AZURE.NOTE]A webalkalmazás az alkalmazás létrehozásakor a PowerShell-parancsmag nem a megfelelő engedélyekkel felhasználók jelentkezhet be a webhely beállításának használja az [Azure klasszikus portál megközelítés](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).

Ha már van az ügyfél és a következő, a Web app sub erőforrásként szerepeltetni a telepítési sablon bérlői-azonosító:

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

A telepítő automatikusan, amelyek egy üres webes és logika alkalmazást együtt AAD használó üzembe helyezése futtatásához kattintson a következő gombra:

[![Azure telepítése](./media/app-service-logic-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

A teljes sablon [logika alkalmazás hívások be egy egyéni API alkalmazás szolgáltatás is és AAD védett](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json)témakört.


### <a name="part-3-populate-the-authorization-section-in-the-logic-app"></a>3 rész: A logika alkalmazásban az engedélyezés szakasz feltöltése

A **HTTP** -művelet **Engedélyezés** csoportban:`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Elem | Leírás |
|---------|-------------|
| típus | Hitelesítés típusa. ActiveDirectoryOAuth hitelesítéshez érték ActiveDirectoryOAuth. |
| Bérlői webhelyen | A bérlői azonosítója azonosítja az Active Directory-ös bérlői webhelyet. |
| a célközönség | Szükséges. Az erőforrás csatlakozik. |
| clientID | Az Azure Active Directory-alkalmazás ügyfél azonosítója. |
| titkos kulcs | Szükséges. Az ügyfél, a token igénylő titkos. |

A fenti sablon beállítása a már rendelkezik, de ha a logikai alkalmazást közvetlenül társszerzőként, kell a teljes engedélyezési szakaszt.

## <a name="securing-your-api-in-code"></a>Az API-kód biztonságossá tétele

### <a name="certificate-auth"></a>Tanúsítvány-hitelesítés

Ügyfél tanúsítványok is használhatja a Web App alkalmazásban a bejövő felkérést érvényesítéséhez. [Hogyan szeretné konfigurálása TLS kölcsönös hitelesítés webalkalmazás](../app-service-web/app-service-web-configure-tls-mutual-auth.md) , hogy hogyan állíthatja be a kód témakörben talál.

*Engedély* szakaszában meg kell adnia: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`.

| Elem | Leírás |
|---------|-------------|
| típus | Szükséges. Hitelesítés típusa. Az ügyfél SSL-tanúsítványok az érték ClientCertificate kell lennie. |
| PFX | Szükséges. A PFX fájl Base64 kódolású tartalmát. |
| jelszó | Szükséges. A jelszó a PFX fájl elérésére. |

### <a name="basic-auth"></a>Alapszintű hitelesítés

Alapszintű hitelesítés (például felhasználónév és jelszó) segítségével ellenőrzi a bejövő kéréseket. Alapszintű hitelesítés közös mintát, és az alkalmazás generál a nyelvi lehetőségek.

*Engedélyezés* csoportban meg kell adnia: `{"type": "basic","username": "test","password": "test"}`.

| Elem | Leírás |
|---------|-------------|
| típus | Szükséges. Hitelesítés típusa. Alapszintű hitelesítés az érték Basic kell lennie. |
| felhasználónév | Szükséges. Felhasználónév hitelesítést végezni. |
| jelszó | Szükséges. Jelszó hitelesítést végezni. |

### <a name="handle-aad-auth-in-code"></a>Fogópont AAD auth a kódot.

Alapértelmezés szerint az Azure Active Directory-hitelesítés, amely engedélyezi a portálon végezze el külön engedélyt. Ha például nem zárolja az adott felhasználó vagy alkalmazás, de csak egy adott bérlői API-val.

Ha meg szeretné korlátozni az API csak a logika alkalmazást, például a kódot, kibonthatja a fejléc, amely tartalmazza a JWT és ellenőrizheti, hogy ki a hívó van, elutasítása bármely kérelmeket, amelyek nem felelnek meg.

További, most alkalmazása a saját kód egészében, és nem használja a a portálon szolgáltatás ki szeretné, ha ez a cikk erről: [Használja az Active Directory hitelesítéshez Azure App szolgáltatásban](../app-service-web/web-sites-authentication-authorization.md).

Továbbra is kell a fenti lépéseket követve alkalmazás identitás az összefüggés-alkalmazás létrehozása, és hívja fel az API-t használja.
