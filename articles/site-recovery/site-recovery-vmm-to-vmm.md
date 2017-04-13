<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket a replikáció az Azure portálon másodlagos VMM webhelyre |} Microsoft Azure"
    description="Azure webhely helyreállítási való replikáció, feladatátvétel és VMM felhőket a Hyper-V VMs helyreállítása az Azure portálon másodlagos VMM webhelyre téve üzembe ismerteti."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>A Hyper-V virtuális gépeken futó VMM felhőket a replikáció az Azure portálon másodlagos VMM webhelyre

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-vmm.md)
- [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Üdvözli a Azure webhely helyreállítási! Ez a cikk, ha azt szeretné, hogy való replikáció helyszíni a Hyper-V virtuális gépeken futó kezelése a másodlagos webhelyre System Center virtuális gép Manager (VMM) felhőket használatát. Ez a cikk ismerteti, hogy miként állíthatja be a replikáció Azure webhely helyreállítás segítségével az Azure-portálon.

> [AZURE.NOTE] Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása.


Az Azure-portálon Azure webhely helyreállítási számos új szolgáltatásokat nyújtja:

- Az Azure-ban portál Azure biztonsági mentése és az Azure webhely helyreállítási szolgáltatások soroljuk egyetlen helyreállítási szolgáltatások tárolóból elemre, így Ön beállítása és kezelése üzemkészségének és a egyetlen helyről (BCDR) helyreállítás. Az egyesített irányítópult figyelésére és tevékenységek kezelése a helyszíni webhelyek és a Azure nyilvános felhő teszi lehetővé.
- A felhasználók a felhőben megoldás szolgáltató (CSP) program kiépítve Azure előfizetések webhely helyreállítási műveletek az Azure-portálon kezelheti.


A cikket elolvasva után tartalmakat tehet közzé esetleges megjegyzéseket, a képernyő alján a Disqus megjegyzések. Kérje meg a technikai kérdések a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>– Áttekintés

Szervezetek van szüksége a BCDR stratégia, amely azt határozza meg, hogyan alkalmazások, feladatok és adatok fut, és a rendelkezésre álló kapcsolatának során tervezett és a tervezett legrövidebb leállás és helyreállítása a normál munka feltétel minél korábban beállítást. A BCDR vonatkozó stratégia kell az üzleti adatok hagyja, biztonságos és helyreállítható, és győződjön meg róla, munkaterhelésekből által folyamatosan elérhető katasztrófa bekövetkezésekor.

Webhely-helyreállítás egy Azure szolgáltatás, amely helyszíni fizikai kiszolgálók és virtuális gépeken futó a felhőbe (Azure) vagy egy másodlagos adatközponthoz való replikáció orchestrating által a BCDR vonatkozó stratégia beleszámít. Kimaradások a fő tartózkodási helyén lévő előfordulásakor, átadni az alkalmazások és a rendelkezésre álló munkaterhelésekből másodlagos helyre. Nem sikerül visszatérhet a fő tartózkodási helyén amikor visszatér a szokásos műveletek. További információ a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

Ebben a cikkben a replikáció kell minden információt a helyszíni VMM felhőket másodlagos VMM webhelyre a Hyper-V VMs. Építészeti áttekintést tervezési információk és a helyszíni kiszolgálók, a replikációs beállításai és a kapacitás tervezés konfigurációs telepítési lépéseket tartalmazza. Miután beállította a infrastruktúra engedélyezheti replikációs gépeken védelme, és ellenőrizze, hogy feladatátvevő működik.

## <a name="business-advantages"></a>Üzleti előnyei

- Webhely helyreállítási üzleti munkaterhelésének és a Hyper-V VMs megjegyzésekről őket a Hyper-V másodlagos kiszolgálón úgy futó alkalmazások védelmet biztosít.
- A helyreállítási Services portál beállítása, kezelése és figyelje a replikáció, feladatátvevő és helyreállítási egyetlen helyről.
- Futtathatja egyszerűen futtatása feladatátadás a helyszíni elsődleges infrastruktúra a másodlagos webhelyet, és visszaállás (visszaállítás) az elsődleges, a másodlagos webhelyről.
- Beállíthatja a helyreállítási terv több számítógépre, úgy, hogy a közös átveszi többszintű alkalmazás munkaterhelésekből.

## <a name="scenario-architecture"></a>Forgatókönyv architektúra

Ezek a forgatókönyv összetevőket:

- **Elsődleges hely**: a elsődleges hely egy vagy több a Hyper-V host kiszolgálón a forrás VMs szeretné való replikáció. Ezek elsődleges kiszolgálók VMM magánjellegű felhő találhatók.
- **Másodlagos webhely**: egy vagy több a Hyper-V host rendszert futtató kiszolgálók, az elsődleges VMs bizonyos célalkalmazás VMs a másodlagos webhelyen vannak. A host kiszolgálók VMM magánjellegű felhő találhatók. Az a felhő lehet az elsődleges kiszolgálón (Ha csak egyetlen VMM kiszolgáló) vagy a másodlagos VMM kiszolgálón.
- **Szolgáltató**: webhely helyreállítási a telepítés során telepítse az Azure webhely helyreállítási szolgáltató VMM kiszolgálókon, és azokat a kiszolgálókat regisztrálása az egy helyreállítási szolgáltatások tárolóból elemre. A szolgáltató a VMM kiszolgálón futó kommunikál webhely helyreállítási HTTPS 443-as való replikáció üzletifolyamat-tervező fölé. Adatok replikáció elsődleges és másodlagos a Hyper-V kiszolgálók között. Replikált adatok belül helyszíni webhelyek és a hálózatok marad, és nem küldi Azure. További információ a [adatvédelmi](#privacy-information-for-site-recovery).

![E2E topológiája](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Adatok adatvédelmi – áttekintés

Az alábbi táblázat összefoglalja, hogyan adatok tárolásának ebben az esetben:
****
Művelet | **Részletek** | **Gyűjtött adatok** | **Használata** | **Szükséges**
--- | --- | --- | --- | ---
**Regisztráció** | VMM kiszolgáló rögzítheti a helyreállítási szolgáltatások tárolóból elemre. Ha később szeretné a kiszolgáló unregister, ezt az Azure portálról a kiszolgáló adatainak törlésével teheti meg. | Miután regisztrált egy VMM kiszolgáló webhely helyreállítási által gyűjtött, folyamatok, és továbbítja a VMM kiszolgáló és a webhely helyreállítási észlelni VMM felhőket azoknak a metaadat-alapú. | Az adatok azonosítása és a megfelelő VMM felhőket beállításainak konfigurálása és a megfelelő VMM kiszolgáló kommunikálni szolgál. | Ez a funkció szükség. Ha nem szeretné az információk küldése webhely helyreállítási, a webhely helyreállítási szolgáltatás kerülni a Webhelyfiókok használatát.
**A replikáció engedélyezése** | Az Azure webhely helyreállítási szolgáltató telepítve van a VMM kiszolgálón és a továbbítás kommunikáció a webhely helyreállítás szolgáltatással. A szolgáltató dinamikus csatolású függvénytár (DLL) is a VMM folyamatban. A szolgáltató telepítése után a "Adatközponthoz helyreállítási" funkció VMM felügyeleti konzolban kap engedélyezett. Új és meglévő VMs engedélyezheti ahhoz, hogy egy virtuális védelme funkció. | Ez a tulajdonság csoportjával a szolgáltató küld a nevét, és a virtuális Azonosítóját webhely helyreállítási.  Windows Server 2012-ben vagy Windows Server 2012 R2 Hyper-V replika engedélyezve van a replikáció. A virtuális gép adatokat kapja replikált egyik számítógépről a Hyper-V között (általában egy másik "helyreállítási" adatközpont található). | Webhely helyreállítási a metaadatok használja a virtuális adatokat az Azure-portálon feltöltése. | Ez a funkció nem kapcsolható ki és alapvető része a szolgáltatást. Ha nem kívánja elküldeni ezt az információt, nem engedélyezi az VMs webhely helyreállítási védelme. Figyelje meg, hogy a webhely helyreállítási szolgáltató által küldött összes adat HTTPS küldi.
**Helyreállítási terv** | Helyreállítási tervek segítségével állíthatja össze a helyreállítási adatközpont üzletifolyamat-tervező csomagjával. Megadhatja, hogy a sorrendben, amelyben VMs vagy egy virtuális gépeken futó csoportja el kell indítani a helyreállítási webhelyén. Megadhatja, hogy minden automatikus parancsfájlok futtatása vagy minden kézi végrehajtandó műveletet, az egyes virtuális helyreállítási időpontjában lesz. Feladatátvevő általában induljanak összehangolt helyreállítási helyreállítási terv szintjén. | Webhely helyreállítási által gyűjtött, folyamatok és a helyreállítási tervet, beleértve a virtuális gép metaadatok és automatizálási parancsfájlokat és a kézi művelet jegyzetek metaadat-alapú metaadatait továbbítja. | A metaadatok az Azure-portálon helyreállítási terv szolgál. | Ez a funkció nem kapcsolható ki és alapvető része a szolgáltatást. Ha nem szeretné az információk küldése webhely helyreállítási, nem hoz létre a helyreállítási tervek.
**Hálózati hozzárendelést** | Térképek hálózati adatait az elsődleges adatközpont helyreállítási adatközpontja. VMs helyreállítási a helyszínen van vissza, ha hálózati hozzárendelést segít a hálózati kapcsolat létrehozásáról. | Webhely helyreállítási által gyűjtött, folyamatok és továbbítja az egyes webhely (elsődleges és adatközponthoz) logikai hálózatok a metaadatok. | A metaadat-alapú hálózat beállításainak, hogy a hálózati adatok képezhető feltöltéséhez használják. | Ez a funkció nem kapcsolható ki és alapvető része a szolgáltatást. Ha nem szeretné az információk küldése webhely helyreállítási, hálózati hozzárendelést nem használja.
**Feladatátvevő (tervezett/tervezett/próba)** | Feladatátvevő átvétele VMs egy VMM kezelt adatok központból között. A feladatátvevő művelet manuális induljanak az Azure-portálon. | A szolgáltató VMM kiszolgálói webhely helyreállítási értesíti a feladatátvevő esemény, és egy feladatátvevő művelet futtatja a Hyper-V állomáson VMM felületeken keresztül. A virtuális tényleges feladatátvételének egyik számítógépről a Hyper-V egy másik és kezelése a Windows Server 2012-ben vagy Windows Server 2012 R2 Hyper-V replika van. Feladatátvevő befejeződése után a szolgáltató az helyreállítási adatközpontban VMM kiszolgálói adatokat küld a sikeres webhely helyreállítási. | Webhely helyreállítási állapotát a feladatátvevő művelet adatokat az Azure-portálon feltöltése küldött adatok használja. | Ez a funkció nem kapcsolható ki és alapvető része a szolgáltatást. Ha nem szeretné az információk küldése webhely helyreállítási, ne használjon feladatátvevő.


## <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

Az alábbiakban kell Azure-ban ebben az esetben telepítése:

**Előfeltételek** | **Részletek**
--- | ---
**Azure**| A [Microsoft Azure](http://azure.microsoft.com/) -fiókra van szüksége. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.


## <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

Az alábbiakban kell a helyszíni elsődleges és másodlagos webhelyek üzembe ebben az esetben:

**Előfeltételek** | **Részletek**
--- | ---
**VMM** | Azt javasoljuk, hogy egy VMM az elsődleges webhelyen és egy VMM kiszolgáló kiegészítő helyén rendszerbe.<br/><br/> Is [bizonyos egy VMM kiszolgálón felhő között](site-recovery-single-vmm.md). Ehhez a VMM kiszolgálón beállított legalább két felhőket van szüksége.<br/><br/> VMM kiszolgálók legalább futnia kell System Center 2012 SP1 a legújabb frissítésekkel együtt.<br/><br/> Minden VMM kiszolgáló egyik van, vagy további felhőket beállítva, és minden felhő rendelkeznie kell a Hyper-V kapacitásának profil beállítása. <br/><br/>Felhőket ábrázoló kell tartalmaznia egy vagy több VMM host csoportot.<br/><br/>További tudnivalók [a VMM felhő háló konfigurálása](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), VMM felhőket beállítása és [Útmutató: hoz létre saját felhőket System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM kiszolgálók internet-hozzáférés szükséges.
**A Hyper-V** | A Hyper-V kiszolgálók legalább futnia kell a Windows Server 2012 a Hyper-V szerepkörrel és telepítve van a legújabb frissítéseket.<br/><br/> A Hyper-V server tartalmaznia kell egy vagy több VMs.<br/><br/>  Az elsődleges és másodlagos VMM felhőket ábrázoló host csoportok a Hyper-V kiszolgálók kell lennie.<br/><br/> Ha a Hyper-V fürt t futtat a Windows Server 2012 R2 telepítenie kell [2961977 frissítése](https://support.microsoft.com/kb/2961977)<br/><br/> A Hyper-V fürt futtatja a Windows Server 2012, ne feledje, hogy fürt ügynök statikus IP cím alapú fürt esetén automatikusan létrehozott nem. Meg kell fürt közvetítő manuális beállítását. [További információ:](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Szolgáltató** | Webhely helyreállítási a telepítés során az Azure webhely helyreállítási szolgáltató VMM kiszolgálókon telepítése. A szolgáltató kommunikál webhely helyreállítási HTTPS 443-as való replikáció téve fölé. Adatok replikáció a helyi hálózati vagy a virtuális Magánhálózati kapcsolat elsődleges és másodlagos a Hyper-V kiszolgálók között.<br/><br/> A szolgáltató a VMM kiszolgálón futó az alábbi URL-hozzáférésre van szüksége: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Ezenkívül tűzfal kommunikációt a VMM-kiszolgálókról az [Azure adatközpont IP-tartományokat](https://www.microsoft.com/download/confirmation.aspx?id=41653) , és lehetővé teszi a (443-as) HTTPS protokollt.

## <a name="prepare-for-deployment"></a>Felkészülés a telepítéshez

Felkészülés a telepítési kell:

1. [Felkészülés a VMM server](#prepare-the-vmm-server) webhely helyreállítási telepítéshez.
2. [Felkészülés a hálózati leképezéshez](#prepare-for-network-mapping). Hálózatok beállítása, hogy a hálózati hozzárendelést beállíthatja.

### <a name="prepare-the-vmm-server"></a>A VMM kiszolgáló előkészítése

Győződjön meg arról, hogy a VMM kiszolgáló megfelel-e a [Előfeltételek](#on-premises-prerequisites) és férhet hozzá a felsorolt URL-címek.


### <a name="prepare-for-network-mapping"></a>Hálózati hozzárendelést előkészítése

Hálózati megfeleltetés térképek kiszolgálókon az elsődleges és másodlagos VMM VMM virtuális hálózatok között:

- Optimálisan helyezze replika VMs a másodlagos a Hyper-V hosts feladatátvétel után.
- Csatlakozás replika VMs megfelelő virtuális hálózatok.
- Ha nem adja meg hálózati replika leképezése VMs nem csatlakozik olyan hálózat feladatátvétel után.
- A hálózat beállítása a webhely helyreállítás során leképezése az alábbi telepítési szükség szükségesek:

    - Győződjön meg arról, hogy a Hyper-V host forráskiszolgáló VMs VMM virtuális hálózathoz csatlakozik. A hálózat kell kötni logikai a felhőben társított hálózathoz.
    - Ellenőrizze, hogy a másodlagos felhő helyreállítási használt rendelkezik-e a megfelelő virtuális hálózatnak. Virtuális hálózat kell kötni logikai hálózat, amely a másodlagos felhőben van társítva.

- [Tudjon meg többet](site-recovery-network-mapping.md) a hálózati hozzárendelést működéséről.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Környezet, amelyben az egyetlen VMM kiszolgáló előkészítése

Ha csak egyetlen VMM kiszolgáló, hogy bizonyos VMs a Hyper-V hosts VMM felhőbeli [Azure](site-recovery-vmm-to-azure.md) vagy másodlagos VMM felhő. Azt javasoljuk, hogy az első lehetőséget, mert a felhő között replikálja nem folytonos, de ha itt van szüksége mit kell tennie:

1. **Állítsa be a Hyper-V virtuális a VMM**. Ha hajtanak végre, kattintson a azonos virtuális VMM által használt SQL Server-példányt colocate javasoljuk. Ezzel időt takaríthat meg, ha csak egy virtuális létrehozni. Használni kívánt SQL Server-üzemszünetek távoli példányának fordul elő, ha meg kell helyreállítása az adott példányhoz, mielőtt VMM visszaállíthatja.
2. **Ellenőrizze a VMM kiszolgáló van konfigurálva legalább két felhőket**. Egy felhőalapú fogja tartalmazni a VMs való replikáció szeretne, és a többi felhő másodlagos helyeként szolgálja. A felhő, amely tartalmazza a védelemmel ellátni kívánt VMs meg kell feleljen az [Előfeltételek](#on-premises-prerequisites).
3. Állítsa be a webhely helyreállítási, a jelen cikkben leírt módon. Hozzon létre és VMM kiszolgáló regisztrálása a tárolóból elemre, a replikáció házirend beállítása és replikáció engedélyezése. Adjon meg, hogy a kezdeti replikáció esedékes a hálózaton keresztül.
4. Hálózati hozzárendelést beállításakor a virtuális hálózat az elsődleges felhő megfeleltetése a másodlagos felhő a virtuális hálózathoz.
5. A Hyper-V kezelője konzolban a Hyper-V replika engedélyezése a Hyper-V állomáson, amely tartalmazza a VMM virtuális, és engedélyezze a virtuális a replikáció. Ellenőrizze, hogy a webhely helyreállítási annak érdekében, hogy a Hyper-V replika beállítások nem bírálni webhely helyreállítási eső felhőket nem hozzáadása a VMM virtuális gépen.
6. Ha hoz létre a helyreállítási tervek feladatátvételi ugyanazon VMM kiszolgáló-et forrás- és célwebhelyek.
7. Nem sikerül fölé, és helyreállítása az alábbiak szerint:

    - Manuális átveszi a VMM virtuális a Hyper-V kezelője használata tervezett feladatátvevő másodlagos webhelyet.
    - Sikertelen a VMs fölé.
    - Az elsődleges VMM virtuális helyreállítása megtörtént, jelentkezzen be az Azure portál -> után helyreállítási szolgáltatások tárolóból elemre és futtatása a VMs feladatátvételhez a másodlagos webhelyről az elsődleges webhelyre.
    - A tervezett feladatátvételt befejeződése után összes erőforrás webböngészőn keresztül elérhetők az elsődleges webhely újra.


### <a name="create-a-recovery-services-vault"></a>Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Management** > **helyreállítási szolgáltatások**. Másik lehetőségként kattinthat, **tallózással keresse meg** > **Helyreállítási szolgáltatások** tárolókban > **Hozzáadás gombra**.

    ![Új tárolóból elemre](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. **Neve** mezőjében adja meg egy rövid nevet, amely azonosítja a tárolóból elemre. Ha egynél több előfizetése van, válasszon közülük.
4. [Új erőforrás csoport létrehozása](../resource-group-template-deploy-portal.md) vagy jelöljön ki egy meglévőt, és adjon meg egy Azure régiót. Gépek fog kell replikált a területre. Jelölje be a támogatott régiók lásd: [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/) adatokat földrajzi elérhetősége
4. Ha azt szeretné, hogy gyorsan elérhesse a tárolóból elemre az irányítópult kattintson a **Rögzítés a irányítópult** > **létrehozása tárolóból elemre**.

    ![Új tárolóból elemre](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

Az új tárolóra jelenik meg az **Irányítópult** > **összes erőforrás**, és a fő **helyreállítási szolgáltatások tárolókban** lap.




## <a name="getting-started"></a>Első lépések

Webhely helyreállítási biztosít a Bevezetés eszköz, amely segít, hogy minél gyorsabban telepítése. Első lépések Előfeltételek ellenőrzi, és bemutatja az webhely helyreállítási telepítési lépéseket a megfelelő sorrendben.

Első lépések, jelölje ki a gépek való replikáció kívánt, és a kívánt helyre való replikáció. Állítsa be a helyszíni kiszolgálók, replikációs házirendek létrehozása és kapacitás tervezés elvégzéséhez. Miután beállította a infrastruktúra lehetősége van engedélyezni a VMs replikációs. Futtassa az adott gépekhez feladatátadás, vagy több gépek átadni a helyreállítási terv létrehozása.

Válassza a webhely helyreállítási üzembe módját kezdje el az első lépések. Az első lépések áramlás kissé attól függően, hogy a replikáció követelmények változik.

## <a name="step-1-choose-your-protection-goal"></a>Lépés: 1: Válassza ki a védelem céljához kapcsolódó ábra

Jelölje be a Mit szeretne való replikáció és a kívánt helyre való replikáció.

1. A **tárolókban helyreállítási szolgáltatások** lap jelölje ki a tárolóból elemre, és kattintson a **Beállítások**gombra.
2. A **Beállítások** > **Első lépések** kattintson a **Webhely helyreállítási** > **lépés 1: Felkészülés infrastruktúra** > **védelem cél**.
3. A **védelem cél** válassza a **helyreállítás webhelyre**, és válassza az **Igen, a Hyper-V**.
4. Válassza az **Igen** , hogy a Hyper-V állomások kezelésére, és válassza az **Igen lehetőséget** , ha rendelkezik egy másodlagos VMM VMM használ. Ha egy VMM kiszolgálón felhőket közötti replikáció esetén telepítését **nem**gombra kattint. Kattintson **az OK**gombra.

    ![Válassza a kitűzött célok](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Lépés: 2: A forrás-környezet beállítása

Telepítse az Azure webhely helyreállítási szolgáltató VMM kiszolgálókon, és a kiszolgáló regisztrálása a tárolóból elemre.

1. Kattintson a **lépés: 2: Felkészülés infrastruktúra** > **forrás**.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Az **Előkészítés forrás** kattintson a **+ VMM** VMM kiszolgáló hozzáadása elemre.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. A **Kiszolgáló hozzáadása** lap ellenőrizze, hogy a **System Center VMM server** **kiszolgáló típusa** , és megjelenik az a VMM kiszolgáló megfelel-e a [Előfeltételek és URL-cím követelményeknek](#on-premises-prerequisites).
4. Töltse le az Azure webhely helyreállítási szolgáltató telepítőfájl.
5. Töltse le a regisztrációs billentyűt. Van szüksége a telepítő futtatásakor. 5 nap után hoz létre, akkor a kulcs nem érvényes.

    ![Adatforrás beállítása](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Telepítse az Azure webhely helyreállítási szolgáltató a VMM kiszolgálón.

> [AZURE.NOTE] Nem kell semmit explicit módon telepíthető a Hyper-V kiszolgálók.


### <a name="set-up-the-azure-site-recovery-provider"></a>Az Azure webhely helyreállítási szolgáltató beállítása

1. A szolgáltató VMM-kiszolgálókon telepítőfájl futtatása. Ha VMM fürt telepíti, és a Provider for telepíti az első alkalommal telepítse az active csomópontot, és a VMM kiszolgáló regisztrálása a tárolóból elemre a telepítés befejezéséhez. Telepítse a szolgáltató a csomóponton. Fürt csomópontok összes működnie kell a szolgáltató azonos verzióban.
2. A telepítő néhány prerequirement szempontú vizsgálatok futtatása és VMM szolgáltatás leállítása engedélyt kér. A VMM szolgáltatás automatikusan újraindul telepítés befejezése után. Ha telepíti a kérni fogja a fürt szerepkör leállítása VMM fürthöz.

2.  A **Microsoft Update** választhatja a frissítések keresése, hogy a szolgáltató frissítések telepítése a Microsoft Update irányelvek szerint.
3. A **telepítés** fogadja el vagy szolgáltató telepítés helyének módosítása, és kattintson a **telepítés**gombra.

    ![Telepítési hely](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Telepítés befejezése után kattintson a **regisztráció** kiszolgáló regisztrálása a tárolóból elemre.

    ![Telepítési hely](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. **Név tárolóból elemre**ellenőrizze a tárolóból elemre a kiszolgáló nyilvántartásba nevét. Kattintson a *Tovább*gombra.

    ![Kiszolgáló regisztráció](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. **Internetkapcsolat esetén** adja meg, hogyan a szolgáltató a VMM kiszolgálón futó csatlakozik az internethez. Kattintson a **Csatlakozás a meglévő proxybeállításokhoz** a kiszolgálón beállított alapértelmezett internetes kapcsolat beállításait.

    ![Internetes beállításai](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Ha egy egyéni proxy használni kívánt állítson be azt a szolgáltatót telepítése előtt. Egyéni proxybeállításokat konfigurálásakor teszten ellenőrizni a proxy kapcsolat fog futni.
    - Ha egyéni proxykiszolgálót használ, vagy az alapértelmezett proxy meg kell adnia a proxy részleteket, például a proxycím és port hitelesítést igényel.
    - Következő URL-címek a VMM kiszolgáló és a Hyper-v hosts elérhető legyen
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Engedélyezi az IP-címek [Azure adatközpont IP-tartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és HTTPS (443) protokoll ismertetett. Azt szeretné, hogy fehér-lista IP-címtartományai az Azure-terület, amelyet használni szeretne használata és a nyugati US.
    - Ha egy VMM RunAs fiók (DRAProxyAccount) használatával a megadott proxyhoz tartozó automatikusan létrejön egy egyéni proxy. A proxykiszolgáló beállítása, hogy ezt a fiókot sikeresen hitelesíthet A VMM RunAs fiókbeállításokat a VMM konzolban módosítható. Ehhez nyissa meg a **beállításokat** a munkaterület, bontsa ki a **biztonsági**, kattintson a **Futtatás fiókok**, és módosítsa a jelszót az DRAProxyAccount. Indítsa újra a VMM szolgáltatást, így ez a beállítás érvénybe kell.


8. **Regisztrációs billentyűt**jelölje ki az Azure webhely helyreállítási letöltött, és másolja a VMM kiszolgáló billentyűt.


10.  A titkosítási beállítást csak akkor használja, ha Ön éppen esetében, amelyek a Hyper-V VMs VMM felhőket az Azure. Ha Ön éppen esetében, amelyek egy másodlagos webhely nem használatos.

11.  A **kiszolgáló nevét**adja meg a tárolóra VMM kiszolgálójába azonosítása egy rövid nevet. A fürt konfiguráció adja meg a VMM fürt szerepkör nevét.
12.  **Szinkronizálás felhőalapú metaadat-alapú** jelölje ki azt meg szeretné-e a VMM kiszolgálón minden felhő metaadatait szinkronizálni a tárolóból elemre. Ez a művelet csak egyszer fordulhat elő, minden egyes kiszolgálón szükséges. Ha nem szeretné az összes felhőket szinkronizálni, ezeket nem jelöli be ezt a beállítást, és egyenként az a felhő tulajdonságok a VMM konzolban minden Felhő szinkronizálása.

13.  Kattintson a **Tovább gombra** a folyamat befejezéséhez. Regisztráció után a VMM kiszolgálóról metaadatok által Azure webhely helyreállítási keresi. A kiszolgáló a **VMM kiszolgálók** lapján a tárolóból elemre a **kiszolgálók** lapján megjelenik.

    ![Kiszolgáló](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Után a kiszolgáló érhető el a webhely helyreállítási konzolban **forrásban** > **Előkészítés forrás** jelölje ki a VMM kiszolgálót, és válassza a a felhőben, ahol a Hyper-V host is található. Kattintson **az OK**gombra.

#### <a name="command-line-installation"></a>Parancssori telepítése

Az Azure webhely helyreállítási szolgáltató a parancssorból telepíthető. Ez a módszer a szolgáltató telepítése Server Core a Windows Server 2012 R2 használható.

1. Töltse le a szolgáltató telepítési fájl és a regisztrációs kulcs mappára. Ha például C:\ASR.
2. A System Center virtuális gép kezelő szolgáltatás leállítása.
3. A rendszergazda jogú parancssort, ezek a parancsok, ki kell olvasni a szolgáltató telepítő futtatása:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. A parancsot a szolgáltató telepítése:

        C:\ASR> setupdr.exe /i

5. Futtassa a parancsok, a kiszolgáló regisztrálása a tárolóból elemre:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Hol találhatók a Paraméterek:

 - **/Credentials**: kötelező paraméter, amely a bejegyzés fő fájlt a helye  
 - **/FriendlyName**: a kiszolgáló nevének a Hyper-V host az Azure webhely helyreállítási portálon megjelenő paramétert kötelező megadni.
 - **/EncryptionEnabled**: használt csak esetében, a VMM amelyek Azure választható paraméter.
 - **/proxyAddress**: választható paraméter, amely megadja a címét a proxykiszolgáló.
 - **/proxyport**: választható paraméter, amely megadja a port a proxykiszolgáló.
 - **/proxyUsername**: választható paraméter, amely a Proxy felhasználónév (Ha a proxy-hitelesítést igényel).
 - **/proxyPassword**: választható paraméter, amely a proxykiszolgáló használata hitelesítése (Ha a proxy-hitelesítést igényel) jelszava.  

## <a name="step-3-set-up-the-target-environment"></a>3 lépés: A Céllista környezet beállítása

Jelölje be a cél VMM kiszolgáló és a felhőben.

1. Kattintson az **Előkészítés infrastruktúra** > **cél** , és válassza ki a használni kívánt cél VMM-kiszolgálóhoz.
2.  Felhőket a kiszolgálón, hogy a rendszer szinkronizálja az webhely helyreállítási jelennek meg. Jelölje be a cél felhőben.

    ![Cél](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Lépés: 4: Beállítása replikációs beállításai

1. Hozzon létre egy új replikációs házirend kattintva **Előkészítés infrastruktúra** > **Replikációs beállításai** > **+ létrehozása és az társítása**.

    ![Hálózati](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. **Létrehozás és társítása házirend** adja meg a házirend nevére. A forrás- és célwebhelyek írja be **A Hyper-V szolgáltatást úgy**kell lennie.
3. Jelölje ki **a Hyper-V host verzió** mely operációs rendszer fut, az állomáson.

    > [AZURE.NOTE] VMM felhőbeli Windows Server (támogatott) különböző verzióit futtató a Hyper-V hosts is tartalmazhat, de a replikáció házirend alkalmazott hosts az azonos operációs rendszert futtató. Ha több, mint egy operációs rendszer verziója fut a hosts hozzon létre külön replikációs házirendek.

4. **Hitelesítés típusa** és a **hitelesítési port** adja meg, hogyan hitelesíti forgalom az elsődleges és helyreállítási a Hyper-V kiszolgálók között. Jelölje ki a **tanúsítványt** , kivéve, ha rendelkezik működő Kerberos környezetben. Azure webhely helyreállítási automatikusan beállítja a tanúsítványok a HTTPS-hitelesítést. Nem kell tennie semmit sem manuálisan. Alapértelmezés szerint port 8083 és 8084 (a tanúsítványok) nyitja meg a Windows tűzfal be a Hyper-V kiszolgálók. **Kerberos**választásakor Kerberos-jegy host-kiszolgálók kölcsönös hitelesítéshez használható. Figyelje meg, hogy ez a beállítás csak akkor jelentősége a Windows Server 2012 R2 rendszeren futó a Hyper-V kiszolgálók.
3. **Másolja a gyakoriság** adja meg, hogy milyen gyakran való replikáció delta adatok a kezdeti replikáció (30 másodpercenként, 5-ös vagy 15 perc) után.
4. A **helyreállítási pont adatmegőrzési**adja meg, órában mennyi ideig az adatmegőrzési ablak az egyes helyreállítási pont. Védett gépek ablak bármely pontjára állíthatók helyre.
6. Az **alkalmazás egységes pillanatkép gyakoriság** adja meg, milyen gyakran (1 – 12 óra) tartalmazó alkalmazás egységes pillanatképek helyreállítási pontok létrehozott. A Hyper-V használja kétféle pillanatképek –, ahol egy teljes virtuális gépen növekményes pillanatképét szabványos pillanatkép, és az alkalmazás adatok belül a virtuális gép pont és az idő pillanatfelvételt-alkalmazás egységes pillanatfelvételek. Alkalmazás egységes pillanatképek kötet szolgáltatás (Árnyékmásolata) használja annak érdekében, hogy alkalmazásokat konzisztens állapotát, ha a pillanatkép származik. Figyelje meg, hogy ha bekapcsolja az alkalmazás egységes pillanatképek, befolyásolja a forrás virtuális gépeken futó alkalmazások teljesítményét. Győződjön meg arról, hogy beállított értéke kisebb, mint konfigurálnia további helyreállítási pontok száma.
7. **Adatok átvitele tömörítés** adja meg, hogy van-e replikált átvitt adatok tömörítve.
8. Jelölje be a megadhatja, hogy a replika virtuális gép törölni kell a forrás virtuális védelem letiltásakor **replika virtuális törlése** . Védelem a forrás törlődik a webhely konzol virtuális letiltásakor engedélyezi ezt a beállítást, ha a VMM webhely helyreállítási beállításainak törlődik a VMM konzolról, és a replika törlődik.
3. A **Kezdeti replikációs módszer** Ha, használja a hálózaton keresztül replikálása adja meg, hogy a kezdeti replikáció indítása, és ütemezze. Mentse a hálózat sávszélessége érdemes lehet ütemezze a elfoglalt óra kívül. Kattintson **az OK**gombra.

    ![Replikációs házirend](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Amikor létrehoz egy új házirendet, a VMM felhő automatikusan ki van társítva. **Replikációs házirend** kattintson **az OK gombra**. További VMM felhőket (és a bennük lévő VMs) társíthat a replikáció házirend **-beállításai** > **replikációs** > házirend neve > **VMM felhő társítani**.

    ![Replikációs házirend](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Kapcsolat nélküli kezdeti replikációs előkészítése

A kiindulási adatok másolat offline replikációs műveleteket hajthat végre. Ez a következőképpen előkészítheti:

- A forrás kiszolgálón egy elérési útját, amelyből az adatok exportálása vitelekor fogja adja meg. Teljes hozzáférés hozzárendelése NTFS és megosztása az engedélyek a VMM szolgáltatás, kattintson az Exportálás elérési útját. A cél kiszolgálón fogja adja meg a elérési útját, amelyből az adatok importálása akkor fordul elő. Rendelje hozzá az Importálás elérési út a ugyanolyan engedélyekkel.
- Az importálás és exportálás elérési út meg van osztva, rendelje hozzá a rendszergazda, kiemelt felhasználó, nyomtatás operátor vagy Server operátor csoporttagság az VMM szolgáltatásfiókhoz a távoli számítógépen, amelyen a megosztott található.
- Hosts, hozzáadásához kattintson az importálás és exportálás elérési út bármelyik Run As fiókok esetén, ha olvasási és hozzárendelése írási engedély VMM Futtatás mint fiókok.
- Az importálás és exportálás megosztások nem kell elhelyezni bármely olyan számítógépen, használja a Hyper-V kiszolgáló, mert visszacsatolási konfigurációs nem támogatja a Hyper-V.
- Az Active Directory a minden a Hyper-V kiszolgáló, amely tartalmazza a virtuális gépeken futó szeretné megvédeni, engedélyezése és beállítása korlátozott delegálás meg a távoli számítógépek, amelyen az importálás és exportálás elérési utak találhatók, az alábbi képlettel történik:
    1. Nyissa meg a tartományvezérlőnek **Active Directory-felhasználók és a számítógépen**.
    2. Kattintson a konzolfáján **tartománynév** > **számítógépek**.
    3. Kattintson a jobb gombbal a Hyper-V kiszolgáló állomásneve > **Tulajdonságok**.
    4. A **Meghatalmazás** lapon kattintson a **csak a megadott szolgáltatások delegálás a számítógép megbízható**.
    5. Kattintson a **bármely hitelesítési protokoll használatával**.
    6. **Hozzáadás** > **felhasználók és a számítógépen**.
    7. Írja be a nevét a számítógéphez, amelyen az exportálási útvonal > **OK gombra**. Az elérhető szolgáltatások listájából, tartsa lenyomva a CTRL billentyűt, és kattintson a **cifs** > **az OK gombra**. Ismételje meg a számítógéphez, amelyen az Importálás elérési útja nevét. Ismételje meg ezt a Hyper-V kiszolgálók további szükséges.


### <a name="configure-network-mapping"></a>Hálózati hozzárendelést konfigurálása

Állítsa be a hálózati hozzárendelést a forrás- és célwebhelyek hálózatok között.

- [Olvasás](#prepare-for-network-mapping) milyen hálózati leképezés gyors áttekintéséhez hajtja végre. A összeadás [További](site-recovery-network-mapping.md) részletesebb leírást.
- Ellenőrizze, hogy virtuális gépeken futó VMM kiszolgálókon virtuális hálózathoz csatlakozik.

Állítsa be a hozzárendelés az alábbi képlettel történik:

1. **Beállítások** > **Webhely helyreállítási infrastruktúra** > **Hálózati leképezés** > **hálózati hozzárendelések** kattintson a **+ hálózati leképezése**.

    ![Hálózati hozzárendelést](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. **Hozzáadás hálózati hozzárendelése** lapon jelölje be a forrás és adja meg a célként VMM kiszolgálókhoz. A virtuális hálózatok VMM kiszolgálókon társított beolvasása.
3. **Forrás hálózat**válassza a hálózat az elsődleges VMM kiszolgáló társított virtuális hálózatok a listából.
6. Jelölje ki a hálózat, a másodlagos VMM kiszolgálón használni kívánt **cél hálózati** . Kattintson **az OK**gombra.

    ![Hálózati hozzárendelést](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Az alábbiakban mi történik, ha a hálózati hozzárendelést kezdődik:

- Bármelyik meglévő replika virtuális gépeken futó, amelyek megfelelnek a forrás virtuális hálózat fog kell a cél virtuális hálózathoz csatlakoztatva.
- Új virtuális gépeken futó, amelyeket a forrás virtuális hálózat csatlakoztatott replikáció után a cél a megfeleltetett hálózathoz fog csatlakoztatva.
- Ha módosítja egy már meglévő hozzárendelése egy új hálózattal, replika virtuális gépeken futó is csatlakoznak az új beállítások használatával.
- A cél hálózatnak több alhálózat és az egy adott alhálózat alhálózat, amelyen a forrás virtuális gép található nevét, majd a replika virtuális gép fog csatlakoznia célalkalmazás alhálózat feladatátvétel után. Ha nincs cél alhálózat egyező nevű, a virtuális gép csatlakozik az első alhálózat a hálózaton.

### <a name="configure-storage-mapping"></a>Tárterület-megfeleltetés konfigurálása
Alapértelmezés szerint egy adatforrás a Hyper-V kiszolgálón virtuális géphez, bizonyos a cél a Hyper-V host kiszolgálóhoz replikált adatokat tárolja az alapértelmezett helyre a Hyper-V kezelője a Hyper-V célállomás jelölt. Replikált adatokat tároló pontosabban beállíthatja a tárterület-hozzárendelés<br/><br/> Tárterület-megfeleltetés konfigurálásához szeretne tároló besorolása elemet, kattintson a forrás beállítása és VMM kiszolgálók Megcélozhat a telepítés megkezdése előtt. Tárterület-megfeleltetés új Azure portálon keresztül jelenleg nem támogatott. Azonban engedélyezhető elérjenek Powershell. [Tudjon meg többet](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Lépés az 5: Kapacitás tervezése

Most, hogy van-e a basic infrastruktúra állíthat be, akkor is kapacitás tervezés és azt tudni, hogy szüksége a további erőforrások.

Webhely helyreállítási tartalmaz egy Excel-alapú kapacitás Csapattervező segít a forrás környezetben, webhely helyreállítási összetevői, hálózati és tárolására szolgáló jobb oldali erőforrásokat. A Csapattervező módszerekkel becsült VMs, a lemez és a tárhely átlagos száma alapján a gyors módban, vagy részletes módban, amelyben a terhelést szintjén ábrák fogja bemeneti futtathatja. Mielőtt elkezdené kell:

- A replikáció környezetben, többek között a VMs, per VMs lemezt, és egy lemezen tárolási információkat gyűjthet.
- Becsüljük napi (tejeskanna) módosítása az replikált delta adatokat is. A [Hyper-V kópia kapacitás Csapattervező](https://www.microsoft.com/download/details.aspx?id=39057) segít ezt is használhatja.

1.  Kattintson a **Letöltés** töltse le az eszközt, majd futtatásával gombra. [A cikkben olvasható](site-recovery-capacity-planner.md) az eszköz olvashatja el.
2.  Ha végzett válassza az **Igen lehetőséget** **futtatta a kapacitás Csapattervező**?

    ![Kapacitás tervezése](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Vezérlőelem sávszélesség

Után a kapacitás Csapattervező a Hyper-V kópiáját valós idejű delta replikációs információt összegyűjtött az Excel-alapú kapacitás Csapattervező eszköz segít a replikáció (kezdeti és delta) szükséges sávszélességet számítja ki. A replikáció használt sávszélességet szabályozhatja beállíthatja a NetQos házirend csoportházirend vagy a Windows PowerShell használatával. Néhány módon teheti meg:

**A PowerShell** | **Részletek**
--- | ---
**Új - NetQosPolicy "QoS cél alhálózathoz"** | Szabályozása a Windows Server 2012 R2 rendszer futtatása egy másodlagos alhálózathoz a Hyper-V host érkező forgalmat. Ha az elsődleges és másodlagos alhálózat eltérőek használja.
**Új - NetQosPolicy "QoS célport"** | Szabályozása a Windows Server 2012 R2 rendszer fut a célport a Hyper-V host érkező forgalmat.
**Új - NetQosPolicy "VMM szabályozási forgalom"** | Szabályozása vmms.exe érkező forgalmat. Ez lesz szabályozása a Hyper-V és Live áttelepítési forgalmat. IP-protokollok és finomíthatja portokat is egyeztetni kell.

Sávszélesség súly beállítások használata, vagy egy másodlagos bittel korlátozza a forgalmat. A művelet az összes csomóponton kell fürtre használata. További információ:


- A [Hyper-V replika forgalom szabályozásának](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/) Balázs Maurer-blog
- További információ a [New-NetQosPolicy parancsmag](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Lépés a 6: Engedélyezése replikációs

Most lehetővé teszi a replikáció az alábbi képlettel történik:

1. Kattintson a **lépés 2: alkalmazás bizonyos** > **forrás**. Az első alkalommal replikációs engedélyezése után, válassza a **+ bizonyos** a replikáció további gépekhez engedélyezéséhez a tárolóból elemre.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. Kattintson a **forrás** lap > jelölje ki a VMM-kiszolgáló és a felhőben, amelyben a Hyper-V állomások való replikáció kívánt találhatók. Kattintson **az OK**gombra.

    ![A replikáció engedélyezése](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. A **Target** lap ellenőrizze a másodlagos VMM-kiszolgáló és a felhőben.
4. Jelölje ki a listából védelemmel ellátni kívánt VMs **virtuális gépeken futó** .

    ![Virtuális gép védelem bekapcsolása](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

A **Védelem bekapcsolása** művelet a beállítások végrehajtásának nyomon követhető > **feladatok** > **webhely helyreállítási feladatokat**. A **Védelem véglegesítése** feladat futtatása után a virtuális gép készen áll a feladatátvételi.


>[AZURE.NOTE] A védelmet a virtuális gépeken futó a VMM konzolban. **Védelem bekapcsolása** kattintson az eszköztáron a virtuális gép Tulajdonságok > **Azure webhely helyreállítás** fülre.

Miután engedélyezte a replikációs **beállításai**a virtuális tulajdonságok megtekintése > **Replikált elemek** > virtuális neve. Az **alapok** irányítópulton megtekintheti a virtuális és állapota a replikáció házirendre vonatkozó információk. További részletekért kattintson a **Tulajdonságok** gombra.

### <a name="onboard-existing-virtual-machines"></a>Beépített meglévő virtuális gépeken futó

Ha van meglévő virtuális gépeken futó VMM a Hyper-V replikával replikálja, akkor fedélzeti őket az Azure webhely helyreállítási védelme az alábbiak szerint:

1. Győződjön meg arról, hogy a meglévő virtuális a Hyper-V kiszolgálójához elsődleges felhőbeli és is, hogy a Hyper-V kiszolgáló szolgáltatója a replika virtuális gép található, a másodlagos felhőben.
2. Ellenőrizze, hogy egy replikációs házirend be van állítva az elsődleges VMM felhőbeli.
2. Az elsődleges virtuális gép replikáció engedélyezése. Azure webhely helyreállítási és VMM biztosítja, hogy az azonos replika host és virtuális gép lép fel, és Azure webhely helyreállítási fogja használni a leképezést, és a megadott beállítások replikációs visszaállítása.


## <a name="step-7-test-your-deployment"></a>7 lépés: A telepítés tesztelése

A telepítés tesztelése egyetlen virtuális géphez próba feladatátvevő futtatása, vagy egy vagy több virtuális gépeken futó tartalmazó helyreállítási terv létrehozása.

### <a name="prepare-for-failover"></a>Feladatátvevő előkészítése

- Teljesen tesztelje a üzembe van szüksége a replikált gép a várt módon működnek-infrastruktúra alakítható ki. Ha meg szeretné vizsgálni, az Active Directory és a DNS-virtuális gép létrehozása a tartományvezérlőnek a DNS-ben, és az Azure-webhely helyreállítás Azure való replikáció ez. Olvassa el a további a [próba feladatátvevő szempontjait az Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Ez a cikk utasításait bemutatják, hogyan nincs hálózat tesztelése feladatátvevő futtatásához. Ezt a beállítást, hogy a virtuális átvétele, de nem tesztelje a hálózati beállításokat a virtuális fog tesztelje. [Tudjon meg többet](site-recovery-failover.md#run-a-test-failover) a további beállításokat.
- Ha a használni kívánt feladatátvételhez próba feladatátvevő helyett vegye figyelembe az alábbiakat:

    - Ha lehetséges állítsa le a elsődleges gépek feladatátvételhez futtatása előtt. Ez biztosítja, hogy nincs-e a forrás- és a replika gépeken futó operációs rendszert futtató, egy időben.
    - Feladatátvételhez futtatásakor az megáll a adatok replikációs az elsődleges gépek, bármilyen adat delta nem vihetők, után feladatátvételhez kezdődik. Ezenkívül ha feladatátvételhez helyreállítási csomag fog futni mindaddig, amíg befejeződik, még akkor is, ha hiba történik.




### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1. Egy egyetlen virtuális **beállításai**átadni > **Replikált elemeket**, kattintson a virtuális > **+ próba feladatátvevő**.

    ![Teszt feladatátvevő](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Helyreállítási csomagra, **Beállítások**átadni > **Helyreállítási tervek**, kattintson a jobb gombbal a terv > **Próba feladatátvevő**. Hozhat létre a helyreállítás tervet, [kövesse az alábbi lépéseket](site-recovery-create-recovery-plans.md).
2. A **Próba feladatátvevő**válassza a **nincs**lehetőséget. Ezt a beállítást választja, tesztelje a virtuális átvétele várt módon működik, de nem csatlakozik a replikált virtuális olyan hálózat.

    ![Teszt feladatátvevő](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Kattintson **az OK** gombra a feladatátvételi megkezdéséhez. A virtuális tulajdonságainak megnyitása vagy a **Próba feladatátvevő** feladat, a **Beállítások**gombra kattintva haladásának nyomon követhető > **feladatok** > **webhely helyreállítási feladatokat**.
4. Ha a feladatátvevő feladat eléri a **teljes tesztelés** fázis, tegye a következőket:

    -  A virtuális replika megtekintése a másodlagos VMM felhőben.
    -  Kattintson **a vizsgálat kész** a próba feladatátvételi befejezéshez.
    -  Kattintson a **jegyzetek** rögzítheti és mentheti a próba feladatátvételi társított észrevételeit.

5. A próba virtuális gépen, amelyen a kópia virtuális gép létezik a host azonos állomáson jön létre. A replika virtuális gép található azonos felhőbeli lett hozzáadva.
6. VMs kezdődő sikeresen ellenőrzése után kattintson **a vizsgálat feladatátvételi befejeződött**. Ebben a szakaszban bármely automatikusan létrehozott webhely helyreállítási a próba feladatátvételkor elemek törlődnek.  

    > [AZURE.NOTE] Ha a teszt feladatátvétel két héttel már továbbra is által hatályba befejeződik.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>A DNS frissítése virtuális IP-cím replikával

Feladatátvétel után a virtuális replika előfordulhat, hogy nincs a elsődleges virtuális számítógép ugyanazt a címet.

- Virtuális gépeken futó frissíti a DNS-kiszolgáló, amely használnak után induljanak el.
- Manuálisan is frissítheti DNS az alábbi képlettel történik:

#### <a name="retrieve-the-ip-address"></a>Az IP-cím beolvasása

Futtassa a mintaparancsfájl beolvasásához IP-címét.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>A DNS frissítése

Futtassa a mintaparancsfájl DNS, adja meg az IP-cím használata az előző mintaparancsfájl lekért frissítéséhez.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Következő lépések

A telepítés után be van állítva, és működik, [megtudhatja, hogy](site-recovery-failover.md) a különböző típusú feladatátadás.
