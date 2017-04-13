<properties
    pageTitle="Hogyan működik a webhely helyreállítási? | Microsoft Azure"
    description="Ez a cikk áttekintést nyújt az helyreállítási webhely-architektúra"
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
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Hogyan működik az Azure webhely helyreállítási?

Ez a cikk az Azure webhely helyreállítási szolgáltatás az alapul szolgáló architektúrája megértéséhez, és a összetevők webhelyé használható. 

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.


## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor. 

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az webhely helyreállítási?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Webhely-helyreállítás az Azure-portálon

Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) létrehozásáról és használatáról az erőforrások: az erőforrás-kezelő Azure modell és a klasszikus szolgáltatások kezelése modell. Azure szintén két portálokról – az [Azure klasszikus portál](https://manage.windowsazure.com/) , amely támogatja a klasszikus telepítési modell, és az [Azure portál](https://portal.azure.com) mindkét telepítési az adatmodellek támogatása.

Webhely-helyreállítás az a klasszikus portál és az Azure portál is elérhető. Az Azure klasszikus portálon a klasszikus szolgáltatások kezelése modellel támogatja a webhely helyreállítási. Az Azure-portálon támogatja a Klasszikus modell vagy erőforrás modell telepítések. [További információ:](site-recovery-overview.md#site-recovery-in-the-azure-portal) telepítéséről az Azure portálján.

Az információk ebben a cikkben classic és az Azure portál telepítések vonatkozik. Különbségek vannak jegyezni, ahol erre lehetőség van.

## <a name="deployment-scenarios"></a>Telepítési esetek

Webhely helyreállítási telepíthető való replikáció esetek többféle téve:

- **Bizonyos VMware virtuális gépeken futó**: Azure vagy egy másodlagos adatközponthoz, hogy bizonyos helyszíni VMware virtuális gépeken futó.
- - **Fizikai gépek replikáció**:, hogy bizonyos fizikai gépek Azure vagy egy másodlagos adatközponthoz fut a Windows vagy Linux rendszerhez. A folyamat fizikai gépek kiegészítését megegyezik szinte VMware VMs kiegészítését a folyamat
- **A Hyper-V VMs bizonyos (nélkül VMM)**:, hogy bizonyos a Hyper-V VMs, amely az Azure VMM nem kezeli.
- **System Center VMM felhőket kezelhetők a Hyper-V VMs replikáció**:, hogy bizonyos helyszíni a Hyper-V virtuális gépeken futó a Hyper-V VMM felhőket ábrázoló host kiszolgálón futó Azure vagy egy másodlagos adatközponthoz. Hogy bizonyos szabványos a Hyper-V kópia vagy SAN replikációs használatával.
- **Áttelepítendő VMs**: webhely helyreállítási [Azure IaaS VMs áttelepítése](site-recovery-migrate-azure-to-azure.md) területek között, illetve [AWS Windows példányok áttelepítése](site-recovery-migrate-aws-to-azure.md) az Azure IaaS VMs is használhatja. Jelenleg csak áttelepítése támogatott, ami azt jelenti, akkor történhet a következő VMs fölé, de nem vissza sikertelen.

Webhely-helyreállítás futtatása a következő VMs és fizikai kiszolgálókon a legtöbb alkalmazások hogy bizonyos. Teljes összefoglaló a támogatott alkalmazások elérheti [milyen munkaterhelésekből Azure webhely helyreállítási megvédheti?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Az Azure párhuzamos: VMware virtuális gépeken futó vagy a fizikai Windows vagy Linux kiszolgálók

Néhány módszert kínál az VMware VMs webhely visszaállítási való replikáció.

- **Az Azure portálon**– amikor rendszerbe webhely helyreállítási az Azure-portálon klasszikus service manager tárolóhoz vagy az erőforrás-kezelő nem sikerül VMs fölé. Számos előnye, beleértve az azt jelenti, hogy a klasszikus vagy az erőforrás-kezelő tároló Azure-ban való replikáció VMware VMs replikálása az Azure-portálon életre. [Tudjon meg többet](site-recovery-vmware-to-azure.md).
- **A klasszikus portálon**-webhely helyreállítási telepítheti a klasszikus portálon élmény használatával. Ennek a klasszikus portálon minden új telepítésekhez kell használni. A környezetben, csak végződhet fölé VMs klasszikus tárolóhoz Azure-ban, és nem erőforrás-kezelő tároló. [Tudjon meg többet](site-recovery-vmware-to-azure-classic.md). A [régi élmény](site-recovery-vmware-to-azure-classic-legacy.md) a klasszikus portálon VMware replikációs beállítása is van. Ez az új telepítések kerülni a Webhelyfiókok használható.  Ha már telepítette az segítségével a régebbi élmény [áttelepítése megismerheti](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) a bővített funkciójú példányban programot.



Építészeti követelményeit webhely helyreállítási való replikáció VMware VMs/fizikai kiszolgálók az Azure-portálra vagy az Azure klasszikus portal (fokozott) hasonlóak, pár eltérések is:

- Ha telepíti az Azure-portálon erőforrás-kezelő-alapú tárolóhoz bizonyos, és használja a erőforrás-kezelő hálózatokhoz történő csatlakozás az Azure VMs feladatátvétel után.
- Ha telepíti az Azure-portálon mindkét LRS, és GRS tároló támogatott. A klasszikus portálon GRS szükség.
- A telepítési folyamatot az egyszerűsített és további felhasználóbarát az Azure-portálon.


Az alábbiakban mire lesz szüksége:

- **Azure-fiók**: szüksége van a Microsoft Azure-fiókjába.
- **Azure tároló**: Azure tároló fiók replikált adatok tárolására szüksége lesz. A klasszikus vagy erőforrás-kezelő tárterület-fiókkal is használhatja. A fiók lehet LRS vagy GRS, amikor rendszerbe állítják az Azure-portálon. Azure tároló replikált adatok vannak tárolva, és Azure VMs is be, amikor feladatátadás. 
- **Azure hálózati**: szüksége van egy Azure virtuális hálózat, amely az Azure VMs fog csatlakozni amikor éppen feladatátvételkor létrehozza őket. A klasszikus service manager modell vagy az erőforrás-kezelő modell létrehozott hálózatokat el az Azure-portálon.
- **A helyszíni konfigurációs kiszolgálója**: szüksége van egy helyszíni Windows Server 2012 R2 gépi, amelyet a fiókkonfigurációs kiszolgáló és egyéb webhely helyreállítási összetevők futtat. Ha éppen replikálása a VMware VMs kell egy könnyen hozzáférhető VMware virtuális. Ha azt szeretné, hogy való replikáció fizikai kiszolgálók a gép fizikai lehet. A számítógépen telepíti az webhely helyreállítási összetevők:
    - **Konfigurációs kiszolgálója**: koordinálja a kommunikációt a helyszíni környezet és Azure között, és adatok replikációs és helyreállítási kezeli.
    - **Folyamatábra-kiszolgáló**: replikációs átjáró működik-e. Azt replikációs adatok fogad védett forrás gépek, optimalizálja a gyorsítótár, a tömörítési és titkosítás és Azure tároló elküldi az adatokat. A védett gépek mobilitás szolgáltatás leküldéses telepítését kezeli, és is VMware VMs automatikus felderítését hajt végre. A telepítési növekedésével kezelése replikációs forgalom növekvő mennyiségű további külön dedikált folyamat kiszolgálók is hozzáadhat.
    - **A mesterlapok tároló kiszolgáló**: replikációs adatkezelési az Azure visszaállás során. 
- **VMware VMs vagy a fizikai kiszolgálók való replikáció**: minden gépi Azure való replikáció, amelyet a mobilitás összetevő lesz szüksége. Ez a szolgáltatás adatokat ír a gépen rögzíti, és a folyamat kiszolgáló úgy továbbítja. Ez az összetevő telepíthető manuálisan, vagy is lehetnek tolni és automatikusan telepíti a folyamat kiszolgáló replikációs géphez engedélyezheti.
- **vSPhere hosts/vCenter kiszolgáló**: szüksége van egy vagy több vSphere host rendszert futtató kiszolgálók VMware VMs. Azt javasoljuk, hogy ezek a hosts kezelése vCenter kiszolgáló rendszerbe.
- **Visszaállás**: van szükség:
    - **Fizikai-tényleges visszaállás nem támogatott**: Ez azt jelenti, hogy ha fizikai Azure-kiszolgálók átveszi és visszavétele is be szeretne, akkor kell nem vissza egy VMware virtuális. Vissza a fizikai kiszolgáló nem nem sikerül. Az Azure virtuális vissza sikertelen lesz szüksége lesz, és ha nem telepíti a fiókkonfigurációs kiszolgáló, mint egy VMware virtuális kell egy külön fő célalkalmazás-kiszolgáló, mint egy VMware virtuális beállítása. Ez szükséges, mert a fő tároló kiszolgáló hogyan kommunikáljon, és ha vissza szeretne állítani egy VMware virtuális lemezt VMware tároló csatolja.
    - - **Ideiglenes folyamat server Azure-ban**: Ha azt szeretné, állítsa be az Azure virtuális konfigurálva folyamat, kezelheti az Azure replikációs kell feladatátvétel után vissza az Azure meghiúsító. A virtuális visszaállás befejezése után törölheti.
    - **Virtuális Magánhálózati kapcsolat**: visszaállás szüksége lesz a virtuális Magánhálózati kapcsolat (vagy a készült Azure ExpressRoute) beállítása az Azure hálózatról a helyszíni webhelyet.
    - **A mesterlapok tároló kiszolgáló helyszíni külön**: A helyszíni fő tároló kiszolgáló visszaállás kezeli. A fő tároló kiszolgáló telepítve van a kiszolgálóra alapértelmezés szerint, de ha éppen nem működnek a forgalom vissza nagy mennyiségű állítson be egy helyszíni külön fő tároló kiszolgáló erre a célra.

**Általános architektúra**

![Továbbfejlesztett](./media/site-recovery-components/arch-enhanced.png)

**Telepítés összetevők**

![Továbbfejlesztett](./media/site-recovery-components/arch-enhanced2.png)

**Visszaállás**

![Továbbfejlesztett visszaállás](./media/site-recovery-components/enhanced-failback.png)


- [Tudjon meg többet](site-recovery-vmware-to-azure.md#azure-prerequisites) is megtudhat a Azure portál telepítésének követelményei.
- [Tudjon meg többet](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) a bővített funkciójú telepítési követelmények a klasszikus portálon.
- [Tudjon meg többet](site-recovery-failback-azure-to-vmware.md) a visszaállás a Auzre portálon.
- [Tudjon meg többet](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) a visszaállás a klasszikus portálon.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Az Azure párhuzamos: VMM nem kezeli a Hyper-V VMs

Hogy bizonyos a Hyper-V VMs, amely nem kezeli System Center VMM a webhely helyreállítási az Azure az alábbi képlettel történik:

- **Az Azure portálon**– amikor rendszerbe webhely helyreállítási az Azure-portálon klasszikus tárolóhoz vagy az erőforrás-kezelő nem sikerül VMs fölé. [Tudjon meg többet](site-recovery-hyper-v-site-to-azure.md).
- **A klasszikus portálon**-webhely helyreállítási telepítheti a klasszikus portálon. A környezetben, csak végződhet fölé VMs klasszikus tárolóhoz Azure-ban, és nem erőforrás-kezelő tároló. [Tudjon meg többet](site-recovery-hyper-v-site-to-azure-classic.md).

A architektúra mindkét telepítésekhez hasonlít, azzal a különbséggel, hogy:

- Ha az erőforrás-kezelő tárolóhoz bizonyos és használható a erőforrás-kezelő hálózatokhoz történő csatlakozás az Azure VMs feladatátvétel után Azure portálon rendszerbe.
- A telepítési folyamatot az egyszerűsített és további felhasználóbarát az Azure-portálon.

Az alábbiakban mire lesz szüksége:

- **Azure-fiók**: szüksége van a Microsoft Azure-fiókjába.
- **Azure tároló**: Azure tároló fiók replikált adatok tárolására szüksége lesz. Az Azure-portálon is használhatja a klasszikus vagy erőforrás-kezelő tárterület-fiókkal. A klasszikus portál klasszikus fiókot is használhatja. Azure tároló replikált adatok vannak tárolva, és Azure VMs létrehozásakor feladatátadás.
- **Azure hálózati**: szüksége van egy Azure hálózat, amely az Azure VMs fog csatlakozni létrehozásakor azok is feladatátvétel után. 
- **A Hyper-v host**: szüksége van egy vagy több Windows Server 2012 R2 Hyper-V kiszolgáló. Webhely helyreállítási a telepítés során kell telepítenie az Azure webhely helyreállítási szolgáltató és a Microsoft Azure helyreállítási szolgáltatási ügynökök az állomáson.
- **A Hyper-V VMs**: szüksége van egy vagy több VMs a Hyper-V kiszolgálón. Azure webhely helyreállítási szolgáltató és az Azure helyreállítási szolgáltatások ügynök a Hyper-V állomáson webhely helyreállítási a telepítés során. A szolgáltató koordinátáit, és a webhely helyreállítási szolgáltatás való replikáció orchestrates az interneten keresztül. A agent adatkezelési adatok replikációs HTTPS 443-as fölé. Kommunikáció a szolgáltató, mind a agent az, hogy a biztonságos és titkosított. Azure-tárolóban lévő replikált adatokat is titkosítva van.

**Általános architektúra**

![Azure-webhelyére a Hyper-V](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Tudjon meg többet](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) is megtudhat a Azure portál telepítésének követelményei.
- [Tudjon meg többet](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) is megtudhat a klasszikus portál telepítésének követelményei.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Az Azure párhuzamos: VMM kezeli a Hyper-V VMs

Hogy bizonyos VMM felhőket a Hyper-V VMs webhely helyreállítási az Azure a következőképpen:

- **Az Azure portálon**– amikor rendszerbe webhely helyreállítási az Azure-portálon klasszikus tárolóhoz vagy az erőforrás-kezelő nem sikerül VMs fölé. [Tudjon meg többet](site-recovery-vmm-to-azure.md).
- **A klasszikus portálon**-webhely helyreállítási telepítheti a klasszikus portálon. A környezetben, csak végződhet fölé VMs klasszikus tárolóhoz Azure-ban, és nem erőforrás-kezelő tároló. [Tudjon meg többet](site-recovery-vmm-to-azure-classic.md).

A architektúra mindkét telepítésekhez hasonlít, azzal a különbséggel, hogy:

- Ha telepíti az Azure-portálon erőforrás-kezelő-alapú tárolóhoz bizonyos, és használja a erőforrás-kezelő hálózatokhoz történő csatlakozás az Azure VMs feladatátvétel után.
- A telepítési folyamatot az egyszerűsített és további felhasználóbarát az Azure-portálon.


Az alábbiakban mire lesz szüksége:

- **Azure-fiók**: szüksége van a Microsoft Azure-fiókjába.
- **Azure tároló**: Azure tároló fiók replikált adatok tárolására szüksége lesz. Az Azure-portálon is használhatja a klasszikus vagy erőforrás-kezelő tárterület-fiókkal. A klasszikus portál klasszikus fiókot is használhatja. Azure tároló replikált adatok vannak tárolva, és Azure VMs létrehozásakor feladatátadás.
- **Azure hálózati**: állítsa be, hogy az Azure VMs megfelelő hálózatokhoz kapcsolódó létrehozásakor azok is feladatátvétel után a hálózati leképezés kell. 
- **VMM kiszolgáló**: fogja egy vagy több helyszíni VMM kiszolgálók System Center 2012 R2 rendszeren futó kell és be egy vagy több magánjellegű felhőket. Ha használja az Azure-ban telepítését portál szüksége lesz logikai, és virtuális hálózatok állítsa be, hogy a hálózati hozzárendelést beállíthatja. Ez a klasszikus portálon nem kötelező.  Egy virtuális hálózati kell kötni logikai hálózat, amely a felhőben van társítva.
- **A Hyper-v host**: szüksége van egy vagy több Windows Server 2012 R2 Hyper-V kiszolgálók VMM felhőbeli.
- **A Hyper-V VMs**: szüksége van egy vagy több VMs a Hyper-V kiszolgálón.

**Általános architektúra**

![Az Azure VMM](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Tudjon meg többet](site-recovery-vmm-to-azure.md#azure-requirements) is megtudhat a Azure portál telepítésének követelményei.
- [Tudjon meg többet](site-recovery-vmm-to-azure-classic.md#before-you-start) is megtudhat a klasszikus portál telepítésének követelményei.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Párhuzamos másodlagos webhelyre: VMware virtuális gépeken futó vagy a fizikai kiszolgálók 

Letölthető InMage Scout az Azure webhely helyreállítási előfizetés a másodlagos webhelyre replikáció VMware VMs vagy a fizikai kiszolgálókhoz. Letölthető az Azure portálról, vagy az Azure klasszikus portálról. 

Állítsa be a összetevő-kiszolgálók minden helyen (konfigurációja, a folyamatot, a fő cél.), és telepítse az egyesített ügynök való replikáció kívánt gépeken. Kezdeti replikáció után az egyes gépen az ügynök delta replikációs módosítások küld a folyamat kiszolgáló. A folyamat kiszolgáló optimalizálja az adatokat, és átadja a fő cél kiszolgálóhoz a másodlagos webhelyen. A kiszolgáló a replikáció folyamat kezeli.

Az alábbiakban szükségesek:

**Azure-fiók**: Ebben az esetben InMage Scout rendszerbe. Szerezze be, hogy szüksége lesz egy Azure-előfizetést. Miután létrehozott egy webhely helyreállítási tárolóból elemre, töltse le a InMage Scout, és telepítse a legújabb frissítéseket állíthatja be a telepítést.
**Folyamatábra-kiszolgáló (elsődleges hely)**: beállítása a folyamat kiszolgáló-összetevő elsődleges webhelyéhez gyorsítótárazás, a tömörítési és adatok optimalizálási kezelheti. Az egyesített ügynök a védelemmel ellátni kívánt gépek leküldéses telepítési is kezeli. 
**VMware ESX/ESXi és vCenter server (elsődleges hely)**: Ha éppen védelme a VMware VMs lesz szüksége a VMware EXS/ESXi hipervizor és tetszőlegesen VMware vCenter kiszolgáló hypervisors kezelése.
- **VMs/tényleges servers (elsődleges hely)**: fog védelemmel ellátni kívánt VMware VMs vagy a Windows vagy Linux fizikai kiszolgálók van szüksége a egyesített Agent. Az egyesített ügynök is telepítve van a gép, mint a fő tároló kiszolgáló. A agent működik minden összetevő közötti kommunikáció szolgáltatóként. 
- - **Konfigurációs kiszolgálója (másodlagos hely)**: A fiókkonfigurációs kiszolgáló az első összetevő telepítése, és már telepítve van a másodlagos webhely kezelése, beállítása és figyelheti a telepítéssel, a kezelés webhelyén vagy a vContinuum konzol segítségével. Csak egyetlen konfigurációs kiszolgáló környezetben van, és telepítenie kell a Windows Server 2012 R2-gépen.
- **vContinuum server (másodlagos hely)**: a konfigurációs kiszolgálójával telepítve van az adott helyen (másodlagos hely). Konzol kezelésére és figyelése a védett környezet biztosít. Az alapértelmezett telepítése a vContinuum kiszolgáló az első fő célalkalmazás-kiszolgáló és az egyesített ügynök telepítve van.
- **A mesterlapok cél server (másodlagos hely)**: A fő tároló kiszolgáló replikált adatokat tároló. Azt adatok kap a folyamat kiszolgálótól, létrehoz egy replika számítógépre a másodlagos webhelyen és az adatmegőrzési adatpontok rendelkezik. Fő cél kiszolgálók szükséges számának gépek védelem, hogy hány függ. Ha azt szeretné, az elsődleges webhelyek meghiúsító fő tároló kiszolgáló nincs túl lesz szüksége. 

**Általános architektúra**

![A VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Párhuzamos másodlagos webhelyre: VMM kezeli a Hyper-V VMs


Hogy bizonyos a Hyper-V VMs által kezelt System Center VMM egy másodlagos adatközponthoz webhely helyreállítási az alábbiak szerint:

- **Az Azure portálon**– amikor rendszerbe webhely helyreállítási az Azure-portálon. [Tudjon meg többet](site-recovery-hyper-v-site-to-azure.md).
- **A klasszikus portálon**-webhely helyreállítási telepítheti a klasszikus portálon. [Tudjon meg többet](site-recovery-hyper-v-site-to-azure-classic.md).

A architektúra mindkét telepítésekhez hasonlít, azzal a különbséggel, hogy:

- Ha az Azure-portálon be kell állítania a hálózati hozzárendelést rendszerbe. Ez nem kötelező, a klasszikus portálon.
- A telepítési folyamatot az egyszerűsített és további felhasználóbarát az Azure-portálon.
- - Klasszikus portál [tároló megfeleltetés](site-recovery-storage-mapping.md) áll rendelkezésre, ha telepíti az Azure-ban.

Az alábbiakban mire lesz szüksége:

- **Azure-fiók**: szüksége van a Microsoft Azure-fiókjába.
- **VMM kiszolgáló**: azt javasoljuk, hogy egy VMM-kiszolgálót az elsődleges webhelyen és egy tartalmazó legalább egy VMM magánjellegű felhő a másodlagos webhelyen. A kiszolgáló legalább futnia kell System Center 2012 SP1 legújabb frissítéseit, és csatlakozik az internethez. Felhőket ábrázoló a Hyper-V lehetőséget a profillal, meg kell rendelkeznie. Az Azure webhely helyreállítási szolgáltató VMM kiszolgálói kell telepítenie. A szolgáltató koordinátáit, és a webhely helyreállítási szolgáltatás való replikáció orchestrates az interneten keresztül. A szolgáltató és Azure közötti kommunikáció, hogy a biztonságos és titkosított.
- **A Hyper-V kiszolgáló**: a Hyper-V kiszolgálók kell elhelyezni, az elsődleges és másodlagos VMM felhőket. A host kiszolgálók legalább futnia kell legújabb frissítéseit a Windows Server 2012 telepítve, és csatlakozik az internethez. Adatok az elsődleges és másodlagos a Hyper-V kiszolgálók keresztül a helyi hálózati vagy Kerberos vagy tanúsítvány hitelesítéssel VPN között van replikált.  
- **Védett gépek**: A Hyper-V host forráskiszolgáló van, hogy legalább egy virtuális, amelyet védeni kíván.

**Általános architektúra**

![A helyszíni helyszíni](./media/site-recovery-components/arch-onprem-onprem.png)


- [Tudjon meg többet](site-recovery-vmm-to-vmm.md#azure-prerequisites) is megtudhat a telepítésének követelményei az Azure-portálon.
- - [Tudjon meg többet](site-recovery-vmm-to-vmm-classic.md#before-you-start) a telepítési követelmények az Azure klasszikus portálon.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Másodlagos webhelyre való replikáció SAN párhuzamos: VMM kezeli a Hyper-V VMs

Egy másodlagos webhelyre az Azure klasszikus portálon SAN replikációs VMM felhőket kezelhetők a Hyper-V VMs, hogy bizonyos. Ebben az esetben jelenleg nem támogatott az új Azure-portálon. 

Ebben az esetben webhely helyreállítási a telepítés során VMM kiszolgálókon fogja telepítse az Azure webhely helyreállítási szolgáltató. A szolgáltató koordinátáit, és a webhely helyreállítási szolgáltatás való replikáció orchestrates az interneten keresztül. Adatok az elsődleges és másodlagos tároló tömbök használatával szinkronizált SAN replikációs között van replikált.

Az alábbiakban mire lesz szüksége:

**Azure-fiók**: szüksége lesz az Azure előfizetéssel
- **SAN tömb**: egy [tömb SAN támogatott](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) az elsődleges VMM kiszolgáló által felügyelt. A SAN egy hálózati infrastruktúrát oszt meg egy másik SAN tömböt a másodlagos webhelyen.
- **VMM kiszolgáló**: azt javasoljuk, hogy egy VMM-kiszolgálót az elsődleges webhelyen és egy tartalmazó legalább egy VMM magánjellegű felhő a másodlagos webhelyen. A kiszolgáló legalább futnia kell System Center 2012 SP1 legújabb frissítéseit, és csatlakozik az internethez. Felhőket ábrázoló a Hyper-V lehetőséget a profillal, meg kell rendelkeznie.
- **A Hyper-V kiszolgáló**: a Hyper-V kiszolgálók az elsődleges és másodlagos VMM felhőket található. A host kiszolgálók legalább futnia kell legújabb frissítéseit a Windows Server 2012 telepítve, és csatlakozik az internethez.
- **Védett gépek**: A Hyper-V host forráskiszolgáló van, hogy legalább egy virtuális, amelyet védeni kíván.

**SAN replikációs architektúra**

![SAN replikációs](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Tudjon meg többet](site-recovery-vmm-san.md#before-you-start) is megtudhat a telepítési követelmények.
### <a name="on-premises"></a>A helyszíni



## <a name="hyper-v-protection-lifecycle"></a>A Hyper-V védelem életciklus

Ez a munkafolyamat látható a folyamat védheti, replikálása és a Hyper-V virtuális gépeken futó keresztül nem működnek. 

1. **Védelem bekapcsolása**: állítsa be a webhely helyreállítási tárolóból elemre, egy VMM felhő vagy a Hyper-V webhelyet replikációs beállításainak konfigurálása és VMs védelmének engedélyezése. A **Védelem bekapcsolása** nevű feladat kezdeményezésére, és a **feladatok** lap figyelhető. A feladat ellenőrzi, hogy a gép Előfeltételek megfelel-e, és ezután elindítja a [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) módszer, amely állítja be a beállítás Azure való replikáció. A **védelem** feladat is elindítja a [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) módszer egy teljes virtuális replikációs inicializálni.
2. **Kezdeti replikáció**: virtuális gép pillanatkép származik, és virtuális merevlemezeken replikált egyesével mindaddig, amíg az általuk esetén minden másolt Azure vagy a másodlagos adatközponthoz. A befejezéséhez szükséges időt virtuális méretét, a hálózat sávszélessége és a kezdeti replikációs módszer függ. Ha a lemez változások történnek a kezdeti replikációs alatt a Hyper-V replika replikációs nyilvántartása nyomon követi a ezekhez a változtatásokhoz mint a Hyper-V replikációs naplók (.hrl) a lemez ugyanabban a mappában található. Minden egyes merevlemez-másodlagos tároló küld társított .hrl fájlt tartalmaz. Figyelje meg, hogy a pillanatkép, és jelentkezzen be a fájlok lemez erőforrásait használják, miközben folyamatban van kezdeti replikáció. Amikor befejeződött a kezdeti replikáció a virtuális pillanatkép törlődik, és a napló delta lemez érintő változások szinkronizálása, és látható is benne.
3. **Véglegesítés védelme**: Kezdeti replikációs a **Véglegesítés védelem** feladat befejezése után beállítja a hálózati és más utáni replikációs beállításai, hogy a virtuális gép védelemmel van ellátva. Ha Ön éppen esetében, amelyek Azure szükség lehet, hogy készen áll a feladatátvevő animáció működését a virtuális gép beállításai. Ezen a ponton futtathatja a próba áttérni ellenőrizze, hogy minden megfelelően működik.
4. **A replikáció**: a kezdeti replikáció után delta szinkronizálás megkezdése replikációs beállításoknak megfelelően. 
    - **Replikációs hiba**: Ha nem sikerül delta replikációs, egy teljes replikációs lenne költséges értelmez sávszélesség vagy az idő, majd újraszinkronizálás fordul elő. A példában ha a .hrl fájlokat éri el a lemez méretét 50 %-os majd a virtuális az újraszinkronizálásra lesznek megjelölve. Újraszinkronizálás kis méretűre állítása a forrás- és célwebhelyek virtuális gépeken futó az ellenőrző összegeket számítások és küldése csak a delta által küldött adatok mennyiségét. Újraszinkronizálás befejezése után delta replikációs folytatódik. Alapértelmezés szerint újraszinkronizálás automatikus futásának munkaidőn kívül van ütemezve, de manuálisan is szinkronizálnia virtuális géphez.
    - **Replikációs hiba**: nincs beépített próbálkozásra replikációs hiba esetén. Ha egy nem-Helyrehozható hiba például hitelesítés vagy engedélyezés hiba, vagy egy replika számítógépre érvénytelen állapotú, nincs ismétlés kell próbált. Ha egy Helyrehozható hiba, például hálózati hibát vagy kevés a szabad terület/memória, akkor a próbálkozásra akkor fordul elő, intervallumok próbálkozások közötti növekvő (1, 2, 4, 8 és 10, és ezután 30 percenként).
4. **Tervezett tervezett feladatátadás**: tervezett vagy nem tervezett feladatátadás meg tudja nyitni, szükség szerint. Ha egy tervezett feladatátvevő, majd a forrás futtatása VMs vannak Leállítás gombot választva adatok adatvesztés elkerülése érdekében. Replika VMs létrehozása után azok a Jóváhagyás függőben állapot esetén helyezni. Kiválasztási őket a feladatátvételi befejezéséhez szükséges, kivéve, ha meg van replikálása a SAN ebben az esetben jóváhagyás automatikus. Visszaállás akkor fordulhat elő, után az elsődleges webhely lépéseket. Ha az, fordított Azure már replikált replikációs karbantarthatók. Egyéb, elindításához fordított replikációs manuálisan.
 

![munkafolyamat](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Következő lépések

[Felkészülés a telepítéshez](site-recovery-best-practices.md)
