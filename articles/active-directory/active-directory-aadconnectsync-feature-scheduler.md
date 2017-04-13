<properties
   pageTitle="Azure AD Connect szinkronizálása: a Feladatütemező |} Microsoft Azure"
   description="Ez a témakör ismerteti a beépített ütemező funkció Azure AD Connect szinkronizálás."
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
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect szinkronizálása: a Feladatütemező
Ez a témakör ismerteti az Azure AD Connect szinkronizálás beépített ütemezőt (más néven szinkronizálási motor).

Ez a funkció az összeállítás 1.1.105.0 (kiadott február 2016) jelent meg.

## <a name="overview"></a>– Áttekintés
Azure AD Connect szinkronizálási szinkronizálja a módosításokat a helyszíni címtárában a Feladatütemező használatával történik. Vannak két ütemező folyamatot, egy jelszó-szinkronizálás, és egy másik objektum/attribútum szinkronizálási és karbantartási műveleteket. Ez a témakör az utóbbi terjed ki.

A korábbi verziókban objektumok és attribútumok ütemezője volt a szinkronizálási motor külső, és a Windows Feladatütemező vagy külön Windows szolgáltatás használt indíthatja el a szinkronizálást. Az ütemező meg, a 1.1-es verziókban beépített a szinkronizálandó motor, és lehetővé teszik az egyes testreszabását. Az új alapértelmezett szinkronizálási gyakoriság 30 percig.

Az ütemező a két feladat felelős:

- A **szinkronizálás ciklus**. A folyamat szeretné importálni, szinkronizálása és exportálása a módosításokat.
- **Karbantartási műveleteket**. Újítsa meg kulcsok és tanúsítványok jelszó alaphelyzetbe állítása és eszköz regisztrációs szolgáltatás (DRS). A műveletek naplóban régi bejegyzéseket törlésére.

Az ütemező magát mindig fut, de csak futtatásához egy vagy nincs az alábbi műveleteket is beállítható. Ha például saját szinkronizálási ciklus folyamatban van szükség, ha ezt a feladatot a Feladatütemező az letiltása, de továbbra is az a karbantartás feladat futtatása.

## <a name="scheduler-configuration"></a>Feladatütemező beállítása
Az aktuális beállítások megtekintéséhez nyissa meg a PowerShell és a Futtatás `Get-ADSyncScheduler`. Meg kell követnie alábbihoz hasonló:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Ha megjelenik **a szinkronizálás parancsot, vagy a parancsmag nem érhető el** Ez a parancsmag futtatásakor, a PowerShell-modult nem betöltött. Ha Azure AD Connect egy tartományvezérlőnek vagy PowerShell korlátozás magasabb szintű, mint az alapértelmezett beállítások a kiszolgálón ennek. Ha ez a hiba jelenik meg, majd futtassa `Import-Module ADSync` elérhetővé teheti a parancsmag.

