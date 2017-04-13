<properties
    pageTitle="Azure Active Directory identitás védelem – letiltásának feloldása felhasználók |} Microsoft Azure"
    description="Megtudhatja, hogyan tiltásának feloldása a felhasználók által az Azure Active Directory identitás védelem házirend blokkolt."
    services="active-directory"
    keywords="Azure active directory-identitás védelmét, felhasználói tiltásának feloldása"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory identitás védelem – letiltásának feloldása a felhasználóknak

Azure Active Directory identitás védelmét beállíthatja a megadott feltételek teljesülése esetén a felhasználók letiltása házirendeket. Általában egy letiltott felhasználói partnerek segélyszolgálat blokkolatlan lesz. A témakörök hajthatja végre blokkolt felhasználó feloldása lépéseit ismerteti.


## <a name="determine-the-reason-for-blocking"></a>Az az oka blokkolása meghatározása

A felhasználó feloldása első lépésként meg kell határoznia a házirendet, amely a felhasználó blokkolta, mert a következő lépésekkel rajta attól függően, hogy milyen típusú. Azure Active Directory identitás védelemmel a felhasználó vagy blokkolhatja a bejelentkezési kockázat vagy egy felhasználói kockázat házirend. 

A házirendet, amely a felhasználónak bejelentkezés kísérlet során lett megjelenő párbeszédpanel címsorból felhasználó letiltotta típusú szerezheti be:

|Házirend | Felhasználói párbeszédpanel|
|--- | --- |
|Bejelentkezés a kockázat | ![Letiltott bejelentkezés](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Felhasználói kockázat | ![Letiltott fiók](./media/active-directory-identityprotection-unblock-howto/104.png) |


A felhasználó letiltotta:

- A bejelentkezési kockázat házirend néven is ismert gyanús bejelentkezés.
- A felhasználó kockázat házirend más néven kockázatára fiók

 
## <a name="unblocking-suspicious-sign-ins"></a>A gyanús feloldásáról bejelentkezési-bővítmények

A gyanús bejelentkezés zárolásának feloldásához, a következő lehetőségei vannak:

1. **Bejelentkezés a már jól ismert helyre vagy eszközről** – gyakori oka letiltott gyanús bejelentkezési bővítmények nem ismeretlen helyekre vagy eszközök a bejelentkezési kísérleteket. A felhasználók gyorsan megállapíthatja, hogy ez az oka blokkoló próbál a bejelentkezés egy már jól ismert helyre vagy eszközről.


3. **Kizárja a házirend** - Ha úgy gondolja, hogy az aktuális konfiguráció a bejelentkezési házirend problémákat okoz a megadott felhasználók számára, kizárhatja a felhasználók belőle. Keresse meg a [bejelentkezési kockázat házirend](active-directory-identityprotection.md#sign-in-risk-policy) további információt.
 
4. **Tiltsa le a házirend** - Ha úgy gondolja, hogy a házirend beállítása problémákat okoz a minden felhasználónak, letilthatja a házirend. Keresse meg a [bejelentkezési kockázati házirend](active-directory-identityprotection.md#sign-in-risk-policy) további információt.


## <a name="unblocking-accounts-at-risk"></a>A kockázat feloldásáról fiókok

Kockázatára fiók zárolásának feloldásához, a következő lehetőségei vannak:

1. A **jelszó alaphelyzetbe állítása** - átállíthatja a felhasználó jelszavát. Lásd: további információt a [kézi biztonságos jelszó alaphelyzetbe állítása](active-directory-identityprotection.md#manual-secure-password-reset) .

2. **Zárja be az összes kockázati esemény** – a felhasználói kockázat házirend blokkolja a felhasználó, ha beállított felhasználói kockázati szintjének elérésének letiltása jött létre. Csökkentheti a felhasználó kockázati esemény jelentett manuálisan bezárásával kockázati szintjét. További részletekért olvassa el [manuálisan a kockázat események bezárása](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Kizárja a házirend** - Ha úgy gondolja, hogy az aktuális konfiguráció a bejelentkezési házirend problémákat okoz a megadott felhasználók számára, kizárhatja a felhasználók belőle. Lásd: [felhasználó kockázat házirendjének](active-directory-identityprotection.md#user-risk-policy) további információt.
 
4. **Tiltsa le a házirend** - Ha úgy gondolja, hogy a házirend beállítása problémákat okoz a minden felhasználónak, letilthatja a házirend. Lásd: [felhasználó kockázat házirendjének](active-directory-identityprotection.md#user-risk-policy) további információt.




## <a name="next-steps"></a>Következő lépések

 Szeretne többet megtudni az Azure Active Directory azonosító adatok védelmét? Nézze meg az [Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md).
 

