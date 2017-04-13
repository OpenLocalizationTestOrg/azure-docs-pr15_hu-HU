<properties
    pageTitle="Azure Active Directory B2C: Google + konfigurációs |} Microsoft Azure"
    description="A Google + fiókokat az Azure Active Directory B2C által biztosított alkalmazások fogyasztói előfizetési és a bejelentkezési nyújtani."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezési nyújtanak a Google + fiókkal rendelkező felhasználóknak

## <a name="create-a-google-application"></a>A Google +-alkalmazás létrehozása

A Google + használni egy identitásszolgáltató az Azure Active Directory (Azure Active Directory) B2C, szüksége hozhat létre a Google + alkalmazást, és adja meg azt a megfelelő paraméterrel. A Google + fiók ehhez szükséges. Ha nincs telepítve egyik, a [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)kattint.

1. Nyissa meg a [Google fejlesztők konzol](https://console.developers.google.com/) , és jelentkezzen be a Google + fiók hitelesítő adatait.
2. Kattintson a **Projekt létrehozása**, adjon meg egy **projekt nevét**, és kattintson a **Létrehozás**gombra.

    ![A Google + – első lépések](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![A Google +,-új projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Kattintson a **API-kezelő** , és kattintson a bal oldali navigációs sávon kattintson a **hitelesítő adatok** .
4. Kattintson a képernyő tetején a **OAuth hozzájárulása képernyő** fülre.

    ![A Google +,-hitelesítő adatok](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Jelölje ki vagy adja meg egy érvényes **E-mail címet**, adja meg a **termék neve**, és kattintson a **Mentés**gombra.

    ![A Google +,-OAuth hozzájárulása képernyő](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Kattintson az **új hitelesítő adatokat** , és válassza ki az **ügyfél-azonosító OAuth**.

    ![A Google +,-OAuth hozzájárulása képernyő](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. A **típus**csoportban jelölje ki a **webalkalmazást**.

    ![A Google +,-OAuth hozzájárulása képernyő](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Adja meg, adjon meg egy **nevet** az alkalmazás `https://login.microsoftonline.com` **engedélyezni a JavaScript forrásokból** mezőjébe, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` az **engedélyezett átirányítása URL-címe** mezőben. **{Bérlői}** cserélje le a bérlő nevére (például contosob2c.onmicrosoft.com). A **{bérlői}** mező értéke a kis-és nagybetűk. Kattintson a **létrehozása**gombra.

    ![A Google +, - ügyfél-azonosító létrehozása](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Másolja az értékeket az **Ügyfél-azonosító** , és az **ügyfél titkos**. Mindkét állítsa be a Google + egy identitásszolgáltató a bérlői webhelyemen szüksége lesz. **Ügyfél titkos kulcs** egy fontos biztonsági hitelesítő adatait.

    ![A Google +,-ügyfél titkos kulcs](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Állítsa be a Google + egy identitásszolgáltató a bérlői webhelyén

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a B2C szolgáltatások lap **Identitásszolgáltatók**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Az identitás-szolgáltató konfigurálása könnyen megjegyezhető **nevet** a szükséges. Ha például írja be "G +".
5. Kattintson az **identitás-szolgáltató típusa**, jelölje be a **Google**, és kattintson az **OK gombra**.
6. Kattintson **a identitásszolgáltató beállítása** , és írja be az ügyfél-azonosító és a Google + alkalmazás, amely a korábban létrehozott ügyfél titkos.
7. Kattintson az **OK gombra** , és kattintson a **Create** a Google + konfigurációjának mentése gombra.
