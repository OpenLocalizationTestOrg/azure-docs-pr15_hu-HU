<properties
   pageTitle="Azure Active Directory jelentés események |} Microsoft Azure"
   description="Elérhető megtekintési és az Azure Active Directory letöltésével események ellenőrzött"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/19/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory jelentés naplózása

*Ez a dokumentáció része a [Azure Active Directory jelentéskészítés útmutató] (aktív-címtár-jelentés-guide.md).*

Az Azure Active Directory naplójelentés segíti a felhasználókat, hogy az illető Azure Active Directory-e védett műveletek azonosítása. Kiemelt műveletek tartalmazza a jogosultságszint módosítása (például szerepkör létrehozása vagy jelszavak átállításáról), módosítását házirend-beállításokat (például jelszóházirendek) és címtár konfiguráció (például tartomány-összevonás beállítások változásairól) módosításainak. A jelentéseket a naplóbejegyzés az esemény nevét, a szereplő, a műveletet, a cél erőforrás, módosítása és dátuma és ideje (UTC szerint megadva) által érintett végző szükséges. Ügyfelek képesek események listájának lekérése az Azure Active Directory az [Azure-portálon](https://portal.azure.com/)keresztül [megtekintése a naplózási naplók](active-directory-reporting-azure-portal.md)leírt módon.


## <a name="list-of-audit-report-events"></a>Jelentés események listája
<!--- audit event descriptions should be in the past tense --->

Események                               | Esemény leírása
------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Felhasználói események**                      |
Felhasználó hozzáadása                             | Megjelenik egy felhasználó a címtárban.
Felhasználó törlése                          | Törölt egy felhasználó a címtárban.
Licenc tulajdonságainak beállítása               | A licenc tulajdonságainak beállítása egy felhasználó a címtárban.
Felhasználó jelszavának alaphelyzetbe állítása                  | A felhasználó a címtárban jelszavának alaphelyzetbe állítása.
Felhasználói jelszó módosítása                 | A felhasználó a címtárban jelszava módosul.
Ügyféllicenc módosítása                  | A felhasználó a címtárban rendelt licenc változik. Szeretné látni, hogy milyen licencek lettek frissítve, az alábbi [frissítés felhasználó](#update-user-attributes) tulajdonságainak megtekintése
Felhasználó módosítása                          | Frissíti a felhasználó a címtárban. [Lásd az alábbi](#update-user-attributes) attribútumok frissíthető.
Beállított kötelező felhasználói jelszó módosítása       | A tulajdonság, amely a felhasználót, hogy módosítani a jelszavát a bejelentkezés kezd.
Felhasználói hitelesítő adatok frissítése        | Felhasználó módosította a jelszavát  
**Eseményekre**                     |
Csoport hozzáadása                            | Csoport létrehozása a címtárban.
Csoport módosítása                         | A címtár csoport frissíteni. Látható, hogy milyen csoport tulajdonságok lettek frissítve, olvassa el az alábbi szakaszt a [Csoport tulajdonságok ellenőrzött](#update-group-attributes)
Csoport törlése                         | Törölt csoport a címtárban.
Tagok hozzáadása csoporthoz            | A címtár csoportban elhelyezett tag.
Tag eltávolítása a csoportból         | Tag eltávolítása egy csoportból az a címtár.
CreateGroupSettings              | Létrehozott csoport beállításai
UpdateGroupSettings          | Csoportbeállítások frissíteni. Látható, hogy milyen csoportbeállítások frissített, olvassa el az alábbi szakaszt a [Csoport tulajdonságok ellenőrzött](#update-group-attributes)
DeleteGroupSettings            | Törölt csoport beállításai
SetGroupLicense                  | Állítsa a csoport licencet.
SetGroupManagedBy              | Felhasználó által kezelt csoport beállítása
AddGroupMember               | A felvett tag csoportba
RemoveGroupMember            | Tag eltávolítása a csoportból
AddGroupOwner                | A felvett tulajdonosát, hogy a csoport
RemoveGroupOwner                 | Eltávolított tulajdonosa csoportból
**Alkalmazás események**               |
Egyszerű hozzáadása                | A szolgáltatás egyszerű hozzáadva a címtárban.
Távolítsa el a szolgáltatás egyszerű             | A szolgáltatás egyszerű eltávolítja a címtárban.
Szolgáltatás fő hitelesítő adatok hozzáadása    | A szolgáltatás egyszerű hozzáadott hitelesítő adatokat.
Távolítsa el a szolgáltatás fő hitelesítő adatok | Eltávolított hitelesítő adatok fő szolgáltatásból.
Meghatalmazás bejegyzés hozzáadása                 | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) létre.
Meghatalmazás-bejegyzés megadása                 | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) frissíteni.
Meghatalmazás bejegyzés törlése              | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) törli.
**Szerepkör-események**                      |
Szerepkör-szerepkör tag hozzáadása              | A felhasználó hozzáadni a címtár szerepkörhöz.
Szerepkör szerepkör tag eltávolítása         | Eltávolítja a felhasználó címtár szerepkörből.
Állítsa a kapcsolattartási adatai      | Vállalati szintű kapcsolattartási beállítások megadása Ide tartoznak a Microsoft Online Services marketing, valamint technikai értesítéseket e-mail címe.
Meghatalmazás bejegyzés hozzáadása                 | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) létre.
Meghatalmazás-bejegyzés megadása                 | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) frissíteni.
Meghatalmazás bejegyzés törlése              | A címtár- [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#OAuth2PermissionGrantEntity) törli.
AddSevicePrincipalOwner          | A felvett tulajdonosát, hogy a szolgáltatás egyszerű.
RemoveSevicePrincipalOwner   | Tulajdonos távolítva szolgáltatás egyszerű.
AddApplication  | Alkalmazás hozzáadása.
UpdateApplication | Alkalmazás frissítése Szeretné látni, hogy melyik alkalmazás beállításainak frissített, olvassa el az alábbi szakaszt az [Alkalmazás Tulajdonságok ellenőrzött](#update-application-attributes)
DeleteApplication | Alkalmazás törlése.
RestoreApplication|Visszaállíthatja az alkalmazást.
AddApplicationOwner   |     Tulajdonos hozzáadása alkalmazást.
RemoveApplicationOwner    |     Távolítsa el a tulajdonos alkalmazásból.
**Szerepkör-események**                         |
Szerepkör-szerepkör tag hozzáadása                 | A felhasználó hozzáadni a címtár szerepkörhöz.
Szerepkör szerepkör tag eltávolítása            | Eltávolítja a felhasználó címtár szerepkörből.
AddRoleDefinition               | A felvett szerepkör-definíció.
UpdateRoleDefinition                | Szerepkör-definíció frissíteni. Látható, hogy milyen szerepkör beállításainak frissített, olvassa el az alábbi szakaszt a [Szerepkör Definition tulajdonságok ellenőrzött](#update-role-definition-attributes)
DeleteRoleDefinition                | Szerepkör-definíció törli.
AddRoleAssignmentToRoleDefinition | Szerepkör-definíciós verziójának hozzáadott szerepkör-hozzárendelés.
RemoveRoleAssignmentFromRoleDefinition | Eltávolított szerepkör-hozzárendelés szerepkör definíciójához képest.
AddRoleFromTemplate     | A felvett szerepkör sablonból.
UpdateRole  | Frissített szerepkör.
AddRoleScopeMemberToRole    | A felvett hatókörű tag szerepkörhöz.
RemoveRoleScopedMemberFromRole  | Szerepkör hatókörű tag eltávolítja.
**Eszköz események (az összes új eseményre vonatkozóan)**                    |
AddDevice               | A felvett eszköz.
UpdateDevice            | Frissített eszköz. Szeretné látni, hogy milyen eszköz tulajdonságainak frissített, olvassa el [eszköz tulajdonságainak Audited](#update-device-attributes) az alábbi szakaszt
DeleteDevice            | A törölt eszközt.
AddDeviceConfiguration      | Konfigurációs hozzáadott eszköz.
UpdateDeviceConfiguration   | Frissített eszköz konfiguráció. Szeretné látni, hogy milyen eszköz beállításainak frissített, olvassa el [eszköz beállításainak Audited](#update-device-configuration-attributes) az alábbi szakaszt
DeleteDeviceConfiguration   | A törölt eszközök konfigurálása.
AddRegisteredOwner     | A felvett regisztrált tulajdonosa eszközre.
AddRegisteredUsers     | A felvett regisztrált felhasználói eszközre.
RemoveRegisteredOwner    | Regisztrált tulajdonosa eltávolítása az eszközről.
RemoveRegisteredUsers    | Regisztrált felhasználók eltávolítása az eszközről.
RemoveDeviceCredentials  | Távolítsa el a eszköz hitelesítő adatokat.
**B2B események**                       |
A köteg feltöltött felkérés.              | A rendszergazda feltöltötte el lehet küldeni a partner felhasználóknak szóló meghívók tartalmazó fájlt.
A köteg felkérés feldolgozása.             | Egy partner felhasználóknak szóló meghívók tartalmazó fájl feldolgozása.
Külső felhasználók meghívása.                | Egy külső felhasználó a címtárban meghívták.
Külső felhasználók meghívása a beváltás elemet.         | Egy külső felhasználónak van beváltott a meghívót a címtárhoz.
Külső felhasználók hozzáadása csoporthoz.          | Egy külső felhasználónak van rendelve tagsági csoport a címtárban.
Külső felhasználók hozzárendelése alkalmazást. | Külső felhasználók hozzárendelték közvetlen hozzáférés az alkalmazás.
Vírus bérlő létrehozása.               | Új bérlő a meghívó visszaváltás készült Azure Active Directory.
Vírus felhasználók létrehozása.                 | A felhasználó hozta egy meglévő bérlői webhelyen az Azure Active Directory a meghívó visszaváltás.
**Felügyeleti egységek (az összes új eseményre vonatkozóan)**             |
AddAdministrativeUnit   |   Adja hozzá a felügyeleti elemet.
UpdateAdministrativeUnit|   Frissítse a felügyeleti egység. Látható, hogy milyen rendszergazdai egység tulajdonságainak frissített, olvassa el az alábbi szakaszt a [rendszergazdai egység tulajdonságainak ellenőrzött](#update-administrative-unit-attributes)
DeleteAdministrativeUnit|   Felügyeleti egység törlése.
AddMemberToAdministrativeUnit|  Tag felvétele felügyeleti egység.
RemoveMemberFromAdministrativeUnit|     Tag eltávolítása felügyeleti egység.
**A címtár-események**                 |
Partner hozzáadása a vállalathoz               | Partner hozzáadása a címtárhoz.
Partner eltávolítása a vállalat          | Partner eltávolítása a címtárból.
DemotePartner   |   Partner lefokozása
Vállalati tartomány felvétele                | A címtárhoz vett fel tartományt.
Vállalati tartomány eltávolítása           | A tartomány eltávolítja a címtárban.
Tartomány frissítése                        | A könyvtár tartomány frissítése. Látható, hogy milyen tartomány tulajdonságai frissített, olvassa el az alábbi szakaszt a [tartomány tulajdonságai ellenőrzött](#update-domain-attributes)
Tartomány-hitelesítés beállítása            | Az alapértelmezett tartomány a vállalat változik.
Állítsa a kapcsolattartási adatai      | Vállalati szintű kapcsolattartási beállítások megadása Ide tartoznak a Microsoft Online Services marketing, valamint technikai értesítéseket e-mail címe.
Szövetséges tartományban beállításai    | Tartomány-összevonás beállításainak frissítése.
Tartomány tulajdonjogának igazolása                        | A könyvtár tartomány ellenőrizve.
E-mailek igazolt tartomány tulajdonjogának igazolása         | Az e-mail megerősítés használatával könyvtár tartomány ellenőrizve.
Vállalati DirSyncEnabled jelölő beállítása   | A tulajdonság, amely lehetővé teszi, hogy könyvtárában az Azure AD-szinkronizálás.
Jelszóházirend beállítása                  | Felhasználói jelszavak hossza és karakter korlátozásokkal beállítása.
Céginformáció beállítása              | Frissítette a vállalati szintű információkat. Lásd: a [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell-parancsmag további információt.
SetCompanyAllowedDataLocation  |    Set vállalati adatok helye engedélyezett.
SetCompanyDirSyncEnabled    |   Beállíthat DirSyncEnabled jelölőt.
SetCompanyDirSyncFeature |  Állítsa be a DirSync szolgáltatást.
SetCompanyInformation |   Céginformáció beállítása.
SetCompanyMultiNationalEnabled    |     Vállalati multinacionális szolgáltatás engedélyezve van beállítva.
SetDirectoryFeatureOnTenant   |     Állítsa be a címtár-szolgáltatás bérlői.
SetTenantLicenseProperties  |   Állítsa be bérlői licenc tulajdonságait.
CreateCompanySettings   |   Vállalati beállítások létrehozása
UpdateCompanySettings   |   Vállalati beállításainak frissítése. Látható, hogy milyen vállalati tulajdonságok lettek frissítve, olvassa el az alábbi szakaszt a [ellenőrzött vállalati tulajdonságai](#update-company-attributes)
DeleteCompanySettings   |   Vállalati beállítások törlése
SetAccidentalDeletionThreshold   |  Állítsa a véletlen törlését küszöbértéknél.
SetRightsManagementProperties   |   Állítsa be a rights management tulajdonságait.
PurgeRightsManagementProperties     |   Véglegesen rights management tulajdonságait.
UpdateExternalSecrets   |   Külső titkos kulcsok frissítése
**Házirend-események (az összes új eseményre vonatkozóan)**           |
AddPolicy   |   Házirend hozzáadása.
UpdatePolicy    |   Frissítse a házirend.
DeletePolicy    |   Szabály törlése
AddDefaultPolicyApplication     |   Házirend hozzáadása alkalmazást.
AddDefaultPolicyServicePrincipal    |   Házirend hozzáadása szolgáltatás egyszerű.
RemoveDefaultPolicyApplication  |   Távolítsa el a házirend alkalmazásból.
RemoveDefaultPolicyServicePrincipal     |   Házirend eltávolítása szolgáltatás egyszerű.
RemovePolicyCredentials     |   Távolítsa el a házirend hitelesítő adatokat.

## <a name="audit-report-retention"></a>Naplózási jelentések adatmegőrzési
Az Azure Active Directory naplójelentés eseményeinek 180 napig megőrződnek. Jelentésekben adatmegőrzési kapcsolatos további tudnivalókért olvassa el az [Azure Active Directory jelentés adatmegőrzési szabályok](active-directory-reporting-retention.md)című témakört.

Az ügyfeleknek tárolja azok események adatmegőrzési hosszabb ideig érdekli a jelentéskészítő API rendszeresen csoportosítani naplózása be külön adattárban használható. Olvassa el az [Ismerkedés a jelentési API-val](active-directory-reporting-api-getting-started.md) további információt.

## <a name="properties-included-with-each-audit-event"></a>Egyes naplózási események részét képező tulajdonságai

A tulajdonság      | Leírás
------------- | --------------------------------------------------------------
Dátum és idő | A dátum és időpont, amikor a naplózási esemény történt
Szereplő         | A felhasználó vagy a szolgáltatás egyszerű tárolón végrehajtott művelet
Művelet        | Végrehajtott művelet
Cél        | A felhasználó vagy a szolgáltatás egyszerű végrehajtott művelet a


## <a name="update-user-attributes"></a>"Felhasználó módosítása" attribútumok
A "Frissítés felhasználó" naplózási esemény milyen felhasználói attribútumok lettek frissítve további információkat tartalmaz. Az egyes attribútumok a korábbi és új értéke is megtalálható.

Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled                      | A felhasználó jelentkezhetnek be.
AssignedLicense                     | A felhasználóhoz rendelt összes licenceket.
AssignedPlan                        | A szolgáltatás tervek a licenceket, a felhasználó eredő.
LicenseAssignmentDetail             | Részletes tudnivalókat a felhasználóhoz rendelt licencek. Például ha csoport alapú licencelése szerepet, ez tartalmazza a csoportot, amelyhez a licenc nyújtani.
Mobil                              | A felhasználó mobiltelefonra.
OtherMail                           | A felhasználó másodlagos e-mail címet.
OtherMobile                         | A felhasználó a mobiltelefon alternatív.
StrongAuthenticationMethod          | Be van állítva a felhasználó által többtényezős hitelesítés, például a hangposta hívása, SMS vagy ellenőrzési kód mobilalkalmazásban hitelesítési módszerek listája.
StrongAuthenticationRequirement     | Többtényezős hitelesítés érvényesül, ha engedélyezve van, vagy a felhasználó letiltotta.
StrongAuthenticationUserDetails     | A felhasználó telefonszámának helyettesítő telefonszám és e-mail címe, többtényezős hitelesítés és a jelszó alaphelyzetbe állítása az ellenőrzéshez használt.
StrongAuthenticationPhoneAppDetail  | Részletes forof telefonos alkalmazások regisztrált 2FA bejelentkezési végrehajtásához
TelephoneNumber                     | A felhasználó telefonszáma.
AlternativeSecurityId               | Az objektum alternatív biztonsági azonosító.
CreationType                        |A felhasználó (akár meghívót, illetve vírus) létrehozásának módja
InviteTicket                        |A felhasználó meghívó jegyek listája.
InviteReplyUrl                      |Szeretne válaszolni a meghívást, URL-címek listája.
InviteResources                     |Erőforrások, amelyhez a felhasználó meghívták listája.
LastDirSyncTime                     |Az objektum frissült miatt szinkronizálásának a mérvadó a legutóbbi címtár (ügyfél, helyszíni).
MSExchRemoteRecipientType           |MSO címzett típus van hozzárendelve. Címzett típusnál [MSO címzett típusok] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx hivatkozik
PreferredDataLocation               |Az elsődleges helye a felhasználó, a csoport, a partner, a nyilvános mappa vagy eszközöm adatok.
ProxyAddresses                      |A cím, amelynek címzett Exchange Server-objektum ismerhető fel az idegen levelezőrendszert.
StsRefreshTokensValidFrom           |Végzi jogkivonat visszavonási információit frissítése – bármely STS frissítés tokenek előtt ezúttal ilyenként kiállított lejárt.
UserPrincipalName                   |Hogy a felhasználó interneten használt bejelentkezési név (UPN).
UserState                           |Felhasználói jóváhagyásra vár/PendingAcceptance/elfogadása/PendingVerification állapotát.
UserStateChangedOn                  |Az utolsó módosítás UserState időbélyeg. Mezőkóddal életciklus-munkafolyamatok.
UserType                            |Felhasználó típusa Tag (0), a vendégként való bekapcsolódáshoz (1), Viral(2).

## <a name="update-group-attributes"></a>"Frissítés csoport" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Besorolás            | Az osztályozás egységesített csoport (HBI, MBI stb.).
Leírás               | Olvasható egy leíró kifejezést az objektumról.  
DisplayName               |Az objektum a megjelenítendő név
DirSyncEnabled            |Azt jelzi, hogy a szinkronizálást a mérvadó a címtárban (ügyfél, helyszíni).
GroupLicenseAssignment    |Licenc hozzárendelése egy
GroupType                 |Csoport típusát, egyesített (0)
IsMembershipRuleLocked    |Azt jelzi, hogy a MembershipRule tulajdonság a csoport önkiszolgáló szolgáltatás beállítása a felhasználók nem módosíthatók. Csak a csoportok, amelyek a GroupType tartalmaz GroupType.DynamicMembership alkalmazható
IsPublic                  |Jelző, ha a csoport nyilvános/magánjellegű-e.
LastDirSyncTime           |Az objektum frissült eredményeként szinkronizálása a mérvadó a legutóbbi címtár (ügyfél, helyszíni környezetbe).
Levelek                      |Elsődleges e-mail címe
MailEnabled               |Azt jelzi, hogy a csoport e-mail lehetőséget.
Mailnickname-beli              |Cím könyv objektum moniker, általában a megelőző e-mail nevét azon részét a "@" szimbólumot.   
MembershipRule            |A feltétel határozza meg, mely tagjai kell ebben a csoportban a csoport önkiszolgáló alkalmazáskezelési szolgáltatás által használt kifejező karakterlánc. Lásd még: IsMembershipRuleLocked. Alkalmazandó csak a csoportok, amelyek a GroupType GroupType.DynamicMembership tartalmaz.
MembershipRuleProcessingState| Egy felsorolási érték feldolgozása a csoport tagsága állapotát definiáló önkiszolgáló csoport management szolgáltatás határozza meg. Alkalmazandó csak a csoportok, amelyek a GroupType GroupType.DynamicMembership tartalmaz.
ProxyAddresses            |A cím, amelynek címzett Exchange Server-objektum ismerhető fel az idegen levelezőrendszert.
RenewedDateTime           |Amikor megújítani legutóbb csoport időbélyeg rekordja.   
SecurityEnabled           |Azt jelzi, hogy a csoport tagsága befolyásolhatják engedélyezési döntéseket.
WellKnownObject           |A címtár-objektumra, előre definiált egyikeként a kijelölő címkék.

## <a name="update-device-attributes"></a>"Eszköz frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled | Azt jelzi, hogy a rendszerbiztonsági tag hitelesíthet.
CloudAccountEnabled | Azt jelzi, hogy a rendszerbiztonsági tag hitelesítheti. Ha az eszköz a helyszíni környezetbe lezárt van fájlrendszer csapatátó InTune.
CloudDeviceOSType | Az eszköz típusa alapján az operációs rendszer pl. Windows RT rendszerben, iOS. Ha értéke szerint (például az Intune), az attribútum válik a címtárban mérvadó DeviceOSType.
CloudDeviceOSVersion | Az operációs rendszer verziója. Ha értéke szerint (például az Intune), az attribútum válik a címtárban mérvadó DeviceOSVersion.
CloudDisplayName | A displayName LDAP attribútum értéke. Ha értéke szerint (például az Intune), az attribútum lesz mérvadó displayName a címtárban.
CloudCreated |Azt jelzi, hogy az objektum felhőszolgáltatások hozta-e.
CompliantUntil | Milyen időpontig eszköz kompatibilis tekinteni.
DeviceMetadata |Az eszköz egyéni metaadatok
DeviceObjectVersion | Az attribútum azonosítja az eszköz a séma verzióját használják.
DeviceOSType |Az eszköz típusa alapján az operációs rendszer pl. Windows RT rendszerben, iOS. A regisztráció szolgáltatás által írt és frissítenie kell a MDM szánt alkalmazáskezelési szolgáltatás vagy STS világos alkalmazáskezelési szolgáltatás.
DeviceOSVersion |Az operációs rendszer verziója. A regisztráció szolgáltatás által írt és frissítenie kell a MDM szánt alkalmazáskezelési szolgáltatás vagy STS világos alkalmazáskezelési szolgáltatás.
DevicePhysicalIds |Többértékű attribútum azonosítóját, a fizikai eszközök tárolására szolgál. Ez kiterjedhet BIOS azonosítók, TPM thumbprints, a hardver egyedi azonosítókat stb.
DirSyncEnabled | Azt jelzi, hogy a szinkronizálást a egy mérvadó (ügyfél, helyszíni környezetbe) a címtárban.
DisplayName |Az objektum a megjelenítendő név
IsCompliant | Az attribútum mobileszköz management állapotát az eszköz kezelésére szolgál.
IsManaged |Az attribútum használatos annak jelzésére, hogy az eszköz kezeli a felhőben-ben.
LastDirSyncTime |Az objektum frissült miatt szinkronizálásának a a mérvadó legutóbbi címtár (ügyfél, helyszíni környezetbe).

## <a name="update-device-configuration-attributes"></a>"Eszközök konfigurálása frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
MaximumRegistrationInactivityPeriod |Lehet, hogy egy eszközt inaktív előtte napok maximális száma eltávolítása számít.
RegistrationQuota   |Egy felhasználó számára engedélyezett eszköz regisztrációk számának korlátozása használt házirend.

## <a name="update-service-principal-configuration-attributes"></a>"A szolgáltatás fő konfigurációja frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AccountEnabled |Azt jelzi, hogy a rendszerbiztonsági tag hitelesíthet.
AppPrincipalId | Külső, az alkalmazás által definiált identitás rendszerbiztonsági tag.
DisplayName |Az objektum a megjelenítendő név
ServicePrincipalName    | A szolgáltatás egyszerű nevét tartalmazó "név/hitelesítésszolgáltató" hol nevét adja meg az alkalmazás class értéket, és a hitelesítésszolgáltató tartalmaz legalább hostname (állomásnév) [: port] vagy a "név", amely azonosítót adja meg a szolgáltatás egyszerű.

## <a name="update-app-attributes"></a>"Alkalmazás frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |A címek (átirányíthatja az URL-ek) egy egyszerű rendelt készlete.
AppId                               |Az alkalmazás azonosítója   
AppIdentifierUri | Alkalmazás URI, amely azonosítja az adott alkalmazást.  Érdemes általában az alkalmazás hozzáférést URL-címet.
AppLogoUrl |Az a CDN tárolt alkalmazás embléma URL-címét.
AvailableToOtherTenants | IGAZ, az alkalmazás egy több bérlői alkalmazás (azaz használható más bérlők által).
DisplayName | Az alkalmazás nevét a megjelenítendő neve
Jogosultság |Alkalmazás jogosultságok listája.
ExternalUserAccountDelegationsAllowed |A jelző jelzi, hogy az erőforrás-alkalmazás megbízható egy és külső felhasználói fiókok meghatalmazás bejegyzéseket hozhat létre.
GroupMembershipClaims |A csoporttagság követelések házirend.
PublicClient | IGAZ, ha az ügyfélnek nem titkos (tehát nem bizalmas ügyfél OAuth2.0 a)
RecordConsentConditions | Felhasználó hozzájárul ahhoz feltételt, a szerződés feltételeit által meghatározott típusú: None (0), SilentConsentForPartnerManagedApp(1). Az érték lesz a diagram API-séma elérhetővé kell, és lehet, hogy csak a bérlői rendszergazdák által beállított/megváltozott.
RequiredResourceAccess |XML-tartalom RequiredResourceAccess tulajdonság értéke.
Webalkalmazás |IGAZ, ha azt jelzi, hogy az alkalmazás webalkalmazást.
WwwHomepage |Az elsődleges weblapot.

## <a name="update-role-attributes"></a>"Szerepkör frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AppAddress |A címek (átirányíthatja az URL-ek) egy egyszerű rendelt készlete.
BelongsToFirstLoginObjectSet |IGAZ, ha azt jelzi, hogy az objektum tartozik a szükséges ahhoz, hogy jelentkezzen be az első az új bérlő rendszergazdájának objektumok csoportja.
Beépített |Azt jelzi, hogy az objektum élettartama tulajdonában van-e a rendszer.
Leírás | Olvasható egy leíró kifejezést az objektumról.  
DisplayName |Az objektum a megjelenítendő név
Mailnickname-beli | Cím könyv objektum moniker, általában a megelőző e-mail nevét azon részét a "@" szimbólumot.
RoleDisabled |Azt jelzi, hogy a szerepkör figyelmen kívül hagyja az access-ellenőrzés céljából.
RoleTemplateId | Szerepkör-sablon identitás.
ServiceInfo |Szolgáltatás-specifikus kiépítési információkat is MOAC és/vagy más szolgáltatás példányainak (milyen típusú ugyanazon vagy más szolgáltatás) által igénybe vehető.
TaskSetScopeReference |Egy TaskSet, szerepkör vagy RoleTemplate társított hatókörök halmazának azonosítja.
ValidationError |A Tulajdonságok vagy objektum rendszergazda művelet feloldásához hivatkozást egy nem tranziens, szolgáltatásspecifikus hiba leíró szövetséges szolgáltatás által közzétett információkat.
WellKnownObject |A címtár-objektumra, előre definiált egyikeként a kijelölő címkék.

## <a name="update-role-definition-attributes"></a>"Szerepkör-definíció frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AssignableScopes    |A RoleDefinition hozzárendelése a rendszerbiztonsági tag hivatkozhat engedélyezési hatókörök gyűjteménye.
DisplayName     |Az objektum a megjelenítendő név
GrantedPermissions  |Ez a RoleDefinition engedélyeket.


## <a name="update-administrative-unit-attributes"></a>"Frissítése felügyeleti egység" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Leírás |   Ez a tulajdonság frissítésekor egy felügyeleti egység leírásának a módosítása.
DisplayName |   Ez a tulajdonság frissítésekor egy felügyeleti egység nevének módosítása.

## <a name="update-company-attributes"></a>"Frissítés vállalat" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
AllowedDataLocation     | Egy helyet, ahol a vállalat engedélyezett a felhasználók számára kell építenie.
AuthorizedServiceInstance   | Szolgáltatás-példányok, amelyhez terv telepíthető nevét.
DirSyncEnabled |Azt jelzi, hogy a szinkronizálást a egy mérvadó (ügyfél, helyszíni környezetbe) a címtárban.
DirSyncStatus |Azt jelzi, hogy a cím könyv objektumok a bérlői környezetben szinkronizálást az mérvadó (ügyfele, helyszíni környezetbe) a címtár; a vállalat objektumok DirSyncEnabled tulajdonság kiterjesztését.
DirSyncFeatures |Ha nyomon szeretné követni a bérlői webhelyen engedélyezett és a letiltott dirsync funkciók beállítása jelző bit.
DirectoryFeatures |Engedélyezett/letiltott directory szolgáltatásainak.
DirSyncConfiguration |Az aktuális bérlőjére tölti konfigurálása tartalmazza.
DisplayName |Az objektum a megjelenítendő név
IsMnc |Egy logikai jelzővel állítsa a vállalat engedélyezve van a vállalati multinacionális funkció "igaz" iff.
ObjectSettings | Az objektum hatókörének vonatkozó beállítások gyűjteménye.
PartnerCommerceUrl |A Partner kereskedelmi webhely URL-címe.
PartnerHelpUrl |A Partner súgójának webhelye URL-címe.
PartnerSupportEmail | A Partner e-mailek támogatási URL-címe.
PartnerSupportTelephone |A Partner támogatás telefonszámát URL-címe.
PartnerSupportUrl |A Partner támogatási webhely URL-címe.
StrongAuthenticationDetails |StrongAuthentication kapcsolatos részleteket.
StrongAuthenticationPolicy |A vállalat házirendjének erős hitelesítés.
TechnicalNotificationMail |E-Mail cím értesíti a vállalat kapcsolatos műszaki problémák.
TelephoneNumber |Telefonszámok, amelyek megfelelnek a ITU ajánlási E.123.
TenantType |A bérlő típusát. Ez az érték nincs megadva, a bérlő esetén a vállalat. Egyéb esetben lehetséges értékei a következők: MicrosoftSupport (0), SyndicatePartner (1), (2) BreadthPartner BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5).
VerifiedDomain |A vállalat kötött DNS-tartománynevek csoportja.

## <a name="update-domain-attributes"></a>"A tartomány frissítése" attribútumok
Attribútum                           | Leírás
-------------------------------     | -------------------------------------------------------------------------------------------------------------------------------------------------------
Funkciók    |Bites jelölők azt mutatja be, a tartomány lehetőségeit, ha vannak ilyenek.
Alapértelmezett |Azt jelzi, hogy a tartomány az alapértelmezett érték; Ha például az alapértelmezett UserPrincipalName utótag, amikor egy rendszergazda hoz létre új felhasználót MOAC.
Kezdeti |Azt jelzi, hogy a tartományt az eredeti tartománynak a vállalat által OCP. Az eredeti tartománynak a Microsoft Online tartományt; egyedi alszint tartomány e.g.contoso3.microsoftonline.com.
LiveType    |Írja be a megfelelő Windows Live névtér, ha van ilyen.
név    | Az endpoint azonosítója.
PasswordNotificationWindowDays  |Értesítést kap egy jelszót a felhasználó lejárta előtt napok számát.
PasswordValidityPeriodDays  | Jelszó kiválóan alkalmas előtt meg kell változtatni napok számát.

Naplóbejegyzések szükséges vezérlő, sok megfelelőségi szabályok. Az Azure Active Directory Audit jelentés segítségével felel meg a megfelelőségi előírásokat ügyfeleknek javasolt, hogy az ügyfél elküldése az ügyfélnek exportált naplójelentés magyarázattal a jelentés részleteit a példányát a súgótémakörben másolatának. Ha szeretné, hogy a vizsgáló a megfelelőségi szabályzat sablonnyilakra Azure jelenleg megértéséhez, közvetlenül a a Microsoft Azure Trust Center [megfelelőségi lap](https://azure.microsoft.com/support/trust-center/compliance/) vizsgáló.
