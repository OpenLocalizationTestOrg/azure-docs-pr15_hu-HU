<properties
    pageTitle="A Hyper-V replikációs az Azure-webhely helyreállításhoz |} Microsoft Azure"
    description="Ez a cikk használatával, amelyek segítségével sikeresen technikai fogalmak telepítése, beállítása és kezelése Azure webhely helyreállítási megértéséhez."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>A Hyper-V replikáció Azure webhely helyreállítás

Ez a cikk ismerteti, amelyek segítséget nyújtanak az sikeresen beállítása és kezelése a Hyper-V webhelyek vagy Azure védelem System Center virtuális gép Manager (VMM) webhely Azure webhely helyreállítási használatával technikai koncepciót is bemutat.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>A helyszíni és Azure közötti replikáció forrás környezet beállítása

A helyszíni és Azure között vészhelyreállítás beállításának részeként feltétlenül töltse le és telepítse az Azure webhely helyreállítási szolgáltatója a VMM kiszolgálón. Telepítse az Azure helyreállítási szolgáltatások ügynök a Hyper-V állomásokon.

![A helyszíni és Azure közötti replikáció VMM webhely telepítésének](media/site-recovery-understanding-site-to-azure-protection/image00.png)

A forrás környezet a Hyper-V felügyelt infrastruktúra beállításának hasonlít egy VMM felügyelt infrastruktúra a forrás környezet beállításához. Az egyetlen különbség, hogy a szolgáltatót, és a ügynök telepítve van a Hyper-V host magát. A VMM környezetben a szolgáltató telepítve van a VMM kiszolgálón, és a ügynök telepítve van a Hyper-V állomásokon (abban az esetben Azure való replikáció).

## <a name="workflows"></a>Munkafolyamatok

### <a name="enable-protection"></a>Védelem bekapcsolása
Virtuális gép védelme az Azure portálon vagy a helyszíni, után egy webhely helyreállítási feladat nevű **védelem** indítja el. Figyelheti a **feladatok** lap.

