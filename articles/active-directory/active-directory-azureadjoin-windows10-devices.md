<properties
    pageTitle="A Windows 10-eszközökön használata a munkahelyen |} Microsoft Azure"
    description="Funkciók pillanatkép nyújt a felhasználóknak és informatikai, milyen különböző módokon eszköz kiépítéstől, és használja a Windows 10 vállalati kontrasztos."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>A Windows 10-eszközökön használata a munkahelyen

Vonatkozik: a Windows 10 PC-re

A Windows 10 nyújt azoknak a szervezeteknek szól, amelyek lehetővé teszik a felhasználók férhetnek hozzá a munka típusú erőforrások biztonságos és megfelelő módon három modell.

- **Azure Active Directory csatlakozás** (Azure Active Directory Join), például elsősorban a felhőben az Office 365-ös forrásokat dolgozók számára. Azure Active Directory illesztés, amely a Windows 10-ben új élmény kiépítési önkiszolgáló munka.
- **Tartomány illesztés**azoknak a szervezeteknek szól, amelyek a helyszíni-alkalmazások és az erőforrások befektetések. Tartomány illesztés lehetővé teszi, hogy a Windows 10, amikor csatlakozik az Azure Active Directory továbbfejlesztett kezelését.
- **Olyan új BYOD megoldást**, a felhasználók, akik szeretne adni egy munkahelyi vagy iskolai Windows és könnyen access erőforrások személyes eszközökön.

Az alábbi táblázat bemutatja a felhasználók és az informatikai rendszergazdák kontrasztos milyen különböző módokon eszköz kiépítéstől, és a Windows 10 vállalati használt funkciók pillanatkép megtekintéséhez:

|                                                                                                                                                                 | Tartomány illesztés     | Azure Active Directory illesztés | Eszköz |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| A Windows eszközön való bejelentkezés munkahelyi vagy iskolai fiókokat.                                                                                                                      | igen             | igen           | nem              |
| Felhasználó egyetlen – bejelentkezés (SSO) az Office 365-ben és az Azure Active Directory-alkalmazásokhoz. Egyszeri bejelentkezés, az azt jelenti, hogy jelentkezzen be az csak egyszer szervezeti erőforrásokat eléréséhez. | igen             | igen           | igen             |
| Felhasználó SSO Kerberos/NTLM-alkalmazás között.                                                                                                                                  | igen             | Korlátozva van       | Igen, a virtuális Magánhálózati keresztül         |
| Erős hitelesítés és kényelmes való bejelentkezés a Microsoft Passport és a Windows Helló munkahelyi vagy iskolai fiókokat.                                                                   | igen             | igen           | igen             |
| A hozzáférést a nagyvállalati verzió a Windows áruházból, munkahelyi vagy iskolai fiókjával (nem Microsoft-fiók).                                                                                    | igen             | igen           | igen             |
| Enterprise-kompatibilis központi felhasználói beállításait a munkahelyi vagy iskolai fiókkal rendelkező eszközökön keresztül.                                                                                 | igen             | igen           | igen             |
| Az azt jelenti, hogy az eszközök, amelyek kompatibilis szervezeti házirendeket szervezeti alkalmazások korlátozhatja a hozzáférést.                                                      | igen             | igen           | igen             |
| A felhasználó önkiszolgáló a kiépítési eszközök a "munkavégzés bárhonnan".                                                                                                | nem              | igen           | igen             |
| Eszközök kezelése lehetőséget.                                                                                                                                       | Igen, az általános védelmi/SCCM keresztül | igen           | igen             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Azure Active Directory csatlakozás és a tartomány használata munkahelyi tulajdonú eszközök Bekapcsolódás a Windows 10-ben

A Windows 10 munka tulajdonú eszközök elérése a munka típusú erőforrások két lehetőségeket kínál:

- Azure Active Directory illesztés
- Tartomány illesztés

 Mindkét lehet attól függően, hogy a szervezet igényeinek és követelmények érvényes beállításokat. Egyes esetekben az szervezetek előnyt jelenthet engedélyezése a központi telepítés mindkét módszerhez.

## <a name="when-to-use-azure-active-directory-join"></a>Mikor érdemes használni az Azure Active Directory csatlakozás

