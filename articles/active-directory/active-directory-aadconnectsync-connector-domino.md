<properties
   pageTitle="Lotus Domino-összekötő |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan kell konfigurálni a Microsoft Lotus Domino-összekötő."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino összekötő technikai útmutató
Ez a cikk ismerteti a Lotus Domino-összekötő. A cikk az alábbi termékek vonatkozik:

- A Microsoft Identitáskezelő 2016 (MIM2016)
- A Forefront identitáskezelő 2010 R2 (FIM2010R2)
    -   Használjon 4.1.3671.0 változata, illetve az újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2 az összekötő érhető el a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=717495)letöltésként.

## <a name="overview-of-the-lotus-domino-connector"></a>A Lotus Domino-összekötő áttekintése
A Lotus Domino Connector lehetővé teszi a szinkronizálási szolgáltatás integrálása az IBM Lotus Domino-kiszolgálóhoz.

A magas szintű szemszögéből az alábbi funkciók a kiadásban az összekötő által támogatott:

A szolgáltatás | Támogatás
--- | ---
Csatlakoztatott adatforrás | Kiszolgáló: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Ügyfél:<li>Lotus Notes 9.x</li>
Felhasználási területei | <li>Objektum életciklus-kezelése</li><li>Csoportok kezelése</li><li>Jelszó kezelése</li>
Műveletek | <li>Teljes és a Delta importálása</li><li>Exportálás</li><li>Beállítása és a HTTP jelszót a jelszó módosítása</li>
Séma | <li>Személy (partner (tanúsítvánnyal nem rendelkező személyek) központi felhasználó)</li><li>Csoport</li><li>Erőforrás (erőforrás, a helyiség, Online értekezletet)</li><li>A levelezési adatbázisok</li><li>Támogatott objektumok attribútumainak dinamikus felfedezése</li>

A Lotus Domino-összekötő a Lotus Notes ügyfél Lotus Domino kiszolgáló kommunikálni használja. Ez a függés következtében egy támogatott Lotus Notes-ügyfél a szinkronizálás kiszolgálón telepítve van. Az ügyfél és a kiszolgáló közötti kommunikáció alkalmazása a Lotus Notes .NET együttműködéshez (Interop.domino.dll) kapcsolaton keresztül. A kapcsolat a megkönnyíti a Microsoft.NET platform és a Lotus Notes-ügyfelek közötti kommunikáció, és a hozzáférés Lotus Domino dokumentumait és nézeteit. Delta importálásra hogy az is lehetséges, hogy használt-e a C++ natív felület (attól függően, hogy a kijelölt delta importálása módszer).

### <a name="prerequisites"></a>Előfeltételek
Mielőtt az összekötő használni, győződjön meg arról, hogy a szinkronizálás kiszolgálón a következő:

- 4.5.2 a Microsoft .NET-keretrendszer vagy újabb verzió
- A Lotus Notes ügyfél telepítenie kell a szinkronizálás kiszolgálón
- A Lotus Domino-összekötő az alapértelmezett Lotus Domino LDAP séma adatbázis (schema.nsf) jelen Domino címtár-kiszolgálón szükséges. Ha nem található fut, vagy indítsa újra az LDAP szolgáltatást a Domino-kiszolgáló által telepítheti.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás-engedélyek
A támogatott műveletek elvégzését Lotus Domino Connector, meg kell az alábbi csoport tagja:

- Teljes hozzáférés-rendszergazdák
- A rendszergazdák
- Adatbázis-rendszergazdák

A következő táblázat felsorolja a az egyes műveletek szükséges engedélyekkel:

Művelet | Az Access jogok
--- | ---
Importálás | <li>Nyilvános dokumentumok olvasása</li><li> Teljes hozzáférés rendszergazda (miután teljes hozzáférést biztosít a Rendszergazdák csoport tagjának, akkor automatikusan hozzáférést a hatékony a vezérlés.)</li>
Exportálhatja, és a jelszó megadása | Hatékony hozzáférés: <li>Dokumentumok létrehozása</li><li>Dokumentumok törlése</li><li>Nyilvános dokumentumok olvasása</li><li>Nyilvános dokumentumok írása</li><li>Replikáció vagy másolás dokumentumok</li>Az exportálási műveletekhez a következő szerepkörök is szükséges: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Műveletek közvetlen és AdminP
A művelet akár lépjen közvetlenül az Domino könyvtár vagy AdminP folyamatát. Az alábbi táblázatok lista összes támogatott objektumok, műveletek és szükség esetén a kapcsolódó végrehajtása módszer:

**Elsődleges címjegyzék**

Objektum | Hozzon létre | Frissítés | Törlése
--- | --- | --- | ---
Személy | AdminP | Közvetlen | AdminP
Csoport | AdminP | Közvetlen | AdminP
MailInDB | Közvetlen | Közvetlen | Közvetlen
Erőforrás | AdminP | Közvetlen | AdminP

**Másodlagos címjegyzék**

Objektum | Hozzon létre | Frissítés | Törlése
--- | --- | --- | ---
Személy | A #HIÁNYZIK | Közvetlen | Közvetlen
Csoport | Közvetlen | Közvetlen | Közvetlen
MailInDB | Közvetlen | Közvetlen | Közvetlen
Erőforrás | A #HIÁNYZIK | A #HIÁNYZIK | A #HIÁNYZIK

Erőforrás létrehozásakor jegyzetek dokumentum jön létre. Hasonlóképpen ha töröl egy erőforrást, törlődik a jegyzetek dokumentumot.

