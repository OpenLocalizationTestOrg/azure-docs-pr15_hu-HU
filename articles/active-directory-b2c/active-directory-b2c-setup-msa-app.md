<properties
    pageTitle="Azure Active Directory B2C: A Microsoft-fiók konfigurálása |} Microsoft Azure"
    description="A Microsoft-fiókkal az alkalmazás, amely az Azure Active Directory B2C által biztosított fogyasztói előfizetési és a bejelentkezési nyújtani."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezési nyújtanak a Microsoft-fiókkal rendelkező felhasználóknak

## <a name="create-a-microsoft-account-application"></a>A Microsoft-fiók alkalmazás létrehozása

Microsoft-fiókot használ az Azure Active Directory (Azure Active Directory) B2C egy identitásszolgáltató, szüksége a Microsoft-fiók alkalmazás létrehozása, és adja meg azt a megfelelő paramétereket számára. Ehhez a Microsoft-fiók szükséges. Ha nincs telepítve egyik, a [https://www.live.com/](https://www.live.com/)kattint.

1. Nyissa meg a [Microsoft alkalmazás regisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be a Microsoft-fiókja hitelesítő adatait.
2. Kattintson **az alkalmazás hozzáadása**elemre.

    ![A Microsoft partner – új alkalmazás hozzáadása](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Adjon meg egy **nevet** az alkalmazáshoz, és kattintson az **alkalmazás létrehozása**.

    ![Microsoft-fiók - alkalmazás nevét](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Másolja az **Alkalmazás azonosítója**értékét. Szüksége lesz, hogy a Microsoft-fiók beállítása a bérlői webhelyemen identitás szolgáltatóként.

    ![Microsoft - fiókjából, azonosítója](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Kattintson a **Hozzáadás** platformon, és válassza a **webhely**.

    ![A Microsoft fiók - platform hozzáadása](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft-fiók - webhelyen](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` az **Átirányítás URL-címe** mezőben. **{Bérlői}** cserélje le a bérlő nevére (például contosob2c.onmicrosoft.com).

    ![Microsoft - fiókjából, átirányítás URL-címe](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Kattintson a **Új jelszó létrehozása** az **Alkalmazás titkos kulcsok** területen. Másolja a vágólapra a képernyőn megjelenő új jelszót. Szüksége lesz, hogy a Microsoft-fiók beállítása a bérlői webhelyemen identitás szolgáltatóként. Erre a jelszóra egy fontos biztonsági hitelesítő adatait.

    ![A Microsoft partner – az új jelszó létrehozása](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft - fiókjából, új jelszót](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. A jelölőnégyzet, amely szerint a **Live SDK támogatja** a **Speciális beállítások** szakaszban. Kattintson a **Mentés**gombra.

    ![Microsoft - fiókjából, Live SDK támogatás](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft-fiók szolgáltatás egy identitásszolgáltató a bérlői webhelyén

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a B2C szolgáltatások lap **Identitásszolgáltatók**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Az identitás-szolgáltató konfigurálása könnyen megjegyezhető **nevet** a szükséges. Írja be például a "MSA".
5. Kattintson az **identitás-szolgáltató típusa**, jelölje be a **Microsoft-fiókkal**, és kattintson az **OK gombra**.
6. Kattintson **a identitásszolgáltató beállítása** és azonosítója és Microsoft-fiók alkalmazás, amely a korábban létrehozott jelszót.
7. Kattintson az **OK gombra** , és kattintson a **Létrehozás** mentheti a Microsoft-fiók konfigurálása.