Azure Active Directory illesztés az új önkiszolgáló munkahelyi kiépítési élmény a Windows 10-ben.  Munka típusú erőforrások, például elsősorban a felhőben az Office 365 eléréséhez dolgozók alkalmazásindítójukban. Jó számítógépekre, táblagépekre és vállalati telefonok konfigurálása legegyszerűbb módszer. Eszközök egységes vezérlők használatával a Windows platformon keresztül mobileszköz-kezelés kezelhetők.

A **Használati Azure Active Directory Bekapcsolódás az alábbi okok miatt**:

- Szeretné engedélyezni, hogy az önkiszolgáló kiépítési eszközök dolgozók útközben.
- Felhasználók biztosítson munka tulajdonú mobil eszközök, például táblagépen és -telefonok.
- A felhasználók egy csoportja kezelése az Azure Active Directory helyett az Active Directory, például szezonális dolgozók, vállalkozóknak vagy diákok szeretne.
- Szeretne csatlakozó funkciók biztosítson korlátozott helyszíni infrastruktúra távoli ág irodák dolgozók számára.
- Nem rendelkezik a helyszíni Active Directory.

Egyes szervezetek fogja használni Azure Active Directory csatlakozni az elsődleges mód, ha a munka tulajdonú eszközök üzembe különösen a legtöbb vagy az összes az erőforrások áttelepítése a felhőben. Hibrid szervezetek, az Active Directory és az Azure Active Directory módja vagy egy másik üzembe attól függően, hogy a felhasználó vagy a részlege is választhat.

Iskolai körzetek érték el és egyetemek, például előfordulhat, hogy oktatói az Active Directory és kezelése az Azure Active Directory diákok. Egyes vállalatok a távoli irodák vagy értékesítési részlegek kezelése az Azure Active Directory érdemes. Azure Active Directory csatlakozás és a tartomány illesztés módszerek egy hibrid szervezeten belüli használható. Azure Active Directory illesztés lehet tartomány való csatlakozás eszközök munkahelyi környezetben üzembe remek kiegészítésének.

**A legtöbb szokásos vállalati erőforrások teljes egészében keresztül a felhőben, ha a szervezet előfordulhat, hogy fogják túlságosan nagyra értékelni további előnyökkel jár Ha**:

- A helyszíni identitás infrastruktúrát függőségek eltávolíthatja.
- Az eszközök telepítési modell azáltal, hogy az önkiszolgáló konfigurációs távolodjon el a képkezelő megoldások első egyszerűsíthető.
- Mobileszköz-kezelés segítségével kezelheti az összes a más platformokhoz készült különböző.

További információt az Azure Active Directory Bekapcsolódás [bővítése felhő funkciók az Azure Active Directory Bekapcsolódás keresztül Windows 10-es eszközök](active-directory-azureadjoin-overview.md)című cikkben talál.

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Mikor érdemes használni a tartományt illesztés (vagy használat megtartása)

Az utolsó 15 évre számos vállalatnál használt tartomány illesztés munka eszközök csatlakozni. Lehetővé teszi a felhasználóknak, hogy jelentkezzen be az eszközök, az Active Directory munkahelyi vagy iskolai fiók. Tartomány illesztés is lehetővé teszi, hogy informatikai központilag és teljesen kezelése ezekre az eszközökre. Szervezetek általában támaszkodhat Képkezelő módszereket, amelyekkel rendelkezést eszközök, és gyakran System Center Configuration Manager (SCCM) vagy a csoport házirend (általános) kezelése használatával őket.

**A vállalati kell használni tartomány illesztés (vagy használat megtartása) az alábbi okok miatt**:

- Ha ezek az eszközök, amelyek NTLM/Kerberos rendszerbe állított Win32-alkalmazások.
- Általános vagy SCCM/DCM eszközök kezeléséhez szükséges.
- Továbbra is használhatja a képkezelő megoldások eszközök beállítása az alkalmazottak szeretne.

A **tartomány illesztés, a Windows 10-ben is biztosít a következő előnyökkel jár, ha csatlakozik az Azure Active Directory**:

- Erős eszköz kötött hitelesítés és kényelmes való bejelentkezés a Microsoft Passport és a Windows Helló munkahelyi vagy iskolai fiókokat.
- Eszközök használata a munkahelyi vagy iskolai fiók (nincs Microsoft-fiók szükséges) a Windows áruházból vállalati való hozzáférést.
- Enterprise-kompatibilis központi a felhasználói beállításokat minden eszközön használt munkahelyi vagy iskolai fiók (nincs Microsoft-fiók szükséges).
- Az azt jelenti, hogy az eszközök, amelyek kompatibilis szervezeti házirendeket szervezeti alkalmazások korlátozhatja a hozzáférést.

