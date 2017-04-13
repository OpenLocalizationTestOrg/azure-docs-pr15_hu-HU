<properties
    pageTitle="Bizonyos VMware virtuális gépeken futó és fizikai kiszolgálók az Azure webhely helyreállítási (régi) az Azure |} Microsoft Azure" 
    description="Megtudhatja, hogy miként való replikáció helyszíni VMs és Azure-kiszolgálók fizikai Windows vagy Linux Azure-webhely helyreállítás segítségével a klasszikus portál régi környezetben." 
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Bizonyos VMware virtuális gépeken futó és fizikai kiszolgálók az Azure az Azure-webhely helyreállításhoz a klasszikus portálon (régi)

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmware-to-azure.md)
- [Klasszikus portál](site-recovery-vmware-to-azure-classic.md)
- [Klasszikus Portal (régi)](site-recovery-vmware-to-azure-classic-legacy.md)


Üdvözli a Azure webhely helyreállítási! Ez a cikk ismerteti a helyszíni VMware virtuális gépeken futó vagy a Windows vagy Linux fizikai kiszolgálók kiegészítését az Azure Azure webhely helyreállítás segítségével a klasszikus portál régi telepítés.

## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor.

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)


>[AZURE.WARNING] Ez a cikk a **régi útmutatást**tartalmaz. Nem használja az új telepítések. Ehelyett a webhely helyreállítási bevezetését tervezi a Azure portál, vagy [használja az alábbi útmutatást](site-recovery-vmware-to-azure-classic.md) a bővített funkciójú telepítésének konfigurálásához az klasszikus portálon [kövesse az alábbi lépéseket](site-recovery-vmware-to-azure.md) . Ha korábban már telepítette a jelen cikkben ismertetett módszerrel, azt javasoljuk, a továbbfejlesztett példányban a klasszikus portálon áttelepítése.


## <a name="migrate-to-the-enhanced-deployment"></a>A továbbfejlesztett példányban áttelepítése

Ez a szakasz csak akkor jelentősége, ha korábban már telepítette a webhely helyreállítási leírtak Ez a cikk.

A meglévő üzembe kell funkció vagy beállítás

1. A helyszíni webhely az új webhely helyreállítási összetevők telepítése.
2. Az új fiókkonfigurációs kiszolgáló automatikus felderítését VMware VMs hitelesítő adatok konfigurálhatja.
3. Fedezze fel az új konfigurációs kiszolgálóval VMware kiszolgálókhoz.
3. Az új konfigurációs kiszolgálója védelem új csoport létrehozása


Mielőtt elkezdené:

- Azt javasoljuk, hogy az áttelepítési karbantartási ablak beállítása.
- A **Gép áttelepítése** beállítás csak akkor, ha a meglévő védelem csoportok egy korábbi telepítése során készített van érhető el.
- Az áttelepítési lépések végrehajtása után is tovább tarthat 15 percet vagy hosszú, frissítse a hitelesítő adatait, és a Fedezze fel, és frissítse a virtuális gépeken futó, így hozzáadhatja őket egy védelem csoport. Frissítheti manuálisan várakozás helyett. 

Áttelepítése az alábbiak szerint:

