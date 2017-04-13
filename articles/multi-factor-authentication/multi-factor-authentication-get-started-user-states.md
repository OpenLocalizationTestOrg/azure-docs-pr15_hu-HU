<properties 
    pageTitle="Microsoft Azure többtényezős hitelesítést felhasználói államok"
    description="Tudjon meg többet az Azure MFA felhasználói államok."
    services="multi-factor-authentication"
    documentationCenter=""
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

# <a name="user-states-in-azure-multi-factor-authentication"></a>Felhasználói államok Azure többtényezős hitelesítés

Azure többtényezős hitelesítés felhasználói fiókok a következő három különálló állapotok van:

Állam | Leírás |Érintett nem tallózó alkalmazások| Jegyzetek
:-------------: | :-------------: |:-------------: |:-------------: |
Letiltott | Többtényezős hitelesítés nem tehát új felhasználó alapértelmezett állapota.|nem|A felhasználó nem többtényezős hitelesítést használ.
Engedélyezve van |Többtényezős hitelesítés van már tehát a felhasználó.|nem.  A munkát a regisztrációs folyamat befejeztéig továbbra is.|A felhasználó engedélyezve van, de nem fejeződött be a regisztrációs folyamat. A következő bejelentkezés a folyamat befejezéséhez kéri.
Vannak érvényben|A felhasználó rendelkezik lett: ebben, és befejeződött a regisztrációs folyamat az többtényezős hitelesítés használatával.|igen.  Alkalmazások alkalmazás jelszó megadását kéri. | Előfordulhat, hogy a felhasználó, vagy előfordulhat, hogy nem befejeződött regisztrációs. Ha az azok a regisztrációs folyamat befejeződik, majd használnak többtényezős hitelesítés. Egyéb esetben a felhasználó felkéri a program a következő bejelentkezés a folyamat befejezéséhez.

## <a name="changing-a-user-state"></a>A felhasználói állapot megváltoztatása
A felhasználók állapota megváltozik, attól függően, hogy engedélyezi vagy tiltja a telepítő többtényezős már, és hogy a felhasználó a folyamat befejeződött.  Ha bekapcsolja MFA egy felhasználóhoz, a felhasználók állapot változik letiltva engedélyezve van.  Miután állapotú módosítását követően engedélyezve, a felhasználó bejelentkezik, és a folyamat befejeződik, állapotukban változik kényszerített.  

### <a name="to-view-a-users-state"></a>A felhasználó állapotának megtekintése
--------------------------------------------------------------------------------
1.  Jelentkezzen be rendszergazdaként az **Azure klasszikus portálon** .
2.  A bal oldalon kattintson **Az Active Directory**.
3.  Az **a címtár** kattintson a az engedélyezni kívánt felhasználó a címtárban.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  A képernyő tetején kattintson a **felhasználók**elemre.
5.  A lap alján kattintson a **Többtényezős hitelesítés kezelése**gombra.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Ekkor megnyílik egy új böngészőlapon.  A felhasználók állapotának megtekintése tudja.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Módosítsa az állapotot, a letiltott való engedélyezve
1.  Jelentkezzen be rendszergazdaként az **Azure klasszikus portálon** .
2.  A bal oldalon kattintson **Az Active Directory**.
3.  Az **a címtár** kattintson a az engedélyezni kívánt felhasználó a címtárban.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  A képernyő tetején kattintson a **felhasználók**elemre.
5.  A lap alján kattintson a **Többtényezős hitelesítés kezelése**gombra.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Ekkor megnyílik egy új böngészőlapon.  Keresse meg a felhasználót, hogy a többtényezős hitelesítést engedélyezni kívánt. Előfordulhat, hogy a nézet tetején módosítása. Győződjön meg arról, hogy az Állapot oszlop értéke **Letiltva.** 
 ![Engedélyezése a felhasználó](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Ellenőrizze,** jelölje be a nevük mellett.
7.  A jobb oldalon kattintson az **Engedélyezés**parancsra.
![Felhasználó engedélyezése](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kattintson az **Engedélyezés többtényezős hitelesítés**parancsra.
![Felhasználó engedélyezése](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Meg kell figyelje a felhasználói állapot megváltozott a **letiltott** **engedélyezve van**.
![Engedélyezése a felhasználóknak](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Miután engedélyezte a felhasználók, javasolt mailben értesítést.  Azt is tájékoztatnia kell őket hogyan használhatják azok nem böngésző alkalmazások alatt álló zárolt elkerülése érdekében.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Az állapot módosítása a engedélyezett/vannak érvényben letiltva
1.  Jelentkezzen be rendszergazdaként az **Azure klasszikus portálon** .
2.  A bal oldalon kattintson **Az Active Directory**.
3.  Az **a címtár** kattintson a az engedélyezni kívánt felhasználó a címtárban.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  A képernyő tetején kattintson a **felhasználók**elemre.
5.  A lap alján kattintson a **Többtényezős hitelesítés kezelése**gombra.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Ekkor megnyílik egy új böngészőlapon.  Keresse meg a felhasználót, hogy a letiltani kívánt. Előfordulhat, hogy a nézet tetején módosítása. Győződjön meg arról, hogy az állapot vagy **engedélyezve** vagy **vannak érvényben.**
7.  **Ellenőrizze,** jelölje be a nevük mellett.
7.  Kattintson a jobb oldalon a **letiltása**.
![Felhasználók letiltása](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Ennek megerősítésére kéri.  Kattintson az **Igen**gombra.
![Felhasználók letiltása](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Ezután látnia kell, hogy a sikeres volt.  Kattintson a **bezárása.** 
 ![Felhasználói letiltása](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
