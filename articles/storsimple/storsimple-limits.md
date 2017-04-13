<properties 
   pageTitle="StorSimple rendszer korlátai |} Microsoft Azure"
   description="Rendszer korlátai és ajánlott méretben StorSimple 8000 sorozat összetevők és a kapcsolatokkal ismerteti."
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

# <a name="storsimple-system-limits"></a>StorSimple rendszer korlátai

## <a name="overview"></a>– Áttekintés

StorSimple méretezhető és rugalmas tároló biztosít az Adatközpont. Azonban néhány korlátozás van érvényben, akkor érdemes szem előtt tartani, a terv, telepítése, és a StorSimple megoldás működnek. Az alábbi táblázat ismerteti a korlátok és néhány javaslatot, hogy akkor is hatékony kihasználása a StorSimple megoldás.

| Korlát azonosító | Határérték | Megjegyzések |
|----------------- | ------|--------- |
| Tárterület-fiókhoz tartozó hitelesítő adatok maximális száma | 64 | |
| Mennyiségi tárolók maximális száma | 64 | |
| Kötet maximális száma | 255 karakter | |
| Helyi meghajtóra rögzített kötet maximális száma | 32 | |
| Kimutatások használati sávszélesség-sablon maximális száma | 168 | Óránként, a hét (24 * 7) mindennap ütemezése. |
| A fizikai eszközök többszintű kötet maximális mérete | 64 TB 8100 és 8600 | 8100 és 8600 olyan fizikai eszközök. |
| A többszintű mennyiségi az Azure virtuális eszközön maximális mérete | 30 TB 8010 <br></br> 64 TB 8020 | 8010 és 8020 az Azure virtuális használó szabványos tároló és támogatási szolgáltatások rendre eszközök. |
| A fizikai eszközök helyileg rögzített kötet maximális mérete | 8100 8.5 TB <br></br> 8600 22.5 TB | 8100 és 8600 olyan fizikai eszközök. |
| ISCSI kapcsolatok maximális száma | 512 | |
| A kezdeményezők iSCSI kapcsolatok maximális száma | 512 | |
| Access vezérlő rekordok száma eszköz maximális száma | 64 | |
| Biztonsági másolat házirend használati kötet maximális száma | 24 | |
| Biztonsági másolatok tartja meg az ütemterv (a biztonságimásolat-házirend) / maximális száma | 64 | |
| Biztonsági másolat házirend használati ütemezések maximális száma | 10 | |
| Bármilyen típusú, amely egy mennyiségi is megmaradnak pillanatképek maximális száma | 256 | A telefonszám tartalmaz pillanatképek helyi és felhőbeli pillanatképek. |
| Egy adott időpontban érvényes, amely bármely eszközön bemutató maximális száma | 10 000 | |
| A biztonsági másolat, visszaállítás párhuzamosan feldolgozhatók, illetve klónozhatja kötet maximális száma | 16 |<ul><li>Ha több mint 16 kötet, azok feldolgozása egymás után amint elérhetővé válnak a feldolgozás helyek.</li><li>Új biztonsági másolatait egy klónozott vagy visszaállított többszintű kötet mindaddig, amíg befejeződik a művelet nem hajtható végre. Egy helyi kötet, azonban biztonsági másolatok engedélyezettek után a mennyiségi online állapotban.</li></ul>|
| Visszaállítás és adatfeliratsor helyreállítása többszintű kötet ideje | < 2 percig tart | <ul><li>A hangerő elérhetővé válik a visszaállítás, illetve az adatfeliratsor működését, függetlenül a kötet mérete 2 percen belül.</li><li>Lehet, hogy a kötet teljesítmény kezdetben lassabban, a legtöbb adatok és metaadatok továbbra is megtalálható a felhőben. Teljesítmény növelheti, adatfolyamok a felhőből az StorSimple eszközre.</li><li>Metaadat-alapú letöltése a teljes időt attól függ, hogy a hozzárendelt mennyiség méretét. Metaadat-alapú automatikusan vett fel, a háttérben az 5 perc hozzárendelt mennyiség adatok TB-os eszközbe. Ezt az értéket a felhőbe internetes sávszélessége is érinti.</li><li>A visszaállítás vagy adatfeliratsor művelet befejeződött, amikor az eszközén található összes metaadat.</li><li>Biztonsági másolat műveletek nem hajthatók végre, amíg a visszaállítás vagy adatfeliratsor művelet teljesen befejeződik.|
| Helyi meghajtóra rögzített kötet helyreállítása ideje visszaállítása | < 2 percig tart | <ul><li>A hangerő elérhetővé válik a visszaállítási művelet, függetlenül a kötet mérete 2 percen belül.</li><li>Lehet, hogy a kötet teljesítmény kezdetben lassabban, a legtöbb adatok és metaadatok továbbra is megtalálható a felhőben. Teljesítmény növelheti, adatfolyamok a felhőből az StorSimple eszközre.</li><li>Metaadat-alapú letöltése a teljes időt attól függ, hogy a hozzárendelt mennyiség méretét. Metaadat-alapú automatikusan vett fel, a háttérben az 5 perc hozzárendelt mennyiség adatok TB-os eszközbe. Ezt az értéket a felhőbe internetes sávszélessége is érinti.</li><li>Többszintű kötet, helyben rögzített kötet, eltérően mennyiségi adatokat is letöltése helyi meghajtóra az eszközön. A visszaállítási művelet befejeződött, amikor a kötet adatait hozni az eszközt.</li><li>Lehet, hogy a visszaállítási művelet hosszú. A visszaállítás végrehajtásához teljes ideje méretét a kiépített helyi kötet, az internetes sávszélessége és a meglévő adatok az eszközön függ. A helyi meghajtóra rögzített kötet biztonsági műveletek végezhetők a visszaállítási művelet alatt.|
|Felhőalapú pillanatképek kamatlábát feldolgozása| 15 percet/TB | <ul><li>Minimális ideig, hogy az a felhő pillanatfelvétel feltöltés TB-nyi hozzárendelt mennyiség adatok biztonsági mentése egy kész. </li><li> Teljes felhő pillanatkép idő összeadásával Ez esetben a feltöltés hatással van az Internet-sávszélesség cloud pillanatkép idő.
| Maximális ügyfél olvasási/írási átviteli (Ha a SSD réteg a felszolgált) * | 920/720 MB/s egyetlen 10 GbE hálózati kapcsolat | Legfeljebb 2 x MPIO és két hálózati kapcsolatok. |
| Maximális ügyfél olvasási/írási átviteli (Ha a merevlemez réteg a felszolgált) * | 120/250 MB/s |
| Maximális ügyfél olvasási/írási átviteli (Ha a sorrend az a felhő réteg),* a frissítés 3 és újabb verziók** | 40/60 MB/s többszintű kötet<br><br>60/80 MB/s kötet többszintű a mennyiségi létrehozása során archiválás választógombbal | Olvasási kapacitása ügyfelek létrehozása és fenntartása elegendő I/O várólista mélység függ. <br><br>Sebesség érhető el, az alapul szolgáló tárterület-fiókkal sebességének függ. | 

& #42; 100 %-os olvasási és írási 100 %-os esetek mérték maximális sebesség per bemenet/kimenet típusa. Tényleges átvitel lehet kisebb és attól függ, hogy i/o-keverjen ki, és a hálózati feltételek.

& #42; & #42; Lehet, hogy a teljesítmény számok 3 frissítés előtt alsó.

## <a name="next-steps"></a>Következő lépések

Tekintse át a [StorSimple rendszerkövetelményei](storsimple-system-requirements.md). 