- **AllowedSyncCycleInterval**. Azure AD a leggyakrabban előforduló szinkronizálási lehetővé teszi. Nem tudja szinkronizálni a gyakrabban ez, és továbbra is támogatott.
- **CurrentlyEffectiveSyncCycleInterval**. Az ütemezés jelenleg az effektust. Értéke megegyezik CustomizedSyncInterval lesz (Ha a beállítása) Ha még nem gyakrabban AllowedSyncInterval. Ha CustomizedSyncCycleInterval módosításához ez után lépnek életbe következő szinkronizálási ciklus.
- **CustomizedSyncCycleInterval**. Ha azt szeretné, hogy az gyakorisággal bármely más az alapértelmezett futtatásához 30 percig ütemezőt, konfigurálja ezt a beállítást. A kép felett az ütemező van beállítva óránként futni. Ha ez egy érték kisebb, mint AllowedSyncInterval, az utóbbi lesz.
- **NextSyncCyclePolicyType**. Delta vagy kezdőbetűje. Határozza meg, hogy a következő futtatás kell csak a folyamat delta módosításához, vagy ha a következő futtatás végezze el a teljes importálása és szinkronizálása, amelyek volna is újbóli feldolgozás új vagy módosított szabályokat.
- **NextSyncCycleStartTimeInUTC**. Következő alkalommal az ütemező indul el a következő szinkronizálási ciklus.
- **PurgeRunHistoryInterval**. A művelet naplók őrizze időt. Ezek a szinkronizálás szolgáltatáskezelő az aláírandó elemen. Az alapértelmezett érték, hogy ezek 7 napig.
- **SyncCycleEnabled**. Azt jelzi, hogy az Ütemező fut az importálás, a szinkronizálási és az exportálási folyamat működését részeként.
- **MaintenanceEnabled**. Ha engedélyezve van-e a karbantartás folyamat megjelenítése Azt tanúsítványok kulcsok frissítése és törlése a műveletek naplót.
- **IsStagingModeEnabled**. Látható, ha engedélyezve van-e a [átmeneti tárolására módot](active-directory-aadconnectsync-operations.md#staging-mode) .

Módosíthatja az egyes beállításokat a `Set-ADSyncScheduler`. A következő paraméterek módosíthatók:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Azure AD a Feladatütemező beállítása vannak tárolva. Ha egy fejlesztői kiszolgálóra, a bármilyen módosítást, az elsődleges kiszolgálón is hatással a fejlesztői kiszolgálóhoz (kivételével IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Szintaxis:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - napok HH - Óra, mm - perc, ss – másodpercig

Példa:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Az ütemező 3 óránként futtatásához változtatja.

Példa:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Módosítja a Feladatütemező naponta futnak.

## <a name="start-the-scheduler"></a>Indítsa el az ütemező
Az ütemező 30 percenként alapértelmezés szerint futnak. Bizonyos esetekben célszerű legyen a ütemezett ciklus verziókövetéssel egy szinkronizálási futtatásához, vagy egy másik típusú futtatásához szükséges.

**Delta szinkronizálási ciklus**  
A delta szinkronizálási ciklus a következő lépésekből áll:

- A minden összekötők delta importálása
- Összekötők delta szinkronizálás
- A minden összekötők exportálása

Elképzelhető, hogy egy sürgős módosítása, amely szinkronizálni kell azonnal ezért ciklust manuálisan futtatásához szükséges. Ha manuálisan futtatásához szükséges ciklust, majd a Futtatás PowerShell `Start-ADSyncSyncCycle -PolicyType Delta`.

**Teljes szinkronizálási ciklus**  
Ha a következő módosításokat tettek, a teljes szinkronizálási ciklust (más néven futtatásához szükséges Kezdeti):

- További objektumok vagy egy adatforrás címtárból importálni kívánt attribútumok hozzáadása
- A szinkronizálási szabályok végrehajtott módosítások
- Megváltozott a [Szűrés](active-directory-aadconnectsync-configure-filtering.md) különféle objektumok szerepeljenek

Ha az alábbi módosításokat tettek, majd futtatásához szükséges teljes szinkronizálási verziókövetéssel egy, a szinkronizálási motor forrásterületekről összesíteni szeretnénk az összekötő térközök lehetőséget tartalmazza. Egy teljes szinkronizálási ciklus a következő lépésekből áll:

- Összekötők a teljes importálása
- Összekötők teljes szinkronizálás
- A minden összekötők exportálása

Futtassa a verziókövetéssel egy teljes szinkronizálás kezdeményezéséhez `Start-ADSyncSyncCycle -PolicyType Initial` a PowerShell-parancssorában. Ez a teljes szinkronizálási ciklus indítja el.

## <a name="stop-the-scheduler"></a>Az Ütemező leállítása
Ha az ütemező le kell szinkronizálási verziókövetéssel egy éppen zajló értekezletet. Ha például a telepítés varázsló indítása és a következő hibaüzenet:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

A szinkronizálási ciklust fut, nem lehet konfigurációs módosítani. Akkor is várnia, amíg a az ütemező a folyamat befejeződik, de meg is szüntetheti meg, végezze el a változtatásokat azonnal. Az aktuális ciklus leállítása nem kártékony és a következő futtatás feldolgozás továbbra is a feldolgozása nem módosításokat.

1. Kezdje azzal, hogy le szeretné állítani az aktuális ciklus PowerShell-parancsmag a az ütemező jelzi `Stop-ADSyncSyncCycle`.
2. Leállítása az ütemező nem le az aktuális tevékenységről az aktuális összekötőt. Hogy le szeretné állítani az összekötő, az alábbi műveletek: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Indítsa el a **Szinkronizálását bemutató szolgáltatást** a start menüből. Nyissa meg az **összekötőket**, jelölje ki az összekötőt az **operációs rendszert futtató** állapotát, és jelölje ki **leállítása** a műveletek.

Az ütemező továbbra is aktív, és újra kezdése tovább lehetőséget.

## <a name="custom-scheduler"></a>Egyéni ütemező
Az ebben a szakaszban ismertetett parancsmag csak rendelkezésre állnak az összeállítás [1.1.130.0](active-directory-aadconnect-version-history.md#111300) és újabb.

Ha a beépített ütemező nem felel meg a, majd ütemezheti az összekötők PowerShell használatával.

### <a name="invoke-adsyncrunprofile"></a>Meghívása ADSyncRunProfile
A profil ilyen összekötő indítható el:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

A nevek [összekötő](active-directory-aadconnectsync-service-manager-ui-connectors.md) és [Futtatása profil nevét](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) a [Szinkronizálás szolgáltatáskezelő felhasználói felület](active-directory-aadconnectsync-service-manager-ui.md)találhatók.

![Futási profil meghívása](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

A `Invoke-ADSyncRunProfile` parancsmag szinkron, azaz nem ad vissza vezérlő mindaddig, amíg az összekötő befejeződött a művelet sikeresen vagy hibát.

Az összekötők ütemezésekor az ajánlási egyike ütemezése őket a következő sorrendben:

1. (Teljes/Delta) A helyszíni könyvtárak, például az Active Directory importálása
2. (Teljes/Delta) Importálás az Azure Active Directory
3. (Teljes/Delta) A helyszíni könyvtárak, például az Active Directory-szinkronizálás
4. (Teljes/Delta) Azure AD-szinkronizálás
5. Azure Active Directory exportálása
6. A helyszíni könyvtárak, például az Active Directory exportálása

A beépített ütemező tekinti meg, ha ez a sorrendjének az összekötők fog futni.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Figyelheti a szinkronizálási motor annak ellenőrzéséhez, hogy elfoglalt, vagy ha üresjárati is. Ezzel a parancsmaggal egy üres eredményt ad vissza, ha a szinkronizálási motor inaktív és nem fut összekötő. Ha az összekötő fut, a nevét, az összekötő ad vissza.

```
Get-ADSyncConnectorRunStatus
```

![Összekötő állapot futtatása](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
A fenti képen az első sor hol üresjárati-e a szinkronizálási motor állam származik. Az Azure Active Directory-összekötő működik, ha a második sorban.

## <a name="scheduler-and-installation-wizard"></a>A Feladatütemező és telepítési varázsló
Ha a telepítés varázsló indítása, majd az ütemező ideiglenesen felfüggeszti. Ennek oka az, feltételezett értéke a konfigurációs módosítások elvégzése lesz, és ezek nem alkalmazható, ha a szinkronizálási motor aktívan fut. Emiatt nem hagyja az telepítővarázslóban megnyitása óta megszakad a szinkronizálási engine bármely szinkronizálási műveletek elvégzésére.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálni](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).
