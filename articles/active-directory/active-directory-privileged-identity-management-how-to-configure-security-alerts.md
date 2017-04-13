<properties
   pageTitle="Biztonsági figyelmeztetések beállítása |} Microsoft Azure"
   description="Biztonsági figyelmeztetések Azure Yammerhez Identitáskezelés bővítmény konfigurálásának ismertetése."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Biztonsági figyelmeztetések konfigurálása a Azure Active Directory Yammerhez Identitáskezelés

## <a name="security-alerts"></a>Biztonsági figyelmeztetések
Azure Yammerhez identitás kezelése (személyes Információkezelő) riasztások hoz létre, ha van, akkor a gyanús vagy a potenciális veszélyt jelentő tevékenységet a környezetben. Ha figyelmeztetés jelenik meg, akkor jelenik meg a személyes Információkezelő irányítópulton. Jelölje ki a riasztást, amely felsorolja a felhasználók és szerepkörök, a figyelmeztetést kiváltó jelentés látható.

![Személyes Információkezelő irányítópult biztonsági figyelmeztetések - képernyőképe][1]



| A felhasználó | Eseményindító | Javaslat |
| ----- | ------- | -------------- |
| **Személyes Információkezelő kívüli hozzárendeli szerepkörök** | A rendszergazda véglegesen egy szerepkört, a személyes Információkezelő felületén kívül van beállítva. | Tekintse át az új szerepkör-hozzárendelés. Mivel a más szolgáltatásokhoz csak rendelhet hozzá állandó rendszergazdák, azt egy arra alkalmas hozzárendeléshez szükség esetén módosítsa. |
| **Vannak túl gyakran aktivált szerepkörök** | Hiba történt a azonos szerepkör túl sok újraaktiválás beállításainak időn belül. | Lépjen kapcsolatba a felhasználót, hogy miért azok aktiválta a szerepkör sok idő látható. A határidő esetleg túl rövid a feladatuk elvégzéséhez őket, illetve esetleg használ parancsfájlok automatikus aktiválására szerepkörbe. |
| **Szerepkörök többtényezős hitelesítés nincs szükség az aktiválás** | A beállításainál engedélyezve MFA nélkül szerepkör létezik. | Azt MFA megkövetelése a legtöbb kiemelt jogosultságokkal rendelkező szerepkörök, de nagyon ösztönzése MFA engedélyezése aktiválási szerepkörök. |
| **A rendszergazdák nem használ a Yammerhez szerepkörök** | Vannak olyan, amely még nem aktiválta a szerepkörök nemrégiben jogosult rendszergazdák. | Indítsa el az access Véleményezés határozza meg, amelyekre már nincs szüksége az access a felhasználóknak. |
| **Túl sok globális rendszergazdái** | Nincsenek további globális rendszergazdái, mint az ajánlott. | Ha nagyszámú globális rendszergazdái, akkor valószínű, hogy a felhasználók első vannak további engedélyeket, mint a nekik szükséges jogosultságokkal. Felhasználók áthelyezése a kisebb Yammerhez szerepkörök, vagy ellenőrizze egy részüket használatára jogosult a szerepkört, hanem véglegesen rendelt. |

## <a name="configure-security-alert-settings"></a>Értesítési beállítások konfigurálása

Testre szabhatja a biztonsági figyelmeztetések a személyes Információkezelő a környezet és a biztonsági céloknak részét. Kövesse az alábbi lépéseket a beállítások lap eléréséhez:

1. Jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és válassza az **Azure Active Directory Yammerhez Identitáskezelés** csempét az irányítópult.
2. Jelölje ki a **kiemelt szerepkörök felügyelt** > **Beállítások** > **értesítési beállítások**.

    ![Keresse meg a biztonsági figyelmeztetések beállításai][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Szerepkörök vannak aktivált túl gyakran" értesítés

Ez a figyelmeztetés eseményindítók, ha egy felhasználó az azonos jogosultsággal rendelkező szerepkör aktiválja a megadott időszakon belül többször. Beállíthatja az időtartamot, mind az aktiválás számát.

- **Az aktiválás megújítás időkeret**: nap, óra, perc adja meg, majd a második az nyomon követhető a gyanús megújítását kívánt időszakra.

- **Az aktiválás megújításának száma**: Adja meg, hány aktiválásra 2 100-ig, worthy riasztás, a kiválasztott időkeret belül figyelembe venni. Módosíthatja a beállítást a csúszka, vagy írja be egy számot a szövegmezőbe.


### <a name="there-are-too-many-global-administrators-alert"></a>"Vannak túl sok a globális rendszergazdák" értesítés

Személyes Információkezelő eseményindítók Ez az értesítés, ha két, különböző feltételek teljesülése, és beállíthatja a őket. Első lépésként olyasvalakivel kell a globális rendszergazdák egy bizonyos küszöbértéket. Második a teljes szerepkör-hozzárendelés bizonyos százalékát globális rendszergazdák kell lennie. Ha csak teljesíti a mérések közül, nem jelenik meg az értesítésre.  

- **Minimális számú globális rendszergazda**: Adja meg a globális rendszergazdák, a 2 100-ig, akkor érdemes megfontolni a nem biztonságos összeg számát.

- **Százalékos aránya a globális rendszergazdák**: rendszergazdáknak, akik globális rendszergazdái százalékának megadása, a 0 %, 100 %-ig, azaz nem biztonságos a környezetben.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"A rendszergazdák nem használ a Yammerhez szerepkörök" értesítés

Ez a figyelmeztetés eseményindítók, ha egy felhasználó egy bizonyos idő mennyiségét szerepkörbe aktiválás nélkül.

- **A napok száma**: Adja meg, hány nap, 0 és 100, válassza a felhasználó szerepkörbe aktiválás nélkül.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
