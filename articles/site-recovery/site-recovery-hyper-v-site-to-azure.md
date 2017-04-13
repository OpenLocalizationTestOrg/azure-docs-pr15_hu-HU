<properties
    pageTitle="A Hyper-V virtuális gépeken futó (nélkül VMM) bizonyos az Azure webhely helyreállítási használata az Azure portál Azure |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure webhely helyreállítási való replikáció, feladatátvétel és a helyszíni a Hyper-V VMs, amely nem kezeli az Azure az Azure portálon VMM visszanyerése téve terjesztése"
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
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>A Hyper-V virtuális gépeken futó (nélkül VMM) bizonyos az Azure Azure webhely helyreállítási használata az Azure-portálra

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-hyper-v-site-to-azure.md)
- [Azure klasszikus](site-recovery-hyper-v-site-to-azure-classic.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-deploy-with-powershell-resource-manager.md)



Üdvözli a Azure webhely helyreállítási! Ez a cikk, ha azt szeretné, hogy a helyszíni a Hyper-V virtuális replikáció gépek, hogy **nem** kezelése a System Center virtuális gépeken futó Manager (VMM) felhőket Azure való használatát. Ez a cikk ismerteti, hogy miként állíthatja be a replikáció Azure webhely helyreállítás segítségével az Azure-portálon.

> [AZURE.NOTE] Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása.

> Azure webhely helyreállítási támogatja a helyreállítási és a Hyper-V virtuális gépeken futó az Azure áttelepítését. Az ebben a cikkben leírt lépéseket azonos vészhelyreállítás vagy való áttelepítés Azure való replikáció beállításakor alkalmazása VMs Azure

Az Azure-portálon Azure webhely helyreállítási számos új szolgáltatást tartalmaz:

- Az Azure-ban portál, Azure biztonsági mentése és Azure webhely helyreállítási szolgáltatások soroljuk egy egyetlen helyreállítási szolgáltatások tárolóból elemre, hogy állíthat be és kezelheti a üzemkészségének és a egyetlen helyről (BCDR) helyreállítás. Az egyesített irányítópult lehetővé teszi, hogy figyelheti, és a tevékenységek kezelése a helyszíni webhelyek és a Azure nyilvános felhőben.
- A felhasználók a felhőben megoldás szolgáltató (CSP) program kiépítve Azure előfizetések webhely helyreállítási műveletek az Azure-portálon kezelheti.
- Az Azure-portálon webhely helyreállítási hogy bizonyos gépek erőforrás-kezelő tároló fiókokhoz. Feladatátvételkor webhely helyreállítási erőforrás-kezelő-alapú VMs Azure hoz létre.
- Webhely helyreállítási klasszikus tároló fiókok és a klasszikus telepítési modell használata VMs feladatátvételének való replikáció támogatása továbbra is.


Miután ez a cikk bejegyzést olvas a visszajelzését Disqus megjegyzések szakaszában a képernyő alján. Kérje meg a technikai kérdések a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>– Áttekintés


Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia tartani az üzleti adatok, a megbízható és a helyreállítható, és biztosítsa, hogy munkaterhelésekből folyamatosan elérhető katasztrófa fordul elő.

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, végződhet keresztül az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Vissza a fő tartózkodási helyén a sikertelen amikor visszatér a szokásos műveletek. További információ a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

Ez a cikk a helyszíni a Hyper-V virtuális bizonyos kell minden információt gépek, hogy **nem** kezelhetők az Azure System Center virtuális gépeken futó Manager (VMM) felhőket tartalmaz. Építészeti áttekintést tervezési információk és a helyszíni kiszolgálók, Azure, egy replikációs házirend és kapacitás tervezés konfigurációs telepítési lépéseket tartalmazza. Miután beállította a infrastruktúra védelemmel ellátni kívánt gépeken replikáció engedélyezése, és hajtsa végre a próba áttérni a beállítási ellenőrzése. Is áttelepítheti a VMs Azure hajt végre, a tervezett feladatátvétel, és véglegesítse a az áttelepítést.

## <a name="business-advantages"></a>Üzleti előnyei

- Helyszínen (Azure) feladatátvevő nyújt a vállalati munkaterhelésének és a Hyper-V virtuális gépeken futó alkalmazásokat.
- Egy egyetlen helyreállítási szolgáltatások konzol biztosít egyszerű beállítása és kezelése a replikáció, feladatátvevő és helyreállítási folyamat.
- Lehetővé teszi, hogy a helyszíni infrastruktúra feladatátadás futtatásával Azure, és fail-back (visszaállítás) az Azure a helyszíni webhelyet.
- Beállíthatja a helyreállítási terv több számítógépre, úgy, hogy a közös átveszi többszintű alkalmazás munkaterhelésekből.

## <a name="scenario-architecture"></a>Forgatókönyv architektúra

Ezek a forgatókönyv összetevőket:

- **A Hyper-V host vagy fürt**: A helyszíni a Hyper-V kiszolgálók vagy fürt. A védelemmel ellátni kívánt VMs fut a Hyper-V állomások logikai a Hyper-V helyeken összegyűjtése történik a webhely helyreállítási a telepítés során.
- **Azure webhely helyreállítási szolgáltatója és helyreállítási szolgáltatások agent**: a telepítés során telepíti az Azure webhely helyreállítási szolgáltató és a Microsoft Azure helyreállítási szolgáltatási ügynökök a Hyper-V kiszolgálók. A szolgáltató kommunikál Azure webhely helyreállítási HTTPS 443-as való replikáció üzletifolyamat-tervező fölé. A Hyper-V kiszolgálón agent adatait replikálja Azure tároló fölé HTTPS 443-as alapértelmezés szerint.
- **Azure**: van szüksége az Azure előfizetéssel, típusú adatokat tárolja replikált Azure tároló fiók és egy Azure virtuális hálózat, hogy az Azure VMs feladatátvétel után hálózathoz csatlakozik.

