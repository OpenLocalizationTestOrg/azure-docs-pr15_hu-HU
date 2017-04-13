<properties
   pageTitle="Hogyan hozhat létre olyan alkalmazás, amely bármely Azure Active Directory felhasználói tudnak bejelentkezni |} Microsoft Azure"
   description="Lépésről lépésre utasításokat, amelyek az alkalmazások készítéséhez bejelentkezhet egy felhasználó az bármely Azure Active Directory-bérlőből, más néven egy több bérlői alkalmazást."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Bejelentkezés az Azure Active Directory (AD) bárki a több elem bérlői alkalmazás minta használatával
Ha sok szervezethez szolgáltatásalkalmazás, a szoftvert, beállíthatja a bármely Azure AD-bérlő bejelentkezések fogadja el az alkalmazás.  Az Azure Active Directory Link végez a többszörös alkalmazás-bérlő.  Bármely Azure AD-ös bérlői felhasználók tudják jelentkezzen be az alkalmazás után hozzájárul ahhoz, hogy a fiók használata az alkalmazást.  

Ha egy meglévő alkalmazást, amelyet a saját fiók rendszer, vagy más típusú más felhő szolgáltatóktól bejelentkezés támogatja, Azure AD-bejelentkezés felvétele az bármely bérlőből az csupán az alkalmazás rögzítése, bejelentkezési hozzáadása a kód oauth2 hitelesítési mód, a OpenID csatlakozás és a SAML és az alkalmazás elhelyezése a Bejelentkezés Microsoft gombbal. Ha többet szeretne tudni az alkalmazás védjegyzés az alábbi gombra.

[! [Gomb bejelentkezés] [AAD-bejelentkezés]][AAD-App-Branding]


Ez a cikk tartalma feltételezi, hogy már jól ismert létrehozása az Azure Active Directory egy egyetlen bérlői alkalmazást.  Ha nem, vissza felfelé a [Fejlesztői útmutató kezdőlap] vezetője[ AAD-Dev-Guide] , és próbálja meg a rövid indítást!

Az alkalmazás alakítani az Azure Active Directory több bérlői alkalmazás négy egyszerű lépésből áll:

1.  Az alkalmazás regisztráció kell több bérlői frissítése
2.  A kód kérelem küld a/Common végpont 
3.  A kód több kibocsátó értékek kezelése
4.  Felhasználói és felügyeleti hozzájárulása megértéséhez, és a megfelelő kódot módosításokat

