<properties
    pageTitle="Azure Active Directory B2C: LinkedIn konfigurációs |} Microsoft Azure"
    description="LinkedIn-fiókkal az alkalmazás, amely az Azure Active Directory B2C által biztosított fogyasztói előfizetési és a bejelentkezési biztosítása"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezési nyújtanak a LinkedIn-fiókkal rendelkező felhasználóknak

## <a name="create-a-linkedin-application"></a>LinkedIn-alkalmazás létrehozása

LinkedIn-identitásszolgáltató az Azure Active Directory (Azure Active Directory) B2C használni, szüksége LinkedIn-alkalmazás létrehozása, és adja meg azt a megfelelő paramétereket számára. Ehhez a LinkedIn-fiókra van szüksége. Ha nincs telepítve egyik, a [https://www.linkedin.com/](https://www.linkedin.com/)kattint.

1. A [LinkedIn fejlesztők webhelyet](https://www.developer.linkedin.com/) , és jelentkezzen be a LinkedIn-fiókja hitelesítő adatait.
2. A menü felső sávon kattintson a **Saját alkalmazások** , és kattintson az **Alkalmazás létrehozása**gombra.

    ![LinkedIn - új alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Az **Új alkalmazás létrehozása** képernyőn adja meg a vonatkozó információkat (**Vállalatnév**, **név**, **Leírás**, **Alkalmazás embléma URL-címe**, **Alkalmazás használata**, **Webhely URL-címe**, **Vállalati e-mail címet** és **Munkahelyi telefon**).
4. Fogadja el a **LinkedIn API használati feltételeket** , és kattintson a **Küldés gombra**.

    ![LinkedIn - nyilvántartás alkalmazás](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Másolja az értékeket az **Ügyfél-azonosító** , és az **Ügyfél titkos**. (Megtalálhatja őket a **Hitelesítési kulcsokat**.) Mindkét LinkedIn-identitásszolgáltató a bérlői webhelyén állítsa be lesz szüksége.

    >[AZURE.NOTE] **Ügyfél titkos kulcs** egy fontos biztonsági hitelesítő adatait.

6. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` az **Átirányítás URL-címek engedélyezett** mezőben (a **OAuth 2.0-s**.). **{Bérlői}** cserélje le a bérlő nevére (például contoso.onmicrosoft.com). Kattintson a **Hozzáadás**gombra, és kattintson a **frissítés**gombra. A **{bérlői}** mező értéke a kis-és nagybetűk.

    ![LinkedIn - alkalmazás beállítása](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Állítsa be a LinkedIn-identitásszolgáltató a bérlői webhelyén

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a B2C szolgáltatások lap **Identitásszolgáltatók**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Az identitás-szolgáltató konfigurálása könnyen megjegyezhető **nevet** a szükséges. Írja be például a "LI".
5. Kattintson az **identitás-szolgáltató típusa**, jelölje be a **LinkedIn**, és kattintson az **OK gombra**.
6. Kattintson **a identitásszolgáltató beállítása** , és írja be az ügyfél-azonosító és a LinkedIn alkalmazás, amely a korábban létrehozott ügyfél titkos.
7. Kattintson az **OK gombra** , és kattintson a **Create** a LinkedIn-konfigurációjának mentése gombra.
