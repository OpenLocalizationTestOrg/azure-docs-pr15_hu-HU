<properties 
    pageTitle="Azure többtényezős hitelesítés jelentések"
    description="Ez ismerteti, hogyan módosíthatja a felhasználói beállításokat, például a felhasználók számára, végezze el ismét a vásárlási telefonos folyamat kényszerítése."
    documentationCenter=""
    services="multi-factor-authentication"
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Az Azure többtényezős hitelesítés a felhőben felhasználói beállításainak kezelése

Rendszergazdaként kezelheti a következő felhasználó és eszköz-beállításokat.  

- [A kijelölt felhasználóknak ismét megadására kapcsolatfelvételi mód](#require-selected-users-to-provide-contact-methods-again)
- [A meglévő jelszavak alkalmazás felhasználó törlése](#delete-users-existing-app-passwords)
- [MFA visszaállítása a felhasználó összes felfüggesztett eszközökön](#restore-mfa-on-all-suspended-devices-for-a-user)






Ez akkor hasznos, ha a számítógépen vagy eszközön elveszett vagy ellopott, vagy el kell távolítania egy felhasználó hozzáférést.


## <a name="require-selected-users-to-provide-contact-methods-again"></a>A kijelölt felhasználóknak ismét megadására kapcsolatfelvételi mód

Ez a beállítás lesz a regisztrációs folyamat befejezéséhez újra esetén az illető kikkel bejelentkezik a felhasználó kezd. Ne feledje, hogy nem böngésző alkalmazások továbbra is működnek, ha a felhasználónak alkalmazás jelszavak azokat.  A felhasználók alkalmazás jelszavak is **törli a kijelölt felhasználók által létrehozott összes meglévő alkalmazás jelszavak**kattintva törölheti.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Hogyan kell kötelezővé tétele a felhasználóknál adja meg ismét kapcsolatfelvételi mód




1. Bejelentkezés az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az a címtár kattintson a a szükséges, adja meg ismét a kapcsolatfelvételi mód kívánt felhasználó a címtárban.
4. A képernyő tetején kattintson a felhasználók elemre.
5. A lap alján kattintson a többtényezős hitelesítési kezelése Ekkor megnyílik a többtényezős hitelesítés lapot.
6. Keresse meg a felhasználót, hogy ki szeretne kezelni, és jelölje be a jelölőnégyzetet a neve mellett található. Előfordulhat, hogy a nézet tetején módosítása.
7. Ez a **kezelés felhasználói beállítások** hivatkozásra a jobb oldalon vele. Kattintson a gombra.
8. Jelölje be a **kijelölt felhasználóknak ismét kapcsolatfelvételi mód számára**.
![Adja meg a kapcsolatfelvételi mód](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
10. Kattintson a Mentés gombra.
11. Kattintson a Bezárás gombra

## <a name="delete-users-existing-app-passwords"></a>A meglévő jelszavak alkalmazás felhasználó törlése

Törli az összes a felhasználó által létrehozott alkalmazás jelszavakat. Az ilyen alkalmazás jelszavak társított nem tallózó alkalmazások megszűnik a munkát, amíg az alkalmazás új jelszó létrehozásakor.

### <a name="how-to-delete-users-existing-app-passwords"></a>Felhasználói jelszavak alkalmazás meglévő törlése

1. Bejelentkezés az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Directory csoportban kattintson az alkalmazás jelszavának törölni kívánt felhasználó a címtárban.
4. A képernyő tetején kattintson a felhasználók elemre.
5. A lap alján kattintson a többtényezős hitelesítési kezelése Ekkor megnyílik a többtényezős hitelesítés lapot.
6. Keresse meg a felhasználót, hogy ki szeretne kezelni, és jelölje be a jelölőnégyzetet a neve mellett található. Előfordulhat, hogy a nézet tetején módosítása.
7. Ez a **kezelés felhasználói beállítások** hivatkozásra a jobb oldalon vele. Kattintson a gombra.
8. Jelölje be **a kijelölt felhasználók által létrehozott összes meglévő alkalmazás jelszavak törlése**.
![Alkalmazás jelszavak törlése](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
10. Kattintson a Mentés gombra.
10. Kattintson a Bezárás gombra.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>MFA visszaállítása a felhasználó összes megjegyzett eszközökön

A rendszergazdák az azt jelenti, hogy a felhasználók eszközök és böngészők többtényezős hitelesítés visszaállítása rendelkeznek. Ha így tesz, ez a művelet eltávolítja az Emlékezzen MFA minden a felhasználó eszközök és böngészők és a felhasználónak kötelező MFA használatával jelentkezzen be a következő alkalommal.

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>MFA visszaállítása a felhasználó összes felfüggesztett eszközökön

1. Bejelentkezés az Azure klasszikus portálra.
2. A bal oldalon kattintson az Active Directory.
3. Az a címtár kattintson a kívánt mfa visszaállítása a felhasználó a címtárban.
4. A képernyő tetején kattintson a felhasználók elemre.
5. A lap alján kattintson a többtényezős hitelesítési kezelése Ekkor megnyílik a többtényezős hitelesítés lapot.
6. Keresse meg a felhasználót, hogy ki szeretne kezelni, és jelölje be a jelölőnégyzetet a neve mellett található. Előfordulhat, hogy a nézet tetején módosítása.
7. Ez a **kezelés felhasználói beállítások** hivatkozásra a jobb oldalon vele. Kattintson a gombra.
8. Jelölje be **az összes megjegyzett eszközön többtényezős hitelesítés visszaállítása**
![alkalmazás jelszavak törlése](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Kattintson a Mentés gombra.
10. Kattintson a Bezárás gombra.
