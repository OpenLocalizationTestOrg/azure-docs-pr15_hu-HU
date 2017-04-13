<properties
    pageTitle="Azure AD Connect szinkronizálása: Azure AD a fiók kezelése |} Microsoft Azure"
    description="Ez a témakör az Azure Active Directory szolgáltatásfiók visszaállítása dokumentumok."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, az Azure AD Connect szinkronizálás Connector-szolgáltatási fiók jelszavának alaphelyzetbe állítása"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect szinkronizálása: Azure AD a fiók kezelése
Ingyenes szolgáltatás az Azure Active Directory-összekötő által használt szolgáltatás fiókot kell használni. Ha a hitelesítő adatok alaphelyzetbe kell majd ez a témakör szól. A globális rendszergazda rendelkezik a hibát, ha például a PowerShell használatá szolgáltatás fiók jelszavának alaphelyzetbe.

## <a name="reset-the-credentials"></a>A hitelesítő adatok alaphelyzetbe állítása
Ha a definiált az Azure Active Directory-összekötő szolgáltatásfiók Azure Active Directory authentication problémák miatt nem tud kapcsolódni, a jelszó alaphelyzetbe.

1. Jelentkezzen be az Azure AD Connect szinkronizálási kiszolgáló, és indítsa el a Powershellt.
2. Futtatása `Add-ADSyncAADServiceAccount`.  
![A PowerShell-parancsmag addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Adja meg az Azure Active Directory globális rendszergazdai hitelesítő adataival.

Ezzel a parancsmaggal a szolgáltatási fiók jelszavának alaphelyzetbe állítása és frissítése az Azure Active Directory és a szinkronizálási motor.

## <a name="known-issues-these-steps-can-solve"></a>Ezeket a lépéseket orvosolható ismert problémái
Ez a szakasz felsoroljuk a hitelesítő adatok alaphelyzetbe állítása az Azure Active Directory-szolgáltatás ügyfél által javított felhasználók által jelzett hibát.

-----------
Esemény 6900  
A kiszolgáló váratlan hiba történt a jelszó módosítása értesítést feldolgozása:  
AADSTS70002: Hiba érvényesítése hitelesítő adatait. AADSTS50054: Régi jelszavát hitelesítéshez használt.

----------
Esemény 659  
Házirend-szinkronizálás jelszó konfiguráció beolvasása közben hibát. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Hiba érvényesítése hitelesítő adatait. AADSTS50054: Régi jelszavát hitelesítéshez használt.

## <a name="next-steps"></a>Következő lépések

**Témakörök – áttekintés**

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
