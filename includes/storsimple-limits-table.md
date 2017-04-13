<!--author=alkohli last changed: 12/15/15-->

| Korlát azonosító | Határérték | Megjegyzések |
|----------------- | ------|--------- |
| Tárterület-fiókhoz tartozó hitelesítő adatok maximális száma | 64 | |
| Mennyiségi tárolók maximális száma | 64 | |
| Kötet maximális száma | 255 karakter | |
| Kimutatások használati sávszélesség-sablon maximális száma | 168 | Óránként, a hét (24 * 7) mindennap ütemezése. |
| A fizikai eszközök többszintű kötet maximális mérete | 64 TB 8100 és 8600 | 8100 és 8600 olyan fizikai eszközök. |
| A többszintű mennyiségi az Azure virtuális eszközön maximális mérete | 30 TB 8010 <br></br> 64 TB 8020 | 8010 és 8020 az Azure virtuális használó szabványos tároló és támogatási szolgáltatások rendre eszközök. |
| A fizikai eszközök helyileg rögzített kötet maximális mérete | 8100 9 TB <br></br> 24 TB 8600 | 8100 és 8600 olyan fizikai eszközök. |
| ISCSI kapcsolatok maximális száma | 512 | |
| A kezdeményezők iSCSI kapcsolatok maximális száma | 512 | |
| Access vezérlő rekordok száma eszköz maximális száma | 64 | |
| Biztonsági másolat házirend használati kötet maximális száma | 24 | |
| Biztonsági másolatok tartja meg az biztonsági házirend használati maximális száma | 64 | |
| Biztonsági másolat házirend használati ütemezések maximális száma | 10 | |
| Bármilyen típusú, amely egy mennyiségi is megmaradnak pillanatképek maximális száma | 256 | Ide tartoznak a pillanatképek helyi és felhőbeli pillanatképek. |
| Egy adott időpontban érvényes, amely bármely eszközön bemutató maximális száma | 10 000 | |
| A biztonsági másolat, visszaállítás párhuzamosan feldolgozhatók, illetve klónozhatja kötet maximális száma | 16 |<ul><li>Ha 16-nál több kötet, azok a fog amint elérhetővé válnak a feldolgozás helyek egymás után feldolgozása.</li><li>Új biztonsági másolatait egy klónozott vagy visszaállított többszintű kötet mindaddig, amíg befejeződik a művelet nem hajtható végre. Egy helyi kötet, azonban biztonsági másolatok engedélyezettek után a mennyiségi online állapotban.</li></ul>|
| Visszaállítás és adatfeliratsor helyreállítása többszintű kötet ideje | < 2 percig tart | <ul><li>A hangerő elérhetővé válik a visszaállítás, illetve az adatfeliratsor működését, függetlenül a kötet méretét 2 percen belül.</li><li>Lehet, hogy a kötet teljesítmény kezdetben lassabban, a legtöbb adatok és metaadatok továbbra is megtalálható a felhőben. Teljesítmény növelheti, adatfolyamok a felhőből az StorSimple eszközre.</li><li>Metaadat-alapú letöltése a teljes időt attól függ, hogy a hozzárendelt mennyiség méretét. Metaadat-alapú automatikusan vett fel, a háttérben az 5 perc hozzárendelt mennyiség adatok TB-os eszközbe. Ezt az értéket a felhőbe internetes sávszélessége is érinti.</li><li>A visszaállítás vagy adatfeliratsor művelet befejeződött, amikor az eszközén található összes metaadat.</li><li>Biztonsági másolat műveletek nem hajthatók végre, amíg a visszaállítás vagy adatfeliratsor művelet teljesen befejeződik.|
| Helyi meghajtóra rögzített kötet helyreállítása ideje visszaállítása | < 2 percig tart | <ul><li>A hangerő elérhetővé válik a visszaállítási művelet, függetlenül a kötet mérete 2 percen belül.</li><li>Lehet, hogy a kötet teljesítmény kezdetben lassabban, a legtöbb adatok és metaadatok továbbra is megtalálható a felhőben. Teljesítmény növelheti, adatfolyamok a felhőből az StorSimple eszközre.</li><li>Metaadat-alapú letöltése a teljes időt attól függ, hogy a hozzárendelt mennyiség méretét. Metaadat-alapú automatikusan vett fel, a háttérben az 5 perc hozzárendelt mennyiség adatok TB-os eszközbe. Ezt az értéket a felhőbe internetes sávszélessége is érinti.</li><li>Eltérően többszintű kötet, helyben rögzített kötet, amíg a mennyiségi adatokat is letöltése helyi meghajtóra az eszközön. A visszaállítási művelet befejeződött, amikor a kötet adatait hozni az eszközt.</li><li>Lehet, hogy a visszaállítási művelet hosszú, és a visszaállítás végrehajtásához teljes ideje méretét a kiépített helyi kötet, az internetes sávszélessége és a meglévő adatok az eszközön függ. A helyi meghajtóra rögzített kötet biztonsági műveletek végezhetők a visszaállítási művelet alatt.|
| Elérhetőség vékony visszaállítása | Utolsó feladatátvevő | |
| Maximális ügyfél olvasási/írási átviteli (Ha a SSD réteg a felszolgált) * | 920/720 MB/s egy egyetlen 10GbE a hálózati kapcsolat | Legfeljebb 2 x MPIO és két hálózati kapcsolatok. |
| Maximális ügyfél olvasási/írási átviteli (Ha a merevlemez réteg a felszolgált) * | 120/250 MB/s |
| Maximális ügyfél olvasási/írási átviteli (Ha a sorrend az a felhő réteg) * | 11/41 MB/s | Olvasás átviteli ügyfelek létrehozása és fenntartása elegendő I/O várólista mélység függ. |

& #42; A 100 %-os olvasási és 100 %-os írási esetek mérték maximális sebesség per bemenet/kimenet típusa. Tényleges átvitel lehet kisebb és attól függ, hogy i/o-keverjen ki, és a hálózati feltételek.