1. További információ: [a klasszikus portálon továbbfejlesztett telepítési](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Tekintse át a bővített funkciójú [architektúrája](site-recovery-vmware-to-azure-classic.md#scenario-architecture)és [előfeltételekről](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. A mobilitás szolgáltatás eltávolítása a gép esetén jelenleg replikálása. A szolgáltatás új verziója telepíti a gépeken hozzáadásakor őket az új védelem csoporthoz.
3. Töltse le a [tárolóból elemre regisztrációs billentyű](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) és [a egységesített beállítási varázslót](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) a fiókkonfigurációs kiszolgáló, a folyamat server és a fő tároló kiszolgáló-összetevők telepítése. További információ a [kapacitás tervezés](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. A [hitelesítő adatok](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) adott webhely helyreállítási VMware-kiszolgálót, automatikusan észleljék az VMware VMs eléréséhez használható. Tudjon meg többet a [szükséges engedélyekkel](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Adja hozzá [vCenter kiszolgálók vagy vSphere tárolja](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Kiszolgálók megjelenjen a webhely helyreállítási portálon további 15 percet is eltarthat.
6. Hozzon létre egy [Új védelem csoportot](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Eltarthat legfeljebb 15 percet, hogy a portálon frissítheti, hogy a virtuális gépeken futó megjelenése és jelennek meg. Ha azt szeretné, hogy csak a kezelés kiszolgálónév kiemelhet (ne kattintson rá) > **frissítés**.
7. Kattintson az új védelem csoport **Gépek áttelepítése**.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. **Válassza a gép** jelölje ki a védelem csoportot, az áttelepítendő, és a gép szeretné áttelepíteni.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. **Célalkalmazás beállításainak konfigurálása** adja meg, hogy szeretné-e az azonos beállítások használata az összes gépekhez, és válassza a folyamat server és a Azure tárterület-fiókot. Ha nem rendelkezik egy külön folyamatot ez lesz az a konfigurációs kiszolgáló IP-címét.


    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Tárterület-fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

10. A **Fiókok adja meg**jelölje ki a fiókot, a folyamat kiszolgálóhoz a gép az új verzió a mobilitás szolgáltatás leküldéses hoz létre.

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Webhely helyreállítási replikált adatok áttelepítése az Azure tárterület-fiókból, amely a megadott. Ha szükséges, így újból felhasználhatja a tárterület-fiókkal telepítette a régi példányban.
12. A feladat befejezése után a program automatikusan szinkronizálja a virtuális gépeken futó. Szinkronizálás befejeződése után törölheti a virtuális gépeken futó a régi védelem csoportból.
13. Miután áttelepítette az összes gépek törölheti a régi védelem csoport.
14. Adja meg a gépek, és az Azure hálózat beállításainak feladatátvevő tulajdonságainak szinkronizálás befejezése után ne felejtse el.
15. Meglévő helyreállítási csomagok esetén áttelepítheti őket a bővített funkciójú környezet, amelyben a **Helyreállítási terv áttelepítése** lehetőséget. Csak akkor tegye meg, miután áttelepítette az összes védett gépek. 

    ![Fiók hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Amikor befejezte az áttelepítési a [bővített funkciójú cikk](site-recovery-vmware-to-azure-classic.md)folytatásához. A régi cikk további részében többé nem megfelelő, és nem szükséges többet az informatikai ** leírt lépéseket.




## <a name="what-do-i-need"></a>Mire van szükségem?

A ábrán a telepítés összetevők.

![Új tárolóból elemre](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Az alábbiakban mire lesz szüksége:

**Összetevő** | **Telepítési** | **Részletek**
--- | --- | ---
**Konfigurációs kiszolgálója** | Az Azure szabványos A3 virtuális gép ugyanabban az előfizetésben, webhely helyreállítási. | A kiszolgáló koordinálja védett gépek, a folyamat server és a fő célalkalmazás kiszolgálók Azure-ban közötti kommunikációt. Létrehozza replikációs és az Azure-ban koordináták helyreállítási feladatátvevő bekövetkezésekor.
**Fő tároló kiszolgáló** | Az Azure virtuális gép – vagy a Windows Server 2012 R2 gyűjteménye (a védelme a Windows gépek) vagy Linux kiszolgáló alapján a Windows server egy OpenLogic CentOS 6.6 gyűjteménye (a Linux gépek védelme) alapján.<br/><br/> Három méretezési lehetőségek állnak rendelkezésre – Standard A4, a normál D14 és a szokásos DS4.<br/><br/> A kiszolgáló a konfigurációs kiszolgálójával azonos Azure hálózathoz kapcsolódik.<br/><br/> A webhely helyreállítási portál beállítása | Kapott és megőrzi a védett gépek használatával létrehozott blob-tárolóhoz Azure tároló fiókban csatolt VHD replikált adatait.<br/><br/> Jelölje ki a szokásos DS4 kifejezetten a védelmet igénylő egységes nagy teljesítmény és a kis késés prémium tárterület-fiókkal munkaterhelésekből konfigurálása.
**Folyamatábra-kiszolgáló** | Egy helyszíni virtuális vagy a fizikai kiszolgálón futó Windows Server 2012 R2<br/><br/> Javasoljuk, hogy a ugyanabba a hálózatba, és a szegmenst, a védelemmel ellátni kívánt gépek elhelyezett, de azt futtatását is lehetővé teszi a másik hálózathoz mindaddig, amíg védett gépek van rá 3. a hálózati láthatóság.<br/><br/> Állítsa be, és regisztrálhatja a konfigurációs kiszolgálóhoz, a webhely helyreállítási portálon. | Védett gépek replikációs adatok a helyszíni folyamat kiszolgáló küldje el. A kapott merevlemez-alapú gyorsítótárat a gyorsítótár-replikáció adatot tartalmaz. Műveletek számos adatokon hajt végre.<br/><br/> Adatok gyorsítótárba, tömörítése és titkosítja a megjelenítheti a fő tároló kiszolgáló elküldése előtt optimalizálja.<br/><br/> A mobilitás szolgáltatás telepítése leküldéses kezeli.<br/><br/> Az automatikus felderítését VMware virtuális gépeken futó hajtja végre.
**A helyszíni gépek** | A helyszíni VMware virtuális gépeken futó, vagy a fizikai kiszolgálón a Windows vagy Linux rendszerhez. | Egy vagy több gépekre vonatkozó replikációs beállításainak konfigurálása Akkor is sikertelen gyakrabban, vagy egy különálló gépi keresztül több számítógépen, amely a helyreállítási tervbe közös gyűjtése. 
**Mobilitás szolgáltatás** | Telepítve van egyes virtuális gép vagy a védelemmel ellátni kívánt fizikai kiszolgálón<br/><br/> Manuálisan telepíteni vagy tolni, és automatikusan telepíti a folyamat kiszolgáló engedélyezheti a replikáció géphez. | A mobilitás szolgáltatás adatokat küld kezdeti replikáció (újraszinkronizálási) során a a folyamat kiszolgáló. Után a gép védett állapotban van, (miután újraszinkronizálási befejeződött) a mobilitás szolgáltatás rögzíti a lemezen a memóriában ír, és küldjön nekik a folyamat kiszolgálóra. A Windows-kiszolgálók Applicationconsistency érhető el VSS használatával
**Azure webhely helyreállítási tárolóból elemre** | Hozzon létre egy webhely helyreállítási tárolóból elemre az Azure előfizetéssel rendelkező és kiszolgáló regisztrálása a tárolóból elemre. | A tárolóból elemre koordináták és orchestrates adatok replikációs, feladatátvevő és helyreállítási a helyszíni webhely és a Azure között.
**Replikációs eljárás** | **Az interneten keresztül**– az interneten keresztül csatorna használja az SSL/TLS biztonságos Azure védett helyszíni kiszolgálókról kommunikál és ismétlések adatokat. Az alapértelmezett beállítás.<br/><br/> **Virtuális Magánhálózati/készült ExpressRoute**– kommunikál ismétlések adatok és a helyszíni kiszolgálók és Azure között a virtuális Magánhálózati kapcsolat fölé. Állítsa be a webhely virtuális Magánhálózaton vagy egy készült ExpressRoute kapcsolat a helyszíni webhely és a Azure hálózati között kell.<br/><br/> Meg kell adja meg, hogyan való replikáció webhely helyreállítási a telepítés során. A szerkezet után úgy van beállítva, a meglévő gépek replikációs érintő nélkül nem módosítható. | Sem a lehetőséghez nyissa meg bármelyik hálózati bejövő portokat védett gépeken. Az összes hálózati forgalmat a helyszíni webhely kezdeményezni. 

## <a name="capacity-planning"></a>Kapacitás tervezése

A fő részből, fontolja meg kell:

- **Forrás környezetben**– a VMware infrastruktúra, adatforrás gép beállításai és követelményeknek.
- **Tartalomfeldolgozó**– a folyamat server, a fiókkonfigurációs kiszolgáló és a fő tároló kiszolgáló 

### <a name="considerations-for-the-source-environment"></a>A forrás-környezet megfontolandó szempontok

- **Maximális méretét**– lehet csatolni virtuális gép lemez aktuális maximális mérete az 1 TB. Maximális mérete az adatforrás lemezen replikálhatók, így is az 1 TB korlátozott.
- **Maximális méret egy adatforrás**– egy adatforrás egyetlen számítógépre maximális hossza 31 TB (31 lemezt) és a fő tároló kiszolgáló kiépítve egy D14-példányt tartson. 
- **Többféle forrásból fő célalkalmazás kiszolgálónként**– több forrásból gépek védhetők egyetlen fő célalkalmazás-kiszolgálóval. Azonban egy adatforrás egyetlen számítógépre védett több fő cél kiszolgálóin, mert bizonyos lemezt, mint egy virtuális, amely a lemez méretének jön létre Azure blob-tárolóhoz nem, és a fő cél kiszolgálóhoz kapcsolt adatok lemezen.  
- **Napi maximális módosítása forrás díjat**– tényező három kell figyelembe venni, amikor a forrás ajánlott módosítása díjat kell. A target alapú szempontokat mérlegelve két IOPS a cél lemezen az egyes műveletekhez kattintson a forrás van szükség. Ennek oka az, a do not use-a régi adatok olvasási és írási az új adatok történik. 
    - **Napi a folyamat kiszolgáló által támogatott sebességének módosítása**– egy adatforrás számítógépre több folyamat kiszolgáló nem időtartamát. Egy egyetlen folyamat kiszolgálón használható legfeljebb 1 TB-nyi napi módosítása ráta. 1 TB így, a maximális napi adatai támogatott adatforrás géphez sebességének módosítása. 
    - **Maximális átviteli támogatják a do not use**-maximális tejeskanna egy adatforrás lemezen nem lehet nagyobb, mint 144 GB/nap (8 K írási méret). A című táblázatban az átviteli és a cél IOPs fő cél szakaszában a különféle méretű írja be. Ennek a számnak kell osztásra két mivel minden egyes adatforrás IOP a cél lemezen 2 IOPS hoz létre. További információ: [Azure méretezhetőség és a teljesítmény cél](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) , a cél prémium verzió tároló használata beállításakor.
    - **A tároló fiók által támogatott maximális átviteli**– forrás több tárterület-fiók nem időtartamát. Adni, hogy a tárterület-fiók szükséges legfeljebb 20 000 kérelmek másodpercenként és, hogy minden egyes adatforrás IOP hoz létre a fő cél kiszolgálón 2 IOPS, javasoljuk, a forrás IOPS keresztben 10 000 megtartani. További információ: [Azure méretezhetőség és a teljesítmény célok](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) , konfigurálása a forrás prémium verzió tároló használata esetén.

### <a name="considerations-for-component-servers"></a>Tartalomfeldolgozó megfontolandó szempontok

Tartalomjegyzék 1 áttekintheti a konfigurációs és a fő cél kiszolgálók virtuális gép méretű.

**Összetevő** | **A telepített Azure-példányok** | **Magmintákat** | **A memória** | **Max lemez** | **Lemez mérete**
--- | --- | --- | --- | --- | ---
Konfigurációs kiszolgálója | Szabványos A3 | 4 | 7 GB | 8 | 1023 GB
Fő tároló kiszolgáló | Szabványos a4 csomag | 8 | 14 GB | 16 | 1023 GB
 | Szabványos D14 | 16 | 112 GB | 32 | 1023 GB
 | Szabványos DS4 | 8 | 28 GB | 16 | 1023 GB

**Tartalomjegyzék 1**

#### <a name="process-server-considerations"></a>A folyamat kiszolgáló kapcsolatos szempontok

Általában folyamat kiszolgáló egyik függ, hogy a napi módosítása ráta át az összes védett munkaterhelésekből.


- Elegendő számítási például beágyazott tömörítés és titkosítás feladatok elvégzéséhez szükséges.
- A folyamat kiszolgáló használja a lemezgyorsítótár alapján. Ellenőrizze, hogy a gyorsítótár ajánlott és a szabad terület átviteli érhető el az adatok módosítását, hálózati szűk vagy üzemszünetek tárolt megkönnyítése érdekében. 
- Győződjön meg róla elegendő a sávszélesség, így a folyamat kiszolgáló is feltölthet a fő cél-kiszolgálót, folytonos adatot védelmet nyújtanak az adatok. 

Táblázat 2 biztosít a folyamat kiszolgáló irányelveihez összefoglalását.

**Adatok sebességének módosítása** | **PROCESSZOR** | **A memória** | **Lemezen gyorsítótárméret**| **Gyorsítótár lemez átviteli** | **A sávszélesség bejövő adatok/kilépési**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 magmintákat @ 2,5 GHz-es) | 4 GB | 600 GB | 7 – 10 MB-os / szekundum | 30 MB/21 MB
300-600 GB | 8 vCPUs (2 sockets * 4 magmintákat @ 2,5 GHz-es) | 6 GB | 600 GB | 11-15 MB / szekundum | 60 MB/42 MB
1 TB 600 GB | 12 vCPUs (2 sockets * 6 magmintákat @ 2,5 GHz-es) | 8 GB | 600 GB | 16-20 MB / szekundum | 100 MB/70 MB
> 1 TB | Egy másik folyamat server telepítése | | | | 

**Táblázat 2**

Ha: 

- Bejövő adatok letöltési sávszélesség (a forrás- és kiszolgáló közötti intranetes).
- Kilépési a feltöltést a sávszélesség (a folyamat kiszolgáló és a fő tároló kiszolgáló közötti internet). Kilépési számok átlagos 30 %-os folyamat kiszolgálón tömörítés feltételezik.
- Gyorsítótár lemez egy külön OS minimális 128 GB ajánlott az összes kiszolgálón a folyamat.
- A gyorsítótár lemez átviteli a következő tároló használta mérés: 8 Társítások meghajtók 10 KB RPM RAID 10 beállításokkal.

#### <a name="configuration-server-considerations"></a>Konfigurációs kiszolgáló kapcsolatos szempontok

Minden egyes konfigurációs kiszolgálója támogatniuk kell a 3-4-es kötet legfeljebb 100 forrás gépek. Ha a telepítő nagyobb, azt javasoljuk, egy másik konfigurációs kiszolgálója rendszerbe. Tartalomjegyzék 1 talál a fiókkonfigurációs kiszolgáló alapértelmezett virtuális gép tulajdonságait. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Fő tároló kiszolgáló és a tárhely fiók kapcsolatos szempontok

Minden fő tároló kiszolgáló tárolására-OS lemez adatmegőrzési kötet és adatok lemezt tartalmazza. Az adatmegőrzési meghajtó a lemez változik lapjának megtartja az adatmegőrzési ablak, a webhely helyreállítási portálon definiált időtartamára.  Olvassa el a fő tároló kiszolgáló virtuális gép tulajdonságainak az tartalomjegyzék 1. Táblázat 3 jeleníti meg, hogyan A4 lemezt használhatók.

**Példány** | **Operációs rendszer lemez** | **Adatmegőrzés** | **Adatok lemez**
--- | --- | --- | ---
 | | **Adatmegőrzés** | **Adatok lemez**
Szabványos a4 csomag | 1 lemezre (1 * 1023 GB) | 1 lemezre (1 * 1023 GB) | 15 lemez (15 * 1023 GB)
Szabványos D14 |  1 lemezre (1 * 1023 GB) | 1 lemezre (1 * 1023 GB) | 31 lemez (15 * 1023 GB)
Szabványos DS4 |  1 lemezre (1 * 1023 GB) | 1 lemezre (1 * 1023 GB) | 15 lemez (15 * 1023 GB)

**Táblázat 3**

A fő tároló kiszolgáló megtervezése kapacitás függ:

- Azure tároló teljesítmény és korlátai
    - Maximális száma erősen lemezt kihasználtság egy szabványos réteg virtuális, körülbelül 40 (20 000/500 IOPS egy lemezen) egyetlen tárterület-fiókjában. Olvassa el a [szokásos tároló sccounts méretezhetőség célok](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) és [prémium tároló sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Napi sebességének módosítása 
-   Adatmegőrzési mennyiségi tárhely.

Megjegyzés:

- Több adatforrás több tárterület-fiók nem időtartamát. Ez a nyissa meg a tárhely az ügyfelek védelem konfigurálásakor adatok lemezre érvényes. Az operációs rendszer és az adatmegőrzési lemezt általában lépjen a automatikusan telepített tárterület-fiókjába.
- A szükséges adatmegőrzési tároló térfogat attól függ, a napi módosítása ráta és az adatmegőrzési napok számát. A fő cél kiszolgálónként szükséges adatmegőrzési tároló napi forrásból származó teljes tejeskanna = * adatmegőrzési napok száma. 
- Minden fő tároló kiszolgáló csak egy adatmegőrzési mennyiségi tartalmaz. Az adatmegőrzési mennyiségi megoszthatja a lemezt a fő tároló kiszolgáló csatlakozik. Példa:
    - Ha egy adatforrás készülék 5 lemezt, és minden lemezre 120 IOPS (méret 8K) a forrás hoz létre, ez megfelelője 240 IOPS egy lemezen (a cél lemezen forrás IO / 2 műveletek). 240 IOPS az Azure lemez IOPS legfeljebb 500 / esik.
    - Az adatmegőrzési hangerejét a 120 * 5 = 600 ez lesz az IOPS, és a előfordulhat, hogy palack nyakvédőt. Ebben az esetben a jó stratégia több lemez adatmegőrzési hangerejének vehet fel, és egy RAID csíkok beállításokkal időtartomány keresztül, akkor lehet. Ez lesz teljesítmény javítása, mivel a IOPS vannak elosztva több meghajtók. A felvenni kívánt adatmegőrzési hangerejének meghajtók számát a következők:
        - IOPS Total a forrás-környezetből / 500
        - A forrás-környezetből (nem tömörített) napi teljes tejeskanna / 287 GB. 287 GB a maximális sebesség naponta egy do not use-által támogatott. Ez a mérőszám 8K, most írási méretével függően változik, mivel ebben az esetben 8K három írási méret tekinti. Ha például ha írási méretének 4K majd átviteli lesz 287/2. És ha a írási mérete 16 KB majd átviteli 287 * 2.
- A szám kötelező tároló fiókok = a teljes source IOPs/10000.


## <a name="before-you-start"></a>Előzetes teendők

**Összetevő** | **Követelmények** | **Részletek**
--- | --- | --- 
**Azure-fiók** | Akkor, ha a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat.
**Azure tárhely** | Szüksége lesz az Azure tároló fiók replikált adatok tárolására<br/><br/> A fiók vagy egy [Szabványos Geo felesleges tárterület-fiók](../storage/storage-redundancy.md#geo-redundant-storage) vagy [prémium tároló](../storage/storage-premium-storage.md)kell lennie.<br/><br/> A Azure webhely helyreállítás szolgáltatással ugyanabban a régióban kell, és társítjuk az azonos előfizetést. Az áthelyezés az [Új Azure portál](../storage/storage-create-storage-account.md) használata erőforráscsoport létrehozott tárterület-fiókok nem támogatjuk.<br/><br/> Megtudhatja, hogy olvassa el [a Microsoft Azure tároló – bevezetés](../storage/storage-introduction.md)
**Azure virtuális hálózati** | Egy Azure virtuális hálózat, amelyen a konfigurációs kiszolgálója és a fő tároló kiszolgáló telepítve lesz szüksége. Meg kell az előfizetést és a régió az Azure webhely helyreállítási tárolóból elemre. Ha bizonyos adatok készült ExpressRoute vagy VPN-kapcsolaton keresztül szeretné az Azure virtuális hálózati kell csatlakoznia a helyszíni hálózaton egy készült ExpressRoute kapcsolaton vagy a webhely virtuális Magánhálózattal.
**Azure erőforrások** | Győződjön meg arról, hogy minden összetevő üzembe elég Azure erőforrásokat. Olvassa el a további az [Azure előfizetési korlátok](../azure-subscription-service-limits.md).
**Azure virtuális gépeken futó** | Az [Azure Előfeltételek](site-recovery-best-practices.md), meg kell felelnie a védelemmel ellátni kívánt virtuális gépeken futó.<br/><br/> **Lemezen count**– 31 lemezt legfeljebb használható egy védett kiszolgálón<br/><br/> **Lemezen méretű**– egyes kapacitásával kerülni a Webhelyfiókok kell 1023 GB-nál nagyobb<br/><br/> **Fürtképzés**– csoportosított kiszolgálók nem támogatott<br/><br/> **Indítási**– egyesített bővíthető belső vezérlőprogram felület UEFI () / bővíthető belső vezérlőprogram Interface(EFI) indítási nem támogatott<br/><br/> **Kötet**– Bitlocker titkosított kötet nem támogatott<br/><br/> **Kiszolgálónevek**– nevek 1 és 63 karakter (betűket, számokat és kötőjelet) közé kell tartalmaznia. A név kell egy betűt vagy számot a végén pedig használja egy betűt vagy számot. Után egy számítógépre védett módosíthatók az Azure nevét.
**Konfigurációs kiszolgálója** | Szabványos A3 virtuális gép az Azure webhely helyreállítási Windows Server 2012 R2 gyűjteménye alapján létrejön az előfizetését a fiókkonfigurációs kiszolgáló. Az első példányt az új felhőalapú szolgáltatás akkor jön létre. Ha nyilvános internetkapcsolat a kapcsolat típusát a fiókkonfigurációs kiszolgáló választotta a felhőbeli szolgáltatástól létrejön egy fenntartott nyilvános IP-címet.<br/><br/> A telepítési hely elérési útja csak angol karakterek kell lennie.
**Fő tároló kiszolgáló** | Azure virtuális gép standard A4, D14 vagy DS4.<br/><br/> A telepítési hely elérési útja csak angol karakterek kell lennie. Példa az elérési út **/usr/local/ASR** Linux operációs rendszert futtató kiszolgáló fő cél kell lennie.
**Folyamatábra-kiszolgáló** | A folyamat kiszolgáló fizikai vagy Windows Server 2012 R2 rendszer fut a legújabb frissítésekben virtuális gép telepítheti. C: telepítése /.<br/><br/> Azt javasoljuk, hogy a kiszolgáló hálózati és az alhálózathoz a védelemmel ellátni kívánt gép szerint helyezze.<br/><br/> A folyamat kiszolgálón telepítse a VMware vSphere CLI 5.5.0. A VMware vSphere CLI összetevő való virtuális gépeken futó vCenter kiszolgáló által felügyelt vagy egy ESXi állomáson futó virtuális gépeken futó felfedezése szükséges a folyamat kiszolgálón.<br/><br/> A telepítési hely elérési útja csak angol karakterek kell lennie.<br/><br/> Nem támogatott reFS fájlrendszerben.
**VMware** | A VMware vSphere hypervisors kezelése VMware vCenter kiszolgáló. Meg kell verzióját kell futtatnia vCenter 5.1 vagy 5.5 a legújabb frissítésekkel együtt.<br/><br/> Egy vagy több vSphere hypervisors VMware virtuális gépeken futó tartalmazó védelemmel ellátni kívánt. A hipervizor a legújabb frissítésekben ESX/ESXi 5.1 vagy 5.5 verzióját kell futnia.<br/><br/> VMware virtuális gépeken futó VMware eszközök telepítése és futtatása kell rendelkeznie. 
**A Windows gépek** | Védett tényleges kiszolgálók vagy VMware virtuális gépeken futó Windows operációs rendszert futtató van követelményeinek.<br/><br/> A támogatott 64 bites operációs rendszer: **A Windows Server 2012 R2**, a **Windows Server 2012**, vagy **Windows Server 2008 R2 a legalacsonyabb SP1**.<br/><br/> A host Name mezőbe, a csatlakozási pontok, az eszköz nevét, a Windows rendszer elérési út (pl.: C:\Windows) kell lennie csak angol nyelven.<br/><br/> Az operációs rendszer C:\ meghajtó kell telepíthető.<br/><br/> Csak egyszerű lemez támogatottak. Dinamikus lemezen nem támogatott.<br/><br/> Védett gépeken tűzfalszabályokat kell teszi lehetővé, Azure.p konfigurálása és a fő cél kiszolgálók eléréséhez ><p>Rendszergazdai jogokkal rendelkező fiók kell (helyi gazdai jogosultsággal kell rendelkeznie a Windows-gépen) a leküldéses telepítése a Windows-kiszolgálók mobilitás szolgáltatást. Ha a megadott egy tartományon kívüli fiókot kell a helyi számítógépen távelérési felhasználói vezérlő letiltása. A teendő adja hozzá a LocalAccountTokenFilterPolicy DUPLASZÓ beállításjegyzék bejegyzést HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System az 1 értéket. A bejegyzés hozzáadása egy megnyitott cmd CLI vagy powershell, és adja meg **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Tudjon meg többet](https://msdn.microsoft.com/library/aa826699.aspx) is megtudhat a hozzáférés-vezérlés.<br/><br/> Után feladatátvételt Ha azt szeretné, hogy a távoli asztali az Azure virtuális gépeken futó győződjön meg arról, hogy a Windows rendszerbe való csatlakozás távoli asztali engedélyezve van a helyszíni számítógépen. Ha nem csatlakozik VPN fölé, tűzfalszabályokat távoli asztali kapcsolat engedélyezze az interneten keresztül.
**Linux gépek** | A támogatott 64 bites operációs rendszert: **Centos 6.4 6.5, 6.6**; **Az oracle vállalati Linux 6.4 6.5 rendszeren futó piros kalap kompatibilis kernel vagy szoros vállalati megjelenés 3.kernelfrissítés (UEK3)** **SUSE Linux vállalati kiszolgáló 11 SP3**.<br/><br/> Kattintson a védett gépek tűzfalszabályokat kell teszi lehetővé, elérje a konfigurációs és a fő cél kiszolgálók Azure-ban.<br/><br/> védett gépeken/etc/hosts-fájlok tartalmaznia kell a helyi állomásnév megfeleltetése összes NIC társított IP-címek bejegyzéseket <br/><br/> Ha szeretne csatlakozni egy Azure virtuális gépen futó Linux feladatátvétel biztonságos rendszerhéj ügyfélprogram (ssh) után, győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatás, kattintson a védett számítógép indítási automatikusan a rendszer indítási van beállítva, és tűzfalszabályokat lehetővé teszi, hogy egy ssh kapcsolatot vele.<br/><br/> A host Name mezőbe, csatlakozási pontok, nevek, az eszköz és Linux rendszerben elérési utak és fájlnevek (pl./stb /; / usr) kell angolul csak.<br/><br/> Védelem engedélyezhető az alábbi tároló helyszíni gépekhez:-<br>Fájlrendszer: EXT3, ETX4, ReiserFS, XFS<br>Többutas szoftver-eszköz hozzárendelést (többutas)<br>Mennyiségi manager: LVM2<br>Fizikai kiszolgálók HP CCISS vezérlő adathordozós nem támogatottak.
**Harmadik fél** | Ebben az esetben néhány telepítés összetevők attól függenek, hogy külső szoftver csak akkor működik megfelelően. Teljes listáját megjelennek a [harmadik felek közleményeinek-szoftverek és az adatok](#third-party)


### <a name="network-connectivity"></a>A hálózati kapcsolat

A helyszíni webhely és a Azure virtuális hálózat, amelyen az infrastruktúra-összetevők (konfigurációs kiszolgálója, fő célalkalmazás-kiszolgálók) telepítik közötti hálózati kapcsolat konfigurálásakor két lehetőség közül választhat. Döntse el, melyik használata előtt telepítheti a fiókkonfigurációs kiszolgáló hálózati kapcsolat lehetőséget kell. Válassza ezt a beállítást, a telepítés során kell. Akkor később már nem módosítható.

**Internet:** Kommunikáció és az adatok között a helyszíni (folyamat server, védett gépek) és az Azure infrastruktúra összetevő kiszolgálók (konfigurációs kiszolgálója, fő tároló kiszolgáló) replikációs történik biztonságos SSL/TLS-kapcsolaton keresztül a helyszíni nyilvános végpontjait konfigurálása és a fő cél kiszolgálókon. (Csak a kivétel ez alól a folyamat és a fő tároló kiszolgáló TCP port 9080, amely titkosítatlan közötti kapcsolat. Csak a replikáció protokoll a replikáció beállításához kapcsolódó vezérlő információkat cseréje a kapcsolaton.)

![Telepítési internet-diagram](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**Virtuális Magánhálózati**: kommunikáció és az adatok között a helyszíni (folyamat server, védett gépek) és az Azure infrastruktúra összetevő kiszolgálók (konfigurációs kiszolgálója, fő tároló kiszolgáló) replikációs történik át a virtuális Magánhálózati kapcsolat a helyszíni hálózaton és a konfigurációs és fő tároló kiszolgáló telepítik az Azure virtuális hálózati között. Győződjön meg arról, hogy a helyszíni hálózaton az Azure virtuális hálózathoz csatlakozik egy készült ExpressRoute kapcsolat vagy a webhely virtuális Magánhálózati kapcsolat által.

![Telepítésdiagram VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Lépés: 1: Hozzon létre egy tárolóból elemre

1. Jelentkezzen be az [adatkezelési portál](https://portal.azure.com).


2. Bontsa ki a **Data Services** > **Helyreállítási szolgáltatások** , és kattintson a **Webhely helyreállítási tárolóból elemre**.


3. Kattintson a **létrehozhat új** > **gyors létrehozásához**.

4. A **név**mezőbe írja be egy rövid nevet, amely azonosítja a tárolóból elemre.

5. **Régió**jelölje ki a földrajzi régióban esetében a tárolóból elemre. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége

6. Kattintson a **Létrehozás tárolóból elemre**.

    ![Új tárolóból elemre](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Jelölje be az állapotsoron a tárolóra sikeresen létrehozott megerősítéséhez. A tárolóból elemre a fő **Helyreállítási-szolgáltatások** lapon megjelenik **aktív** .

## <a name="step-2-deploy-a-configuration-server"></a>Lépés: 2: A konfigurációs server telepítése

### <a name="configure-server-settings"></a>Beállításainak konfigurálása

1. A **Helyreállítási-szolgáltatások** lapon kattintson a tárolóból elemre kattintva nyissa meg az első lépések lap. Rövid útmutató az ikonnal bármikor is megnyitható.

    ![Rövid útmutató az első ikonra](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. A legördülő listában jelölje ki **egy helyszíni webhely VMware/fizikai kiszolgálók és Azure között**.
3. Kattintson az **Előkészítése Target(Azure) erőforrások** **Üzembe konfigurációs kiszolgálója**.

    ![Konfigurációs server telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. Az **Új konfigurációs kiszolgáló részletek** megadása:

    - A fiókkonfigurációs kiszolgáló és a hitelesítő adatok való nevét.
    - Írja be a hálózati kapcsolat legördülő lista kijelölése, **Nyilvános internetes** vagy a **virtuális Magánhálózati**. Figyelje meg, hogy nem tudja módosítani ezt a beállítást, miután érvényesül.
    - Jelölje ki az Azure hálózati, amelyen a kiszolgáló el szeretné helyezni. Virtuális Magánhálózati helyezése használata biztos abban, hogy az Azure hálózati csatlakozik a helyszíni hálózaton is. 
    - Adja meg a belső IP-címének és alhálózathoz, amelyet a kiszolgálón fog kiosztani. Figyelje meg, hogy az első négy IP-címek bármely alhálózat vannak fenntartva belső Azure használatát. Minden olyan elérhető IP-címet használja.
    
    ![Konfigurációs server telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Az Azure webhely helyreállítási Windows Server 2012 R2 gyűjteménye alapján szabványos A3 virtuális gép **az OK** gombra kattintva létrejön a fiókkonfigurációs kiszolgáló előfizetését. Az első példányt az új felhőalapú szolgáltatás akkor jön létre. Ha azt választotta, hogy az interneten keresztül a felhőbeli szolgáltatástól fenntartott nyilvános IP-cím jön létre. Figyelheti a folyamatban lévő a **feladatok** lap.

    ![Monitor folyamatban](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Ha az interneten keresztül csatlakozik, a fiókkonfigurációs kiszolgáló után nem telepített Megjegyzés: a nyilvános IP-cím rendel hozzá a **virtuális gépeken futó** lapon az Azure-portálon. Kattintson a **Végpontok** lap Megjegyzés: a nyilvános HTTPS-port megfeleltetve magánjellegű 443-as port. Amikor regisztrál az a fő cél és a folyamat kiszolgálók a konfigurációs kiszolgálóval szüksége lesz az újabb ezt az információt. A kiszolgáló ezeket a végpontokat üzembe helyezéséhez:

    - HTTPS: A nyilvános portot használja összehangolására tartalomfeldolgozó és Azure közötti kommunikáció az interneten keresztül. A magánjellegű 443-as port tartalomfeldolgozó és Azure közötti kommunikáció koordinálása fölé VPN szolgál.
    - Egyéni: A nyilvános portot használja visszaállás eszköz kommunikáció az interneten keresztül. Privát port 9443 visszaállás eszköz kommunikációs használható VPN fölé.
    - A PowerShell: Személyes port 5986
    - Távoli asztali: személyes port 3389
    
    ![Virtuális végpontok](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Nem törölheti, vagy módosíthatja az a nyilvános vagy magánjellegű portszámot bármely végpontok konfigurációs server telepítése során létre.

A kiszolgáló telepítve van egy automatikusan létrehozott Azure felhőszolgáltatásában fenntartott IP-cím. A felhőalapú szolgáltatást a fenntartott címének annak érdekében, hogy a konfigurációs kiszolgáló felhőalapú szolgáltatás IP-címe változatlan marad a keresztül, a virtuális gépeken futó (beleértve a fiókkonfigurációs kiszolgáló) újraindul van szükség. A fenntartott nyilvános IP-címet kell manuálisan nem lefoglalt a fiókkonfigurációs kiszolgáló leszereltek vagy fenntartott fogja marad. Alapértelmezett legfeljebb 20 fenntartott nyilvános IP-címek előfizetésenként van. [Tudjon meg többet](../virtual-network/virtual-networks-reserved-private-ip.md) is megtudhat a fenntartott IP-címek. 

### <a name="register-the-configuration-server-in-the-vault"></a>A konfigurációs kiszolgálója rögzítheti a tárolóból elemre

1. Az **Első lépések** lapon kattintson a **Cél erőforrások előkészítése** > **Töltse le a regisztrációs billentyűt**. A fő fájl automatikusan jön létre. Érvénytelen, így az 5 nap után jön létre. Másolja a vágólapra a fiókkonfigurációs kiszolgáló.
2. **Virtuális** gépeken futó jelölje be a kiszolgáló a virtuális gépeken futó listából. Nyissa meg az **Irányítópult** lapon, és kattintson a **Csatlakozás**gombra. **Nyissa meg** a letöltött fájl RDP jelentkezzen be a távoli asztali változatában konfigurációs kiszolgálója. Ha VPN használata esetén a belső IP-címét (a bejelentkezéskor telepítette a fiókkonfigurációs kiszolgáló megadott) használható a helyszíni webhely távoli asztali kapcsolaton. Az Azure webhely helyreállítási konfigurációs kiszolgáló beállítási varázsló automatikusan elindul, amikor először jelentkezik be.

    ![Regisztráció](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. A **Külső Szoftvertelepítés** kattintson az **elfogadom** töltse le és telepítse a MySQL.

    ![MySQL-telepítés](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Hozzon létre hitelesítő adatait, és jelentkezzen be a MySQL-kiszolgálói példány **MySQL-kiszolgáló részleteket** .

    ![MySQL hitelesítő adatok](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. **Internetbeállítások** adja meg, hogyan fog a fiókkonfigurációs kiszolgáló csatlakozik az internethez. Megjegyzés:

    - Ha egy egyéni proxy használni kívánt állítson be azt a szolgáltatót telepítése előtt.
    - Kattintson a **Tovább gombra** a vizsgálat futnak ellenőrizni a proxy-kapcsolat.
    - Ha egyéni proxykiszolgálót használ, vagy az alapértelmezett proxy adja meg a proxy adatait, beleértve a cím, portokkal és hitelesítő adatok kell hitelesítést igényel.
    - Az alábbi URL-címek kell lennie a proxy keresztül érhető el:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Ha IP-cím alapú tűzfalszabályokat győződjön meg arról, hogy a szabályok az [Azure adatközpont IP-tartományok](https://msdn.microsoft.com/library/azure/dn175718.aspx) és HTTPS (443) protokoll ismertetett IP-címek és a fiókkonfigurációs kiszolgáló közötti kommunikáció engedélyezése vannak beállítva. Azt szeretné, hogy az Azure-terület, amelyet használni szeretne használata a fehér-lista IP-tartományokat, és a nyugati USA-beli.

    ![A proxykiszolgáló regisztráció](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. **Internetszolgáltató hiba Message honosítási beállításai** adja meg, milyen nyelven hibaüzenetek jelennek meg szeretné jeleníteni.

    ![Hiba üzenet regisztráció](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. **Azure webhely helyreállítási regisztrációs** tallózással keresse meg és jelölje ki a fontosabb fájlt másolta a kiszolgálóra.

    ![Regisztráció fontos fájl](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. A varázsló befejezése lapon válassza ki ezeket a beállításokat:

    - Jelölje be a **Fiók kezelése párbeszédpanel indítása** megadhatja, hogy a fiókok kezelése párbeszédpanel nyíljon meg a varázsló befejezése után.
    - Jelölje ki **egy asztali ikonnal Cspsconfigtool létrehozása** asztali parancsikon hozzáadásához kattintson a fiókkonfigurációs kiszolgáló, így megnyithatja a **Fiókok kezelése** párbeszédpanel bármikor anélkül, hogy futtassa újra a varázslót.

    ![Teljes regisztráció](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. A varázsló a **Befejezés** gombra. A jelszó jön létre. Másolja a vágólapra biztonságos helyen. Hitelesítő és a folyamatot, és a fő cél kiszolgálók regisztrálása a fiókkonfigurációs kiszolgáló szüksége. Is használják a konfigurációs kiszolgáló kommunikáció csatorna integritás biztosítása érdekében. A jelszó helyreállíthatók, de majd kell újraregisztrálása a fő cél és feldolgozása kiszolgálót az új jelszó használatával.

    ![A jelszó](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Regisztráció után a fiókkonfigurációs kiszolgáló megjelenik a **Konfigurációs kiszolgálóihoz** lapon az a tárolóból elemre.

### <a name="set-up-and-manage-accounts"></a>Állítsa be, és a fiókok kezelése

A telepítés során a webhely helyreállítási kéri hitelesítő adatokat, az alábbi műveletek:

- Egy VMware fiókkal, így thatSite helyreállítási is automatikusan feltárás VMs vCenter kiszolgálókon vagy a vSphere hosts. 
- Hozzáadásakor gépek védelme érdekében, hogy a webhely helyreállítási telepíthetem a mobilitás szolgáltatás őket.

A kiszolgáló a regisztráció után megnyithatja a **Fiókok kezelése** párbeszédpanel hozzáadásának és kezelésének fiókoknak a következő műveletek kell használni. Számos lehetőség közül választhat:

- Nyissa meg a parancsikont, akkor választotta létrehozott meg a fiókkonfigurációs kiszolgáló (cspsconfigtool) beállítása az utolsó lapján a párbeszédpanel.
- Nyissa meg a párbeszédpanel a fejezze be a kiszolgáló beállítása.

1. **A fiókok kezelése** kattintson a **Fiók hozzáadása**elemre. Is módosítása és törlése a meglévő fiókok.

    ![Fiókok kezelése](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. **Számla részletei** adja meg egy fióknevet kell használnia az Azure és a hitelesítő adatait (tartomány/neve). 

    ![Fiókok kezelése](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Csatlakozás a konfigurációs kiszolgálója 

A konfiguráció kiszolgálóhoz való csatlakozáshoz két módja van:

- VPN-webhelyek vagy készült ExpressRoute kapcsolaton
- Az interneten 

Megjegyzés:

- Internet-hozzáféréssel a végpontok a virtuális gép kiszolgáló nyilvános virtuális IP-cím együtt használja.
- A virtuális Magánhálózati kapcsolat használja a belső IP-címét a kiszolgáló a végpontot magánjellegű portokat együtt.
- A helyszíni kiszolgálókról a különböző összetevő-kiszolgálókat (konfigurációs kiszolgálója, fő tároló kiszolgáló) még egyszer használatos döntés kívánja-e csatlakozni (vezérlők, és a replikáció adatok) futó Azure virtuális Magánhálózati kapcsolat vagy az interneten keresztül. Ezt követően ez a beállítás nem módosítható. Ha nem tesz kell telepítsen újra az alkalmazási példát, és a gép reprotect.  


## <a name="step-3-deploy-the-master-target-server"></a>3 lépés: A fő cél server telepítése

1. **Felkészülés a Target(Azure)**erőforrások > **Deploy fő tároló kiszolgáló**.
2. Adja meg a fő célalkalmazás kiszolgáló részletek és a hitelesítő adatait. A kiszolgáló a fiókkonfigurációs kiszolgáló azonos Azure hálózaton fog telepíthetők. Ha egy Azure virtuális gép végrehajtásához kattintson egy Windows vagy Linux rendszerhez gyűjteménye a jön létre.

    ![Tároló kiszolgáló beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Figyelje meg, hogy az első négy IP-címek bármely alhálózat vannak fenntartva belső Azure használatát. Adja meg, hogy minden olyan elérhető IP-címet.

>[AZURE.NOTE] Jelölje be a szabványos DS4, ha annak érdekében, hogy a szolgáltató I/O intenzív munkaterhelésekből [prémium tárterület](../storage/storage-premium-storage.md)-fiókkal egységes magas I/O teljesítmény és a kis késés igénylő munkaterhelésekből védelmének beállítása.


3. A Windows fő célalkalmazás kiszolgáló virtuális ezeket a végpontokat jön létre. Figyelje meg, hogy nyilvános végpontok jönnek létre, amelyek csak a csatlakozás az interneten keresztül.

    - Egyéni: Nyilvános port való replikáció adatokat küldeni az interneten keresztül a folyamat kiszolgáló által használt. Privát port 9443 szolgál a folyamat kiszolgáló által replikációs adatok küldése a fő cél kiszolgálóhoz VPN fölé.
    - Custom1: A metaadat-alapú küldeni az interneten keresztül a folyamat kiszolgáló által használt port nyilvános. Privát port 9080 a folyamat kiszolgáló által használt metaadatok VPN Használatát a fő cél kiszolgálóhoz küldését.
    - A PowerShell: Személyes port 5986
    - Távoli asztali: személyes port 3389

4. Ezeket a végpontokat Linux fő tároló kiszolgáló virtuális jön létre. Megjegyzés: a nyilvános végpontok létrehozása csak akkor, ha az interneten keresztül csatlakozik.

    - Egyéni: Nyilvános port való replikáció adatokat küldeni az interneten keresztül folyamat kiszolgáló által használt. Privát port 9443 szolgál a folyamat kiszolgáló által replikációs adatok küldése a fő cél kiszolgálóhoz VPN fölé.
    - Custom1: Nyilvános portot használja a folyamat kiszolgáló metaadatok küldeni az interneten keresztül. Privát port 9080 a folyamat kiszolgáló által használt metaadatok küldeni a fő cél kiszolgálóhoz a virtuális Magánhálózati keresztül
    - SSH: Privát port 22-es hibakód

    >[AZURE.WARNING] Nem törölheti, vagy módosíthatja az a nyilvános vagy magánjellegű portszámot bármely, a végpontok a fő cél server telepítése során létre.

5. A **virtuális gépeken futó** Várakozás a indítsa el a virtuális gépen.

    - Ha a Windows server Megjegyzés távoli asztali részleteket.
    - Ha Linux kiszolgáló és a virtuális Magánhálózati keresztül csatlakozik Megjegyzés: a virtuális gép belső IP-címét. Ha az interneten keresztül csatlakozik Megjegyzés: a nyilvános IP-címet.

6.  Jelentkezzen be a kiszolgáló a telepítés befejezéséhez, és regisztrálhatja a konfigurációs kiszolgálóval. 
7.  Ha Windows futtatja:

    1. A virtuális gép távoli asztali kapcsolat kezdeményezhet. Az első alkalommal jelentkezik be a parancsfájl PowerShell ablakban fog futni. Ne zárja be. Amikor befejeződött, a Host ügynök Config eszköz automatikusan megjelenik a kiszolgáló regisztrálása.
    2. **Host ügynök Config** adja meg a fiókkonfigurációs kiszolgáló és a 443-as port belső IP-címét. Használhatja a belső címet és a 443-as port magánjellegű még akkor is, ha nem csatlakozik VPN fölé, mert a virtuális gép csatolva van a fiókkonfigurációs kiszolgáló azonos Azure hálózaton. Kilépés a **HTTPS használata** engedélyezve van. Adja meg a fiókkonfigurációs kiszolgáló, amely a korábban feljegyzett a hozzáférési kódot. Kattintson az **OK** regisztrálhatja a kiszolgálón. A hálózati Címfordítást beállítások figyelmen kívül hagyható. Nem használja ezeket.
    3. Ha a becsült adatmegőrzési meghajtó követelmény legfeljebb 1 TB beállíthatja a virtuális lemez és [tárhelyek](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) adatmegőrzési mennyiségi (R:)
    
    ![A Windows server-fő cél](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Ha Linux futtatja:
    1. Győződjön meg arról, hogy telepítette a legújabb Linux Integration Services (LIS) a fő cél server telepítése előtt. A legújabb LIS együtt útmutatást találhat a telepítési [Itt](https://www.microsoft.com/download/details.aspx?id=46842). A LIS telepítése után, indítsa újra a számítógépet.
    2. **Felkészülés a célhely (Azure) erőforrások** kattintson a **Töltse le és telepítse a külön szoftver (csak a Linux fő tároló kiszolgáló)**lehetőséget. A letöltött tar fájl másolása a virtuális gép egy sftp ügyfélprogramban. Másik lehetőségként a telepített linux fő cél kiszolgálóra való bejelentkezéshez, és *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* segítségével töltse le a a fájlt.
    2. Jelentkezzen be a kiszolgáló biztonságos rendszerhéj ügyfélprogram. Ha a virtuális Magánhálózati fölé a Azure hálózathoz kapcsolódik használja a belső IP-címét. Egyéb esetben használja a külső IP-cím és az SSH nyilvános végpontot.
    3. A fájlok kinyerése a gzipped telepítő futtatásával: **tar – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux fő tároló kiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Győződjön meg arról, hogy Ön a címtárban, amelybe a tar fájl tartalmának kicsomagolta.
    5. A konfiguráció kiszolgáló jelszava paranccsal helyi fájl másolása * *halvány* `<passphrase>` * > passphrase.txt**
    6. A parancs futtatása "**sudo. / -t telepíteni mindkét - a szolgáltató -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s y - c https -P passphrase.txt**".

    ![Tároló kiszolgáló regisztrálása](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Várjon néhány percet (10-15), és a lapon ellenőrizze, hogy a fő cél kiszolgálóhoz **kiszolgálókon**regisztrált szerepel-e > **Konfigurációs kiszolgálóihoz** **Kiszolgáló részletek** lapra. Ha Linux futtat, és nem regisztrált futtassa a fogadó config eszközt újra /usr/local/ASR/Vx/bin/hostconfigcli. A hozzáférési engedélyek beállítása a legfelső szintű chmod futtatásával kell.

    ![Tároló kiszolgáló ellenőrzése](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Regisztráció a fő tároló kiszolgáló szeretné, hogy a portálon befejezése után legfeljebb 15 percet is eltarthat. Azonnal frissíteni, kattintson a **frissítés** parancsra a **Konfigurációs kiszolgálóihoz** lapon.

## <a name="step-4-deploy-the-on-premises-process-server"></a>Lépés: 4: A helyszíni folyamat server telepítése

Mielőtt elkezdené javasoljuk konfigurálása a statikus IP-címet a folyamat kiszolgálón, hogy szeretné állandó újraindul garanciát.

1. Kattintson a Quick Start > **folyamat Server telepítése a helyszíni** > **Töltse le és telepítse a folyamat kiszolgáló**.

    ![Folyamat server telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  A letöltött zip-fájl másolása a kiszolgáló, amelyen a folyamat server telepítése fogjuk. A zip-fájl két telepítőfájlokat tartalmazza:

    - A Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - A Microsoft-ASR_CX_8.4.0.0_Windows*

3. Bontsa ki a archívumba, és a telepítőfájlokat másolása a kiszolgáló helyére.
4. Futtassa a **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** telepítési fájlt, és kövesse az utasításokat. Ezzel a a telepítéshez szükséges külső alkatrészek telepíti.
5. Futtassa **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. A **Kiszolgáló mód** lapján válassza ki a **Folyamat kiszolgálót**.
7. **Környezet a részletek** lapon tegye a következőket:


    - Ha a virtuális VMware védelemmel ellátni kívánt gépek kattintson az **Igen** gombra
    - Ha csak a fizikai kiszolgálók védelme szeretne, és így nem kell telepíteni a kiszolgálón folyamat VMware vCLI. Kattintson a **nem** gombra, és továbbra is.

8. VMware vCLI telepítésekor, vegye figyelembe a következőket:

    - **Csak VMware vSphere CLI 5.5.0 támogatott**. A folyamat kiszolgáló vSphere CLI frissítéseket és más verziói nem működik.
    - Töltse le a vSphere CLI 5.5.0 a [itt.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Ha telepítette az vSphere CLI előtt kezdte a folyamat server telepítése, és a telepítő nem találta meg, várjon legfeljebb öt perc ahhoz, próbálkozzon újra. Ezzel biztosíthatja, hogy az összes környezeti változó szükséges vSphere CLI kimutatására inicializálni megfelelően.

9.  **Folyamatábra-kiszolgáló hálózati kártya kijelölés** válassza a hálózati kártya, amelyet a folyamat kiszolgáló kell használnia.

    ![Kártya kijelölése](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Konfigurációs kiszolgáló részleteket**:

    - Az IP-címet és a portszámot, ha VPN keresztül csatlakozik adja meg a fiókkonfigurációs kiszolgáló és a 443-as portszámként belső IP-címét. Más módon adja meg a nyilvános virtuális IP-cím és megfeleltetett nyilvános HTTP végpontot.
    - Írja be a jelszó konfigurációs kiszolgáló.
    - Ha le szeretné tiltani a hitelesítési szolgáltatás telepítése az automatikus leküldéses használatakor, törölje a jelet **mobilitás ellenőrzése szolgáltatás szoftver aláírás** . Aláírásának ellenőrzése szüksége van internetkapcsolat, a folyamat kiszolgálóról.
    - Kattintson a **Tovább**gombra.

    ![Regisztráció konfigurációs kiszolgálója](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. **Válassza a telepítési meghajtó** válassza a gyorsítótár-meghajtóra. A folyamat kiszolgáló van szüksége a gyorsítótár meghajtót 600 GB szabad lemezterület. Kattintson a **telepítés**. 

    ![Regisztráció konfigurációs kiszolgálója](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Fontos tudni, hogy akkor lehet, hogy újra kell indítani a kiszolgálót, a telepítés befejezéséhez. A **Konfigurációs kiszolgálója** > **Server részletek** ellenőrizze, hogy a folyamat kiszolgáló jelenik meg, és sikeresen van regisztrálva a tárolóból elemre.

>[AZURE.NOTE]Regisztráció a fiókkonfigurációs kiszolgáló alatt jelennek meg a folyamat kiszolgáló befejezése után legfeljebb 15 percet is eltarthat. Azonnal frissítse a fiókkonfigurációs kiszolgáló frissítése a konfigurációs kiszolgáló lap alján a frissítés gombra kattintva
 
![Folyamatábra-kiszolgáló ellenőrzése](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Ha a folyamat kiszolgáló regisztrálásakor aláírás ellenőrzése a mobilitás szolgáltatás nem letiltja az alábbiak szerint később is megteheti:

1. Jelentkezzen be rendszergazdaként a folyamat kiszolgáló, és nyissa meg a fájlt C:\pushinstallsvc\pushinstaller.conf szerkesztésre. A szakasz a **[PushInstaller.transport]** vegye fel a sor: **SignatureVerificationChecks = "0"**. Mentse és zárja be a fájlt.
2. Indítsa újra a InMage PushInstall szolgáltatást.


## <a name="step-5-update-site-recovery-components"></a>Lépés az 5: Update webhely helyreállítási összetevők

Webhely helyreállítási összetevői időről-időre frissülnek. Új frissítések telepítését kell őket a következő sorrendben:

1. Konfigurációs kiszolgálója
2. Folyamatábra-kiszolgáló
3. Fő tároló kiszolgáló
4. Visszaállás eszköz (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Szerezze be, és a frissítések telepítése


1. A konfiguráció, folyamat és fő cél kiszolgálók frissítései az **Irányítópult**webhely helyreállítási szerezhet be. Linux telepítéshez bontsa ki a fájlokat a gzipped telepítő a és a parancs futtatásával "sudo. telepítése" a frissítés telepítése.
2. [Töltse le](http://go.microsoft.com/fwlink/?LinkID=533813) a legújabb frissítésének a visszaállás tool(vContinuum).
3. Ha futtatja a virtuális gépeken futó vagy a fizikai kiszolgálók már a mobilitás szolgáltatás telepítve van, elérheti frissítéseket a szolgáltatás az alábbi képlettel történik:

    - **A beállítás 1**: Töltse le a frissítések:
        - [A Windows Server (csak a 64 bites)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (csak a 64 bites)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Az Oracle vállalati Linux 6.4,6.5 (csak a 64 bites)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux vállalati kiszolgáló SP3 (csak a 64 bites)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - A folyamat kiszolgáló frissítése után a mobilitás szolgáltatás frissített verziójának érhetők el a folyamat kiszolgálón C:\pushinstallsvc\repository mappában.
    - **2 beállítás**: Ha egy régebbi verzióját a mobilitás szolgáltatás telepítve van a számítógépre, automatikusan frissítheti a mobilitás szolgáltatást a számítógépen az adatkezelési portálról.

        1. Győződjön meg arról, hogy a folyamat kiszolgáló frissül.
        2. Győződjön meg arról, hogy a védett számítógép megfelel-e a mobilitás szolgáltatás automatikusan terjesztése [Előfeltételek](#install-the-mobility-service-automatically) , úgy, hogy a frissítés a várt módon működik.
        2. Jelölje ki a védelem csoportot, jelölje ki a védett számítógép, és kattintson a **frissítés mobilitás szolgáltatás**. Ez a gomb csak akkor érhető el, ha a mobilitás szolgáltatás újabb verzióját. 

            ![Jelölje be a kiszolgáló vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Válassza a fiókok adja meg a rendszergazdai fiók a mobilitás szolgáltatás, kattintson a védett kiszolgáló frissíthetők. Kattintson az OK gombra, és várja meg a indítóval kezdeményezett feladat elvégzéséhez.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Lépés a 6: VCenter kiszolgálók vagy vSphere hosts hozzáadása

1. Kattintson a **kiszolgálók** > **Konfigurációs kiszolgálóihoz** > konfigurációs kiszolgálója >**Hozzáadás vCenter Server** vCenter kiszolgáló vagy vSphere állomás hozzáadása.

    ![Jelölje be a kiszolgáló vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Adja meg a kiszolgáló vagy host adatait, és jelölje ki a folyamat kiszolgálót, megtudhatja, hogy használt.

    - Ha a vCenter kiszolgáló nem fut, az alapértelmezett 443 port adja meg a portszámot, amelyen a vCenter kiszolgálón fut.
    - A folyamat kiszolgáló a vCenter server/vSphere host hálózaton kell lennie, és VMware vSphere CLI 5.5.0 telepítve kell lennie.

    ![vCenter kiszolgálójának beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Feltárás befejezése után a vCenter kiszolgáló megjelenik a listában a kiszolgáló beállításainak részleteit.

    ![vCenter kiszolgálójának beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. A rendszergazdai jogokkal nem rendelkező fiók hozzáadása a kiszolgáló vagy host használata, győződjön meg arról, hogy a fiók jogosultságai a következő:

    - vCenter fiókok és kell rendelkeznie adatközponthoz, adattárhoz, mappa, Host, hálózati, az erőforrás, tároló nézetek, virtuális gép vSphere elosztott kapcsoló jogosultságokkal engedélyezve van.
    - vSphere host fiókok kell rendelkeznie az adatközpont, adattárhoz, mappa, Host, hálózati, erőforrás, virtuális gép és vSphere elosztott kapcsoló jogosultságokkal engedélyezve



## <a name="step-7-create-a-protection-group"></a>7 lépés: A védelem csoport létrehozása

1. Nyissa meg a **Védett elemek** > **Védelem csoport** > **védelem a csoport létrehozása**.

    ![Védelem csoport létrehozása](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. A **Védelem csoport beállítások megadása** lapon adja meg a csoport nevét, és válassza a fiókkonfigurációs kiszolgáló, amelyen létre szeretne hozni a csoport.

    ![Védelem csoport beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. A lapon **Adja meg a replikáció** beállításainak a replikáció a csoport összes gépekhez használt.

    ![Védelem csoport replikációs](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Beállítások:
    - **Több virtuális konzisztencia**: Ha bekapcsolja a létrehozott megosztott alkalmazás egységes helyreállítási pontok védelem csoportjában a gépek között. Ez a beállítás az összes védelem csoportjában a gép futtatja a azonos terhelését esetén leginkább megfelelő. Minden számítógép ugyanazt az adatpont visszakerül. Csak a Windows-kiszolgálók érhető el.
    - **Készletben küszöb**: értesítések jön létre, ha folytonos adatok védelme replikációs Készletben meghaladja a beállított Készletben küszöbértéket.
    - **Helyreállítási pont adatmegőrzési**: Adja meg az adatmegőrzési ablakban. Védett gépek bármely pontjára, ezen ablakon belül állíthatók helyre.
    - **Alkalmazás egységes pillanatkép gyakoriság**: Itt adhatja meg, milyen gyakran helyreállítási pontokat tartalmazó alkalmazás egységes pillanatképek jön létre.

Azok az **Elemek védett** lapon esetén létrehozott figyelheti a védelem csoport.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>8 lépés: A védelemmel ellátni kívánt gépek beállítása

A mobilitás szolgáltatás telepítése virtuális gépeken futó és a védelemmel ellátni kívánt fizikai kiszolgálók kell. Kétféle módon teheti meg:

- Automatikusan leküldéses, és a szolgáltatás telepítése a folyamat kiszolgálóról minden gépen.
- Manuális telepítés a szolgáltatást. 

### <a name="install-the-mobility-service-automatically"></a>A mobilitás szolgáltatás automatikusan telepítése

Védelem a csoportnak gépek hozzáadásakor a mobilitás szolgáltatás automatikusan tolni és a folyamat kiszolgáló által minden számítógépre telepítve. 

**Leküldéses automatikusan a mobilitás szolgáltatás telepítse a Windows-kiszolgálók:** 

1. Telepítse a legújabb frissítéseket, a folyamat kiszolgáló, ismertetett módon [lépés 5: telepítse a legújabb frissítéseket](#step-5-install-latest-updates), és győződjön meg arról, hogy a folyamat kiszolgáló érhető el. 
2. Győződjön meg arról a forrás gép és a folyamat kiszolgáló közötti hálózati kapcsolat van, és a forrás gép elérhető a folyamat kiszolgálóról.  
3. A Windows tűzfalat lehetővé teszi a **fájl- és nyomtatómegosztás** és a **Windows Management műszerezettségi**. A Windows tűzfal beállításai csoportban jelölje be a beállítás "-alkalmazás vagy szolgáltatás átengedése a tűzfal", és jelölje ki az alkalmazást, az alábbi képen látható módon. A tartományhoz tartozó gépekhez beállíthatja a tűzfal házirend a csoportházirend-Objektumot.

    ![Tűzfal beállításai](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. A leküldéses telepítés végrehajtásához használt fiókot a rendszergazda csoportból a védelemmel ellátni kívánt gépen kell lennie. A mobilitás szolgáltatás telepítése leküldéses csak használják ezeket a hitelesítő adatokat, és kell megadnia őket Amikor felvesz egy számítógépre védelem csoport.
5. Ha a megadott felhasználói fiók tartományi fiók nem kell a helyi számítógépen távelérési felhasználói vezérlő letiltása. A teendő adja hozzá a LocalAccountTokenFilterPolicy DUPLASZÓ beállításjegyzék bejegyzést HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System az 1 értéket. A bejegyzés hozzáadása egy megnyitott cmd CLI vagy powershell, és adja meg **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Leküldéses automatikusan a mobilitás szolgáltatás telepítése Linux kiszolgálókon:**

1. Telepítse a legújabb frissítéseket, a folyamat kiszolgáló, ismertetett módon [lépés 5: telepítse a legújabb frissítéseket](#step-5-install-latest-updates), és győződjön meg arról, hogy a folyamat kiszolgáló érhető el.
2. Győződjön meg arról a forrás gép és a folyamat kiszolgáló közötti hálózati kapcsolat van, és a forrás gép elérhető a folyamat kiszolgálóról.  
3. Ellenőrizze, hogy a fiók legfelső szintű felhasználó a forrás Linux kiszolgálón.
4. Győződjön meg arról, hogy a forrás Linux kiszolgálón/etc/hosts fájl tartalmaz-e a bejegyzéseket, amely a helyi állomásnév megfeleltetése összes NIC társított IP-címek.
5. Telepítse a legújabb openssh, openssh-kiszolgáló, openssl csomagok a védelemmel ellátni kívánt gépen.
6. Győződjön meg róla, SSH engedélyezve és futó port 22. 
7. SFTP alrendszerek és a jelszó-hitelesítés engedélyezése az sshd_config fájl az alábbi képlettel történik: 

    - a) lépjen legfelső szintű.
    - b) fájl /etc/ssh/sshd_config keresse a vonal, amely **PasswordAuthentication**kezdődik.
    - c) rész megjegyzésének törlése a sort, és módosítsa a "nem", "Igen" értéket.

        ![Linux mobilitás](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) keresse meg a vonal, amely alrendszer kezdődik, és vegye ki a megjegyzésjeleket a vonalat.
    
        ![Linux leküldéses mobilitás](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Ellenőrizze az adatforrás gépi Linux változat támogatott. 
 
### <a name="install-the-mobility-service-manually"></a>A mobilitás szolgáltatás manuális telepítése

A használt mobilitás szolgáltatás telepítése szoftvercsomagok C:\pushinstallsvc\repository folyamat kiszolgálóján vannak. Jelentkezzen be a folyamat kiszolgáló és a megfelelő telepítőcsomag másolja a forrás géphez, az alábbi táblázat alapján:-

| Forrás operációs rendszer                               | Mobilitás service csomag folyamat kiszolgálón                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| A Windows Server (csak a 64 bites)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4 6.5 6.6 (csak a 64 bites)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux vállalati kiszolgáló 11 SP3 (csak a 64 bites)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Az Oracle vállalati Linux 6.4 6.5 (csak a 64 bites)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**A mobilitás szolgáltatás manuálisan a Windows server telepítése**, tegye a következőket:

1. A **Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** csomag másolja a folyamat címtár elérési útját a táblázatban felsorolt felett a forrás géphez.
2. Telepítse a mobilitás szolgáltatás a végrehajtható fájl futtatása a forrás gépen.
3. A telepítő utasításokat követve.
4. Jelölje ki a **mobilitás szolgáltatást** a szerep, és kattintson a **Tovább**gombra.
    
    ![Mobilitás szolgáltatás telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. A telepítési könyvtárban az alapértelmezett telepítési elérési útjaként hagyja, és kattintson a **telepítés**gombra.
6. Adja meg az IP-cím és HTTPS-port konfigurációs kiszolgáló **Állomás ügynök Config** .

    - Ha az interneten keresztül csatlakozik a port adja meg a nyilvános virtuális IP-cím és a nyilvános HTTPS-végpont.
    - Ha VPN keresztül csatlakozik adja meg a belső IP-cím és a 443-as portszámként. Szabadság **Használata HTTPS** ellenőrizni.

    ![Mobilitás szolgáltatás telepítése](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Adja meg a kiszolgáló konfigurációs jelszó, és kattintson az **OK gombra** a mobilitás szolgáltatás regisztrálhatja a konfigurációs kiszolgálóval.

**A parancssorból futtatása:**

1. A jelszó másolja a CX a fájlt "C:\connection.passphrase" a kiszolgálón, és ez a parancs futtatása. Ebben a példában CX i 104.40.75.37 és HTTPS-port 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Manuális Linux kiszolgálón mobilitás szolgáltatás telepítése**:

1. Másolja a vágólapra a fenti táblázatban a forrás géphez folyamat kiszolgálóról alapján megfelelő tar archívumba.
2. Nyissa meg a rendszerhéj programot, és bontsa ki a tömörített tar archívumban helyi elérési út hajtja végre`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Passphrase.txt fájl létrehozása a helyi könyvtár, amelybe kibontotta a tar archívumba tartalmának beírásával *`echo <passphrase> >passphrase.txt`* rendszerhéj.
4. A mobilitás szolgáltatás telepítése beírásával *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Adja meg az IP-cím és a port:

    - Ha az interneten keresztül a fiókkonfigurációs kiszolgáló csatlakozik, adja meg a konfigurációs kiszolgáló virtuális nyilvános IP-cím és a nyilvános HTTPS-végpont `<IP address>` és `<port>`.
    - Ha egy VPN-kapcsolaton keresztül csatlakozik adja meg a belső IP-cím és a 443-as.

**A parancssorból futtatásához**:

1. A jelszó másolja a CX a fájlt "passphrase.txt" a kiszolgálón, és ez parancsai futtathatók. Ebben a példában CX i 104.40.75.37 és HTTPS-port 62519:

A gyártási kiszolgálón telepítése:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
A target kiszolgálón telepítése:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Hozzáadásakor védelem csoportba gépek, amelyeken már a mobilitás szolgáltatás, majd a leküldéses telepítés megfelelő verziójú kimarad.


## <a name="step-9-enable-protection"></a>Lépés a 9: Engedélyezése védelme

Védelem bekapcsolása vegyen fel virtuális gépeken futó és fizikai kiszolgálók védelem a csoportnak. Mielőtt elkezdené, vegye figyelembe, hogy:

- Virtuális gépeken futó megjelenése 15 percenként és Azure webhely helyreállítási feltárás után jelennek meg, hogy legfeljebb 15 percig is eltarthat.
- Környezet módosításokat a virtuális gépen (például VMware eszközök telepítése) is készíthet frissíteni kell a webhely helyreállítási legfeljebb 15 percet.
- Érdemes észlelt legutóbb az **Utolsó a kapcsolattartó** mező az vCenter server/ESXi állomáshoz a **Konfigurációs kiszolgálóihoz** lapon.
- Ha már létrehozott védelem csoportot, és ezt követően vCenter kiszolgáló vagy ESXi állomás hozzáadása vesz igénybe 15 percet, frissítheti az Azure webhely helyreállítási portál és a virtuális gépeken futó szeretné, hogy a **védelem csoportba gépek hozzáadása** párbeszédpanelen.
- Ha azonnal folytatásához gépek felvétele védelem csoportba ütemezett felfedezése várakozás nélkül szeretné, jelölje ki a konfigurációs kiszolgálója (ne kattintson rá), és kattintson a **frissítés** gombra.
- Beállításakor virtuális gépeken futó vagy a fizikai gépek védelem a csoportnak a folyamat kiszolgáló automatikusan verembe küldi, és a mobilitás szolgáltatás telepítése az adatforrás-kiszolgálón, ha még nincs telepítve az informatikai.
- Az automatikus leküldéses rendszerére való használatra győződjön meg arról, hogy be van állítva a védett gépek az előző lépésben leírt módon.

Adja hozzá a gép az alábbi képlettel történik:

1. Kattintson a **védett elemek** > **Védelem csoport** > **gépek** > **gépek hozzáadása**. A legjobb azt javasoljuk, hogy védelem csoportok tükörképének kell lenniük a munkaterhelésekből, hogy az azonos csoportba adott alkalmazást futtató számítógépeken hozzáadott.
2. **Jelölje ki** a virtuális gépeken futó Ha fizikai kiszolgálók védelmén a **Fizikai gépek felvétele** varázslóban adja meg az IP-cím és a felhasználóbarát név. Ezután jelölje ki a operációs rendszer termékcsaládon kívüli.

    ![V középre kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. **Jelölje ki** a virtuális gépeken futó Ha VMware virtuális gépeken futó védelmén jelöljön ki egy vCenter kiszolgálót, amely a virtuális gépeken futó (vagy a EXSi fogadó, amelyen futtatja) kezeli, és válassza a gép.

    ![V középre kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. Jelölje ki a fő cél kiszolgálók és a tárhely való replikáció használja, és adja meg, hogy a beállításokat az összes munkaterhelésekből használandó **Cél erőforrások adja meg** . Jelölje ki a [Prémium tároló fiókot](../storage/storage-premium-storage.md) egységes nagy IO teljesítmény és a kis késés annak érdekében, hogy a szolgáltató IO intenzív munkaterhelésekből igénylő munkaterhelésekből védelmének beállítása közben. Ha prémium tároló használjon a terhelést merevlemezeken a szeretné, válassza a mesteralakzat Target DS sorozatának szeretne. A diaminta céllista nem DS sorozat prémium tároló lemez nem használható.

    >[AZURE.NOTE] Az áthelyezés az [Új Azure portál](../storage/storage-create-storage-account.md) használata erőforráscsoport létrehozott tárterület-fiókok nem támogatjuk.

    ![vCenter kiszolgáló](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Jelölje ki a fiókot, a mobilitás szolgáltatás telepítése védett gépeken használni kívánt **Fiókokat adja meg** . A fiók hitelesítő adatait a mobilitás szolgáltatás az automatikus telepítésére van szükség. Ha tudja kiválasztani fiókkal gondoskodjon arról, hogy beállítása egyet a 2 leírt módon. Figyelje meg, hogy ehhez a fiókhoz Azure nem használhatók. A Windows server a fiókot a forráskiszolgálóval a rendszergazdai jogosultságokkal kell tartalmaznia. A fiók Linux legfelső szintű kell lennie.

    ![Linux hitelesítő adatok](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Kattintson a Befejezés gépek hozzáadása a védelem csoport, és indítsa el az egyes gépi kezdeti replikációs. A **feladatok** lap állapot figyelheti meg.

    ![V középre kiszolgáló hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Ezenkívül figyelheti a védelem állapot kattintson a **Védett elemek** > védelem csoportnévre > **virtuális gépeken futó** . Kezdeti replikáció befejeződik, és a gép adatszinkronizálás után azok **védett** állapot jelennek meg.

    ![Virtuális gép feladatok](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>A védett számítógép tulajdonságai

1. Miután egy számítógépre **védett** állapota feladatátvevő tulajdonságait is beállíthatja. A védelem csoport adatait jelölje be a számítógépet, és nyissa meg a **beállítás** lapon.
2. Módosíthatja a nevet, amelyet szentelnek feladatátvétel és Azure virtuális gép méretének után a gép Azure-ban. Emellett bejelölheti a Azure hálózat, amelyhez a számítógép csatlakoztatva feladatátvétel után kell.

    > [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.

    ![Virtuális gép tulajdonságainak beállítása](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Megjegyzés:

- Az Azure számítógép nevét kell felelnie Azure.
- Alapértelmezés szerint a replikált virtuális gépeken futó Azure-ban és az Azure hálózat nem kapcsolódik. Ha azt szeretné, replikált virtuális gépeken futó kommunikáció feltétlenül Azure hálózatához beállítása.
- A mennyiségi egy VMware virtuális gép vagy a fizikai kiszolgálón átméretezésekor kritikus állapotba kerül. Ha van szüksége az méretének módosítása, tegye a következőket:

    - a) a beállításnak méretét.
    - b) a **virtuális gépeken futó** lapon jelölje ki a virtuális gép, és kattintson az **Eltávolítás**gombra.
    - c) **virtuális gép eltávolítása** válassza a **védelem (helyreállítási használható kevésbé részletes adatok megjelenítése és átméretezése mennyiségi) letiltása**lehetőséget. Ez a beállítás letiltja a védelem, de megőrzi a helyreállítási pontok Azure-ban.

        ![Virtuális gép tulajdonságainak beállítása](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) engedélyezheti a virtuális gép védelmét. Amikor újból a védelem engedélyezi az adatokat a átméretezett kötet átkerülnek Azure.

    

## <a name="step-10-run-a-failover"></a>10 lépés: Feladatátvevő futtatása

Jelenleg csak futtatását is lehetővé teszi a védett VMware virtuális gépeken futó és fizikai kiszolgálók tervezett feladatátadás. Vegye figyelembe az alábbiakat:



- Mielőtt feladatátvevő kezdeményezéséhez bizonyosodjon meg arról, hogy a konfigurálása és a fő cél kiszolgálók futtatása és a megfelelő. Egyéb esetben feladatátvevő sikertelen lesz.
- Forrás gépek feladatátvételhez részeként nem leállt. A védett kiszolgálók adatok replikációs feladatátvételhez elvégzéséhez leáll. Kell a gép törlése a védelem csoportot, és vegye fel újból a váratlan feladatátvételi befejezése után újra védelme a gépek indítása érdekében.
- Ha azt szeretné, adatvesztés nélkül átadni, győződjön meg arról, hogy a webhely elsődleges virtuális gépeken futó ki vannak kapcsolva a feladatátvételi elindítása előtt.

1. A **Helyreállítási tervek** lapon, és adja hozzá a helyreállítási terv. Adja meg a terv adatait, és válassza ki a **Azure** a célként.

    ![Helyreállítási terv konfigurálása](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. **Jelölje ki** a virtuális gépen jelöljön ki egy védelem csoportot, és válassza a gépek a csoport hozzáadása a helyreállítási csomagot. [További információ:](site-recovery-create-recovery-plans.md) helyreállítási csomagokról.

    ![Virtuális gépeken futó hozzáadása](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Ha szükséges testre szabhatja a csomagot, csoportok és a sorozat a rendelés létrehozásához a helyreállítás mely gépek terv vannak nem sikerült fölé. Felveheti a képernyőn megjelenő utasításokat a kézi műveletekhez és parancsfájlok is. A parancsfájlok, amikor az Azure helyreállítás felvehetők [Azure automatizálási Runbooks](site-recovery-runbook-automation.md)használatával.

4. A a **Helyreállítási tervek** lapon jelölje ki azt a csomagot, majd kattintson a **Nem tervezett feladatátvételre**.
5. **Erősítse meg című** témakörében ellenőrizze a feladatátvevő irányának (az Azure), és válassza ki a átadni a helyreállítási pontjára.
6. Várakozás a feladatátvevő feladat befejezéséhez, és kattintson a feladatátvételi működésének ellenőrzése meg a várt módon, és a replikált virtuális gépeken futó sikeres indítása érdekében Azure-ban.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Lépés 11: Fail vissza nem sikerült az Azure gépek keresztül

[Tudjon meg többet](site-recovery-failback-azure-to-vmware-classic-legacy.md) arról, hogy miként ahhoz, hogy a sikertelen operációs rendszert futtató számítógépeken fölé Azure biztonsági másolatot a helyszíni környezetben.


## <a name="manage-your-process-servers"></a>A folyamat kiszolgálók kezelése

A folyamat kiszolgáló replikációs adatokat küld a fő cél server Azure-ban, és új VMware virtuális gépeken futó vCenter kiszolgálón hozzáadott találja. Az alábbi esetekben célszerű a telepítési folyamatot kiszolgálójába módosítása:

- Ha az aktuális folyamat kiszolgáló megszakad.
- Ha a helyreállítási pont cél (Készletben) a szervezet fogadható el szintre nő.

Ha szükséges, a replikáció néhány vagy összes helyszíni VMware virtuális gépeken futó és fizikai kiszolgálók áthelyezése egy másik folyamat-kiszolgálóval. Példa:

- **Hiba**: Ha nem sikerül egy folyamatábra-kiszolgálóval, vagy nem érhető el védett számítógép replikációs áthelyezheti egy másik folyamat kiszolgálóra. Az adatforrás gépi és replika gépi metaadatok átkerülnek az új folyamat-kiszolgáló és adatok újraszinkronizálja van. Az új folyamat kiszolgáló automatikusan kapcsolódik a vCenter kiszolgáló automatikus felderítése végrehajtásához. Figyelheti a webhely helyreállítási irányítópulton folyamat kiszolgálók állapotát.
- **Módosítsa a Készletben terheléselosztási**– terheléselosztás javítását, hogy egy másik folyamat kiszolgáló kiválasztása a webhely helyreállítási portálon, és lépjen egy vagy több gépek replikációs azt kézi terheléselosztás. Ebben az esetben a kijelölt adatforrás és replika gépek metaadatok átkerül az új folyamat kiszolgáló. Az eredeti folyamat kiszolgáló marad a vCenter kiszolgáló csatlakozik. 

### <a name="monitor-the-process-server"></a>A folyamat kiszolgáló figyelése

Ha egy folyamat kiszolgáló állapot figyelmeztetés jelenik meg a webhely helyreállítási irányítópulton kritikus állapotban van. Nyissa meg az esemény fülre, és ezután Lehatolás ide adott feladatokat a feladatok lapon az állapot, hogy kattint. 

### <a name="modify-the-process-server-used-for-replication"></a>A replikáció használt folyamatot kiszolgáló módosítása

1. Nyissa meg a **kiszolgáló** > **Konfigurációs kiszolgálóihoz** > konfigurációs kiszolgálója > **Kiszolgáló részleteket**.
2. Kattintson a **Folyamat kiszolgálók** > a módosítani kívánt kiszolgáló**Módosítása folyamat kiszolgáló** .

    ![1 folyamat kiszolgáló megváltoztatása](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. A **Változás folyamat Server** > **Cél folyamat kiszolgálóhoz** , jelölje be az új kiszolgáló szeretne használni, és válassza az új kiszolgálóra való replikáció kívánt virtuális gépeken futó. Kattintson a szabad terület és a használt memória a kiszolgáló neve mellett a információk ikonra. A kijelölt virtuális gépeken bizonyos az új folyamat kiszolgálóra kötelező átlagos tér annak érdekében, hogy a döntéseket betöltése jelenik meg.

    ![2 folyamat kiszolgáló megváltoztatása](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Kattintson a esetében, amelyek az új kiszolgálóra folyamat megkezdéséhez. Figyelje meg, hogy ha eltávolítja az összes virtuális gépeken futó folyamat kiszolgáló, amely fontos azt már nem megjelenjen-e a kritikus figyelmeztetés az irányítópult.


## <a name="third-party-software-notices-and-information"></a>Harmadik féltől származó szoftver hirdetményeket és információk

Do Not Localize vagy lefordítása

A szoftver- és a Microsoft-termék vagy szolgáltatás futtató belső vezérlőprogram alapul, vagy ezzel a paranccsal beépíti a projektek, az alább felsorolt anyag (együttesen: "külső kód").  A Microsoft a harmadik féltől származó kód nem szerzőnek.  Az eredeti szerzői jogi közleményt és licenc, a melyik Microsoft fogadott e harmadik fél kód oda az alábbi vannak beállítva.

Harmadik fél kód összetevőinek a projektek, az alább felsorolt kapcsolatos szakasz található információkat. Ezek a licencek és információk találhatók csak tájékoztatási célra.  Harmadik fél kód van folyamatban relicensed, Microsoft a Microsoft licencelési szoftverlicenc a Microsoft-termék vagy szolgáltatás szerint.  

Szakasz az adatokat, amely alatt áll rendelkezésre, Microsoft csoportban az eredeti licencfeltételeket harmadik fél kód összetevők kapcsolódik.

A teljes fájlelérési előfordulhat, hogy megtalálható a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=529428). A Microsoft fenntartja összes nem kifejezett jogot benne, hogy felismerhetővé, estoppel vagy más módon.
