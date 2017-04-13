<properties
    pageTitle="Hogyan engedélyezhető a proxy-alkalmazásokkal natív ügyfele alkalmazások közzététele |} Microsoft Azure"
    description="Bemutatja, hogyan engedélyezhető az Azure Active Directory alkalmazás Proxy összekötő távoli eléréséhez a helyszíni alkalmazások biztonságos kommunikáció natív ügyfele alkalmazások."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Hogyan engedélyezhető a proxy alkalmazások vezérléséhez natív ügyfele alkalmazások

Azure Active Directory szolgáltatásalkalmazás-Proxy sokan használják böngésző alkalmazások, például a SharePoint, az Outlook Web Access és a vonalak egyéni üzleti alkalmazások közzététele. Is használható közzététele natív ügyfele alkalmazásokat, amelyek eltérnek web Apps alkalmazások, mert azok első egy eszközre telepítve. Ez történik, szabványos engedélyezheti a HTTP-fejlécekben küldött tokenek kiadott Azure Active Directory támogatásával.

![A végfelhasználók, az Azure Active Directory és a közzétett alkalmazások viszonya](./media/active-directory-application-proxy-native-client/richclientflow.png)

Az Azure Active Directory Authentication Library, mely az összes hitelesítési teljesen gondoskodik és számos különböző ügyfél környezet támogatja az ajánlott módszer az ilyen alkalmazások közzététele környezetbe. Szolgáltatásalkalmazás-Proxy illeszkedik a [Webes API forgatókönyv natív alkalmazás](active-directory-authentication-scenarios.md#native-application-to-web-api). Az erre történik, az alábbi képlettel történik:

## <a name="step-1-publish-your-application"></a>Lépés: 1: Az alkalmazás közzététele

Más alkalmazás módon, tegye közzé a proxy alkalmazás, adhatnak a felhasználóknak és premium vagy az egyszerű licencek neki. További információ: [szolgáltatásalkalmazás-Proxy közzététel alkalmazásokat](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Lépés: 2: Az alkalmazás beállítása

Állítsa be az alábbi képlettel történik a tartozó alkalmazásban:

1. Jelentkezzen be az Azure klasszikus portálra.
2. Jelölje ki az Active Directory ikont a bal oldali menüben, és válassza ki a címtárban.
3. A felső menüben kattintson az **alkalmazások**elemre. Ha nincs alkalmazások felvétele a címtárhoz, ezen az oldalon csak jelennek meg az **alkalmazás hozzáadása** hivatkozásra. Kattintson a hivatkozásra, vagy azt is megteheti, hogy kattint a parancssávon a **Hozzáadás** gombra.
4. **Milyen feladatot szeretne tenni** lapján kattintson a **szervezeten elkészítésének az alkalmazás hozzáadása**hivatkozására.
5. A **mondja el nekünk az alkalmazásról** lapon adjon meg egy nevet az alkalmazáshoz, és válassza a **natív ügyfélalkalmazás**. Kattintson a nyílikonra, a folytatáshoz.
6. **Alkalmazás adatai** lapon adja meg az **Átirányítás URI** natív ügyfele alkalmazásához, majd a jelet a befejezéshez kattintson.

Az alkalmazás kerül, és ekkor megnyílik a rövid útmutató lapra az alkalmazás.

## <a name="step-3-grant-access-to-other-applications"></a>3 lépés: A hozzáférés engedélyezése az egyéb alkalmazások

A natív alkalmazás más alkalmazások címtárában kitenni engedélyezése:

1. A felső menüben kattintson az **alkalmazások**elemre, jelölje be az új natív alkalmazást, és kattintson a **beállítás**.
2. Görgessen le az **engedélyek egyéb alkalmazások** csoportban. Kattintson a **Hozzáadás alkalmazás** gombra, és jelölje ki a hozzáférést az eredeti alkalmazást használni kívánt proxy alkalmazást, és kattintson a jobb alsó sarokban. A **Meghatalmazott engedélyeit** legördülő menüből válassza ki az új jogosultsági.

![Egyéb alkalmazások képernyőképe - engedélyeket az alkalmazás hozzáadása](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Lépés: 4: Az Active Directory Authentication Library szerkesztése

Szerkesztheti a natív alkalmazás kódot a hitelesítési környezetben, az Active Directory hitelesítési tár (ADAL) a következők:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

A változók módosítania kell, az alábbi képlettel történik:

- **TenantId** jobbra után "/ címtár /" megtalálható a globálisan egyedi azonosítója, az alkalmazás **beállítása** lapon URL-címét.
- **Frontend URL-cím** URL-címet jelenti az előtér megadott a Proxy alkalmazásban, és a proxy alkalmazás **beállítása** lapon található.
- A natív alkalmazás **Ügyfél-azonosító** **a natív alkalmazás lap** található.
- **Átirányítás URI natív alkalmazás** **a natív alkalmazás lap** található.

![Új natív alkalmazás lap kép beállítása](./media/active-directory-application-proxy-native-client/new_native_app.png)

A natív alkalmazás folyamat kapcsolatos további tudnivalókért olvassa el a [webes API tartozó alkalmazásban](active-directory-authentication-scenarios.md#native-application-to-web-api)című témakört.


## <a name="see-also"></a>Lásd még:

- [A saját tartománynév alkalmazások közzététele](active-directory-application-proxy-custom-domains.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Követelések tartsa szem előtt alkalmazások használata](active-directory-application-proxy-claims-aware-apps.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
