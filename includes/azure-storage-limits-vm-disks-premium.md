**Virtuális gép lemez: egy fiókot korlátai**

Erőforrás|Alapértelmezett korlát
---|---
Fiók egy teljes kapacitásával|35 TB
Fiók egy teljes pillanatkép kapacitás|10 TB
Max sávszélességének fiók (bejövő adatok + kilépési<sup>1</sup>)|< 50 GB/s =

<sup>1</sup> Minden adat (kérelmek) tárterület-fiókjába küldött *bejövő adatok* hivatkozik. Az összes adat (válasz) tárterület-fiókból fogadott *kilépési* hivatkozik.

**Virtuális gép lemez: / lemez korlátai**

Prémium tároló lemez típusa | P10 | P20 | P30
---|---|---|---
Lemez mérete | 128 orrot | 512 orrot | 1024 orrot (1 TB)
Max IOPS egy lemezen | 500 | 2300 | 5000
Max átviteli egy lemezen | Másodpercenként 100 MB | 150 MB / szekundum | 200 MB / szekundum
Lemezen per tároló fiók maximális száma | 280 | a 70 | 35-tel

**Virtuális gépen lemez: egy virtuális korlátai**

Erőforrás|Alapértelmezett korlát
---|---
Max IOPS egy virtuális|A GS5 80,000 IOPS virtuális<sup>1</sup>
Max átviteli egy virtuális|2000 MB/s GS5 virtuális<sup>1</sup>

<sup>1</sup> Olvassa el az [Virtuális](../articles/virtual-machines/virtual-machines-linux-sizes.md) méretre más virtuális méretű vonatkozó korlátok. 
