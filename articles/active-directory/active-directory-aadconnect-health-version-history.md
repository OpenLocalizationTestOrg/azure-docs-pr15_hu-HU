<properties
    pageTitle="Azure AD Connect állapotjelző korábbi verziók"
    description="Ez a témakör a kiadásairól az Azure Active Directory csatlakozás állapot, és mi rendszernek szerepel."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect állapot: Megjelenés korábbi verziók

Az Azure Active Directory csapata rendszeresen frissíti a Azure Active Directory csatlakozás állapotjelző új szolgáltatást és funkciót. Ez a cikk felsorolja a verziókat és a szolgáltatások vannak adva.

## <a name="october-2016"></a>Október 2016-ban
**Agent Update:**
- Azure Active Directory csatlakozás ügynök Active Directory összevonási szolgáltatások \(2.6.408.0 verziója\)
    1. Az ügyfél IP-címek észlelése a hitelesítési kérések fejlesztések
    2. Hibajavítás szóló jelzés kezelésével kapcsolatban
- Azure Active Directory csatlakozás ügynök az Active Directory tartományi szolgáltatások (2.6.408.0 verzió)
    1. A hibajavítás szóló jelzés kezelésével kapcsolatban.
- Azure Active Directory csatlakozás állapotjelző agent Azure AD Connect verzió 1.1.281.0 kiadott szinkronizálási (2.6.353.0 verzió)
    1. Adja meg a szükséges adatokat a szinkronizálási hiba kimutatásokhoz
    2. Hibajavítás szóló jelzés kezelésével kapcsolatban

**Új előzetes verzió szolgáltatások:**
- Azure AD-szinkronizálás hibajelentések csatlakoztatása

**Új funkciók:**
- Az AD FS - Azure Active Directory csatlakozás állapot IP-cím mező érhető el a jelentés felső 50 felhasználókról a hibás felhasználónevével és jelszavával.

## <a name="july-2016"></a>Július 2016-ban

**Új előzetes verzió szolgáltatások:**

- [Azure AD Connect az Active Directory tartományi szolgáltatások állapotát](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Január 2016-ban


**Agent Update:**

- Azure Active Directory csatlakozás ügynök Active Directory összevonási szolgáltatások (2.6.91.1512 verzió)


**Új funkciók:**

- [Teszt kapcsolódási eszköz az Azure Active Directory csatlakozás állapot ügynökök beállítása](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>A Skype 2015 november


**Új funkciók:**

- [Szerepköralapú hozzáférés-vezérlés](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) támogatása


**Új előzetes verzió szolgáltatások:**

- [Szinkronizálás az azure Active Directory csatlakozás állapota](active-directory-aadconnect-health-sync.md).

**Javított problémákat:**

- Hibajavítás a hibák ügynök regisztrációk alatt látható.

## <a name="september-2015"></a>A Skype 2015 szeptember

**Új funkciók:**

- Felhasználónév-jelszó jelentés rossz Active Directory összevonási szolgáltatások
- A támogatás hivatkozással nem hitelesített HTTP-Proxy beállítása
- A támogatás hivatkozással Server core ügynökök beállítása
- Értesítések Active Directory összevonási szolgáltatások fejlesztései
- Töltse fel a fejlesztések az Azure Active Directory csatlakozás egészségügyi Agent az AD FS-kapcsolatot, és az adatok.


**Javított problémákat:**

- Hibajavítás használatát háttérismeretek az Active Directory összevonási szolgáltatások hiba típusnál.


## <a name="june-2015"></a>Június 2015

**Azure Active Directory csatlakozás állapot kezdeti kiadását az Active Directory összevonási szolgáltatások és AD FS-Proxy.**

**Új funkciók:**

- Active Directory összevonási szolgáltatások és AD FS-Proxy kiszolgálók az értesítő e-mailek figyelésére küldendő értesítéseket.
- Active Directory összevonási szolgáltatások topológiája és a minták az Active Directory összevonási szolgáltatások teljesítmény számláló könnyen elérheti.
- Trend sikeres jogkivonat-összehívásokban csoportosítva vannak alkalmazások, hitelesítési módszereket stb kérése hálózati helyet AD FS-kiszolgálókon.
- Trendek hibás kérés AD FS-kiszolgálókon csoportosítva vannak alkalmazások, hiba típusok stb.
- Egyszerűbb ügynök telepítési Azure Active Directory globális rendszergazdai hitelesítő adataival.  




## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni Monitor identitás infrastruktúra és a szinkronizálás a felhőben szolgáltatások](active-directory-aadconnect-health.md).
