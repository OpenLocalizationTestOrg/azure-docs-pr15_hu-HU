<properties
   pageTitle="Azure AD Connect: Az automatikus frissítés |} Microsoft Azure"
   description="Ez a témakör ismerteti az Azure AD Connect szinkronizálás beépített automatikus frissítése szolgáltatása."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/24/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Az automatikus frissítés
Ez a funkció az összeállítás 1.1.105.0 (kiadott február 2016) jelent meg.

## <a name="overview"></a>– Áttekintés
Gondoskodhat arról, hogy a Azure AD Connect telepítés értéke mindig naprakész eddiginél egyszerűbben az **Automatikus frissítés** funkciót. Ez a funkció a sürgős telepítéseihez alapértelmezés szerint engedélyezve van, és a DirSync frissítések. Új verzió megjelenésekor-telepítés frissítése automatikusan történik.

Az automatikus frissítés alapértelmezés szerint bekapcsolt meg az alábbiakról:

- Express-beállítások telepítési és a DirSync frissítések.
- SQL-Express LocalDB használ, akkor mit Express beállítások mindig használata. Az SQL Express DirSync a LocalDB is használhatja.
- Az Active Directory fiókot kell alapértelmezett MSOL_ létrehozott Express beállításával és a DirSync.
- A metaverse legfeljebb 100 000 objektumok rendelkezik.

Az automatikus frissítés aktuális állapotát megtekintheti az PowerShell-parancsmag a `Get-ADSyncAutoUpgrade`. A következő államok van:

Állam | Megjegyzés
---- | ----
Engedélyezve van | Az automatikus frissítés érhető el.
Felfüggesztett | Adja meg csak a rendszer. A rendszer már nem jogosult a automatikus verziófrissítése.
Letiltott | Az automatikus frissítés le van tiltva.

Módosíthatja, hogy **engedélyezett** és a **letiltott** a közötti `Set-ADSyncAutoUpgrade`. Csak a rendszer az állapot **Felfüggesztve**kell állítani.

Az automatikus frissítés Azure Active Directory csatlakozás állapota a frissítési infrastruktúrájának használ. Az automatikus frissítés a munkát feltétlenül megnyitott URL-címét a proxykiszolgáló az **Azure Active Directory csatlakozás állapota** [az Office 365 URL-EK és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)ismertetett módon.

Ha a felhasználói felület **Szinkronizálási szolgáltatáskezelő** a kiszolgálón fut, majd a frissítés fel kell függeszteni a felhasználói felület bezárásáig.

## <a name="troubleshooting"></a>Hibaelhárítás
A csatlakozás telepítés nem frissíti magát meg a várt módon, kövesse ezeket a lépéseket követve megtudhatja, mit lehet rossz.

Első lépésként az első nap kiadott egy új verzió kísérli automatikus frissítés nem várt. Van egy szándékos véletlenszerűséget frissítésre van megpróbálkozik vele a rendszer, így nem kell alarmed, ha a telepítés azonnal nem frissített előtt.

Ha úgy gondolja, hogy valamit, amit nem megfelelő, először futtassa `Get-ADSyncAutoUpgrade` annak érdekében, hogy az automatikus frissítés engedélyezve van.

