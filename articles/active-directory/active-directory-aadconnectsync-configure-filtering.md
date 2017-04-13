<properties
    pageTitle="Azure AD Connect szinkronizálása: szűrés konfigurálása |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure AD Connect szinkronizálás szűrés konfigurálása."
    services="active-directory"
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect szinkronizálási: szűrés konfigurálása
Szűréssel, megadhatja, hogy milyen objektum jelenjen meg Azure AD a helyszíni címtárból. Az alapértelmezett beállítások megnyitja az összes objektumot a beállított erdőkben minden tartományban. Ez általában az ajánlott konfiguráció. A végfelhasználók használata az Office 365-munkaterhelésekből, például az Exchange online-ban és a Skype vállalati verzió, úgy, hogy e-mailek küldése, hívja fel a Mindenki teljes globális címlistában élvezhetik. Az alapértelmezett beállításokkal az azonos élmény őket egy helyszíni Exchange- vagy Lync végrehajtása amilyent volna őket.

Egyes esetekben szükség van néhány módosítást az alapértelmezett beállítások. Íme néhány példa:

- A [több elem Azure Active Directory-címtár topológia](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory)használni kívánja. Szűrő szabályozhatja, hogy melyik objektumhoz egy adott kell szinkronizálnia kell majd Azure Active directory.
- Próbaüzem futtatja az Azure vagy az Office 365-ben, és Azure Active Directory felhasználók csak egy részhalmazát csak szeretne. A kis próbaüzem fontos nem szeretné, hogy a teljes globális címjegyzékben bemutatják a használható funkciók körét.
- Ha sok szolgáltatás és más nem szeretné, hogy az Azure Active Directory nem személyes fiókok.
- Megfelelőségi okok miatt nem törli a minden olyan felhasználói fiókokat helyszíni. Csak le őket. De az Azure Active Directory csak jelen aktív partnereket.

Ez a cikk bemutatja, hogy miként különböző szűrési módszerek beállítása.

> [AZURE.IMPORTANT]A Microsoft nem támogatja a módosítása, illetve az Azure AD Connect szinkronizálás ezeket a műveleteket hivatalos ismertetését kívüli működésének. Az alábbi műveletek valamelyikét Azure AD Connect szinkronizálási várttól eltérően működik, vagy nem támogatott állapotának járhat, és emiatt a Microsoft nem nyújt technikai támogatást ilyen telepítések.

## <a name="basics-and-important-notes"></a>Ha szeretne, és a fontos jegyzeteket
Azure AD Connect szinkronizálás engedélyezheti bármikor szűrés. Indítsa el a címtár-szinkronizálás alapértelmezett beállításokkal, és kattintson a szűrés konfigurálása, ha az objektumokat, amelyek kiszűri már nem szinkronizálódnak Azure AD. Ez a változás az Azure Active Directory, amely korábban már szinkronizált, de azok majd szűrt objektumainak az Azure Active Directory törlődnek.

