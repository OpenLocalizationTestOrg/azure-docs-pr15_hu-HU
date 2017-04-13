Az Azure virtuális gép támogatja az adatok lemezt számos csatolása. Az optimális teljesítmény eléréséhez érdemes lehetséges szabályozásának elkerülése érdekében a virtuális géphez csatlakoztatott erősen területekre lemezt számának korlátozása. Ha az összes lemez nem készül erősen használhatók egyszerre, a tárterület-fiók támogatniuk kell a egy nagyobb szám lemezt.

- **Szabványos tároló fiókok:** Egy szabványos tárterület-fiókkal rendelkezik 20 000 IOPS teljes kérések maximális sebesség. A teljes IOPS összes a virtuális gépen merevlemezeken egy szabványos tárterület-fiókban nem haladhatja meg ezt a korlátot.

    Egy a kérelem ráta által alapján egységes tárterület-fiók által támogatott erősen területekre lemez számát nagyjából ki kell számítani. A példában egy egyszerű réteg virtuális erősen területekre lemezt maximális száma 66 (20 000/300 IOPS egy lemezen), és a szokásos réteg virtuális, körülbelül 40 (20 000/500 IOPS egy lemezen), célszerű az alábbi táblázatban látható módon. 
 
- **Prémium verzió tároló használata:** A prémium tároló fióknak 50 GB/s teljes maximális sebesség mértéke. A teljes kapacitásának összes virtuális lemezt keresztül nem haladhatja meg ezt a korlátot.