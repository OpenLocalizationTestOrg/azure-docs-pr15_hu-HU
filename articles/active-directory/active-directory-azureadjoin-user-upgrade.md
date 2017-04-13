<properties
    pageTitle="Kattintson a beállítások ikonra az Azure Active Directory Windows 10-es eszköz beállítása |} Microsoft Azure"
    description="Ebből a cikkből megtudhatja, hogyan felhasználók is csatlakozni tudnak az Azure Active Directory, a beállítások menüben."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Kattintson a beállítások ikonra az Azure Active Directory Windows 10-es eszköz beállítása
Ha már használja a Windows 7 vagy Windows 8-ban, és a számítógépén vagy eszközén Windows 10 frissült, akkor csatlakozhat Azure Active Directory (Azure Active Directory) a beállítások menüben.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Ha be szeretne kapcsolódni az Azure Active Directory, a beállítások menüben


1. Kattintson a **Start** menüben a **Beállítások** gomb.
2. A **Beállítások**csoportban jelölje be a **rendszer**->**kapcsolatos**->**Azure Active Directory csatlakozni**.
<center>
![Bekapcsolódás az Azure Active Directory, a beállítások menüben](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Az Azure Active Directory Bekapcsolódás üzenet ablakában a **Tovább** gombra.
<center>
![Csatlakozás Azure AD-üzenet ablakában](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Adja meg a bejelentkezési hitelesítő adatait. A bejelentkezés módja tartalmazni fogja a teljes hitelesítés szükséges lépéseket. Ha a szövetséges bérlő egy része, a rendszergazda szolgáltatja a szervezete által üzemeltetett összevonási felület.
<center>
![Adja meg a bejelentkezési hitelesítő adataimat](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Ha a szervezet Azure Active Directory való csatlakozás Azure többtényezős hitelesítés van konfigurálva, adja meg a második tényező a folytatás előtt.
6. **Ez az eszköz kell kezelni engedélyezése** képernyőn kattintson az **Elfogadás** gombra.
7. Ekkor megjelennek az üzenet "eszköze most már csatlakozott a szervezet az Azure Active Directory".


## <a name="additional-information"></a>További információk
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
* [Jelszavak – Microsoft Passport nélkül hitelesítő identitások](active-directory-azureadjoin-passport.md)