Mielőtt elkezdené, így változik szűrési, győződjön meg arról, hogy, [letiltása az ütemezett feladat](#disable-scheduled-task) , akkor ne véletlenül exportálja a módosításokat, amelyeket még nem igazolta helyes.

Mivel a szűrést, eltávolíthatja egyszerre sok objektum, ellenőrizze, hogy az új szűrők helyesek, mielőtt megkezdené a módosításokat exportálása Azure Active Directory szeretne. Miután befejezte a konfigurálási lépéseket, erősen ajánlott [ellenőrzési lépések](#apply-and-verify-changes) végrehajtásával, exportálni, és végezze el a módosításokat az Azure Active Directory.

Több objektum törlése véletlenül védekezésben, [véletlen törlés tiltása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) a szolgáltatás alapértelmezés szerint engedélyezve van. Ha sok objektum miatt (alapértelmezés szerint 500) szűrés törléséhez kövesse a lépéseket a jelen cikkben a törli az Azure Active Directory folyamatát engedélyezni szeretné.

Mielőtt November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), módosítás szűrő beállításait, és a jelszó-szinkronizálással építés használata esetén majd szeretné elindítani az összes jelszavak teljes szinkronizálást, miután befejezte a konfiguráció. Bemutatjuk, hogy miként indíthatja el a jelszó teljes szinkronizálási [elindítása](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords)című témakör nyújt az összes jelszavak teljes szinkronizálást. Ha 1.0.9125 vagy újabb, majd a **teljes szinkronizálást** rendszeres műveletet is és számítja ki Ha jelszavak kell szinkronizálnia ebben a lépésben további már nem szükséges.

Objektumok **felhasználói** véletlenül törölt Azure AD egy szűrési hiba miatt, ha eltávolításával hozza létre újból a felhasználó objektumok az Azure Active Directory a szűrési beállításokat, és ezt követően szinkronizálja ismét a könyvtárak. Ez a művelet a felhasználók visszaállítása a Lomtárból az Azure Active Directory. Más objektumtípusok azonban nem tudja törlés visszavonása. Ha például ha véletlenül töröl egy olyan biztonsági csoportot, és azt vezérlés használt erőforrásként, a csoport és a hozzáférés-vezérlési listák nem állíthatók helyre.

Azure AD Connect csak törli azt még egyszer tekintett hatókör objektumokat. Ha egy másik szinkronizálási motor által létrehozott objektumok az Azure Active Directory, és az objektumok nem szerepelnek a hatókör, szűrés hozzáadása nem távolítja el őket. Ha például ha DirSync kiszolgálóval indítja el, és azt a teljes címtárban teljes másolatát létre az Azure Active Directory, és új Azure AD Connect szinkronizálási kiszolgáló engedélyezve van az elejétől szűréssel párhuzamosan telepítése, nem távolítja el a felesleges objektumok DirSync által létrehozott.

A szűrési konfiguráció telepítésekor vagy újabb verzióját Azure AD Connect frissítése tárolja. Az mindig ellenőrizze, hogy a konfiguráció nem véletlenül változott frissítését követően egy újabb verziójára a Váltás az első szinkronizálás futtatása előtt a legjobb megoldás.

Ha egynél több erdő, majd a cikkben ismertetett szűrési beállításokat kell vonatkozni minden erdőben (feltételezve, hogy azt szeretné, hogy ugyanazt a konfigurációt, az összes el őket).

### <a name="disable-scheduled-task"></a>Ütemezett feladat letiltása
A beépített ütemezőt, amely elindítja a szinkronizálást ciklus 30 percenként letiltásához kövesse az alábbi lépéseket:

1. Nyissa meg a PowerShell kérdés.
2. Futtatása `Set-ADSyncScheduler -SyncCycleEnabled $False` letiltása az ütemezőt.
3. Ebben a témakörben ismertetett módon végezze el a módosításokat.
4. Futtatása `Set-ADSyncScheduler -SyncCycleEnabled $True` ahhoz, hogy az ütemező újra.

**Ha egy Azure AD Connect build 1.1.105.0 előtt**  
Az ütemezett feladat, amely elindítja a szinkronizálást ciklus 3 óránként letiltásához kövesse az alábbi lépéseket:

1. Indítsa el a **Feladatütemező** a start menüből.
2. Közvetlenül a **Feladatütemező könyvtár**, keresse meg a tevékenység neve **Azure AD-szinkronizálás ütemezőt**, kattintson a jobb gombbal és **letiltásához**jelölje be.  
![A Feladatütemező](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Most beállításainak módosítása és a szinkronizálási motor kézi futtatása a **szinkronizálás szolgáltatáskezelő** konzolról.

A szűrési módosítások befejezése után ne felejtse el beépített részei, és **engedélyezheti** újra a tevékenység vissza.

## <a name="filtering-options"></a>Szűrési lehetőségek
Az alábbi szűrési konfigurációs típusok is alkalmazható a címtár-Szinkronizáló eszköz:

- [**Csoport-alapú**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): egyetlen csoport alapuló szűrés csak állítható be a telepítővarázslóval kezdeti telepítése. További nem vonatkozik a jelen témakör.

- [**Tartományalapú**](#domain-based-filtering): ezt a beállítást lehetővé teszi, hogy mely tartományok alkalmazással az Azure Active Directory szinkronizáló megadását. Lehetővé teszi a tartományok eltávolítása a szinkronizálási motor konfiguráció, ha módosítja a helyszíni infrastruktúrájába Azure AD Connect szinkronizáló telepítése után.

- [**Szervezeti egység alapú**](#organizational-unitbased-filtering): A szűrést lehetővé teszi, hogy akkor válassza ki a szervezeti szinkronizál az Azure Active Directory. Ez a beállítás az összes objektumtípusok a kijelölt szervezeti szolgál.

- [**Attribútum alapú**](#attribute-based-filtering): Ezzel a beállítással objektumok az objektumok attribútum értékek alapján szűrni. Különböző objektumtípusok különböző szűrők is.

Egyszerre több szűrési lehetőségek is használhatja. Ha például is használhatja OU alapuló szűrés úgy, hogy csak objektumok azonos idő attribútum-alapú szűrés az objektumok további szűréséhez és egy szervezeti. Több szűrési módszer, a szűrők használatakor logikai és a szűrők között.

## <a name="domain-based-filtering"></a>Szűrés tartományalapú
A szakasz ismerteti a lépéseket a tartomány-szűrő beállítása. Ha hozzá, illetve eltávolítja a erdő tartományok Azure AD Connect telepítése után, akkor is a szűrési beállítások frissítése.

A használni kívánt úgy módosíthatja a tartományalapú szűrés, a telepítés varázslót, és az új [tartomány és a szűrést szervezeti](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)futtatásával. A telepítés varázslót az automatizálás, ebben a témakörben ismertetett feladatokat.

Csak akkor kövesse ezeket a lépéseket, ha valamiért nem tudja a telepítés varázslót.

Szűrési konfigurációs tartományalapú a következő lépésekből áll:

- [Jelölje ki a tartományok](#select-domains-to-be-synchronized) , amelyek a szinkronizálást kell foglalni.
- Hozzáadott és eltávolított tartományokhoz módosítsa a [profilok futtatni](#update-run-profiles).
- [Alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Jelölje ki a szinkronizálandó tartományok
**A tartomány szűrő beállításához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be az Azure AD Connect sync-fiókkal, amely a **ADSyncAdmins** biztonsági csoport tagja futtató kiszolgáló.
2. Indítsa el a **Szinkronizálási szolgáltatás** a start menüből.
3. Jelölje ki az **összekötőket** , és **összekötők** listájában jelölje ki az összekötőt az **Active Directory tartományi szolgáltatások**típusú. A **Műveletek**csoportban válassza a **Tulajdonságok parancsot**.  
![Összekötő tulajdonságai](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kattintson a **címtár-partíciók konfigurálása**.
5. A **címtár-partíciót kiválasztása** listában jelölje ki, és kijelölésének megszüntetése a tartományokat, szükség szerint. Győződjön meg arról, hogy csak a szinkronizálni kívánt partíciók vannak-e jelölve.  
![Partíciót](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Ha megváltoztatta a helyszíni Active Directory infrastruktúra- és az erdőből távolítva, illetve a felvett tartományok kattintson egy frissített listát a **frissítés** gombra. Amikor frissíti, a program megkérdezi, hitelesítő adatokat. Adja meg a hitelesítő adatokat olvasási hozzáférést a helyszíni Active Directory. Nem rendelkezik arra a felhasználóra, amely már előre kitölt párbeszédpanel.  
![Frissítés szükséges](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Ha végzett, zárja be a **Tulajdonságok** párbeszédpanelen kattintson **az OK gombra**. Tartományok eltávolítása az erdőből, ha közli, hogy a tartomány el lett távolítva üzenet előugró és a konfigurációs fog kiüríteni.
7. Továbbra is módosítsa a [profilok futtatni](#update-run-profiles).

### <a name="update-run-profiles"></a>Frissítés futtatása profilok
A tartomány szűrőben frissített, ha meg is frissítenie kell a Futtatás profilokat.

1. **Összekötők** listájában az összekötőt, amelyet az előző lépésben a módosított nincs bejelölve. **Műveletek**kattintson a **Futtatás profilok konfigurálása**.  
![Összekötő profilok futtatása](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

A következő profilok beállításához szükséges:

- Teljes importálása
- A teljes szinkronizálás
- Delta importálása
- Delta szinkronizálás
- Exportálás

Az egyes a öt profilokat a következő lépésekkel **felvett** tartományokhoz:

1. Jelölje ki a Futtatás profilt, és kattintson az **Új lépést**.
2. **Lépés konfigurálása** lapon a **típus** legördülő listában jelölje ki a lépés azonos nevű a profil állítja be. Kattintson a **Tovább gombra**.  
![Összekötő profilok futtatása](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Az **Összekötő beállítása** lapon a **partíciót** legördülő listában válassza a hozzáadta a tartományt szűrő a tartomány nevét.  
![Összekötő profilok futtatása](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Zárja be a **Futtatás profil beállítása** párbeszédpanelt, kattintson a **Befejezés gombra**.

Az egyes a öt profilokat a következő lépésekkel **eltávolítja** tartományokhoz:

1. Jelölje be a Futtatás profilt.
2. Ha az **érték** a **partíciót** attribútum GUID, jelölje ki a Futtatás lépést, és **Lépés törlése**parancsára.  
![Összekötő profilok futtatása](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Az eredményt kell, hogy minden egyes tartományhoz szeretne szinkronizálni a Futtatás profilokhoz lépés fog szerepelni.

A **Profilok konfigurálása futtatása** párbeszédpanel bezárásához kattintson az **OK gombra**.

- A konfiguráció befejezéséhez [alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Szervezeti egység alapú szűrése
Az előnyben részesített módosítása a szervezeti egységre alapuló szűrés módja a telepítés varázslót, és az új [tartomány és a szűrést szervezeti](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering)futtatásával. A telepítés varázslót az automatizálás, ebben a témakörben ismertetett feladatokat.

Csak akkor kövesse ezeket a lépéseket, ha valamiért nem tudja a telepítés varázslót.

**Szervezeti egység – alapú szűrés beállításához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be az Azure AD Connect sync-fiókkal, amely a **ADSyncAdmins** biztonsági csoport tagja futtató kiszolgáló.
2. Indítsa el a **Szinkronizálási szolgáltatás** a start menüből.
3. Jelölje ki az **összekötőket** , és **összekötők** listájában jelölje ki az összekötőt az **Active Directory tartományi szolgáltatások**típusú. A **Műveletek**csoportban válassza a **Tulajdonságok parancsot**.  
![Összekötő tulajdonságai](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kattintson a **Címtár-partíciók konfigurálása**, jelölje ki a beállítani kívánt tartományt, és válassza a **tárolók**.
5. Amikor a rendszer kéri, küldje el hitelesítő adatokat olvasási hozzáférést a helyszíni Active Directory. Nem rendelkezik arra a felhasználóra, amely már előre kitölt párbeszédpanel.
6. **Tárolók kijelölése** párbeszédpanelen törölje a jelet a szervezeti, amelyeket nem szeretne szinkronizálni a felhőben könyvtár, és kattintson **az OK**gombra.  
![SZERVEZETI EGYSÉG](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - A Windows 10-es számítógépek sikeresen szinkronizálandó az Azure Active Directory számára a **számítógép** -tárolóra kell választani. Ha a tartományhoz csatlakoztatott számítógép más egységekben helyezkednek el, ellenőrizze, azok legyen kijelölve.
  - Ha több erdőket meghatalmazások kell választani a **ForeignSecurityPrincipals** tároló. Ez a tároló lehetővé teszi, hogy a erdőközi biztonsági csoport tagsága el kell hárítani.
  - Ha engedélyezte a eszköz visszaírást szolgáltatást, akkor a **RegisteredDevices** szervezeti kell választani. Ha egy másik visszaírást szolgáltatás, használhatja csoport visszaírást, például ellenőrizze, hogy ezekre a helyekre ki van jelölve.
  - Jelölje be a bármely más szervezeti egység, ahol a felhasználók, iNetOrgPersons, csoportok, névjegyek és számítógépek találhatók. A képet mind a ManagedObjects szervezeti találhatók.
7. Ha végzett, zárja be a **Tulajdonságok** párbeszédpanelen kattintson **az OK gombra**.
8. A konfiguráció befejezéséhez [alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Attribútum alapuló szűrés
Ellenőrizze, hogy a a November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), vagy később építése ezeket a lépéseket a munkát.

A legtöbb rugalmasan szűrő objektumok attribútum alapuló szűrés. A [deklaráció kiépítési](active-directory-aadconnectsync-understanding-declarative-provisioning.md) a power segítségével ellenőrzés szinte minden eleme, ha az objektum kell szinkronizálnia a Azure AD.

Szűrő alkalmazása a [bejövő](#inbound-filtering) az Active Directory számára a metaverse és [kimenő](#outbound-filtering) a metaverse az Azure AD mindkét. Szűrés a bejövő óta, amely a legegyszerűbb megőrzéséhez ajánlott. Kimenő szűrés csak használandó egynél több erdőből objektumok csatlakozni, mielőtt a kiértékelés történhet szükség esetén.

### <a name="inbound-filtering"></a>Bejövő szűrése
Bejövő alapuló szűrés használja az alapértelmezett beállítások hol objektumok, látogasson el a Azure AD a szinkronizálandó értéke nincs beállítva metaverse attribútum cloudFiltered kell rendelkeznie. Ha az attribútum értéke **Igaz**, az objektum nem szinkronizálja. Meg kell nem állíthatók be **hamis** szándékosan. Használatával biztosítható, hogy más szabályok az azt jelenti, hogy egy érték közreműködés, ez az attribútum most már csak szabad, hogy az **Igaz** vagy a **NULL** (hiányzó) értékek.

A bejövő szűrése használatával a kiemelt **hatókör** megállapíthatja, hogy mely objektumai kell, vagy nem szinkronizálódnak. Ez a hol, hajtsa végre a módosításokat a szervezeten követelmények igazítása. A hatókör modul van a **csoport** és a **záradék** határozza meg, ha szinkronizálási szabály hatóköre kell lennie. Egy **csoport** tartalmaz egy vagy több **záradék**. Egy logikai és több záradékok és egy logikai vagy több csoportok között van.

Példa tudassa velünk megtekintése:  
![Hatókör](./media/active-directory-aadconnectsync-configure-filtering/scope.png) meg kell értelmezni **(részleg = informatikai) vagy (részleg = értékesítés és a c = US)**.

A minták és az alábbi lépéseket használja a user objektumban szerepel példaként, de -et is ez az összes objektum típusa.

Az alábbi példa a sorrend értéke 500 kezdje. Ez az érték köszönhetően szabályok kiértékelése után a kimenő kész szabályokat (előrébb, újabb numerikus értéket).

#### <a name="negative-filtering-do-not-sync-these"></a>Szűrés, "nem szinkronizálhatja ezek" negatív
A következő példában a kiszűrése (nem szinkronizálódnak) minden felhasználó, ahol **extensionAttribute15** **Nincs szinkronizálás**értékkel rendelkezik.

1. Jelentkezzen be az Azure AD Connect sync-fiókkal, amely a **ADSyncAdmins** biztonsági csoport tagja futtató kiszolgáló.
2. Indítsa el a **Szinkronizálási szabályok szerkesztőt** a start menüből.
3. **Bejövő** nincs bejelölve, és kattintson az **Új szabály hozzáadása**.
4. Nevezze el a szabály leíró, például: "*az Active Directory – felhasználói DoNotSyncFilter a*". Jelölje ki a megfelelő erdő **felhasználói** **CS objektum típusa**és **személy** **Objektumtípus té Elemet**. **Kapcsolat típusa**jelölje be a **Bekapcsolódás** prioritású írjon be egy értéket, jelenleg nem használja egy másik szinkronizálási szabály (például 500), és kattintson a **Tovább gombra**.  
![Bejövő 1 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. **Scoping szűrő** **Csoport hozzáadása**gombra kattintva **Záradék hozzáadása**kattintson, és attribútum válassza a **ExtensionAttribute15**. Ellenőrizze, hogy az operátor értéke **megegyezik** , és írja be a **Nincs szinkronizálás** értéket az érték mezőben. Kattintson a **Tovább**gombra.  
![Bejövő 2 hatóköre](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Hagyja üresen a **Bekapcsolódás** szabályokat, és kattintson a **Tovább gombra**.
7. **Átalakítási hozzáadása**gombra, jelölje ki a **FlowType** az **állandó**, jelölje be a cél attribútum **cloudFiltered** , és a forrás mezőbe írja be a **Igaz**. Kattintson a **Hozzáadás gombra** kattintva mentheti a szabályt.  
![Bejövő 3 transzformációt hajt végre.](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. A konfiguráció befejezéséhez [alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Szűrés, a "csak szinkronizálása ezek" pozitív
Pozitív szűrés kifejező izgalmasabbá lehet, mert az objektumok, amelyek nem szinkronizálható, például az üléstermek egyértelmű is figyelembe van.

A pozitív szűrést két szinkronizálási szabályok szükséges. Egy (vagy több) az objektumok szinkronizálása, és egy második megfelelő hatókörének tényleges – az összes szinkronizálása szabály a szűrő el van még nem határozták meg szinkronizálandó objektumként objektumait.

A következő példában a felhasználó objektumok, ahol a részleg attribútum értéke az **értékesítési**csak szinkronizálás.

1. Jelentkezzen be az Azure AD Connect sync-fiókkal, amely a **ADSyncAdmins** biztonsági csoport tagja futtató kiszolgáló.
2. Indítsa el a **Szinkronizálási szabályok szerkesztőt** a start menüből.
3. **Bejövő** nincs bejelölve, és kattintson az **Új szabály hozzáadása**.
4. Nevezze el a szabály leíró, például ",*az Active Directory – a felhasználó értékesítés szinkronizálása*". Jelölje ki a megfelelő erdő **felhasználói** **CS objektum típusa**és **személy** **Objektumtípus té Elemet**. **Kapcsolat típusa**jelölje be a **Bekapcsolódás** prioritású írjon be egy értéket, jelenleg nem használja egy másik szinkronizálási szabály (például 501), és kattintson a **Tovább gombra**.  
![Bejövő 4 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. **Scoping szűrő** **Csoport hozzáadása**gombra, **Záradék hozzáadása**gombra, és attribútum válassza a **részleg**. Ellenőrizze, hogy az operátor értéke **megegyezik** , és írja be az **Értékesítés** érték az érték mezőbe. Kattintson a **Tovább**gombra.  
![Bejövő 5 hatóköre](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Hagyja üresen a **Bekapcsolódás** szabályokat, és kattintson a **Tovább gombra**.
7. **Átalakítási hozzáadása**gombra, jelölje be a **FlowType** az **állandó**, jelölje be a cél attribútum **cloudFiltered** , és a forrás mezőbe írja be a **hamis**. Kattintson a **Hozzáadás gombra** kattintva mentheti a szabályt.  
![Bejövő 6 transzformációt hajt végre.](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Ez egy speciális eset, ahol beállíthatja cloudFiltered kifejezetten hamis.

    Most már van a tényleges – az összes szinkronizálása szabály létrehozásához.

8. Nevezze el a szabály leíró, például: "*az Active Directory – felhasználói tényleges összes szűrőt a*". Jelölje ki a megfelelő erdő **felhasználói** **CS objektum típusa**és **személy** **Objektumtípus té Elemet**. **Kapcsolat típusa**jelölje be a **Bekapcsolódás** pedig elsőbbségi jelenleg nem használja egy másik szinkronizálási szabály (például 600) értéket. Kijelölt elsőbbségi értéke nagyobb (alacsonyabb prioritású) az előző szinkronizálási szabály van, de néhány szoba is balra, így azt is további szűrési szinkronizálási szabályok később szeretne hozzáadni a további részlegek szinkronizálásának indítása. Kattintson a **Tovább**gombra.  
![Bejövő 7 leírása](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Hagyja üresen **Scoping szűrőt** , és kattintson a **Tovább**gombra. Egy üres szűrő azt jelzi, hogy a szabály az összes objektum vonatkoznak.
10. Hagyja üresen a **Bekapcsolódás** szabályokat, és kattintson a **Tovább gombra**.
11. **Átalakítási hozzáadása**gombra, jelölje be a **FlowType** az **állandó**, jelölje be a cél attribútum **cloudFiltered** , és a forrás mezőbe írja be a **Igaz**. Kattintson a **Hozzáadás gombra** kattintva mentheti a szabályt.  
![Bejövő 3 transzformációt hajt végre.](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. A konfiguráció befejezéséhez [alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

Ha kell, akkor az első típus, ahol szerepeltetni kívánt további objektumok a szinkronizálást a többi szabály hozhat létre.

### <a name="outbound-filtering"></a>Kimenő szűrése
Egyes esetekben célszerű a szűrés csak azt követően az objektumok csatlakoztak az metaverse az ehhez szükséges. Azt is, például kell tekintse meg a levelezési attribútum erőforráserdőben és a fiók erdőből határozza meg, ha egy objektum szinkronizálja a userPrincipalName attribútum. Ezekben az esetekben hoz létre a szűrés a kimenő szabály alapján.

Ebben a példában a hol mail, mind a userPrincipalName végződjön szűrési ezt csak a felhasználók módosítása @contoso.com vannak szinkronizálva:

1. Jelentkezzen be az Azure AD Connect sync-fiókkal, amely a **ADSyncAdmins** biztonsági csoport tagja futtató kiszolgáló.
2. Indítsa el a **Szinkronizálási szabályok szerkesztőt** a start menüből.
3. A **Szabályok típusa**csoportban kattintson a **kimenő**.
4. Keresse meg a nevű **kifelé AAD – felhasználói Bekapcsolódás SOAInAD a**szabályt. Kattintson a **Szerkesztés**gombra.
5. Az előugró ablakot, a válasz **Igen** a szabály másolatot szeretne készíteni.
6. A **Leírás** lapon módosítsa elsőbbségi még nem használt érték, például 50.
7. Kattintson a **Scoping szűrőt** a bal oldali navigációs. Kattintson a **záradékot kell megadnia**, attribútum válassza a **levelek**, a operátor választó **ENDSWITH**és a azonosító típusa **@contoso.com**. Kattintson a **záradékot kell megadnia**, attribútum választó **userPrincipalName**, operátor választó **ENDSWITH**és azonosító típusa **@contoso.com**.
8. Kattintson a **Mentés**gombra.
9. A konfiguráció befejezéséhez [alkalmazása és a módosítások ellenőrzéséhez](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Alkalmazása és a módosítások ellenőrzéséhez
Miután kiválasztotta a módosításokat, ezek az objektumok már szerepel a rendszer kell alkalmazni. Az is előfordulhat, hogy jelenleg nem a szinkronizálási motor objektumok célszerű lehet feldolgozni, és a szinkronizálási motor kell olvasni a forrás rendszer ismét a megerősítéséhez a tartalmát.

Ha a **tartomány** vagy a **szervezeti egység** szűrés konfigurációs módosította, majd szüksége **teljes importálás** **Delta szinkronizálási**követ.

Ha módosított konfigurációs **attribútum** szűrés használatával, majd szüksége **teljes**szinkronizálást.

Kövesse az alábbi lépéseket:

1. Indítsa el a **Szinkronizálási szolgáltatás** a start menüből.
2. Jelölje ki az **összekötőket** , és **összekötők** listájában jelölje ki az összekötőt, a korábbi módosítása konfiguráció jelzik. A **Műveletek**csoportban válassza a **Futtatás**parancsot.  
![Összekötő futtatása](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. A **Profilok futtatása**válassza ki a műveletet, a korábbi részében. Ha két művelet futtatásához szükséges, futtassa az első szakasz után a második (az **állapot** oszlopban a a kijelölt összekötő **Inaktív** ).

A szinkronizálás után minden módosítás exportálandó elő készítve. Mielőtt a ténylegesen változtatásokat az Azure Active Directory, ellenőrizze, hogy helyesek-e ezek a változások szeretne.

1. Egy cmd kérdés és megnyitásához`%Program Files%\Microsoft Azure AD Sync\bin`
2. Futtatása:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Az összekötő neve szinkronizálás szolgáltatásban találhatók. Van egy hasonló "contoso.com – AAD" nevet az Azure Active Directory.
3. Futtatása:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Ekkor a % temp %, amely a Microsoft Excel is vizsgálni export.csv nevű fájl. Ez a fájl lehet exportálni kívánt minden módosítást tartalmaz.
5. Az adatok és a konfigurációs el a szükséges módosításokat, és ezeket a lépéseket ismét (importálás, szinkronizálása és ellenőrzés) futtatása mindaddig, amíg a módosításokat, amely lehet exportálni kívánt várják.

Ha meg elégedve, exportálása Azure AD a módosításokat.

1. Jelölje ki az **összekötőket** , és **összekötők** listájában válassza az Azure Active Directory-összekötő. A **Műveletek**csoportban válassza a **Futtatás**parancsot.
2. A **profilok futtatni**jelölje ki a **exportálása**parancsot.
3. Ha módosításokat sok objektumok törlése, majd hibaüzenet jelenik meg az Exportálás a Ha a szám nem csak a megadott küszöbértékét (Ez alapértelmezés szerint 500). Ha ez a hiba jelenik meg, majd meg kell ideiglenesen tiltsa le a [véletlen törlés megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)szolgáltatás.

Most pedig ahhoz, hogy az ütemező újra.

1. Indítsa el a **Feladatütemező** a start menüből.
2. Közvetlenül a **Feladatütemező könyvtár**, keresse meg a tevékenység neve **Azure AD-szinkronizálás ütemezőt**, kattintson a jobb gombbal és **engedélyezéséhez**jelölje be.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
