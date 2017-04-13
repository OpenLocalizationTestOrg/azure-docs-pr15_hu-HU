<properties
    pageTitle="Növekményes pillanatképek használata biztonsági mentése és helyreállítása Azure virtuális gépeken futó |} Microsoft Azure"
    description="Hozzon létre egy egyéni megoldás biztonsági mentése és helyreállítása a Azure virtuális gép merevlemezeken növekményes pillanatképek használata."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Biztonsági növekményes pillanatképek Azure virtuális gép lemezre mentése

## <a name="overview"></a>– Áttekintés

Tárterület Azure BLOB rögzítenie lehetővé teszi. Egy adott időpontban érvényes abban az időpontban blob állam rögzítése lehetőséget. Ez a cikk leírja azt eset: hogyan karbantarthatja pillanatképek használatáról virtuális gép lemezt a biztonsági másolatok. Ez a módszer is használhatja, válassza a nem használandó Azure biztonsági és helyreállítási szolgáltatás, és létrehoz egy egyéni biztonsági stratégia a virtuális gép lemezen.

Azure virtuális gép lemezt, oldal BLOB Azure-tárolóban lévő tárolja. Azt is hozhat létre a csoportot a jelen cikkben virtuális gép lemez biztonsági stratégia, mivel azt fogja kell hivatkozó pillanatképek, oldal BLOB környezetében. Többet szeretne tudni a pillanatképek, hivatkozni [Blob-pillanatfelvétel létrehozásának](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Mi az pillanatkép?

Egy blob-pillanatfelvétel blob, amely az időpontban fennálló rögzíthető írásvédett verziója. Pillanatkép létrehozása után azt is kell olvasni, másolja, vagy törölt, de nem módosíthatók. Egy adott időpontban érvényes blob kevésbé részletes adatokat a időpillanatban megjelenő ad lehetőséget. Addig, amíg a többi verziót 2015-04-05, az azt jelenti, hogy másolja a teljes pillanatképek van. A többi verziót 2015-07-08 és fölött, akkor is másolhatja növekményes pillanatképek.

## <a name="full-snapshot-copy"></a>Teljes pillanatkép másolása

A másolt pillanatképek másik tárterület-fiókhoz az alap blob biztonsági másolatait megtartása blob-ként. Pillanatkép is másolhatja a alap blob, amely olyan, mintha a blob visszaállítása a korábbi verziójú fölé. Pillanatkép fiókból másolja a program egy tároló között, amikor azt elfoglalja, az alap lap blob azonos térköz. Ezért másolása teljes pillanatképek tárterület-fiókból egy másikra lassú lesz, és is felhasználja sok helyet tároló cél fiók.

>[AZURE.NOTE] Ha más helyre másolja az alap blob, az elérhető információk, amelyek a blob nem kerülnek együtt. Hasonlóképpen ha felülírja az alap blob példányt, az alap blob társított pillanatképek nem érinti a probléma és alap blob neve alatti sértetlen marad.

### <a name="back-up-disks-using-snapshots"></a>Készítsen biztonsági másolatot a lemez pillanatképek használata

A virtuális gép merevlemezeken biztonsági stratégia, mint a lemez vagy lap blob periodikus pillanatfelvételek, és másolja a eszközök, például a [Másolás Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) -művelet vagy [AzCopy](storage-use-azcopy.md)fiókját egy másik tároló. Pillanatkép másolhat egy másik nevet a cél lap blob. Az eredményül kapott cél lap blob pedig írható lap blob pillanatkép nem. Ez a cikk későbbi részében azt leírja pillanatképek használatáról virtuális gép lemezt a biztonsági másolatok végrehajtandó lépéseket.

### <a name="restore-disks-using-snapshots"></a>Lemezen pillanatképek használata visszaállítása

Pedig a biztonsági másolat pillanatképek egyikét a rögzített stabil korábbi verzió visszaállítása a lemez, amikor másolhatja át az alap lap blob pillanatkép. Után a pillanatkép az alap lapra az előléptetett blob-, a pillanatkép marad, de a forrást a rendszer felülírja, amely is egyaránt olvasása és írása másolatot. Ez a cikk későbbi részében azt leírja a lemez előző verziójának visszaállítása annak pillanatképét lépéseket.

### <a name="implementing-full-snapshot-copy"></a>Végrehajtási teljes pillanatkép másolása

Hajtsa végre az alábbi lépéseket is alkalmazhat a teljes pillanatkép másolatának

-   Először is, hogy az alap blob- [Pillanatfelvétel Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) műveletének pillanatképét.
-   Ezután másolja a pillanatkép cél tárterület-fiókját [Másolás Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
-   Ismételje meg ezt a folyamatot, az alap blob biztonsági másolatait.

## <a name="incremental-snapshot-copy"></a>Növekményes pillanatkép másolása

Az új szolgáltatás a [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API készítsen biztonsági másolatot a lap BLOB vagy a lemezt a pillanatképek sok hatékonyabb módszerre biztosít. Az API-t a módosítások listájának között az alap blob- és a pillanatképek adja eredményül. Ez csökkenti a biztonságimásolat-fiókhoz használt tárterület mennyiségét. Az API lap BLOB prémium tárhely, valamint a szabványos tároló használatát támogatja. Ez az API használ, most készíthet gyorsabb és hatékony biztonsági megoldások az Azure VMs. Ez lesz elérhető a többi verziójához 2015-07-08 vagy újabb.

Növekményes pillanatkép-példány lehetővé teszi másolása tárterület-fiókból egy másik, közötti különbség

-   Alap blob és annak pillanatképét vagy
-   Bármely két elérhető információk, amelyek az alap blob

Megadva, az alábbi feltételek teljesülése esetén

- A blob Jan – 1 – 2016-os vagy újabb hozták létre.
- A blob nem felül [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) vagy [Másolás Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) két pillanatképek között.


**Megjegyzés**: Ez a funkció a prémium verzió és a szokásos Azure lap BLOB érhetők el.

Ha van egy egyéni biztonsági stratégia, amely egy adott időpontban érvényes, használja a pillanatképek egy tárterület-fiókból egy másik másol nagyon lassú lehet és rendelkezésre álló tárterület méretének sok fogyaszt. Másolja a teljes pillanatkép biztonságimásolat-fiókjába, hanem a biztonságimásolat-lap blob egymást követő pillanatképek közötti különbség is írhat. Ebben az esetben jelentősen csökken a másolás és a biztonsági másolatok tárolására szolgáló terület ideje.

### <a name="implementing-incremental-snapshot-copy"></a>Végrehajtási növekményes pillanatkép másolása

Hajtsa végre az alábbi lépéseket is alkalmazhat növekményes pillanatkép másolása

-   Pillanatkép az alap blob- [Pillanatfelvétel Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx)használatával.
-   Másolja a pillanatkép a cél biztonságimásolat-fiókra [Másolás Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Ez a biztonságimásolat-lap blob lesz. A biztonságimásolat-lap blob pillanatképet készíthet, és tárolni a biztonságimásolat-fiókjában.
-   Az alap blob-pillanatfelvétel Blob használja egy másik pillanatkép.
-   Ismerkedés az első és második egy adott időpontban érvényes [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx)használatával alap blob közötti különbség. Az új paraméter **prevsnapshot** segítségével adja meg a pillanatkép el szeretné küldeni a különbség a DateTime értéket. Ha a paraméter nem tartalmaz adatokat, a többi válasz tartalmazni fogja csak a lapok között cél pillanatkép és az előző pillanatkép törlése lapjait is beleértve megváltozott.
-   A módosítások alkalmazása a biztonságimásolat-lap blob [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) használata
-   Végül a biztonságimásolat-lap blob pillanatképet készíthet, és a tárolja a biztonságimásolat-fiók.

A következő szakaszban azt részletesen leírja további hogyan karbantarthatja növekményes pillanatkép másolással lemezt a biztonsági mentés

## <a name="scenario"></a>Eset

Ebben a részben, amely magában foglalja a virtuális gép lemezt pillanatképek használata egyéni biztonsági stratégia példa bemutatja azt.

Fontolja meg egy Lépésben sorozathoz Azure virtuális csatolt prémium tároló P30 lemezen. A *mypremiumdisk* nevű P30 lemez egy prémium tároló fiók *mypremiumaccount*nevű vannak tárolva. Egy szabványos tároló fiók *mybackupstdaccount* nevű *mypremiumdisk*biztonsági másolata tárolására szolgáló lesz. Azt szeretné megtartani *mypremiumdisk* pillanatképét 12 óránként.

Tárterület-fiók és a lemez létrehozásával kapcsolatos tudnivalók, olvassa el a [kapcsolatos Azure tároló fiókok](storage-create-storage-account.md).

Azure VMs mentésével kapcsolatos további tudnivalókért olvassa el a [terv Azure virtuális biztonsági mentést](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Növekményes pillanatképek használata lemezen biztonsági másolatait megőrzéséhez lépések

Az alábbiakban ismertetett lépések *mypremiumdisk* pillanatfelvételek, és a karbantartása: a biztonsági másolatok *mybackupstdaccount*. A biztonsági mentés *mybackupstdpageblob*néven normál lap blob lesz. A biztonságimásolat-lap blob mindig tükrözni fogja a *mypremiumdisk*az utolsó pillanatképként azonos állapotát.

1.  Első lépésként hozzon létre a biztonságimásolat-lap blob a prémium tároló lemez. Művelet, egy pillanatkép *mypremiumdisk* *mypremiumdisk_ss1*neve.
2.  Másolja a pillanatkép mybackupstdaccount *mybackupstdpageblob*nevű lap blob-ként.
3.  *Mybackupstdpageblob* *mybackupstdpageblob_ss1*, [Pillanatkép Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) használatával nevű pillanatképét készíthet, és tárolja azt a *mybackupstdaccount*.
4.  A biztonsági másolat lap alatt hozzon létre egy másik pillanatképét *mypremiumdisk*, *mypremiumdisk_ss2*szerint és tárolja azt a *mypremiumaccount*.
5.  Ismerkedés a növekményes módosításokat a két pillanatképek *mypremiumdisk_ss2* és *mypremiumdisk_ss1*, a *mypremiumdisk_ss2* [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) *mypremiumdisk_ss1*időbélyegzőjét **prevsnapshot** paraméter használata között. A biztonságimásolat-lap blob *mybackupstdpageblob* *mybackupstdaccount*növekményes módosítások írni. Ha a változás törölt tartománya van azok törölni kell a biztonságimásolat-lap blob a. Növekményes módosítások írni a biztonságimásolat-lap blob [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) használata
6.  A biztonságimásolat-lap blob *mybackupstdpageblob*, *mybackupstdpageblob_ss2*nevű pillanatkép. Törölje az előző pillanatkép *mypremiumdisk_ss1* prémium tárterület-fiókból.
7.  Biztonsági másolat ablakok ismételje meg a 4-6. Ezzel a módszerrel karbantarthatja biztonsági másolatait *mypremiumdisk* szabványos tárterület-fiókjában.

![Készítsen biztonsági másolatot a lemez növekményes pillanatképek használata](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Lemezen visszaállítani pillanatképek lépései

Az alábbiakban ismertetett lépések prémium lemez, *mypremiumdisk* egy korábbi pillanatfelvétel a biztonságimásolat-fiók *mybackupstdaccount*a visszaállítja.

1.  Melyik közülük a prémium lemez visszaállítani kívánt időben. Tegyük fel, hogy ez pillanatkép *mybackupstdpageblob_ss2*, a biztonságimásolat-fiók *mybackupstdaccount*tárolt.
2.  Mybackupstdaccount, a előléptetése a pillanatkép *mybackupstdpageblob_ss2* biztonságimásolat-alap lap új blob *mybackupstdpageblobrestored*szerint.
3.  A visszaállított biztonságimásolat-lap blob, *mybackupstdpageblobrestored_ss1*nevű pillanatkép.
4.  Másolja a visszaállított lap blob *mybackupstdpageblobrestored* *mybackupstdaccount* való *mypremiumaccount* , az új prémium lemez *mypremiumdiskrestored*.
5.  Pillanatkép *mypremiumdiskrestored*, *mypremiumdiskrestored_ss1* című későbbi növekményes biztonsági másolatok készítésére.
6.  Pont a DS adatsorokat virtuális a visszaállított *mypremiumdiskrestored* lemezre jelölőnégyzetből, és a virtuális a régi *mypremiumdisk* leválasztása.
7.  A megkezdéséhez biztonsági másolatot a visszaállított lemez *mypremiumdiskrestored*, a *mybackupstdpageblobrestored* a használják a biztonságimásolat-lap blob az előző szakaszban ismertetett.

![Egy adott időpontban érvényes lemez visszaállítása](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Következő lépések

További tudnivalók az egy adott időpontban érvényes blob létrehozásához, és használja az alábbi hivatkozásokat a virtuális biztonsági infrastruktúra megtervezése.

- [Blob-pillanatfelvétel létrehozásának](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [A virtuális biztonsági infrastruktúra megtervezése](../backup/backup-azure-vms-introduction.md)
