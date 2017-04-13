<properties
   pageTitle="Azure AD Connect: Fiókok és engedélyek |} Microsoft Azure"
   description="Ez a témakör ismerteti a fiókok használja, és a létrehozott és a szükséges engedélyekkel."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Fiókok és engedélyek
Az Azure AD Connect telepítővarázslóban két különböző elérési útját kínál:

- Express-beállításai a varázsló további jogosultság szükséges, úgy, hogy akkor is beállítása a konfigurációban, anélkül, hogy a felhasználók létrehozása és külön-külön engedélyek konfigurálása.

- Az egyéni beállítások a varázsló kínál további lehetőségek és lehetőségeket, de vannak olyan helyzetek, amelyben győződjön meg arról, hogy saját maga rendelkezik a megfelelő engedélyekkel kell.

## <a name="related-documentation"></a>Kapcsolódó dokumentáció
Ha nem olvassa el a dokumentációja [integrálása a helyszíni identitás és Azure Active Directory](../active-directory-aadconnect.md), a következő táblázat a kapcsolódó témakörökre mutató hivatkozásokat tartalmaz.

A témakör |  
--------- | ---------
Telepítse a Express beállításokkal | [Express telepítésével Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Telepítse a testre szabott beállítások használata | [Egyéni telepítési az Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
A DirSync frissítés | [Frissítés az Azure Active Directory-szinkronizáló (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Express-telepítési beállítások
Express-beállításai az telepítővarázslóban AD DS vállalati rendszergazdai hitelesítő adatokat kér, hogy a helyszíni Active Directory beállítható az Azure AD Connect szükséges engedélyekkel. A DirSync frissít, ha a DirSync által használt fiók jelszavának alaphelyzetbe állítása az Active Directory tartományi szolgáltatások vállalati rendszergazdák hitelesítő adatok szolgálnak. Azure Active Directory globális rendszergazdai hitelesítő adatok is szükséges.

Varázslónak az az oldala  | Összegyűjtött hitelesítő adatok | Szükséges engedélyek| Használható
------------- | ------------- |------------- |-------------
A #HIÁNYZIK|A telepítés varázslót felhasználói| A helyi kiszolgáló rendszergazdája| <li>A helyi fiókot, a [szinkronizálási motor szolgáltatásfiók](#azure-ad-connect-sync-service-account)használt hoz létre.
Azure Active Directory csatlakoztatása| Azure Active directory hitelesítő adatok | Globális rendszergazdai szerepkör az Azure Active Directory | <li>A az Azure Active directory Szinkronizáló engedélyezése.</li>  <li>[Azure AD-fiókot](#azure-ad-service-account) , amellyel a folyamatban lévő szinkronizálási műveletek az Azure Active Directory létrehozása.</li>
Active Directory tartományi Szolgáltatásokban összekapcsolása | A helyszíni Active Directory hitelesítő adatok | A vállalati rendszergazdák (EA) csoportba, az Active Directoryban| <li>Az Active Directory hoz létre egy [fiókot](#active-directory-account) , és engedélyt ad hozzá. Ez a fiók létrehozása használatával olvasási és írási directory adatait a szinkronizálás során.</li>

### <a name="enterprise-admin-credentials"></a>Vállalati rendszergazdai hitelesítő adatok
Ezek a hitelesítő adatok csak a telepítés során használt, és a telepítés befejezése után használhatók. Érdemes a vállalati rendszergazda, és nem a Domain Admin, győződjön meg arról, hogy az engedélyek az Active Directory beállítható, hogy minden tartományban.

### <a name="global-admin-credentials"></a>Globális rendszergazdai hitelesítő adatok
Ezek a hitelesítő adatok csak a telepítés során használt, és nem használják a telepítés befejezése után. A módosítások szinkronizálása az Azure Active Directory használt [Azure AD-fiók](#azure-ad-service-account) létrehozása szolgál. A fiók is lehetővé teszi, hogy szinkronizálási szolgáltatásként az Azure Active Directory.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>A létrehozott Active Directory tartományi szolgáltatások engedélyeinek számlája express beállításai
Az olvasásra és írásra Active Directory tartományi szolgáltatásokban létrehozott [fiók](#active-directory-account) van, a következő engedélyeket, amikor a létrehozott express beállítások:

Engedély | Használható
---- | ----
<li>A címtár-módosítások bizonyos</li><li>Párhuzamos címtár összes módosítása | Jelszó-szinkronizálás
Olvasási/írási felhasználó összes tulajdonság | Az importálás és az Exchange hibrid
Olvasási/írási összes tulajdonságok iNetOrgPerson | Az importálás és az Exchange hibrid
Olvasási/írási csoport minden tulajdonság | Az importálás és az Exchange hibrid
Olvasási/írási forduljon az összes tulajdonság | Az importálás és az Exchange hibrid
Jelszó alaphelyzetbe állítása | Felkészülés a jelszó visszaírást engedélyezése

## <a name="custom-settings-installation"></a>Egyéni beállítások telepítése
Ha az egyéni beállításokat használ, a csatlakozás Active Directoryhoz használt fiókot a telepítés előtt hozható létre. Az engedélyek ehhez a fiókhoz engedélyt kell adni [a Tartományi fiók létrehozása](#create-the-ad-ds-account)az találhatók.

Varázslónak az az oldala  | Összegyűjtött hitelesítő adatok | Szükséges engedélyek| Használható
------------- | ------------- |------------- |-------------
A #HIÁNYZIK | A telepítés varázslót felhasználói|<li>A helyi kiszolgáló rendszergazdája</li><li>Ha egy teljes SQL-kiszolgálót használ, a felhasználónak kell lennie a rendszer rendszergazdai (Nyelvű) SQL-</li>| Alapértelmezés szerint a helyi [szinkronizálási motor szolgáltatásfiók](#azure-ad-connect-sync-service-account)használt fiók létrehozása A fiók csak akkor jön létre, amikor a rendszergazda nem adja meg egy adott partner.
Szinkronizálási szolgáltatások, fiókbeállítás szolgáltatás telepítése | Active Directory vagy a helyi felhasználói fiók hitelesítő adatait. | Felhasználó engedélyekkel rendelkező által az telepítővarázslóban | Ha a rendszergazda adja meg a fiók, ehhez a fiókhoz szolgál a szolgáltatásfiók a szinkronizálási szolgáltatás.
Azure Active Directory csatlakoztatása | Azure Active directory hitelesítő adatok| Globális rendszergazdai szerepkör az Azure Active Directory| <li>A az Azure Active directory Szinkronizáló engedélyezése.</li>  <li>[Azure AD-fiókot](#azure-ad-service-account) , amellyel a folyamatban lévő szinkronizálási műveletek az Azure Active Directory létrehozása.</li>
A könyvtárak csatlakoztatása | Azure Active Directory csatlakoztatott erdőkön a helyszíni Active Directory hitelesítő adatok | Az engedélyek függ, milyen funkciókat engedélyezi és megtalálható a [fiók létrehozása az Active Directory tartományi szolgáltatások](#create-the-ad-ds-account) |Ehhez a fiókhoz olvasása és írása directory adatait a szinkronizálás során szolgál.
Active Directory összevonási szolgáltatások kiszolgálón | A lista minden kiszolgálóhoz a varázsló által gyűjtött hitelesítő adatok nem elegendő a csatlakozás a bejelentkezési adatok annak a felhasználónak a varázsló futtatása esetén | Tartomány-rendszergazda | Telepítése és konfigurálása az Active Directory összevonási szolgáltatások szerepkör.
Alkalmazás proxy webkiszolgálón |A lista minden kiszolgálóhoz a varázsló által gyűjtött hitelesítő adatok nem elegendő a csatlakozás a bejelentkezési adatok annak a felhasználónak a varázsló futtatása esetén | Helyi rendszergazdai a cél számítógépen | Telepítésének és konfigurálásának WAP kiszolgálói szerepkör.
A proxykiszolgáló adatvédelmi hitelesítő adatok |Összevonás megbízható hitelesítő adatok (a hitelesítő adatokat a proxy a FS adatvédelmi tanúsítványt regisztrálni használ. |Tartományi fiók, amely egy helyi rendszergazda az AD FS-kiszolgáló | Eredeti regisztrációs FS-WAP adatvédelmi tanúsítvány.
Active Directory összevonási szolgáltatások szolgáltatásfiókjának, "A tartomány felhasználói fiók beállítás használata" | Active Directory-felhasználói hitelesítő | Tartomány felhasználó | Az Active Directory felhasználói fiók hitelesítő adatokkal rendelkező találhatók az AD FS-szolgáltatás fiókon szolgál.

### <a name="create-the-ad-ds-account"></a>A Tartományi fiók létrehozása
Azure AD Connect telepítésekor a fiókot, **a könyvtárak csatlakoztatása** lapon adja meg az Active Directory szerepelnie kell, és rendelkezik a szükséges engedélyeket. Az telepítővarázslóban nem ellenőrzi az engedélyek és az esetleges problémák csak található szinkronizálás során.

Engedélyezi a választható funkció függ, hogy szüksége van az engedélyeket. Ha több tartománya van, az engedélyeket az erdőben tartomány jogosultsággal kell rendelkeznie. Ha ezek közül a funkciók nem engedélyezi, az alapértelmezett **Tartomány felhasználói** engedélyek megfelelőek.

A szolgáltatás | Engedélyek
------ | ------
Jelszó-szinkronizálás | <li>A címtár-módosítások bizonyos</li>  <li>Párhuzamos címtár összes módosítása
Hibrid Exchange-telepítés | A felhasználók, csoportok és névjegyekhez az [Exchange hibrid visszaírást](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) dokumentált attribútumok írási engedély.
Jelszó visszaírást | A felhasználók [-ös jelszavát az első lépések](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) a dokumentálni attribútumok írási engedély.
Eszköz visszaírást | Egy PowerShell-parancsprogramot [eszköz visszaírást](../active-directory-aadconnect-feature-device-writeback.md)ismertetett módon az engedélyeket.
Csoport visszaírást | Olvasása, létrehozása, módosítása és törlése a szervezeti egységre, ahol a felosztás csoportok el szeretné helyezni az objektumok csoportosítása.

## <a name="upgrade"></a>Frissítés
Frissítéskor Azure AD Connect egy példánya az új verziójára van szüksége a következő engedélyeket:

Egyszerű | Szükséges engedélyek | Használható
---- | ---- | ----
A telepítés varázslót felhasználói | A helyi kiszolgáló rendszergazdája | Frissítse a bináris.
A telepítés varázslót felhasználói | ADSyncAdmins tagja | A módosítások szinkronizálása szabályok és egyéb konfigurációs.
A telepítés varázslót felhasználói | Ha egy teljes SQL-kiszolgálót használ: DBO (vagy hasonló) a szinkronizálási motor adatbázis | Ellenőrizze az adatbázis szint módosításához, például a táblázatok frissítése az új oszlopok.

## <a name="more-about-the-created-accounts"></a>További információ a létrehozott fiókok

### <a name="active-directory-account"></a>Az Active Directory-fiókhoz
Ha sürgős beállításokat használja, akkor egy fiókot az Active Directory-szinkronizálás használt jön létre. A létrehozott fiók a erdő gyökértartomány a felhasználó tárolóban található, és rendelkezik a nevét előtaggal együtt **MSOL_**. A fiók nem jár le hosszú összetett jelszóval jön létre. A jelszóházirend tartománya van, győződjön meg arról, hogy hosszú, és a komplex jelszavakat szeretne tenni ehhez a fiókhoz.

![Active Directory-fiók](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD Connect szinkronizálási szolgáltatás fiókok
A helyi szolgáltatásfiók létrejön az telepítővarázslóban (kivéve, ha megadott egy fiókot az egyéni beállításokat is). A fiók **AAD_** elé, és a tényleges szinkronizálási szolgáltatás szerint futtatásához használt. Ha egy tartományvezérlőnek Azure AD Connect telepíti, a fiók jön létre a tartomány. Ha az SQL server, vagy ha használhatja a proxy-hitelesítést igényel, operációs rendszert futtató távoli kiszolgálót használ, a **AAD_** szolgáltatásfiók kell elhelyezni, a tartomány.

![Szolgáltatás-fiók szinkronizálása](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

A fiók nem jár le hosszú összetett jelszóval jön létre.

Ehhez a fiókhoz a a más fiókokhoz tartozó jelszavakat tárolja a biztonságos módon használják. Az ilyen fiókokat jelszavak tárolja titkosítva az adatbázisban. A titkos kulcsokat a titkosítási kulcs védett a titkosítási szolgáltatás titkos kulcs titkosítás Windows adatok védelme API (DPAPI) segítségével. Nem, alaphelyzetbe a jelszó-szolgáltatási fiók, mivel a Windows rendszer majd destroy a titkosítási kulcs biztonsági okokból.

Ha egy teljes SQL-kiszolgálót használ, a szolgáltatásfiók a szinkronizálási motor létrehozott adatbázis DBO. A szolgáltatás nem működnek, mint bármely más engedélyekkel rendelkező szánt. Egy SQL-bejelentkezés is jön létre.

A fiók is engedéllyel rendelkezik fájlokat, beállításkulcsainak és egyéb objektumok, a szinkronizálási motor kapcsolatos.

### <a name="azure-ad-service-account"></a>Azure Active Directory-szolgáltatási fiók
Azure AD-fiók létrejön a szinkronizálási szolgáltatás használatát. Ehhez a fiókhoz azonosítania kell magát a megjelenítendő név szerint.

![Active Directory-fiók](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

A fiók használják a kiszolgáló neve azonosítania kell magát a második részben álló a felhasználó nevét. A kép a kiszolgáló neve, FABRIKAMCON. Ha kiszolgáló átmeneti tárolására, a minden kiszolgáló van a saját fiók. Azure Active Directory van legfeljebb 10 szinkronizálási szolgáltatás partnert.

A fiók nem jár le hosszú összetett jelszóval jön létre. Azt, hogy csak a címtár-szinkronizálási műveletek elvégzéséhez engedéllyel rendelkezik **Címtár-szinkronizálás fiókok** speciális szerepkörbe adják. A speciális beépített szerepkör kívül az Azure AD Connect varázsló nem jogosultsággal, és az Azure-portálra megjelenítése ehhez a fiókhoz, a **felhasználó**szerepkörhöz.

![Active Directory-fiók szerepkör](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](../active-directory-aadconnect.md).
