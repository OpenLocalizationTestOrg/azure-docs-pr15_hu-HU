<properties
    pageTitle="Azure Active Directory B2C: Amazon konfigurációs |} Microsoft Azure"
    description="Előfizetési és a bejelentkezési biztosítása az Amazon fiókokat az Azure Active Directory B2C által biztosított alkalmazások vonzóbbak lehetnek."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Előfizetési és a bejelentkezési megadására Amazon fiókkal rendelkező felhasználóknak

## <a name="create-an-amazon-application"></a>Amazon alkalmazás létrehozása

Amazon az Azure Active Directory (Azure Active Directory) B2C egy identitásszolgáltató használatához szükséges Amazon alkalmazás létrehozása, és adja meg azt a megfelelő paraméterrel. Ehhez az Amazon fiókra van szüksége. Ha nincs telepítve egyik, a [http://www.amazon.com/](http://www.amazon.com/)kattint.

1. Nyissa meg az [Amazon Developer Center](https://login.amazon.com/) , és jelentkezzen be az Amazon fiók hitelesítő adatait.
2. Ha még nem tette meg, kattintson a **Feliratkozás**, regisztrációs kövesse a Fejlesztőeszközök és fogadja el a házirend.
3. Kattintson az **Új alkalmazás regisztrálása**.

    ![Regisztrálás az Amazon webhelyén az új alkalmazás](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Alkalmazás adatokat (**név**, **Leírás**és **Adatvédelmi közlemény URL-CÍMÉT**), és kattintson a **Mentés**gombra.

    ![Az új alkalmazást az Amazon regisztrálásakor alkalmazás adatainak a megadásával](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. A **Webhely beállításai** szakaszban másolja a vágólapra az **Ügyfél-azonosító** , és az **Ügyfél titkos**értéket. (Meg kell **Titkos megjelenítése** gombra kattintva jelenik meg.) Mindkét Amazon konfigurálja a bérlői webhelyemen egy identitásszolgáltató használatához szükséges. Kattintson a **szerkesztése** elemre a szakasz alján. **Ügyfél titkos kulcs** egy fontos biztonsági hitelesítő adatait.

    ![Ügyfél-azonosító, és az ügyfél titkos kezeléséről az Amazon új alkalmazáshoz](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Adja meg `https://login.microsoftonline.com` **Engedélyezni a JavaScript forrásokból** mezőjében és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a **Visszatérési URL-címek engedélyezett** mezőben. **{Bérlői}** cserélje le a bérlő nevére (például contoso.onmicrosoft.com). Kattintson a **Mentés**gombra. A **{bérlői}** mező értéke a kis-és nagybetűk.

    ![Az új alkalmazás Amazon visszatérési URL- és JavaScript forrásokból kezeléséről](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Állítsa be Amazon egy identitásszolgáltató a bérlői webhelyén

1. Kövesse ezeket a lépéseket [Nyissa meg azt a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) az Azure portálon.
2. Kattintson a B2C szolgáltatások lap **Identitásszolgáltatók**.
3. Válassza a **+ Hozzáadás** a lap tetején.
4. Az identitás-szolgáltató konfigurálása könnyen megjegyezhető **nevet** a szükséges. Írja be például a "Amzn".
5. Kattintson az **identitás-szolgáltató típusa**, jelölje be az **Amazon**, és kattintson az **OK gombra**.
6. Kattintson **a identitásszolgáltató beállítása** , és írja be az ügyfél-azonosító, és az ügyfél titkos az Amazon alkalmazás, amely a korábban létrehozott.
7. Kattintson az **OK gombra** , és kattintson a **Létrehozás** az Amazon konfigurációjának mentése gombra.
