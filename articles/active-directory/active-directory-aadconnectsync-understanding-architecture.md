<properties
   pageTitle="Azure AD Connect szinkronizálása: a architektúra ismertetése |} Microsoft Azure"
   description="Ez a témakör ismerteti az Azure AD Connect szinkronizálási architektúrája és a kifejezések."
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
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect szinkronizálási: a architektúra ismertetése
Ez a témakör bemutatja az Azure AD Connect szinkronizálási egyszerű architektúrája. Több szempontból hasonlít a Megelőzők MIIS 2003, 2007-es ILM és FIM 2010. Azure AD Connect szinkronizálási e technológiák alakulása. Ha ismeri a ezen korábbi technológiák közül, ez a témakör tartalma lesz ismerős lehet is. Ha először a szinkronizálást, majd a témakör szól. Azonban nem követelmény, hogy ez a témakör a testreszabási Azure AD Connect sync (szinkronizálás motor neve a jelen témakör) való sikeres részleteit.

## <a name="architecture"></a>Architektúra
A sync engine több csatlakoztatott adatforrásból a tárolt objektumok integrált nézetének létrehozása, és személyes információk az adatforrások kezeli. A beépített nézet az azonosító adatok beolvasása a csatlakoztatott adatforrások és szabályhalmaz, ez az információ feldolgozási módjának meghatározása határozza meg.

### <a name="connected-data-sources-and-connectors"></a>Csatlakoztatott adatforrás és összekötők
A sync engine folyamatok identitás adatait különböző tárházakban, például az Active Directory- vagy SQL Server-adatbázishoz. Minden adat tárházba, amely rendezi, hogy az adatok adatbázis hasonló formátumban, és, amely a szokásos adathozzáférés módszert kínál a szinkronizálási motor potenciális adatok forrása jelölt. A szinkronizálási motor által szinkronizált adatok tárházakban úgynevezett **adatforrásokhoz kapcsolt** vagy a **csatlakoztatott könyvtárak** (CD-hez).

A sync engine **összekötő**nevű modulon belül csatlakoztatott adatforrás interakció ágyaz be. Különböző típusú csatlakoztatott adatforrás csatlakozóval egy adott. Az összekötő megfelelője a szükséges műveletet, amelyet a csatlakoztatott adatforrás értelmez formátumba.

Összekötők hívásokat API (olvasási és írási) azonosító információkat Exchange rendelkező csatlakoztatott adatforrás. Is érdemes lehet hozzáadni a bővíthető kapcsolódási keretrendszer használatával egyéni összekötő. Az alábbi ábrán egy összekötőt kapcsolódását csatlakoztatott adatforrás a szinkronizálási motor.

