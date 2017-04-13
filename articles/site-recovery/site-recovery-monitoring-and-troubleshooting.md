<properties
    pageTitle="Figyelésére és a védelmet a virtuális gépeken futó és fizikai kiszolgálók elhárítása |} A Microsoft Auzre" 
    description="Azure webhely helyreállítási koordináták a replikáció, feladatátvevő és Azure vagy egy másodlagos adatközponthoz helyszíni-kiszolgálón található virtuális gépeken futó visszanyerése. Ez a cikk segítségével és VMM vagy a Hyper-V webhely védelmének hibaelhárítása." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Figyelésére és virtuális gépeken futó és fizikai kiszolgálók védelme – problémamegoldás

A figyelés és hibaelhárítási útmutatója lehetővé teszi, hogy a replikáció szolgáltatásállapot nyomon követése és Azure webhely helyreállítási kapcsolatos problémák megoldása a technikákat.

## <a name="understanding-the-components"></a>Az alkatrészek ismertetése

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>A helyszíni és Azure között a replikáció VMware/tényleges a webhelyek üzembe.
A telepítő DR között a helyszíni VMware/tényleges gépi; Konfigurációs kiszolgálója, a fő cél és a folyamat Server kell konfigurálni. Miközben a forráskiszolgáló Azure webhely helyreállítási védelem bekapcsolása mobilitás szolgáltatás telepíti. Bejegyzés helyszíni üzemszünetek forráskiszolgáló sikertelen át Azure-ügyfelek igényeinek beállítása egy folyamat Server Azure-ban és a fő tároló kiszolgáló helyszíni védelme a forráskiszolgálóval vissza a helyszíni létrehozott után. 