További információt az Azure Active Directory Bekapcsolódás [bővítése felhő funkciók az Azure Active Directory Bekapcsolódás keresztül Windows 10-es eszközök](active-directory-azureadjoin-overview.md)című cikkben talál.

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Csatlakozás a munkahelyi vagy iskolai személyes azonosításra tulajdonú eszközök engedélyezése

A támogatási BYOD a vállalaton belüli, a Windows 10 lehetőséget nyújt a felhasználó a munkahelyi vagy iskolai fiók felvétele a számítógépen, táblagépen vagy telefonon. Után a felhasználó hozzáadása egy munkahelyi vagy iskolai fiókjával, az eszköz regisztrált Azure Active Directory és tetszőlegesen: Ebben a szervezete beállította mobileszköz kezelési rendszerben. A címtár jelennek meg ezek az eszközök másként "Regisztrált" összehasonlítása az Azure Active Directory csatlakozott. Informatikai administraors alkalmazhat másik házirendek világosabb érintőképernyő nyújtó eszközökön személyes azonosításra tulajdonú mint munka tulajdonú eszközökön, ha azt szeretné, információk alapján.

A felhasználók hozzáadása egy munkahelyi vagy nagyon kényelmesen iskolai fiókját a személyes eszközére. Ehhez a munka-alkalmazások, az első alkalommal történő elérésekor, vagy manuálisan is a beállítások menü keresztül működnek. Ehhez a fiókhoz SSO számára szervezeti erőforrásokat.

További információt az Azure Active Directory Bekapcsolódás olvassa el a [Csatlakozás tartományhoz csatlakoztatott eszközök: Azure AD a Windows 10 találkozik](active-directory-azureadjoin-devices-group-policy.md)című témakört.

## <a name="enable-domain-join-or-azure-ad-join"></a>Tartomány vagy Azure Active Directory illesztés engedélyezése

A korábban ismertetett minden módszer (tartomány illesztés, Azure Active Directory illesztés és Hozzáadás működik, vagy iskolai fiókjával) a Windows 10-es felhasználói felület belépési pontról van-e. Azonban az összes szükséges informatikai rendszergazda funkcióhoz az infrastruktúrára előtt a felület működni fog.

## <a name="requirements-for-deploying-azure-ad-join"></a>Telepíti az Azure Active Directory Bekapcsolódás vonatkozó követelmények

Azure Active Directory Bekapcsolódás üzembe bármely beállítása a felhasználók a következőkre lesz szüksége:

