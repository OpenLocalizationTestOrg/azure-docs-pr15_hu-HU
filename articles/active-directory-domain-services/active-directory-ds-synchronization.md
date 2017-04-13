<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Szinkronizálás felügyelt tartományban lévő |} Microsoft Azure"
    description="Szinkronizálás az Azure Active Directory tartományi szolgáltatások felügyelt tartományban ismertetése"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Szinkronizálás az Azure Active Directory tartományi szolgáltatások felügyelt tartományban
Az alábbi ábra szemlélteti, hogyan működik a szinkronizálás az Azure Active Directory tartományi szolgáltatásokban felügyelt tartományok.

![Szinkronizálás topológia az Azure Active Directory tartományi szolgáltatások](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>A helyszíni címtárból a Azure AD-bérlő-szinkronizálás
Azure AD Connect szinkronizálási szinkronizálni a felhasználói fiókok, a csoport tagságát szolgál, és hitelesítő adatok kivonatolja az Azure Active Directory-bérlőhöz. Felhasználó tulajdonságainak (UPN) például partnerek és a helyszíni biztonsági AZONOSÍTÓK (biztonsági azonosító) vannak szinkronizálva. Ha használja az Azure Active Directory tartományi szolgáltatások, NTLM és Kerberos-hitelesítés szükséges hitelesítő régebbi tiltva is a Azure AD-bérlő szinkronizálódnak.

Ha írási-back állítja be, az Azure Active directory bekövetkezett változásokat a rendszer szinkronizálja ismét a helyszíni Active Directory. Például ha módosítja jelszavát az Azure Active Directory önkiszolgáló jelszó módosítása funkcióival, a módosított jelszó frissül a helyszíni Active Directory tartományi.

> [AZURE.NOTE] Mindig a legújabb Azure AD Connect segítségével módon javítását az összes ismert hibák.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>A felügyelt tartomány a Azure AD-bérlő szinkronizálása
Felhasználói fiókok, csoporttagság és hitelesítő adatok tiltva alapján szinkronizálja a Azure AD-bérlő Azure Active Directory tartományi szolgáltatások felügyelt tartománya. A szinkronizálási folyamat automatikusan karbantarthatók. Nem kell beállítani, figyelheti, vagy a szinkronizálási folyamat kezelése. A szinkronizálási folyamat akkor is egy-way/egyirányú jellegű. A felügyelt tartomány írásvédett nagymértékben kivételével minden egyéni szervezeti hoz létre. Emiatt nem lehet módosítani a felhasználói attribútumok, a felhasználói jelszavak vagy a csoporttagság a felügyelt tartományon belül. Emiatt nem nincs fordított szinkronizálása a felügyelt tartomány vissza a Azure AD-bérlő a változtatásokat.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Több elem erdőből szinkronizálás a helyszíni környezet
Sok szervezet egy helyszíni meglehetősen összetett identitás infrastruktúra több fiók erdők álló rendelkezik. Azure AD Connect támogatja a szinkronizált felhasználók, csoportok és a hitelesítő adatok tiltva a több elem erdőkörnyezetben az Azure Active Directory-bérlőhöz.

Viszont a Azure AD-bérlő sokkal egyszerűbbé és strukturálatlan névtér. Ahhoz, hogy a felhasználók tudnak stabilan hozzáférni az Azure Active Directory által biztosított alkalmazások, egyszerű Felhasználónévi ütközések feloldása a felhasználói fiókok különböző erdőkben között. Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány medvék zárja be a Azure AD-bérlő hasonlóságot. Emiatt a felügyelt tartomány látható egy egyszerű szervezeti struktúra. Az összes felhasználók és csoportok a "AADDC felhasználók" tárolóban, függetlenül a helyszíni tartomány vagy erdő, amelyhez azok voltak szinkronizálta a tárolja. Előfordulhat, hogy egy hierarchikus szervezeti konfigurált helyszíni struktúra. A felügyelt tartomány azonban továbbra is tartalmaz egy egyszerű, strukturálatlan szervezeti struktúra.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Kivételek – mi nem szinkronizálódnak a felügyelt tartomány
A következő objektumok és attribútumok nem szinkronizálódnak a Azure AD-bérlő vagy a felügyelt tartomány:

- **Attribútumok kizárt:** Előfordulhat, hogy bizonyos attribútumok zárni az Azure Active Directory-bérlőhöz a helyszíni tartomány használata az Azure AD Connect a szinkronizálás lehetősége. Ezek a kizárt attribútumok nem érhetők el a felügyelt tartományban.

- **Házirendek csoport:** A helyszíni tartomány csoport házirendeket nem szinkronizálódnak a felügyelt tartományára.

- **SYSVOL megosztás:** Hasonlóképpen a SYSVOL megosztás a helyszíni tartomány tartalmát nem szinkronizálódnak a felügyelt tartomány.

- **Számítógép-objektumok:** A helyszíni tartományhoz számítógépek számítógép-objektumok nem szinkronizálódnak a felügyelt tartományára. Ezek a számítógépek megbízhatósági kapcsolat a felügyelt tartománnyal rendelkezik, és nem csak a helyszíni tartomány tartoznak. A felügyelt tartományban a számítógép-objektumok csak kifejezetten tartomány-tagja a felügyelt tartományhoz számítógépekhez készült megtalálta.

- **SidHistory attribútumok a felhasználók és csoportok:** Az elsődleges felhasználó elsődleges csoportjának a helyszíni tartomány biztonsági azonosítók szinkronizálja a felügyelt tartomány. Azonban meglévő SidHistory attribútumainak a felhasználók és csoportok nem szinkronizálódnak a helyszíni tartomány a felügyelt tartományára.

- **Szervezeti egység (OU) struktúrák:** A helyszíni tartomány megadott szervezeti egység nem szinkronizálja a felügyelt tartományára. A felügyelt tartomány két beépített szervezeti szerepelnek. Alapértelmezés szerint a felügyelt tartománynak egy egyszerű szervezeti struktúra. Előfordulhat, hogy azonban szeretne [létrehozni a felügyelt tartományban lévő egyéni szervezeti](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Hogyan adott attribútumok vannak szinkronizálva a felügyelt tartományára
Az alábbi táblázat néhány gyakori attribútum sorolja fel, és ismerteti, hogyan őket a rendszer szinkronizálja a felügyelt tartomány.

| A felügyelt tartomány attribútum | Forrás | Jegyzetek |
|:---|:---|:---|
|(UPN)|A Azure AD-bérlő használati UPN attribútum|Az egyszerű Felhasználónévi attribútum a Azure AD-bérlő az, ha a felügyelt tartomány szinkronizálása. Ezért a legtöbb megbízható úgy jelentkezzen be a felügyelt tartomány használ a (UPN).|
|SAMAccountName|Felhasználó mailnickname-beli a Azure AD-bérlő attribútum, vagy automatikusan létrehozott|Az SAMAccountName attribútummal megosztásban az Azure Active Directory-ös bérlőben az mailnickname-beli attribútumból. Ha több felhasználói fiókot ugyanazzal a mailnickname-beli attribútummal rendelkezik, akkor az SAMAccountName jön létre automatikusan. Ha a felhasználó mailnickname-beli vagy UPN előtag 20 karakternél hosszabb, az SAMAccountName jön létre automatikusan SAMAccountName attribútumok 20 karakter vonatkozó igényeinek kielégítéséhez.|
|Jelszavak|A Azure AD-bérlő a jelszavát|NTLM- vagy Kerberos-hitelesítés (más néven kiegészítő hitelesítő adatok) szükséges hitelesítő adatok tiltva a Azure AD-bérlő a vannak szinkronizálva. Ha a Azure AD-bérlő szinkronizált bérlői webhelyet, a helyszíni tartomány kifejezéskészletébe ezeket a hitelesítő adatokat.|
|Elsődleges felhasználó vagy csoport biztonsági AZONOSÍTÓK|Automatikusan létrehozott|A felhasználó vagy csoport fiókok elsődleges biztonsági AZONOSÍTÓK automatikusan létrehozott tartományban a felügyelt. Az attribútum nem egyezik meg az elsődleges felhasználó vagy csoport biztonsági AZONOSÍTÓK az objektum a helyszíni Active Directory tartományi. Az eltérés oka az, hogy a felügyelt tartomány névtere különböző biztonsági AZONOSÍTÓK a helyszíni tartomány-nál.|
|A felhasználók és csoportok biztonsági azonosítók|A helyszíni elsődleges felhasználók és csoportok biztonsági AZONOSÍTÓK|A felhasználók és csoportok a felügyelt tartománya SidHistory attribútum értéke a megfelelő elsődleges felhasználó vagy csoport biztonsági AZONOSÍTÓK a helyszíni tartomány megfelelően. Ez a szolgáltatás segítségével megkönnyítheti az felvonó-és-shift helyszíni alkalmazások felügyelt a tartományhoz, mivel az nem kell újra-vezérlés erőforrásokat.|

> [AZURE.NOTE] **Jelentkezzen be a felügyelt tartományban (UPN) formátumban:** Az SAMAccountName attribútummal lehet automatikusan létrehozott a felügyelt tartományban lévő egyes felhasználói fiókok. Ha több felhasználó van azonos mailnickname-beli attribútuma vagy felhasználók túl hosszú UPN prefixumokban, a felhasználóknak az SAMAccountName lehet automatikusan létrehozott. Emiatt a SAMAccountName formátumban (például CONTOSO100\joeuser) kifolyólag sem jelentkezzen be a tartomány megbízható lehetőséget. A felhasználók automatikusan létrehozott SAMAccountName UPN előtag eltérhetnek. Az egyszerű formázás (például 'joeuser@contoso100.com') való bejelentkezéshez a felügyelt tartomány megbízható.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Olyan objektumok, amelyek a felügyelt tartományból az Azure Active Directory-bérlőhöz nem szinkronizálódnak
A jelen cikk egy előző szakaszban leírtak nem nincs szinkronizálása felügyelt tartománya vissza az Azure Active Directory-ös bérlői webhelyen. Előfordulhat, hogy szeretne [létrehozni egy egyéni szervezeti egység (OU)](./active-directory-ds-admin-guide-create-ou.md) a felügyelt tartományban. További más szervezeti, felhasználók, csoportok vagy belül e egyéni szervezeti fiókok hozhat létre. A létrehozott egyéni szervezeti belüli objektumok közül egyik sem a rendszer szinkronizálja ismét a Azure AD-bérlő. Az objektumok használata csak a felügyelt tartományon belül érhetők el. Ezért ezeknek az objektumoknak nem láthatók Azure Active Directory PowerShell-parancsmagok, Azure Active Directory Graph API vagy az Azure Active Directory kezelése felhasználói felület használatával.


## <a name="related-content"></a>Kapcsolódó tartalom
- [Szolgáltatások – Azure Active Directory tartományi szolgáltatások](active-directory-ds-features.md)

- [Telepítési forgatókönyvek - Azure Active Directory tartományi szolgáltatások](active-directory-ds-scenarios.md)

- [Azure Active Directory tartományi szolgáltatások hálózati megfontolandó szempontok](active-directory-ds-networking.md)

- [Azure Active Directory tartományi szolgáltatások – első lépések](active-directory-ds-getting-started.md)
