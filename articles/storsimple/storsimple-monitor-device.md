<properties 
   pageTitle="Figyelheti a StorSimple eszköz |} Microsoft Azure"
   description="A StorSimple kezelő szolgáltatás használata a Lync-I/O teljesítmény, kapacitás kihasználása, hálózati teljesítmény és eszköz teljesítményének ismerteti."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>A StorSimple kezelő szolgáltatás használata a Lync-StorSimple eszköze 

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás adott eszközök figyelni a StorSimple megoldás belül is használhatja. Egyéni diagramok I/O teljesítmény, kapacitás kihasználtsági, hálózati teljesítmény és eszköz teljesítménymutatók alapján hozhat létre. 

Az ellenőrző adatokat egy adott eszközhöz az Azure klasszikus portálon megtekintéséhez jelölje ki a StorSimple kezelő szolgáltatás. Kattintson a **Monitor** fülre, és válassza az eszközök listája. A **Monitor** lap a következő információkat tartalmazza.

## <a name="io-performance"></a>I/O teljesítménye 

**I/O** teljesítménymutatók számok kapcsolódó olvasható számát, majd közötti vagy műveleteket iSCSI kezdeményező kapcsolódási írhat a kiszolgáló és az eszköz vagy az eszközön, és a felhőben. A teljesítmény egy adott mennyiségi, egy adott mennyiségi tároló vagy az összes mennyiségi tárolók mérhető.

A diagram az alábbi jeleníti meg az I/O a kezdeményező egy gyártási eszközhöz a kötet eszközére. Rajzolja ki a mértékek olvasása és írása / szekundum bájt, olvassa el és írási IO műveletek / szekundum, és olvasása és írása késések.

