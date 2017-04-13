<properties
pageTitle="A bejelentkezési lapja az Azure Active Directory előnézetben testreszabása |} Microsoft Azure"
description="Megtudhatja, hogy miként vehet fel a vállalat arculati elemek az Azure bejelentkezési lapra"
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
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>A vállalat arculati elemek a bejelentkezési lapon az Azure Active Directory előnézetben hozzáadása

Elkerülhető, sok cégek alkalmazni szeretné a egységes megjelenés és működés a webhelyek és a szolgáltatások kezelése őket. Azure Active Directory-előnézet ezt a lehetőséget biztosít a azáltal, hogy a bejelentkezési lapja a cég emblémájának és egyéni színsémák az alábbi módokon testre. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) A bejelentkezési lapon, amely a identitásszolgáltató használata Azure Active Directory webes alkalmazást vagy az Office 365-ben való bejelentkezéskor megjelenő lap. Önnek reagálnia kell megadnia a hitelesítő adatait a lapon.

Kattintva jelenítse meg a vállalat arculatának, színek és egyéb testre szabható elemek ezen a lapon című témakörben olvashat az alábbi képek a különbség a két változat között.

Az alábbi kép mutatja és az Office 365 bejelentkezési lapján a egy asztali számítógép **előtt** a Testreszabás példa:

![Az Office 365 bejelentkezési lapján előtt testreszabása](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Az alábbi kép mutatja és az Office 365 bejelentkezési lapján a egy asztali számítógépén futó **után** a Testreszabás példa:

![Az Office 365 bejelentkezési lapján után testreszabása](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>A bejelentkezési lapja testreszabása

Általában ha böngészőalapú access a felhőben alkalmazások és szolgáltatások, amelyek a szervezet előfizetője van szüksége, használhatja a bejelentkezési lapja.

Ha telepítette a módosításokat a bejelentkezési lapon, igénybe vehet órává jelennek meg a módosítások.

Védjegyzett bejelentkezési lap csak akkor jelenik meg, ha egy szolgáltatásba, például https://outlook.com/**contoso**.com vagy https://mail bérlői-specifikus URL-alapú. **a Contoso**. com.

A szolgáltatás nem bérlői adott URL-címekkel megtekintésekor (pl.: https://mail.office365.com), a márka bejelentkezési lapja jelenik meg. Ebben az esetben a védjegyzés jelenik meg, miután megadta a felhasználói Azonosítójában, vagy kijelölte egy felhasználó mozaikokra bontás.

> [AZURE.NOTE]
>
- A tartománynév szerepelnie kell a Azure portál, amelyen beállította a **tartományok** részében "aktív" esetleges. További tudnivalókért lásd: az [egyéni tartománynév hozzáadása](active-directory-domains-add-azure-portal.md).
- Bejelentkezési lapja arculati elemek nem kerülnek át a fogyasztói bejelentkezési lapra, a Microsoft. Jelentkezzen be Microsoft-fiókkal, ha megjelenhet Azure Active Directory által nyújtott felhasználói csempék védjegyzett listáját, de a szervezet védjegyzés nem vonatkozik a Microsoft fiók bejelentkezési lapra.

A bejelentkezési lapján a **bejelentkezve szeretnék maradni** jelölőnégyzet lehetővé teszi, hogy egy felhasználó bejelentkezve maradjon, ha a Bezárás gombra, és nyissa meg újra a böngészőjükben. 

   ![Szeretnék jelentkezett be](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Munkamenet élettartam nincs hatással. A jelölőnégyzet a bejelentkezési lapon Azure Active Directory elrejtheti.
A jelölőnégyzet megjelenítését attól függ, hogy a **bejelentkezve szeretnék maradni letiltva**beállítást.

   ![Szeretnék jelentkezett be](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Ha el szeretné rejteni a jelölőnégyzet, ez a beállítás **Igen**értékre. 

> [AZURE.NOTE] Az Office 2010 és SharePoint Online egyes funkcióinak attól függenek, hogy a felhasználók nem ezt a jelölőnégyzetet. Ha úgy állítja be ezt a beállítást rejtett, a felhasználók további és váratlan képernyőn megjelenő utasításokat bejelentkezési látható.




**A vállalat arculati elemek a címtárhoz hozzáadása:**

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. A **felhasználók és csoportok** lap válassza a **vállalat arculati elemek**lehetőséget.

4. Kattintson a **felhasználók és csoportok - vállalat arculati elemek** lap, válassza a **Szerkesztés** parancsot.

    ![Egyéni arculati elemek szerkesztése](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Módosítsa a testre szabni kívánt elemeket. Minden elem nem kötelező.

6. Kattintson a **Mentés**gombra.

Azt kitölthetik órává az elvégzett módosításokat a megjelenő védjegyzés bejelentkezési lapra.

## <a name="next-steps"></a>Következő lépések

[Nyelvspecifikus vállalat arculati elemek hozzáadása](active-directory-branding-localize-azure-portal.md)
