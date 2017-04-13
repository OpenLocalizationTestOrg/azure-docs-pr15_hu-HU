<properties
    pageTitle="Új felhasználók felvétele az Azure Active Directory |} Microsoft Azure"
    description="Új felhasználók hozzáadása vagy módosítása a felhasználói adatok az Azure Active Directory ismerteti."
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Új felhasználók vagy a Microsoft-fiókkal rendelkező felhasználók felvétele az Azure Active Directory

Vehet fel felhasználókat a címtárában kitöltéséhez. Ez a cikk ismerteti, hogy új felhasználók felvétele a szervezet, és hogyan vehet fel felhasználókat, akiknek van Microsoft-fiók. Más könyvtárak az Azure Active Directory-felhasználók felvétele vagy felhasználók hozzáadása a partner cégek által készített kapcsolatos további tudnivalókért lásd: [felhasználók felvétele az egyéb könyvtárak vagy a partner cégek az Azure Active Directory](active-directory-create-users-external.md). Felvett felhasználók alapértelmezés szerint nem rendelkezik rendszergazdai jogosultságokkal, de a szerepköröket rendelhet hozzájuk bármikor.

## <a name="add-a-user"></a>Felhasználó hozzáadása

1. Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.
2. Jelölje ki az **Active Directory**, és válassza ki a szervezeti könyvtár nevét.
3. A **felhasználók** lapon jelölje ki, és ezután parancs az eszköztáron válassza ki a **Felhasználó hozzáadása**.
4. A **mondja el a felhasználóra vonatkozó** lapon a **felhasználó típusa**csoportban a következők közül választhat:

    - **Új felhasználók a szervezet** – új felhasználói fiók hozzáadása a címtárban.
    - **Meglévő Microsoft-fiókkal rendelkező felhasználó** – Microsoft fogyasztói meglévő fiók felvétele a címtárban (például Outlook-fiók)

5. Attól függően, hogy a **felhasználó típusa**csoportban adja meg a felhasználónév (az új felhasználó) vagy (a Microsoft-fiókkal rendelkező felhasználóként) e-mail címet.
6. A felhasználói **profil** lapján adja meg az első és utolsó nevét, egy könnyen megjegyezhető nevet és a **szerepkörök** listából felhasználói szerepkörön. Felhasználó és a rendszergazdai szerepkörök kapcsolatos további tudnivalókért olvassa el a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md)című témakört. Adja meg, hogy a felhasználó **Többtényezős hitelesítés engedélyezése** .
7. Az **ideiglenes jelszót első** lapján jelölje be **létrehozása**.

> [AZURE.IMPORTANT] Ha a szervezet egynél több tartományt is használ, kapcsolatos tudnivalók az alábbi szempontokat, amikor a felhasználói fiók felvétele:
>
> - Az ugyanaz az egyszerű felhasználóneve (UPN) a felhasználói fiókok tartományok **első** felvenni, például geoffgrisso@contoso.onmicrosoft.com, **követ** geoffgrisso@contoso.com.
> - **Ne** legyen geoffgrisso@contoso.com felvétele előtt geoffgrisso@contoso.onmicrosoft.com. Ebben a sorrendben, és lehet lehető visszavonásához.

## <a name="change-user-information"></a>Felhasználói adatok módosítása

Módosíthatja, hogy minden olyan felhasználó attribútum kivételével az objektum azonosítójával.

1. Nyissa meg a címtárban.
2. A **felhasználók** lapon jelölje ki, és válassza ki a módosítani kívánt felhasználó megjelenítendő nevét.
3. Végezze el a módosításokat, és kattintson a **Mentés**gombra.

A felhasználót, hogy módosítani szeretné a helyszíni Active Directoryból szinkronizálva van, ha nem módosítható a felhasználói adatok, ezzel az eljárással. A felhasználó módosításához használja a helyszíni Active Directory-kezelő eszközök.

## <a name="guest-user-management-and-limitations"></a>A vendégként való bekapcsolódáshoz felhasználókezelés és korlátai

Vendégfiókokat más könyvtárak, akik SharePoint dokumentumokat, alkalmazások és más erőforrások: Azure eléréséhez, a címtárában meghívták felhasználóit. A vendégként való bekapcsolódáshoz címtárában fióknak az alapul szolgáló UserType attribútum meg a "Vendég." Normál (konkrétan a címtárában tagjai) felhasználója a UserType attribútum "Tag."

Vendégek beállított egy korlátozott jogok a címtárban. Mindezen jogok információ a többi felhasználó a címtárban felderítésére vendégek korlátozza. Jó helyen jár, vendégként való bekapcsolódáshoz felhasználók továbbra is használhatják a felhasználók és csoportok, amelyen az illető dolgozik az erőforrásokhoz rendelt. A vendégként való bekapcsolódáshoz felhasználók végezhetik el:

- Lásd: más felhasználók és csoportok, amelyekhez hozzá van rendelve egy Azure-előfizetéséhez társított
- Lásd: csoportok, amelyekhez tartoznak tagjainak
- Más felhasználó a címtárban, ha azokat meg, hogy a teljes e-mail címét, a felhasználó keresése
- Csak azok keresése – megjelenítendő nevét, e-mail címét, egyszerű felhasználónév (UDN) és miniatűrt korlátozott felhasználók attribútumok korlátozott beállítási
- A címtár igazolt tartományt listájának
- Beleegyezés az alkalmazáshoz, hozzáférést őket a ugyanazt a tagok között a címtár

## <a name="set-guest-user-access-policies"></a>A vendégként való bekapcsolódáshoz felhasználói hozzáférés házirendek beállítása:

A **beállítás** lapon könyvtár Vendég-felhasználóknak a hozzáférés vezérléséhez beállításokat tartalmazza. A beállításokkal címtár globális rendszergazdának csak az Azure klasszikus portal segítségével módosíthatók. Jelenleg később semmilyen módszerrel PowerShell vagy API-val.

A **beállítás** lapon az Azure klasszikus Portal megnyitásához válassza ki **Az Active Directory**, és válassza a mappa nevét.

![Azure Active Directory lap beállítása][1]

Kattintson a beállítások Vendég-felhasználóknak a hozzáférés vezérléséhez szerkesztheti.

![a vendégként való bekapcsolódáshoz felhasználók Access lehetőségek][2]


## <a name="whats-next"></a>Következő lépések

- [Felhasználók hozzáadása más könyvtárak vagy a partner cégek az Azure Active Directory szolgáltatásból](active-directory-create-users-external.md)
- [Azure Active Directory felügyelete](active-directory-administer.md)
- [Az Azure Active Directory jelszavak kezelése](active-directory-manage-passwords.md)
- [Az Azure Active Directory csoportok kezelése](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
