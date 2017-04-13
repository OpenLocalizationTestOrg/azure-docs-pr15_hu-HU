<properties
    pageTitle="Azure MFA és AD FS |} Microsoft Azure"
    description="Ez a leírja, hogy miként veheti használatba Azure MFA és AD FS Azure többtényezős hitelesítést weblapot."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Az Azure többtényezős hitelesítés és az Active Directory összevonási szolgáltatások használatába



<center>![Felhőalapú](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Ha szervezete rendelkezik összevont a helyszíni Active Directory és az Azure Active Directory az Active Directory összevonási szolgáltatásokat használó, két lehetőség az Azure többtényezős hitelesítés használatával.

- Biztonságos, felhőalapú erőforrások Azure többtényezős hitelesítés vagy az Active Directory összevonási szolgáltatások használata
- Azure többtényezős hitelesítést kiszolgálót használ, a felhő és a helyszíni erőforrások biztosítása

Az alábbi táblázat összefoglalja a tartomány ellenőrzéséhez felület biztonságossá tétele Azure többtényezős hitelesítés és AD FS erőforrások között

|Ellenőrzés élmény – Tallózás-alapú alkalmazás|Ellenőrzés élmény - alkalmazások nem böngészős
:------------- | :------------- | :------------- |
Azure AD-erőforrások Azure többtényezős hitelesítés használatával biztonságossá tétele |<li>Az első ellenőrző lépésben történik a helyszíni Active Directory összevonási szolgáltatások használatával.</li> <li>A második lépés a felhőben hitelesítéssel elvégzett phone-alapú módszer.</li>|A végfelhasználók alkalmazás jelszavak használata való bejelentkezéshez.
Azure Active Directory erőforrásokat, használja az Active Directory összevonási szolgáltatások biztonságossá tétele |<li>Az első ellenőrző lépésben történik a helyszíni Active Directory összevonási szolgáltatások használatával.</li><li>A második lépésként elvégzett helyszíni vállalta az igény szerint.</li>|A végfelhasználók alkalmazás jelszavak használata való bejelentkezéshez.

A telepítés által okozott problémák alkalmazás jelszavakkal szövetséges felhasználók számára:

- Alkalmazás jelszavak hitelesítéssel felhőben, ezért ezeket figyelmen kívül hagyása összevonási ellenőrzése. Összevonás csak aktívan használatos-alkalmazás jelszó beállítását.
- A helyszíni hozzáférés-vezérlés ügyfél beállításai vannak nem fogadja el a jelszavak alkalmazást.
- Elvesznek a helyszíni alkalmazás jelszavak hitelesítési naplózás lehetőséget.
- Fiók letiltása és törlése órát is igénybe vehet legfeljebb három a címtár szinkronizálását, hogy késleltetné a felhőben identitást alkalmazás jelszavak letiltása vagy törlése.

## <a name="next-steps"></a>Következő lépések

Azure többtényezős hitelesítés vagy a Azure többtényezős hitelesítést kiszolgáló az AD FS beállításáról olvashat az alábbi cikkekben talál:

- [Biztonságos, felhőalapú erőforrások Azure többtényezős hitelesítés és az Active Directory összevonási szolgáltatások használata](multi-factor-authentication-get-started-adfs-cloud.md)
- [Azure többtényezős hitelesítés kiszolgáló használata esetén a Windows Server 2012 R2 AD FS biztonságos felhő és a helyszíni erőforrások](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Azure többtényezős hitelesítést kiszolgáló használata esetén az AD FS 2.0 felhő és a helyszíni erőforrások biztosítása](multi-factor-authentication-get-started-adfs-adfs2.md)