![IO teljesítmény kezdeményező az eszközre](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Az ugyanarra az eszközre a bemeneti és kimeneti műveletet az adatok az eszközről a felhőbe összes mennyiségi tárolók rajzolja ki. Ezen az eszközön az adatok csak olyan a lineáris réteg, és semmi sem rendelkezik kiömlött a felhőbe. Nincsenek eszközről a felhőbe történő írási és olvasási műveletek. A diagram csúcsok így időközönként az 5 perc, amely megfelel a gyakoriság, amelynél a a szívveréséhez be van jelölve az eszköz és a szolgáltatás között van. 

![IO teljesítmény cloud eszközről](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


2:00 pm kezdve mennyiségi adatok ugyanarra az eszközre, felhőalapú pillanatkép lett venni. Ennek következtében adatok halad az eszközről a felhőben. Ez az időtartam olvasása-írása a felhőbe voltak fel. A IO diagram a különböző mérési módja miatt az idő, amikor a pillanatkép felvételének megfelelő csúcsot jeleníti meg. 

![IO teljesítmény felhő pillanatképet cloud eszköz](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>A kapacitás kihasználása 

**Kapacitás kihasználtsági** nyomon követi a kötet, mennyiségi tárolók vagy eszköz által használt tárterület adatok mennyiségét kapcsolódó mértékek. Az elsődleges tárhely, a felhőbeli tárhelyről vagy a eszköz tároló kapacitás hasznosítása alapján jelentéseket hozhat létre. Egy adott kötet, egy adott mennyiségi tároló vagy az összes mennyiségi tárolók kapacitás kihasználása mérhető.


Elsődleges, a felhőben, és a következőképpen eszköz tárolókapacitású írható le:

###<a name="primary-storage-capacity-utilization"></a>Elsődleges tároló kapacitás kihasználása
 
A fenti diagramokról előtt az adatok deduplicated és tömörített StorSimple kötet írt adatok mennyiségét mutatják. Az elsődleges tárterület-kihasználtság megtekintése összes kötet vagy egyetlen kötet.

Ha az elsődleges tároló mennyiségi kapacitás kihasználtsági diagramok az összes kötet, és minden egyes kötet megjelenítése, majd mindkét ezekben az esetekben az elsődleges adatokat összesíteni, lehetnek nem egyezik a két szám közötti. Az összes kötet teljes elsődleges adatok előfordulhat, hogy nem adja ki az elsődleges adatok az egyes mennyiségének összegeként. Az alábbi okai lehetnek:

- **Az összes kötet felvett pillanatkép adatok**: Ez a probléma csak akkor, ha a frissítés 3-nél korábbi verziót futtatja látható. Az összes kötet látható elsődleges adatai az elsődleges minden kötet és a pillanatkép adatok összegét. Az elsődleges adatok jelennek meg egy adott kötet csak a hangerőt a kiosztott adatok mennyiségét felel meg (és nem tartalmaz mennyiségi pillanatkép adatok az).

    Ez is magyarázható az alábbi egyenlettel történik:

    *Elsődleges adatok (összes kötet) = SZUM (elsődleges adatok (i mennyiségi) + (i mennyiségi) pillanatkép adatok mérete)*
    
    *Ha az elsődleges adatok (i mennyiségi) = az i mennyiségi rendelt elsődleges adatok mérete*
 
    Ha a pillanatképek törlődnek a szolgáltatáson keresztül, a Törlés a háttérben aszinkron kész gombra. Pillanatkép törlését követően frissíteni kell a mennyiségi adatok mérete némi időbe telhet. 

    Ha a frissítés fut, 3 és újabb verziók, majd pillanatkép adatokat nem szerepel a mennyiségi adatok. És az elsődleges kihasználtsági képlettel számíthatja:

    * Elsődleges adatok (összes kötet) = SZUM (elsődleges adatok (mennyiségi i)
    
    *Ha az elsődleges adatok (i mennyiségi) = az i mennyiségi rendelt elsődleges adatok mérete*
 
- **Kötet figyelése letiltja az összes kötet szereplő**: kötet az eszközön, amelynek figyelése ki van kapcsolva, ha az ellenőrzési adatok e egyes mennyiségek nem lesz elérhető a diagramon. Azonban a diagram összes kötet adatait tartalmazza a kötet, amelynek figyelése ki van kapcsolva. 
 
- **Törölt kötet tartalmazza az összes kötet élő társított biztonsági másolat**: Ha törli a kötet pillanatkép adatokat tartalmazó, de továbbra is megmarad a társított pillanatképek, majd, nem egyezik meg.

- **Az összes kötet szereplő törölt kötet**: bizonyos esetekben régi kötet előfordulhat, hogy létezik, akkor is, ha ezeket a törölt. Nem látható a törlési hatása és igazolhatja, hogy az eszköz alsó kapacitással. Lépjen kapcsolatba a Microsoft Support ezek kötet eltávolítása szükség.

A következő oszlopdiagramokon StorSimple eszköz elsődleges tároló kapacitás hasznosítása előtt és után egy felhőalapú pillanatképet felvételének. Mivel ez csak mennyiségi adatok, egy felhőalapú pillanatképet ne módosítsa az elsődleges tároló. Amint látható, a diagram a felhő pillanatkép készítése eredményeként az elsődleges kapacitás kihasználtsági nincs különbség jeleníti meg. A felhő pillanatkép körülbelül 2:00 pm eszközön futó használatának lépései.

![Elsődleges kapacitás kihasználtsági felhő pillanatkép előtt](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Felhőalapú pillanatképet elsődleges kapacitás kihasználása](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Ha futtatja a 2 frissítése, vagy nagyobb, oszthatja az elsődleges tároló kapacitás kihasználtsági lefelé az egyes kötet, összes kötet, összes többszintű kötet és összes helyileg rögzített kötet alább látható módon. Összes helyileg rögzített kötet szerinti bontásban lehetővé teszi, hogy gyorsan megállapítani, hogy mennyi a helyi réteg ki nem ürül.

![Az összes helyileg rögzített kötet elsődleges kapacitás kihasználása](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Felhőalapú tárolási kapacitás kihasználása

A fenti diagramokról használt felhőbeli tárhelyről mennyiségét mutatják. Ezeket az adatokat deduplicated és tömörítve. Ez az összeg felhő pillanatképek, előfordulhat, hogy adatokat tartalmaznak, amelyek nem minden elsődleges mennyiségi tükröződik és régebbi vagy szükséges adatmegőrzés célokra legyen, amely tartalmazza. Hasonlítsa össze az elsődleges, és bár a szám nem azonos a tároló fogyasztási adatokat arról csökkentése adatsebesség, hogy a felhő. A következő diagramok megjelenítése, felhőalapú tárolási kapacitás kihasználtsági StorSimple eszköz előtt és után egy felhőalapú pillanatképet felvételének. A felhő pillanatkép lépések az körülbelül 2:00 pm eszközön futó, és láthatja, hogy a felhőben kapacitás kihasználtsági felvétel egy időben 4.04 GB 5.73 MB növelésével.

![Felhőalapú kapacitás kihasználása felhő pillanatkép előtt](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![A felhő felhő pillanatképet kapacitás kihasználása](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Eszköz tároló kapacitás kihasználása

A fenti diagramokról megjelenítése a teljes kihasználtság az eszközhöz, amely több mint elsődleges tároló kihasználtsági lesz, mert a SSD lineáris réteg tartalmazza. A réteg adatokat tartalmazó is megtalálható összeg szerepel az eszköz a többi rétegek. A SSD lineáris réteg kapacitása ahhoz, hogy amikor az új adatok érkezik, a régi adatok kerül, a merevlemez réteg (ekkor deduplicated és tömörített) és ezt követően a felhőbe nem szakad.

Az idő múlásával elsődleges kapacitás kihasználása és eszköz kapacitás kihasználása valószínűleg nő közös mindaddig, amíg az adatok megkezdi a felhőbe többszintű kell. Ezen a ponton a eszköz kapacitás kihasználtsági valószínűleg elkezdi plateau, de az elsődleges kapacitás kihasználtsági nő, több adat íródott.

A következő oszlopdiagramokon StorSimple eszköz elsődleges tároló kapacitás hasznosítása előtt és után egy felhőalapú pillanatképet felvételének. A felhőben pillanatkép 2:00 du. lépések és az eszköz kapacitás kihasználtsági lépések adott időpontban csökkenő. Az eszköz tároló kapacitás kihasználtsági csökkent 11.58 GB 7.48 GB. Ez azt jelzi, hogy valószínűleg nem tömörített adatait a lineáris SSD réteg lett deduplicated, tömörítve, helyezi át a merevlemez réteg. Figyelje meg, hogy az eszköz amelynek már nagy mennyiségű adatot a SSD és a merevlemez-meghatározási, ha esetleg nem látható a csökkentése. Ebben a példában az eszköznek van egy kis mennyiségű adattal.

![Eszköz kapacitás kihasználtsági felhő pillanatkép előtt](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Eszköz kapacitás kihasználtsági felhő pillanatképet](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Hálózati teljesítmény

**Hálózati teljesítmény** nyomon követi a mértékek, a kiszolgáló és az eszközt, valamint az eszköz és a felhő között a hálózati kapcsolódási iSCSI kezdeményező az átvitt adatok mennyiségét kapcsolatos. Figyelheti a mérőszám az egyes iSCSI hálózati kapcsolatok az eszközön.

A következő diagramok megjelenítése a hálózaton átvitt adatok 0 és adatok 4, mindkét 1 GbE hálózati kapcsolatok az eszközön. Ebben a példában adatok 0 cloud-engedélyezték mivel adatok 4 iSCSI engedélyezve van. Megjelenik a bejövő és kimenő forgalmat StorSimple mobileszközére. A diagram 3:24 pm kezdve a strukturálatlan sorban van miatt arra, hogy szükséges adatok összegyűjtése csak 5 percenként, és figyelmen kívül hagyja. 

![Hálózati teljesítmény Data4 számára](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Hálózati teljesítmény Data4 számára](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Eszköz teljesítményének 

**Eszköz teljesítményének** nyomon követi a mértékek, az eszköz a teljesítménnyel kapcsolatos. A következő diagramon a Processzor kihasználtsági stat eszköz gyártási jeleníti meg.

![Processzor-kihasználtság eszköz](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [a StorSimple kezelő szolgáltatás eszköz irányítópult](storsimple-device-dashboard.md)használatáról.

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
