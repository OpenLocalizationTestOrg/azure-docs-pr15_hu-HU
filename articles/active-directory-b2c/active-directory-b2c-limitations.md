<properties
    pageTitle="Azure Active Directory B2C: Korlátai és korlátozásokat |} Microsoft Azure"
    description="Korlátozások és Azure Active Directory B2C korlátozások"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Korlátai és korlátozásai

Több funkciók és Azure Active Directory (Azure Active Directory) B2C még nem támogatott funkciói is van. Számos e ismert problémák és korlátozások címzettje lesz Ugrás előre, de legyen tudatában legyenek ezeknek fogyasztói elérésű alkalmazások használata az Azure Active Directory B2C készítésekor.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Az Azure Active Directory B2C bérlők kibocsátása során esetlegesen fellépő problémák

Ha problémákat tapasztal a [az Azure Active Directory B2C bérlői kibocsátása](active-directory-b2c-get-started.md)során, olvassa el a [az Azure AD-bérlő és Azure Active Directory B2C bérlői – hibák és megoldások létrehozása](active-directory-b2c-support-create-directory.md) útmutatást.

Megjegyzendő, hogy vannak ismert hibák egy meglévő B2C bérlői törlése és hozza létre újból a azonos nevű tartományban. Meg kell B2C bérlő létrehozása, egy másik tartománynevet.

## <a name="note-about-b2c-tenant-quotas"></a>B2C bérlői kvóták kapcsolatos megjegyzés:

Alapértelmezés szerint B2C-ös bérlői felhasználók számát az 50 000 felhasználók korlátozott. Ha előléptetése a B2C bérlői kvóta van szüksége, forduljon támogatási.

## <a name="branding-issues-on-verification-email"></a>A megerősítő e-mailt esetleges problémákról

Az alapértelmezett megerősítő e-mailt a Microsoft védjegyzés tartalmazza. Azt törlődik a jövőben. Fontos a [vállalat esetleges funkció](../active-directory/active-directory-add-company-branding.md)használatával eltávolíthatja azt.

## <a name="restrictions-on-applications"></a>Alkalmazások korlátozásai

