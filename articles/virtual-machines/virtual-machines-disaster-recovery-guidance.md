<properties
    pageTitle="Mi a teendő abban az esetben, ha egy Azure a szolgáltatási zavarok hatással van az Azure virtuális gépeken futó |} Microsoft Azure"
    description="Ismerje meg, mi a teendő abban az esetben, ha egy Azure a szolgáltatási zavarok Azure virtuális gépeken futó hatással van."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Mi a teendő abban az esetben, ha egy Azure a szolgáltatási zavarok Azure virtuális gépeken futó hatással van.

A Microsoft azt munkára szigorú ellenőrizze, hogy a szolgáltatások mindig elérhető szükség szerint csoportokat. Arra utasítja a vezérlő túl néha hatással lehet us nem tervezett szolgáltatás megszakadása okozó módon.

A Microsoft kötelezettséget üzemidőt és a kapcsolatok esetén, egy szolgáltatási szerződés SZOLGÁLTATÁSISZINT-kínál szolgáltatásai. Az egyes Azure szolgáltatások SLA [Azure Service szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)találhatók.

Azure már számos beépített platform funkciók, amelyek támogatják a könnyen hozzáférhető alkalmazásokat. További információt az alábbi szolgáltatások, olvassa el a [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetőségét](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Ez a cikk bemutatja az olyan igaz visszaállításához összeomlást követő helyreállítás során, ha egy teljes terület egy üzemszünetek fő természeti katasztrófa vagy kiterjedt szolgáltatáskimaradás miatt. Ezek a ritka előfordulások, de el kell készítenie, hogy van-e egy üzemszünetek egy teljes terület lehetőséget. Ha egy teljes terület a szolgáltatási zavarok, az adatok helyileg felesleges példányainak volna átmenetileg nem érhető el. Ha engedélyezte a replikáció geo, táblázatok és tároló Azure BLOB három további másolatok egy másik régióbeli tárolja. Egy teljes területi üzemszünetek vagy az elsődleges régió nem helyreállítható katasztrófa esetén Azure amelyek összes a DNS-bejegyzések a geo replikált területére.

>[AZURE.NOTE]Ügyeljen arra, hogy keresztül folyamathoz vezérlőhöz nem rendelkezik, és a régió szintű szolgáltatás megszakadása csak bekövetkezik. Emiatt kell is használja arra, a többi az alkalmazás-specifikus biztonsági stratégiák elérhetőségét a legmagasabb szintű eléréséhez. További információ című a [vészhelyreállítás adatok stratégiák](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery).

Segít a ritka előfordulások kezelni, elvégezheti a szükséges Azure virtuális gépeken futó az teljes terület, ahol az Azure virtuális gép alkalmazás telepítve van a szolgáltatási zavarok esetén az alábbi útmutatást.

##<a name="option-1-wait-for-recovery"></a>1 beállítást: Helyreállítási megvárja, amíg
Ebben az esetben nincs további teendő hajtana szükség. Ismerkedjen meg, hogy dolgozunk szorgalmasan szolgáltatáselérhetőség visszaállításához. Az aktuális szolgáltatásállapot az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)megjelenik.

>[AZURE.NOTE]Ha nem állított be Azure webhely helyreállítási, virtuális gép biztonsági másolatot, geo felesleges tárterület olvasásra vagy geo felesleges tároló előtt az állásidőt azzal az a legjobb megoldást. Ha állított be geo felesleges tároló vagy geo felesleges tárterület olvasásra a tárterület-fiók a virtuális virtuális merevlemezeken lévő (VHD) tároló, keresse meg a viszonyítási kép virtuális helyreállítása, és próbálkozzon egy új virtuális belőle kiépítése. Ez a beállítás nem előnyben részesített mert nincsenek szinkronizálásával kapcsolatos adatok garanciáit, kivéve ha az Azure virtuális biztonsági mentési és Azure webhely helyreállítási alkalmazásra. Ezért ezt a beállítást előfordulhat, hogy működik.

Azonnali hozzáférést szeretne virtuális gépeken futó felhasználókat az alábbi két lehetőség állnak rendelkezésre.  

>[AZURE.NOTE]Ne feledje, hogy bizonyos adatok elvesztését lehetősége van az alábbi lehetőségek egyikét.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>2 a beállítás: A virtuális visszaállítása biztonsági másolatból
Az ügyfelek, akik állította be a virtuális biztonsági másolatot visszaállíthatja a virtuális a biztonsági mentése és helyreállítása helyétől.

Egy új virtuális visszaállítása biztonsági másolatból Azure, olvassa el [az Azure virtuális gépeken futó visszaállítása](../backup/backup-azure-restore-vms.md).

Segít megtervezése az Azure virtuális gépeken futó biztonsági infrastruktúra, [megtervezése](../backup/backup-azure-vms-introduction.md)című cikk tartalmazza a virtuális biztonsági infrastruktúra Azure-ban.

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Lehetőség 3: Feladatátvevő kezdeményezzen Azure webhely helyreállítási használatával
Azure webhely helyreállítási dolgozunk a probléma által sújtott Azure virtuális gépeken futó állította be, ha az azok másolatai visszaállíthatja a VMs. Ezek a replikák az Azure vagy a helyszíni találhatók. Ebben az esetben a meglévő kópiából egy új virtuális hozhat létre. Azure webhely helyreállítási replikájának a VMs visszaállítani, olvassa el az [Azure IaaS áttelepítése virtuális gépeken futó az Azure-webhely helyreállításhoz Azure területek között](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Azure virtuális gépen futó operációs rendszer és a adatok lemezt fog kell replikált másodlagos virtuális, hogy ha geo felesleges tároló vagy geo felesleges tároló olvasásra fiókot, de egyes virtuális replikált egymástól függetlenül. Ez az engedélyszint a replikáció nem garantálja konzisztencia a replikált VHD keresztül. Ha az alkalmazás és/vagy adatbázisokra, amelyek e adatok lemezt függőségek egymással, azt nem garantálja, hogy az összes VHD replikált egy pillanatképként. Azt is előfordulhat, hogy a virtuális replika geo felesleges tároló vagy olvasásra geo felesleges tárterület-alkalmazás egységes pillanatképét indítsa el a virtuális eredményez.

##<a name="next-steps"></a>Következő lépések
Egy vészhelyreállítás és magas elérhetősége stratégia megvalósításáról kapcsolatos további információért lásd: [vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Egy felhőalapú platform funkciók részletes technikai megértése fejleszt, olvassa el az [Azure tűrőképessége technikai útmutatást](../resiliency/resiliency-technical-guidance.md).

Biztonsági másolatának VMs című témakörben talál [készítsen biztonsági másolatot az Azure virtuális gépeken futó](../backup/backup-azure-vms.md).

Megtudhatja, hogy miként téve, és a tényleges (és virtuális) a Windows és Linux gépek VMWare és a Hyper-V VMs futtatni, lásd: [Azure webhely helyreállítás](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)védelme automatizálásához Azure webhely helyreállítási használni.

Ha a képernyőn megjelenő utasításokat nem törölje a jelet, vagy ha azt szeretné, hajtsa végre a műveleteket, az Ön nevében a Microsoftnak, forduljon az [Ügyfélszolgálathoz](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
