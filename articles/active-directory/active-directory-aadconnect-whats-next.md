<properties
    pageTitle="Azure AD Connect: A következő lépések és az Azure AD Connect kezelése |} Microsoft Azure"
    description="További információ az alapértelmezett beállítások és műveleti feladatok kiterjesztése az Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Következő lépések és Azure AD Connect kezelése
Az alábbi speciális műveleti témakörök, amelyek lehetővé teszik az Azure Active Directory csatlakoztatása a szervezet igényeinek, és igényeinek megfelelő testreszabásához.  

## <a name="add-additional-sync-administrators"></a>Szinkronizálási további rendszergazdák hozzáadása
Alapértelmezés szerint csak a telepítés és a helyi rendszergazdák történt felhasználó fogja tudni a telepített szinkronizálási motor kezelése. További személyek is elérhessék és kezelheti a szinkronizálási motor keresse meg a csoportot, ADSyncAdmins nevű helyi kiszolgálói, és vegye fel őt a csoportba.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Licencek hozzárendelése az Azure Active Directory prémium verzió és a vállalati mobilitás felhasználókhoz

Most, hogy a felhasználók a felhőbe lett szinkronizálva, meg kell osztani egy licencet, például az Office 365 felhő alkalmazással használatbavétele.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Az Azure Active Directory prémium vagy vállalati mobilitás Suite licenc hozzárendelése
--------------------------------------------------------------------------------
1. Bejelentkezés az Azure-portálra rendszergazdaként.
2. A bal oldalon válassza **Az Active Directory**.
3. Az Active Directory lapon kattintson duplán a címtárban, amelynek a felhasználóknak engedélyezni kívánt.
4. A címtár-lap tetején válassza a **licencek**lehetőséget.
5. A licencek lapon jelölje ki az Active Directory prémium vagy vállalati mobilitás csomagot, és kattintson a **hozzárendelni**.
6. A párbeszédpanelen jelölje ki a felhasználókat szeretne licenceket rendelnie, és kattintson a pipa ikonra a módosítások mentéséhez.


## <a name="verifying-the-scheduled-synchronization-task"></a>Az ütemezett szinkronizálás tevékenység ellenőrzése
Ha azt szeretné, kattintson a szinkronizálás állapotának ellenőrzése, ezt megteheti, jelölje be az Azure-portálon is.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Az ütemezett szinkronizálás tevékenység ellenőrzése
--------------------------------------------------------------------------------
1. Bejelentkezés az Azure-portálra rendszergazdaként.
2. A bal oldalon válassza **Az Active Directory**.
3. Az Active Directory lapon kattintson duplán a címtárban, amelynek a felhasználóknak engedélyezni kívánt.
4. A címtár-lap tetején válassza a **Címtár-integráció**.
5. A-integráció a helyi active directory Megjegyzés: az utolsó szinkronizálás.

<center>![Felhőalapú](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Ütemezett szinkronizálás feladat elindítása
Ha módosítani szeretné a szinkronizálás feladat futtatása, ehhez az Azure AD Connect varázsló ismételt futtatásával.  Szüksége lesz az Azure Active Directory hitelesítő adatait.  A varázslóban válassza ki a **Testreszabás szinkronizálási beállításainak** feladatot, és kattintson a Tovább gombra a varázsló lépéseit. Végén győződjön meg arról, hogy a **Indítsa el a szinkronizálást, amint az a kezdeti konfigurációs befejezése** jelölőnégyzet be van jelölve.

<center>![Felhőalapú](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

További információt az Azure AD Connect szinkronizálás: ütemezőt, lásd: az [Azure Active Directory csatlakozás ütemező](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Azure AD Connect elérhető további műveletek
A kezdeti telepítés után az Azure AD Connect mindig indíthat el a varázsló ismét az Azure AD Connect kezdő lapot vagy asztali parancsikon.  Megfigyelheti, hogy használni a varázsló elvégzése lehetőségeket is biztosít néhány új feladatok formájában.  

Az alábbi táblázat összefoglalja az alábbi műveleteket, és egy rövid leírást mindegyiket.

![Bekapcsolódás a szabály](./media/active-directory-aadconnect-whats-next/addtasks.png)


További tevékenységek | Leírás
------------- | ------------- |
A kijelölt forgatókönyv megtekintése  |Lehetővé teszi a jelenlegi Azure AD Connect megoldás megtekintéséhez.  Ide tartoznak a általános beállítások, a szinkronizált könyvtárak, a szinkronizálási beállítások, a stb.
A szinkronizálás beállításainak testreszabása | Megváltoztathatja az aktuális konfiguráció, például a további Active Directory-erdők hozzáadása a konfigurációs vagy engedélyezés szinkronizálási beállításokat, például a felhasználói, csoport, az eszköz vagy a jelszó írási-vissza.
Fejlesztői üzemmód engedélyezése |  Ez lehetővé teszi, hogy később szinkronizálja szakaszinformáció, de semmi nem exportálja az Azure Active Directory vagy az Active Directory.  Ez előtt meg szeretné tekinteni a szinkronizálás előfordulási helyük teszi lehetővé.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