![Ossz1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Mindkét irányba adatfolyam is, de azt nem lehet flow mindkét irányba egyidejű. Más szóval összekötő beállítható úgy, hogy az adatokat a csatlakoztatott adatforrás sync engine vagy a csatlakoztatott adatforrás sync engine flow engedélyezése, de ezek a műveletek közül csak akkor fordulhat elő, egyszerre egy objektum és attribútum. Lehet, hogy az egyes objektumokhoz és a különböző attribútumok különböző irányát.

Adja meg egy összekötőre, adja meg a szinkronizálni kívánt objektumtípusok. Olyan objektumok, amelyek szerepelnek a szinkronizálás hatókörének adja meg az objektum típusa határozza meg. A következő lépésként jelölje ki a jellemzőket szinkronizálni, egy attribútum listára nevezik. Ezeket a beállításokat módosíthatja bármikor nyomon követhetik a vállalati szabályokat. Ha használja az Azure AD Connect telepítővarázslóban, ezekkel a beállításokkal konfigurálták.

Objektumok csatlakoztatott adatforrás exportálásához az attribútum listára tartalmaznia kell legalább egy adott objektumtípus csatlakoztatott adatforrás létrehozásához szükséges minimális attribútumok. Ha például az **sAMAccountName** attribútummal szerepelnie kell a attribútum listára a user objektumban exportálása az Active Directory, mert az Active Directory objektumait felhasználói definiált **sAMAccountName** attribútummal kell rendelkeznie. Ismét a telepítés nem ebben a konfigurációban meg.

Ha a csatlakoztatott adatforrás szerkezeti összetevőket használ, például partíciót vagy tároló –-objektumok rendszerezésére, amelyeket egy adott megoldás a csatlakoztatott adatforrás területein korlátozhatja.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>A sync engine névtér belső szerkezete
A teljes sync engine névtér áll, amely a személyes adatok tárolására két névtér. A két névtér a következők:

- Az összekötő helyet (CS)
- A metaverse (té Elemet)

Az **összekötő hely** -e egy átmeneti területen, amely a kijelölt objektumok megadott csatlakoztatott adatforrás és a megadott attribútum elküldésének listában tulajdonságait tartalmazza. A sync engine összekötő térköz használ, megállapíthatja, hogy mi változott az csatlakoztatott adatforrás és előkészítése a bejövő módosításokat. A sync engine is használja a összekötő szóköz kimenő változások exportáláshoz a csatlakoztatott adatforrás előkészítése. A sync engine átmeneti területként különböző összekötő szóközt megtartja az egyes összekötő.

A fejlesztői terület használatával a szinkronizálási motor marad, függetlenül a csatlakoztatott adatforrásból, és nem érinti a elérhetőségét, és a kisegítő lehetőségek. Identitás adatait bármikor eredményt adja, az adatokat az átmeneti terület használatával is feldolgozása. A sync engine kérhet csak módosításainak a csatlakoztatott adatforrás belül az utolsó végződő kommunikációs munkamenet vagy csak a változásokat meg leküldéses óta identitás információkat még nem kapta meg a csatlakoztatott adatforrás, amely csökkenti a forgalmat a szinkronizálási motor és a csatlakoztatott adatforrás között.

Ezeken kívül sync engine tárolja, hogy a összekötő szóköz szakaszában az összes objektum állapotinformációt. Új adatok érkezésekor sync engine mindig értékeli ki, hogy az adatok már szinkronizálva.

A **metaverse** egy több csatlakoztatott adatforrásból származó, minden egyesített objektumok egyetlen globális, integrált nézetének nyújtó összesített azonosító információkat tartalmazó tárolás területen. Metaverse objektumok jönnek létre, amely a csatlakoztatott adatforrások és szabályok, amelyek lehetővé teszik a szinkronizálás testreszabása beolvasása identitás adatok alapján.

Az alábbi ábrán az összekötő terület névtér és a metaverse névtér a szinkronizálási motor belül.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Szinkronizálási motor identitás-objektumok
A sync engine objektumai megadott vagy a csatlakoztatott adatforrás-objektumok vagy az az integrált nézet motor szinkronizált azokat az objektumokat. Minden sync engine objektum egy globálisan egyedi azonosítója (GUID) kell rendelkeznie. GUID adatintegritás és objektumok express kapcsolatának biztosítanak.

### <a name="connector-space-objects"></a>Összekötő terület objektumok
Ha szinkronizálási motor csatlakoztatott adatforrás kommunikál, felolvassa az identitás információt a csatlakoztatott adatforrás, és használja ezeket az információkat a megadott az identitás-objektum létrehozása az összekötő szóköz. Nem hozhat létre, és egyenként törölje ezeket az objektumokat. Azonban manuálisan törölhető a összekötő szóközt összes objektumát.

Az összekötő helyen minden objektumhoz tartozik-e két attribútumait:

- Egy globálisan egyedi azonosítója (GUID)
- Egy megkülönböztető név (DN)

Ha a csatlakoztatott adatforrás hozzárendel egy egyedi attribútum az objektum, az összekötő helyen objektumok is horgony tulajdonság. A horgony attribútum egyedileg kell azonosítania a csatlakoztatott adatforrás objektumának. A sync engine keresse meg az objektum megfelelő ábrázolása a csatlakoztatott adatforrás a horgony használja. Szinkronizálási motor feltételezi, hogy az objektum a horgony soha nem változik a élettartam az objektum fölé.

Az összekötők számos ismert egyedi azonosítót kaphat használatával automatikusan az egyes objektumokra horgony készítése, annak importálásakor. Ha például az Active Directory-összekötő a **objectGUID** attribútum használja horgony. Nem adja meg pontosan meghatározott egyedi azonosítót kaphat csatlakoztatott adatforrások horgony generációs megadhatja a Connector beállítása részeként.

Ebben a lehetőséget választja, a horgony épül egyikét, vagy az objektum további egyedi attribútumokkal írja be, sem változtatás, valamint hogy egyedileg azonosítja az objektumot, az összekötő helyre (például az alkalmazott számát vagy a felhasználói azonosító).

Egy összekötő terület objektum lehet az alábbiak egyikét:

- Egy átmeneti tárolásra szolgáló objektum
- Helyőrző

### <a name="staging-objects"></a>Objektumok előkészítése
Egy átmeneti tárolásra szolgáló objektum egy példányát a kijelölt objektum típusa jelzi a csatlakoztatott adatforrásból. A globálisan egyedi azonosítója, és a megkülönböztető név mellett egy átmeneti tárolásra szolgáló objektum mindig értéket tartalmaz, amely jelzi az objektum típusa

Mindig importálásra átmeneti-objektumhoz tartozik-e a horgony attribútum értéket. Szinkronizálási motor által újonnan kiépítéstől és te000129565 hoznak létre a csatlakoztatott adatforrás előkészítése objektumok nem rendelkezik a horgony attribútum értéket.

Átmeneti objektumok is végrehajtása üzleti attribútumok és műveleti információjának szinkronizálási motor által hajtsa végre a szinkronizálás aktuális értékét. Műveleti adatok közé tartozik az átmeneti tárolásra szolgáló objektum elő van készítve frissítések típusú jelző jelölők. Ha egy átmeneti tárolásra szolgáló objektum kapott új identitás információt a csatlakoztatott adatforrásból, amely még nem dolgozott, az objektum **importálása függőben**van megjelölve. Ha egy átmeneti tárolásra szolgáló objektum új azonosító információkat, amelyek a csatlakoztatott adatforrás még nem exportálja van, akkor **függőben exportálás**megjelölve.

Egy átmeneti tárolásra szolgáló objektum lehet egy importálási vagy exportálási objektum. A sync engine importálása objektum objektum információt a csatlakoztatott adatforrásból kapott használatával hoz létre. Amikor sync engine kap egy új objektum, amely megfelel a az összekötő a kijelölt objektum típusú információ, létrehoz az objektum importálása az összekötő helyen, a megadott a csatlakoztatott adatforrás az objektumot.

Az alábbi ábrán látható, amely a csatlakoztatott adatforrás az objektum importálása objektum.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

A sync engine objektum exportálása a metaverse objektum információk használatával hoz létre. Objektumok exportálása a következő kommunikációs munkamenetben exportálja a csatlakoztatott adatforrás. A sync engine szemszögéből objektumok exportálása nem léteznek a csatlakoztatott adatforrás még. Emiatt a horgony attribútum exportálás objektum nem érhető el. Az objektum kap sync engine, miután a csatlakoztatott adatforrás létrehoz egy egyedi a horgony attribútum az objektum.

Az alábbi ábrán látható, hogyan exportálás objektum van készült azonosító információkat a metaverse.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

A szinkronizálási motor által az objektumot a csatlakoztatott adatforrásból importintézkedésekkel megerősíti, az Exportálás az objektum. Objektumok exportálása válhat objektum importálása sync engine kap őket, hogy a csatlakoztatott adatforrásból származó a következő importálás közben.

### <a name="placeholders"></a>Helyőrzők
A sync engine objektumokra tárolása strukturálatlan névteret használja. Néhány csatlakoztatott adatforrásokhoz, például az Active Directory azonban a hierarchikus névteret használják. Hierarchikus névteret adatait alakíthat ki egy egyszerű, strukturálatlan névtér, szinkronizálási motor használ helyőrzők megőrzése a hierarchia.

Minden egyes helyőrző egy objektum hierarchikus neve szinkronizálás motor be nem importálja, de szükség a hierarchikus neve összeállításához összetevője (például szervezeti egység). Olyan objektumok, amelyek nem vannak átmeneti objektumok, az összekötő helyre az csatlakoztatott adatforrás hivatkozás által létrehozott szünetek kitöltés.

A sync engine is helyőrzők használja, amely még nem importálva hivatkozott objektumokra tárolása. Például ha szinkronizálási van beállítva, ha meg szeretné jeleníteni a kezelő attribútum az *Abbie Spencer* objektumra, és a kapott érték van olyan objektum, amely nem importálta még, például *CN Tóth Sperry, CN = = felhasználók, Adatközpont fabrikam Adatközpont = = hu*, a kezelő információkat helyőrzőként az összekötő helyen tárolja. Ha később importálja a kezelő objektumot, a helyőrző objektum felülírja az átmeneti tárolásra szolgáló objektum, amely a kezelő.

### <a name="metaverse-objects"></a>Metaverse objektumok
Egy metaverse objektum tartalmaz az összesített nézet szinkronizálási motor az összekötő helyen átmeneti tárolásra szolgáló objektum van. Szinkronizálási motor metaverse objektumok információk segítségével az objektum importálása hoz létre. Több összekötő terület objektumok csatolható egyetlen metaverse-objektum, de nem lehet csatolni egy összekötő terület objektum egynél több metaverse objektum.

Metaverse objektumok nem kell manuálisan létrehozott vagy törölt. A sync engine automatikusan törli a metaverse tartalmazó objektumok nem összekötő terület objektumokat mutató hivatkozást a összekötő helyen.

Egy megfelelő objektumtípusnak belül a metaverse csatlakoztatott adatforrás belüli objektumok megfeleltetéséhez sync engine biztosít egy bővíthető sémát az objektum típusa és társított attribútumokat előre definiált. Új objektumtípusok és metaverse objektumok attribútumainak hozhat létre. Attribútumok lehet egyetlen értékű vagy többértékű, és a attribútum típusú is karakterláncok, hivatkozások, számok és logikai értékeket.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Kapcsolatok átmeneti tárolásra szolgáló és metaverse objektumok között
A szinkronizálási motor névtere belül az adatfolyam szerint engedélyezve van a hivatkozás kapcsolat átmeneti tárolásra szolgáló és metaverse objektumok között. A fejlesztői metaverse objektum csatolt objektum neve egy **adatbázis-objektum** (vagy **összekötő objektum**). A fejlesztői csatolt objektumot, amely nem metaverse-objektum neve egy **objektum tartományról** (vagy **nem csatlakozó objektum**). Az illesztés és tartományról kifejezések nem összekeverni a az adatok importálása és exportálása egy csatlakoztatott címtárból felelős összekötők előnyben részesített.

Helyőrzők soha nem kapcsolódnak metaverse-objektum

Egy illesztett objektum magában foglalja a átmeneti tárolásra szolgáló objektum és csatolt kapcsolata metaverse egyetlen objektummá. Illesztett objektumok szinkronizálása egy összekötő terület objektum és egy metaverse objektum között attribútum értékei használhatók.

Amikor egy átmeneti tárolásra szolgáló objektum egy illesztett objektum változik a szinkronizálás során, attribútumok az átmeneti tárolásra szolgáló objektum és a metaverse objektum között is flow. Attribútum folyamat kétirányú, ezenkívül pedig attribútum szabályok importálása és attribútum szabályok exportálása.

Egy egyetlen összekötő terület objektumot csak egy metaverse objektum csatolható. Azonban minden metaverse objektum csatolható több összekötő terület objektum ugyanazon vagy másik összekötő szóközök, az alábbi ábrán látható módon.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Az átmeneti tárolásra szolgáló objektum és egy metaverse objektum csatolt viszonya állandó, ezért csak a megadott szabályok szerint el kell távolítani.

Egy kilépett célja egy átmeneti tárolásra szolgáló objektum, amely nem kapcsolódik metaverse objektumokat. Az attribútum kilépett objektum értékei nem feldolgozása bármilyen további, a metaverse belül. A csatlakoztatott adatforrás a megfelelő objektum attribútum értékeket nem frissülnek szinkronizálási motor által.

Kilépett objektumok használatával személyes adatok tárolására sync Engine, és később dolgozza fel. Egy átmeneti tárolásra szolgáló objektum megőrzési összekötő térköz kilépett objektumként sok előnyökkel jár. A rendszer már rendelkezik elérhetővé tett az objektum szükséges adatait, mert nincs szükség az objektum ismét a következő importálás során a megadott létrehozása a csatlakoztatott adatforrásból. Ezzel a módszerrel sync engine mindig rendelkezik a csatlakoztatott adatforrás teljes pillanatképét még akkor is, ha nem a csatlakoztatott adatforrás jelenleg nincs kapcsolat. Kilépett objektumok alakítható illesztett objektumok és fordítva, attól függően, hogy a megadott szabályokat.

Az objektum importálása kilépett objektumként jön létre. Objektum exportálása egy illesztett objektum kell lennie. A rendszer logika kényszeríti a szabályt, valamint minden export-objektum, amely nem egy illesztett objektum törlése

## <a name="sync-engine-identity-management-process"></a>Szinkronizálás motor identitás kezelése folyamat
Az identitás-kezelési folyamat szabályozza az identitás adatai között különböző csatlakoztatott adatforrás frissítésének módját. Az Identitáskezelés fordul elő, az három folyamatok:

- Importálás
- A szinkronizálás
- Exportálás

Az importálási folyamat során sync engine kiértékeli a bejövő levelek identitás adatait egy csatlakoztatott adatforrásból. Észlel, amikor azt hoz létre új átmeneti tárolásra szolgáló objektumok vagy az összekötő helyre szinkronizálás meglévő átmeneti tárolásra szolgáló objektumok frissíti.

A szinkronizálás során sync engine frissíti a metaverse megfelelően az összekötő helyen előfordult módosításokat, és frissíti a összekötő szóköz a módosításokat, amelyeket a metaverse történtek megfelelően.

Az exportálási folyamat során a szinkronizálási motor végre változtatásokat, amely a közvetlen hozzáférésű fejlesztői objektumok és, hogy a program megjelöli a Exportálás függőben verembe küldi.

Az alábbi ábrán látható, hogy hol egyes a folyamatok fordul elő, az Identitáskezelés információáramlás csatlakoztatott adatforrásból származó között.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importálási folyamat
Az importálási folyamat során sync engine kiértékeli a frissítések azonosító információkat. Szinkronizálási motor hasonlítja össze egy átmeneti tárolásra szolgáló objektum identitás információt a csatlakoztatott adatforrás kapott személyes adatokat, és határozza meg, hogy szükség van-e az átmeneti tárolásra szolgáló objektum az frissítések. Ha az átmeneti tárolásra szolgáló objektum frissíteni az új adatokat, az átmeneti tárolásra szolgáló objektum importálása függőben van jelölve.

Átmeneti tárolására objektumok, az összekötő helyre szinkronizálás előtt, által sync engine csak a megváltozott a személyes adatokat is feldolgozása. Ez a folyamat az alábbi előnyöket nyújtja:

- **Hatékony szinkronizálás**. A kis méretre van állítva a szinkronizálás során feldolgozott adatok mennyiségét.
- **Hatékony újraszinkronizálás**. Módosíthatja, hogy hogyan sync engine dolgozza fel azonosító információkat az adatforráshoz a szinkronizálási motor újracsatlakozás nélkül.
- **A szinkronizálás megtekintése lehetőséget**. Ellenőrizze, hogy helyesek-e az identitás-kezelési folyamat a feltételezéseket szinkronizálás tekintheti meg.

Minden objektumhoz, az összekötő megadott a szinkronizálási motor először próbál keresse meg az objektum ábrázolása az összekötő helyre az összekötőt. Szinkronizálási motor összekötő térköz átmeneti tárolásra szolgáló objektumainak vizsgál, és megpróbálja megkeresni a megfelelő átmeneti tárolásra szolgáló objektum, amely egyező horgony attribútummal rendelkezik. Ha nem létező átmeneti tárolásra szolgáló objektum egyező horgony attribútummal rendelkezik, sync engine próbálja meg egy megfelelő átmeneti tárolásra szolgáló objektum megkülönböztető azonos nevű.

Ha szinkronizálási motor megtalálja a megfelelő megkülönböztető név szerint, de nem a horgony átmeneti tárolásra szolgáló objektum, az alábbi speciális hiba lép fel:

- Az összekötő szóköz található az objektumnak nincs horgony, ha sync engine az objektum eltávolítja a összekötő szóköz, és az meg van csatolva, **próbálja meg a következő szinkronizálás futtatása a kiépítési**metaverse objektum jelöli. Majd az új importálása objektum hoz létre.
- Ha az összekötő szóköz található objektum horgony, majd sync engine feltételezi, hogy az objektum vagy neve vagy a csatlakoztatott címtárban törölt. Az összekötő terület objektum egy új, ideiglenes megkülönböztető név azt rendeli, így akkor is szakaszban a bejövő objektum. A régi objektum lesz **ideiglenes (tranziens)**, az összekötő, és importálja a átnevezése vagy törlés elhárításához várakozás.

Ha szinkronizálási motor egy átmeneti tárolásra szolgáló objektum, amely megfelel a megadott az összekötő objektum keresi meg, milyen típusú változtatások céldokumentumát határozza meg. Például sync engine előfordulhat, hogy átnevezése vagy törlése a csatlakoztatott adatforrás az objektumot, vagy az objektum attribútum értékek frissítése csak akkor lehet, hogy.

A frissített adatoknak átmeneti objektumok importálása függőben vannak megjelölve. Különböző típusú függőben import érhetők el. Attól függően, hogy az importálási folyamat eredményét egy átmeneti tárolásra szolgáló az összekötő helyre az objektumnak függőben importálási típusok az alábbiak egyikét:

- A **Nincs lehetőséget**. Az átmeneti tárolásra szolgáló objektum attribútumai közül nincs változtatás érhetők el. Szinkronizálási motor nem megjelölheti azokat az ilyen típusú importálása függőben.
- **Adja hozzá**. Az átmeneti tárolásra szolgáló objektum az összekötő helyet az új importálása objektumot. Szinkronizálási motor megjelöli a típus importálása a metaverse további feldolgozásra függőben.
- A **frissítés**. Szinkronizálási motor megtalálja a megfelelő átmeneti tárolásra szolgáló objektum összekötő térköz és megjelöli a függőben lévő import típus szerint, így a frissítéseket a tulajdonságait a metaverse dolgozható. A frissítésekkel együtt objektum átnevezése.
- **Törlése**. Szinkronizálási motor megtalálja a megfelelő átmeneti tárolásra szolgáló objektum az összekötő helyen, és megjelöli a függőben lévő import e típusú, így a törölheti, a csatolt objektumot.
- A **Törlés/hozzáadása**. Szinkronizálási motor megtalálja a megfelelő átmeneti tárolásra szolgáló objektum összekötő térköz, de nem egyezik meg az objektum típusa. Ebben az esetben a törlés hozzáadása-módosítás elő van készítve. A törlés hozzáadása-szinkronizálási motor módosítás azt jelzi, hogy az objektum egy teljes újraszinkronizálás kell oka, hogy különböző csoportjaihoz szabályok vonatkoznak az objektum az objektum írja be a változások.

A függőben lévő import állapota átmeneti tárolásra szolgáló objektum megadásával jelentősen csökkentheti a feldolgozása a szinkronizálás során, mert ezzel így lehetővé teszi, hogy a rendszer csak az adatok frissített objektumok feldolgozása adatok mennyiségét.

### <a name="synchronization-process"></a>Szinkronizálási folyamat
Szinkronizálás két kapcsolódó folyamatok áll:

- Bejövő a szinkronizálás a metaverse tartalmának frissítése az adatokat az összekötő terület használatával.
- Kimenő szinkronizálás frissítésekor az összekötő területet a metaverse az adatok felhasználásával.

Az összekötő helyen előkészített információk segítségével a bejövő szinkronizálás hoz létre a metaverse az adatokat, amely a csatlakoztatott adatforrás integrált nézetét. Az összes átmeneti tárolásra szolgáló objektum vagy csak azok a függőben lévő import információkkal vannak összesíti, a szabályok beállításaitól függően.

A kimenő szinkronizálási folyamat frissítések exportálni az objektumok, ha metaverse objektumok módosítása.

Bejövő szinkronizálás az integrált megtekintése a személyes adatai csatlakoztatott adatforrásból mentsen a metaverse hoz létre. Sync engine lehet feldolgozni identitás adatait bármikor a legfrissebb azonosító információkat, hogy a csatlakoztatott adatforrásból használatával.

**Bejövő szinkronizálás**

Bejövő szinkronizálás az alábbi folyamatok tartalmazza:

- **Kiépítése** (ez is a **kivetítés** neve, ha ezt a folyamatot megkülönböztetése kimenő szinkronizálási kiépítési fontos). A szinkronizálási motor hoz létre egy új metaverse objektumot egy átmeneti tárolásra szolgáló objektum alapján, és csatolja őket. Rendelkezés objektumszintű művelet.
- **Csatlakozás**. A Sync engine csatol létező metaverse objektum egy átmeneti tárolásra szolgáló objektum. Illesztés objektumszintű művelet.
- **Importálás attribútum továbbításához**. Szinkronizálási motor frissíti az attribútum értékeit, attribútum folyamat a metaverse az objektum neve. Importálás attribútum folyamat egy átmeneti tárolásra szolgáló objektum és egy metaverse objektum közötti kapcsolat igénylő attribútum szintű műveletre.

Rendelkezés a rendszer csak, amely a metaverse objektumok hoz létre. Rendelkezést csak kilépett objektumok importálása objektumok hatással van. Során rendelkezni a szinkronizálási motor felel meg az objektum importálása objektum típusát, és mindkét objektum, így létrehozása egy illesztett objektum közötti kapcsolatot létesít egy metaverse objektum hoz létre.

Az illesztés folyamat is létrehozza az objektum importálása és egy metaverse objektum közötti kapcsolat. Csatlakozás és nyújtására közötti különbség, hogy az illesztés folyamat kell lennie, hogy az importálás objektum meglévő metaverse objektumra, ahol a rendelkezést folyamat objektumot hoz létre új metaverse vannak csatolva.

Szinkronizálási motor próbálja bekapcsolódni az objektum importálása metaverse objektumra a szinkronizálás szabály konfigurációban megadott feltételeknek.

A rendelkezés és csatlakozhat hozzá folyamat során az sync engine egy kilépett objektum csatol egy metaverse objektumra, és csatlakozott. E objektumszintű műveletek befejeztével szinkronizálási motor frissítheti a társított metaverse objektum attribútum értékeket. Ez a folyamat importálása attribútum munkafolyamat neve.

Importálás attribútum folyamat összes objektum importálása új adatokat, és egy metaverse objektum kapcsolódnak fordul elő.

**Kimenő szinkronizálás**

Kimenő szinkronizálandó frissítések objektumok exportálni, ha egy metaverse objektum módosítása, de a program nem törli. Kimenő szinkronizálási célja, hogy metaverse objektumok módosításai szükség van az összekötő szóközöket átmeneti tárolásra szolgáló objektumainak frissítés ki szeretné számítani. Bizonyos esetekben a módosítások kérheti, hogy minden összekötő szóközt objektumainak frissülnek. Az Exportálás hatékonyabbá objektumok exportálása függőben a program megjelöli a fejlesztői olyan objektumok, amelyek megváltoznak. Ezek exportálás objektumok később elküldte azokat a csatlakoztatott adatforrás az exportálási folyamat során.

Kimenő szinkronizálási három folyamatok foglalja magában:

- **Kiépítése**
- **Megszüntetés**
- **Exportálás attribútum továbbításához**

Kiépítési és megszüntetés mindkét objektumszintű műveletek, amelyek. Megszüntetés attól függ, hogy kiépítési, mert csak kiépítési kezdeményezhet azt. Megszüntetés jelentkezik kiépítési eltávolítja a metaverse objektum és exportálás objektum közötti kapcsolatot.

Kiépítési mindig elindítja, amikor változások történnek a metaverse objektumokhoz. Ha metaverse objektumok, szinkronizálási motor hajthatják végre az alábbi műveleteket a kiépítési folyamat részeként:

- Hozzon létre illesztett objektumok, ahol egy metaverse objektum egy újonnan létrehozott exportálás objektum kapcsolódik.
- Nevezze át a csatolt objektumot.
- Egy metaverse objektum és az objektumok létrehozása egy kilépett objektum átmeneti közötti kapcsolatok disjoin.

Ha szinkronizálási motor hozzon létre egy új összekötő objektumot kiépítési van szüksége, az átmeneti tárolásra szolgáló objektumra, amelyhez a metaverse objektum csatolva van értéke mindig exportálás objektumként, mivel az objektum még nem létezik a csatlakoztatott adatforrás.

Ha egy illesztett objektumra, és egy kilépett-objektum létrehozása disjoin szinkronizálási motor kiépítési igényel induljanak megszüntetés. A deprovisioning folyamat törli az objektumot.

Során megszüntetés, exportálás objektum törlése nem fizikailag törli az objektumot. Az objektum **Törölt**, ami azt jelenti, hogy, hogy a törlési művelet előkészített-e meg az objektum van megjelölve.

Exportálás attribútum folyamat akkor is előfordul a kimenő szinkronizálás során, hasonlóan, hogy importálása attribútum folyamat akkor fordul elő, a bejövő szinkronizálás során. Exportálás attribútum folyamat metaverse és exportálás olyan objektumok, amelyek adatbázis között csak akkor fordul elő.

### <a name="export-process"></a>Exportálási folyamat során
Az exportálási folyamat során sync engine vizsgálja meg, hogy a program megjelöli a függőben Exportálás az összekötő helyen, és elküldi a frissítéseket a csatlakoztatott adatforrás összes export-objektumot.

A sync engine megállapítható, hogy az Exportálás egyik, de nem tudja elég megállapítani, hogy helyesek-e az identitás-kezelési folyamat. A csatlakoztatott adatforrás-objektumok mindig más folyamatok módosítható. Mivel sync engine nem állandó kapcsolatot a csatlakoztatott adatforrás, még nem elegendő az objektum jellemzőinek feltételezéseket végezze el a csatlakoztatott adatforrás az Exportálás értesítést alapján.

Például az csatlakoztatott adatforrás folyamat változhat az objektum jellemzőinek vissza az eredeti értékükre (Ez azt jelenti, hogy a csatlakoztatott adatforrás sikerült felülírása az értékeket az adatok szinkronizálása motor által tolni és a csatlakoztatott adatforrás sikeresen alkalmazása után azonnal).

A szinkronizálási motor tárolók exportálni, és mindegyik átmeneti tárolásra szolgáló objektum állapot adatainak importálása. Az attribútumok az attribútum listára megadott értékeket az utolsó exportálás megváltoztak, ha tárolására importálása és exportálása állapot lehetővé teszi, hogy szinkronizálási motor megfelelően reagálni. Szinkronizálási motor erősítse meg, hogy a csatlakoztatott adatforrás exportált attribútum értékeket az importálási folyamat használja. Az importált és exportált adatait, összehasonlítása az alábbi ábrán látható módon lehetővé teszi a szinkronizálási motor sikeres volt-e az exportálás, vagy ha szüksége van meg kell ismételni megállapításához.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Például ha szinkronizálási motor exportálja attribútum C, amelynek értéke az 5, egy csatlakoztatott adatforrás, tárolja az C = 5 a memóriában exportálás állapotát. Minden egyes további exportálása a objektum eredmények exportálása C = 5 a csatlakoztatott adatforrás újra mivel sync engine feltételezi, hogy ez az érték nem folyamatosan alkalmazása az objektum (Ez azt jelenti, kivéve, ha egy másik érték nemrégiben importálta az csatlakoztatott adatforrásból) kísérlete. Az Exportálás memória nincs bejelölve, C = 5 az objektum az importálási művelet során érkezésekor.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
