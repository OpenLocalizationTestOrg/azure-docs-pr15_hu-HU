<properties
   pageTitle="Az Azure Active Directory-alkalmazás jegyzék ismertetése |} Microsoft Azure"
   description="Az Azure Active Directory alkalmazás jegyzék, mely-identitás alkalmazást Azure AD-ös bérlői jelöl, és OAuth engedélyezési, hozzájárulása élmény és az egyéb megkönnyítésére szolgál részletes körét."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Az Azure Active Directory-alkalmazás jegyzék ismertetése

Az Azure Active Directory (AD) együttműködő alkalmazások regisztrálva az Azure Active Directory bérlői, az alkalmazás állandó identitás konfigurációjának nyújtó kell lennie. Ez a beállítás van egyeztetni futásidőben forgatókönyvek, amelyek lehetővé teszik az erőforrás kihelyezése és ügynök hitelesítési/engedélyezési keresztül Azure AD-alkalmazás engedélyezése. Az Azure Active Directory alkalmazásmodell kapcsolatos további tudnivalókért olvassa el a [hozzáadása, frissítése, és az alkalmazás eltávolítása] [ ADD-UPD-RMV-APP] cikket.

## <a name="updating-an-applications-identity-configuration"></a>Az alkalmazások identitás konfiguráció frissítése

Lehetőség ténylegesen több érhető el az alkalmazások identitás konfigurálásról, amelyek változnak a lehetőségek és fokban kifejezett nehézségi, többek között az alábbi tulajdonságainak módosítása:

- A ** [Azure klasszikus portál] [ AZURE-CLASSIC-PORTAL] webes felhasználói felülete** lehetővé teszi, hogy az alkalmazás a leggyakoribb tulajdonságainak módosítása. Ez a leggyorsabb és legalább egy hiba jobban módja a szolgáltatásalkalmazás tulajdonságainak módosítása, de nem ad teljes hozzáférés az összes tulajdonságait, például a következő két módszerrel.
- Speciális felhasználási területei, ahol frissíteni kell tulajdonságaik vannak, amelyek nem láthatóak az Azure klasszikus portálon módosíthatja az **alkalmazás cikkét**. A fókusz a jelen cikk, és elindítása a következő szakaszban részletesebb Budai tárgyalja.
- Hogy az is lehetséges, hogy **Írja be a [Diagram API -t] használó alkalmazás[ GRAPH-API] ** frissítése az alkalmazás, amely a legtöbb munkamennyiség igényel. Ez lehet vonzó lehetőség, hogy, ha az éppen írt kezelő szoftvert, vagy szolgáltatásalkalmazás tulajdonságainak automatizált módon rendszeresen frissítenie kell.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Használatával az alkalmazás jegyzék frissítése az alkalmazások identitás beállítása
Az [Azure klasszikus portálon]keresztül[AZURE-CLASSIC-PORTAL], letöltésével kezelheti az identitás alkalmazást, és egy JSON feltöltése fájl megjelenítése területen egy alkalmazás jegyzék nevezik. A könyvtár nincs tényleges fájl vannak tárolva. Az alkalmazás jegyzék csupán HTTP GET műveletet az Azure Active Directory Graph API alkalmazás entitás, a feltöltés pedig az alkalmazás egyed HTTP javítás műveletet.

Emiatt annak érdekében, hogy a formátum és az alkalmazás jegyzék tulajdonságok ismertetése, meg kell a Graph API- [Alkalmazás egyed] hivatkozás[ APPLICATION-ENTITY] dokumentációt. Frissítések, ha az alkalmazás feltöltése cikkét elvégezhető példája:

