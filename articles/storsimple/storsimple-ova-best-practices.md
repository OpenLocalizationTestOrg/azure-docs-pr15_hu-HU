<properties 
   pageTitle="Gyakorlati tanácsok StorSimple virtuális tömb |} Microsoft Azure"
   description="Az ajánlott eljárások üzembe helyezése és kezelése a StorSimple virtuális tömb ismerteti."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Ajánlott eljárások a StorSimple virtuális tömb

## <a name="overview"></a>– Áttekintés

Microsoft Azure StorSimple virtuális tömb egy integrált tárolás megoldás tároló tevékenységek között egy helyszíni virtuális eszközön, operációs rendszert futtató hipervizor és a Microsoft Azure felhőbeli tárhelyről kezelő. Virtuális tömb StorSimple hatékony, költséghatékony ahelyett, hogy a 8000 sorozat fizikai tömbben. A virtuális tömb futtatását is lehetővé teszi a meglévő hipervizor infrastruktúra támogatja a iSCSI és a kis-és Középvállalatok protokollok és távoli office/ág office felhasználási területei jól illeszkedik. További információt a StorSimple megoldásokra lépjen [A Microsoft Azure StorSimple áttekintése](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Ez a cikk bemutatja a gyakorlati tanácsok a kezdeti telepítés telepítési és a StorSimple virtuális tömb adatkezelési folyamán végrehajtott. Ezekkel az ajánlott eljárásokkal érvényesített iránymutatást adjon a beállítása és kezelése a virtuális tömb. Ez a cikk az informatikai rendszergazdák üzembe helyezéséhez és kezeléséhez az Adatközpont esetén a virtuális tömbök cél.

Azt javasoljuk, hogy a gyakorlati tanácsok biztosíthatja, hogy az eszköz esetén továbbra is a megfelelőségi módosítják a telepítő vagy művelet folyamat periodikus ismerkedést. A virtuális tömb, segítségért [forduljon a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) ezekkel az ajánlott eljárásokkal végrehajtása során kell tapasztal kapcsolatos problémák megoldásához.

## <a name="configuration-best-practices"></a>Ajánlott eljárások a konfiguráció 

Ezekkel az ajánlott eljárásokkal irányelveknek megfelelően van szüksége a kezdeti telepítés és üzembe virtuális tömb során követendő terjed ki. Ezekkel az ajánlott eljárásokkal kapcsolatos csoportházirend-beállításai, méretezése, a hálózat beállítása, tároló fiókok konfigurálása és megosztás és a virtuális tömb kötet hozhat létre virtuális gép kiépítési tartalmazza. 

### <a name="provisioning"></a>Kiépítése 

Virtuális tömb StorSimple kiépítve a hipervizor (a Hyper-V vagy VMware) a virtuális gép (virtuális) áll, a kiszolgáló. Kiépítésekor a virtuális gép, győződjön meg arról, hogy a szolgáltató tudja kellő erőforrások jelöl ki. További információért lépjen [minimális erőforrásigények](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) hozhatók létre a tömbképletet. 

A következő tanácsok végrehajtása a kiépítésekor a virtuális tömb:


|                        | A Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Virtuális gép típusa**   | **2 előállítása** Virtuális használatáról a Windows Server 2012 vagy újabb rendszerű és a *.vhdx* képként. <br></br> **1 előállítása** Virtuális használatáról a Windows Server 2008 vagy újabb rendszerű és a *.vhd* képként.                                                                                                              | Virtuális gép verzió 8-11-es használatával *.vmdk* kép felhasználásával.                                                                      |
| **Memória típusa**            | Állítsa be a **statikus**memória. <br></br> Ne használja a **dinamikus memóriafoglalási** lehetőséget.            |                                                    |
| **Lemezen adattípus**         | Építse **dinamikusan növekvő**szerint.<br></br> **Rögzített méretű** hosszú ideig tart. <br></br> Ne használja a **differenciálást** lehetőséget.                                                                                                                   | Adja meg a **Vékony rendelkezést** beállítást.                                                                                      |
| **Adatok lemez módosítása** | Kiterjesztett, illetve kicsinyítésével nem engedélyezett. Ehhez tegye kísérlet eszközön összes helyi adatok elvesztése eredményez.                       | Kiterjesztett, illetve kicsinyítésével nem engedélyezett. Ehhez tegye kísérlet eszközön összes helyi adatok elvesztése eredményez. |

### <a name="sizing"></a>Átméretezése

A StorSimple virtuális tömb méretezése, vegye figyelembe az alábbi tényezőket:

- Helyi foglalás kötet vagy megosztások. A szóköz körülbelül 12 %-át az egyes kiépített többszintű mennyiségi vagy a megosztás a helyi rétegben van fenntartva. A szóköz 10 %-os nagyjából is fájlrendszer helyileg rögzített kötet fenntartva.
- Pillanatkép beleszámítva. Nagyjából 15 %-kal hely a helyi réteg pillanatképek van fenntartva.
- Segítségre van szüksége a visszaállítása. Ha egy új művelet visszaállítása módon, átméretezése visszaállítása szükséges hely kell figyelembe. Visszaállítás a kész, a megosztás és a mennyiségi azonos méretű vagy nagyobb.
- Néhány pufferelési bármely váratlan növekedési kell rendelni.

Az előző tényezők alapján, a méretező követelményeknek is képviselteti magát az alábbi egyenlettel történik:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Az alábbi példák bemutatják, hogyan egy saját igényeknek megfelelően alakítható virtuális tömb méretét.

#### <a name="example-1"></a>Példa: 1:
A virtuális tömb szeretné engedélyezni szeretné 

- a 2 TB kiépítése többszintű mennyiségi vagy hálózati megosztáshoz.
- egy 1 TB kiépítése többszintű mennyiségi vagy hálózati megosztáshoz.
- helyi meghajtóra rögzített kötet 300 GB-os kiépítése, vagy megoszthatja.


Az előző kötet vagy megosztások tudassa velünk ki kell számítani a helyigény a helyi rétegben. 

Első lépésként az egyes többszintű mennyiségi és a megosztás helyi foglalás lenne a mennyiségi/megosztás mérete 12 %-kal egyenlő. A helyi meghajtóra rögzített mennyiségi/megosztás a helyi foglalás (mellett a kiépített mérete) helyileg rögzített mennyiségi/megosztás méretét 10 %-át. Ebben a példában szükséges

- Helyi foglalás 240 GB (egy 2 TB többszintű mennyiségi/megosztás)
- Helyi foglalás 120 GB (egy 1 TB többszintű mennyiségi/megosztás)
- 330 GB helyileg rögzített mennyiségi vagy (10 %-a helyi foglalás hozzáadása kiépítéstől 300 GB méretű) megosztása

A teljes hely a helyi réteg, amennyiben szükséges: 240 GB + 120 GB + 330 GB = 690 GB.

Legalább mennyi hely a helyi réteg második, mint a legnagyobb egyetlen Foglalás szükséges. További ennyi abban az esetben, ha szeretné visszaállítani a felhőben pillanatkép használja. Ebben a példában a legnagyobb helyi foglalás 330 GB (beleértve a fájlrendszer foglalás), úgy, hogy a 690 GB: 690 GB + 330 GB = 1020 GB vegyen fel.
Ha a későbbi további visszaállítása végrehajtotta azt, azt is mindig tárterületet szabadítson fel a az előző visszaállítási művelet a.

Harmadik szükséges az összes helyi hely eddig tárolására helyi pillanatfelvételek, 15 %-át, hogy csak 85 %-át érhető el. Ebben a példában, amely körüli lenne 1020 GB = 0.85&ast;kiépített adatok TB lemezre jelölőnégyzetből. Igen, a kiépített adatok lemez lenne (1020&ast;(1/0.85)) = 1 200 GB = 1,20 TB ~ 1,25 TB (kerekítés a legközelebbi KVARTILIS)

Amíg nem várt növekedés és új visszaállítása, egy helyi körül lemez kell rendelkezni 1,25-1,5 TB.

> [AZURE.NOTE] Azt is javasoljuk, hogy a merevlemezre vékonyan már kiépítve. Ennek ellenére oka az, hogy a visszaállítás szóköz csak van szükség, ha a visszaállítandó öt napnál régebbi-adatok. Elemszintű helyreállítási adatok visszaállítása az utolsó öt nappal anélkül, a további térközt a visszaállítás teszi lehetővé.

#### <a name="example-2"></a>Példa 2: 
A virtuális tömb szeretné engedélyezni szeretné 

- a 2 TB kiépítése mennyiségi többszintű
- helyi meghajtóra kiemelt 300 GB-os kötet létrehozása

A helyi lemezterület foglalás többszintű kötet/megosztások és 10 %-os helyileg rögzített kötet/megosztások 12 % alapján, szükség

- Helyi foglalás 240 GB (2 TB többszintű mennyiségi/megosztás)
- 330 GB helyileg rögzített mennyiségi vagy a megosztás (helyi foglalás 10 %-os hozzáadása a 300 GB-os kiépítéstől szóköz)

Teljes hely a helyi rétegben kötelező: 240 GB + 330 GB = 570 GB

A visszaállítás szükséges minimális helyi hely 330 GB. 

a teljes lemezterület 15 %-kal használatos pillanatképek tárolására, így 0.85 csak akkor érhető el. Igen, a lemez mérete (900&ast;(1/0.85)) = 1.06 TB ~ 1,25 TB (kerekítés a legközelebbi KVARTILIS) 

Amíg minden váratlan NÖV, 1,25-1,5 TB helyi lemezen kiépítése.


### <a name="group-policy"></a>A csoportházirend

A Csoportházirend-infrastruktúrát, amely lehetővé teszi, hogy a felhasználók és a számítógépen meghatározott konfigurációk megvalósítása. Csoportházirend-objektumok, amelyek kapcsolódnak a következő Active Directory tartományi szolgáltatások (AD DS) tárolók szereplő csoportházirend-beállításai: webhelyek, a tartományok vagy szervezeti egységeket. 

Ha a virtuális tömb a tartományhoz, csoportházirend-objektum alkalmazható rá. A csoportházirend-objektum telepítheti az alkalmazásokat, például a víruskereső szoftvert, hogy a művelet a StorSimple virtuális tömb befolyásolhatja.

Emiatt azt javasoljuk, hogy meg:

-   Győződjön meg arról, hogy a virtuális tömb a saját szervezeti egységre az Active Directory. 

-   Győződjön meg arról, hogy nem csoportházirend-objektumok (GPO) a virtuális tömb vonatkoznak. Hogy győződjön meg arról, hogy a virtuális tömb (gyermek csomópontjának) nem automatikusan örökli bármely csoportházirend-objektumok a szülő zárolhatja. További információért lépjen [Öröklődés blokkolása](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Hálózati

A hálózati konfigurálásról a virtuális tömb keresztül a helyi webes felhasználói felület történik. A virtuális hálózati kapcsolat engedélyezve van a hipervizor, amelyben a virtuális tömb már kiépítve keresztül. A [Hálózati beállítások](storsimple-ova-deploy3-fs-setup.md) lap segítségével a virtuális hálózati kapcsolat IP-címe, alhálózat és az átjáró beállítása.  Is beállíthatja az elsődleges és másodlagos DNS-kiszolgáló időbeállítások és választható proxy beállításai az eszközön. A hálózati konfigurálásról a legtöbb egy egyszeri beállításához. Tekintse át a [hálózati követelmények StorSimple](storsimple-ova-system-requirements.md#networking-requirements) üzembe helyezése a virtuális tömb előtt.

A virtuális tömb telepítésekor azt javasoljuk, kövesse az alábbi ajánlott eljárást:

-   Győződjön meg arról, hogy a hálózat, amelyben a virtuális tömb mindig telepítik kapacitása ahhoz, hogy az 5 MB Internet-sávszélesség (vagy több) jelöl ki. 

    -   Az Internet-sávszélesség segítségre van szüksége a a terhelést tulajdonságokat és az adatok módosítása függően változik.

    -   Az adatok módosítása, amely is kezelhető közvetlenül az Internet-sávszélesség arányos. Példaként véve a biztonsági a 5 MB sávszélesség rendezhetők 8 órát adatainak módosítása körül a 18 GB-ot. Négyszer több sávszélesség (20 MB), a kezelésére képesek négyszer további adatainak módosítása (72 GB). 

-   Gondoskodjon arról, hogy az internetes kapcsolat az mindig elérhető lesz. Az eszközökön kapcsolata időről-időre vagy mellékletei nem megbízható internetes kapcsolatok a felhőben adatokhoz való hozzáférés mérvű vonhat és eredményezhet beállításai nem támogatottak.

-   Ha szeretne egy iSCSI kiszolgálójával az eszköz telepítése: 
    -   Azt javasoljuk, hogy tiltsa le a **első IP-cím automatikusan** (DHCP) lehetőséget. 
    -   Statikus IP-címek konfigurálása. Meg kell adnia egy elsődleges és másodlagos DNS-kiszolgáló.

    -   Ha több hálózati kapcsolaton a virtuális definiáló tömb, csak az első hálózati felület (alapértelmezés szerint a kapcsolat az **Ethernet**) elérhetik a felhőben. Ha szabályozni szeretné a forgalmat típusát, több virtuális hálózati kapcsolat létrehozása a virtuális tömb (konfigurált iSCSI kiszolgáló), és e felületek csatlakoztatása különböző alhálózathoz.

-   Csak a felhőben sávszélesség szabályozása (használja a virtuális tömb), az útválasztó vagy a tűzfalat szabályozásának beállítása. Az a hipervizor szabályozásának határozza meg, ha azt fogja szabályozása összes a jegyzőkönyvek iSCSI és a kis-és Középvállalatok csak a felhőben sávszélesség helyett. 

-   Győződjön meg arról, hogy időt szinkronizálási hypervisors engedélyezve van. Ha használata a Hyper-V, jelölje ki a virtuális tömb a Hyper-V kezelője, válassza a **Beállítások &gt; Integration Services**, és győződjön meg arról, hogy be van-e jelölve az **idő-szinkronizálás** .

### <a name="storage-accounts"></a>Tárterület-fiókok

Virtuális StorSimple a tömb egyetlen tárolási fiókkal társítható. Ehhez a fiókhoz tároló lehet automatikusan generált tároló fiókkal, a szolgáltatást, mint az azonos előfizetés-fiók vagy egy tárterület-fiókkal kapcsolatos egy másik előfizetést. További tudnivalókért lásd: [a virtuális tömb tároló fiókok](storsimple-ova-manage-storage-accounts.md)kezelése.

A virtuális tömb társított tárterület-fiókok használata az alábbi javaslatokat.

-   Csatolás több virtuális tömb egyetlen tárolási fiókkal, ha kiszámíthatja a maximális kapacitás (64 TB) egy virtuális tömb és a tárterület-fiók (500 TB) maximális mérete. Ez a teljes méretű virtuális tömbök rendelhető, hogy tárterületet fiókkal körülbelül 7-es számú korlátozza.

-   Amikor a tároló új fiók létrehozása
    -   Azt javasoljuk, hogy Ön hozza létre a tartományban lévő legközelebb a távoli office/irodában amennyiben telepítve van a StorSimple virtuális tömb késések minimalizálásához.

    -   Ellátni szem előtt, hogy nem tud-e tároló fiók áthelyezése különböző területei között. A szolgáltatás is előfizetésekben nem aktiválhatók.

    -   Használjon tárhely, amely a redundancia között a adatközpontokkal. GEO-redundáns tároló (GRS), a zóna felesleges tároló (ZRS) és a helyi meghajtóra felesleges tároló (LRS) formázást támogatják a virtuális tömb való használatra. További információt a tárterület-fiókok különböző típusú lépjen [Azure tároló replikáció](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Megosztás és a kötet

A StorSimple virtuális tömb megosztások is kiépítése, fájlkiszolgálóra, valamint egy iSCSI kiszolgálójával konfigurálásakor kötet konfigurálásakor. A gyakorlati tanácsok létrehozásához, és az kötet méretét és a típus konfigurált kapcsolódnak.

#### <a name="volumeshare-size"></a>Mennyiségi/megosztás mérete

A virtuális tömb is kiépítése a megosztások, fájlkiszolgálóra, valamint egy iSCSI kiszolgálójával konfigurálásakor kötet konfigurálásakor. A gyakorlati tanácsok létrehozásához, és az kötet méretét és a típus konfigurált vonatkoznak. 

Tartsa szem előtt az alábbi gyakorlati tanácsokat kiépítésekor megosztások vagy kötet virtuális eszközén.

-   A fájlok méretének viszonyított többszintű megosztás kiépített méretének hatással lehet a tiering teljesítményét. Nagyméretű fájlok használata okozhat a lassú réteg meg. Ha nagyobb fájlokkal dolgozik, azt javasoljuk, hogy a legnagyobb fájl, a megosztás méretű 3 %-nál kisebb.

-   A virtuális tömbben lévő legfeljebb 16 kötet/megosztások hozhat létre. A kiemelt helyi meghajtóra, ha a kötet megosztások lehet, és 2 TB 50 GB között. Többszintű, ha a kötet megosztások 20 TB 500 GB között kell lennie. 

-   A szükséges adatok fogyasztása, valamint a jövőbeli NÖV kiszámíthatja a mennyiségi létrehozásakor. Miközben a mennyiségi később nem bonthatók ki, bármikor visszaállíthatja egy nagyobb mennyiségi.

-   A hangerő létrehozása után, nem zsugorítása a hangerő StorSimple a méretét.
   
-   Írás közben a többszintű mennyiségi StorSimple, a mennyiségi adatok elérésekor egy bizonyos küszöbértéket (viszonyítva a mennyiségi fenntartva helyi szóköz), a IO folyamatban van. Továbbra is a mennyiségi írási lelassul a IO jelentősen. Abban az esetben, ha túl (azt ne aktívan állítsa le a felhasználót a írási túl a kiépített kapacitás) kiépített kapacitásának többszintű kötet írhat, az értesítést arról, hogy rendelkezik a oversubscribed látni. A figyelmeztetés jelenik meg, miután elengedhetetlen intézkedéseket javító például törlése a kötet adatait, vagy a hangerő visszaállítása egy nagyobb mennyiségi (mennyiségi kiterjesztett jelenleg nem támogatott).

-   Katasztrófa helyreállítási használata esetben megengedett megosztások/kötet száma 16 pedig párhuzamosan feldolgozhatók megosztások/kötet maximális száma is 16, a megosztás/kötet száma nem éri el a Készletben és RTOs hatással. 

#### <a name="volumeshare-type"></a>Mennyiségi/megosztás típusa

StorSimple mennyiségi/megosztás kétféle alapján a használatát támogatja: helyileg rögzítve és többszintű. Helyileg rögzített kötet/megosztások thickly kiépített, mivel a többszintű kötet megosztások vékonyan kiépített környezetet. 

Azt javasoljuk, hogy a következő tanácsok végrehajtása StorSimple kötet/megosztások beállításakor:

-   Azonosítják az mennyiségi szeretné telepíteni, mielőtt kötet létrehozása a feladatok alapján. Helyi meghajtóra rögzített kötet használata munkaterhelésekből, amely van szükség az adatok helyi garanciákkal (még alatt egy felhőalapú üzemszünetek) és, alacsony felhő késések. A virtuális tömb kötet hoz létre, miután a mennyiségi típus nem módosítható a helyi meghajtóra kiemelt való többszintű vagy *fordítva*. Helyi meghajtóra rögzített kötet példaként létrehozása, ha telepíti az SQL-munkaterhelésekből vagy munkaterhelésekből szolgáltatója virtuális gépeken futó (VMs); fájl megosztása munkaterhelésekből többszintű kötet használatával.

-   A nagyméretű fájlok méretének kezelése során felmerülő, jelölje be a kisebb a gyakran használt archiválás adatok lehetőséget. Ez a beállítás engedélyezve van az adatátvitel a felhőbe felgyorsításához deduplication adattömb nagyobb méretű 512 k használatos.

#### <a name="volume-format"></a>Mennyiségi formázása

A iSCSI kiszolgálón StorSimple kötet hoz létre, miután kell inicializálni csatlakoztatása és formázhatja a őket. Ez a művelet az StorSimple eszköz csatlakozik, az állomáson történik. Alábbi gyakorlati tanácsokat amikor csatlakoztatása és formázása a StorSimple állomáson kötet.

-   Gyorsformázás összes StorSimple kötet.

-   StorSimple kötet formázása, ha egy hozzárendelés egység méret (Ausztráliai) 64 KB használata (alapértelmezett érték a 4 KB). A 64 KB Ausztráliai van tesztjei alapján megállapított házon belül a közös StorSimple munkaterhelésekből és más munkaterhelésekből.

-   Egy iSCSI konfigurálva a StorSimple virtuális tömb használatakor nem átnyúló vagy dinamikus lemezen ezek kötet vagy lemezen StorSimple által nem támogatott.

#### <a name="share-access"></a>Az access megosztása

Megosztás a virtuális tömb fájlkiszolgálón létrehozásakor kövesse az alábbiakat:

-   Megosztás létrehozásakor hozzárendelése egy felhasználói csoportot helyett egy felhasználó megosztási rendszergazdaként.

-   A megosztás a Windows Intéző megosztások szerkesztésével létrehozását követően a jogosultságainak kezelheti.

#### <a name="volume-access"></a>Mennyiségi access

A StorSimple virtuális tömb iSCSI kötet konfigurálásakor fontos, a hozzáférés vezérléséhez, ahol szükséges. Annak megállapításához, hogy melyik kiszolgálók elérhetik a kötet, létrehozása és az access-ellenőrzési rekordokat (ACRs) társítása StorSimple kötet.

Használja az alábbi gyakorlati tanácsokat, ACRs StorSimple kötet beállításakor:

-   Legalább egy ACR mindig társítása egy mennyiségi.

-   Több ACRs csak egy fürtkörnyezetben definiálása.

-   Ha egynél több ACR hozzárendelése a kötet, győződjön meg arról, hogy a kötet nem elérhetővé tett oly módon, ha egyidejűleg elérhető több-a csoportosított állomás. Ha több ACRs rendelt kötet, egy figyelmeztető üzenet megjelenik az Ön konfigurációjának véleményezésére.

### <a name="data-security-and-encryption"></a>Adatok biztonsági és titkosítás:

A StorSimple virtuális tömb adatok biztonsági és titkosítási szolgáltatást, amely biztosítja a bizalmas és az adatok integritását tartalmaz. Ezek a funkciók használata esetén ajánlott kövesse az alábbi ajánlott eljárást: 

-   Adja meg a felhőalapú tárolási titkosítási kulcs AES-256 titkosítási létrehozásához, az adatokat a felhőbe a virtuális tömbből elküldése előtt. A kulcs nincs szükség, ha az adatok titkosított rajta. A kulcs is létrehozott, és biztonságos, például az [Azure kulcs tárolóra](../key-vault/key-vault-whatis.md)kulcskezelő rendszert használ maradjon.

-   A StorSimple kezelő szolgáltatás a tárhely fiók beállításakor győződjön meg arról, hogy engedélyezze az SSL mód létrehozása a biztonságos csatornát a hálózati kommunikáció a StorSimple és a felhő között.

-   A tároló fiókok billentyűparancsai (szerint újra szeretne hozzáférni az Azure tárhelyszolgáltatáshoz) rendszeres eléréséhez módosításokat-fiókjához alapján rendszergazda módosított listáját.

-   A virtuális tömbben lévő adatok tömörített és Azure elküldés előtt deduplicated. Az adatok Deduplication szerepkör-szolgáltatás használata a Windows Server-szolgáltató nem javasoljuk.


## <a name="operational-best-practices"></a>Műveleti ajánlott eljárások

Az üzemeltetési gyakorlati tanácsokat olyan útmutatások, kell követni a mindennapi felügyeleti vagy a virtuális tömb során. Ezekkel az eljárásokkal vonatkoznak, például biztonsági másolatok visszaállítása egy biztonsági csoportjából feladatátvevő elvégzéséhez véve adott engedélykezelési feladatai, inaktiválása és törlése a tömb rendszer használatát és állapot ellenőrzése és a víruskeresés ellenőrzi a virtuális tömb a.

### <a name="backups"></a>Biztonsági másolatok

A virtuális tömbben lévő adatok biztonsági másolatot készíteni a felhőbe kétféle módon, egy alapértelmezett automatikus napi biztonsági mentése az egész eszköz indítása 22:30 vagy igény szerinti kézi biztonsági keresztül. Alapértelmezés szerint az eszköz automatikusan létrehozza a napi felhő pillanatképek tartózkodó azt az összes adatot. További információért lépjen a [biztonsági másolatot készíteni a StorSimple virtuális tömb](storsimple-ova-backup.md).

A gyakoriság és az alapértelmezett biztonsági másolatok társított adatmegőrzési nem módosíthatók, de az idő, amelynél a napi biztonsági másolatok mindennap kezdeményezett beállíthatja. Az automatikus biztonsági másolatok a kezdési időpontot a tartalomkeresési, akkor javasoljuk, hogy:

-   A biztonsági másolatok ütemezése essen. Biztonsági másolat kezdetének nem egybe kell számos host IO.

-   Igény szerinti kézi biztonsági kezdeményezhet, egy eszköz feladatátvevő vagy a virtuális tömbben lévő adatok védelme előtt a Karbantartás ablakban végrehajtásához tervezésekor.

### <a name="restore"></a>Visszaállítása

Egy biztonsági másolatból kétféleképpen állítsa vissza tudja állítani: visszaállítása másik mennyiségi vagy megosztás vagy helyreállításhoz egy elemszintű (csak egy virtuális tömböt fájlkiszolgálóra konfigurált elérhető). Elemszintű helyreállítási lehetővé teszi a fájlok és mappák részletes helyreállítás StorSimple az eszközre a megosztások egy felhőalapú biztonsági másolatból. További információért lépjen [visszaállítása biztonsági másolatból](storsimple-ova-restore.md).

A visszaállítás végrehajtásakor tartsa szem előtt az alábbi útmutatásokat:

-   A StorSimple virtuális tömb nem támogatja a helyi visszaállítása. Ez azonban könnyen lehet megvalósítani egy két lépésből álló folyamat: tömb és egy másik mennyiségi/megosztás majd visszaállítja a virtuális lévő térközt hozhat.

-   Egy helyi mennyiségi visszaállításával tartsa szem előtt a visszaállítás lesznek egy ideig futó műveletet. Abban az esetben, ha a mennyiségi gyorsan online állapotba, az adatok továbbra is a háttérben hidratált kell.

-   A mennyiségi típus változatlan marad a visszaállítás során. A többszintű mennyiségi vissza egy másik többszintű mennyiségi, és egy másik helyileg helyileg rögzített kötet kiemelt mennyiségi.

-   A hangerőt, vagy a megosztás visszaállítása biztonsági másolatból meg beállítása, ha nem sikerül a visszaállítási feladat, a cél mennyiségi, vagy megosztás továbbra is hozható létre a portálon. Fontos, hogy ez használaton kívüli cél mennyiségi törlése vagy a portálon: Ez az elem jövőbeli kérdéseket minimalizálásához megosztása.

### <a name="failover-and-disaster-recovery"></a>Feladatátvétel és katasztrófa helyreállítás

Eszköz feladatátvevő lehetővé teszi az adatok a *forrás* eszközről a adatközpontban áttelepítése ugyanabban vagy egy másik földrajzi helye, amely egy másik *cél* eszközre. Az eszköz feladatátvevő nem a teljes eszközre. Feladatátvételkor a forrás eszköz adatainak felhőbe módosításaira tulajdonjogát, amely a célként megadott eszköz.

A StorSimple virtuális tömb, csak sikerül fölé egy másik, azonos StorSimple kezelő szolgáltatás felügyelt virtuális tömböt. Egy 8000 sorozat eszközön vagy egy másik StorSimple kezelő szolgáltatás (mint az adatforrás-eszközhöz) által felügyelt tömb egy áttérni nem engedélyezett. Többet szeretne tudni a feladatátvevő szempontok, keresse fel [az eszköz feladatátvevő előfeltételei](storsimple-ova-failover-dr.md).

Végrehajtásakor egy fail fölé a virtuális tömb, tartsa szem előtt a következőket:

-   Tervezett feladatátvételt célszerű ajánlott a legjobb összes a kötet/megosztás offline előtt a feladatátvételi kezdeményezése állapotba. Kövesse az operációs rendszer-specifikus utasításokat az állomáson a kötet/megosztások offline első készíteni, majd kattintson azok offline állapotba virtuális eszközén.

-   A fájl kiszolgálói vészhelyreállítás (DR) javasoljuk, csatlakozás a célként megadott eszköz forrásaként ugyanahhoz a tartományhoz, hogy automatikusan megtörténik a megosztási engedélyeket. Csak a célként megadott eszköz ugyanabban a tartományban való áttérés támogatja a jelenlegi kiadásába.

-   Miután sikeresen befejeződött a DR, a rendszer automatikusan törli az adatforrás eszközt. Abban az esetben, ha az eszköz már nem érhető el, a virtuális gép, amely a host rendszeren, kiépítéstől továbbra is van felhasználás erőforrásokat. Azt javasoljuk, hogy a virtuális számítógéphez törlése a host rendszer meg, hogy minden díjat merülnek fel.

-   Megjegyzendő, hogy akkor is, ha a feladatátvétel sikertelen, **adatai mindig biztonságos a felhőben**. Fontolja meg a következő három helyzetleírás, amelyben a feladatátvételi nem fejeződött be:

    -   Hiba történt, például amikor DR előtti ellenőrzést történik a feladatátvételi kezdeti fázisaiban. Ebben az esetben a célként megadott eszköz továbbra is használható. A cél ugyanarra az eszközre feladatátvételi is újra.

    -   Hiba történt a tényleges feladatátvevő folyamat során. Ebben az esetben a célként megadott eszköz van megjelölve használhatatlanná válik. Építse és egy másik cél virtuális tömböt konfigurálása és kell használnia, amely feladatátvételi.

    -   A feladatátvétel befejeződött, amelyet törölt a forrás eszköz követően, de a célként megadott eszköz magában a kérdések és semmilyen adatot sem fér hozzá. Az adatok továbbra is biztonságban vannak-e a felhőben, és könnyen olvasható egy másik virtuális tömb létrehozása, és kattintson az használata azt a cél eszközként a DR.

### <a name="deactivate"></a>Inaktiválása

Ha inaktiválta egy StorSimple virtuális tömböt, az eszköz és a megfelelő StorSimple kezelő szolgáltatás közötti kapcsolat Server alkalmazás. Az inaktiválás **végleges** művelet, és nem vonható vissza. Inaktív eszköz nem lehet regisztrálni a StorSimple Manager szolgáltatással újra. További információért lépjen [a StorSimple virtuális tömb inaktiváljon](storsimple-deactivate-and-delete-device.md).

Tartsa szem előtt az alábbi gyakorlati tanácsokat amikor a virtuális tömb inaktiválása:

-   A virtuális eszköz inaktiválásával előtt az adatok felhőben pillanatkép. Ha inaktiválta egy virtuális tömböt, a helyi eszköz minden adat elvész. Pillanatkép készítése felhő lehetővé teszi, a helyreállítani az adatokat egy későbbi szakaszában.

-   Csak inaktiválta egy StorSimple virtuális tömböt, hogy leállítása vagy törlése az ügyfelek és a hosts függnek, hogy az eszköz.

-   Inaktív eszköz törlése, ha már nem használja, hogy azt nem felmerülés díjak.

### <a name="monitoring"></a>Figyelése

Győződjön meg arról, hogy a StorSimple virtuális tömb megfelelő folyamatos állapotban van, szüksége figyelheti a tömb, valamint információk kapott a rendszer, például az értesítéseket. Lync-általános állapotát, a virtuális tömb, az alábbi gyakorlati tanácsokat végrehajtása:

- Állítsa be a virtuális tömb adatok lemez, valamint az operációs rendszer szabad lemezterület nyomon követéséhez figyelése. Ha a Hyper-V szolgáltatást futtató, System Center virtuális gép Manager (SCVMM) és a System Center műveletek Manager (SCOM) figyelheti a virtualizációs hosts is használhatja.   

- A virtuális tömb értesítések küldésére bizonyos használatát szinten értesítő e-mailek konfigurálhatja.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>A keresési index és a vírus-ellenőrzési alkalmazások

Egy StorSimple virtuális tömböt is automatikusan az első csoportba tartozó a Microsoft Azure felhőbe a helyi réteg adatait. Az alkalmazások, például egy indexben keresés vagy a víruskeresés StorSimple-on tárolt adatok beolvasásához használatakor kell, hogy a felhőben adatok nem érhető el és lekért vissza szeretné a helyi réteg gondoskodik.

Azt javasoljuk, hogy a következő tanácsok végrehajtása a virtuális tömb az index keresési, illetve vírus-ellenőrzés beállításakor:

-   Automatikusan beállított teljes pásztázás műveletek letiltása.

-   Többszintű kötet vagy állítson be az index keresési vírus beolvasás egy növekményes víruskeresési. A szeretné ellenőrizni, hogy csak az új adatok valószínűleg tartózkodó a helyi réteg. Az adatok, amelyek a felhőbe többszintű növekményes művelet során nem érhető el.

-   A helyes keresési szűrők és a beállítások van konfigurálva, hogy csak az olyan fájltípusok beolvasott beszerzése érdekében. Például kép (JPEG, GIF és TIFF) fájlok és műszaki rajzokat kell nem ellenőrzi a növekvő vagy a teljes indexet újraépítő során.

Ha az indexelő folyamat Windows, kövesse az alábbiakat:

-   Ne használja a Windows indexelő többszintű kötet, akkor visszahívása nagy mennyiségű adatot (TBs) a felhőből, ha az index kell lennie az újonnan létrehozott gyakran. Az index Újraépítés volna beolvashatja tartalmuk indexelendő összes fájltípusok.

-   Használja a Windows az indexelő folyamat helyileg rögzített kötet, ez csak szeretne elérni a a helyi rétegek létrehozhatja a tárgymutatót (a felhőben adatokat nem éri el) az adatokat.

### <a name="byte-range-locking"></a>Byte tartomány zárolása

Alkalmazások bájt a megadott tartomány zárolhatja belül a fájlokat. Ha bájt tartomány zárolása engedélyezve van a StorSimple ír alkalmazásokkal, majd tiering nem működik a virtuális tömb. Tiering használata a fájlok elérhető összes részeinek nem zárolt kell lennie. A virtuális tömb a többszintű kötet bájt tartomány zárolása nem támogatott.

Ajánlott orvoslására bájt tartomány zárolása a következők:

-   Kapcsolja ki az alkalmazás logika a zárolás bájt tartományban.

-   A kiemelt kötet helyileg használata (Ha nem a többszintű) ezzel az alkalmazással kapcsolatos adatokhoz. Helyi meghajtóra rögzített kötet nem az első csoportba tartozó be a felhőben.

-   Ha használ helyben a kiemelt kötet bájt tartomány zárolás engedélyezve, a mennyiségi származnak online befejezése a visszaállítás előtt. Ezekben az esetekben meg kell várnia, hogy a visszaállítás.

## <a name="multiple-arrays"></a>Több tömb

Több virtuális tömbök szükség lehet a növekvő munkakészlet adatokat tartalmazó elérése a felhőben, így a az eszköz teljesítményét érintő sikerült spill-fiókjához telepíthető. Ezekben az esetekben célszerű skála eszközök munkakészlet növekedésével. Ehhez egy vagy több eszközön hozzá kell adni a helyszíni adatok központban. Az eszközök fel, akkor a következőket teheti:

-   Felosztás adatok aktuális készlete.
-   Új munkaterhelésekből üzembe új hangereje megfelelően.
-   Ha több virtuális tömbök telepít, akkor javasoljuk, hogy a terheléselosztási szempontjából a tömb elosztása különböző hipervizor hosts.

-  Több virtuális tömbök (ha konfigurált egy fájlt vagy iSCSI-kiszolgáló) terjesztett fájl rendszer Namespace telepíthető. A lépések részletes leírását lépjen [Terjesztett fájl rendszer Namespace megoldás hibrid felhőalapú tárolási telepítési útmutatóját](https://www.microsoft.com/download/details.aspx?id=45507). Az elosztott fájlrendszer replikációs jelenleg nem ajánlott a virtuális tömb való használatra. 


## <a name="see-also"></a>Lásd még:
További tudnivalók [felügyelheti a StorSimple virtuális tömb](storsimple-ova-manager-service-administration.md) StorSimple Manager Service.
