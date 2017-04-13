<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket a replikáció a webhely-helyreállítás Azure Portal segítségével Azure |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure webhely helyreállítási való replikáció, feladatátvétel és VMM felhőket a Hyper-V VMs helyreállítása az Azure az Azure portálon téve terjesztése"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>A Hyper-V virtuális gépeken futó VMM felhőket a replikáció az Azure webhely helyreállítási használata az Azure portál Azure |} Microsoft Azure

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-azure.md)
- [Azure klasszikus](site-recovery-vmm-to-azure-classic.md)
- [Erőforrás-kezelő PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [A PowerShell klasszikus](site-recovery-deploy-with-powershell.md)

Üdvözli a Azure webhely helyreállítási! Ez a cikk, ha azt szeretné, hogy való replikáció helyszíni a Hyper-V virtuális gépeken futó kezelése a System Center virtuális gép Manager (VMM) felhőket az Azure Azure webhely helyreállítás segítségével az Azure-portálon használja.

> [AZURE.NOTE]Azure van két különböző [telepítési modellek](../resource-manager-deployment-model
> ) az erőforrások létrehozásáról és használatáról: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása.


Az Azure-portálon Azure webhely helyreállítási számos új szolgáltatásokat nyújtja:

- Az Azure-portálon az Azure biztonsági mentési és Azure webhely helyreállítási szolgáltatások soroljuk egyetlen helyreállítási szolgáltatások tárolóból elemre, így Ön beállítása és kezelése üzemkészségének és a egyetlen helyről (BCDR) helyreállítás. Az egyesített irányítópult figyelésére és tevékenységek kezelése a helyszíni webhelyek és a Azure nyilvános felhő teszi lehetővé.
- A felhasználók a felhőben megoldás szolgáltató (CSP) program kiépítve Azure előfizetések webhely helyreállítási műveletek az Azure-portálon kezelheti.
- Az Azure-portálon webhely helyreállítási hogy bizonyos gépek Azure erőforrás-kezelő tároló fiókokhoz. Feladatátvételkor webhely helyreállítási erőforrás-kezelő-alapú VMs Azure hoz létre.
- Webhely helyreállítási klasszikus tároló fiókok való replikáció támogatása továbbra is. Feladatátvételkor a webhely helyreállítási klasszikus modellt használja VMs hoz létre.


A cikket elolvasva után tartalmakat tehet közzé esetleges megjegyzéseket, a képernyő alján a Disqus megjegyzések. Kérje meg a technikai kérdések a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor.

Webhely-helyreállítás egy Azure szolgáltatás, amely a BCDR vonatkozó stratégia beleszámít a helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz orchestrating replikáció. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

Ebben a cikkben a replikáció kell minden információt a helyszíni VMM felhőket az Azure a Hyper-V VMs. Építészeti áttekintést tervezési információk és Azure, a helyszíni kiszolgálók, replikációs beállításai és kapacitás tervezés konfigurációs telepítési lépéseket tartalmazza. Miután beállította a infrastruktúra engedélyezheti replikációs gépeken védelme, és ellenőrizze, hogy feladatátvevő működik.


## <a name="business-advantages"></a>Üzleti előnyei

- Webhely helyreállítási üzleti munkaterhelésének és a Hyper-V VMs futó alkalmazások helyszínen védelmet biztosít.
- A helyreállítási Services portál beállítása, kezelése és figyelje a replikáció, feladatátvevő és helyreállítási egyetlen helyről.
- Egyszerűen futtathatja feladatátadás a helyszíni infrastruktúra Azure és csomópontoktól (visszaállítás) az Azure a Hyper-V kiszolgálók a helyszíni webhely.
- Beállíthatja a helyreállítási terv több számítógépre, úgy, hogy a közös átveszi többszintű alkalmazás munkaterhelésekből.


## <a name="scenario-architecture"></a>Forgatókönyv architektúra


Ezek a forgatókönyv összetevőket:

- **VMM kiszolgáló**: egy egy vagy több felhőket helyszíni VMM kiszolgálóval.
- **A Hyper-V host vagy fürt**: a Hyper-V kiszolgálók vagy fürt VMM felhőket kezelhetők.
- **Azure webhely helyreállítási szolgáltatója és helyreállítási szolgáltatások agent**: a telepítés során a VMM kiszolgálón, és a Microsoft Azure helyreállítási szolgáltatások agent a Hyper-V kiszolgálók telepíti az Azure webhely helyreállítási szolgáltató. A szolgáltató a VMM kiszolgálón keresztül HTTPS 443-as való replikáció üzletifolyamat-tervező kommunikál a webhely helyreállítási. A Hyper-V kiszolgálón agent adatait replikálja Azure tároló fölé HTTPS 443-as alapértelmezés szerint.
- **Azure**: van szüksége az Azure előfizetéssel, típusú adatokat tárolja replikált Azure tároló fiók és egy Azure virtuális hálózat, hogy az Azure VMs feladatátvétel után hálózathoz csatlakozik.

![E2A topológiája](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

Az alábbiakban kell Azure-ban ebben az esetben telepítése.

**Előfeltételek** | **Részletek**
--- | ---
**Azure-fiók**| A [Microsoft Azure](http://azure.microsoft.com/) -fiókra van szüksége. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | A replikált adatok tárolására szabványos Azure tárterület-fiókra van szüksége. LRS vagy GRS tároló fiók is használhatja. GRS javasoljuk, hogy a adatokat rugalmas egy területi üzemszünetek esetén, vagy ha az elsődleges régió sem állíthatók. [Tudjon meg többet](../storage/storage-redundancy.md). A fiók kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre.<br/><br/>Prémium tároló használata nem támogatott.<br/><br/> Azure tároló replikált adatok vannak tárolva, és Azure VMs létrehozásakor feladatátadás. <br/><br/> [Információ](../storage/storage-introduction.md) Azure tárhely.
**Azure hálózati** | Egy Azure virtuális hálózat csatlakozó Azure VMs feladatátvevő akkor fordul elő, ha szüksége van. A hálózat kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre.

## <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

Mire van szükség a helyszíni

**Előfeltételek** | **Részletek**
--- | ---
**VMM**| Egy vagy több VMM kiszolgálók System Center 2012 R2 rendszeren futó. Minden VMM kiszolgálón beállított egy vagy több felhőket kell rendelkeznie. Felhő tartalmaznia kell:<br/><br/> Egy vagy több VMM host csoportot.<br/><br/> Egy vagy több a Hyper-V kiszolgálók vagy fürt minden host csoportban.<br/><br/>[Tudjon meg többet](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) is megtudhat a VMM felhőket beállítása.
**A Hyper-V** | A Hyper-V kiszolgálók legalább futnia kell **A Windows Server 2012 R2** a Hyper-V szerepkör vagy a **Microsoft a Hyper-V Server 2012 R2** és telepítve van a legújabb frissítéseket.<br/><br/> A Hyper-V server tartalmaznia kell egy vagy több VMs.<br/><br/> A Hyper-V kiszolgáló vagy fürt, amely tartalmazza a kívánt való replikáció VMs egy VMM felhőben kell kezelni.<br/><br/>A Hyper-V kiszolgálók kell csatlakoznia az interneten, vagy közvetlenül egy proxyn keresztül.<br/><br/>A Hyper-V kiszolgálók említett telepített [2961977](https://support.microsoft.com/kb/2961977) a cikk megoldásai kell rendelkeznie.<br/><br/>A Hyper-V kiszolgálók internet-hozzáférés szükséges adatokat a replikáció Azure.
**Szolgáltató és agent** | Azure webhely helyreállítási a telepítés során a VMM kiszolgálón, és a helyreállítási szolgáltatások agent a Hyper-V hosts fogja telepítse az Azure webhely helyreállítási szolgáltató. A szolgáltatót, és a ügynök még nem csatlakozott a Azure, közvetlenül az interneten keresztül, illetve proxyn keresztül. Ne feledje, hogy egy HTTPS-alapú proxy nem használható. A proxykiszolgáló VMM server és a Hyper-V hosts engedélyezze a hozzáférést: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>A VMM-kiszolgálón Ha IP-cím alapú tűzfal szabályokat, ellenőrizze, hogy a szabályok kommunikációt Azure. Engedélyeznie kell az [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és HTTPS (443-as) portot.<br/><br/>Engedélyezi az IP-címtartományok az Azure a régió, az előfizetése, és a nyugati US.<br/><br/>ráadásul. a proxykiszolgáló VMM kiszolgálói https://www.msftncsi.com/ncsi.txt hozzáférésre van szüksége.


## <a name="protected-machine-prerequisites"></a>Védett számítógép vonatkozó követelmények


**Előfeltételek** | **Részletek**
--- | ---
**Védett VMs** | Csak nem sikerül egy virtuális fölé, hogy az, hogy a név, a Azure virtuális rendelt megfelel-e [Azure Előfeltételek](site-recovery-best-practices.md#azure-virtual-machine-requirements). Miután engedélyezte a virtuális replikációs módosíthatók a nevét. <br/><br/> Egyes kapacitásával védett gépeken 1023 GB-nál több kerülni a Webhelyfiókok lehet. A virtuális beállíthatja, hogy legfeljebb 16 lemez (tehát legfeljebb 16 TB).<br/><br/> A megosztott lemez Vendég fürt nem támogatott.<br/><br/> Az egyesített bővíthető belső vezérlőprogram felület UEFI () / bővíthető belső vezérlőprogram Interface(EFI) indítási nem támogatott.<br/><br/> Ha a forrás virtuális hálózati kártya csoportos együttműködési azt alakul át egy egyetlen hálózati Azure való áttérés után<br/><br/>Védelme VMs Linux operációs rendszert futtató statikus IP-címmel használata nem támogatott.

## <a name="prepare-for-deployment"></a>Felkészülés a telepítéshez

Felkészülés a telepítési kell:

1. [Az Azure-hálózat beállítása](#set-up-an-azure-network) , amelyben Azure VMs kerül feladatátvétel után.
2. [Azure tároló fiók beállítása](#set-up-an-azure-storage-account) replikált adatokat.
4. [Felkészülés a VMM server](#prepare-the-vmm-server) webhely helyreállítási telepítéshez.
5. [Felkészülés a hálózati leképezéshez](#prepare-for-network-mapping). Hálózatok beállítása, hogy a webhely helyreállítási a telepítés során hálózati hozzárendelést beállíthatja.

### <a name="set-up-an-azure-network"></a>Az Azure-hálózat beállítása

Hogy az Azure VMs feladatátvevő fog csatlakozni, miután létrehozott egy Azure hálózat szükséges.

- A hálózat kell ugyanabban a régióban, mint a központi telepítését a helyreállítási szolgáltatások tárolóból elemre.
- Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült ugyanúgy tudja beállítani az [Erőforrás-kezelő](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [Klasszikus üzemmódban](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)Azure hálózat.
- Azt javasoljuk, hogy Ön a hálózat beállítása első lépések. Ha nem, akkor van rá szükség webhely helyreállítási a telepítés során.

> [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.


### <a name="set-up-an-azure-storage-account"></a>Azure tároló fiók beállítása

- Azure replikált adatok tárolására egy szabványos Azure tárterület-fiókkal kell. A fiók kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre.
- Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült, a fiók beállítása az [Erőforrás-kezelő](../storage/storage-create-storage-account.md) vagy [Klasszikus módjában](../storage/storage-create-storage-account-classic-portal.md).
- Azt javasoljuk, hogy a fiók beállítása első lépések. Ha nem, akkor van rá szükség webhely helyreállítási a telepítés során.

> [AZURE.NOTE] [Tárterület-fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

### <a name="prepare-the-vmm-server"></a>A VMM kiszolgáló előkészítése

- Győződjön meg arról, hogy a VMM kiszolgáló megfelel-e a [Előfeltételek](#on-premises-prerequisites).
- Webhely helyreállítási a telepítés során megadhatja, hogy minden felhő VMM kiszolgálón kell lennie az Azure-portálon érhető el. Ha csak bizonyos felhőket szeretné megjeleníteni a portálon, engedélyezheti a felhőbe a VMM felügyeleti konzolban ezt a beállítást.


### <a name="prepare-for-network-mapping"></a>Hálózati hozzárendelést előkészítése

Akkor be kell állítania hálózati hozzárendelést webhely helyreállítási a telepítés során. Hálózati egyhez forrás VMM virtuális hálózatok, és adja meg a célként ahhoz, hogy a következő Azure hálózatok között:

- Ugyanazon a hálózaton keresztül nem működnek gépek csatlakozhat egymással, akkor is, ha az azok esetén nem nem sikerült fölé ugyanúgy vagy a helyreállítási megegyező csomagot.
- Ha a hálózati átjáró a cél Azure hálózati állította be, a helyszíni virtuális gépeken futó Azure virtuális gépeken futó lehet csatlakozni.
- A hálózat beállítása a leképezése itt szolgáltatás kell készítse elő:

    - Győződjön meg arról, hogy a Hyper-V host forráskiszolgáló VMs VMM virtuális hálózathoz csatlakozik. A hálózat kell kötni logikai a felhőben társított hálózathoz.
    - Az Azure hálózat ismertetett módon [fenti](#set-up-an-azure-network)

- [Tudjon meg többet](site-recovery-network-mapping.md) a hálózati hozzárendelést működéséről.


## <a name="create-a-recovery-services-vault"></a>Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Management** > **helyreállítási szolgáltatások**. Másik lehetőségként kattinthat, **tallózással keresse meg** > **Helyreállítási szolgáltatások** tárolókban > **Hozzáadás gombra**.

    ![Új tárolóból elemre](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. A **nevét**adja meg egy rövid nevet, amely azonosítja a tárolóból elemre. Ha egynél több előfizetése van, válasszon közülük.
4. [Erőforrás csoport létrehozása](../resource-group-template-deploy-portal.md), vagy jelöljön ki egy meglévőt. Adjon meg egy Azure régiót. Gépek fog kell replikált a területre. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége
4. Ha azt szeretné, hogy gyorsan elérhesse a tárolóból elemre az irányítópult, kattintson a **Rögzítés a irányítópult** > **létrehozása tárolóból elemre**.

    ![Új tárolóból elemre](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Az **Irányítópult**megjelenik az új tárolóra > **összes erőforrás**, és a fő **helyreállítási szolgáltatások tárolókban** lap.

## <a name="getting-started"></a>Első lépések

Webhely helyreállítási biztosít a Bevezetés eszköz, amely segít, hogy minél gyorsabban telepítése. Első lépések Előfeltételek ellenőrzi, és bemutatja az webhely helyreállítási telepítési lépéseket a megfelelő sorrendben.

Első lépések, jelölje ki a gépek való replikáció kívánt, és a kívánt helyre való replikáció. Beállította a helyszíni kiszolgálók, az Azure tároló fiókok és a hálózatok. Replikációs házirendek létrehozása, és végezze el a tervezési kapacitása. Miután beállította a infrastruktúra lehetősége van engedélyezni a VMs replikációs. Futtassa az adott gépekhez feladatátadás, vagy több gépek átadni a helyreállítási terv létrehozása.

Válassza a webhely helyreállítási üzembe módját kezdje el az első lépések. Az első lépések áramlás kissé attól függően, hogy a replikáció követelmények változik.



## <a name="step-1-choose-your-protection-goals"></a>Lépés: 1: Válassza ki a védelem célok

Jelölje be a Mit szeretne való replikáció és a kívánt helyre való replikáció.

1. A **tárolókban helyreállítási szolgáltatások** lap jelölje be a tárolóból elemre, és kattintson a **Beállítások**gombra.
2. Az **Első lépések** kattintson a **Webhely helyreállítási** > **lépés 1: Felkészülés infrastruktúra** > **védelem cél**.

    ![Válassza a kitűzött célok](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. A **védelem cél** jelölje ki **Az Azure**, és válassza az **Igen, a Hyper-V**. Válassza az **Igen gombra** kattintva erősítse meg az, hogy használja VMM a Hyper-V hosts és a helyreállítási webhely kezelése. Kattintson **az OK**gombra.

    ![Válassza a kitűzött célok](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Lépés: 2: A forrás-környezet beállítása

Telepítse az Azure webhely helyreállítási szolgáltató VMM kiszolgálói, és a kiszolgáló regisztrálása a tárolóból elemre. Telepítse az Azure helyreállítási szolgáltatások ügynök a Hyper-V hosts.

1. Kattintson a **lépés: 2: Felkészülés infrastruktúra** > **forrás**.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Az **Előkészítés forrás** kattintson a **+ VMM** VMM kiszolgáló hozzáadása elemre.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-azure/set-source2.png)

3. A **Kiszolgáló hozzáadása** lap ellenőrizze, hogy a **System Center VMM server** **kiszolgáló típusa** , és megjelenik az a VMM kiszolgáló megfelel-e a [Előfeltételek és URL-cím követelményeknek](#on-premises-prerequisites).
4. Töltse le az Azure webhely helyreállítási szolgáltató telepítőfájl.
5. Töltse le a regisztrációs billentyűt. Van szüksége a telepítő futtatásakor. A kulcs öt nappal azután, hoz létre, akkor nem érvényes.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Telepítse az Azure webhely helyreállítási szolgáltató a VMM kiszolgálón.


### <a name="set-up-the-azure-site-recovery-provider"></a>Az Azure webhely helyreállítási szolgáltató beállítása

1.  A szolgáltató telepítőfájl futtatása.
2. A **Microsoft Update** választhatja a frissítések keresése, hogy a szolgáltató frissítések telepítése a Microsoft Update irányelvek szerint.
3. A **telepítés**fogadja el vagy szolgáltató telepítés helyének módosítása, és kattintson a **telepítés**gombra.

    ![Telepítési hely](./media/site-recovery-vmm-to-azure/provider2.png)

4. Telepítés befejezésekor kattintson a **regisztráció** VMM kiszolgáló regisztrálása a tárolóból elemre.
5. **Tárolóból elemre a beállítások** lapon kattintson a **Tallózás** jelölje ki a tárolóból elemre kulcs fájlt. Adja meg az Azure webhely helyreállítási előfizetést és a tárolóból elemre nevét.

    ![Kiszolgáló regisztráció](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. **Internetkapcsolat**esetén adja meg, hogyan a szolgáltató a VMM kiszolgálón futó helyreállítási webhely az interneten keresztül csatlakozás lesz.

    - Ha azt szeretné, hogy a szolgáltató közvetlenül csatlakozni kattintson a **Csatlakozás közvetlenül az Azure webhely helyreállítási proxy nélkül**.
    - Ha a meglévő proxy-hitelesítést igényel, vagy a **Csatlakozás az Azure webhely helyreállítási proxykiszolgáló használata**egyéni proxy választó használni kívánt.
    - Egyéni proxy használata esetén adja meg a cím, portokkal és hitelesítő adatokat.
    - Ha használ proxykiszolgálót kell már engedélyezte a [Előfeltételek](#on-premises-prerequisites)ismertetett URL-címét.
    - Ha egy VMM RunAs fiók (DRAProxyAccount) használatával a megadott proxyhoz tartozó automatikusan létrejön egy egyéni proxy. A proxykiszolgáló beállítása, hogy ezt a fiókot sikeresen hitelesíthet A VMM RunAs fiókbeállításokat a VMM konzolban módosítható. A **Beállítások**területen bontsa ki a **biztonsági** > **Futtatása fiókok**, majd módosítsa a jelszót az DRAProxyAccount. Indítsa újra a VMM szolgáltatást, így ez a beállítás érvénybe kell.

    ![internetes](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Az elfogadás vagy a program automatikusan létrehozza az adatok titkosításhoz SSL-tanúsítvány helyének módosítása. Ha engedélyezi az Azure webhely helyreállítási portálon Azure védett felhő titkosítás ezt a tanúsítványt használja. A tanúsítvány biztonsága. Azure feladatátvevő futtatásakor szüksége visszafejtése, hogy ha az adatok titkosítása engedélyezve van.


8. A **kiszolgáló nevét**adja meg a tárolóra VMM kiszolgálójába azonosítása egy rövid nevet. A fürt konfiguráció adja meg a VMM fürt szerepkör nevét.
9. **Szinkronizálási felhőalapú metaadat-alapú** engedélyezése, ha azt szeretné, hogy minden felhő VMM kiszolgálói metaadatait szinkronizálni a tárolóból elemre. Ez a művelet csak egyszer fordulhat elő, minden egyes kiszolgálón szükséges. Ha nem szeretné az összes felhőket szinkronizálni, ezeket nem jelöli be ezt a beállítást, és egyenként az a felhő tulajdonságok a VMM konzolban minden Felhő szinkronizálása. Kattintson a **regisztráció** a folyamat befejezéséhez.

    ![Kiszolgáló regisztráció](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Regisztráció kezdődik. Regisztráció befejezése után a kiszolgáló **beállításai**megjelenik > **kiszolgálók** fel a tárolóból elemre.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Az Azure webhely helyreállítási szolgáltató parancssori telepítése

Az Azure webhely helyreállítási szolgáltató a parancssorból telepíthető. Ez a módszer a szolgáltató telepítése Server Core a Windows Server 2012 R2 használható.

1. Töltse le a szolgáltató telepítési fájl és a regisztrációs kulcs mappára. Ha például C:\ASR.
2. A rendszergazda jogú parancssort, ezek a parancsok, ki kell olvasni a szolgáltató telepítő futtatása:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Ezt a parancsot a összetevők telepítése:

            C:\ASR> setupdr.exe /i

4. Futtassa a parancsok, a kiszolgáló regisztrálása a tárolóból elemre:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Ha:

- **/Credentials**: Itt adhatja meg, ahol a bejegyzés fő fájl található paramétert kötelező megadni.  
- **/FriendlyName**: a kiszolgáló nevének a Hyper-V host az Azure webhely helyreállítási portálon megjelenő paramétert kötelező megadni.
- - **/EncryptionEnabled**: választható paraméter VMM a Hyper-V VMs verzióazonosítójának esetén felhőket az Azure. Adja meg, ha titkosítani virtuális gépeken futó Azure-ban (a többi titkosítás). Gondoskodjon arról, hogy a fájl nevét a **.pfx** bővítményét. Titkosítás alapértelmezés szerint be van kapcsolva.
- **/proxyAddress**: választható paraméter, amely megadja a címét a proxykiszolgáló.
- **/proxyport**: választható paraméter, amely megadja a port a proxykiszolgáló.
- **/proxyUsername**: választható paraméter, amely a proxy felhasználónév (Ha a proxy-hitelesítést igényel).
- **/proxyPassword**: választható paraméter, amely megadja a jelszót (Ha a proxy-hitelesítést igényel) a proxykiszolgáló hitelesítést végezni.


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Az Azure helyreállítási szolgáltatások ügynököt a Hyper-V állomásokon

1. Miután beállította a szolgáltató, le kell töltenie a telepítőfájlt az Azure helyreállítási szolgáltatások ügynök. A telepítő futtatása a Hyper-V-kiszolgálókon a VMM felhőben.

    ![A Hyper-V webhelyek](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. **Előfeltételek ellenőrzése** lapon kattintson a **Tovább**gombra. Bármely hiányzó előfeltételek automatikusan települ.

    ![Előfeltételek helyreállítási szolgáltatások Agent](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. A **Telepítési beállítások** lapon fogadja el vagy módosíthatja a telepítés helyének és a gyorsítótár helyét. A gyorsítótár adhatja meg a meghajtó, amelynek legalább 5 GB rendelkezésre álló tárhely, de azt javasoljuk, hogy a gyorsítótár meghajtót 600 GB vagy több szabad területet. Kattintson a **telepítés**.
4. Telepítés befejezése után kattintson a **Bezárás** gombra Befejezés gombra.

    ![Regisztráció MARS Agent](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Azure helyreállítási Webhelyszolgáltatások ügynök parancssori telepítése

A Microsoft Azure helyreállítási szolgáltatások Agent telepítheti a parancssorból a következő parancsot:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Webhely helyreállítása a Hyper-V hosts internetkapcsolat proxykiszolgáló beállítása

A helyreállítási szolgáltatások agent a Hyper-V hosts rendszeren futó internet-hozzáféréssel Azure virtuális replikációs szüksége van. Ha egy proxyn keresztül is szeretne hozzáférni az internethez, állítsa be az alábbi képlettel történik:

1. Nyissa meg a Microsoft Azure biztonsági másolat MMC beépülő modul a Hyper-V állomáson. Alapértelmezés szerint a Microsoft Azure biztonsági másolat használható érhető el az asztal vagy a C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\wabadmin.
2. A beépülő modult a **Tulajdonságainak módosítása**gombra.
3. A **Proxybeállítások** lapon adja meg a proxy kiszolgáló adatai.

    ![Regisztráció MARS Agent](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Győződjön meg arról, hogy a agent érhető el az URL-címeit, a [vonatkozó követelmények](#on-premises-prerequisites)ismertetett.


## <a name="step-3-set-up-the-target-environment"></a>3 lépés: A Céllista környezet beállítása

Adja meg a replikáció és Azure VMs fog csatlakozni feladatátvétel után az Azure hálózati használandó Azure tároló fiókot.

1.  Kattintson az **Előkészítés infrastruktúra** > **cél** , és válassza ki a használni kívánt Azure előfizetést.
2.  Adja meg a telepítési modellt feladatátvétel után VMs használni szeretne.
3.  Webhely helyreállítási ellenőrzi, hogy van-e egy vagy több kompatibilis Azure tároló fiókok és a hálózatok.

    ![Tárhely](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Ha eddig nem hozott létre a egy tárterület-fiókot, és hozzon létre egy újat szeretne erőforrás-kezelő használatával válassza a **+ tárterület-fiókot** , hogy szövegközi ehhez.  Egy fióknevet, típusa, előfizetés és helyét adja meg a **tárterület-fiók létrehozása** lap. A fiók a helyreállítási szolgáltatások tárolóra ugyanazon a helyen kell lennie.

    ![Tárhely](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Megjegyzés:

    - Ha szeretne a Klasszikus modell használata tárterület-fiók létrehozása, el, amely az Azure-portálon. [tudj meg többet](../storage/storage-create-storage-account-classic-portal.md)
    - Replikált adatok használata egy prémium tárterület-fiókot, meg kell szabványos további tárterület-fiók beállítása replikációs naplók, amely rögzítheti helyszíni adatok folyamatban lévő módosítása.

4.  Ha eddig nem hozott létre a egy Azure hálózat, és hozzon létre egy újat szeretne **+ hálózat** , beágyazott elvégzendő erőforrás-kezelővel gombra. Adja meg a **virtuális hálózat létrehozása** lap nevet a hálózatnak, címtartományokat, alhálózat adatait, előfizetést, és helyét. A hálózat a helyreállítási szolgáltatások tárolóra ugyanazon a helyen kell lennie.

    ![Hálózati](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Ha létre szeretne hozni egy hálózati klasszikus modellt használja az Azure-portálon kell megtennie. [Tudjon meg többet](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Hálózati hozzárendelést konfigurálása

- [Olvasás](#prepare-for-network-mapping) gyors áttekintést olvashat arról, milyen hálózati leképezés tartalmaz. [Kérjük, olvassa el](site-recovery-network-mapping.md) a részletesebb leírást.
- Győződjön meg arról, hogy a virtuális gépeken futó VMM kiszolgálói egy virtuális hálózathoz kapcsolódik, és már létrehozott legalább egy Azure virtuális hálózat. Egyetlen hálózat Azure virtuális hálózatok összevonása csatolható.

Állítsa be a hozzárendelés az alábbi képlettel történik:

1. A **Beállítások** > **Webhely helyreállítási infrastruktúra** > **hálózati hozzárendelések** > ,**Hálózati-leképezés**, kattintson a **hálózati leképezés +** ikonra.

    ![Hálózati hozzárendelést](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. A **Hozzáadás hálózati hozzárendelést** jelölje be a VMM forráskiszolgáló, és **Azure** a célként.
3. Ellenőrizze az előfizetést, és a telepítési modell feladatátvétel után.
4. **Forrás hálózat**jelölje ki a listából a VMM kiszolgáló társított megfeleltetni kívánt forrás helyszíni virtuális hálózaton.
5. **Cél hálózati**jelölje ki az Azure hálózati mely kópiában Azure VMs kerül esetén létrehozásakor. Kattintson **az OK**gombra.

    ![Hálózati hozzárendelést](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Az alábbiakban mi történik, ha a hálózati hozzárendelést kezdődik:

- Megfeleltetés megkezdésekor a forrás virtuális hálózaton meglévő VMs kapcsolódik a cél hálózathoz. Új VMs csatlakozik a forrás virtuális hálózathoz kapcsolódik a megfeleltetett Azure hálózathoz replikáció során.
- Ha módosítja egy már meglévő hálózati hozzárendelést, replika virtuális gépeken futó is csatlakoznak az új beállítások használatával.
- A cél hálózatnak több alhálózat és az egy adott alhálózat alhálózat, amelyen a forrás virtuális gép található nevét, majd a replika virtuális gép fog csatlakoznia célalkalmazás alhálózat feladatátvétel után.
- Ha nincs cél alhálózat egyező nevű, a virtuális gép csatlakozik az első alhálózat a hálózaton.



## <a name="step-4-set-up-replication-settings"></a>Lépés: 4: Beállítása replikációs beállításai


1. Való replikáció az új házirendek létrehozásához kattintson az **Előkészítés infrastruktúra** > **Replikációs beállításai** > **+ létrehozása és az társítása**.

    ![Hálózati](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. **Létrehozás és társítása házirendet**adja meg a házirend nevére.
3. **Másolja a gyakoriság**adja meg, hogy milyen gyakran való replikáció delta adatok a kezdeti replikáció (30 másodpercenként, 5-ös vagy 15 perc) után.
4. A **helyreállítási pont adatmegőrzési**adja meg, órában mennyi ideig az adatmegőrzési ablak az egyes helyreállítási pont. Védett gépek ablak bármely pontjára állíthatók helyre.
6. Az **alkalmazás egységes pillanatkép gyakoriság**, adja meg, milyen gyakran (1 – 12 óra) tartalmazó alkalmazás egységes pillanatképek helyreállítási pontok létrehozott. A Hyper-V használja kétféle pillanatképek –, ahol egy teljes virtuális gépen növekményes pillanatképét szabványos pillanatkép, és az alkalmazás adatok belül a virtuális gép pont és az idő pillanatfelvételt-alkalmazás egységes pillanatfelvételek. Alkalmazás egységes pillanatképek kötet szolgáltatás (Árnyékmásolata) használja annak érdekében, hogy alkalmazásokat konzisztens állapotát, ha a pillanatkép származik. Figyelje meg, hogy ha bekapcsolja az alkalmazás egységes pillanatképek, befolyásolja a forrás virtuális gépeken futó alkalmazások teljesítményét. Győződjön meg arról, hogy beállított értéke kisebb, mint konfigurálnia további helyreállítási pontok száma.
3. Adja meg a **Kezdeti replikációs kezdetének**, mikor induljon el a kezdeti replikáció. A replikáció az internet-sávszélesség fölé, érdemes lehet az elfoglalt órákat kívüli ütemezze.
5. Az **Azure-on tárolt adatok titkosítása**párbeszédpanelen adja meg, hogy titkosítsa az Azure-tárolóban lévő többi adatot. Kattintson **az OK**gombra.

    ![Replikációs házirend](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Amikor létrehoz egy új házirendet, a VMM felhő automatikusan ki van társítva. Kattintson az **OK gombra**. További VMM felhőket (és a bennük lévő VMs) társíthat a replikáció házirend **-beállításai** > **replikációs** > házirend neve > **VMM felhő társítani**.

    ![Replikációs házirend](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Lépés az 5: Kapacitás tervezése

Most, hogy van-e a basic infrastruktúra állíthat be, akkor is kapacitás tervezés és azt tudni, hogy szüksége a további erőforrások.

Webhely helyreállítási biztosít a kapacitás Csapattervező segít a forrás környezetben, webhely helyreállítási összetevői, hálózati és tárolására szolgáló jobb oldali erőforrásokat. A Csapattervező módszerekkel becsült VMs, a lemez és a tárhely átlagos száma alapján a gyors módban, vagy részletes módban, amelyben a terhelést szintjén ábrák fogja bemeneti futtathatja. Mielőtt elkezdené kell:

- A replikáció környezetben, többek között a VMs, per VMs lemezt, és egy lemezen tárolási információkat gyűjthet.
- Becsüljük napi módosítása (tejeskanna) a replikált adatokat is. A [Hyper-V kópia kapacitás Csapattervező](https://www.microsoft.com/download/details.aspx?id=39057) segít ezt is használhatja.

1.  Kattintson a **Letöltés** töltse le az eszközt, majd futtatásával gombra. [A cikkben olvasható](site-recovery-capacity-planner.md) az eszköz olvashatja el.
2.  Ha végzett válassza az **Igen lehetőséget** **futtatta a kapacitás Csapattervező**?

    ![Kapacitás tervezése](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Hálózati sávszélesség kapcsolatos szempontok

A kapacitás Csapattervező eszközzel kiszámítása a replikáció (kezdeti replikációs, majd delta) szükséges sávszélességet. A replikáció sávszélesség-használat szabályozhatják néhány lehetőség áll rendelkezésére:

- **Sávszélességet**: a Hyper-V adott állomáson keresztül Ugrás másodlagos webhelyre másolásához a Hyper-V-forgalmat. A sávszélesség a kiszolgálón is szabályozása.
- **Az animáció működését sávszélesség**:, befolyásolhatja a sávszélesség-replikáció beállításkulcsainak néhány használatával használja.

#### <a name="throttle-bandwidth"></a>A sávszélesség szabályozása

1. Nyissa meg a Microsoft Azure biztonsági másolat MMC beépülő modul a Hyper-V kiszolgálón. Alapértelmezés szerint a Microsoft Azure biztonsági másolat használható érhető el az asztal vagy a C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\wabadmin.
2. A beépülő modult a **Tulajdonságainak módosítása**gombra.
3. A **Throttling** lapon jelölje be az **internetes sávszélesség-használat biztonsági műveletekhez szabályozásának engedélyezése**, és a munka korlátozások, és nem munka óra. 512 KB és másodpercenként 102 MB van érvényes tartományok.

    ![A sávszélesség szabályozása](./media/site-recovery-vmm-to-azure/throttle2.png)

A [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag használatával szabályozásának beállítása. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** azt jelzi, hogy nincs szabályozásának szükséges.


#### <a name="influence-network-bandwidth"></a>Befolyással sávszélességének

A **UploadThreadsPerVM** beállításazonosítót vezérlők, melyek a lemezen (kezdeti vagy delta replikáció) adatátvitel szálak száma. Nagyobb érték esetén a replikáció használható hálózati sávszélesség nő. A **DownloadThreadsPerVM** beállításazonosítót adatátvitel visszaállás során használt szálak számát adja meg.

1. A beállításjegyzékben nyissa meg azt a **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - Módosítsa a **UploadThreadsPerVM** értékét (vagy ha még nem létezik, hozza létre a kulcsot) a vezérlő szálak lemez replikáció használja.
    - Módosítsa a **DownloadThreadsPerVM** értékét (vagy ha még nem létezik, hozza létre a kulcsot) a vezérlő szálak visszaállás forgalom az Azure használható.
2. Az alapértelmezett érték a 4. Egy "overprovisioned" hálózat a beállításkulcsok meg kell változtatni, az alapértelmezett értékeket. A maximális érték 32. Lync-forgalmat optimalizálhatja az értéket.

## <a name="step-6-enable-replication"></a>Lépés a 6: Engedélyezése replikációs

Most lehetővé teszi a replikáció az alábbi képlettel történik:

1. Kattintson a **lépés 2: alkalmazás bizonyos** > **forrás**. Miután engedélyezte a replikáció először **+ bizonyos** a replikáció további gépekhez engedélyezéséhez a tárolóból elemre kattint.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. Kattintson a **forrás** lap > jelölje ki a VMM kiszolgáló és a felhőben, amelyben a Hyper-V hosts találhatók. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. **Cél** kijelölése az előfizetést, post átváltó telepítési modell és a tárterület-fiók replikált adatok használata

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Jelölje ki a használni kívánt tárterület-fiókot. Ha azt szeretné, mint a másik tárterület-fiókkal rendelkezik, [Hozzon létre egy újat](#set-up-an-azure-storage-account)is. **Új létrehozása**kattintva hozhat létre a tárterület-fiókba az erőforrás-kezelő modell. Ha szeretne a Klasszikus modell használata tárterület-fiók létrehozása, teheti meg, hogy [az Azure-portálon](../storage/storage-create-storage-account-classic-portal.md). Kattintson **az OK**gombra.
5. Jelölje be az Azure hálózat, amelyhez Azure VMs fog csatlakozni, ha az azok esetén is be feladatátvétel után alhálózat. Válassza a **Konfigurálás most kijelölt gépekhez** hálózati beállítás alkalmazása az összes gépekhez ki védelmét. Válassza a **Beállítás később** jelölje ki az Azure hálózati per gépi. A másik hálózathoz használni kívánt esetén akkor is, [Hozzon létre egy újat](#set-up-an-azure-network). Az erőforrás-kezelő adatmodellt használó hálózat létrehozásához kattintson az **Új létrehozása**. Ha létre szeretne hozni egy hálózati klasszikus modellt használja a fogja, hogy [az Azure-portálon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)el. Jelölje ki a alhálózat, ha van ilyen. Kattintson **az OK**gombra.
6. **Virtuális**gépeken futó > **Jelölje ki a virtuális gépeken futó** kattintson, és válassza a minden kívánt való replikáció gépet. Jelölje ki a replikáció engedélyezhető gépek csak. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. A **Tulajdonságok** > **tulajdonságainak beállítása**, jelölje be az operációs rendszer a kijelölt VMs és a rendszer lemez. Kattintson **az OK**gombra. Beállíthatja, hogy további tulajdonságot később.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. **Replikációs**beállításai > **replikációs beállítások**csoportban jelölje ki a replikáció házirendet a védett VMs alkalmazni szeretné. Kattintson **az OK**gombra. A replikáció házirend **-beállításai**módosíthatók > **replikációs házirendek** > házirend neve > **Beállítások szerkesztése elemre**. A módosítások alkalmazása már replikálja gépek, és új gépek segítségével.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/enable-replication7.png)

A **Védelem bekapcsolása** feladat **beállításai**végrehajtásának nyomon követhető > **feladatok** > **webhely helyreállítási feladatokat**. A **Védelem véglegesítése** feladat futtatása a gép után van készen áll a feladatátvevő.

### <a name="view-and-manage-vm-properties"></a>Megtekintheti és kezelheti a virtuális tulajdonságai

Azt javasoljuk, hogy ellenőriznie kell a forrás gép tulajdonságait. Ne feledje, hogy az Azure virtuális neve meg kell felelnie, [Azure virtuális gép](site-recovery-best-practices.md#azure-virtual-machine-requirements)követelményeknek.

1. Kattintson a **Beállítások** > **Védett elemek** > **Replikált elemek** >, és válassza a gép a részletek megjelenítéséhez.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. **Tulajdonságok** megtekintheti a virtuális replikációs és feladatátvevő adatait.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. A **számítási és a hálózati** > ,**Tulajdonságok kiszámítania** megadhatja, hogy az Azure virtuális nevét és a cél méretét. Módosítsa a név [Azure követelmények](site-recovery-best-practices.md#azure-virtual-machine-requirements) betartása, ha módosítani szeretné. Is megtekintheti, és módosítsa az adatokat a cél hálózati, alhálózat és IP-címet, amely a Azure virtuális hozzá van rendelve. Vegye figyelembe az alábbiakat:

    - Beállíthatja, hogy a cél IP-címet. Ha Ön nem adja meg egy címet, a sikertelen fölé gépi DHCP fogja használni. Ha meg, amely nem érhető el, feladatátvételkor, a feladatátvételi sikertelen lesz. Az azonos target IP-cím próba feladatátvételi használható, ha a cím érhető el a próba feladatátvevő hálózaton.
    - A hálózati adaptereken számát határozza meg a méretét, adja meg a cél virtuális a számítógépen, az alábbi képlettel történik:

        - A forrás számítógép hálózati adaptereken értéke kisebb vagy egyenlő adaptereken engedélyezett a cél gépi méretét a száma, majd a cél van adaptereken ugyanannyi forrásaként.
        - Ha a forrás virtuális gép kártya száma meghaladja a megengedett a tároló méretét, majd használja a cél maximális.
        - Ha például egy adatforrás számítógépre van két hálózati adaptereken és a cél gépi mérete négy támogatja, a cél gép két adaptereken lesz. Ha a forrás számítógépben két adaptert, de a támogatott cél mérete csak akkor támogatja a egy a cél gép csak egy kártya lesz.     
        - Ha a virtuális fogja az összes csatlakoznak ugyanabba a hálózatba több hálózati adaptereken.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  A **lemez** megtekintheti a virtuális fog kell replikált az operációs rendszer és az adatok lemezt.



## <a name="step-7-test-your-deployment"></a>7 lépés: A telepítés tesztelése

Tesztelje a telepítő futtatását is lehetővé teszi a próba feladatátvevő egy virtuális egyetlen számítógépre vagy a helyreállítási terv, amely tartalmazza az egy vagy több virtuális gépeken futó.


### <a name="prepare-for-failover"></a>Feladatátvevő előkészítése

- Javasoljuk, hogy a Azure gyártási hálózatról (Ez az alapértelmezett működés Azure-ban egy új hálózati létrehozásakor) hoz létre, amely magában elszigetelt új Azure hálózat tesztelése feladatátvevő futtatásához. [Tudjon meg többet](site-recovery-failover.md#run-a-test-failover) : a próba feladatátadás futtatása.
- A legjobb teljesítmény elérése érdekében jelenik meg átadni Azure, telepítse az Azure ügynök a védett gépen. Gyorsabb betöltése teszi azt, és a hibaelhárítási segítséget nyújt. Telepítse a [Linux](https://github.com/Azure/WALinuxAgent) vagy [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -ügynök.
- Teljesen tesztelje a üzembe van szüksége a replikált gép a várt módon működnek-infrastruktúra alakítható ki. Ha meg szeretné vizsgálni, az Active Directory és a DNS-virtuális gép létrehozása a tartományvezérlőnek a DNS-ben, és az Azure-webhely helyreállítás Azure való replikáció ez. Olvassa el a további a [próba feladatátvevő szempontjait az Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Ha a használni kívánt feladatátvételhez próba feladatátvevő helyett vegye figyelembe az alábbiakat:

    - Ha lehetséges állítsa le a elsődleges gépek feladatátvételhez futtatása előtt. Ez biztosítja, hogy nincs-e a forrás- és a replika gépeken futó operációs rendszert futtató, egy időben.
    - Feladatátvételhez futtatásakor az megáll a adatok replikációs az elsődleges gépek, bármilyen adat delta nem vihetők, után feladatátvételhez kezdődik. Ezenkívül ha feladatátvételhez helyreállítási csomag fog futni mindaddig, amíg befejeződik, még akkor is, ha hiba történik.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Felkészülés a csatlakozás Azure VMs feladatátvétel után

Ha szeretne csatlakozni az Azure VMs RDP segítségével feladatátvétel után, győződjön meg arról, hogy tegye a következőket:

**A helyszíni gépen átadása előtt**:

- Az access az interneten keresztül RDP engedélyezése, győződjön meg arról, hogy TCP- és UDP szabályok kerülnek, az a **nyilvános**, és győződjön meg arról, hogy RDP engedélyezett-e a **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások** minden profilhoz.
- A webhely kapcsolaton keresztül hozzáférés engedélyezése a RDP a számítógépen, és győződjön meg arról, hogy RDP engedélyezett-e a **Windows tűzfal** -> **engedélyezett alkalmazások és szolgáltatások** **tartomány** és a **saját** hálózatokhoz.
- Telepítse az [Azure virtuális ügynök](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) a helyszíni gépen.
- Győződjön meg arról, hogy az operációs rendszer SAN házirend értéke OnlineAll. [tudj meg többet]( https://support.microsoft.com/kb/3031135)
- Kapcsolja ki az IPSec szolgáltatás, a feladatátvételi futtatása előtt.

**Kattintson az Azure virtuális feladatátvétel után**:

- Adja hozzá a nyilvános végpont RDP-protokoll (port 3389), és adja meg a bejelentkezési hitelesítő adatait.
- Gondoskodjon arról, hogy nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.
- Próbáljon meg csatlakozni. Ha nem tud csatlakozni, ellenőrizze, hogy fut-e a virtuális. További hibaelhárítási tippeket szól ez a [cikk](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Ha szeretne hozzáférni az Azure virtuális Linux futtatása után feladatátvevő biztonságos rendszerhéj ügyfélprogram (ssh), tegye a következőket:

**A helyszíni gépen átadása előtt**:

- Győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatást a Azure virtuális rendszer indításakor automatikus indításúként van beállítva.
- Ellenőrizze, hogy tűzfalszabályokat engedélyezése egy SSH kapcsolatot vele.

**Kattintson az Azure virtuális feladatátvétel után**:

- A hálózati biztonsági csoport szabályokat a virtuális fölé, amelyhez csatlakozik az Azure alhálózat sikertelen engedélyeznie kell a SSH porthoz bejövő kapcsolatokkal.
- Egy nyilvános végpontot kell létrehozni, hogy engedélyezze a bejövő kapcsolatokat az SSH port (TCP-port 22 alapértelmezés szerint).
- Ha a virtuális a virtuális Magánhálózati kapcsolat (Express útvonal vagy webhelyre történő VPN) keresztül érhető el, majd az ügyfél használható segítségével közvetlenül csatlakozik a virtuális SSH keresztül.


### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

A próba feladatátvevő ne a következő futtatása:

1. Egy egyetlen virtuális **beállításai**átadni > **Replikált elemeket**, kattintson a virtuális > **+ próba feladatátvevő**.
2. Helyreállítási csomagra, **Beállítások**átadni > **Helyreállítási tervek**, kattintson a jobb gombbal a terv > **Próba feladatátvevő**. Hozhat létre a helyreállítás tervet, [kövesse az alábbi lépéseket](site-recovery-create-recovery-plans.md).

3. **Teszt című** témakörében jelölje ki a Azure hálózat, amelyhez Azure VMs csatlakozás után feladatátadás.
4. Kattintson **az OK** gombra a feladatátvételi megkezdéséhez. A virtuális tulajdonságainak megnyitása vagy a **Próba feladatátvevő** feladat, a **Beállítások**gombra kattintva haladásának nyomon követhető > **webhely helyreállítási feladatokat**.
5. Amikor a feladatátvételi eléri a **teljes tesztelés** fázis, tegye a következőket:

    1. A replika virtuális gép megtekintése az Azure-portálon. Győződjön meg arról, hogy a virtuális gép sikeresen elindul.
    2. Ha access virtuális gépeken futó felfelé beállítása a helyszíni hálózatról a virtuális géphez távoli asztali kapcsolaton kezdeményezhet.
    3. Kattintson **a vizsgálat teljes** befejezéséhez azt.
    4. Kattintson a **jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.
    5. Kattintson **a vizsgálat feladatátvételi befejeződött**. Az automatikus kapcsolja ki és törölje a próba virtuális gép tesztkörnyezetben karbantartása.
    6. Ebben a szakaszban azok az elemek és automatikusan létrehozott webhely helyreállítási a próba feladatátvételkor VMs törlődnek. További elemek próba feladatátvételi létrehozott nem törlődnek.

    > [AZURE.NOTE] Ha a teszt feladatátvétel két héttel már továbbra is által hatályba befejeződik.

6. A feladatátvételi befejeződése után is kell láthatja az Azure replika gépi jelennek meg az Azure-portálon > **virtuális gépeken futó**. Győződjön meg róla, hogy a virtuális megfelelő méretű, a megfelelő hálózathoz csatlakozik, és futó.
7. Ha meg [készített kapcsolatok feladatátvétel után](#prepare-to-connect-to-Azure-VMs-after-failover) látnia kell a Azure virtuális csatlakozni.


## <a name="monitor-your-deployment"></a>A telepítési figyelése

Itt látható, miként figyelheti a beállításokat, az állapot és az állapot a webhely helyreállítási telepítéshez:

1. Kattintson a **Essentials** irányítópult eléréséhez tárolóra nevére. Az irányítópult azt is megteheti webhely helyreállítási feladatok, replikációs állapotát, a helyreállítási terv, kiszolgáló állapotának és eseményeket.  A csempék és az elrendezésekre vonatkozó lehetőségek, amelyek a leghasznosabb, beleértve a más webhely helyreállítási és biztonsági másolat tárolókban állapotának megjelenítése Essentials is testre szabhatja.

    ![Alapjai](./media/site-recovery-vmm-to-azure/essentials.png)

2. Az **állapot** csempére figyelheti a webhely kiszolgálók (VMM vagy konfigurációja), amely tapasztalt probléma megoldásához, és a webhely helyreállítási keletkezett az elmúlt 24 óra az eseményeket.
3. Kezelése és a Lync-replikáció **Replikált elemek**, **Helyreállítási tervek**, és a **Webhely helyreállítási feladatok** csempék. Feladatok **beállításai**részletezve -> **feladatok** -> **Webhely helyreállítási feladatokat**.


## <a name="next-steps"></a>Következő lépések

A telepítés után be van állítva, és működik, [megtudhatja, hogy](site-recovery-failover.md) a különböző típusú feladatátvevő.