!["A védelem bekapcsolása" feladatra a listában, a feladatok](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

A **védelem** feladat ellenőrzi a Előfeltételek a [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) metódus előtt. Ez a módszer Azure való replikáció védelem konfigurált bemenetben használatával hoz létre.

A **védelem** feladat a helyszíni a kezdeti replikáció indítása a [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) metódus. Ez a módszer küld Azure virtuális lemezt a virtuális gépen.

![A "Védelem bekapcsolása" feladat részletei](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>A virtuális gépen védelem véglegesítése
[A Hyper-V virtuális pillanatkép](https://technet.microsoft.com/library/dd560637.aspx) kezdeti replikációs induljanak venni. Virtuális merevlemezeken olyan feldolgozott egyesével, mindaddig, amíg az összes lemez Azure van feltöltve. A szokásos módon ekkor egy ideig, ha végzett, a lemez méretét és a sávszélesség alapján. Optimalizálhatja a hálózati használatát, megtudhatja, [hogy miként kezelheti a helyszíni az Azure védelem hálózati sávszélesség-használat](https://support.microsoft.com/kb/3056159).

A kezdeti replikáció befejezése után a **Véglegesítés védelem a virtuális gépen** feladat konfigurálása a hálózati és utáni replikációs beállításai Miközben folyamatban van a kezdeti replikáció:

- A lemez összes módosítás követik nyomon. 
- További lemezterület pillanatkép és a Hyper-V replika Log (HRL) fájlok elfogyasztott mennyiség megjelenítésére.

A kezdeti replikációs kiegészítésének a Hyper-V virtuális pillanatkép törlődik. A törlés eredményez adatok változtatások egyesítése a szülő-lemezre kezdeti replikáció után.

!["A virtuális gépen védelem véglegesítése" feladatra a listában, a feladatok](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta replikációs
A Hyper-V replika replikációs nyilvántartása, a Hyper-V replika replikációs motor nyomon követi a módosításokat egy virtuális merevlemezre, a Hyper-V replika (*.hrl) naplófájlok részét képező. HRL fájlok társított lemezt ugyanabban a mappában.

Minden lemezre, hogy be van állítva a replikáció az esetlegesen társított HRL fájlba. A napló küldi a vevőkód tároló kezdeti replikációs befejeződése után. Naplózási az Azure úton van, ha az elsődleges változásai ugyanabban a mappában egy másik naplófájl követi.

Kezdeti replikációs vagy a delta replikációs során virtuális replikációs állapota a virtuális nézetben [Monitor replikációs állapot virtuális gép](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)említett figyelheti meg.  

### <a name="resynchronization"></a>Újraszinkronizálás
Ha mind delta replikáció megszakad, és teljes körű első replikációs költséges értelmez a hálózat sávszélessége vagy idő virtuális gép újraszinkronizálás van megjelölve. Például ha HRL fájl méretét és Üzenetcsoportok 50 %-át a teljes lemez méretét, a virtuális gépre van megjelölve újraszinkronizálás. Újraszinkronizálás kis méretűre állítása a forrás- és célwebhelyek virtuális gép lemezt ellenőrző összegeket számítások, és csak a differenciál elküldheti a hálózaton keresztül küldött adatok mennyiségét.

Újraszinkronizálás befejezése után normál delta replikációs kell önéletrajz. Ha egy hálózati üzemszünetek vagy egy másik üzemszünetek fordul elő újraszinkronizálás folytathatja.

Alapértelmezés szerint automatikusan ütemezett újraszinkronizálás van beállítva kívüli munkaidő fordulhat elő. Ha a virtuális gép manuálisan kell újraszinkronizálja, jelölje ki a virtuális gép a portálon, és kattintson a **szinkronizálnia**.

![Kézi Újraszinkronizálás](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Újraszinkronizálás algoritmust rögzített blokk chunking hol oszthatók forrás- és célwebhelyek fájlok rögzített szövegadattömb. Az egyes adattömb ellenőrző összegeket létrehozott és összehasonlításra megállapíthatja, hogy melyik blokkolja a forrásból származó kell a cél vonatkozni.

### <a name="retry-logic"></a>A folyamat működését újrapróbálkozási
Nincs beépített újrapróbálkozási logika replikációs hibákat. Ez a módszer két kategóriába sorolhatók:

| Kategória                  | Felhasználási területei                                    |
|---------------------------|----------------------------------------------|
| Nem helyreállítható hiba     | Nincs ismétlés pró. Virtuális gép replikációs állapota **kritikus**, és a rendszergazda beavatkozás szükség. Példák: <ul><li>Hibás virtuális lánc</li><li>A replika virtuális gép érvénytelen állapotú</li><li>Hálózati hitelesítési hiba</li><li>Hitelesítési hiba</li><li>Virtuális gép, hogy nem található a Hyper-V önálló kiszolgáló esetén</li></ul>|
| A Helyrehozható hiba         | Újrapróbálkozások minden replikációs időköz, használja az exponenciális vissza kikapcsolási, amely növeli a kísérletek időköze (1, 2, 4, 8, 10 perc) elsőre elején fordul elő. Ha hiba nem szűnik meg, próbálja meg újra a 30 percenként. Példák: <ul><li>Hálózati hiba</li><li>Alacsony szabad lemezterület</li><li>Alacsony memóriahasználat feltétel</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>A Hyper-V virtuális gép védelme és helyreállítási életciklus

![A Hyper-V virtuális gép védelme és helyreállítási életciklus](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Más mutató hivatkozások

- [Figyelésére és VMware, VMM, a Hyper-V és fizikai webhelyek védelmet – problémamegoldás](./site-recovery-monitoring-and-troubleshooting.md)
- [A Microsoft Support nyúló](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Azure webhely helyreállítási kapcsolatos gyakori hibákra, és a megoldások](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
