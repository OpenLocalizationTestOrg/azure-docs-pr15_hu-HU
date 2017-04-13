<properties
    pageTitle="Első lépések a Azure Active Directory identitás védelme és a Microsoft Graph |} Microsoft Azure"
    description="Egy Microsoft Graph query – bevezetés nyújt a kockázat események és a kapcsolódó információk az Azure Active Directory."
    services="active-directory"
    keywords="Azure active directory-azonosító adatok védelmét, kockázati esemény, rés, biztonsági házirendek: Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Első lépések a Azure Active Directory identitás védelme és a Microsoft Graph

Microsoft Graph pedig a Microsoft egységesített API végpont az otthoni [Azure Active Directory azonosító adatok védelmét meg](active-directory-identityprotection.md) API-khoz. Az első API **identityRiskEvents**, lehetővé teszi a Microsoft Graph lekérdezés listáját a [kockázat események](active-directory-identityprotection-risk-events-types.md) és a kapcsolódó információk. Ez a cikk a lépéseket, ez az API lekérdezése kap. Egy a mélység bemutatása, a teljes dokumentáció és a hozzáférést a Graph Intéző lásd: a [Microsoft Graph-webhelyet](https://graph.microsoft.io/).

Az identitás-védelem eléréséhez a Microsoft Graph keresztül három lépésből áll:

1. Egy ügyfél titkos az alkalmazás hozzáadása. 

2. A titkos és néhány egyéb információkat vehet használja a Microsoft Graph, amelyre szeretne kapni egy hitelesítési jogkivonat hitelesítést végezni. 

3. Ez a token segítségével kéréseket küld az API-végpontot, és visszatérhet az identitás-védelem adatok.

Mielőtt hozzákezdene, akkor van szükség:

- Az alkalmazás létrehozása az Azure Active Directory rendszergazdai jogosultságokkal
- A bérlő tartománynevet (például contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Egy ügyfél titkos az alkalmazás hozzáadása


1. [Jelentkezzen be](https://manage.windowsazure.com) rendszergazdaként a Azure klasszikus portálra. 

1. A bal oldali navigációs területen kattintson **Az Active Directory**. 

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. A **címtár** listából válassza ki a címtárban, amelynek a címtár-integrációs engedélyezni szeretné.

3. A felső sávon kattintson az **alkalmazások**elemre.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. **Milyen feladatot szeretne tenni** párbeszédpanelen kattintson a **szervezeten fejlesztésével az alkalmazás hozzáadása**lehetőséget.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. A **mondja el nekünk az alkalmazásról** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    egy. **Neve** mezőbe írja be az alkalmazás nevét (például: AADIP kockázati esemény API-alkalmazás).

    b. **Írja be**jelölje ki a **webes alkalmazás és/vagy a webes API**.

    c billentyűkombinációt. Kattintson a **Tovább**gombra.


5. Az **alkalmazás tulajdonságai** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    egy. Az **Bejelentkezési URL** mezőbe írja be a `http://localhost`.

    b. Az **Alkalmazás azonosítója URI** mezőbe írja be a `http://localhost`.

    c billentyűkombinációt. Kattintson a **kész**gombra.


A lehet most konfigurálhatja az alkalmazást.

![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Alkalmazás engedélyt adhat az API


1. A menüben, a képernyő tetején, az alkalmazás lapon kattintson a **Konfigurálás**gombra. 

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. **Engedélyek egyéb alkalmazások** csoportban kattintson az **alkalmazás hozzáadása**elemre.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. Az **engedélyek egyéb alkalmazások** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    egy. Jelölje be **A Microsoft Graph**.

    b. Kattintson a **kész**gombra.


1. Kattintson a **Alkalmazásengedélyek: 0**, és válassza az **identitás kockázati esemény információkat olvashatja**.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. A lap alján a **Mentés** gombra.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Első hívóbetű

1. Az alkalmazás lapon, a **billentyűk** szakaszában kattintson a egyéves as időtartam.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. A lap alján a **Mentés** gombra.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. billentyűk csoportban az újonnan létrehozott kulcs értékét másolja és illessze be egy megbízható helyre.

    ![Alkalmazás létrehozása](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Ha megszakad a kulcsot, akkor térjen vissza a szakasz, és hozzon létre egy új kulcsot. A kulcs egy titkos megtartása: bárki, aki hozzáférhet az adatokhoz.


1. A **Tulajdonságok** csoportban az **Ügyfél-azonosító**másolni, és illessze be egy megbízható helyre. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Microsoft Graph hitelesítő és a lekérdezés az identitás kockázat események API

Ezen a ponton rendelkeznie kell:

- A fenti lépésben kimásolt ügyfél-azonosító

- A fenti lépésben kimásolt billentyűt

- A bérlő tartománynevet


Egy bejegyzés kérelem küldése hitelesítést végezni, `https://login.microsoft.com` törzsébe az alábbi paraméterekkel:

- grant_type: "**client_credentials**."

- erőforrás: "**https://graph.microsoft.com**."

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Meg kell adnia az értékeket a **client_id** és a **client_secret** paramétert.

Sikerrel jár, ha ez egy hitelesítési jogkivonat adja eredményül.  
Hívja fel az API-t, a következő paraméterrel fejléc létrehozása:

    `Authorization`=”<token_type> <access_token>"


Hitelesítő, amikor a token típusát, és a hozzáférési jogkivonat a található a visszaadott jogkivonat.

Küldje el az alábbi API URL-cím kérésre ezt a fejlécet:`https://graph.microsoft.com/beta/identityRiskEvents`

A kérdésre adott választ sikerrel jár, ha gyűjteménye identitás kockázat események és a kapcsolódó adatok OData JSON formátumban, amelynek elemezni, és tetszés szerint kezeli.

Az alábbiakban a hitelesítése, és hívja fel a Powershell használatá API minta kódját.  
Csak az ügyfél-azonosító billentyűt, és tartomány bérlői.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Következő lépések

Gratulálunk, csak az első hívás Microsoft Graph elvégzett!  
Most a lekérdezés identitás kockázat események és használni az adatokat, de tetszés szerint.

Ha többet szeretne megtudni a Microsoft Graph, és hogyan hozhat létre a Graph API alkalmazások, jelölje be a [dokumentáció](https://graph.microsoft.io/docs) és sokkal a a [Microsoft Graph-webhelyet](https://graph.microsoft.io/). Győződjön meg arról, hogy az [Azure Active Directory-identitás védelem API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) lap, amely felsorolja a identitás védelem API-k Graph könyvjelzőt szeretne. Új módszerek a identitás védelem keresztül API hozzáadunk, látni fog őket, hogy a lapon.


## <a name="additional-resources"></a>További források

- [Azure Active Directory identitás-védelem](active-directory-identityprotection.md)

- [Kockázat események észleli Azure Active Directory identitás védelem típusai](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph-hoz](https://graph.microsoft.io/)

- [A Microsoft Graph áttekintése](https://graph.microsoft.io/docs)

- [Azure Active Directory-identitás védelmi szolgáltatás legfelső szintű](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
