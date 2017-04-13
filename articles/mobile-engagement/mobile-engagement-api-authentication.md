<properties 
    pageTitle="Mobil részvételét REST API-hoz a hitelesítéshez"
    description="Megtudhatja, hogy miként hitelesítést végezni az Azure Mobile tetszés szerint elmélyedhet REST API-hoz" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Mobil részvételét REST API-hoz a hitelesítéshez

## <a name="overview"></a>– Áttekintés

A dokumentum egy érvényes AAD Oauth jogkivonat hitelesíteni a Mobile tetszés szerint elmélyedhet REST API-k beszerzése ismerteti. 

Feltételezett értéke Azure érvényes előfizetése van, és létrehozott egy Mobile tetszés szerint elmélyedhet alkalmazás segítségével a [Fejlesztői oktatóanyagok](mobile-engagement-windows-store-dotnet-get-started.md)közül.

## <a name="authentication"></a>Hitelesítés

A Microsoft Azure Active Directory jogkivonat hitelesítéshez OAuth alapján. 

Hitelesítési egy API kérésre sorrendben egy engedélyezési fejléc hozzá kell adnia minden kérést, amely az alábbi képernyő:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Azure Active Directory tokenek lejár, az 1 óra értéket.

Többféleképpen jogkivonat eléréséhez. Mivel az API-k általában úgynevezett egy felhőbeli szolgáltatástól, az API-ja kulcs használni kívánt. Az API-ja kulcs az Azure terminológia szolgáltatás fő jelszó neve. Az alábbi lépésekkel lehet manuális beállítása.

### <a name="one-time-setup-using-script"></a>Egyszeri beállításához (parancsfájl használatával)

Kövesse a készlete hajtja végre a telepítést használata egy PowerShell-parancsprogramot, a telepítő minimális időt vesz igénybe, de a legtöbb megengedett alapértékeit használja az alábbi utasításokat. Tetszés szerint is kövesse a [Kézi beállítás](mobile-engagement-api-authentication-manual.md) ezzel az Azure portálról közvetlenül és finomabb konfigurációs lehetőségek. 

