<properties
    pageTitle="Azure többtényezős hitelesítés jelentések"
    description="Ez ismerteti a Azure többtényezős hitelesítés funkcióval - jelentéseket."
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

# <a name="reports-in-azure-multi-factor-authentication"></a>Az Azure többtényezős hitelesítés jelentései

Azure többtényezős hitelesítés több jelentést, és a szervezet által használt biztosít. Ezeket a jelentéseket a többtényezős hitelesítés Kezelőportálja, amely előírja, hogy van-e az Azure MFA-szolgáltatóval, vagy egy Azure MFA, Azure Active Directory Premium vagy vállalati mobilitás csomagja licenc érhető el. Az alábbiakban felsoroljuk a rendelkezésre álló jelentéseket.

Jelentések a Azure kezelőportálja segítségével érheti el.

név| Leírás
:------------- | :------------- |
Használat | A használati jelentések adatainak megjelenítése teljes használatát, a felhasználó összesítő és a felhasználó adatai.
Kiszolgáló állapota|A jelentés megjeleníti a fiókkal társított többtényezős hitelesítést kiszolgálók állapotát.
Letiltott felhasználó|Ezeket a jelentéseket kérések engedélyezése és tiltása a felhasználók előzményeinek megjelenítése.
Megkerülésével felhasználó|Az előzmények kérelmek figyelmen kívül hagyása többtényezős hitelesítés, hogy a felhasználó telefonszám látható.
Csalás értesítés|A dátumtartomány során benyújtott csalás riasztások előzményeit látható.
Aszinkron|Listák jelentések feldolgozás és állapotuk várakozik. Amikor elkészült a jelentés letöltése, vagy a jelentés megtekintéséhez hivatkozás megadva.

## <a name="to-view-reports"></a>Jelentések megtekintése

1.  Jelentkezzen be http://azure.microsoft.com
2.  A bal oldalon válassza az Active Directory.
3.  Válasszon az alábbi lehetőségek közül:
    - **A beállítás 1**: kattintson a többtényezős hitelesítés szolgáltatók fülre. Válassza ki az MFA-szolgáltatót, és kattintson a kezelés gombra a képernyő alján.
    - **A beállítás 2**: jelölje be a címtárában, majd kattintson a beállítás fülre. Többtényezős hitelesítés csoportjában válassza a Manage szolgáltatás beállításai. A MFA-szolgáltatás beállításai lap alján kattintson az Ugrás a portál hivatkozásra.
4.  Az Azure többtényezős hitelesítést Kezelőportálja jelenít meg a nézet a bal oldali navigációs jelentés szakasz. További lehetőségek jelölje be a fent leírt jelentéseket.

<center>![Felhőalapú](./media/multi-factor-authentication-manage-reports/report.png)</center>


**További források**

* [A felhasználóknak](./end-user/multi-factor-authentication-end-user.md)
* [Azure többtényezős hitelesítés MSDN webhelyen](https://msdn.microsoft.com/library/azure/dn249471.aspx)