Ellenőrizze, nyitotta meg a szükséges URL-címek a proxykiszolgáló vagy a tűzfalat. Az automatikus frissítés Azure Active Directory csatlakozás állapot használ az [Áttekintés](#overview)leírt módon. Ha használ proxykiszolgálót, feltétlenül egészségügyi [proxykiszolgáló](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)használatára van beállítva. Azure Active Directory és a [kapcsolat állapota](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) is tesztelje.

Az ellenőrzött Azure AD a kapcsolatot pedig vizsgálja meg a eventlogs a. Nyissa meg az Eseménynapló nevet, és nézze meg az **alkalmazás** Eseménynapló. Adja meg az Eseménynapló szűrő **Azure Active Directory csatlakozás frissítése a** forrást és az esemény azonosító tartomány **300-399**.  
![Az automatikus frissítés Eseménynapló szűrő](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogfilter.png)  

Ekkor megjelenik az automatikus frissítés állapotának társított eventlogs.  
![Az automatikus frissítés Eseménynapló szűrő](./media/active-directory-aadconnect-feature-automatic-upgrade/eventlogresult.png)  

Az eredmény kód áttekintheti az állam előtaggal tartalmaz.

Eredmény kód előtag | Leírás
--- | ---
A siker | A telepítés sikeresen frissült.
UpgradeAborted | Ideiglenes feltétel a frissítés le. Azt megpróbálja újra, és az elvárásoknak, hogy később tud-e.
UpgradeNotSupported | A rendszer a konfiguráció, amely a rendszer automatikusan frissítendő zárolja tartalmaz. Azt megpróbálja tekintheti meg, ha az állapot változik, de az elvárásoknak, hogy a rendszer kézzel kell frissíteni.

Az alábbiakban megtalálja a leggyakoribb üzenetek. Nem szerepel az összes, de az eredmény üzenet kell lennie, törölje a jelet a Mi a probléma van.

Eredményének üzenetben | Leírás
--- | ---
**UpgradeAborted** |
UpgradeAbortedCouldNotSetUpgradeMarker | Nem sikerült a beállításjegyzék írni.
UpgradeAbortedInsufficientDatabasePermissions | A beépített Rendszergazdák csoport nincs engedélye az adatbázishoz. Manuális frissítése az Azure AD Connect a probléma megoldására legújabb verzióját.
UpgradeAbortedInsufficientDiskSpace | Nincs elegendő lemezterülettel frissítésének támogatása.
UpgradeAbortedSecurityGroupsNotPresent | Nem keresse meg és nem megoldani a szinkronizálási motor által használt összes biztonsági csoportokat.
UpgradeAbortedServiceCanNotBeStarted | A NT szolgáltatás a **Microsoft Azure AD-szinkronizálás** nem indult el.
UpgradeAbortedServiceCanNotBeStopped | A NT szolgáltatás a **Microsoft Azure AD-szinkronizálás** leállítása sikertelen volt.
UpgradeAbortedServiceIsNotRunning | Nem működik a NT szolgáltatás a **Microsoft Azure AD-szinkronizálás** .
UpgradeAbortedSyncCycleDisabled | A SyncCycle lehetőség az [Ütemező](active-directory-aadconnectsync-feature-scheduler.md) letiltását.
UpgradeAbortedSyncExeInUse | A [szinkronizálás szolgáltatáskezelő felhasználói felület](active-directory-aadconnectsync-service-manager-ui.md) meg nyitva, a kiszolgálón.
UpgradeAbortedSyncOrConfigurationInProgress | A telepítés varázslót fut, vagy a szinkronizálást a ütemezett kívül az ütemezőt.
**UpgradeNotSupported** |
UpgradeNotSupportedCustomizedSyncRules | A saját egyéni szabályok hozzáadta a konfiguráción.
UpgradeNotSupportedDeviceWritebackEnabled | Az [eszköz visszaírást](active-directory-aadconnect-feature-device-writeback.md) funkció engedélyezte.
UpgradeNotSupportedGroupWritebackEnabled | A [csoport visszaírást](active-directory-aadconnect-feature-preview.md#group-writeback) funkció engedélyezte.
UpgradeNotSupportedInvalidPersistedState | A telepítés még nem egy Express beállításai vagy a DirSync frissítése.
UpgradeNotSupportedMetaverseSizeExceeeded | A metaverse akkor több mint 100 000 objektumokat.
UpgradeNotSupportedMultiForestSetup | Egynél több erdő csatlakozik. Gyors telepítés csak egy erdő csatlakozik.
UpgradeNotSupportedNonLocalDbInstall | Az SQL Server Express LocalDB adatbázis nem használ.
UpgradeNotSupportedNonMsolAccount | Az [Active Directory-összekötő fiók](active-directory-aadconnect-accounts-permissions.md#active-directory-account) nem az alapértelmezett MSOL_ fiók eltűnt.
UpgradeNotSupportedStagingModeEnabled | A kiszolgáló [átmeneti tárolására módot](active-directory-aadconnectsync-operations.md#staging-mode)kell van beállítva.
UpgradeNotSupportedUserWritebackEnabled | A [felhasználó visszaírást](active-directory-aadconnect-feature-preview.md#user-writeback) funkció engedélyezte.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