### <a name="ports-and-protocols"></a>Portok és protokollok
Az IBM Lotus Notes-ügyfél és Domino-kiszolgálók kommunikáció jegyzetek távoli eljárás hívás (NRPC) hol NRPC kell használnia a TCP/IP használatával. Az alapértelmezett portszámot 1352, de a Domino-rendszergazda által módosítható.

### <a name="not-supported"></a>Nem támogatott
A Lotus Domino-összekötő a jelenlegi verziójában nem támogatja a következő műveleteket:

- Helyezze a postaláda-kiszolgálók között.

## <a name="create-a-new-connector"></a>Hozzon létre egy új összekötőt

### <a name="client-software-installation-and-configuration"></a>Ügyfél szoftverek telepítése és konfigurálása
Lotus Notes-ról kell telepíteni a kiszolgálón **előtt** az összekötő telepítve van.

Ha telepíti, feltétlenül megadható, hogy a **Felhasználók telepíthetik**. Az alapértelmezett **Többfelhasználós telepítése** nem működik.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

A szolgáltatások lapon telepítse az csak a szükséges Lotus Notes-szolgáltatások és az **Ügyfél egyszeri bejelentkezési**. Egyszeri bejelentkezés szükség az összekötő engedélyezni szeretné a Domino-kiszolgálóra való bejelentkezéshez.  
![Notes2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Megjegyzés:** Egyszer kezdje egy felhasználó a fiókot, az összekötő szolgáltatásfiók használata ugyanazon a kiszolgálón tárolt Lotus Notes-ról. Győződjön meg arról is a Lotus Notes-ügyfél, a kiszolgáló bezárásához. Az összekötő próbálja meg a Domino-kiszolgálóhoz való csatlakozáshoz egyszerre nem működik.

### <a name="create-connector"></a>Összekötő létrehozása
Lotus Domino összekötő létrehozásához **Szinkronizálási szolgáltatás** válassza a **Kezelés ügynök** és **létrehozása**. Jelölje ki a **Lotus Domino (Microsoft)** összekötőt.  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Ha a szinkronizálási szolgáltatás verziójától kínál az azt jelenti, hogy konfigurálása **architektúrája**, ellenőrizze, hogy az összekötő **folyamat**futtatása értéke az alapértelmezett érték.

### <a name="connectivity"></a>Kapcsolat
A kapcsolat lapon adja meg a Lotus Domino-kiszolgáló neve és írja be a bejelentkezési hitelesítő adatait.  
![Kapcsolat](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

A kiszolgáló Domino tulajdonság két-formátumokat támogatja a kiszolgáló neve:

- Kiszolgálónév
- Kiszolgálónév/könyvtárnév

**Kiszolgálónév/könyvtárnév** formátuma az előnyben részesített formátum az attribútum mivel gyorsabban választ, amikor az összekötő kapcsolódik a Domino-kiszolgálóhoz.

A megadott felhasználóazonosító fájlt a szinkronizálási szolgáltatás a konfigurációs-adatbázisban vannak tárolva.

**Delta** importáláshoz telepítette ezeket a beállításokat:

- A **Nincs lehetőséget**. Az összekötő nem minden delta import.
- **frissítés/Frissítés**. Az összekötő hatással van delta importálás hozzáadása, és frissítse a műveletek. Törlés a **Teljes importálási** művelet szükség. Ez a művelet a .net együttműködéshez használ.
- **Hozzáadása és frissítése és törlése**. Az összekötő hatással van delta importálás hozzáadása, frissítése és törlése a műveletek. Ez a művelet a natív C++ felületek használ.

A **Séma beállítások** , a következő lehetőségei vannak:

- Az **alapértelmezett séma**. Az összekötő észleli a Domino-kiszolgálóról a sémában. Az alapértelmezett beállítás.
- **DSML sémában**. Csak akkor használja, ha a Domino-kiszolgáló nem jelenítik meg a sémában. Ezután hozzon létre egy DSML fájlt a séma és inkább importálja. DSML kapcsolatos további tudnivalókért olvassa el a [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml)című témakört.

Kattintson a Tovább gombra, amikor a rendszer ellenőrzi a felhasználónév és jelszó konfigurációs paramétereket.

### <a name="global-parameters"></a>A globális paraméterek
A globális paraméterek lapon kell konfigurálni az időzóna és az importálás és exportálási művelet lehetőség.  
![A globális paraméterek](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

A **Domino a kiszolgálón beállított időzónát** paraméter határoz meg, hogy a Domino-kiszolgáló helyét.

Ez a beállítás szükség a támogatási **delta importálása** műveletek, mert a szinkronizálás lehetővé teszi szolgáltatás közötti utolsó két import változásokkal meghatározni.

#### <a name="import-settings-method"></a>Beállítások importálása módszer
A **Végrehajtásához teljes importálása által** beállításokkal foglalja magában:

- Keresés
- Nézet (ajánlott)

**Keresés** Domino az indexelő használ, de közös, hogy valós időben nem frissülnek az indexek és a kiszolgálóról visszaadott adatok nem mindig helyes-e. Sok módosításokkal rendszer a beállítás általában jól nem működik, és hamis törli bizonyos helyzetekben nyújt. **Keresés** viszont **Nézet**gyorsabb.

**Nézet** módszer az ajánlott óta adat helyes állapotának biztosít. Érdemes kissé lassabb keresés.

#### <a name="creation-of-virtual-contact-objects"></a>A virtuális kapcsolattartói objektumok létrehozása
A **hozható létre az \_partner objektum** beállításokkal foglalja magában:

- Nincs lehetőség
- Nem hivatkozás értékek
- Hivatkozás és a nem hivatkozás értékek

A Domino a hivatkozás attribútumok tartalmazhatnak formátumokban más objektumok hivatkozni szeretne. Engedélyezni szeretné a különböző változatok, az összekötő eszközök képviseli \_lépjen kapcsolatba az objektumokat, más néven **Virtuális partnerek** (VC). Az objektumok létrehozott, így azok is csatlakozni tudnak a meglévő té Elemet objektumok vagy új objektumként tervezett. Ezzel a módszerrel attribútum hivatkozások megőrizhetők.

A beállítás engedélyezése és a tartalom hivatkozás attribútum formátuma nem DN, ha egy \_partner objektum jön létre. Egy tag attribútum csoport például SMTP-címeket is tartalmazhat. Akkor is, hogy a rövid_név és más attribútumait hivatkozás attribútumok szerepel. Ebben az esetben a **Hivatkozás nem értékek**kijelölése Ez a beállítás az Domino megvalósítás leggyakoribb beállítása.

Ha Lotus Domino van konfigurálva, hogy külön címjegyzékek ugyanazon objektumhoz, amely különböző megkülönböztető névvel rendelkező, lehetőség is létrehozhat \_lépjen kapcsolatba az objektumok, amelyek szerepelnek a címjegyzék összes hivatkozást az értékek. Ebben az esetben jelölje ki a **és a nem-hivatkozási** lehetőséget.

A **név** Domino az attribútum Ha több értéket, majd is engedélyezni szeretné virtuális névjegyek létrehozása, hivatkozásokat, amelyeket tud feloldani. Ha például az attribútum beállíthatja, hogy több érték házasság vagy válás után. Jelölje ki a **engedélyezése jelölőnégyzetet. Több értéket tartalmaz, név** példánkban.

A helyes attribútumain összefűzésével a \_kapcsolattartó objektum lenne csatlakozik a té Elemet objektum.

Következő objektumoknak VC =\_partner hozzáadni a DN.

#### <a name="import-settings-conflict-object"></a>Beállítások importálása, objektum ütköznek:
**Ütközés objektum kizárása**

Nagy Domino végrehajtására akkor lehet, hogy több objektum van-e az azonos DN replikációs problémák miatt. Ebben az esetben az összekötő két, különböző UniversalIDs, de azonos DN objektumok volna látható. Az ütközés okozhatja egy tranziens objektum összekötő térköz hoznak létre. Az összekötő figyelmen kívül hagyhatja a replikáció áldozatok mint Domino a kijelölt objektumok. Az ajánlási legyen kijelölt ezt a jelölőnégyzetet.

#### <a name="export-settings"></a>Beállítások exportálása
A **Hivatkozások frissítésének használata AdminP** beállítás nincs bejelölve, ha exportálási hivatkozás attribútumok tag, például közvetlen hívás, és nem használja a AdminP folyamat. Csak akkor használja ezt a lehetőséget, ha AdminP nincs beállítva a hivatkozási integritás megőrzéséhez.

#### <a name="routing-information"></a>Útválasztási adatok
A Domino akkor lehet, hogy egy hivatkozás attribútum van beágyazva, a DN egy utótag útválasztási adatok. Ha például a csoport tagsági attribútum tartalmazó **CN=example/organization@ABC**. Az utótag @ABC útválasztási adatok. A útválasztási adatok Domino e-maileket küldhet a megfelelő Domino rendszer, amely lehet a rendszer egy másik szervezet használt. A útválasztási adatok mezőben adja meg a az összekötő hatóköre a vállalatnál használt útválasztási mértékegységeket. Ha ezen értékek egyikét megtalálható utótag beállítva, egy hivatkozás attribútum, a útválasztási adatok törlődik a hivatkozást. Egy hivatkozás értéket az útválasztási utótag nem feleltethetők egy ezeket az értékeket adja meg, ha egy \_partner objektum jön létre. Ezek \_partner objektumok létre ** RO=@ ** a DN illeszteni. Ezeknél \_partner objektumok, az alábbi attribútumok is hozzáadódnak engedélyezése a csatlakozás a tényleges objektumra, szükség esetén: \_routingName, \_contactName, \_displayName, és UniversalID.

#### <a name="additional-address-books"></a>További címjegyzékek
Ha nincs **címtár támogatási** telepítette, másodlagos címjegyzékek nevét adja meg, amelyek ezután manuálisan megadhatja címjegyzékek.

#### <a name="multivalued-transformation"></a>Többértékű transzformációt hajt végre.
Lotus Domino sok attribútumok többértékű. A megfelelő metaverse attribútumok általában egyetlen értéket tartalmazó. Úgy konfigurálja az importálás és az exportálási művelet beállítást, lehetősége van engedélyezni a szóban forgó attribútumai szükséges fordítás segítség az összekötő.

**Exportálás**  
Az exportálási művelet beállítást kétféleképpen támogatja:

- Elem összefűzése
- Elem cseréje

**Elem cseréje** – Ha bejelöli ezt a beállítást, az összekötőt mindig távolítsa el az aktuális értékeket az attribútum Domino, és cserélje le azokat a megadott értékekkel. A megadott értékelni, lehet, hogy egyetlen értékű vagy többértékű.

Példa: A Segéd egy személy objektum attribútum az alábbi értékeket:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = János Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ha egy új asszisztens **David Alexander** nevű személy objektum van rendelve, eredménye:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Elem hozzáfűzése** – Ha bejelöli ezt a beállítást, az összekötő megőrzi a Domino attribútum a meglévő értékeket, és új értékek beillesztése az adatok lista tetején.

Példa: A Segéd egy személy objektum attribútum az alábbi értékeket:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = János Smith/OU=Contoso/O=Americas,NAB=names.nsf

Ha egy új asszisztens **David Alexander** nevű személy objektum van rendelve, eredménye:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = János Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importálás**  
Az importálási művelet beállítás kétféleképpen támogatja:

- Alapértelmezett
- Többértékű egyetlen értéket

**Alapértelmezett** – Ha az alapértelmezett beállítást választja, az összes összes attribútum értékei importált.

Egy egyetlen értékű attribútum **Multivalued egyetlen értéket** – Ha bejelöli ezt a választógombot, a többértékű attribútum alakul. Több érték szerepel-e, ha az érték (Ez az érték a szokásos is legkésőbbi) felső használják.

Példa: A Segéd egy személy objektum attribútum az alábbi értékeket:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = János Smith/OU=Contoso/O=Americas,NAB=names.nsf

A legutóbbi módosítás attribútum **David Alexander**. Az importálási művelet beállítás Multivalued egyetlen értékre van állítva, mert összekötő csak importálja **David Alexander** az összekötő területre.

A többértékű attribútum alakítani egyetlen értékű attribútumok logika nem vonatkozik a csoport tagsági attribútum, és a személy név attribútum.

Azt is lehetséges importálása és exportálása átalakítása vonatkozó szabályok többértékű attribútumait per tulajdonság, a globális szabály kivételként. Adja meg ezt a lehetőséget, írja be a [objektumtípus]. [attribútumnév] a **kizárási attribútum lista importálása** és **exportálása az kizárási attribútum lista** mezőben. Ha például Person.Assistant adja meg, és a globális jelző be van állítva, az összes érték importálni, ha csak az első érték a program importálja a Segédhez.

#### <a name="certifiers"></a>Certifiers
Az összekötő minden szervezeti/szervezeti egység sorolja fel. Személy objektumok exportálása a elsődleges címjegyzék engedélyezni a jelszóval egy certifier szükség.

Ha az összes certifiers ugyanazt a jelszót, a **jelszót az összes Certifers** használható. Ezután írja be ide, és adja meg csak a certifier fájlt.

Ha csak importálja, majd nincs bármely certifiers megadásához.

### <a name="configure-provisioning-hierarchy"></a>Állítsa be a kiépítési hierarchia
A Lotus Domino összekötő konfigurálásakor ugorja át a párbeszédpanel lapon. A Lotus Domino összekötő nem támogatja a kiépítési hierarchia.  
![A kiépítési hierarchiája](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciót és hierarchiák beállítása:
Partíciók és hierarchiákat konfigurálásakor ki kell választania a NAB=names.nsf néven elsődleges címjegyzékben. Mellett az elsődleges címjegyzék ha vannak ilyenek választhat másodlagos címjegyzékeket.  
![Partíciót](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Jelölje ki a attribútumok
Az attribútumok konfigurálásakor ki kell választania a elé attribútumainak ** \_MMS\_**. Következő attribútumok szükségesek, ha Ön kiépítése új Lotus Domino-objektumok

![Attribútumok](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objektum életciklus-kezelése
Ez a témakör a különböző objektumok Domino áttekintése.

### <a name="person-objects"></a>Személy objektumok
A személy objektum jelzi a felhasználók számára a szervezet és a szervezeti egységre. Alapértelmezett attribútumait mellett a Domino rendszergazda egyéni attribútumok is adhat egy személy-objektumot. Legalább egy személy objektum összes kötelező attribútum kell tartalmaznia. Kötelező attribútum listáját olvassa el a [Lotus Notes-tulajdonságok](#lotus-notes-properties)című témakört. Egy személy objektum regisztrációhoz az alábbi előfeltételek teljesülniük kell:

- A címjegyzék (names.nsf) kell meghatározva, és az elsődleges címjegyzék kell lennie.
- Rendelkeznie kell a O/OU certifier azonosító és a jelszót egy felhasználó regisztrálhatja a szervezet / szervezeti egység.
- A Lotus Notes-tulajdonságok személy objektum meghatározott be kell állítani. A Tulajdonságok kiépítési a személy objektum segítségével. További információt a dokumentumon belül a [Lotus Notes-tulajdonságok](#lotus-notes-properties) nevű című.
- A kezdeti HTTP személy jelszava-attribútum és a kiépítési során.
- A személy objektum kell lennie a következő három támogatott típusú egyikét:
    1. Normál felhasználót, hogy egy levelezési fájlt és a felhasználói azonosító
    2. Központi felhasználói (normál felhasználó tartalmazza az összes központi adatbázisfájlok)
    3. Partnerek (nincs azonosító fájl rendelkező felhasználóként)

Személyek (kivéve a partnerek lapon) további sorolhatók US és nemzetközi felhasználók értékük alapján meghatározott a \_MMS\_IDRegType tulajdonság. Ezen személyek a jegyzetek ügyfél Lotus Domino-kiszolgálók, eléréséhez van a jegyzetek azonosító, és egy személy dokumentumot. Ha jegyzeteket mail, majd azok is levelezési fájl. A felhasználónak kell regisztrálni a aktívvá válik. További tudnivalókért lásd:

- [Jegyzetek felhasználók beállítása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Felhasználói regisztráció](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Felhasználók kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Felhasználók átnevezése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Ezek a műveletek elvégezni a Lotus Domino és majd importálja a szinkronizálási szolgáltatás.

### <a name="resources-and-rooms"></a>Erőforrások és a helyiségek
Egy erőforrás egy Lotus Domino egy adatbázist, egy másik típusú. Erőforrások a berendezések, például kivetítő különféle típusú üléstermek lehetnek. Vannak Lotus Domino connector által támogatott források, az erőforrás típusa attribútum definiált altípus közül:

Erőforrás típusa | Erőforrás típusa attribútum
--- | ---
Szoba | 1
Erőforrás (egyéb) | 2
Online értekezlet | 3

Az erőforrás objektumtípus a munkát, az alábbiakra szükség:

- Erőforrás-foglaláshoz adatbázis már szerepel az összekapcsolt Domino-kiszolgáló
- A webhelyen már van definiálva az erőforrás

Az erőforrás-foglaláshoz adatbázis háromféle dokumentumokat tartalmazza:

- Profil a webhelyen
- Erőforrás
- Foglalás

Erőforrás-foglaláshoz adatbázis beállításával kapcsolatos további tájékoztatásért lásd: [az erőforrás zárolások adatbázis beállítása](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Létrehozása, frissítése, és erőforrások törlése**  
A létrehozás, frissítése és törlése műveleteket végzi a Lotus Domino összekötőre, az erőforrás-foglaláshoz adatbázisban. Források az (Ez azt jelenti, hogy az elsődleges címjegyzék) Names.nsf dokumentumként jönnek létre. Ha többet szeretne tudni a szerkesztése és törlése az erőforrásokat lásd: [szerkesztése és törlése az erőforrás-dokumentumok](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importálása és exportálása az erőforrásokhoz tartozó művelet**  
Az erőforrások importálja vagy exportálja a szinkronizálás szolgáltatásból, mint bármely más objektum típusa lehetnek. Jelölje be az objektum típusa erőforrásként konfigurálása során. Az Exportálás művelethez rendelkeznie kell a részleteket az erőforrás típusa, a konferencia adatbázis és a webhely neve.

### <a name="mail-in-databases"></a>A levelezési adatbázisok
A levelezési adatbázis egy adatbázist, amely a levelek fogadására lett tervezve. A Lotus Domino-postaládáját, hogy a program nem társítja bármely Lotus Domino adott felhasználói fiók (Ez azt jelenti, hogy ne jelenjen meg saját azonosító fájl- és jelszó). A levelezési adatbázis egy egyedi felhasználóazonosító ("rövid neve"), és a saját e-mail cím társítva van.

Ha a saját e-mail címet, amely a különböző felhasználók is megoszthatók külön postaládák szükség van (például group@contoso.com), a levelezési adatbázis jön létre. A hozzáférést a postaládához szabályozható keresztül a Access vezérlő lista (vezérlés), amely tartalmazza a kattintva nyissa meg a postaláda áll rendelkezésre a jegyzetek felhasználók nevét.

A szükséges attribútumokat listáját a [Kötelező attribútum](#mandatory-attributes) nevű a jelen cikk című.

Adatbázis a levelek fogadására tervezésekor Lotus Domino a levelezési adatbázisok dokumentum jön létre. A dokumentum minden egy másolatot az adatbázisról tároló kiszolgáló Domino-címtár léteznie kell. Részletes leírását a levelezési adatbázisok dokumentum létrehozása olvassa el a [levelek az adatbázis-dokumentum létrehozása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html)című témakört.

A levelezési adatbázis létrehozása, mielőtt az adatbázis már léteznie kell (kell hozta Lotus felügyeleti) a Domino-kiszolgálón.

### <a name="group-management"></a>Csoportok kezelése
A Lotus Domino-csoportok kezelése a részletes áttekintése is beolvashat az alábbi forrásokat:

- [Csoportok használata](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [A csoport létrehozása](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Létrehozó és módosító felhasználói csoportok](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Csoportok kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Csoport átnevezése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Jelszó kezelése
Egy regisztrált Lotus Domino felhasználói jelszavak két típusa létezik:

1. Felhasználó jelszavának (User.id-fájlban tárolt)
2. Internetes / HTTP jelszó

A Lotus Domino összekötő csak azokat a műveleteket, HTTP jelszóval támogatja.

Végezze el a jelszavak kezelését, a számítógépen engedélyeznie kell a jelszó kezelése az adatkezelési ügynök tervezőben az összekötő. Ahhoz, hogy a jelszavak kezelését, engedélyezze a **jelszavak kezelését** **Konfigurálása bővítmények** párbeszédpanel lapon.  
![Bővítmények konfigurálása](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

A Lotus Domino összekötő támogatás követő internetes jelszó műveleteket:

- Jelszó beállítása: Jelszó beállítása a felhasználó Domino a HTTP/Internet új jelszót állítja be. Alapértelmezés szerint a fiókot is a nem zárolt. A zárolás feloldása jelző van téve a szinkronizálási motor WMI felületén.
- Jelszó módosítása: Ebben az esetben a felhasználó megváltoztathatja a jelszó vagy a rendszer egy megadott idő után módosíthatja a jelszavát. Ez a művelet állapotba helyezze, mind (a régi és új jelszavát) kötelező. Miután módosította, a Lotus Domino frissül az új jelszót.

További tudnivalókért lásd:

- [Az Internet zárolása funkció használata](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internetes jelszavak kezelése](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Hivatkozási információk
Ez a szakasz sorolja fel, például az attribútumok leírása és a Lotus Domino összekötő attribútum követelményei.

### <a name="lotus-notes-properties"></a>A Lotus Notes-tulajdonságok
A Lotus Domino Directoryhoz, kiépítése személy objektumok, amikor az objektumok rendelkeznie a tulajdonságok egy adott csoportja töltve egyedi értékeket. Ezek az értékek csak a szükséges műveleteket létre.

Az alábbi táblázat felsorolja a tulajdonságok, és azok leírását tartalmazza.

A tulajdonság | Leírás
--- | ---
\_MMS_AltFullName | Felhasználó alternatív teljes nevét.
\_MMS_AltFullNameLanguage | A felhasználó alternatív teljes nevét tartalmazó használandó nyelvet.
\_MMS_CertDaysToExpire | Lejárta előtt a tanúsítványt az aktuális dátum közötti napok számát. Ha nincs megadva, akkor az alapértelmezett dátum az aktuális dátumtól két év.
\_MMS_Certifier | Tulajdonság, amely a certifier szervezeti hierarchia nevét tartalmazza. Példa: A szervezeti egységre = OrganizationUnit, O = szervezeti, C ország =.
\_MMS_IDPath | Ha a tulajdonság üres, nem felhasználói azonosító fájl a szinkronizálási kiszolgálón helyileg jön létre. Ha a tulajdonságot a fájl nevét tartalmazza, a felhasználói azonosító fájl a madata mappában jön létre. A tulajdonság is tartalmazhat, teljes elérési útját.
\_MMS_IDRegType | Személyek partnerek, a felhasználók US és a nemzetközi felhasználók fajtái. Az alábbi táblázat a lehetséges értékek: <li>0 - ügyfél</li><li>1 – USA-beli felhasználói</li><li>2 – nemzetközi felhasználói</li>
\_MMS_IDStoreType | Kötelező tulajdonsága az Amerikai Egyesült Államokban és nemzetközi felhasználók. A tulajdonság egy egész értéket tartalmaz, amely meghatározza, hogy a felhasználóazonosító tárolja a jegyzetek címjegyzék vagy az a személy e-mail fájlt mellékletként. A címjegyzék melléklet felhasználói azonosító fájl esetén tetszés szerint létrehozhatók-e a fájlként \_MMS_IDPath. <li>Üres - tár azonosítója fájl azonosító tárolóból elemre, nincs azonosító fájl (névjegyek használt).</li><li> 1 – mellékletet a jegyzetek címjegyzékben. A \_MMS_Password tulajdonságot be kell állítani a felhasználói azonosító fájlok, amelyek a mellékletek</li><li>2 – az a személy levelezési fájl azonosító tárolja. A \_MMS_UseAdminP meg kell hamis, hogy a levelezési fájl személy regisztrálásakor hozható létre. A \_MMS_Password tulajdonságot be kell állítani a felhasználói azonosító fájlokat.</li>
\_MMS_MailQuotaSizeLimit | Az e-mail fájl adatbázis áll rendelkezésre megabájt száma.
\_MMS_MailQuotaWarningThreshold | Az e-mail fájl adatbázis áll rendelkezésre, mielőtt egy figyelmeztetés megabájt száma.
\_MMS_MailTemplateName | A felhasználó e-mail fájl létrehozásához használt e-mail sablonfájl. Ha meg van adva egy sablont, a levelezési fájl a megadott sablon alapján hoz létre. Ha nincs sablon van megadva, az alapértelmezett sablon fájl használatával hozza létre a fájlt.
\_MMS_OU | A szervezeti neve alatt a certifier nem kötelező tulajdonsága. Ez a tulajdonság üres, a kapcsolatokat kell lennie.
\_MMS_Password | Felhasználó számára kötelező tulajdonsága. A tulajdonság a jelszót az azonosító fájl az objektum tartalmazza.
\_MMS_UseAdminP | IGAZ, ha a levelezési fájl kell létrehozni a AdminP folyamat a Domino-kiszolgáló (aszinkron szeretne az exportálási folyamat során) tulajdonság kell beállítani. Ha tulajdonság értéke HAMIS, a levelezési fájl jön létre a Domino felhasználóval (szinkron be az exportálási folyamat során).

A felhasználó egy társított azonosító fájlt a \_MMS_Password tulajdonság kell tartalmaznia egy értéket. E-mail eléréséhez a Lotus Notes ügyfél keresztül a felhasználó tulajdonságainak levelezokiszolgalo és MailFile értéket kell tartalmaznia.

E-mail eléréséhez böngészőn keresztül, a következő tulajdonságokat értékeket kell tartalmaznia:

- MailFile - kötelező tulajdonsága, amely tartalmazza az elérési utat a Posta fájlt tároló Lotus Domino-kiszolgálón.
- Levelezokiszolgalo - kötelező tulajdonsága, amely tartalmazza a Lotus Domino-kiszolgáló neve. Ez az érték a nevet, a Lotus levelezési fájl Domino kiszolgálói létrehozásakor.
- HTTPPassword - választható tulajdonság, amely tartalmazza az objektum Web access jelszavát.

A levelek videofunkcióinak nélkül Domino-kiszolgáló elérésére, a HTTPPassword tulajdonság értéket kell tartalmaznia. A MailFile tulajdonság és a levelezokiszolgalo tulajdonság üres is lehet.

A \_MMS_ IDStoreType = 2 (levelezési fájl id áruházból), a NotesRegistrationclass MailSystem tulajdonság értéke REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Kötelező attribútum
A Lotus Domino összekötő főként támogatja a következő objektumok (dokumentum tartalomtípusoknál):

- Csoport
- A levelezési adatbázisok
- Személy
- Kapcsolattartó (nincs certifier használó személy)
- Erőforrás

Ez a szakasz minden támogatott objektum exportálása Domino-kiszolgálón kötelező attribútum.

Objektumtípus | Kötelező attribútum
--- | ---
Csoport | <li>ListName</li>
Fő-adatbázisban | <li>Név</li><li>MailFile</li><li>Levelezokiszolgalo</li><li>MailDomain</li>
Személy | <li>Utónév</li><li>MailFile</li><li>Rövid_név</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Kapcsolattartó (nincs certifier használó személy) | <li>\_MMS_IDRegType</li>
Erőforrás | <li>Név</li><li>Erőforrástípus</li><li>ConfDB</li><li>Erőforráskapacitás</li><li>Webhely</li><li>DisplayName</li><li>MailFile</li><li>Levelezokiszolgalo</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Gyakori hibák és kérdések

### <a name="schema-detection-does-not-work"></a>Séma észlelése nem működik.
A séma feltárása tudja, hogy megtalálható-e a schema.nsf fájlt a Domino-kiszolgáló szükség. Ez a fájl csak akkor jelenik meg, ha LDAP telepítve van a kiszolgálón. Ha a séma nem észlelhető, ellenőrizze az alábbiakat:

- A fájl schema.nsf megtalálható a gyökérmappából Domino kiszolgáló
- A felhasználó rendelkezik engedélyekkel a schema.nsf fájl megtekintéséhez.
- Kényszerített újra kell indítani az LDAP-kiszolgálóval. Nyissa meg a **Lotus Domino konzol** , és frissítse a séma **Megállapítani, hogy LDAP ReloadSchema** paranccsal.

### <a name="not-all-secondary-address-books-are-visible"></a>Nem minden másodlagos címjegyzékek láthatók.
Az Domino-összekötő a **Címtár támogatási** engedélyezni szeretné a másodlagos címjegyzékek keresése funkció támaszkodik. Ha a másodlagos címjegyzékek hiányoznak, ellenőrizze, ha [Címtár támogatási](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) engedélyezett és a Domino-kiszolgálón beállított.

### <a name="custom-attributes-in-domino"></a>Egyéni attribútumok Domino
Ha ki szeretné terjeszteni a séma, így jelenik meg, az összekötő felhasználható egyéni attribútum Domino többféle módon szerepelnek.

**1 megközelítés: Lotus Domino séma bővítése**

1. Hozzon létre egy sablon másolati példányát Domino címtár {PUBNAMES. NTF} (kell az alapértelmezett sablont az IBM Lotus Domino könyvtár nem testre) következő [ezeket a lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) :
2. Nyissa meg a Másolás a Domino címtár sablont {CONTOSO. NTF}-sablon Domino tervezőben készült, és kövesse az alábbi lépéseket:
    - Kattintson a megosztott elemek és bontsa ki a segédűrlap
    - Kattintson duplán a {objektumnév} $InheritableSchema segédűrlapot (ahol a {objektumnév} annak a nevét, az alapértelmezett szerkezeti objektum osztály, például: személy).
    - Adjon nevet a séma {MyPersonAtrribute}, és a megfelelő attribútum, amely a hozzáadni kívánt attribútumot. Hozzon létre egy mezőt, válassza a **Create** menü, és válassza a **mező** menüből.
    - A hozzáadott mező tulajdonságainak beállítása típusának megfelelő, a stílus, a méretét, a betűtípus és a mező tulajdonságok ablak a többi kapcsolódó paraméter kiválasztásával.
    - Az alapértelmezett érték megegyezik az adott attribútum megadott neveként attribútum megtartása (például a Ha attribútumnév MyPersonAttribute, fontos az alapértelmezett érték azonos nevű).
    - Mentse a ${objektumnév} InheritableSchema segédűrlap frissített értékeket.
3. Lecseréli a Domino-címtár sablont {PUBNAMES. NTF} az új egyéni sablon {CONTOSO. NTF} szerint az alábbi [lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Zárja be a Domino-rendszergazda, és indítsa újra az LDAP szolgáltatást, és frissítse a LDAP séma Domino konzol megnyitása:
    - Domino konzoljában Beszúrás **Domino parancs** szöveg beállítást újraindítja LDAP - [Indítsa újra a tevékenység LDAP]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)parancsa.
    - LDAP betöltéséhez séma paranccsal megállapítani, hogy LDAP - LDAP ReloadSchema megállapítani, hogy
5. Megnyitott Domino Admin, és válassza a személyek és csoportok lapján, hozzáadott attribútum tükröződik domino megtekintéséhez adja hozzá a személy.
6. Nyissa meg a Schema.nsf a **fájlok** fülre, és látható a hozzáadott attribútum tükröződik dominoPerson LDAP objektum osztály be.

**2 megközelítés: Hozzon létre egy auxClass egyéni attribútum, és az objektum osztály társítása**

1. Hozzon létre egy sablon másolati példányát Domino címtár {PUBNAMES. NTF} következő [lépések](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) szerint (soha nem testreszabása az alapértelmezett az IBM Lotus Domino könyvtár sablon):
2. Nyissa meg a Másolás a Domino címtár sablont {CONTOSO. Sablon NTF} az Domino Designer hozza létre.
3. A bal oldali ablaktáblában válassza a megosztott kódot, majd a segédűrlapot.
4. Kattintson az új segédűrlapot
5. Hajtsa végre az alábbi módon adja meg az új segédűrlapot tulajdonságait:
    - Nyissa meg az új segédűrlapot tartalmazó, válassza a Tervező - segédűrlap tulajdonságai
    - A Name tulajdonság melletti adja meg a kiegészítő objektum osztály – például TestSubform nevét.
    - A "Felvétele a Beszúrás segédűrlap... párbeszédpanel" beállítás a beállítások tulajdonság megtartása
    - Szüntesse meg a beállítások tulajdonság "leképezési át a megjegyzések HTML."
    - Változatlanul a Tulajdonságok parancsot, és a segédűrlap tulajdonságai párbeszédpanel bezárásához.
    - Mentse és zárja be az új segédűrlapot.
6. Hajtsa végre az alábbi módon meghatározása a külső objektum osztály mező felvétele:
    - Nyissa meg a hozott létre segédűrlapot.
    - Válassza a létrehozása – mezőben.
    - Az alapvető tudnivalók lapon, a mező párbeszédpanelen a neve mellett adja meg bármely nevét, például: {MyPersonTestAttribute}.
    - A hozzáadott mező tulajdonságainak beállítása a típus, a stílus, a méretét, a betűtípus és a kapcsolódó tulajdonságok választásával.
    - Az alapértelmezett érték megegyezik az adott attribútum megadott neveként attribútum megtartása (például a Ha attribútumnév MyPersonTestAttribute, fontos az alapértelmezett érték azonos nevű).
    - A segédűrlap mentheti a frissített értékekkel, majd tegye a következőket:
        - A bal oldali ablaktáblában válassza a megosztott kódot, majd a segédűrlap
        - Jelölje be az új segédűrlapot, és válassza a Tervező - tervezés tulajdonságait.
        - Kattintson a bal oldalról a harmadik fülre, és válassza a **propagálása Ez a Tiltás kialakítás módosítása**.
7. Nyissa meg a ${objektumnév} ExtensibleSchema segédűrlapon ({objektumnév} esetén az alapértelmezett szerkezeti objektum osztály – például a személy neve).
8. Erőforrás beszúrása, és jelölje be a segédűrlapot (létrehozott, például – TestSubform), és mentse a ${objektumnév} ExtensibleSchema segédűrlapot.
9. Lecseréli a Domino-címtár sablont {PUBNAMES. NTF} az új egyéni sablon {CONTOSO. NTF} szerint az alábbi [lépéseket](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Zárja be a Domino-rendszergazda, és indítsa újra az LDAP szolgáltatást, és frissítse a LDAP séma Domino konzol megnyitása:
    - Domino konzoljában Beszúrás **Domino parancs** szöveg beállítást újraindítja LDAP - [Indítsa újra a tevékenység LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)parancsa.
    - LDAP betöltéséhez séma paranccsal megállapítani, hogy LDAP **Megállapítani, hogy LDAP ReloadSchema**.
11. Nyissa meg a Domino-rendszergazda, és válassza a személyek és csoportok fülre kattintva megtekintheti a hozzáadott attribútum tükröződik domino hozzáadása személy (a mások a fülre).
12. Nyissa meg a Schema.nsf a **fájlok** fülre, majd a hozzáadott attribútum tükröződik TestSubform LDAP kiegészítő objektum osztály alatt látható.

**Megközelítés 3: A ExtensibleObject osztály egyéni attribútum felvétele**

1. A legfelső szintű könyvtár alóli {Schema.nsf} fájl megnyitása
2. A bal oldali menüben **Összes séma a dokumentumok** csoportban jelölje be a LDAP objektum osztályok, és kattintson a **osztály objektum hozzáadása** gomb:
3. Adja meg az LDAP nevét (ahol zzz az alapértelmezett szerkezeti objektum osztály, például a személy nevét a) {zzzExtensibleSchema} formájában. Ha ki szeretné terjeszteni a séma személy objektum osztály, például LDAP nevének {PersonExtensibleSchema} megadására.
4. Adja meg a felettes osztály objektumnév, ha ki szeretné terjeszteni a séma kívánt. Például ha ki szeretné terjeszteni a séma személy objektum osztály, adja meg a felettese objektum osztálynév {dominoPerson}:
5. Adja meg az objektum osztály megfelelő egy érvényes Objektumazonosító.
6. Jelölje ki a bővített/egyéni attribútumok szerinti kötelező kötelező és választható attribútum típusú mezők területen:
7. Után a szükséges attribútumokat ad a ExtensibleObjectClass, kattintson a **Mentés és Bezárás**gombra.
8. Egy ExtensibleObjectClass megfelelő alapértelmezett objektum osztály kiterjesztett attribútumokkal rendelkező jön létre.

## <a name="troubleshooting"></a>Hibaelhárítás

-   Kapcsolatos hibák elhárítása az összekötő naplózás engedélyezése a további tudnivalókért lásd az [annak engedélyezése esemény-nyomkövetés nyomon követése az összekötőkhöz](http://go.microsoft.com/fwlink/?LinkId=335731).