Lássuk az egyes lépések részletesen. Akkor is nekiláthat [a több elem bérlői minták lista][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Több elem bérlői kell regisztráció frissítése
Alapértelmezés szerint a web app/API regisztrációk az Azure Active Directory egyetlen bérlői.  Is módosíthat a regisztrációt több bérlői a Váltás a "Alkalmazás van több bérlői" keresése az beállítása lapon az alkalmazás regisztráció az [Azure klasszikus portál] [ AZURE-classic-portal] és beállítása a "Igen".

Megjegyzés: Előtt alkalmazás több bérlői kell végezni, a Azure Active Directory csak az alkalmazás azonosítója URI globálisan egyedinek kell lennie az alkalmazás. Az alkalmazás azonosítója URI az alkalmazás azonosítjuk protokoll üzenetekben módokon.  Egy egyetlen bérlői alkalmazás elegendő, ha az alkalmazás azonosítója URI által belül egyedinek kell lennie.  Több elem bérlői alkalmazáshoz egyedinek kell lennie globálisan, az Azure Active Directory megtalálhatja az alkalmazás az összes bérlők között.  Az alkalmazás azonosítója URI van, amely megfelel a az Azure AD-bérlő igazolt tartományt állomásnév kötötten globális egyediségét lép érvénybe.  Ha például contoso.onmicrosoft.com, majd egy érvényes volt a neve a bérlő alkalmazás azonosítója URI akkor `https://contoso.onmicrosoft.com/myapp`.  Ha a bérlő volt az igazolt tartományt `contoso.com`, akkor is érvényes alkalmazás azonosítója URI `https://contoso.com/myapp`.  Az alkalmazás beállítása, több bérlői meghiúsul, ha az alkalmazás azonosítója URI nem követi a minta.

Natív ügyfele regisztrációk több bérlői alapértelmezés szerint.  Nem kell tennie semmit, hogy a natív ügyfél alkalmazás regisztrációs több bérlői.

## <a name="update-your-code-to-send-requests-to-common"></a>A kód összehívásokat küld/Common
Egy egyetlen bérlői alkalmazásban kérések bejelentkezés elküldi a bérlő bejelentkezés végpontot.   Ha például contoso.onmicrosoft.com a végpontot a következő lesz:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Összehívásokat küld egy bérlői végpontot, hogy az adott bérlői alkalmazások bérlői bejelentkezhet a felhasználók (vagy vendégek).  Több elem bérlői alkalmazással az alkalmazás nem tudja látható milyen bérlői a felhasználó származik, így nem kérést küld egy bérlői végpontot.  Ehelyett összehívásokat, amely az összes Azure AD-bérlők között multiplexes zárólap:

    https://login.microsoftonline.com/common

Ha az Azure AD meg a/Common kérelem érkezik végpontot, akkor a felhasználó bejelentkezik és következtében a mely a felhasználó bérlői találja.  A közös végpont működik-e minden Azure Active Directory által támogatott hitelesítéstípusok protokoll /: OpenID csatlakozhat, a OAuth 2.0-s verziója, a SAML 2.0-s és a Webszolgáltatás-összevonás.

A bejelentkezési választ, majd az alkalmazás, amely a felhasználó jogkivonat tartalmazza.  A token kibocsátó értéke milyen a felhasználó bérlői Ez az alkalmazás.  Ha a választ a/Common vissza végpontot, a token kibocsátó értékének felel meg a felhasználó bérlői.  Fontos megjegyzés: a/Common végpont nem bérlői webhelyet, és nem a kibocsátó, hogy csak egy multiplexer.  / Common használata esetén az alkalmazás tokenek érvényesítéséhez logika, figyelembe véve a frissítésre szorul. 

Korábbiak, több bérlői alkalmazások is biztosítania kell egy következetes bejelentkezés módja a felhasználóknak, védjegyzés irányelvek Azure Active Directory alkalmazása után. Ha többet szeretne tudni az alkalmazás védjegyzés az alábbi gombra.

[! [Gomb bejelentkezés] [AAD-bejelentkezés]][AAD-App-Branding]

A/Common e nézzük végpont és részletes kód Önnél.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>A kód több kibocsátó értékek kezelése
Webalkalmazások és a webes API-khoz fogadása, és az érvényesítés token származhat Azure AD.  

> [AZURE.NOTE] Mialatt natív ügyfélalkalmazásokban kérése a tokenek kapott Azure Active Directory, ehhez API-khoz, ha ellenőrizni szeretné küldeni.  Natív alkalmazások nem ellenőrizni a tokenek és kell kezelné átlátszatlanná válik.

Nézzük meg, az alkalmazás miként ellenőrzi az tokenek fogad Azure AD.  Egy egyetlen bérlői alkalmazás általában megnyílik egy végpont érték, például:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

és metaadatok URL-cím (jelen esetben OpenID csatlakozás) Egyenletszerkesztővel hasonlóan:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

két fontos információkat vehet tokenek érvényesítéséhez használt letöltéséhez: a bérlő általi aláírásának kulcsok és a kibocsátó értéket.  Minden egyes Azure AD-bérlő egyedi kibocsátó értéke az űrlap:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

Ha globálisan egyedi azonosítója értéke az Átnevezés vannak verzióját a bérlő bérlői azonosítója.  Ha a metaadat-alapú hivatkozására feletti kattintva `contoso.onmicrosoft.com`, megtekintheti, hogy a dokumentum kibocsátó értéket.

Amikor egy egyetlen bérlői alkalmazás ellenőrzi a jogkivonat, ellenőrzi az aláírást az aláírási kulcsok a metaadat-alapú dokumentumból szemben a jogkivonat, és ellenőrzi, a token kibocsátó értéke megegyezik, amely a metaadat-dokumentum található.

A/Common óta végpont nem felel meg a bérlői webhelyre, és nem a kibocsátó, amikor a metaadatait kibocsátó értéke megvizsgálja/közös van egy sablon URL-címe helyett a tényleges érték:

    https://sts.windows.net/{tenantid}/

Emiatt a több elem bérlői alkalmazás nem tudja érvényesíteni a tokenek csak a metaadatait kibocsátó értéke megkülönböztetésével a `issuer` a token értéket.  A több elem bérlői alkalmazások logika döntse el, mely kibocsátó értékek érvényesek, és amelyekre nincs szüksége van, a kibocsátó érték azonosító részét a bérlő alapján.  

Például ha egy több bérlői alkalmazás csak lehetővé teszi, hogy jelentkezzen be az adott bérlők, akik regisztráltak a szolgáltatásra, majd azt ellenőriznie kell vagy a kibocsátó érték vagy az `tid` formál érték a jogkivonat, győződjön meg arról, hogy a bérlői előfizetők a listában.  Ha a több elem bérlői alkalmazások csak személyek foglalkozik, és nem minden access alapuló döntések bérlők, majd azt figyelmen kívül hagyhatja a kibocsátó értéket teljesen.

A [Kapcsolódó tartalmak](#related-content) szakasz, ez a cikk végén található több bérlői mintákban kibocsátó érvényességi le van tiltva, ahhoz, hogy bármely Azure AD-bérlői bejelentkezni.

Most Vegyük szemügyre több bérlői alkalmazások bejelentkezés felhasználók számára a felhasználói felület.

## <a name="understanding-user-and-admin-consent"></a>Ismertetése felhasználó és a rendszergazda hozzájárulása
Jelentkezzen be az alkalmazás az Azure Active Directory, hogy a felhasználónak az alkalmazás meg kell jelennie a felhasználó bérlőhöz.  Ezzel a szervezet, mint az egyedi házirendek érvényesek, amikor a felhasználók nem a bérlői jelentkezzen be az alkalmazás dolgot kell tennie.  Egyetlen bérlői alkalmazáshoz a regisztráció az egyszerű; történik, amikor regisztrál az alkalmazás az [Azure klasszikus portál]egyetlen[AZURE-classic-portal].

Több elem bérlői alkalmazáshoz az alkalmazás a kezdeti regisztráció abban a Azure AD-bérlő a fejlesztő által használt.  Egy másik bérlői a felhasználó bejelentkezik az alkalmazás első alkalommal, amikor Azure Active Directory megkéri, hogy az engedélyeket az alkalmazás által kért beleegyezés.  Ha járul, majd az alkalmazás neve *szolgáltatás fő* ábrázolása a felhasználó bérlői jön létre, és bejelentkezés továbbra is. A meghatalmazás is rekordok az alkalmazás jóváhagyását adja a felhasználó a címtárban jön létre. Lásd: az [alkalmazás és a szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] további információt az alkalmazás alkalmazás és a ServicePrincipal objektumok, és hogyan egymással vonatkoznak.

![Beleegyezés egyrétegű alkalmazásba][Consent-Single-Tier] 

A felhasználó hozzájárul ahhoz élmény az engedélyeket az alkalmazás által kért érinti.  Azure Active Directory kétféle engedélyek, csak alkalmazás és a meghatalmazott támogatja:

- Meghatalmazott jogosultsági hozzárendeli az alkalmazás lehetővé teszi, hogy a műveleteket csak egy részhalmazát bejelentkezve felhasználóként a felhasználó teheti meg.  Például biztosíthat az alkalmazás a meghatalmazott engedélyt a bejelentkezett felhasználó naptár olvasható.
- Az alkalmazás csak az engedélyt közvetlenül az alkalmazáshoz.  Például az engedélyeket csak az alkalmazás az alkalmazás csak a Felhasználólista egy bérlői webhelyen olvasható, és ehhez függetlenül attól, aki be van jelentkezve az alkalmazás képes legyen.

Egyes engedélyeit is kell hozzájárulását normál felhasználó, míg másokat egy Bérlői rendszergazda kifejezett engedélyét. 

### <a name="admin-consent"></a>Rendszergazdai hozzájárulása
Csak alkalmazás engedélyek mindig csak egy Bérlői rendszergazda kifejezett engedélyét.  Ha az alkalmazás kéri az alkalmazás csak jogosultsági, és a normál felhasználó megkísérel jelentkezzen be az alkalmazás, az alkalmazás fog hibaüzenet közli, hogy a felhasználó nem tudja beleegyezés.

Bizonyos meghatalmazott engedélyeit is szükség egy Bérlői rendszergazda kifejezett engedélyét.  Például az azt jelenti, hogy írható bejelentkezve felhasználóként Azure AD vissza egy Bérlői rendszergazda hozzájárulása van szükség.  Csak alkalmazás engedélyek, például egy közönséges felhasználó ideje próbál bejelentkezni az alkalmazáshoz rendszergazdai engedély szükséges meghatalmazott engedélyt kér, ha az alkalmazás hibaüzenetet kap.  Engedély szükséges-e vagy sem felügyeleti hozzájárulása határozzák meg, hogy az erőforrás közzé a fejlesztői, és az erőforrás dokumentációjában lehet.  Az Azure Active Directory Graph API-val és a Microsoft Graph API az elérhető engedélyeket ismertető témakörökre mutató hivatkozások szerepelnek, a [Kapcsolódó tartalmi](#related-content) című szakaszát.

Ha az alkalmazás admin felhasználó hozzájárul ahhoz szükséges engedélyeket, meg kell lennie kézmozdulat az alkalmazásban, például egy gomb vagy egy hivatkozást, ahol az a felügyeleti kezdeményezhet a műveletet.  A kérelmet, az alkalmazás küld, a művelet eredménye egy szokásos oauth2 hitelesítési mód/OpenID csatlakozás engedélyezési kérelmet, de, amely magában foglalja a `prompt=admin_consent` karakterlánc paraméteres lekérdezés.  Miután a rendszergazda hozzájárult, és a szolgáltatás egyszerű jön létre a bérlői, kérések későbbi bejelentkezés nincs szükség a `prompt=admin_consent` paraméter.   A rendszergazda úgy döntött, a kért engedélyek elfogadhatók, mivel a bérlői webhelyemen nincs más felhasználók az adott ponttól beleegyezés kérni fogja.

A `prompt=admin_consent` paraméter is használható, nincs szükség rendszergazdai jóváhagyását adja, de szeretne adni, ahol a bérlői rendszergazdai "feliratkozik" eszköz az engedélyeket igénylő alkalmazások egyszeri alkalmazáshoz, és nincs más felhasználók ettől kezdve jóváhagyását adja a program kéri.

Ha egy alkalmazáshoz rendszergazdai kifejezett engedélyét, és az alkalmazásba bejelentkezik a rendszergazda, de a `prompt=admin_consent` paraméter nem küldi el, a felügyeleti tudják sikeresen beleegyezés az alkalmazás, de csak a felhasználói fiók fog beleegyezés.  Normál felhasználók fog még mindig nem tudja való bejelentkezést és az alkalmazás hozzájárul.  Ez akkor hasznos, ha meg szeretné adni, a bérlői rendszergazda ismerje meg az alkalmazást, mielőtt más felhasználók hozzáférést.

Bérlői rendszergazda letilthatja a normál felhasználók alkalmazások hozzájárul lehetősége.  Ha ez a lehetőség le van tiltva, felügyeleti hozzájárulása mindig szükség az alkalmazás, a bérlői webhelyemen be kell állítani.  Szeretne rendszeresen jóváhagyásával tiltható le az alkalmazás tesztelése, talál a konfigurációs váltás Azure Active Directory bérlőhöz a [Azure klasszikus portál]konfigurációs szakasz[AZURE-classic-portal].

> [AZURE.NOTE] Egyes alkalmazások szeretné egy élmény, ahol normál felhasználók csak beleegyezés az eredetileg, és később az alkalmazás a rendszergazda, és a kérelem engedélyek admin hozzájárulása igénylő is tartalmazhat.  Ehhez a ma Azure AD az egyetlen alkalmazásból regisztráció nincs mód nem.  A közelgő Azure Active Directory v2 végpont lehetővé teszi, futásidőben engedélyért alkalmazások helyett regisztrációs időben, amelyek lehetővé teszik, ebben az esetben.  További tudnivalókért lásd: az [Azure Active Directory alkalmazásmodell v2 Fejlesztőeszközök útmutató][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Felhasználó hozzájárul ahhoz és a több szálon alkalmazások
Előfordulhat, hogy az alkalmazás többszintű, minden egyes képviseli saját regisztrációs Azure AD.  Például egy belső alkalmazás, amely felhívja a webes API vagy webalkalmazás, hogy hívja fel a webes API-val.  A mindkét esetben az ügyfél (a natív app vagy a web app) kéréseket engedélyek hívja fel az erőforrás (webes API-val).  Az ügyfél kell sikeresen átadni kívánt hozzájárult e be a bérlői összes erőforrás, amelyhez engedélyek kér már léteznie kell a bérlői.  Ha ez a feltétel nem teljesül, a Azure Active Directory, hogy az erőforrás hozzá kell adnia először hibát ad vissza.

Ez lehet, hogy a program hibát, ha két vagy több alkalmazás regisztrációk, például egy külön ügyfél- és erőforrás áll a logikai alkalmazás.  Hogyan tudja elkezdeni az erőforrás be az ügyfél bérlői első?  Azure Active Directory ebben az esetben, mivel az ügyfél- és erőforrás szeretné átadni kívánt hozzájárult e egy lépésben, ahol megjelenik az engedélyeket az ügyfél és a felhasználó hozzájárul ahhoz lapon az erőforrás által igényelt összegeként terjed ki.  Ha engedélyezni szeretné a jelenség, az erőforrás-alkalmazás regisztrációs tartalmaznia kell az ügyfél alkalmazás azonosítója, a `knownClientApplications` a saját alkalmazások jegyzék.  Példa:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Ez a tulajdonság frissíthető keresztül az erőforrás- [alkalmazás jegyzék][AAD-App-Manifest], és a több szálon natív ügyfele, hívja fel a webes API minta a [Kapcsolódó tartalmak](#related-content) szakasz, ez a cikk végén található. Az alábbi ábrán hozzájárulása áttekintést nyújt a több szálon alkalmazásba:

![Beleegyezés több szálon ismert ügyfél alkalmazásba][Consent-Multi-Tier-Known-Client] 

Hasonló eset történik, ha az alkalmazás a különböző rétegek regisztrálva van a bérlők különböző.  Például érdemes megvizsgálni az esetben, hívja az Office 365-ben az Exchange Online API natív ügyfélalkalmazás tudnivalóit.  A natív alkalmazást, és az eredeti alkalmazást egy bérlői futhat újabb fejleszt, az Exchange Online szolgáltatás egyszerű jelen kell lennie.  Ebben az esetben az ügyfélnek kell vásárolnia az Exchange online-ban a szolgáltatás fő, létre kell hozni a bérlői rendelkezik.  Egy API-t a szervezeten kívül a Microsoft által épített, amíg az API-t a fejlesztői kell az ügyfelek számára az ügyfél bérlői webhelyre, például egy weblapot használata a jelen cikkben ismertetett eljárások hozzájárulása meghajtók alkalmazásuk beleegyezés ad lehetőséget.  A szolgáltatás egyszerű a bérlői webhelyemen létrehozását követően az eredeti alkalmazást az API tokenek szerezhet be.

Az alábbi ábrán hozzájárulása áttekintést nyújt a több szálon alkalmazás különböző bérlők regisztrált:

![Beleegyezés több szálon többrésztvevős alkalmazásba][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Felhasználó hozzájárul ahhoz visszavonása
Felhasználók és a rendszergazdák hozzájárulása az alkalmazás bármikor visszavonhatja:

- Felhasználók visszavonni a hozzáférést az egyes alkalmazások, az [Access-alkalmazások Panel] eltávolításával[ AAD-Access-Panel] listában.
- A rendszergazdák alkalmazások való hozzáférés visszavonása eltávolítja azokat az Azure Active Directory kezelése csoportban az [Azure klasszikus portal]segítségével Azure Active Directory[AZURE-classic-portal].

Ha a rendszergazda hozzájárul az alkalmazás az összes felhasználó számára egy bérlőhöz, felhasználók nem vonható vissza az access egyenként.  Csak a rendszergazda hozzáférést vonhatja, és csak a teljes alkalmazást.

### <a name="consent-and-protocol-support"></a>Felhasználó hozzájárul ahhoz és protokollok támogatása
Az Azure Active Directory keresztül az OAuth OpenID csatlakozni, támogatott hozzájárulása Webszolgáltatás-Összevonás és a SAML protokollok.  A SAML és Webszolgáltatás-összevonás protokollok nem támogatják a `prompt=admin_consent` paraméter, ezért felügyeleti felhasználó hozzájárul ahhoz csak alkalmazáson keresztül OAuth és OpenID csatlakozni.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Több elem bérlői alkalmazások és a gyorsítótár-hozzáférési jogkivonat
Több elem bérlői alkalmazások hozzáférési jogkivonat, hívja fel az Azure Active Directory eső API-khoz is megnyithatja.  A több elem bérlői alkalmazás az Active Directory hitelesítési tár (ADAL) használata esetén kezdetben a token/Common, használja a felhasználó kérése hibára arra választ kap, és kérjen meg egy későbbi token, hogy a felhasználók is használja a/Common.  Mivel az Azure Active Directory válasza származik, a bérlői, nem/közös, ADAL gyorsítótárát a jogkivonat, hogy a bérlőjének. A későbbi hívást/Common egy hozzáférési jogkivonat beszerzése a felhasználók tévesztések a gyorsítótár-bejegyzés, és a felhasználónak kell megadnia a jelentkezzen be újra.  Hiányzik a gyorsítótár elkerülése érdekében győződjön meg arról, hogy további hívások már bejelentkezve felhasználóhoz történik a bérlő végpontot.

## <a name="related-content"></a>Kapcsolódó tartalom

- [Több elem bérlői alkalmazás minta][AAD-Samples-MT]
- [Alkalmazások irányelveket védjegyzése][AAD-App-Branding]
- [Azure Active Directory fejlesztői útmutató][AAD-Dev-Guide]
- [Alkalmazás és a szolgáltatás egyszerű objektumok][AAD-App-SP-Objects]
- [Alkalmazások integrálása az Azure Active Directory][AAD-Integrating-Apps]
- [A felhasználó hozzájárul ahhoz keretrendszer áttekintése][AAD-Consent-Overview]
- [A Microsoft Graph API jogosultsági hatókörök][MSFT-Graph-AAD]
- [Azure Active Directory Graph API jogosultsági hatókörök][AAD-Graph-Perm-Scopes]

Az alábbi Disqus Megjegyzések szakaszt segítségével visszajelzést, és segítse a szolgáltatás szűkítése és alakíthat ki a tartalmat.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