1. Lekérése az Azure PowerShell legújabb verzióját [az alábbi](http://aka.ms/webpi-azps). A letöltési utasításokat a további tudnivalókért lásd: erre a [hivatkozásra](../powershell-install-configure.md).  

2. Azure PowerShell telepítése után az alábbi parancsokkal annak érdekében, hogy az **Azure modul** telepítve van:

    egy. Ellenőrizze, hogy az Azure PowerShell-modult érhető el a rendelkezésre álló modulok listáját. 
    
        Get-Module –ListAvailable 

    ![Rendelkezésre álló Azure modulok][1]
        
    b. Ha nem találja az Azure PowerShell-modult a fenti listában, majd futtassa a következő kell:
        
        Import-Module Azure 
        
3. Bejelentkezés az Azure erőforrás-kezelő a PowerShell futtassa az alábbi parancsot, és a felhasználónév és jelszó megadásával az Azure-fiók: 
        
        Login-AzureRmAccount

4. Ha több előfizetéssel, majd futtassa a következő:

    egy. Lista beszerzése az összes, és másolja a használni kívánt előfizetés SubscriptionId. Ellenőrizze, hogy az előfizetés ugyanarra a számítógépre, amely mellett a mobil tetszés szerint elmélyedhet Appot, amely az API-k használata vezérléséhez fogja. 

        Get-AzureRmSubscription

    b. A következő parancsot a SubscriptionId nyújtó konfigurálása az előfizetést, hogy használhatók.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. A helyi számítógépre másolja a vágólapra a [New-AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) parancsfájl szövegét, és mentse egy PowerShell-parancsmag (pl. `APIAuth.ps1`), majd hajtsa végre, akkor `.\APIAuth.ps1`. 
    
6. A parancsfájl kérni fogja adatbevitelt **principalName**. Adja meg, hogy szeretné-e az Active Directory-alkalmazás (például APIAuth) létrehozása egy megfelelő nevet. 

7. A parancsprogram befejeződése után jelenik meg a következő négy értékeket AD a programozás útján hitelesítéshez kell ezért ügyeljen billentyűparanccsal másolja a vágólapra. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**vagy **titkos**.

    TenantId szerint használni kívánt `{TENANT_ID}`, mint ApplicationId `{CLIENT_ID}` és titkos, mint `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Az alapértelmezett biztonsági házirend tiltása a rendszert futtató egy PowerShell-parancsfájlokat. Ha igen, a végrehajtás házirend parancsfájl végrehajtása a következő parancsot használja ideiglenes konfigurálása:

        > Set-ExecutionPolicy RemoteSigned

8. Az alábbiakban hogyan PS parancsmagok halmazára lenne. 

    ![][3]

9. Az Azure kezelőportálja ellenőrizze, hogy egy új Active Directory-alkalmazás nevét a megadott nevű **principalName** csoportban **a munkahelyemen alkalmazásokat megjelenítése a tulajdonosa**a parancsprogram készült.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Érvényes token lépést

1. Hívja fel az alábbi paramétereknek API, és ellenőrizze, hogy le szeretné cserélni a BÉRLŐ\_azonosító, az ügyfél\_Azonosítóját és az ügyfél\_titkos:

    - **Kérés URL-CÍMÉT** , *https://login.microsoftonline.com/ {BÉRLŐI\_azonosító} / oauth2 hitelesítési mód/jogkivonat*
    - **HTTP-tartalomtípus fejléc** *alkalmazás/x-www-form-urlencoded* szerint
    - **HTTP-összehívás törzsébe** *Adja meg az\_típus = ügyfél\_hitelesítő adatok & client_id = {ügyfél\_azonosító} & client_secret = {ügyfél\_titkos} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Az alábbi példa felkérés el:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Íme egy példa válasz:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    Ebben a példában szereplő URL-kódolás a bejegyzés paraméterek `resource` értéke ténylegesen `https://management.core.windows.net/`. URL-címet is ügyeljen arra, hogy kódolását `{CLIENT_SECRET}` , lehet, hogy a speciális karaktereket tartalmaz.

    > [AZURE.NOTE] A tesztelésre, például [Fiddler](http://www.telerik.com/fiddler) vagy a [Chrome Postman bővítmény](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) HTTP ügyfél eszköz is használata 

2. Most már minden API-hívás, többek között az engedélyezési kérelem fejléc:

        Authorization: Bearer {ACCESS_TOKEN}

    Ha egy 401 állapotkódot adja vissza, jelölje be a választ szervezet azt lehet mondani, a token lejárt. Ebben az esetben kap egy új token.

##<a name="using-the-apis"></a>Az API-k használata

Most, hogy érvényes token, készen áll az API-hívásokhoz.

1. Az egyes API-összehívási ablak szüksége lesz érvényes, le nem járt token szerezte be az előző szakaszban átadandó.

2. Szüksége lesz a beépülő modul egyes paraméterek URI, amely azonosítja az alkalmazás-összehívásba. A kérés URI néz ki a következő

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Ismerkedés a paraméterek, kattintson az alkalmazás neve és irányítópult, és kattintson megjelenik egy lapja a 3 paraméterekkel az alábbiakhoz hasonlóan.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** az erőforráscsoport neve lesz **MobileEngagement** kivéve, ha létrehozott egy újat. 

    ![Mobil tetszés szerint elmélyedhet API URI paraméterei][2]

>[AZURE.NOTE] <br/>
>1. Figyelmen kívül hagyása az API legfelső szintű címet, mint ez volt az előző API.<br/>
>2. Ha az alkalmazás Azure klasszikus portál használatával létrehozott majd szüksége az alkalmazás erőforrás nevét, amely ugyanaz, mint az alkalmazás neve magát. Ha az Azure-portálon hozta létre az alkalmazását, az alkalmazás nevére, magát a (nincs alkalmazás erőforrás nevét és az alkalmazás nevét az új portál készült alkalmazások nem megkülönböztetése) kell használni.  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