![A helyszíni és Azure közötti replikáció webhely telepítésének VMware/tényleges](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>A helyszíni webhely között a replikáció VMM a webhelyek üzembe.

Helyszíni két helyen; közötti DR beállításának részeként Azure webhely helyreállítási szolgáltató letölti és telepíti a VMM kiszolgálón kell. Szolgáltató van szüksége annak érdekében, hogy a helyszíni műveleteket, mint a védelem, Leállítás elsődleges oldalát virtuális gépeken futó feladatátadás stb részeként Azure portálról indított az összes művelet kap lefordítani internetkapcsolat.

![A helyszíni webhely között a replikáció VMM webhelyek üzembe](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>A helyszíni és Azure közötti replikáció VMM webhely telepítésének.

A helyszíni és Azure; közötti DR beállításának részeként Azure webhely helyreállítási szolgáltató letölti és telepíti a VMM kiszolgálón együtt Azure helyreállítási szolgáltatások anyag, amely a Hyper-V állomásokon kell telepíteni kell. Olvassa el az [Azure védelem ismertetése webhely](./site-recovery-understanding-site-to-azure-protection.md) további információt.

![A helyszíni és Azure közötti replikáció VMM webhely telepítésének](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>A Hyper-V webhely telepítésének replikációs a helyszíni és Azure között

Ez az azonos VMM telepítési – csak különbség szolgáltató & ügynök kap telepítve a Hyper-V gazdawebhelyének magát. Olvassa el az [Azure védelem ismertetése webhely](./site-recovery-understanding-site-to-azure-protection.md) további információt.

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Lync-konfigurációja, a védelem és helyreállítási műveletek

Minden automatikus rendszer-Helyreállítás művelet ellenőrzött kap, és követi nyomon az "Feladatok" lapon. Bármely konfigurációja, a védelem vagy a helyreállítási hiba esetén keresse meg a feladatok lap, és látható, hogy vannak-e hibák.

![Lync-konfigurációja, a védelem és helyreállítási műveletek](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Ha megtalálta a feladatok nézetben a hibák, akkor jelölje ki a FELADATOT, és kattintson a részletek, hogy a feladat.

![Lync-konfigurációja, a védelem és helyreállítási műveletek](media/site-recovery-monitoring-and-troubleshooting/image4.png)

A részletek segítségével azonosítani tudja a lehetséges oka és a probléma való megjelöléséről.

![Lync-konfigurációja, a védelem és helyreállítási műveletek](media/site-recovery-monitoring-and-troubleshooting/image5.png)

A fenti esetben úgy tűnik, hogy egy másik olyan művelet, amelyek miatt, amelyek védelmet konfigurálása sikertelen folyamatban van. Gondoskodjon arról, hogy a probléma az ajánlási – ott után szerinti kattintson újra a művelet elindítására RESART megoldásához.

![Lync-konfigurációja, a védelem és helyreállítási műveletek](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Indítsa újra a beállítással az összes műveletet – azokat, amelyeket vissza szeretne térni az objektumra, és ismételje meg a műveletet még egyszer az Újraindítás lehetőség nem érhető el. Minden feladat meg kell szüntetni bármely pontján ideig, miközben folyamatban lévő használata a Mégse gombra.

![Lync-konfigurációja, a védelem és helyreállítási műveletek](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Virtuális gép replikációs rendszerállapot figyelése

Automatikus rendszer-Helyreállítás szolgáltató központi és távoli az Azure-portálon keresztül az egyes a védett személyek figyelése. Nyissa meg a védett elemekhez ott után, válassza a VMM FELHŐKET vagy védelem csoportok. VMM FELHŐKET lap csak alapú VMM telepítések és minden más esetben a védelem csoportok lapon a védett személyek van.

![Virtuális gép replikációs rendszerállapot figyelése](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Innen után jelölje ki a megfelelő felhő vagy a védelem csoport alatt a védett entitás. Miután kiválasztotta a védett entitás összes engedélyezett műveletek az alsó ablaktáblában jelennek meg.

![Virtuális gép replikációs rendszerállapot figyelése](media/site-recovery-monitoring-and-troubleshooting/image9.png)

ÁLLAPOT fontos – a virtuális gép esetben a fent látható módon választhatja az alsó hibaüzenet jelenik meg a Részletek gombra. A "lehetséges okok" alapján, és a "Ajánlási" említett a probléma megoldásához.

![Virtuális gép replikációs rendszerállapot figyelése](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Virtuális gép replikációs rendszerállapot figyelése](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Megjegyzés: Ha vannak olyan aktív műveleteket, amelyek a folyamatban lévő vagy sikertelen nyissa meg a feladatok nézetben a feladat meghatározott hibák megtekintése a korábbiak.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>A helyszíni a Hyper-V kapcsolatos problémák megoldása

A helyszíni a Hyper-V kezelője konzol csatlakozzon, jelölje ki a virtuális gép, és a replikáció állapot látható.

![A helyszíni a Hyper-V kapcsolatos problémák megoldása](media/site-recovery-monitoring-and-troubleshooting/image12.png)

Kritikus – *Nézet replikációs állapota* a részletek megtekintéséhez, ebben az esetben *Replikációs állapot* alatt álló jelzi.

![A helyszíni a Hyper-V kapcsolatos problémák megoldása](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Olyan esetekben, amikor fel van függesztve replikációs a virtuális gépre, kattintson a jobb gombbal választó *replikációs*->*Önéletrajz replikációs*
![fellépő helyszíni a Hyper-V kapcsolatos problémák](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Abban az esetben, ha virtuális gép áttelepíti egy új a Hyper-V állomása (a fürt vagy egy különálló számítógépre), amely automatikus rendszer-Helyreállítás révén konfigurálták, akkor a virtuális gép replikációs nem hatással. Győződjön meg arról, hogy az új a Hyper-V állomás minden a használati-követelmény megfelel-e, és automatikus rendszer-Helyreállítás van beállítva.

### <a name="event-log"></a>Eseménynaplójának

| Esemény források                | Részletek                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Alkalmazások és a szolgáltatás naplók/a Microsoft/VirtualMachineManager/Server /-rendszergazda** (VMM Server)   |  Ezzel a megoldással számos különböző VMM problémáinak hasznos naplózást. |
| **Alkalmazások és a naplókat/MicrosoftAzureRecoveryServices/replikációs szolgáltatás** (A hyper-V állomás)   | Ez a hasznos naplózás számos a Microsoft Azure helyreállítási szolgáltatások Agent hibák elhárításához nyújt. <br/> ![A Hyper-V állomás forrás](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Alkalmazások és a szolgáltatás naplók/Microsoft/Azure webhely helyreállítási/szolgáltató/műveleti** (A hyper-V állomás)   | Ez a hasznos naplózás számos a Microsoft Azure webhely helyreállítási szolgáltatás hibák elhárításához nyújt. <br/> ![A Hyper-V állomás forrás](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Alkalmazások és a szolgáltatás naplók/a Microsoft/Windows/a Hyper-V-VMMS /-rendszergazda** (A hyper-V állomás) | Ezzel a megoldással hasznos naplózásának hibaelhárítás számos a Hyper-V virtuális gép kezelése. <br/> ![A Hyper-V állomás forrás](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>A Hyper-V replikációs naplózási beállítások

Az összes a Hyper-V replika kapcsolatos eseményeket a Hyper-V-VMMS\\felügyeleti napló alatt található **alkalmazások- és szolgáltatásnaplók\\Microsoft\\Windows**. Ezenkívül egy analitikus napló engedélyezhető a Hyper-V-VMMS. Ha engedélyezni szeretné a napló, először győződjön az elemzési és hibakeresési naplók az Eseménynapló láthatóvá. Az Eseménynapló, majd a **Nézet menü**megnyitásához kattintson **elemzési és hibakeresési naplók megjelenítése**.

![A helyszíni a Hyper-V kapcsolatos problémák megoldása](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Egy analitikus napló a Hyper-V-VMMS látható.

![A helyszíni a Hyper-V kapcsolatos problémák megoldása](media/site-recovery-monitoring-and-troubleshooting/image15.png)

**Műveletek** ablaktáblában válassza a **Naplózás engedélyezése**. Miután engedélyezte, megjelenik az **Teljesítmény Monitor** , egy esemény-nyomkövetés munkamenet alatt található **adatgyűjtő.**

![A helyszíni a Hyper-V kapcsolatos problémák megoldása](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Gyűjtött adatok megtekintéséhez először a nyomkövetési munkamenet leállítása a napló letiltásával majd mentse a naplót és újra megnyitja az Eseménynapló nevet vagy egyéb eszközök segítségével tetszés szerint átalakíthatja.



## <a name="reaching-out-for-microsoft-support"></a>A Microsoft Support nyúló

### <a name="log-collection"></a>Log gyűjtemény

VMM webhely védelmét olvassa el az [Automatikus rendszer -Helyreállítás napló webhelycsoport, támogatás diagnosztika Platform (SDP) eszközzel](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) a szükséges naplógyűjtés.

A Hyper-V webhely védelmét töltse le az [eszközre](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) , és a naplógyűjtés a Hyper-V állomáson végre.

Felhasználási területei VMware/fizikai olvassa el a szükséges naplógyűjtés [Azure helyreállítási napló webhelycsoport VMware és fizikai webhely védelme](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) .

Eszköz gyűjt a naplókat véletlenszerűen elnevezett almappában **%LocalAppData%\ElevatedDiagnostics** csoportban a helyi meghajtóra

![Példa a lépéseket a Hyper-V webhely védelem jelenik meg.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>A támogatási jegyek megnyitása

Támogatási jegyek tipp az automatikus rendszer-Helyreállítás, keresse meg a Azure támogatja a <http://aka.ms/getazuresupport> az URL-cím használatával

## <a name="kb-articles"></a>KB cikkek

-   [Hogyan őrizheti meg védett virtuális gépeken futó keresztül nem sikerült vagy átkerül az Azure betűjele](http://support.microsoft.com/kb/3031135)
-   [A helyszíni az Azure védelem hálózati sávszélesség-használat kezelése](https://support.microsoft.com/kb/3056159)
-   [Automatikus rendszer-Helyreállítás: "a fürt erőforrás nem található" hibaüzenet jelenik meg ahhoz, hogy virtuális gép védelme](http://support.microsoft.com/kb/3010979)
-   [Dátumtáblázatok ismertetése és a Hyper-V replika útmutató – problémamegoldás](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Automatikus rendszer-Helyreállítás kapcsolatos gyakori hibákra, és a megoldások

Az alábbiakban a gyakori hibák, előfordulhat, hogy gombra kattint, és azok megoldásait. A hiba mindegyik külön wikilap ismertetését.

### <a name="general"></a>Általános
-   <span style="color:green;">Új</span> [Feladatok hiba miatt sikertelen "művelet van folyamatban." Hiba 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Új</span> A hiba nem működnek "Kiszolgáló nem csatlakozik az internethez" [feladatok. Hiba 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>A telepítő
-   [Belső hiba miatt nem lehet regisztrálni a a VMM kiszolgáló. Olvassa el a hiba a feladatok nézet további információt a webhely helyreállítási portálon. Futtassa újra a kiszolgáló regisztrálása telepítőt.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Kapcsolat nem hozható létre a Hyper-V helyreállítás-kezelő tárolóból elemre kattintva. Később újra próbálkozik, vagy ellenőrizze a proxybeállításokat.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguráció
-   [Nem hozható létre a védelem csoport: Hiba történt a kiszolgálók lista beolvasása.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [A Hyper-V host fürt tartalmaz legalább egy statikus hálózati adapteren, vagy nincs csatlakoztatott adaptereken DHCP használatára van beállítva.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM nincs engedélye a művelet befejezéséhez](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Nem lehet kijelölni a tárterület-fiókot az előfizetésben, miközben a védelem beállítása](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Védelem
- <span style="color:green;">Új</span> A hiba "védelem nem tudtuk konfigurálhatók a virtuális gép" hibás [védelem bekapcsolása. Hiba 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Új</span> Védelem bekapcsolása [hiányában a hiba "védelem nem tudtuk engedélyezni a virtuális gépet." Hiba 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Új</span> Szövegbevitel Live-be áthelyezendő [élő áttelepítési hibák 23848 – a virtuális gép fog. Ez lehet oldaltörés a virtuális gép helyreállítási védelem állapotának.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Nem sikerült, mert Agent nincs telepítve a állomásgép védelem bekapcsolása](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [A megfelelő szolgáltató a replika virtuális gépen nem található - miatt alacsony számítási erőforrások](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [A megfelelő szolgáltató a replika virtuális gépen nem található - miatt nem logikai hálózathoz csatlakozik](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Nem tud csatlakozni replika állomásgép - való kapcsolat nem hozható létre](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Helyreállítási
- VMM nem sikerül elvégezni a host művelet-
    -   [A kijelölt helyreállítási pont virtuális gép átadni: Általános hozzáférés-megtagadási hibát.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [A Hyper-V nem sikerült a kijelölt helyreállítási pont virtuális gép átadni: a művelet megszakítja, próbálja meg egy újabb helyreállítási pont. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   A kapcsolat a kiszolgálóval nem sikerült létrehozott (0x00002EFD)
        -   [A Hyper-V nem sikerült a virtuális gép fordított replikációs engedélyezése](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [A Hyper-V nem sikerült a replikáció virtuális gép virtuális gép engedélyezése](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Virtuális gép feladatátvevő nem érvényesíthetők.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [A helyreállítási csomagot tartalmaz virtuális gépeken futó, amelyek nem készen áll a tervezett feladatátvevő](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [A virtuális gép nem készen áll a tervezett feladatátvevő](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Virtuális gép nem fut, és nem van kapcsolva](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [Sáv művelet out történt egy virtuális gép és a jóváhagyás feladatátvevő sikertelen](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Tesztelni
    -   [Feladatátvevő nem indítható el, mivel a próba feladatátvevő folyamatban van](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Új</span>  Feladatátvevő időtúllépés történik "PreFailoverWorkflow feladathoz WaitForScriptExecutionTaskTimeout" a virtuális gép vagy az alhálózathoz, amelyhez a számítógép tartozik társított hálózati biztonsági csoport a konfigurációs beállítások miatt. Olvassa el ["PreFailoverWorkflow tevékenység WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) részleteket.


### <a name="configuration-server-process-server-master-target"></a>Konfigurációs kiszolgáló, folyamat, fő cél
Konfigurációs kiszolgáló (CS), folyamat (PS is), fő Targer (MT)
-   [A ESXi host, amelyen a PS/CS üzemelteti, mint egy virtuális nem sikerül egy lila képernyő.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Távoli asztali feladatátvétel után – hibaelhárítás
-   Vevőknek van projektvezetők problémák való csatlakozáshoz a sikertelen az Azure virtuális fölé. [A hibaelhárítási dokumentum RDP használja a virtuális be](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Egy nyilvános IP hozzáadása egy erőforrás manager virtuális gépen
Ha nem csatlakozik az Azure-Express útvonal vagy a webhely virtuális Magánhálózati kapcsolatot a **Csatlakozás** gombra a portálon szürke, kell létrehozása és hozzárendelése a nyilvános IP-címet a virtuális RDP/SSH használata előtt. Hajtsa végre az alábbi lépéseket követve egy nyilvános IP-cím hozzáadása a hálózati kapcsolaton, virtuális gép.  

![Egy nyilvános IP hozzáadása a, a hálózati kapcsolaton nem sikerült, virtuális gép keresztül](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)