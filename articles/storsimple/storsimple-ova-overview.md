<properties
   pageTitle="Virtuális tömb StorSimple áttekintése |} Microsoft Azure"
   description="A StorSimple egy integrált tárolás megoldás tároló tevékenységek között egy helyszíni virtuális eszköz és a Microsoft Azure felhőbeli tárhelyről kezelő virtuális tömb ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>A StorSimple virtuális tömb – bevezetés

## <a name="overview"></a>– Áttekintés

Üdvözli a Microsoft Azure StorSimple virtuális tömb, egy tároló tevékenységek között egy helyszíni virtuális eszközön, operációs rendszert futtató hipervizor és a Microsoft Azure felhőbeli tárhelyről kezelő integrált tárolás megoldás. A virtuális tömb (más néven a StorSimple helyszíni virtuális eszköz), egy hatékony költséghatékony és könnyen kezelhető fájlkiszolgálóra vagy iSCSI kiszolgáló megoldást, amely megszünteti a problémák és a vállalati tárolási és adatvédelem társított költségek számos. A virtuális tömb argumentum különösen jól illeszkedik távoli office/ág office (ROBO) felhasználási területei.

Az Áttekintés a virtuális tömb koncentrál. 

- Lépjen a StorSimple 8000 sorozat áttekintése [StorSimple 8000 sorozat: hibrid felhő megoldást](storsimple-overview.md). 

