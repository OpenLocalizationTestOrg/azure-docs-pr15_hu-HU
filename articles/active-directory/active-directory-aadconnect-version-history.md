<properties
   pageTitle="Azure AD Connect: Megjelenés korábbi verziók |} Microsoft Azure"
   description="Ez a témakör felsorolja az összes operációs rendszere Azure AD Connect és Azure AD-szinkronizálás"
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
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Megjelenés korábbi verziók

Az Azure Active Directory csapata rendszeresen frissíti Azure AD Connect új szolgáltatást és funkciót. Nem minden kiegészítések összes célközönségek vonatkoznak.

Ez a cikk segít a azon verziói, amelyek vannak adva nyomon követheti a és megértéséhez, hogy módosítania kell a legújabb verzióra vagy nem lett tervezve.

Ez a kapcsolódó témakörök listája:

A témakör |  
--------- | --------- |
Frissítés az Azure AD Connect lépései | Más módszerek [a legkésőbbiig korábbi verziójú frissítés](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect kiadásra.
Szükséges engedélyek | Frissítés alkalmazása, akkor olvassa el a [fiókok és engedélyek](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade) szükséges engedélyek
Letöltés| [Letöltés: Azure AD csatlakoztatása](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Ki: 2016 augusztus

**Javított problémákat:**

- Változások szinkronizálása intervallum nem történik addig, amíg következő szinkronizálási ciklus befejeződése után.
- Azure AD Connect varázsló nem fogadja el az Azure AD-fiók amelynek felhasználónév kezdődik aláhúzásjelet (\_).
- Azure AD Connect varázsló nem sikerült hitelesíteni Azure AD-fiókot, amennyiben a fiók jelszava túl sok speciális karaktereket tartalmaz. Hibaüzenet jelenik meg "nem lehet érvényesíteni a hitelesítő adatait. Váratlan hiba történt." adja vissza.
- Eltávolítás a kiszolgáló átmeneti Azure AD-bérlő jelszó-szinkronizálás letiltása, és jelszó-szinkronizálás active server, melyről okoz.
- A jelszó-szinkronizálás meghiúsul ritka esetekben nincs jelszó kivonat tárolja a felhasználó nem.
- Fejlesztői üzemmód server Azure AD Connect engedélyezésekor jelszó visszaírást nincs letiltva ideiglenesen.
- Amikor a kiszolgáló átmeneti tárolására mód nem Azure AD Connect varázsló megjelenítése a tényleges jelszó-szinkronizálás és a jelszó visszaírást konfigurációt. Őket mindig azt mutatja, amint tiltható le.
- Jelszó-szinkronizálás és a jelszó visszaírást konfigurációs módosításait nem állandó Azure AD Connect varázslóval, amikor a kiszolgáló átmeneti tárolására módban.

**Fejlesztések:**

- Kezdés-ADSyncSyncCycle parancsmag frissített jelzi, hogy sikeresen el egy új szinkronizálási ciklust, vagy nem tudja, hogy.
- A felvett a Stop-ADSyncSyncCycle parancsmag szinkronizálási ciklus és a műveletet, amely jelenleg folyamatban lévő befejezéséhez.
- Frissített Stop-ADSyncScheduler parancsmag szinkronizálási ciklus és a műveletet, amely jelenleg folyamatban lévő befejezéséhez.
- [A címtár-bővítmények](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD Connect varázsló konfigurálásakor Active Directory-attribútum "Karakterlánc Teletex" típusú most lehet kiválasztani.

## <a name="111890"></a>1.1.189.0
Ki: június 2016

**Rögzített problémák és a fejlesztések:**

- Azure AD Connect most FIPS kompatibilis kiszolgáló telepíthető.
    - A jelszó-szinkronizálás a [Jelszó-szinkronizálás és FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips) című témakör tartalmaz.
- Rögzített problémát, ahol NetBIOS nevét nem oldható szeretne a Tartománynevet, az Active Directory-összekötő a.

## <a name="111800"></a>1.1.180.0
Ki: május 2016

**Új funkciók:**

- Tájékoztató és segít, hogy a tartomány ellenőrzéséhez, ha Azure AD Connect futtatása előtt egyáltalán nem.
- Frissítés a [Microsoft Cloud Németország](active-directory-aadconnect-instances.md#microsoft-cloud-germany)támogatja.
- A legújabb [Microsoft Azure kormányzati felhőalapú](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktúrájának új URL-cím követelményeknek hozzáadott támogatása.

**Rögzített problémák és a fejlesztések:**

- Megjelenik a szűrés a szinkronizálási szabály szerkesztő, hogy szinkronizálási szabályok megtalálhatóság érdekében.
- Nagyobb teljesítmény törlésekor az összekötő szóközt.
- Rögzített egy problémák, amikor ugyanazon objektumhoz lett törölt mind ugyanazon futtatása (a hívott törlés/hozzáadása) hozzáadott.
- Letiltott szinkronizálási szabály nem lesznek objektumok ismételt engedélyezése a beépített, és a frissítési vagy könyvtár séma attribútumok frissítése.

## <a name="111300"></a>1.1.130.0
Ki: április 2016

**Új funkciók:**

- Többértékű attribútum [Címtár](active-directory-aadconnectsync-feature-directory-extensions.md)-bővítmények hozzáadott támogatása.
- Verziónak támogatnia kell az [Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) figyelembe veendő frissíthető további konfigurációs variációk.
- Adja hozzá [egyéni](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler)ütemezője néhány parancsmagok.

## <a name="111190"></a>1.1.119.0
Ki: március 2016

**Javított problémákat:**

- Arról, hogy a jelszó-szinkronizálás óta Express telepítés nem használhatók a Windows Server 2008 (előtti-R2) nem támogatott adott operációs rendszeren.
- Az egyéni szűrő beállításokkal DirSync frissítésének nem működött a várt módon működik.
- Újabb verziójára frissítéskor és nem módosul a beállításait, a teljes importálása szinkronizálás nem ütemezhető.

## <a name="111100"></a>1.1.110.0
Ki: 2016 február

**Javított problémákat:**

- Frissítés a korábbi verziókban nem működik, ha a telepítés nem szerepel az alapértelmezett **C:\Program Files** mappát.
- Ha telepíti, és a kijelölés megszüntetése, **Indítsa el a szinkronizálási folyamat …** a telepítés varázslót végén újbóli futtatásával a telepítés varázslót nem teszi lehetővé az ütemezőt.
- Az ütemező nem fog működni, kiszolgálók, ahol a dátum/idő formátuma nem hu-hu a várt módon működik. Is letiltja `Get-ADSyncScheduler` való visszatéréshez a megfelelő időpontokat.
- Ha egy korábbi verzióját Azure AD Connect az ADFS, mint a bejelentkezés lehetőséget, és a frissítés telepítette, a telepítés varázslót újra nem futtathatók.

## <a name="111050"></a>1.1.105.0
Oldani: 2016 február

**Új funkciók:**

- [Automatikus frissítése](active-directory-aadconnect-feature-automatic-upgrade.md) szolgáltatása Express beállítások ügyfeleknek.
- A globális rendszergazdák MFA és a személyes Információkezelő használata az telepítővarázslóban támogatása.
    - Engedélyeznie kell a proxy MFA használatakor is engedélyezni a https://secure.aadcdn.microsoftonline-p.com forgalmat.
    - Meg kell https://secure.aadcdn.microsoftonline-p.com hozzáadása a megbízható helyek listára többtényezős megfelelően működik.
- Engedélyezze, hogy a felhasználó bejelentkezési módszer módosítása a kezdeti telepítése után.
- Engedélyezi a [tartomány és a szervezeti egységre szűrés](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) a telepítővarázslóban. Ez is lehetővé teszi, hogy csatlakozás erdőkhöz, ha nem az összes tartományok állnak rendelkezésre.
- [A Feladatütemező](active-directory-aadconnectsync-feature-scheduler.md) rendszer beépített a szinkronizálási motor.

**Funkciók, ám az előnézeti megszerzése:**

- [Eszköz visszaírást](active-directory-aadconnect-feature-device-writeback.md).
- [A címtár-bővítmények](active-directory-aadconnectsync-feature-directory-extensions.md).

**Új előzetes verzió szolgáltatások:**

- Az új alapértelmezett intervallum érték 30 perc ciklus szinkronizál. Minden korábbi verziókban 3 órát pedig régebben. Összeadja az [Ütemező](active-directory-aadconnectsync-feature-scheduler.md) szabályalkalmazási támogatás.

**Javított problémákat:**

- A DNS ellenőrzése a tartományok lapon nem mindig ismerik fel a tartományok.
- ADFS konfigurálása tartomány rendszergazdai hitelesítő adatait kérő üzenet.
- A helyszíni Active Directory-fiókok nem ismeri fel a telepítés varázslót, ha egy másik, mint a gyökértartomány DNS fában a tartományban található.

## <a name="1091310"></a>1.0.9131.0
Ki: a Skype 2015 December

**Javított problémákat:**

- Jelszó-szinkronizálás nem működik, amikor módosítja az Active Directory tartományi szolgáltatások, de works jelszavak jelszó beállításakor.
- Ha a proxykiszolgáló, az Azure Active Directory-hitelesítés során a telepítést, vagy törölje előfordulhat, hogy nem frissítési az beállítása lapon.
- A teljes Azure AD Connect egy korábbi verzióból származó frissítése az SQL Server meghiúsul, ha Ön nem SQL-rendszergazdai.
- A távoli az Azure AD Connect egy korábbi verzióból származó frissítése az SQL Server jelennek meg a következő hiba: "Nem sikerült az ADSync SQL-adatbázis eléréséhez".

## <a name="1091250"></a>1.0.9125.0
Ki: a Skype 2015 November

**Új funkciók:**

- Beállíthatja az ADFS-alapú az Azure Active Directory adatvédelmi.
- Az Active Directory séma frissítheti és újragenerálása szinkronizálási szabályok.
- A szinkronizálási szabályok tilthatja le.
- A szinkronizálási szabályokban új szövegkonstans "AuthoritativeNull" is meghatározásával.

**Új előzetes verzió szolgáltatások:**

- [Szinkronizálás az azure Active Directory csatlakozás állapota](active-directory-aadconnect-health-sync.md).
- A jelszó-szinkronizálás [Azure Active Directory tartományi szolgáltatások](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) támogatása.

**Új támogatott forgatókönyv:**

- Több a helyszíni telepítésű Exchange támogatja. [Hibrid környezetekben, az Active Directory-erdők több](https://technet.microsoft.com/library/jj873754.aspx) talál további információt.

**Javított problémákat:**

- Jelszó-szinkronizálási problémák:
    - A hatókör áthelyezése a blokkolás a hatókör objektum nem lesz a szinkronizált jelszó. Ez a incudes OU és attribútum szűrés.
    - Új OU, ha meg szeretné jeleníteni a Szinkronizáló kijelölése egy teljes jelszó-szinkronizálás nincs szükség.
    - Ha engedélyezve van a letiltott felhasználó nincs szinkronban a jelszót.
    - A jelszó ismét várólista végtelen és el lett távolítva az előző határértékén 5000 objektumok inaktívvá tenni.
    - [Továbbfejlesztett hibaelhárítás](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Nem tud csatlakozni az Active Directory Windows Server 2016 erdő funkcionális szinthez.
- Nem lehet módosítani a csoport csoportjának szűrés kezdeti telepítése után az.
- Már nem hoz létre egy új felhasználói profil minden felhasználójának, tegye a jelszó módosítása engedélyezett jelszó visszaírást tartalmazó kimutatások Azure AD Connect kiszolgálón.
- Nem tudják használni az értékek hosszú egész szinkronizálja szabályok hatókörök.
- A jelölőnégyzet "eszköz visszaírást" Letiltva marad, nem érhető el a tartomány vezérlők esetén.

## <a name="1086670"></a>1.0.8667.0
Ki: a Skype 2015 augusztus

**Új funkciók:**

- Az Azure AD Connect telepítővarázslóban most honosított, az összes Windows Server nyelvekre.
- Verziónak támogatnia kell figyelembe az Azure Active Directory-jelszó szolgáltatások használatakor zárolásának feloldása

**Javított problémákat:**

- Azure AD Connect telepítővarázslóban összeomlik, ha egy másik felhasználó továbbra is fennáll, telepítése, hanem a személy, aki az első lépések a telepítést.
- Ha egy korábbi eltávolításának írnia távolítanom az Azure AD Connect szinkronizálási Azure AD Connect sikertelen, még nem lehetséges újratelepítése.
- Nem tudja telepíteni az Azure AD Connect Express telepítés használata, ha a felhasználó nem szerepel a erdő_gyökértartománya a, vagy ha egy, az Active Directory nem angol nyelvű verzióját használja.
- Ha nem lehet megoldani a felhasználó Active Directory-fiókját teljes Tartománynevét, egy félrevezető "Véglegesítse a séma nem sikerült" hibaüzenet jelenik meg.
- Az Active Directory összekötő használt fiók megváltozásakor kívül a varázsló a varázsló további futtatja a sikertelen lesz.
- Azure AD Connect előfordul, hogy a tartományvezérlőnek telepítése sikertelen lesz.
- Nem lehet engedélyezése, és tiltsa le a "Fejlesztői üzemmód", ha a bővítmény attribútumok hozzá lett adva.
- Jelszó visszaírást néhány konfigurálása az Active Directory-összekötő a hibás jelszó miatt sikertelen lesz.
- Nem lehet frissíteni a DirSync, ha dn attribútum szűrés szerepel.
- Fölösleges processzorhasználata jelszó-visszaállítás használata esetén.

**Eltávolított előzetes verzió szolgáltatások:**

- A [felhasználó visszaírást](active-directory-aadconnect-feature-preview.md#user-writeback) ideiglenesen el lett távolítva megtekintése funkció előzetes ügyfeleink visszajelzései alapján. Rá újra belekerül később, ha azt van címezve a megadott visszajelzést.

## <a name="1086410"></a>1.0.8641.0
Ki: június 2015

**Azure AD Connect kezdeti kiadását.**

Azure AD Connect Azure AD-szinkronizálás a módosított nevét.

**Új funkciók:**

- Telepítési [Beállítások Express](./connect/active-directory-aadconnect-get-started-express.md)
- Is [ADFS konfigurálása](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- [A DirSync frissítése](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) is
- [Véletlen törlés tiltása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Bevezetett [átmeneti tárolására mód](active-directory-aadconnectsync-operations.md#staging-mode)

**Új előzetes verzió szolgáltatások:**

- [Felhasználói visszaírást](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Csoport visszaírást](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Eszköz visszaírást](active-directory-aadconnect-feature-device-writeback.md)
- [A címtár-bővítmények](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Ki: május 2015

**Új követelmény:**

- Azure AD-szinkronizálás most a .net keretrendszer verzió 4.5.1 telepítésére van szükség.

**Javított problémákat:**

- Jelszó visszaírást az Azure Active Directory servicebus kapcsolódási hibával meghiúsul.

## <a name="104910413"></a>1.0.491.0413
Ki: április 2015

**Rögzített problémák és a fejlesztések:**

- Az Active Directory-összekötő nem dolgozza fel törlése helyesen, ha a Lomtár engedélyezve van, és ott amelyek tartományban több tartományt találhatók.
- Az importálási műveletekhez teljesítményét továbbfejlesztettük az Azure Active Directory Connector.
- Amikor egy csoport túllépte az tagság értéket (alapértelmezés szerint a korlátot értéke 50k objektumok), az Azure Active Directory törölték a csoportot. Az új működése, hogy a csoport marad, hiba elő, és nincs tagság egyesíthető változtatás exportálásának módját.
- Egy új objektum nem kell építenie, ha az azonos DN a szakaszos delete már szerepel a összekötő szóköz.
- Egyes objektumok szinkronizálva a delta szinkronizálás során, bár nem változnak meg az objektum előkészített vannak megjelölve.
- Jelszó-szinkronizálás kényszerítése, az előnyben részesített Adatközpont lista is eltávolítja.
- Egyes objektumok államok CSExportAnalyzer hibát.

**Új funkciók:**

- Illesztés most csatlakozhat "ANY" objektumtípust a té Elemet.

## <a name="104850222"></a>1.0.485.0222
Ki: 2015-február

**Fejlesztések:**

- Továbbfejlesztett importálási teljesítményét.

**Javított problémákat:**

- Jelszó-szinkronizálás végzett betartja attribútum szűréssel használt cloudFiltered attribútum. Szűrt objektumok már nem tekinthetők meg a jelszó-szinkronizálás hatókörét.
- Ritka olyan esetekben, ahol az a topológia volna nagyon sok tartomány vezérlők, a jelszó nem működik a szinkronizálás.
- "Leállt-kiszolgálón" importálásakor az Azure Active Directory-összekötő a mobileszköz-kezelés után engedélyezve van az Azure Active Directory/Intune.
- Csatlakozás több tartományt ugyanabban a tartományban lévő idegen rendszerbiztonsági (FSP), akkor az nem egyértelmű illesztés hibát okoz.

## <a name="104751202"></a>1.0.475.1202
Ki: 2014-es December

**Új funkciók:**

- Végezze el az attribútum alapuló szűrés jelszó-szinkronizálás most támogatja. További részletekért olvassa el [a jelszó-szinkronizálás szűréssel](active-directory-aadconnectsync-configure-filtering.md).
- Az attribútum msDS ExternalDirectoryObjectID ad vissza íródott. Ezzel hozzáadja az oauth2 hitelesítési mód mindkettő Online eléréséhez használja az Office 365-alkalmazások támogatása, és a helyszíni postaládák az Exchange-alapú.

**Rögzített frissítésével kapcsolatos problémák:**

- A kiszolgáló a bejelentkezési segéd újabb verzióját érhető el.
- Egyéni telepítési útvonal Azure AD-szinkronizálás telepítéséhez használt.
- Az érvénytelen egyéni illesztési feltétel blokkolja a frissítés.

**Egyéb javítások:**

- A sablonok rögzíteni az Office Pro Plus.
- Rögzített való telepítéskor felmerülő hibák kötőjel kezdődő felhasználónevek okozza.
- Rögzített elvesztése a sourceAnchor beállítást, ha futtatja a telepítés varázslót másodszori.
- Rögzített esemény-nyomkövetés nyomkövetési jelszó-szinkronizálás

## <a name="104701023"></a>1.0.470.1023
Ki: 2014-es október

**Új funkciók:**

- Több jelszó-szinkronizálás a helyszíni Active Directory Azure ad.
- Az összes Windows Server nyelvekre honosított telepítési felület

**AADSync 1.0 Georgia verzióról**

Ha már telepítve van a Azure AD-szinkronizálás, nincs el kell végeznie abban az esetben a Blokkolás kész szinkronizálási szabályok megváltoztak egy további lépéssel. Miután frissítette a 1.0.470.1023 engedje fel, a szinkronizálás vannak módosította a szabályokat létrejön egy újabb példánya. Módosított szinkronizálási szabályhoz tegye a következőket:

- Keresse meg a szinkronizálási szabály módosította, és jegyezze fel a módosításokat.
- A szinkronizálási szabály törlése.
- Keresse meg az új szinkronizálási szabály létrehozása az Azure AD-szinkronizálás, és újra alkalmazza a változtatásokat.

**Active Directory-engedélyek**

Az Active Directory-fiók jogosultsággal kell rendelkeznie engedélyezni szeretné, hogy olvassa el a jelszó tiltva az Active Directory további engedélyeket. Az engedélyeket neve "Replikálása címtár-módosítások" és "Replikálása címtár módosításokat az összes". Mindkét engedélyek szükségesek tudja felolvasni a jelszó tiltva

## <a name="104190911"></a>1.0.419.0911
Ki: 2014-es szeptember

**Azure AD-szinkronizálás kezdeti kiadását.**

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitások Azure Active Directoryval való integrálásának](active-directory-aadconnect.md).
