<properties
    pageTitle="Azure Active Directory B2C: Facebook konfigurációs |} Microsoft Azure"
    description="Facebook-fiókkal az alkalmazás, amely az Azure Active Directory B2C által biztosított fogyasztói előfizetési és a bejelentkezési nyújtani."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezési nyújtanak a Facebook-fiókkal rendelkező felhasználóknak

## <a name="create-a-facebook-application"></a>Facebook-alkalmazás létrehozása

Facebook-identitásszolgáltató az Azure Active Directory (Azure Active Directory) B2C használni, meg kell Facebook-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterrel. Ehhez a Facebook-fiókra van szüksége. Ha nincs telepítve egyik, a [https://www.facebook.com/](https://www.facebook.com/)kattint.

1. Nyissa meg a [fejlesztők számára a Facebookra](https://developers.facebook.com/) annak webhelyén, és jelentkezzen be a Facebook-fiókja hitelesítő adatait.
2. Ha még nem tette meg, meg kell Facebookkal fejlesztői regisztrálhatja. Ehhez kattintson a **regisztrálhatja** (kattintson a lap jobb felső sarkában található), fogadja el a Facebook-házirendeket és hajtsa végre a regisztráció.
3. Kattintson a **Saját alkalmazások hivatkozásra** , és kattintson a **Hozzáadás új alkalmazás**. Válassza a **webhely** , mint a platform, és kattintson a **Kihagyás és az alkalmazás azonosítója létrehozása**.

    ![Facebook - új alkalmazás hozzáadása](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook - webhely - új alkalmazás hozzáadása](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook - alkalmazás azonosító létrehozása](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Az űrlapon adja meg a **Megjelenítendő név**, egy érvényes **Partner e-mailek**, a megfelelő **kategóriát**, és kattintson az **Alkalmazás-azonosító létrehozása**. Ehhez az, hogy fogadja el a Facebook-platform házirendek, és fejezze be az online biztonsági jelölőnégyzetet.

    ![Facebook - alkalmazás új azonosító létrehozása](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Kattintson a bal oldali navigációs sávon kattintson a **Beállítások** gombra.
6. Kattintson a **+ Hozzáadás Platform** , és válassza a **webhely**.

    ![Facebook - beállítások](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebook - beállítások – webhely](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. A **Webhely URL-címe** mezőben adja meg a [https://login.microsoftonline.com/](https://login.microsoftonline.com/) , és kattintson a **Módosítások mentése**gombra.

    ![Facebook - webhely URL-címe](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Másolja az **Alkalmazás azonosítója**értékét. Kattintson a **megjelenítése** , majd másolja az **Alkalmazás**titkos értéket. Mindkét Facebook-identitásszolgáltató a bérlői webhelyén állítsa be lesz szüksége. **Alkalmazás titkos kulcs** egy fontos biztonsági hitelesítő adatait.

    ![Facebook - alkalmazás azonosítója és az alkalmazás titkos kulcs](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Kattintson a **+ Hozzáadás termék** a bal oldali navigációs sávon, majd a **Facebook bejelentkezési**melletti **Használatba** gombra kattintva.

    ![Facebook - Facebook bejelentkezés](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` az **Ügyfél OAuth Settings** szakasz **érvényes OAuth átirányítás URL-címe** mezőben. **{Bérlői}** cserélje le a bérlő nevére (például contosob2c.onmicrosoft.com). Kattintson a **Módosítások mentése** az oldal alján.

    ![Facebook - OAuth átirányítási URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. A Facebook-alkalmazás által Azure Active Directory B2C használhatóvá tételéhez szüksége nyilvánosan elérhetővé szeretné tenni. Ehhez **Alkalmazás Véleményezés** gombjával a bal oldali navigációs, és kapcsolja a Váltás a **Igen** értékre a lap tetején, és kattintson a **Megerősítés**.

    ![Facebook - alkalmazás nyilvános](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Állítsa be Facebook-identitásszolgáltató a bérlői webhelyén

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a B2C szolgáltatások lap **Identitásszolgáltatók**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Az identitás-szolgáltató konfigurálása könnyen megjegyezhető **nevet** a szükséges. Írja be például a "FB".
5. Kattintson az **identitás-szolgáltató típusa**, jelölje be a **Facebook**, és kattintson az **OK gombra**.
6. Kattintson **az identitásszolgáltató beállítása** , és írja be az alkalmazás azonosítója és az alkalmazás titkos (annak a Facebook-alkalmazás, amely a korábban létrehozott) az **Ügyfél-azonosító** , és az **ügyfél titkos** mezők rendre.
7. Kattintson az **OK gombra**, és kattintson a **Create** a Facebook-konfigurációjának mentése gombra.
