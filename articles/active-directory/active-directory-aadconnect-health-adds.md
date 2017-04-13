
<properties
    pageTitle="Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások |} Microsoft Azure"
    description="Ez a az Azure Active Directory csatlakozás állapotjelző lap, amely bemutatja, hogyan lehet Active Directory tartományi szolgáltatások figyelése."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások
A következő dokumentáció az Active Directory tartományi szolgáltatások az Azure Active Directory csatlakozás rendszerállapot figyelése. Active Directory tartományi szolgáltatások verziók a következők: Windows Server 2008 R2, a Windows Server 2012-ben és a Windows Server 2012 R2.

AD FS az Azure Active Directory csatlakozás rendszerállapot figyelése a további tudnivalókért olvassa el a [Használatával Azure Active Directory csatlakozás állapota az AD FS](active-directory-aadconnect-health-adfs.md)című témakört. Emellett a Azure AD Connect (szinkronizálás) az Azure Active Directory csatlakozás állapot ellenőrzése című témakörből [Használatával Azure Active Directory csatlakozás állapota szinkronizálás](active-directory-aadconnect-health-sync.md).

![Azure AD Connect állapota az Active Directory tartományi szolgáltatások](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Azure Active Directory riasztásainak állapot csatlakoztatása az Active Directory tartományi szolgáltatások
A riasztások szakaszban belül az Active Directory tartományi szolgáltatások, Azure Active Directory csatlakozás állapot teszi lehetővé a tartomány vezérlők kapcsolatos aktív, és a megoldott riasztások listáját. Aktív vagy megoldott jelzést kijelölése további információt és megoldási lépéseinek, egy új lap nyílik meg, valamint a dokumentumaiban mutató hivatkozásokat. Riasztási típusonként beállíthatja, hogy egy vagy több példány, amelyek megfelelnek az egyes, hogy az adott értesítést által érintett tartomány vezérlők. Értesítés a lap alján kattintson duplán a szóban forgó tartományt vezérlő többet szeretne tudni az értesítési példányhoz egy további lap megnyitásához.

Belül ez a lap engedélyezése az értesítő e-mailek nyomon követése riasztásokkal parancsát, és a időtartomány módosítása a nézethez. Időtartomány kibontása lehetővé teszi az előzetes megszűnt-e értesítéseket szeretné látni.

![Azure AD Connect szinkronizálási hiba](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Tartomány vezérlők irányítópult
Az irányítópult a környezet fő működési mértékek és állapot állapotát a felügyelt tartomány vezérlők topológiai nézetének biztosít. Gyors azonosítását, előfordulhat, hogy további vizsgálat bármelyik tartomány vezérlők segít a bemutatott mérési módja miatt. Alapértelmezés szerint az oszlopok csak egy részhalmazát jelenik meg. A megjeleníthető oszlopok teljes készlete, kattintson duplán az oszlop parancsot is megkeresheti. Jelölje ki az oszlopot, amely Önt leginkább érdeklő, az Active Directory tartományi szolgáltatások környezet állapotának megtekintéséhez vigye az irányítópult egyetlen és könnyen bekapcsolja.

![Tartomány-vezérlők](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

A saját tartomány vagy a webhelyet, amely a környezet topológia megértéséhez hasznos tartomány vezérlők is csoportosítva. Végül ha duplán kattint a lap fejlécében, az irányítópulton maximalizálja használja a rendelkezésre álló képernyő birtok. Ez a nézetet akkor hasznos, ha több oszlopban jelennek meg.

## <a name="replication-status-dashboard"></a>A replikáció állapot irányítópult
Az irányítópult megjelenítése a replikáció állapotát és a megfigyelt tartomány vezérlők topológiát biztosít. A legutóbbi replikációs kísérlet állapotának található hibából hasznos dokumentációjában együtt szerepel. Kattintson duplán a tartományvezérlőnek a hibát, például az adatokkal egy új lap megnyitása: a hiba, javasolt a hibaelhárítási dokumentációt megoldási lépéseinek és hivatkozások részleteit.

![A replikáció állapota](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Figyelése
Ez a funkció grafikus trendek különböző teljesítmény teljesítménymutatókat, amely folyamatosan begyűjtési minden olyan felügyelt tartomány vezérlőt tartalmaz. Annak a tartományvezérlőnek teljesítmény egyszerűen hasonlítható a erdő más felügyelt tartomány vezérlők között. Ezenkívül megtekintheti különböző teljesítmény számláló egymás mellett, amelyeket akkor hasznos, ha a hibaelhárítás a környezetben.

![Figyelése](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Alapértelmezés szerint azt van megadva négy teljesítmény számláló; azonban is mások által a Szűrés gombra, és jelölje be vagy törölje minden kívánt teljesítmény számláló. Ezenkívül duplán kattinthat a teljesítmény számláló diagram egy új, az egyes vezérlők felügyelt tartomány adatpontok tartalmazó lap megnyitásához.

## <a name="related-links"></a>Kapcsolódó hivatkozások

* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect állapot ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect állapot műveletek](active-directory-aadconnect-health-operations.md)
* [Azure Active Directory segítségével kapcsolatba állapotjelző Active Directory összevonási szolgáltatások](active-directory-aadconnect-health-adfs.md)
* [Azure Active Directory csatlakozás állapota szinkronizálás használata](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect állapotjelző – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect állapotjelző korábbi verziók](active-directory-aadconnect-health-version-history.md)