A következő típusú alkalmazásokat jelenleg nem támogatottak az Azure Active Directory B2C. Az alkalmazások által támogatott típusú leírását, tekintse át [Azure Active Directory B2C: típusú alkalmazások](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Egyetlen lap alkalmazások (JavaScript)

Számos modern alkalmazás egyetlen lap alkalmazás (egészében) előtér-elsősorban a JavaScript írt van, és gyakran használja a biztonságos jelszó-hitelesítés keretrendszer, például AngularJS, Ember.js, Durandal és stb. Ez a folyamat még nem áll rendelkezésre az Azure Active Directory B2C.

### <a name="daemons--server-side-applications"></a>Démonok / kiszolgálóoldali alkalmazások

Alkalmazások tartalmazó, hosszú ideig futó folyamatok vagy anélkül, hogy a felhasználó jelenlétét, amelyek működnek kell védett erőforrások, például a webes API-khoz elérése oly módon is. Ezeket az alkalmazásokat hitelesítheti és tokenek első az alkalmazás azonosítója (helyett delegált egy ügyfél-azonosító) az [OAuth 2.0 ügyfél hitelesítő adatok folyamat](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow)használatával. Ez a folyamat még nem érhető el az Azure Active Directory B2C, így most az alkalmazások abban, hogy tokenek csak azt követően egy interaktív fogyasztói bejelentkezési folyamat történt.

### <a name="standalone-web-apis"></a>Különálló webes API-hoz

Az Azure Active Directory B2C van az azt jelenti, hogy [a webes API OAuth 2.0-s tokenek használatával, biztonságos összeállítása](active-directory-b2c-apps.md#web-apis). Azonban, hogy a webes API csak akkor tokenek érkeznek megosztó alkalmazás azonosító ügyfélről Nem támogatott felépíteni a webes API-val, amely számos különböző ügyfelektől érhető el.

### <a name="web-api-chains-on-behalf-of"></a>A webes API lánccal (a-nevében-:)

Sok architektúrákban egy másik lefelé irányuló webes API, mind az Azure Active Directory B2C által biztosított hívás kell, hogy a webes API tartalmazzák. Ebben az esetben a natív ügyfélprogramokat sorolja fel pazarolják egy webes API vissza, amely meghívja a a Microsoft online tárhelyre, például az Azure Active Directory Graph API közös.

Ebben az esetben láncolt webes API OAuth 2.0-s Jwt Bearer hitelesítő adatok megadását, más néven tovább nevében – az áramlás használatával nem használható. Azonban a On-nevében – a folyamat nem jelenleg használható az Azure Active Directory B2C.

## <a name="restriction-on-libraries-and-sdks"></a>A korlátozás a tárakban és SDK

A támogatott Microsoft tárak Azure Active Directory B2C működő készlete jelenleg korlátozott. Van még a .NET-alapú web Apps alkalmazások és szolgáltatások, valamint NodeJS web Apps alkalmazások és szolgáltatások támogatása.  Azt is, hogy az Azure Active Directory B2C a Windows és az egyéb .NET-alkalmazások használható MSAL neve előzetes .NET ügyfél tárban.

Hogy jelenleg nem rendelkeznek támogatja a nyelvek vagy platformhoz, többek között az iOS és Android-alapú tárat.  Ha szeretne egy másik platform, mint a fent említett épülnek, a Megnyitás-forrás SDK csomagjában talál, az [OAuth 2.0-s és OpenID csatlakozás protokoll hivatkozás](active-directory-b2c-reference-protocols.md) szükség szerint használata ajánlott.  Azure Active Directory B2C alkalmazza az OAuth & OpenID csatlakozni, amely lehetővé teszi annak integrációs általános OAuth vagy OpenID csatlakozás tár használata.

Az iOS és az első lépések az Android oktatóprogramokat, amely tesztelése Megnyitás-adatforrás-tárak használata a Azure Active Directory B2C való kompatibilitás érdekében.  Az összes a rövid oktatóanyagok érhetők el az [első lépések](active-directory-b2c-overview.md#getting-started) szakaszában.

## <a name="restriction-on-protocols"></a>A korlátozás a protokollok

Azure Active Directory B2C OpenID csatlakozás és a OAuth 2.0-s támogatja. Azonban nem az összes funkciójára és lehetőségére, az egyes protokollok végrehajtani. Jobban megértheti az Azure Active Directory B2C, olvassa el a [OpenID teremthetnek OAuth 2.0-s protokoll hivatkozás](active-directory-b2c-reference-protocols.md)támogatott protokollt funkciók körét. SAML és WS-Fed protokoll támogatás nem érhető el.

## <a name="restriction-on-tokens"></a>A korlátozás a tokenek

Az Azure Active Directory B2C által kibocsátott tokenek számos JSON webes tokenek vagy JWTs végrehajtását. Nem minden JWTs (más néven "követelések") tárolt adatok számára azonban nem igazán, meg kell lennie, vagy nem található. Többek között a "sub" és "preferred_username" követelések.  Értékeket, a formátum vagy a követelések időbeli változás szerinti marad, hogy a meglévő házirendek tokenek változnak – számíthat az értékeket, gyártás-alkalmazásokban.  Módosíthatja az értékeket, akkor képet ad konfigurálni ezekhez a változtatásokhoz az egyes a házirendek lehetőséget.  A jelenleg az Azure Active Directory B2C szolgáltatás által kibocsátott tokenek nhttp://OnlineHelp.microsoft.com/hu-hu/Bing/dn261810.aspx, olvassa el a saját [jogkivonat-hivatkozást](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Beágyazott csoportok korlátozása

Beágyazott csoporttagság az Azure Active Directory B2C bérlők nem támogatja. Nem tervezzük hozzáadása ezt a lehetőséget.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Azure Active Directory Graph API-megkülönböztető lekérdezés funkció korlátozása

Az [Azure Active Directory Graph API-megkülönböztető lekérdezés funkció](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) használata nem támogatott, az Azure Active Directory B2C bérlők. Ebben az esetben a hosszú távú ütemterv.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>A klasszikus Azure portálon felhasználókezelés kapcsolatos problémák

Az Azure portálon B2C funkciók érhetők el. Azonban az Azure klasszikus portal segítségével elérheti a más bérlői funkcióiról, például a felhasználók kezelése. Jelenleg létezik néhány ismert problémák a felhasználók kezelése (a **felhasználók** lapon) az Azure klasszikus portálon:

- Egy helyi felhasználó (tehát a fogyasztói ki feliratkozik egy e-mail címet és jelszót, vagy egy tartozó felhasználónevével és jelszavával) a **Felhasználónév** mező nem felelnek meg a bejelentkezési azonosító (e-mail cím vagy username) előfizetési során használt. Ennek oka a mező jelenik meg az Azure klasszikus portál ténylegesen az egyszerű felhasználónév (UPN), nem használt B2C helyzetekben. A helyi fiók bejelentkezési azonosító megtekintéséhez [Graph Explorer](https://graphexplorer.cloudapp.net/)a user objektumban található. Közösségi fiók felhasználóval (tehát a fogyasztói ki regisztrál Facebook, a Google +, stb.) megtalálja a ugyanazt a problémát, de ebben az esetben nem nincs bejelentkezési azonosítóját a felolvasás.

    ![Helyi fiókok - (UPN)](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Helyi fiókok a felhasználó a program nem lehet szerkeszteni a mezőkre, és mentse a módosításokat a **profil** lapon.

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Rendszergazda által kezdeményezett jelszó alaphelyzetbe állítása az Azure klasszikus portálon kapcsolatos problémák

Ha a jelszó alaphelyzetbe egy helyi ügyfélalapú fogyasztási az Azure klasszikus Portal (a **Jelszó alaphelyzetbe állítása** parancsot a **felhasználók** lapon), a fogyasztói nem tudja módosítani a jelszavát a következő bejelentkezés, ha egy bejelentkezési használja, vagy jelentkezzen be a házirendet, és ki az alkalmazások zárolva lesz. Kerülő segítségével az [Azure Active Directory Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) (nélkül jelszólejárati) a fogyasztói jelszó alaphelyzetbe állítása vagy egy bejelentkezési helyett egy bejelentkezési házirend használata, vagy jelentkezzen be a házirend.

## <a name="issues-with-creating-a-custom-attribute"></a>Egyéni attribútum létrehozásával kapcsolatos problémák

[Azure a portálon felvett egyéni attribútum](active-directory-b2c-reference-custom-attr.md) nem azonnal készül a B2C bérlői webhelyén. Be kell legalább egy, a házirendek az egyéni attribútum a B2C bérlői létrejönnek és Graph API keresztül elérhetővé válnak a.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>A klasszikus Azure portálon tartomány igazolásával kapcsolatos problémák

Jelenleg nem tudja ellenőrizni sikeresen a [Azure klasszikus portál](https://manage.windowsazure.com/)tartomány számára.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>A bejelentkezési problémák MFA házirend Safari böngészőkben

Időnként a Safari böngészőben (hibás kérés) HTTP 400 a hibát tartalmazó bejelentkezési házirendek (Ha be van kapcsolva MFA) az fail kérelmeket. Ez a Safari's alacsony cookie fájlméretet érintő korlátozásokról esedékes. Néhány keressen ilyen hibát megoldásokat alkalmazhatja:

- A "regisztráció vagy bejelentkezés házirend" a "bejelentkezési házirend" helyett használja.
- A házirend kért **alkalmazás követelések** számának csökkentése.