- Egy Azure AD-előfizetést.
- Az Azure Active Directory prémium verzióra szóló előfizetés, például a mobileszköz management automatikus igénylésének, ha szüksége van további lehetőségeket.
- Mobileszköz-kezelés – például Microsoft Intune előfizetést, az Office 365-ös mobileszköz-kezelés, vagy a partner mobileszköz management szállítók integrálása az Azure Active Directory közül. (Lásd: a [Gyakori kérdések szakaszban](#frequently-asked-questions) a jelen cikk további információt a vége).

Ha Önök létesítményeit hibrid, azt ajánljuk, hogy a helyszíni könyvtár kiterjesztése Azure Active Directory Azure AD Connect rendszerbe.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Azure Active Directory csatlakozik a tartomány használatára vonatkozó követelmények

Tartomány illesztés továbbra is működik, mint mindig rendelkezik. Azonban az Azure Active Directory előnyök szüksége lesz az alábbiakat:

- Egy Azure AD-előfizetést.
- Ha ki szeretné terjeszteni a helyszíni könyvtár az Azure Active Directory Azure AD Connect telepítése.
- Egy házirendet, amely lehetővé teszi az Azure Active Directory feltételes hozzáférjen a tartományhoz eszközöket.
- Egy házirendet, amely lehetővé teszi a "tartományhoz" eszközök elérését, ha szeretné engedélyezni szeretné a korlátozhatja a hozzáférést az egyes eszközök esetén.
- System Center Configuration Manager 1509 ahhoz, hogy kompatibilis eszközök igénylő szabályairól technikai előzetes verziót. (Lásd: a TechNet dokumentáció és blogbejegyzésből).

További információt a tartomány illesztés, a Windows 10-ben című [Csatlakozás tartományhoz csatlakoztatott eszközök: Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>BYOD és a "Munkahelyi vagy iskolai fiók hozzáadása" használatára vonatkozó követelmények

Ahhoz, hogy "saját eszköz hozása" (BYOD) munkahelyi vagy iskolai fiókkal rendelkező szüksége van a következőket:

- Egy Azure AD-előfizetést.
- Az Azure Active Directory prémium verzióra szóló előfizetés, például a mobileszköz management automatikus igénylésének, ha szüksége van további lehetőségeket.

## <a name="requirements-for-using-microsoft-passport"></a>A Microsoft Passport használatára vonatkozó követelmények

Ahhoz, hogy a Microsoft Passport, a következőkre lesz szüksége:

- Egy nyilvános kulcs infrastruktúrájának Microsoft Passport használó tanúsítvány-alapú hitelesítés támogatására.
- Egy Microsoft Passport az Azure Active Directory csatlakozás és a munkahelyi vagy iskolai fiókokat használó tanúsítvány-alapú hitelesítés támogatására Intune-előfizetést.
- System Center Configuration Manager 1509 verzió Technical Preview (lásd: a TechNet dokumentáció és blogbejegyzésből) által használt Microsoft Passport tartomány illesztés tanúsítvány-alapú hitelesítés támogatására.
- Ez a szabály Microsoft Passport engedélyezve van a szervezeten belül.

Egy nyilvános kulcsú infrastruktúra helyett billentyű-alapú Microsoft Passport engedélyezheti az alábbiak szerint:

- Telepítse a Windows Server 2016 "Munkakörnyezeti kép 1" DCs (nincs szükség a tartomány vagy erdő működési szintek; az egyes Active Directory-hely elegendő redundancia szolgáló több DCs).
- A Microsoft Passport ahhoz, hogy a szervezet házirendjének beállítása.

További információt a Microsoft Passport és a Windows Helló a Windows 10-ben < link-to-MS-Passport-and-Windows-Hello-document > témakörben talál.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Melyik partner mobileszköz management termékek integrálása az Azure Active Directory?

A következő alvállalkozó termékek integrálása az Azure Active Directory egységesített regisztrációs és feltételes hozzáférés beállítása a Windows 10-es:

- AirWatch VMware szerint
- Citrix Xenmobile
- Lightspeed Mobile Manager
- SOTI helyszíni mobileszköz-kezelés
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Mi a helyzet a munkahelye csatlakozni, a Windows 10?
Munkahelye illesztés BYOD engedélyezése a Windows 8.1 rendszerben használt. A Windows 10-ben BYOD engedélyezve van keresztül "Hozzáadása egy munkahelyi vagy iskolai fiókját" a dokumentum korábbi című cikkben ismertetett módon. Azoknak a szervezeteknek szól együttműködő nem a mobileszköz-kezelés az Azure Active Directory, felhasználók is az eszköz beléptetéséhez be manuálisan keresztül **Beállítások**kezelése > **fiókok** > **munka access**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Felhasználók csatlakozhat a Microsoft-fiókjukkal a tartományi fiók a Windows 10-ben?
Nem a Windows 10. Windows 8.1 rendszerben eszközök tartományhoz tartozó felhasználók sikerült "Csatlakozás" a Microsoft-fiókjukkal (például Hotmail, Live, Outlook, Xbox, stb.) a tartományi fiók ahhoz, hogy egyes szolgáltatások például SSO élő szolgáltatások, a Windows áruház használati, és a felhasználói beállításokat minden eszközön barangoló. A Windows 10 a "Csatlakozás" funkció Microsoft-fiók van többé nem érhető el. A felhasználó felveheti egy vagy több Microsoft-fiók SSO fogyasztói szolgáltatások, például a Windows áruházból lehetővé teszi, hogy további fiókokat. Ez történik, az alábbi **Beállítások** > **fiókok** > **fiókját**.

Felhasználók, akik Windows 8.1 tartományhoz eszközökről frissít, és ki volt, a Microsoft-fiókjukkal csatlakozik, automatikusan lesz a csatlakoztatott a listához további fiókok használata a Microsoft-fiókjukkal.


## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