- **A Declare jogosultsági hatókörök (oauth2Permissions)** a webes API által elérhetővé tett. A "Közzéteszi webes API-hoz való egyéb alkalmazások" című témakörben alkalmazásokban [integrálása az Azure Active Directory címtárral] [ INTEGRATING-APPLICATIONS-AAD] felhasználói megszemélyesítési megvalósításáról információt a meghatalmazott jogosultsági hatókör a oauth2Permissions használatával. A korábban említett, alkalmazás entitás tulajdonságai ismertetett Graph API [entitás és összetett típusú] [ APPLICATION-ENTITY] valhttps://technet.microsoft.com/library/dn741250.aspx foglalkozó referenciacikkünket, beleértve a oauth2Permissions tulajdonságot, amely [OAuth2Permission]típusú gyűjtemény[APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Az alkalmazás által elérhetővé tett alkalmazás Declare szerepkörök (appRoles)**. Az alkalmazás egyed appRoles tulajdonság típusa [AppRole]gyűjteménye[APPLICATION-ENTITY-APP-ROLE]. [Szerepköralapú hozzáférés-vezérlés felhő alkalmazások használata az Azure Active Directory] témakörben[ RBAC-CLOUD-APPS-AZUREAD] a cikk végrehajtás példát.
- **A Declare ismert ügyfél alkalmazások (knownClientApplications)**, amely lehetővé teszi, hogy a felhasználó hozzájárul ahhoz a megadott ügyfél futó erőforrás/webes API logikailag kötik.
- **Azure AD-csoport tagsági formál kibocsátása kérése** a bejelentkezett felhasználó (groupMembershipClaims).  Ez is beállítható a felhasználó címtár szerepkörök tagságot kapcsolatos igények kiadását. Lásd: a [felhő alkalmazásokban engedélyezési az Active Directory-csoportok] [ AAD-GROUPS-FOR-AUTHORIZATION] a cikk végrehajtás példát.
- **Adja meg az alkalmazás a támogatási OAuth 2.0-s Implicit engedélyezése** flow (oauth2AllowImplicitFlow). A támogatási folyamat típusú beágyazott JavaScript weblapok és egyetlen lap alkalmazások (biztonságos jelszó-hitelesítés) használt. Implicit engedély megadása a további tudnivalókért lásd: a [forgalom az Azure Active Directory ismertetése implicit oauth2 hitelesítési mód megadása][IMPLICIT-GRANT].
- **A titkos kulcs tanúsítványok X509 használatának engedélyezése** (keyCredentials). Lásd [az Office 365 szolgáltatás és démon alkalmazások készítése] [ O365-SERVICE-DAEMON-APPS] és [Fejlesztői útmutató auth Azure erőforrás-kezelő API-val való] [ DEV-GUIDE-TO-AUTH-WITH-ARM] végrehajtása példák cikkeket.
- **Hozzáadás új alkalmazás azonosítója URI** az alkalmazás (identifierURIs[]). Alkalmazás azonosítója URL-címe használatával egyértelműen azonosítják az alkalmazások az Azure AD-bérlő belül (vagy több Azure AD-bérlők, több bérlői felhasználási területei ellenőrzött egyéni tartomány használatával minősített keresztül). Erőforrás alkalmazásba, vagy egy erőforrás-alkalmazáshoz egy access jogkivonat beszerzésekor engedélyek kérésekor használják. Ha ezt az elemet, akkor az alkalmazás kezdőlap bérlői abban a megfelelő szolgáltatás egyszerű servicePrincipalNames [] webhelycsoport frissítése történik.

Az alkalmazás jegyzék a nyomon követheti az alkalmazás regisztrációs állapota jó módszer is tartalmaz. Érhető el a JSON formátumot, mert a fájl ábrázolása ellenőrizhető be az adatforrás-vezérlő, az alkalmazás-forráskód együtt.

## <a name="step-by-step-example"></a>Lépés a lépést példa alapján
Lehetővé teszi a lépéseket az alkalmazás identitás-konfiguráció – az alkalmazás jegyzék frissítése elemre. Azt kiemeli az előző példában egyikét meg kell adnia egy erőforrás alkalmazást az új jogosultsági hatókör ábrázoló:

1. Nyissa meg az [Azure klasszikus portál] [ AZURE-CLASSIC-PORTAL] , és jelentkezzen be egy szolgáltatási vagy közös rendszergazdája jogosultságokkal rendelkező fiókkal.

2. Miután már hitelesített, görgessen lefelé, és válassza ki a Azure "Active Directory" bővítmény a bal oldali navigációs panel (1), majd kattintson a a hol található a az alkalmazás Azure AD-bérlő regisztrált a (2).

    ![Jelölje ki a Azure AD-bérlő][SELECT-AZURE-AD-TENANT]

3. Amikor megjelenik a címtár lapra, kattintson a "Alkalmazások" (1) regisztrált a bérlői webhelyemen alkalmazások listájának megjelenítéséhez a lap tetején lévő. Keresse meg az alkalmazást szeretné frissíteni a listában, és kattintson rá (2).

    ![Jelölje ki a Azure AD-bérlő][SELECT-AZURE-AD-APP]

4. Most, hogy az alkalmazás fő lapján lehetőséget választotta, figyelje meg a lapon (1) a "Kezelése cikkét" funkciót. Ha ez a hivatkozásra kattintott, a rendszer kéri, töltse le, és töltse fel a nyilvánvalóan JSON-fájlt. Kattintson a "Letöltés jegyzék" (2). Ez azonnal követni kell a Letöltés gombra kattintva "Letöltése cikkét" (3), majd vagy nyissa meg, és mentse a fájlt helyileg (4) megerősítést kér megerősítést kérő párbeszédpanel.

    ![A jegyzék kezelése, töltse le a beállítás][MANAGE-MANIFEST-DOWNLOAD]

    ![Töltse le a jegyzék][DOWNLOAD-MANIFEST]