- Az StorSimple 5000/7000 sorozat eszköz tudni lépjen a [StorSimple Online súgójában](http://onlinehelp.storsimple.com/).

A virtuális tömb támogatja a iSCSI vagy a kiszolgáló üzenet letiltása (kis-és Középvállalatok) protokoll. Azt a meglévő hipervizor infrastruktúra fut, és a felhőben, felhőalapú biztonsági másolatot, gyors helyreállítás, elemszintű helyreállítási és katasztrófa helyreállítási szolgáltatásai tiering biztosít.

Az alábbi táblázat összefoglalja a virtuális tömb fontos funkcióját.

| A szolgáltatás | Virtuális tömb |
| ------- | ------------- |
|Telepítési követelmények | Használja a virtualizációs infrastruktúra (a Hyper-V vagy VMware)|
| Elérhetőség | Egyetlen csomópont |
| Teljes kapacitás (beleértve a felhőben) |Legfeljebb 64 TB használható kapacitás egy virtuális eszköz |
| Helyi kapacitásának | A 6.4-es TB használható kapacitással egy virtuális eszköz (szükség 8 TB 500 GB szabad lemezterület kiépítése) 390 GB|
| Natív protokollok | iSCSI vagy a kis-és Középvállalatok |
| Helyreállítási idő cél (RTO) | iSCSI: függetlenül a méret kisebb, mint 2 percig tart |
| Helyreállítási pont cél (Készletben) | Napi és igény szerinti biztonsági mentés |
| Tárterület tiering | Használja a hőtérkép megfeleltetés megállapíthatja, hogy milyen adatokat kell többszintű, vagy kicsinyítés |
| Támogatás | A szolgáltató által támogatott virtualizációs infrastruktúra |
| Teljesítmény | Attól függően, hogy a mögöttes infrastruktúra változó |
| Adatok mobilitás | Ugyanarra az eszközre vissza tudja állítani vagy elemszintű helyreállítási (fájl server) |
| Tárterület-meghatározási | Helyi hipervizor tárterület és a felhőben |
| Megosztás mérete |Többszintű: legfeljebb 20 TB; a kiemelt helyileg: maximum 2 TB |
| Mennyiségi mérete |Többszintű: legfeljebb 5 TB; a kiemelt helyileg: 500 GB |
| Egy adott időpontban érvényes | Egységes összeomlik |
| Elemszintű helyreállítás | Igen; felhasználók vissza tudja állítani, a megosztás |

## <a name="why-use-storsimple"></a>Miért érdemes használni a StorSimple?

StorSimple csatlakozik a felhasználók és kiszolgálók Azure tároló perc alatt, az alkalmazás módosítás nem.

Az alábbi táblázat néhány fontosabb előnyökkel jár, amely a virtuális tömb megoldást biztosít.

| A szolgáltatás | Előny |
|---------|---------|
| Áttetsző integrációja | A virtuális tömb támogatja a iSCSI vagy a kis-és Középvállalatok Protocol (protokoll). Az adatok mozgás a helyi réteg és a felhő réteg között, folytonos és a felhasználó számára.|
| Csökkentett tárolási költségek | StorSimple az aktuális igények a leggyakrabban használt meleg adatok teljesítéséhez elegendő helyi tároló kiépítése. Tárhely van szüksége, a nagyobb, mint a StorSimple rétegek hideg adatok az költséghatékony felhő tároló. Az adatok deduplicated és a felhőbe, hogy tovább csökkentse a tárhelyre és a költség elküldése előtt tömörítve.|
| Egyszerűsített tárhely kezelése | StorSimple központi kezelése a felhőben StorSimple segítségével több eszközök kezeléséhez nyújt.| 
| Korszerűbb helyreállítási és megfelelőség | StorSimple megkönnyíti a gyorsabb vészhelyreállítás azonnal visszaállítása a metaadat-alapú és az adatok helyreállítása, szükség szerint. Ez azt jelenti, hogy a minimális zavarok továbbra is a szokásos műveletek.|
| Adatok mobilitás | A felhőbe többszintű adatok helyreállítás és az áttelepítés céljából más helyekről érhetők el. Figyelje meg, hogy visszaállíthatja csak az eredeti virtuális tömb az adatokat. Katasztrófa helyreállítási szolgáltatásai azonban ha vissza szeretne állítani a teljes virtuális tömböt egy másik virtuális tömböt használni.|

## <a name="storsimple-workload-summary"></a>Összefoglaló StorSimple terhelést

A támogatott StorSimple feladatok összefoglalását alatt található táblázatos.

| Eset                | Terhelést              | Támogatott |  Korlátozások                                  | Verzió              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO együttműködési      | Fájl megosztása          | igen       | Lásd: [a fájl kiszolgálói maximális számával](storsimple-ova-limits.md). <br>Lásd: [támogatott kis-és Középvállalatok verzió rendszerkövetelményei](storsimple-ova-system-requirements.md).   | Az összes verzió      |


## <a name="workflows"></a>Munkafolyamatok

A StorSimple virtuális tömb különösen alkalmas az alábbi munkafolyamatok:

- [Felhőalapú tárolási kezelése](#cloud-based-storage-management)
- [Hely független biztonsági mentése](#location-independent-backup)
- [Adatok védelme és katasztrófa helyreállítása](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Felhőalapú tárolási kezelése

Az operációs rendszert futtató az Azure klasszikus portálon StorSimple-kezelő szolgáltatás több eszközön, és több helyen tárolt adatok kezelésére is használhatja. Az elosztott ág helyzetekben különösen hasznos. Megjegyzés: a különálló példányai a StorSimple kezelő szolgáltatás virtuális tömbök és a tényleges StorSimple eszközök kezeléséhez létre kell hoznia. 

![felhőalapú tárolási kezelése](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Hely független biztonsági mentése

A virtuális tömb felhő pillanatképek független helyét, a pont és az idő vagy egy példányát a mennyiségi megosztás tartalmaz. Felhőalapú pillanatképek alapértelmezés szerint engedélyezve vannak, és nem tiltható le. Az összes kötet, megosztások biztonsági másolatot egyszerre egy egyetlen napi biztonsági házirenden keresztül, és szükség esetén további alkalmi másolatok készíthet.

### <a name="data-protection-and-disaster-recovery"></a>Adatok védelme és katasztrófa helyreállítása

A virtuális tömb támogatja az alábbi adatok védelme és katasztrófa helyreállítási jelenik meg:

- A **mennyiségi vagy a megosztás visszaállítása** – használatát a visszaállítás új munkafolyamat állíthatja helyre a mennyiségi vagy a megosztás. A teljes mennyiségi vagy a megosztás visszaállításához használandó ezt a módszert.
- **Elemszintű helyreállítási** – megosztások biztonsági másolata egyszerűsített elérésének engedélyezése. Egyszerűen visszaállíthatja egy külön fájlba mappából egy speciális .backup érhető el a felhőben. Ez a képesség visszaállítása felhasználói-alapú és felügyeleti beavatkozás nélkül szükség.
- **Vészhelyreállítás** – kötet, vagy egy új virtuális tömböt megosztások helyreállításának feladatátvevő képessége használatát. Az új virtuális tömb létrehozása és regisztrálhatja a StorSimple Manager szolgáltatással, akkor az eredeti virtuális tömb átveszi. Az új virtuális tömb majd feltételezi a kiépített erőforrásokat. 

## <a name="virtual-array-components"></a>Virtuális tömbelemek

A virtuális tömb tartalmazza az alábbiakat:

- [Virtuális tömb](#virtual-array) – a hibrid felhő tárolóeszközre a virtualizált környezetben vagy hipervizor kiépítve virtuális gép alapján.  
- [StorSimple kezelő szolgáltatás](#storsimple-manager-service) – bővítménye, amellyel az Azure klasszikus portálon kezelheti egy vagy több StorSimple érhető el az eltérő földrajzi helyek egyetlen webes kapcsolatról. A StorSimple kezelő szolgáltatás segítségével létrehozása és szolgáltatások kezelése, megtekintése és eszközök és értesítések kezelése és kezelheti a kötet, megosztás és a meglévő pillanatképek.
- [Helyi webes felhasználói felülete](#local-web-user-interface) – egy webes beállítani az eszközt, hogy azt a helyi hálózathoz csatlakozik, és kattintson az eszköz regisztrálása az StorSimple kezelő szolgáltatás használt. 
- [Parancssor](#command-line-interface) – a Windows PowerShell felhasználói felülete, amely a virtuális tömb a támogatási indítása is használhatja.
Az alábbi szakaszok ismertetik, minden összetevő részletesebben, és ismertetik hogyan a megoldást adatok rendezése, foglal helyet, illetve elősegíti tároló kezelése lapon, és az adatok védelme.

### <a name="virtual-array"></a>Virtuális tömb

A virtuális tömbben elsődleges tárolására szolgál, kezeli a felhőbeli tárhelyről való kommunikáció és a biztonság és az eszközön tárolt összes adatot titkosságának biztosítható egyetlen csomópontot tároló megoldás.

A virtuális tömböt egy a modellben, amely letölthető érhető el. A tároló tömb legnagyobb kapacitása nem 6.4-es TB az eszköz (ha benne az alapul szolgáló tároló követelmény 8 TB) és a 64 TB többek között a felhőbeli tárhelyek. 

A virtuális tömb az alábbi szolgáltatásokat foglalja magában:

- Tényleges költség. Azt megkönnyíti a meglévő virtualizációs infrastruktúra használja, és a meglévő a Hyper-V vagy VMware hipervizor telepíthetők.
- A adatközponthoz található, és a konfigurálhatók egy iSCSI server vagy fájlt. 
- Az a felhő integrálva van.
- Biztonsági mentés a felhőbe, amely is megkönnyítése vészhelyreállítás és egyszerűsítése elemszintű helyreállítási (ILR) tárolja. 
- A virtuális tömb frissítések alkalmazhat, ahogyan szeretné a fizikai eszközök alkalmazása.

>[AZURE.NOTE] A virtuális tömb nem bonthatók ki. Ezért fontos a megfelelő tároló létesítése a virtuális eszköz létrehozásakor. 

### <a name="storsimple-manager-service"></a>StorSimple kezelő szolgáltatás

Microsoft Azure StorSimple webes felhasználói felületet (StorSimple kezelő szolgáltatás), amely lehetővé teszi, hogy központilag adatközponthoz kezelése és a felhő tároló biztosít. A StorSimple kezelő szolgáltatás használatával végezze el az alábbi műveleteket:

- Több StorSimple virtuális tömbök kezelése egyetlen szolgáltatáson keresztül. 
- Konfigurálása és StorSimple eszközök biztonsági beállításainak kezelése. (A felhőben titkosítás a Microsoft Azure API-khoz függ.)
- Tárterület-fiókhoz tartozó hitelesítő adatok és a tulajdonságok beállítása.
- Konfigurálása és kötet vagy megosztások kezelése.
- Készítsen biztonsági másolatot, és állítsa vissza az adatokat a kötet vagy megosztások.
- Teljesítmény figyelését.
- Tekintse át a rendszerbeállítások, és lehetséges problémák azonosítására.

A StorSimple kezelő szolgáltatás használatával végezze el a virtuális tömb napi felügyeleti.

További tudnivalókért olvassa el az [használata a StorSimple kezelő szolgáltatás felügyelete StorSimple eszközére](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Helyi webes felhasználói felülete

A virtuális tömb webes felhasználói Felületet egyszeri konfigurálása és a regisztrációs az eszköz a StorSimple Manager szolgáltatással használt tartalmazza. Állítsa le a indítsa újra a virtuális tömb, diagnosztikai vizsgálatok futtatása, frissítése, szoftver, a eszközt rendszergazdai jelszó módosítása, rendszer naplók megtekintése és lépjen kapcsolatba a Microsoft Support egy szolgáltatáskérelem benyújtásának felhasználhatja azt. 

Webes a felhasználói felület használatával kapcsolatos további tudnivalókért olvassa el az [használata a webes felhasználói felületének felügyelheti a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Parancssor

A tartalmazza a Windows PowerShell-felület lehetővé teszi, hogy segít virtuális eszközén előforduló problémák elhárítására kezdeményezése a Microsoft Support támogatási munkamenetet.

## <a name="storage-management-technologies"></a>Tárhely kezelése technológiák

A virtuális tömb és más összetevő, mellett a StorSimple megoldást a következő szoftver technológiákat használja a fontos adatok gyors hozzáférést biztosít, tárterület-felhasználás csökkentése és a virtuális tömb-on tárolt adatok védelme biztonsági:

- [Az automatikus tároló tiering](#automatic-storage-tiering) 
- [Helyi meghajtóra rögzített és az kötet](#locally-pinned-shares-and-volumes)
- [Deduplication adat tömörítése többszintű és biztonsági másolat készül a felhőbe](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Ütemezett és igény szerinti biztonsági másolatok](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Az automatikus tároló tiering

A virtuális tömböt egy új tiering mechanizmusa segítségével a virtuális tömb és a felhőben tárolt adatok kezelése. Vannak olyan csak két rétegek: a helyi virtuális tömb és Azure felhőbeli tárhelyek. A StorSimple virtuális tömb adatok automatikus elrendezése be a réteg hőtérkép, amelyek aktuális használatát, a kor és a Kapcsolatok nyomon követi a egyéb adatok alapján. -Adatok legaktívabb (hottest) helyi van tárolva, miközben a felhőbe automatikusan átkerül a kisebb aktív és inaktív adatok. (Az összes biztonsági másolatok tárolni a felhőben.) StorSimple igazítása újrarendezi adatok és a tárterület-hozzárendelése nem szokásai módosítása. Például információk válhat kevésbé aktív idővel. Fokozatosan kevésbé aktív lesz, mint azt többszintű meg a felhőben. Azonos adatokat újra aktívvá válik, ha azt többszintű a a tárhely tömböt.

Adatok egy adott többszintű megosztás vagy a mennyiségi biztosítva van a saját helyi réteg helyet. (körülbelül 10 %-a teljes kiépített szóköz az adott megosztás vagy a mennyiség). Amíg ez csökkenti a rendelkezésre álló tárhely virtuális az eszközre az adott megosztás vagy a hangerőt, biztosíthatja, hogy egy megosztás vagy mennyiségi tiering nincs hatással a többi megosztás vagy a kötet tiering igényeitől. Egy Igen zsúfolt terhelést egy megosztás vagy mennyisége ily módon minden más munkaterhelésekből a felhőbe nem lehetséges. 

![az automatikus tároló tiering](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Megadhatja, hogy a helyi meghajtóra, kiemelt kötet, ebben az esetben az adatok a virtuális tömb marad, és soha nem többszintű a felhőbe. További információért lépjen [helyileg kiemelt és az kötet](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Helyi meghajtóra rögzített és az kötet

Megfelelő megosztás és a helyi meghajtóra, kiemelt kötet hozhat létre. Ezt a lehetőséget, hogy a kritikus alkalmazások által igényelt adatok a virtuális tömb marad, és a felhőbe soha nem többszintű biztosítja. Helyi meghajtóra rögzített és az kötet van a következő funkciók: 

- Ezek nem tárgyai felhő késések vagy csatlakozási problémákat tapasztal.
- Azok a StorSimple felhő biztonsági mentése és katasztrófa helyreállítási szolgáltatásai továbbra is élvezhetik.

Vissza tudja állítani egy helyi meghajtóra a kiemelt megosztás vagy mennyiségi, többszintű vagy többszintű megosztás vagy mennyiségi helyben, kiemelt. 

További információt a helyi meghajtóra rögzített kötet olvassa el az [használata a StorSimple kezelő szolgáltatás kötet kezeléséhez](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Deduplication adat tömörítése többszintű és biztonsági másolat készül a felhőbe

StorSimple deduplication és az adatok tömörítést használja tovább csökkentse a felhőben tárhelyre. Deduplication csökkenti a teljes tárolt adatok megadása redundancia kiküszöbölésével tárolt adatok. Amikor adatokat, StorSimple figyelmen kívül hagyja az adatok nem változik, és csak a változásokat rögzíti. Ezeken kívül StorSimple csökkenti tárolt adatok mennyiségét azonosító, és eltávolítása ismétlődő adatokat. 

>[AZURE.NOTE] A virtuális tömb-on tárolt adatok nem deduplicated vagy tömörítve. Az összes deduplication és tömörítés fordul elő, mielőtt a az adatokat küldi át a felhőalapú levelezésre.

### <a name="scheduled-and-on-demand-backups"></a>Ütemezett és igény szerinti biztonsági másolatok

StorSimple adatok védelmi funkciókkal lehetővé teszi azok igény szerinti biztonsági másolatok létrehozására. Ezenkívül egy alapértelmezett biztonsági ütemezés biztosítja, hogy adatok biztonsági másolatot készíteni a napi. Biztonsági másolatok kell venni a felhőben tárolt növekményes pillanatképek formájában. Egy adott időpontban érvényes, amely csak a módosítás az utolsó biztonsági mentés óta rögzítéséhez lehet létrehozni és gyorsan visszaállítani. Ezek a pillanatképek katasztrófa helyreállítási helyzetekben különösen fontos lehet, mert a cseréje másodlagos tároló rendszerek (például a szalag biztonsági másolat), és lehetővé teszi az adatok visszaállítása a adatközponthoz vagy más webhelyek szükség esetén.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan [készítse elő a virtuális tömb portál](storsimple-ova-deploy1-portal-prep.md).


