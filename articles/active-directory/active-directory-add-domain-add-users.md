<properties
    pageTitle="Felhasználók hozzárendelése egy egyéni tartományt az Azure Active Directory |} Microsoft Azure"
    description="Hogyan lehet felhasználói fiókok és Azure Active Directory az egyéni tartomány feltöltéséhez."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Felhasználók hozzárendelése az egyéni tartomány

Azure Active Directory az egyéni tartomány hozzáadása után, hogy külön előkészületek nélkül használhatja őket hitelesítése hozzá kell adnia a tartományhoz a felhasználói fiókok.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>A vállalati hálózaton könyvtárból a szinkronizált felhasználók

Ha már meg van kapcsolatot között a helyszíni Active Directory és Azure Active Directory, a szinkronizálási kitöltheti a fiókok. Azure Active Directory szinkronizálni a helyszíni Active Directory hogyan további tudnivalókért olvassa el a [integrálása a helyszíni identitás és Azure Active Directory](active-directory-aadconnect.md)című témakört.

## <a name="users-added-and-managed-in-the-cloud"></a>Felhasználók hozzáadása és kezelése a felhőben

A tartomány meglévő felhasználói fiók módosítása:

1.  Nyissa meg az Azure klasszikus portált globális rendszergazdának vagy egy felhasználói rendszergazda rendelkező fiók használatával

2.  Nyissa meg a címtárban.

3.  Válassza a **felhasználók** lapot.

4.  Jelölje ki a felhasználót a listából.

5.  Módosítsa a tartomány, a felhasználó számára, és válassza a **Mentés**gombra.

Ezt is megteheti [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) alrendszerrel vagy a [Diagram API -val](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Jelöljön ki egy egyéni tartományt, amikor új felhasználót hoz létre

1.  Nyissa meg az Azure klasszikus portált globális rendszergazdának vagy egy felhasználói rendszergazda rendelkező fiók használatával

2.  Nyissa meg a címtárban.

3.  Válassza a **felhasználók** lapot.

4.  A parancssáv válassza a **Hozzáadás**elemre.

5.  Amikor hozzáadja a felhasználó nevét, válasszon az egyéni tartományt a tartományok listája.

6.  Válassza a **Mentés**.

## <a name="next-steps"></a>Következő lépések

-   [Egyéni tartomány nevekkel egyszerűsítése érdekében a bejelentkezés módja a felhasználók számára](active-directory-add-domain.md)

-   [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)

-   [Tudnivalók a Azure Active Directory domain management fogalmak](active-directory-add-domain-concepts.md)