5. Ebben a példában azt a fájlt mentette a helyi meghajtóra, számunkra, hogy szerkesztő megnyitása, módosíthatja a JSON és mentése ismét lehetővé. Az alábbiakban a JSON-struktúra néz ki a Visual Studio JSON-szerkesztő belül. Megjegyzés: az [Alkalmazás egyed] ezt követi a séma[ APPLICATION-ENTITY] amint azt már említettük:

    ![A nyilvánvalóan JSON frissítése][UPDATE-MANIFEST]

    Ha például feltételezve, hogy a megvalósítása/elérhetővé tétele az erőforrás-alkalmazás (API) "Employees.Read.All" nevű új jogosultsági szeretnénk, egyszerűen vegyen fel egy új/második elem oauth2Permissions gyűjteményhez, ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    A bejegyzés egyedinek kell lennie, és ezért kell létrehozni egy új globálisan egyedi azonosítója (GUID) a `"id"` tulajdonság. Ebben az esetben mert adott azt `"type": "User"`, ezzel az engedéllyel is lehet átadni kívánt hozzájárult e az erőforrás/API-alkalmazás regisztrált a bérlői bármilyen fiókhoz, az Azure Active Directory hitelesíti. Adja meg ezt az ügyfél nevében a fiók eléréséhez alkalmazás engedéllyel. A leírás és a megjelenítendő név karakterláncok használja hozzájárulása során, valamint a az Azure klasszikus portálon megjelenítését.

6. Ha végzett a jegyzék frissítése, térjen vissza az Azure klasszikus portál az Azure Active Directory alkalmazás lapján, és kattintson a "Kezelése cikkét" funkció újra (1). De ebben az esetben válassza a "Feltöltése cikkét" lehetőséget (2). A letöltés hasonlóan, fog fogja köszönteni és újra egy második párbeszédpanel segítségével kér a JSON-fájl helyét. Kattintson a "Keresse meg a fájlt..." (3), majd a "Válassza ki a fájlfeltöltési" párbeszédpanel használatával jelölje ki a JSON-fájlt (4), és nyomja le a "Open". A párbeszédpanel eltűnik, miután jelölje be a "OK" jelet (5), és a jegyzék fel kell tölteni.  

    ![A jegyzék kezelése, töltse fel a beállítás][MANAGE-MANIFEST-UPLOAD]

    ![Töltse fel a nyilvánvalóan JSON][UPLOAD-MANIFEST]

    ![Töltse fel a nyilvánvalóan JSON - megerősítése][UPLOAD-MANIFEST-CONFIRM]

Most, hogy a jegyzék mentésekor adhat meg a bejegyzett alkalmazás ügyfélelérési az új jogosultsági azt fölé kerül. Ez esetben az Azure klasszikus portál webes felületének helyett használhatja az ügyfélalkalmazás jegyzék szerkesztése:  

1. Először nyissa meg a "" lap az ügyfélalkalmazás, amelyhez hozzá kívánja adni a hozzáférést az új API-nak, és kattintson a "Alkalmazás hozzáadása" gombra.
2. Ezután, formában jelennek meg a bejegyzett erőforrás-alkalmazást (API-k) a bérlői webhelyemen listáját. Kattintson a plusz / + szimbólum jelölje ki az erőforrás-alkalmazáshoz neve mellett.  
3. Kattintson a pipa ikonra a képernyő jobb alsó sarkában.
4. Amikor visszatér az ügyfélnek beállítása lapon a "Alkalmazás hozzáadása" című szakaszát, látni fogja az új erőforrás alkalmazást a listában. Ha a "Meghatalmazott engedélyeit" szakasz jobbra lévő sorát mutat, látni fogja a legördülő lista jelenik meg. Kattintson a listára, majd jelölje be az új jogosultsági annak érdekében, hogy vegye fel az ügyfél kért engedélyek listája.. Megjegyzés: Ez az új jogosultsági kíván tárolni a ügyfélalkalmazás identitás konfigurációs, a "requiredResourceAccess" webhelycsoport tulajdonságban.

![Egyéb alkalmazások engedélyek][PERMS-TO-OTHER-APPS]

![Egyéb alkalmazások engedélyek][PERMS-SELECT-APP]

![Egyéb alkalmazások engedélyek][PERMS-SELECT-PERMS]

Ez azt. Most már az alkalmazások használata a saját új identitás konfiguráció fog futni.

## <a name="next-steps"></a>Következő lépések
- Kattintson az alkalmazások alkalmazás és a szolgáltatás egyszerű objektum(ok) viszonya, lásd: [alkalmazás és a szolgáltatás fő objektumok az Azure Active Directory][AAD-APP-OBJECTS].
- Lásd: az [Azure Active Directory Fejlesztőeszközök szószedet] [ AAD-DEVELOPER-GLOSSARY] néhány alapvető Azure Active Directory (AD) fejlesztői fogalmak definícióját.

Az alábbi DISQUS Megjegyzések szakaszt segítségével visszajelzést, és segítse a szolgáltatás pontosítása és a tartalom alakzathoz.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

