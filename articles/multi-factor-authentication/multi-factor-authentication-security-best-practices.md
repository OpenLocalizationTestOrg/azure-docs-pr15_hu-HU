<properties 
    pageTitle="Biztonsággal kapcsolatos gyakorlati tanácsok az Azure MFA használatával"
    description="A dokumentum tartalmaz ajánlott eljárások megalkotása Azure fiókok Azure MFA használata"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Biztonsággal kapcsolatos gyakorlati tanácsok az Azure többtényezős hitelesítés használatával az Azure Active Directory-fiókok

Amikor a hitelesítési folyamat fejlesztése, a többtényezős hitelesítés az előnyben részesített választási lehetőségek, a legtöbb szervezet számára. Azure többtényezős hitelesítés (MFA) lehetővé teszi a vállalatok a biztonság és megfelelőség követelményeknek közben kezeléséről a felhasználók egy egyszerű bejelentkezés módja. Ez a cikk kiterjed az Azure MFA elfogadásához megtervezésekor vegye figyelembe gyakorlati tanácsokat.

## <a name="best-practices-for-azure-multi-factor-authentication-in-the-cloud"></a>Gyakorlati tanácsok az Azure többtényezős hitelesítés a felhőben
Annak érdekében, hogy az összes felhasználót biztosítson többtényezős hitelesítés, és kövesse az Azure többtényezős hitelesítés kínál bővített szolgáltatások előnyei, szüksége lesz engedélyezze az összes felhasználót többtényezős hitelesítést Azure.  Ez végezhető el az alábbiak egyikét:

- Azure Active Directory Premium vagy a vállalati mobilitás Suite
- Többtényezős hitelesítés szolgáltató

### <a name="azure-ad-premiumenterprise-mobility-suite"></a>Azure Active Directory prémium/vállalati mobilitás programcsomagban

