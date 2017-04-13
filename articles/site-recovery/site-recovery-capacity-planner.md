<properties
    pageTitle="Kapacitás védelmet virtuális gépeken futó és Azure webhely helyreállítási fizikai kiszolgálók megtervezése |} Microsoft Azure"
    description="Azure webhely helyreállítási koordináták a replikáció, feladatátvevő és helyreállítási virtuális gépeken futó és a helyszíni Azure vagy egy másodlagos helyszíni webhelyen található fizikai kiszolgálók." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Kapacitás védelmet virtuális gépeken futó és Azure webhely helyreállítási fizikai kiszolgálók megtervezése

Az Azure webhely helyreállítási kapacitás Csapattervező eszköz segítségével megállapítása a Hyper-V VMs, a VMware VMs és a Windows vagy Linux fizikai kiszolgálók az Azure-webhely helyreállításhoz kapacitás követelményei.


## <a name="overview"></a>– Áttekintés

A forrás-környezet és a feladatok elemzése és tudni, sávszélesség igényeinek, server szükséges erőforrások típusait a forráshely az és, hogy szüksége van a célhely az erőforrások (virtuális gépeken futó és tároló stb), a webhely helyreállítási kapacitásának Csapattervező használatával. 

Az eszköz megnyitása különböző módokban, néhány futtatását is lehetővé teszi:

- **Rövid tervezés**: az eszköz ebben a módban, hogy a hálózat és a kiszolgáló előrejelzések VMs, a lemezt, a tárhely és a módosítása ráta átlagos száma alapján Futtatás.
- **Részletes tervezés**: az eszköz futtatása ebben a módban, és adja meg az egyes terhelést virtuális szintjén részleteit. Virtuális kompatibilitási elemezni, és elérheti a hálózat és a kiszolgáló előrejelzések.

## <a name="before-you-start"></a>Előzetes teendők

Az eszköz futtatása előtt

