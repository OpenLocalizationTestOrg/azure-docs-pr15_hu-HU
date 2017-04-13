<properties
pageTitle="Nyelvspecifikus vállalat arculati elemek a bejelentkezési lapon az Azure Active Directory előnézetben hozzáadása |} Microsoft Azure"
description="Megtudhatja, hogy miként adja hozzá az adott nyelv vállalata védjegyzés, képek és szöveg Azure bejelentkezési lapjára"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Nyelvspecifikus vállalat arculati elemek a bejelentkezési lapon az Azure Active Directory előnézetben hozzáadása

Elkerülhető, sok cégek alkalmazni szeretné a egységes megjelenés és működés a webhelyek és a szolgáltatások kezelése őket. Azure Active Directory-előnézet ezt a lehetőséget biztosít a azáltal, hogy a bejelentkezési lapja a cég emblémájának és egyéni színsémák az alábbi módokon testre. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) A bejelentkezési lapon, amely a identitásszolgáltató használata Azure Active Directory webes alkalmazást vagy az Office 365-ben való bejelentkezéskor megjelenő lap. Önnek reagálnia kell megadnia a hitelesítő adatait a lapon.

## <a name="customizing-the-sign-in-page-for-another-language"></a>A bejelentkezési lapjának egy másik nyelven testreszabása

Nyelvspecifikus elemek felveheti a egyéni bejelentkezési lapja csak akkor, ha már létrehozott egy egyéni bejelentkezési lapján, a [vállalat arculati elemek a bejelentkezési lapon adja hozzá](active-directory-branding-custom-signon-azure-portal.md). Egy bejelentkezési lapra egy oldal címtár is beállíthatja az alapértelmezés szerinti testre szabható elemek. Miután beállította az oldal elemei alapértelmezés szerint, a másik nyelvhez több verziója is beállíthatja. Is keverjen ki és különböző elemek megfelelően. Ha például teheti:

- Hozzon létre egy alapértelmezett **bejelentkezési lap képe** , amely működik az összes már, akkor az egyes verzióiban: angol, francia, és hozzon létre. A böngészőben az alábbi két nyelven egyik beállításakor a nyelvspecifikus látható, miközben az alapértelmezett ábra minden más nyelven jelenik meg.

- Állítsa be a szervezet számára (például a japán vagy héber verzió) különböző emblémák.

Azt javasoljuk, hogy őrizze meg a nyelvi változatok számát alacsony, karbantartás és teljesítmény okok miatt.

**A vállalat arculati elemek a címtárhoz hozzáadása:**

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. A **felhasználók és csoportok** lap válassza a **vállalat arculati elemek**lehetőséget.

4. Kattintson a **felhasználók és csoportok - vállalat arculati elemek** lap, jelölje ki a **nyelv hozzáadása** parancsot.

    ![Nyelvspecifikus esetleges elemek felvétele](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Módosítsa a testre szabni kívánt elemeket. Minden elem nem kötelező.

6. Kattintson a **Mentés**gombra.

Azt kitölthetik órává az elvégzett módosításokat a megjelenő védjegyzés bejelentkezési lapra.

## <a name="next-steps"></a>Következő lépések

[A vállalat arculati elemek és a bejelentkezési lap hozzáadása](active-directory-branding-custom-signon-azure-portal.md)