![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Az első ajánlott a Azure Active Directory Premium vagy a vállalati mobilitás Suite felhőben elfogadása Azure többtényezős lépésként licencek hozzárendelése a felhasználókhoz.  Azure többtényezős hitelesítés képezi a programcsomagok részét, és olyan a szervezet nem kell semmit, ha ki szeretné terjeszteni a többtényezős hitelesítés képesség minden felhasználó számára további.

Amikor többtényezős hitelesítés beállítása fontolja meg a következő:

- Nem kell hozzon létre egy többtényezős hitelesítés szolgáltatóval.  Azure Active Directory prémium verzió és az Enterprise mobilitás csomagot Azure többtényezős hitelesítés megtalálható.  Ha hoz létre egy Auth szolgáltatóval olvasható dupla számlát kapni.
- Azure AD Connect követelmény csak akkor, ha a rendszer a helyszíni Active Directory-környezet szinkronizálása az Azure Active directory. Ha csak egy helyszíni Active Directory-példányt tartson nem szinkronizálja a rendszer Azure Active Directory címtárhoz, szükségtelen Azure AD Connect.


### <a name="multi-factor-auth-provider"></a>Többtényezős hitelesítés szolgáltató

![Többtényezős hitelesítés szolgáltató](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Ha nem rendelkezik Azure Active Directory Premium vagy az Enterprise mobilitás csomagot majd az első ajánlott elfogadása Azure többtényezős a felhőben lépésként hozzon létre egy MFA Auth szolgáltatóval. Bár a globális rendszergazdák számára történő MFA a szervezet kiterjesztése minden felhasználó számára, és végezze el a többtényezős hitelesítés lehetőség, hogy szüksége van egy többtényezős hitelesítés szolgáltató kell üzembe helyezésekor Azure Active Directory, akik alapértelmezés szerint MFA érhető el.
A Auth szolgáltató kiválasztásakor kell címtár kiválasztása, és vegye figyelembe a következőket:

- Többtényezős hitelesítés szolgáltató létrehozása Azure Active Directory címtárhoz szükségtelen.
- Szüksége lesz, ha másnak szeretné többtényezős hitelesítés kiterjesztése a felhasználóit és/vagy a globális rendszergazdák kihasználhatja a szolgáltatások, például az adatkezelési portál, egyéni Üdvözlések és jelentések engedélyezni szeretné a többtényezős hitelesítés szolgáltató társíthatja az Azure Active directory.
- DirSync vagy AAD Sync követelmény csak akkor, ha az Azure Active directory a helyszíni Active Directory-környezet szinkronizálása. Ha csak egy Azure AD-címtár, amely nincs szinkronizálva a helyszíni Active Directory példányának használ, nem szükséges DirSync vagy AAD Sync.
- Ha Azure Active Directory Premium vagy az Enterprise mobilitás csomagot, nem szükséges hozzon létre egy többtényezős hitelesítés szolgáltatóval. Csak akkor van szüksége a licenc hozzárendelése egy felhasználóhoz, és ezután megkezdheti MFA bekapcsolása a felhasználók számára.
- Ügyeljen arra, hogy a válassza ki a megfelelő használatát modell a vállalati (auth vagy engedélyezett felhasználónként), a használatát modell kiválasztása után nem módosítható.

### <a name="user-account"></a>Felhasználói fiók
MFA engedélyezése a felhasználóknak, ha a felhasználói fiókok lehet-e az három alapvető államok egyik: letiltva, engedélyezve vagy vannak érvényben.
Annak érdekében, hogy a legmegfelelőbb beállítás esetén a telepítéshez használni a az alábbi lépéseket:

- Amikor a felhasználó állam letiltva van beállítva, a felhasználó nem többtényezős hitelesítést használ. Ez az alapértelmezett állapota.
- A felhasználói állapot beállítása esetén engedélyezve van, az azt jelenti, hogy a felhasználó engedélyezve van, de nem fejeződött be a regisztrációs folyamat. A következő bejelentkezés a folyamat befejezéséhez kéri. Ez a beállítás nem böngésző alkalmazások nincs hatással. Minden alkalmazás továbbra is működnek, amíg a regisztrációs folyamat befejeződik.
- A felhasználói állapot beállítása esetén kényszerített, az azt jelenti, hogy a felhasználó előfordulhat, hogy, vagy előfordulhat, hogy nem befejeződött regisztrációs. Ha a regisztrációs folyamat befejezése majd használnak többtényezős hitelesítés. Egyéb esetben a felhasználó felkéri a program a következő bejelentkezés a regisztrációs folyamat befejezéséhez. Ez a beállítás alkalmazások a böngészőben nem befolyásolja. Az alkalmazások nem fog működni, amíg az alkalmazás jelszavak létrehozott és használt.
- Sablonnal a felhasználói értesítés érhető el az [Ismerkedés az Azure többtényezős hitelesítés a felhőben](multi-factor-authentication-get-started-cloud.md) cikkben MFA átállá kapcsolatban a felhasználók e-mailt küldhet.

### <a name="supportability"></a>Támogatási lehetőségek

Mivel a felhasználók többsége csak a jelszavak használatáról hitelesítést végezni vannak szokott, fontos, hogy a vállalat előtérbe tájékoztatás összes felhasználóra vonatkozó ezt a folyamatot. A Jelenléti állapot csökkentheti az annak valószínűsége, hogy a felhasználók kisebb problémák megoldásához MFA az ügyfélszolgálathoz kérni fogja.
Vannak azonban olyan bizonyos esetekben, ahol ideiglenes letiltása MFA szükség. Használja az alábbi útmutatásokat megérteni az adott esetek kezelése:

- Győződjön meg arról, hogy a technikai támogatási munkatársaknak jól ismerik fogópont forgatókönyvek, ahol a mobilalkalmazásban vagy a telefon nem kap egy értesítést vagy a telefonhívás-, és emiatt a felhasználó nem tud bejelentkezni. "Kihagyásával" többtényezős hitelesítés hitelesítést végezni a egyetlen felhasználójának engedélyezni egyszeri figyelmen kívül hagyása beállításokat is engedélyezhet őket. A figyelmen kívül hagyása ideiglenes, és a megadott számú másodpercig után lejár.
- Ha szükséges, a megbízható IP-címei képesség az Azure MFA is élvezheti. Ez a szolgáltatás lehetővé teszi, hogy egy felügyelt vagy szövetséges bérlői rendszergazdái kikerülheti többtényezős hitelesítés a felhasználóknak, hogy a vállalat helyi intranet elemet a bejelentkezéshez. A szolgáltatások az Azure Active Directory prémium, vállalati mobilitás csomagja Azure többtényezős hitelesítés licencekkel rendelkező Azure AD-bérlők érhetők el.


## <a name="best-practices-for-azure-multi-factor-authentication-on-premises"></a>Gyakorlati tanácsok a helyszíni Azure többtényezős hitelesítés
Ha a vállalat úgy döntött, hogy a saját infrastruktúra ahhoz, hogy MFA kihasználhatja, az Azure többtényezős hitelesítést kiszolgáló helyszíni telepítéséhez szükséges lesz. A kiszolgáló MFA-összetevők az alábbi ábrán látható:

![Többtényezős hitelesítés szolgáltató](./media/multi-factor-authentication-security-best-practices/server.png)
*alapértelmezés szerint nincs telepítve ** telepítve van, de alapértelmezés szerint nincs engedélyezve


Azure többtényezős hitelesítést kiszolgáló biztonságos, felhőalapú Azure Active Directory-fiókok férnek hozzá a helyszíni erőforrásokat és használható.  Jó helyen jár ez is csak az összevonási végezhető el.  Ez azt jelenti, hogy kell az AD FS van, és azt a Azure AD-bérlő az összevont.
Mikor érdemes többtényezős hitelesítés kiszolgáló beállítása a következő:

- Ha Ön biztonságossá tétele Azure Active Directory erőforrásokat, használja az Active Directory összevonási szolgáltatások, a hitelesítési 1st tényező történik majd a helyszíni Active Directory összevonási szolgáltatások használatával és a 2nd tényező elvégzett helyszíni vállalta az igény szerint.
- Még nem annak követelménye, hogy az Azure többtényezős hitelesítést kiszolgáló telepíthető az Active Directory összevonási szolgáltatások összevonási kiszolgáló azonban a többtényezős hitelesítést adaptert Active Directory összevonási szolgáltatások Windows Server 2012 R2 rendszerben futó AD FS kell telepíteni. Egy másik számítógépen, telepítse a kiszolgálót, feltéve hogy verzióját, és az AD FS-adaptert külön-külön telepítse az Active Directory összevonási szolgáltatások összevonási kiszolgálón. Végre az alábbi eljárást utasításokért nézze meg a kártyát külön-külön telepítése.
- Az többtényezős hitelesítés AD FS-adaptert telepítővarázslóban nevű PhoneFactor rendszergazdák az Active Directoryban biztonsági csoport hoz létre, és hozzáadja az Active Directory összevonási szolgáltatások szolgáltatásfiók az összevonási szolgáltatás ebben a csoportban. Célszerű meggyőződni a tartomány vezérlőn, hogy valóban létrejön a PhoneFactor rendszergazdák csoport, és az Active Directory összevonási szolgáltatások szolgáltatásfiók a csoport tagjának. Ha szükséges, az Active Directory összevonási szolgáltatások szolgáltatási fiók felvétele a tartományvezérlőnek a PhoneFactor rendszergazdák csoport manuálisan.

### <a name="user-portal"></a>Felhasználói portál
A portál fut az Internet Information Server (IIS) webhely, amely lehetővé teszi, hogy az önkiszolgáló funkciók és a teljes felhasználói felügyeleti lehetőségeket biztosít. Használja az alábbi lépéseket az összetevő konfigurálása:

- IIS 6 vagy újabb verzióját szükség
- ASP.NET v2.0.507207 van telepítve, és regisztrálva
- A kiszolgáló peremhálózat telepíthető.



### <a name="app-passwords"></a>Alkalmazás jelszavak
Ha a szervezet identitásszolgáltatóval összevont egyszeri bejelentkezés használata az Azure Active Directory és fogja Azure MFA kell használnia, majd tartsa szem előtt a következőket alkalmazás jelszavak használata esetén (ne feledje, hogy ez csak érvényes szövetséges (SSO) használatos):

- Az alkalmazás jelszó Azure Active Directory ellenőrzött lesz, és ezért megkerüli az összevonási. Összevonás csak használatos alkalmazás jelszó beállítását.
- Szövetséges (SSO) felhasználói jelszavak kíván tárolni a szervezeti azonosítóját. A felhasználó elhagyja a vállalatot, ha rendelkezik ezekkel az információkkal flow szervezeti azonosító használatával DirSync valós időben. Fiók letiltása és törlése órát is igénybe vehet legfeljebb 3 szinkronizálni, hogy késleltetné alkalmazás jelszó letiltása és törlése az Azure Active Directory.
- A helyszíni hozzáférés-vezérlés ügyfél beállításai vannak nem fogadja el a jelszó alkalmazás
- A helyszíni hitelesítés naplózás / naplózás lehetőség nem érhető el a jelszó alkalmazás
- További végfelhasználói oktatási szükség a Microsoft Lync 2013 ügyfél számára.
- Bizonyos speciális építészeti tervek megkövetelheti kombinációjával szervezeti felhasználónév és a jelszavakat és az alkalmazás jelszavak többtényezős hitelesítés az ügyfelekkel, attól függően, hogy hol hitelesítő használatakor. Az ügyfelek számára, hogy a helyszíni infrastruktúrát hitelesítést használja a egy szervezeti felhasználónevével és jelszavával. Az Azure Active Directory hitelesítő ügyfelek az alkalmazás jelszót használja.
- Alapértelmezés szerint a felhasználók nem alkalmazás jelszavak, létrehozása, ha vállalata van szüksége, vagy ha felhasználóimnak, hogy az alkalmazás jelszót bizonyos esetekben kell győződjön meg arról, hogy be van jelölve a beállítás engedélyezése a felhasználóknak jelentkezhet be nem böngésző alkalmazások alkalmazás jelszavak létrehozásához.

## <a name="additional-considerations"></a>További megfontolandó szempontok
Használja az alábbi lista lehet áttekinteni néhány további szempontok és a gyakorlati tanácsok az egyes összetevőknek, amely a helyszíni telepített:

A módszer|Leírás
:------------- | :------------- |
[Az Active Directory összevonási szolgáltatás](multi-factor-authentication-get-started-adfs.md)|Többtényezős hitelesítést Azure Active Directory összevonási szolgáltatások beállításával kapcsolatos információk.
[RADIUS Authenticaton](multi-factor-authentication-get-started-server-radius.md)|  A telepítő és Azure MFA-kiszolgáló konfigurálása sugarú vonatkozó információk.
[IIS-hitelesítés](multi-factor-authentication-get-started-server-iis.md)|A telepítő és Azure MFA-kiszolgáló konfigurálása az IIS vonatkozó információk.
[A Windows Authenticaton](multi-factor-authentication-get-started-server-windows.md)|  A telepítő és a Windows-hitelesítéssel az Azure-MFA kiszolgáló konfigurálása vonatkozó információk.
[LDAP-hitelesítés](multi-factor-authentication-get-started-server-ldap.md)|A telepítő és Azure MFA-kiszolgáló konfigurálása LDAP hitelesítéssel vonatkozó információk.
[Távoli asztali átjáró és Azure többtényezős hitelesítést kiszolgáló RADIUS használata](multi-factor-authentication-get-started-server-rdg.md)|  A telepítő és Azure MFA-kiszolgáló konfigurálása a távoli asztali átjáró RADIUS használata vonatkozó információk.
[A Windows Server Active Directory szinkronizáló](multi-factor-authentication-get-started-server-dirint.md)|Információ a telepítő és konfigurálása az Active Directory és az Azure MFA kiszolgáló közötti szinkronizálást.
[A Azure többtényezős hitelesítés kiszolgáló mobil alkalmazás webes szolgáltatás üzembe helyezése](multi-factor-authentication-get-started-server-webservice.md)|Telepítés és konfigurálása az Azure MFA server webszolgáltatás vonatkozó információk.
[Speciális VPN Azure többtényezős hitelesítés konfigurálása](multi-factor-authentication-advanced-vpn-configurations.md)|LDAP vagy RADIUS Cisco ASA Citrix Netscaler és boróka/Pulse biztonságos VPN készülékek beállításáról.


## <a name="additional-resources"></a>További források
Ez a cikk gyakorlati tanácsokat kiemeli az Azure többtényezős, miközben vannak egyéb információforrások, amelyek a MFA bevezetését tervezi közben is használhatja. Az alábbi lista fontos témakörök, amelyek segítségére lehetnek a folyamat során foglalja magában:

- [Az Azure többtényezős hitelesítés jelentései](multi-factor-authentication-manage-reports.md)
- [Gyakorlat Azure többtényezős hitelesítés beállítása](multi-factor-authentication-end-user-first-time.md)
- [Azure többtényezős hitelesítés – gyakori kérdések](multi-factor-authentication-faq.md)