1. A környezet, többek között a VMs, egy virtuális, egy lemezen tároló lemezt információkat gyűjthet.
2. A napi módosítása (tejeskanna) kamatlábát replikált adatok azonosításához. Ennek módja:

    - Ha a Hyper-V VMs esetén replikálása, majd töltse le a [Hyper-V kapacitás tervezési eszköz](https://www.microsoft.com/download/details.aspx?id=39057) a változás ráta eléréséhez. [Tudjon meg többet](site-recovery-capacity-planning-for-hyper-v-replication.md) : Ezzel az eszközzel. Azt javasoljuk, hogy ez az eszköz futtatása fölé egy hetet, átlagokat rögzíthet.
    - Ha VMware virtuális gépeken futó esetén replikálása, használja a [készülék tervezési vSphere kapacitás](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) a tejeskanna ráta megállapítása.
    - Ha a becsült manuálisan kell fizikai kiszolgálók esetén replikálása.

## <a name="run-the-quick-planner"></a>A gyors Csapattervező futtatása
1.  Töltse le és nyissa meg az [Azure webhely helyreállítási kapacitás Csapattervező](http://aka.ms/asr-capacity-planner-excel) eszközt. Ezért válassza a Szerkesztés engedélyezése, és amikor a rendszer kéri, a tartalom engedélyezése Makrófuttatás kell. 
2.  **Jelölje ki a Csapattervező típusát** a legördülő listában jelölje ki **A Csapattervező rövid** .

    ![Első lépések](./media/site-recovery-capacity-planner/getting-started.png)

3.  A **Csapattervező kapacitás** munkalap adja meg a szükséges információkat. Az alábbi képernyőképen pirossal bekarikázott összes mezőjét kell töltenie.

    - **Jelölje ki az Ön esetében** válassza **Az Azure a Hyper-V** vagy **VMware/fizikai az Azure**.
    - , **Átlag napi adatok módosítása ráta (%)** információk elhelyezése a gyűjtse össze a [Hyper-V kapacitás tervezési eszköz](site-recovery-capacity-planning-for-hyper-v-replication.md) vagy a [készülék tervezési vSphere kapacitása](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **A tömörítési** csak a tömörítési ajánlja fel, amikor esetében, VMware VMs vagy a fizikai kiszolgálók amelyek Azure vonatkozik. Azt becslése 30 vagy több, de szükség szerint a beállítás módosítása. A Hyper-V VMs kiegészítését Azure tömörítés szeretne egy harmadik fél készülék például Riverbed is használhatja. 
    -  Mennyi ideig kell megőrizni kópiák adja meg **Adatmegőrzési bemeneti adatok alapján** . Ha meg van VMware végre vagy a szövegbeviteli értékét fizikai kiszolgálók nap. Ha éppen replikálása a Hyper-V szolgáltatást úgy beállítani, hogy mennyi órán belül.
    -  A **kell végrehajtania, mely kezdeti replikációs virtuális gépeken futó, a köteg órák számát** és **a virtuális gépeken futó, egy kezdeti replikációs köteg** szövegbeviteli kezdeti replikációs követelményeket kiszámításához használt beállítások.  Ha telepítve van a webhely helyreállítási, fel kell tölteni a kezdeti teljes adatkészletet. 

    ![Bemeneti adatok alapján](./media/site-recovery-capacity-planner/inputs.png)

2.  Miután már az értékeket a forrás környezetben, megjelenített kimenete:

    - **Delta replikációs szükséges sávszélességet** (MB/sec). A delta replikáció hálózati sávszélesség az átlagos napi módosítása adatsebesség számolja ki.
    - **Az első replikációs szükséges sávszélességet** (MB/sec). A kezdeti replikációs értékek helyezi el a kezdeti replikáció hálózati sávszélesség számolja ki. 
    - **Tárhely szükséges (GB)** rendszer szükséges Azure tárterületkorlátot.
    - **Szabványos tároló fiókok teljes IOPS** számítása 8 K IOPS egység méretét a szabványos tárterületkorlátot fiókok alapján történik.  A forrás VMs lemez és a napi adatai alapján fontos Csapattervező kiszámítása a számot a sebességének módosítása. A részletes Csapattervező kiszámítása a szám, amely normál Azure VMs és adatok vannak megfeleltetve VMs száma alapján az adott VMs a sebességének módosítása. 
    - **Szabványos tároló számlák száma** itt védelme a VMs szükséges szabványos tároló fiókok száma. Ne feledje, hogy egy szabványos tárterület-fiókot is tartsa lenyomva az ujját legfeljebb 20000 IOPS át egy szabványos tárolóban lévő minden VMs legfeljebb 500 IOPS egy lemezen támogatott. 
    - **Megfelelő számot blob lemezt a** lemezt Azure tároló létrehozott számát adja vissza.
    - **Prémium tároló számlák száma szükséges** itt prémium verzió tároló használata szükséges a VMs védelme száma. Figyelje meg, hogy a virtuális magas IOPS (nagyobb, mint 20000) a forrás egy prémium tárterület-fiók szükséges. A prémium tároló fiók legfeljebb 80000 IOPS tárolhat.
    - **Teljes IOPS prémium tárolón** számítása 256 K IOPS egység méretét, a teljes prémium tároló fiókok alapján történik.  A forrás VMs lemez és a napi adatai alapján fontos Csapattervező kiszámítása a számot a sebességének módosítása. A részletes Csapattervező kiszámítása a szám, amely prémium Azure virtuális (DS és GS adatsor) és az adatok vannak megfeleltetve VMs száma alapján az adott VMs a sebességének módosítása. 
    - **Szükséges konfigurációs kiszolgálóihoz számát** mutatja, hány konfigurációs kiszolgálóihoz szükségesek a telepítéshez (1)
    - **További folyamat kiszolgálók száma szükséges** mutatja, hogy mellett a folyamat kiszolgáló alapértelmezés szerint a konfigurációs kiszolgálón beállított szükség-e további folyamat kiszolgálók.
    - **100 % -os további tárterületet, kattintson a forrás** jeleníti meg a szükséges-e a forráshely további tárterületet.
            
    ![Kimenet](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>A részletes Csapattervező futtatása


1.  Töltse le és nyissa meg az [Azure webhely helyreállítási kapacitásának Csapattervező](http://aka.ms/asr-capacity-planner-excel) eszközt. Ezért válassza a Szerkesztés engedélyezése, és amikor a rendszer kéri, a tartalom engedélyezése Makrófuttatás kell. 
2.  **Jelölje ki a Csapattervező típusát** a legördülő listában jelölje ki **A Csapattervező részletes** .

    ![Első lépések](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  A **Terhelést a minősítési** munkalap adja meg a szükséges információkat. Ki kell töltenie a megjelölt mezőket.

    - **Processzormagok** adja meg a forrás kiszolgálón magmintákat száma.
    - Adja meg a forráskiszolgálóval RAM méretének **memóriafoglalást MB-ban** . 
    - A **Szám a NIC** hálózati adaptereken számának megadása a forrás kiszolgálón. 
    -  A **teljes tárterület (GB)-ban** adja meg a virtuális tároló összesített méretét. A példában ha a forráskiszolgálóval 500 GB 3 lemezt, majd a teljes tárterület méretének 1500 GB.
    -  **Csatolt lemez számát** adja meg a forráskiszolgálóval a lemez száma.
    -  Adja meg az átlag kihasználtsági **lemez kapacitás kihasználtsági** .
    -  **Napi módosítása ráta (%)** adja meg a napi adatok módosítása a forráskiszolgálóval mértéke.
    -  **Azure-leképezés** méretű vagy Azure virtuális méretét, hogy a megfeleltetni kívánt adja meg. Ha nem szeretne manuális ehhez kattintson a**IaaS VMs számítja ki**. Figyelje meg, hogy ha a szövegbeviteli kézi beállítás, majd kattintson az IaaS VMs számítja ki a kézi beállítás előfordulhat, hogy frissíthetők, mert a számítási folyamat automatikusan azonosítja a legmegfelelőbb Azure virtuális méretét.

    ![A minősítési terhelést](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Ha rákattint **IaaS VMs kiszámítása** az alábbi, mit jelent:

    - Ellenőrzi a kötelező bemeneti adatok alapján.
    - IOPS számítja ki, és a javításukhoz javasolt, a legjobb Azure virtuális aize minden VMs, hogy jogosult az Azure való replikáció egyezés. Ha az Azure virtuális megfelelő méretű nem az általuk észlelt kibocsátott hibát jelez. Példa számát lemezek a csatolt 65 hiba esetén problémák óta a legnagyobb mérete Azure virtuális 64.
    - Egy tárterület-fiókot az Azure virtuális használható javasolja.
    - Szabványos tárhely és a terhelést a szükséges prémium tároló fiókok teljes számát számítja ki. Görgessen le a jobb oldali Azure tárolási típusa és a tárterület-fiók egy forráskiszolgáló használható megtekintése
    - Befejeződik, és a többi szükséges tárolási típusa (normál vagy magasabb szintű) egy virtuális, és hozza létre csatolt hozzárendelve a tábla rendezése. Az összes VMs (van virtuális minősített?) az Azure-oszlopban biztonsági követelményeknek megfelelő Igen mutatja. Ha egy virtuális legfeljebb nem biztonsági Azure hiba jelenik meg.

Oszlopok ε OI kimeneti, és adja meg mindegyik virtuális adatokat.

![A minősítési terhelést](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Példa
Az eszköz szerepel példaként, az értékekkel, a táblázatban, hat VMs számítja ki, és rendel a legmegfelelőbb Azure virtuális és az Azure tárhelyre.

![A minősítési terhelést](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- A példa kimeneti vegye figyelembe az alábbiakat:
    
    - Az első oszlop VMs, lemez és tejeskanna érvényességi oszlop.
    - Az öt VMs két szabványos tároló fiókok és egy prémium tárterület-fiók van szükség. 
    -  Mivel az egy vagy több lemezen legfeljebb 1 TB VM3 védelem nem jogosultak.
    -  VM1 VM2 használható és az első szabványos tárterület-fiók
    -  VM4 használhatja a második szabványos tárterület-fiókot.
    -  VM5 és VM6 szükség prémium tároló fiókra, és mindkét használata egy adott fiókhoz.

    >[AZURE.NOTE]  A normál és premium tároló IOPS virtuális szinten, és nem lemez szintjén kerülnek kiszámításra. A szokásos virtuális gép egy lemezen legfeljebb 500 IOPS képes kezelni. Ha egy lemezről IOPS nagyobb, mint 500 prémium tároló lesz szüksége. Azonban ha IOPS lemez több mint 500, de az összes virtuális lemezt IOPS korlátok támogatási szabványos Azure virtuális (virtuális méret, hozza létre, a kártyák, Processzor, memória száma) kattintson a Csapattervező választja ki egy szabványos virtuális, és nem a Tartományi vagy GS adatsort. Kézi frissítése a hozzárendelés Azure méret cella virtuális megfelelő DS vagy GS adatsorozatot tartalmazó kell.

5. Után minden részlet helyen vannak, kattintson az **adatok küldése a Csapattervező eszköz** nyissa meg a **Beosztását Csapattervező** Munkaterhelésekből kiemelt kattintva jelenítse meg, hogy legyenek védelem használatára jogosult-e.


### <a name="submit-data-in-the-capacity-planner"></a>A kapacitásának Planner az adatok elküldése

1.  A **Csapattervező kapacitás** munkalap megnyitásakor töltve a beállításoknak alapján. A word "Terhelést" jelzi, hogy a bemeneti **Terhelést minősítési** munkalap **Infra bemenetben forrás** cellában jelenik meg. 
2.  Ha szeretné módosítani kell a **Terhelést a minősítési** munkalap módosíthatja, és kattintson ismét az adatok küldése a Csapattervező eszközre.  

    ![A Csapattervező kapacitás](./media/site-recovery-capacity-planner/capacity-planner.png)