![A Hyper-V webhely-architektúra](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

Az alábbiakban amire szüksége lesz az Azure ebben az esetben telepítéséhez.

**Előfeltételek** | **Részletek**
--- | ---
**Azure-fiók**| Akkor, ha a [Microsoft Azure](http://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.
**Azure tárhely** | Egy szabványos tárterület-fiókkal kell. LRS vagy GRS tároló fiók is használhatja. GRS javasoljuk, hogy a adatokat rugalmas egy területi üzemszünetek esetén, vagy ha az elsődleges régió sem állíthatók. [Tudjon meg többet](../storage/storage-redundancy.md). A fiók kell lennie az ugyanazon régió, a helyreállítási szolgáltatások tárolóból elemre.<br/><br/> Prémium tároló használata nem támogatott.<br/><br/> Azure tároló replikált adatok vannak tárolva, és Azure VMs létrehozásakor feladatátadás.<br/><br/> [Információ](../storage/storage-introduction.md) Azure tárhely.
**Azure hálózati** | Szüksége van egy Azure virtuális hálózat, amely az Azure VMs fog csatlakozni feladatátadás. A helyreállítási szolgáltatások tárolóra azonos régió az Azure virtuális hálózati kell lennie.

## <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

Amire szüksége lesz az alábbiakban a helyszíni.

**Előfeltételek** | **Részletek**
--- | ---
**A Hyper-V** | Egy vagy több helyszíni kiszolgálón a **Windows Server 2012 R2** legújabb frissítésekkel és a Hyper-V szerepkör engedélyezve vagy **Microsoft a Hyper-V Server 2012 R2**.<br/><br/>A Hyper-V server tartalmaznia kell egy vagy több virtuális gépeken futó.<br/><br/>A Hyper-V kiszolgálók kell csatlakoznia az interneten, vagy közvetlenül egy proxyn keresztül.<br/><br/>A Hyper-V kiszolgálók javítások említett [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") telepítve vannak.
**Szolgáltató és agent** | Azure webhely helyreállítási a telepítés során az Azure webhely helyreállítási szolgáltató kell telepítenie. A szolgáltatót telepíteni az Azure helyreállítási szolgáltatások ügynök minden a Hyper-V kiszolgálón futó virtuális gépeken futó védelemmel ellátni kívánt telepíti. Egy webhely helyreállítási tárolóból elemre az összes a Hyper-V kiszolgálón van, hogy ugyanazt a szolgáltató és ügynök.<br/><br/>A szolgáltató kell Azure webhely helyreállítási csatlakoztatása az interneten keresztül. Lehet küldeni a forgalmat, vagy közvetlenül egy proxyn keresztül. Figyelje meg, hogy a HTTPS-alapú proxy nem támogatott. A proxykiszolgáló engedélyezze a hozzáférést: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>A kiszolgálón Ha IP-cím alapú tűzfal szabályokat, ellenőrizze, hogy a szabályok kommunikációt Azure. [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS (443-as) port lehetővé kell.<br/><br/>Engedélyezi az IP-címtartományok az Azure a régió, az előfizetése, és a nyugati US.

## <a name="protected-machine-prerequisites"></a>Védett számítógép vonatkozó követelmények


**Előfeltételek** | **Részletek**
--- | ---
**Védett VMs** | Nem sikerül egy virtuális fölé előtt kell győződjön meg arról, hogy a nevet, amelyet jogokat kap az Azure virtuális [Azure Előfeltételek](site-recovery-best-practices.md#azure-virtual-machine-requirements)megfelel-e. Miután engedélyezte a virtuális replikációs módosíthatók a nevét.<br/><br/> Egyes kapacitásával védett gépeken 1023 GB-nál több kerülni a Webhelyfiókok lehet. A virtuális beállíthatja, hogy legfeljebb 16 lemez (tehát legfeljebb 16 TB).<br/><br/> A megosztott lemez Vendég fürt nem támogatott.<br/><br/> Ha a forrás virtuális hálózati kártya csoportos együttműködési azt alakul át egy egyetlen hálózati Azure való áttérés után<br/><br/>Védelme VMs Linux operációs rendszert futtató statikus IP-címmel használata nem támogatott.

## <a name="prepare-for-deployment"></a>Felkészülés a telepítéshez

Felkészülés a telepítési kell:

1. [Az Azure-hálózat beállítása](#set-up-an-azure-network) , amelyben Azure VMs kerül létrehozásakor azok is feladatátvétel után.
2. [Azure tároló fiók beállítása](#set-up-an-azure-storage-account) replikált adatokat.
3. [Felkészülés a Hyper-V hosts](#prepare-the-hyper-v-hosts) elérhetik a szükséges URL-címek biztosítása érdekében.

### <a name="set-up-an-azure-network"></a>Az Azure-hálózat beállítása

Az Azure-hálózat beállítása. Meg kell, hogy az Azure VMs létrehozott után feladatátvevő hálózathoz csatlakozik.

- A hálózat ugyanabban a régióban, mint, amelyben a helyreállítási szolgáltatások tárolóra fogja üzembe kell lennie.
- Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült ugyanúgy tudja beállítani az [Erőforrás-kezelő](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) vagy [Klasszikus üzemmódban](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)Azure hálózat.
- Azt javasoljuk, hogy Ön a hálózat beállítása első lépések. Ha Ön nem kell tennie, webhely helyreállítási a telepítés során.

> [AZURE.NOTE] Webhely helyreállítási pontról hálózatok [hálózatok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott.

### <a name="set-up-an-azure-storage-account"></a>Azure tároló fiók beállítása

- Azure replikált adatok tárolására egy szabványos Azure tárterület-fiókkal kell.
- Attól függően, hogy az erőforrás modell használni kívánt az Azure VMs keresztül nem sikerült a fiók beállítása fogja az [Erőforrás-kezelő](../storage/storage-create-storage-account.md) vagy [Klasszikus üzemmódban](../storage/storage-create-storage-account-classic-portal.md).
- Azt javasoljuk, hogy a tárhely fiók beállítása első lépések. Ha Ön nem kell tennie, webhely helyreállítási a telepítés során. A fiókok kell ugyanabban a régióban, mint a helyreállítási szolgáltatások tárolóból elemre.

> [AZURE.NOTE] [Tárterület-fiókok áttelepítése](../resource-group-move-resources.md) az azonos előfizetésben erőforráscsoport vízszintesen vagy a előfizetésekben nem támogatott üzembe helyezése a webhely helyreállítási használt tárterület-fiókokat.

### <a name="prepare-the-hyper-v-hosts"></a>Felkészülés a Hyper-V hosts

- Győződjön meg arról, hogy a Hyper-V hosts megfeleljenek a [Előfeltételek](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Management** > **biztonsági mentése és a webhely helyreállítási (MOBILE)**. Másik lehetőségként kattinthat, **tallózással keresse meg** > **Helyreállítási szolgáltatások** tárolókban > **Hozzáadás gombra**.

    ![Új tárolóból elemre](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. **Neve** mezőjében adja meg egy rövid nevet, amely azonosítja a tárolóból elemre. Ha egynél több előfizetése van, válasszon közülük.
4. [Új erőforrás csoport létrehozása](../resource-group-template-deploy-portal.md) vagy jelöljön ki egy meglévőt, és adjon meg egy Azure régiót. Gépek fog kell replikált a területre. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége
4. Ha azt szeretné, hogy gyorsan elérhesse a tárolóból elemre az irányítópult kattintson a **Rögzítés a irányítópult** , és kattintson a **Létrehozás tárolóból elemre**.

    ![Új tárolóból elemre](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

Az új tárolóra jelenik meg az **Irányítópult** > **összes erőforrás**, és a fő **helyreállítási szolgáltatások tárolókban** lap.

## <a name="getting-started"></a>Első lépések

Webhely helyreállítási biztosít a Bevezetés eszköz, amely segít, hogy minél gyorsabban telepítése. Első lépések Előfeltételek ellenőrzi, és bemutatja az webhely helyreállítási telepítési lépéseket a megfelelő sorrendben.

Első lépések, jelölje ki a gépek való replikáció kívánt, és a kívánt helyre való replikáció. Beállította a helyszíni kiszolgálók, az Azure tároló fiókok és a hálózatok. Replikációs házirendek létrehozása, és végezze el a tervezési kapacitása. Miután beállította a infrastruktúra engedélyeznie VMs a replikáció. Futtassa az adott gépekhez feladatátadás, vagy több gépek átadni a helyreállítási terv létrehozása.

Válassza a webhely helyreállítási üzembe módját kezdje el az első lépések. Az első lépések áramlás kissé attól függően, hogy a replikáció követelmények változik.



## <a name="step-1-choose-your-protection-goals"></a>Lépés: 1: Válassza ki a védelem célok

Jelölje be a Mit szeretne való replikáció és a kívánt helyre való replikáció.

1. A **tárolókban helyreállítási szolgáltatások** lap jelölje ki a tárolóból elemre, és kattintson a **Beállítások**gombra.
2. A **Beállítások** > **Első lépések** kattintson a **Webhely helyreállítási** > **lépés 1: Felkészülés infrastruktúra** > **védelem cél**.

    ![Válassza a kitűzött célok](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. A **védelem cél** jelölje ki **Az Azure**, és válassza az **Igen, a Hyper-V**. Válassza a **nincs** nem használja az VMM megerősítéséhez. Kattintson **az OK**gombra.

    ![Válassza a kitűzött célok](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Lépés: 2: A forrás-környezet beállítása

Állítsa be a Hyper-V webhelyet, telepítse az Azure webhely helyreállítási szolgáltató és az Azure helyreállítási szolgáltatások ügynök a Hyper-V hosts és a hosts rögzítheti a tárolóból elemre.


1. Kattintson a **lépés: 2: Felkészülés infrastruktúra** > **forrás**. Vegye fel a Hyper-V új webhely elhelyezése a Hyper-V hosts vagy fürt, kattintson a **+ Hyper-V webhelyet**.

    ![Adatforrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. A **webhely létrehozása a Hyper-V** lap adja meg a webhely nevét. Kattintson **az OK**gombra. Jelölje ki az imént létrehozott webhely.

    ![Adatforrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Válassza a **+ Hyper-V Server** kiszolgáló hozzáadása a webhelyhez.
4. A **Kiszolgáló hozzáadása** > **kiszolgálótípus** ellenőrizze, hogy látható-e a **Hyper-V server** . Győződjön meg arról, hogy a Hyper-V kiszolgáló fel szeretné venni megfelel-e a [Előfeltételek](#on-premises-prerequisites) , és elérheti a megadott URL-ek.
4. Töltse le az Azure webhely helyreállítási szolgáltató telepítőfájl. Ez a fájl, a szolgáltató, mind a helyreállítási szolgáltatások agent telepítheti a Hyper-V állomásokon futtatásával.
5. Töltse le a regisztrációs billentyűt. Ez a telepítő futtatásakor mobiltelefon szükséges. 5 nap után hoz létre, akkor a kulcs nem érvényes.

    ![Adatforrás beállítása](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Futtassa a szolgáltató telepítőfájlt a Hyper-V webhelyet felvett minden állomáson. Ha telepíti a Hyper-V fürthöz, futtassa a telepítő minden csomóponton. Telepítése és minden egyes a Hyper-V fürt csomópontok regisztrálása biztosítja, hogy virtuális gépeken futó védett marad-e, ha az azok áttelepítése csomópontok keresztül.

### <a name="install-the-provider-and-agent"></a>Telepítse a szolgáltató és agent

1. A szolgáltató telepítőfájl futtatása.
2. A **Microsoft Update** választhatja a frissítések keresése, hogy a szolgáltató frissítések telepítése a Microsoft Update irányelvek szerint.
3. A **telepítés** fogadja el vagy szolgáltató telepítés helyének módosítása, és kattintson a **telepítés**gombra.
4. **Tárolóból elemre a beállítások** lapon kattintson a **Tallózás** gombra a tárolóból elemre kulcs letöltött fájlt. Adja meg az Azure webhely helyreállítási előfizetés, a tárolóból elemre nevét és a Hyper-V webhelyre, amelyhez a Hyper-V kiszolgáló tartozik.

    ![Kiszolgáló regisztráció](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5.a **Proxybeállításokat** adja meg, hogyan a szolgáltató a kiszolgálón telepített Azure webhely helyreállítási az interneten keresztül csatlakozás lesz.

- Ha azt szeretné, hogy a szolgáltató közvetlenül csatlakozni kattintson a **Csatlakozás közvetlenül a proxy nélkül**.
- Ha azt szeretné, szeretne kapcsolatba lépni a proxy, amely jelenleg beállítása a kiszolgálón kattintson a **Csatlakozás a meglévő proxybeállításokat**.
- Ha a meglévő proxy-hitelesítést igényel, vagy egy egyéni proxy **kapcsolatba lépni a proxybeállítások egyéni**szolgáltatót a kapcsolat válassza a használni kívánt.
- Ha egy egyéni proxy kell adnia a címet, port és hitelesítő adatok
- Használata a proxy győződjön meg arról, hogy az URL-címeit, a [vonatkozó követelmények](#on-premises-prerequisites) ismertetett engedélyezett keresztül.

    ![internetes](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. telepítés befejezése után kattintson a **regisztráció** kiszolgáló regisztrálása a tárolóból elemre.

![Telepítési hely](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7 regisztrációs befejezése a metaadat-alapú után a Hyper-V server Azure webhely helyreállítási által beolvasott, és megjelenik a kiszolgáló **beállításai** > **Webhely helyreállítási infrastruktúra** > **A Hyper-V Hosts** lap.


### <a name="command-line-installation"></a>Parancssori telepítése

Az Azure webhely helyreállítási szolgáltatója és agent is lehet telepíteni a következő parancsot használja. Ez a módszer a szolgáltató telepítése a kiszolgáló Core a Windows Server 2012 R2 használható.

1. Töltse le a szolgáltató telepítési fájl és a regisztrációs kulcs mappára. Ha például C:\ASR.
2. A rendszergazda jogú parancssort, ezek a parancsok, ki kell olvasni a szolgáltató telepítő futtatása:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Ezt a parancsot a összetevők telepítése:

            C:\ASR> setupdr.exe /i

4. Futtassa az ezek a parancsok, a kiszolgáló regisztrálása a tárolóból elemre: CD-re C:\Program Files\Microsoft Azure webhely helyreállítási Provider\
            C:\Program Files\Microsoft Azure webhely helyreállítási szolgáltató\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /hitelesítő adatok<path of the credentials file>

<br/>
Ha:

- **/Credentials** : kötelező paraméter, amely a bejegyzés fő fájlt a helye  
- **/FriendlyName** : a kiszolgáló nevének a Hyper-V host az Azure webhely helyreállítási portálon megjelenő paramétert kötelező megadni.
- **/proxyAddress** : választható paraméter, amely megadja a címét a proxykiszolgáló.
- **/proxyport** : választható paraméter, amely megadja a port a proxykiszolgáló.
- **/proxyUsername** : választható paraméter, amely a Proxy felhasználónév (Ha a proxy-hitelesítést igényel).
- **/proxyPassword** : választható paraméter, amely a proxykiszolgáló használata hitelesítése (Ha a proxy-hitelesítést igényel) jelszava.


## <a name="step-3-set-up-the-target-environment"></a>3 lépés: A Céllista környezet beállítása

Adja meg a replikáció és Azure VMs fog csatlakozni feladatátvétel után az Azure hálózati használandó Azure tároló fiókot.

1.  Kattintson az **Előkészítés infrastruktúra** > **cél** , és válassza ki a használni kívánt Azure előfizetést.
2.  Adja meg a telepítési modellt feladatátvétel után VMs használni szeretne.
3.  Webhely helyreállítási ellenőrzi, hogy van-e egy vagy több kompatibilis Azure tároló fiókok és a hálózatok.

    ![Tárhely](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Ha eddig nem hozott létre a egy tárterület-fiókot, és hozzon létre egy újat szeretne erőforrás-kezelővel **+ tárterület** -fiók elemre kattintva válasszon, hogy szövegközi. Egy fióknevet, típusa, előfizetés és helyét adja meg a **tárterület-fiók létrehozása** lap. A fiók a helyreállítási szolgáltatások tárolóra ugyanazon a helyen kell lennie.

    ![Tárhely](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Ha létre szeretne hozni egy tárterület-fiókját a Klasszikus modell el fogja, hogy [az Azure-portálon](../storage/storage-create-storage-account-classic-portal.md).

5.  Ha eddig nem hozott létre a egy Azure hálózat, és hozzon létre egy újat szeretne **+ hálózat** , beágyazott elvégzendő erőforrás-kezelővel gombra. Adja meg a **virtuális hálózat létrehozása** lap nevet a hálózatnak, címtartományokat, alhálózat adatait, előfizetést, és helyét. A hálózat a helyreállítási szolgáltatások tárolóra ugyanazon a helyen kell lennie.

    ![Hálózati](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Ha létre szeretne hozni egy hálózati klasszikus modellt használja a fogja, hogy [az Azure-portálon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)el.


## <a name="step-4-set-up-replication-settings"></a>Lépés: 4: Beállítása replikációs beállításai

1. Hozzon létre egy új replikációs házirend kattintva **Előkészítés infrastruktúra** > **Replikációs beállításai** > **+ létrehozása és az társítása**.

    ![Hálózati](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. **Létrehozás és társítása házirend** adja meg a házirend nevére.
3. **Másolja a gyakoriság** adja meg, hogy milyen gyakran való replikáció delta adatok a kezdeti replikáció (30 másodpercenként, 5-ös vagy 15 perc) után.
4. A **helyreállítási pont adatmegőrzési**adja meg, órában mennyi ideig az adatmegőrzési ablak az egyes helyreállítási pont. Védett gépek ablak bármely pontjára állíthatók helyre.
6. Az **alkalmazás egységes pillanatkép gyakoriság** adja meg, milyen gyakran (1 – 12 óra) tartalmazó alkalmazás egységes pillanatképek helyreállítási pontok létrehozott. A Hyper-V használja kétféle pillanatképek –, ahol egy teljes virtuális gépen növekményes pillanatképét szabványos pillanatkép, és az alkalmazás adatok belül a virtuális gép pont és az idő pillanatfelvételt-alkalmazás egységes pillanatfelvételek. Alkalmazás egységes pillanatképek kötet szolgáltatás (Árnyékmásolata) használja annak érdekében, hogy alkalmazásokat konzisztens állapotban, amikor a pillanatfelvétel származik. Figyelje meg, hogy ha bekapcsolja az alkalmazás egységes pillanatképek, befolyásolja a forrás virtuális gépeken futó alkalmazások teljesítményét. Győződjön meg arról, hogy beállított értéke kisebb, mint konfigurálnia további helyreállítási pontok száma.
3. Adja meg a **Kezdeti replikációs kezdetének** mikor induljon el a kezdeti replikáció. A replikáció az internet-sávszélesség fölé, érdemes lehet az elfoglalt órákat kívüli ütemezze. Kattintson **az OK**gombra.

    ![Replikációs házirend](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Amikor létrehoz egy új házirendet, a Hyper-V webhely automatikusan ki van társítva. Kattintson az **OK gombra**. Több replikációs házirend **-beállításai**a Hyper-V webhely (és a benne lévő VMs) is társíthat > **replikációs** > házirend neve > **Társítani a Hyper-V webhelyet**.

## <a name="step-5-capacity-planning"></a>Lépés az 5: Kapacitás tervezése

Most, hogy van-e a basic infrastruktúra állíthat be, akkor is kapacitás tervezés és azt tudni, hogy szüksége a további erőforrások.

Webhely helyreállítási biztosít a kapacitás Csapattervező segít a forrás környezetben, webhely helyreállítási összetevői, hálózati és tárolására szolgáló jobb oldali erőforrásokat. A Csapattervező módszerekkel becsült VMs, a lemez és a tárhely átlagos száma alapján a gyors módban, vagy részletes módban, amelyben a terhelést szintjén ábrák fogja bemeneti futtathatja. Mielőtt elkezdené kell:

- A replikáció környezetben, többek között a VMs, per VMs lemezt, és egy lemezen tárolási információkat gyűjthet.
- Becsüljük napi módosítása (tejeskanna) a replikált adatokat is. A [Hyper-V kópia kapacitás Csapattervező](https://www.microsoft.com/download/details.aspx?id=39057) segít ezt is használhatja.

1.  Kattintson a **Letöltés** töltse le az eszközt, majd futtatásával gombra. [A cikkben olvasható](site-recovery-capacity-planner.md) az eszköz olvashatja el.
2.  Ha végzett válassza az **Igen lehetőséget** **futtatta a kapacitás Csapattervező**?

    ![Kapacitás tervezése](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Hálózati sávszélesség kapcsolatos szempontok

A kapacitás Csapattervező eszközzel kiszámítása a replikáció (kezdeti replikációs, majd delta) szükséges sávszélességet. A replikáció sávszélesség-használat szabályozhatják néhány lehetőség áll rendelkezésére:

- **Sávszélességet**: Azure történő másolásához a Hyper-V forgalmat a Hyper-V adott állomáson keresztül kerül. A sávszélesség a kiszolgálón is szabályozása.
- **Az animáció működését sávszélesség**:, befolyásolhatja a sávszélesség-replikáció beállításkulcsainak néhány használatával használja.

#### <a name="throttle-bandwidth"></a>A sávszélesség szabályozása

1. Nyissa meg a Microsoft Azure biztonsági másolat MMC beépülő modul a Hyper-V kiszolgálón. Alapértelmezés szerint a Microsoft Azure biztonsági másolat használható érhető el az asztal vagy a C:\Program Files\Microsoft Azure helyreállítási szolgáltatások Agent\bin\wabadmin.
2. A beépülő modult a **Tulajdonságainak módosítása**gombra.
3. A **Throttling** lapon jelölje be az **internetes sávszélesség-használat biztonsági műveletekhez szabályozásának engedélyezése**, és a munka korlátozások, és nem munka óra. 512 KB és másodpercenként 102 MB van érvényes tartományok.

    ![A sávszélesség szabályozása](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

A [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag használatával szabályozásának beállítása. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** azt jelzi, hogy nincs szabályozásának szükséges.


#### <a name="influence-network-bandwidth"></a>Befolyással sávszélességének

1. A beállításjegyzékben nyissa meg azt a **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Befolyásolhatja a sávszélesség-forgalmat replikáló lemezen, módosítsa a értékét a **UploadThreadsPerVM**, vagy ha még nem létezik, hozza létre a kulcsot.
    - Befolyásolhatja a sávszélesség visszaállás forgalom az Azure, módosítania kell a **DownloadThreadsPerVM**értéket.
2. Az alapértelmezett érték a 4. Egy "overprovisioned" hálózat a beállításkulcsok meg kell változtatni, az alapértelmezett értékeket. A maximális érték 32. Lync-forgalmat optimalizálhatja az értéket.

## <a name="step-6-enable-replication"></a>Lépés a 6: Engedélyezése replikációs

Most lehetővé teszi a replikáció az alábbi képlettel történik:

1. Kattintson a **lépés 2: alkalmazás bizonyos** > **forrás**. Miután engedélyezte a replikáció az első alkalommal fogja a **+ bizonyos** a replikáció további gépekhez engedélyezéséhez a tárolóból elemre kattintson.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. Kattintson a **forrás** lap > jelölje be a Hyper-V webhelyet. Kattintson **az OK**gombra.
3. **Cél** kijelölése a tárolóból elemre előfizetést, és a feladatátvétel után (klasszikus vagy az erőforrás-kezelés) Azure-ban használni kívánt feladatátvevő modell
4. Jelölje ki a használni kívánt tárterület-fiókot. Ha azt szeretné, mint a másik tárterület-fiókkal rendelkezik, [Hozzon létre egy újat](#set-up-an-azure-storage-account)is. **Új létrehozása**kattintva hozhat létre a tárterület-fiókba az erőforrás-kezelő modell. Ha létre szeretne hozni egy tárterület-fiókját a Klasszikus modell el fogja, hogy [az Azure-portálon](../storage/storage-create-storage-account-classic-portal.md). Kattintson **az OK**gombra.
5.  Jelölje be az Azure hálózat, amelyhez Azure VMs fog csatlakozni, ha az azok esetén is be feladatátvétel után alhálózat. Válassza a **Konfigurálás most kijelölt gépekhez** hálózati beállítás alkalmazása az összes gépekhez ki védelmét. Válassza a **Beállítás később** jelölje ki az Azure hálózati per gépi. A másik hálózathoz használni kívánt esetén akkor is, [Hozzon létre egy újat](#set-up-an-azure-network). Az erőforrás-kezelő adatmodellt használó hálózat létrehozásához kattintson az **Új létrehozása**. Ha létre szeretne hozni egy hálózati klasszikus modellt használja a fogja, hogy [az Azure-portálon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)el. Jelölje ki a alhálózat, ha van ilyen. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. **Virtuális**gépeken futó > **Jelölje ki a virtuális gépeken futó** kattintson, és válassza a minden kívánt való replikáció gépet. Jelölje ki a replikáció engedélyezhető gépek csak. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. A **Tulajdonságok** > **tulajdonságainak beállítása**, jelölje be az operációs rendszer a kijelölt VMs és a rendszer lemez. Ellenőrizze, hogy Azure virtuális név (cél) megfelel [Azure virtuális gép](site-recovery-best-practices.md#azure-virtual-machine-requirements) , és módosítsa a azt, ha módosítani szeretné. Kattintson **az OK**gombra. Beállíthatja, hogy további tulajdonságot később.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. **Replikációs**beállításai > **replikációs beállítások**csoportban jelölje ki a replikáció házirendet a védett VMs alkalmazni szeretné. Kattintson **az OK**gombra. A replikáció házirend **-beállításai**módosíthatók > **replikációs házirendek** > házirend neve > **Beállítások szerkesztése elemre**. A módosítások alkalmazása már replikálja gépek, és új gépek használható.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

A **Védelem bekapcsolása** feladat **beállításai**végrehajtásának nyomon követhető > **feladatok** > **webhely helyreállítási feladatokat**. A **Védelem véglegesítése** feladat futtatása a gép után van készen áll a feladatátvevő.

### <a name="view-and-manage-vm-properties"></a>Megtekintheti és kezelheti a virtuális tulajdonságai

Azt javasoljuk, hogy ellenőriznie kell a forrás gép tulajdonságait.

1. Kattintson a **Beállítások** > **Védett elemek** > **Replikált elemek** >, és válassza ki a gépet.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. **Tulajdonságok** megtekintheti a virtuális replikációs és feladatátvevő adatait.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. A **számítási és a hálózati** > ,**Tulajdonságok kiszámítania** megadhatja, hogy az Azure virtuális nevét és a cél méretét. Módosítsa a név Azure követelmények betartása, ha módosítani szeretné. Is megtekintheti, és módosítsa az adatokat a cél hálózati, alhálózat és jogokat kap az Azure virtuális IP-címet. Vegye figyelembe az alábbiakat:

    - Beállíthatja, hogy a cél IP-címet. Ha Ön nem adja meg egy címet, a sikertelen fölé gépi DHCP fogja használni. Ha meg, amely nem érhető el, feladatátvételkor, a feladatátvételi sikertelen lesz. Az azonos target IP-cím próba feladatátvételi használható, ha a cím érhető el a próba feladatátvevő hálózaton.
    - A hálózati adaptereken számát határozza meg a méretét, adja meg a cél virtuális a számítógépen, az alábbi képlettel történik:

        - A forrás számítógép hálózati adaptereken értéke kisebb vagy egyenlő adaptereken engedélyezett a cél gépi méretét a száma, majd a cél van adaptereken ugyanannyi forrásaként.
        - Ha a forrás virtuális gép kártya száma meghaladja a megengedett a tároló méretét, majd a cél maximális használt.
        - Ha például egy adatforrás számítógépre van két hálózati adaptereken és a cél gépi mérete négy támogatja, a cél gép két adaptereken lesz. Ha a forrás számítógépben két adaptert, de a támogatott cél mérete csak akkor támogatja a egy a cél gép csak egy kártya lesz.     
        - Ha a virtuális fogja az összes csatlakoznak ugyanabba a hálózatba több hálózati adaptereken.

    ![A replikáció engedélyezése](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  A **lemez** megtekintheti a virtuális fog kell replikált az operációs rendszer és az adatok lemezt.


## <a name="step-7-test-the-deployment"></a>7 lépés: A telepítés tesztelése

Tesztelje a telepítő futtatását is lehetővé teszi a próba feladatátvevő egy virtuális egyetlen számítógépre vagy a helyreállítási terv, amely tartalmazza az egy vagy több virtuális gépeken futó.


### <a name="prepare-for-test-failover"></a>Teszt feladatátvevő előkészítése

- Javasoljuk, hogy a Azure gyártási hálózatról (Ez az alapértelmezett működés Azure-ban egy új hálózati létrehozásakor) hoz létre, amely magában elszigetelt új Azure hálózat tesztelése feladatátvevő futtatásához. [Tudjon meg többet](site-recovery-failover.md#run-a-test-failover) : a próba feladatátadás futtatása.
- A legjobb teljesítmény elérése érdekében jelenik meg átadni Azure, telepítse az Azure ügynök a védett gépen. Gyorsabb betöltése teszi azt, és a hibaelhárítási segítséget nyújt. Telepítse a [Linux](https://github.com/Azure/WALinuxAgent) vagy [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -ügynök.
- Teljesen tesztelje a üzembe szüksége lesz a replikált gép a várt módon működnek-infrastruktúra alakítható ki. Ha meg szeretné vizsgálni, az Active Directory és a DNS-virtuális gép létrehozása a tartományvezérlőnek a DNS-ben, és az Azure-webhely helyreállítás Azure való replikáció ez. Olvassa el a további a [próba feladatátvevő szempontjait az Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
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

- Egy nyilvános IP-cím hozzáadása a társított a Azure virtuális engedélyezni RDP hálózati.
- Gondoskodjon arról, hogy nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.
- Próbáljon meg csatlakozni. Ha nem tud kapcsolatba ellenőrizze, hogy fut-e a virtuális. További hibaelhárítási tippeket szól ez a [cikk](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Ha szeretne hozzáférni az Azure virtuális Linux futtatása után feladatátvevő biztonságos rendszerhéj ügyfélprogram (ssh), tegye a következőket:

**A helyszíni gépen átadása előtt**:

- Győződjön meg arról, hogy a biztonságos rendszerhéj szolgáltatást a Azure virtuális rendszer indításakor automatikus indításúként van beállítva.
- Ellenőrizze, hogy tűzfalszabályokat engedélyezése egy SSH kapcsolatot vele.

**Kattintson az Azure virtuális feladatátvétel után**:

- A hálózati biztonsági csoport szabályokat a virtuális fölé, amelyhez csatlakozik az Azure alhálózat sikertelen engedélyeznie kell a SSH porthoz bejövő kapcsolatokkal.
- Egy nyilvános végpontot kell létrehozni, hogy engedélyezze a bejövő kapcsolatokat az SSH port (TCP-port 22 alapértelmezés szerint).
- Ha a virtuális a virtuális Magánhálózati kapcsolat (Express útvonal vagy webhely VPN) keresztül érhető el, majd az ügyfél használható segítségével közvetlenül csatlakozik a virtuális SSH keresztül.

### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

A próba feladatátvevő ne a következő futtatása:

1. Egy egyetlen virtuális **beállításai**átadni > **Replikált elemeket**, kattintson a virtuális > **+ próba feladatátvevő**.

    ![Teszt feladatátvevő](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Helyreállítási csomagra, **Beállítások**átadni > **Helyreállítási tervek**, kattintson a jobb gombbal a terv > **Próba feladatátvevő**. Hozhat létre a helyreállítás tervet, [kövesse az alábbi lépéseket](site-recovery-create-recovery-plans.md).

3. **Teszt című** témakörében jelölje ki a Azure hálózat, amely Azure VMs is csatlakoznak feladatátadás után.

    ![Teszt feladatátvevő](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Kattintson **az OK** gombra a feladatátvételi megkezdéséhez. A virtuális kattintva nyissa meg a properies, vagy a **Próba feladatátvevő** feladat, a **Beállítások**gombra kattintva haladásának nyomon követhető > **webhely helyreállítási feladatokat**.
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


## <a name="failover"></a>Feladatátvevő

Kezdeti replikációs befejezése után a gépekhez, hívhatja feladatátadás, szükség szerint. Webhely helyreállítási feladatátadás - próba feladatátvevő, a tervezett feladatátvevő és a tervezett feladatátvevő különböző típusú támogatja.
[Tudjon meg többet](site-recovery-failover.md) a különböző típusú feladatátadás és részletes leírást, hogy mikor és hogyan végezhetők el mindegyiket.

> [AZURE.NOTE] Ha a leképezés Azure virtuális gépeken futó áttelepítéséhez, azt ajánljuk, hogy a [tervezett feladatátvevő művelet](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) segítségével a virtuális gépeken futó áttelepítése az Azure. Miután az áttelepített alkalmazás érvényesítése használata próba feladatátvevő Azure-ban, [Kész áttelepítési](#Complete-migration-of-your-virtual-machines-to-Azure) említett lépésekkel hozhatja létre a virtuális gépeken futó áttelepítése befejezéséhez. Nem kell a jóváhagyás vagy a törlés végrehajtásához. Kész áttelepítési az áttelepítés befejeződött, a védelmet a virtuális gép eltávolítja, és leállítja a gép Azure webhely helyreállítási számlázását.


###<a name="run-an-unplanned-failover"></a>Feladatátvételhez futtatása

Meg kell kiválasztani, amikor egy elsődleges hely elérhetetlenné válik miatt nem várt esemény, például a power üzemszünetek, illetve vírus támadások. Ez az eljárás ismerteti, hogyan "feladatátvételhez" helyreállítási tervhez futtatásához. Másik lehetőségként a feladatátvételi egyetlen virtuális géphez futtathatja a virtuális gépeken futó lapon. Mielőtt nekikezdene, ellenőrizze az összes a virtuális gépeken futó átadni kívánt befejeződött a kezdeti replikáció.

1. Válassza a **helyreállítási tervek > recoveryplan_name**.
2. Kattintson a helyreállítás a Tervezés lap, **Nem tervezett feladatátvételre**.
3. A **tervezett feladatátvevő** lapon válassza ki a forrás- és célwebhelyek helyeken.
4. Jelölje ki a **virtuális gépeken futó leállíthatja, és a legfrissebb adatok szinkronizálása** adja meg, hogy webhely helyreállítási próbálja meg a védett virtuális gépeken futó leállítás, és szinkronizálja az adatokat, hogy a legújabb az adatok felett sikertelen lesz.
5. A feladatátvétel, után a virtuális gépeken futó szerepelnek, a jóváhagyás állapota függőben.  Kattintson a **Jóváhagyás** véglegesítse a feladatátvételi gombra.

[tudj meg többet](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Töltse ki a virtuális gépeken futó az Azure áttelepítése


>[AZURE.NOTE] Az alábbi lépéseket csak akkor, ha telepít át az Azure virtuális gépeken futó alkalmazása

1. Ténykedés tervezett feladatátvevő említett [Itt](site-recovery-failover.md)
2. A **beállításai > elemek replikált**, kattintson a jobb gombbal a virtuális gép, és válassza a **Kész áttelepítési**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Kattintson az **OK gombra** az áttelepítés végrehajtásához. Kattintson a virtuális tulajdonságainak megnyitása vagy a teljes áttelepítési feladat használatával nyomon követhető előrehaladását **Beállítások > webhely helyreállítási feladatok**.


## <a name="monitor-your-deployment"></a>A telepítési figyelése

Itt látható, miként figyelheti a beállításokat, az állapot és az állapot a webhely helyreállítási telepítéshez:

1. Kattintson a **Essentials** irányítópult eléréséhez tárolóra nevére. Az irányítópult azt is megteheti webhely helyreállítási feladatok, replikációs állapotát, a helyreállítási terv, kiszolgáló állapotának és eseményeket.  A csempék és az elrendezésekre vonatkozó lehetőségek, amelyek a leghasznosabb, beleértve a más webhely helyreállítási és biztonsági másolat tárolókban állapotának megjelenítése Essentials is testre szabhatja.

    ![Alapjai](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. **Állapot** csempéjén figyelheti a webhely-kiszolgálók, amelyek tapasztalt probléma megoldásához, és a webhely helyreállítási keletkezett az elmúlt 24 óra az eseményeket.
3. Kezelése és a Lync-replikáció **Replikált elemek**, **Helyreállítási tervek**, és a **Webhely helyreállítási feladatok** csempék. Feladatok **beállításai**részletezve -> **feladatok** -> **Webhely helyreállítási feladatokat**.
