<properties
   pageTitle="Problémamegoldás: "Az Active Directory" eleme hiányzó vagy nem érhető el |} Microsoft Azure "
   description="Mi a teendő, ha az adatkezelési portálon Azure Active Directory menüpont nem jelenik meg."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Problémamegoldás: "Az Active Directory" eleme hiányzó vagy nem érhető el

Az Azure Active Directory-funkciók és szolgáltatások használatára vonatkozó utasításokat számos kezdődő "Az Azure adatkezelési portált, és kattintson **az Active Directory**." De mi a teendő, ha az Active Directory melléket vagy menü elem nem jelenik meg, vagy ha **Nem érhető el**van megjelölve? Ez a témakör a projektcsapatok. Azt ismerteti, hogy a feltételeknek a melyik **Active Directory** nem jelenik meg, vagy nem érhető el, és bemutatja, hogyan folytatásához.

## <a name="active-directory-is-missing"></a>Az Active Directory nem található

Általában **Az Active Directory** elem jelenik meg a bal oldali navigációs menüt. Azure Active Directory eljárások utasításait feltételezik, hogy ez a cikk a nézetben.

![Képernyőkép: az Azure Active Directory](./media/active-directory-troubleshooting/typical-view.png)

Az Active Directory-elemet a bal oldali navigációs menüt a jelenik meg, amikor az alábbi feltételek bármelyike teljesül. Egyéb esetben az elem nem jelenik meg.

* Az aktuális felhasználó feliratkozva a Microsoft-fiókkal (korábbi nevén Windows Live ID azonosító).

    VAGY

* Az Azure bérlői könyvtár és az aktuális számla a címtár-rendszergazda.

    VAGY

* Az Azure bérlői legalább egy Azure Active Directory hozzáférés-vezérlés (ACS) névteret tartalmaz. További tudnivalókért lásd: az [Access vezérlő Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    VAGY

* Az Azure bérlői legalább egy Azure többtényezős hitelesítés szolgáltató tartalmaz. További tudnivalókért olvassa el a [Azure többtényezős hitelesítésszolgáltatók felügyelete](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)című témakört.

A hozzáférés-vezérlés névtér vagy többtényezős hitelesítés szolgáltató létrehozásához kattintson a **+ Új** > **Alkalmazás szolgáltatások** > **Az Active Directory**.

Úgy juthat az könyvtárában rendszergazdai jogosultságokra, rendelkezik rendszergazdai szerepkör hozzárendelése fiókját rendszergazdaként. A részletekért olvassa el a [rendszergazdai szerepkörök hozzárendelése](active-directory-assign-admin-roles.md)című témakört.

## <a name="active-directory-is-not-available"></a>Az Active Directory nem érhető el.

Amikor rákattint az **+ Új** > **Alkalmazás Services**, **Az Active Directory** elem jelenik meg. Kifejezetten az Active Directory-elem jelenik meg, ha bármelyik az Active Directory-szolgáltatásait, például a címtár, hozzáférés-vezérlés vagy többtényezős hitelesítés, az aktuális felhasználó érhető el.

Az oldal betöltése, miközben az elem halványan jelenik meg és azonban **Nem érhető el**van megjelölve. Ez egy ideiglenes állapot. Ha pár másodpercre várja, elérhetővé válik az elemet. A késleltetés meghosszabbítása van, ha gyakran frissítése az weblapon megoldja a problémát.

![Képernyőkép: az Active Directory nem érhető el.](./media/active-directory-troubleshooting/not-available.png)
