
<properties
    pageTitle="Azure webhely helyreállítás: Párhuzamos a Hyper-V virtuális gépeken futó egy VMM kiszolgálón |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan való replikáció a Hyper-V virtuális gépeken futó, amikor csak egyetlen VMM kiszolgáló."
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
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Bizonyos a Hyper-V virtuális gépeken futó egy VMM kiszolgálón

Olvassa el a cikk bemutatja, hogyan való replikáció a Hyper-V virtuális gépeken futó VMM felhő a Hyper-V host kiszolgálón található, ha csak egyetlen VMM kiszolgáló a környezetben.

Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása. Ez a cikk az Azure-portálon replikációs beállítása tartalmazza.


Ha ez a cikk elolvasása után kérdése van, bejegyzése őket a Disqus megjegyzéseket, a jelen cikk vagy a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alján.

## <a name="overview"></a>– Áttekintés

Ha szeretné való replikáció a Hyper-V VMs a Hyper-V állomása VMM található, és csak egyetlen VMM kiszolgálón van, akkor az [Azure való replikáció](site-recovery-vmm-to-azure.md), vagy a egyetlen VMM kiszolgálón felhő között.

Azt javasoljuk, hogy, bizonyos az Azure mert feladatátvevő és helyreállítási nem folytonos verzióazonosítójának felhő között, és a manuális lépésekkel számos van szükség. Ha szeretné, hogy való replikáció csak a VMM kiszolgálót használ, akkor is tegye a következőket:

- Egyetlen önálló VMM kiszolgáló névkiszolgálóit
- Kiterjesztett Windows fürt rendszerbe egyetlen VMM kiszolgáló névkiszolgálóit


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Bizonyos webhelyek egyetlen önálló VMM kiszolgálóval

![Különálló virtuális VMM kiszolgáló](./media/site-recovery-single-vmm/single-vmm-standalone.png)

Ebben az esetben a egyetlen VMM server telepítése a elsődleges hely virtuális gép szerint, és a virtuális bizonyos webhely helyreállítási és a Hyper-V replika egy másodlagos webhelyre.

1. Állítsa be a Hyper-V virtuális a VMM. Javasoljuk, hogy telepítenie az SQL Server-példányt, időt takaríthat meg a azonos virtuális VMM által használt. Használni kívánt SQL Server, illetve egy üzemszünetek távoli példányának fordul elő, ha meg kell helyreállítása az adott példányhoz, mielőtt VMM visszaállíthatja.
2. Győződjön meg arról, hogy van-e beállítva legalább két felhőket a VMM kiszolgálóra. Egy felhőalapú tartalmazza a VMs való replikáció szeretne, és a másodlagos helyeként szolgál más a felhőben. A felhő, amely tartalmazza a védelemmel ellátni kívánt VMs kell rendelkeznie:

    - Egy vagy több VMM host csoportok tartalmazó egy vagy több a Hyper-V kiszolgálók minden host csoportban.
    - Legalább egy a Hyper-V virtuális gép egyes a Hyper-V kiszolgálón esetén.

3. Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre, készítése és letöltése a tárolóból elemre regisztrációs kulcsa és VMM kiszolgáló regisztrálása a tárolóból elemre. Regisztráció során a az Azure webhely helyreállítási szolgáltató a VMM kiszolgálón telepítenie.
4. Állítsa be a VMM virtuális meg egy vagy több felhőket, és adja hozzá ezeket a felhő a Hyper-V állomások.
3. Az felhő védelem beállításainak konfigurálása Az egyetlen VMM-kiszolgáló nevének a forrás- és célwebhelyek helyként adja meg. Adja meg a hálózati hozzárendelést, a felhőben, az a, a virtuális hálózathoz a replikáció felhő védelemmel ellátni kívánt VMs virtuális hálózatai rendelni.
4. A kívánt védelme a hálózaton keresztül, mivel a két felhőket ugyanazon a kiszolgálón található VMs kezdeti replikáció engedélyezése.
4. A Hyper-V kezelője konzolban a Hyper-V replika engedélyezése a Hyper-V állomáson, amely tartalmazza a VMM virtuális, és engedélyezze a virtuális a replikáció. Ellenőrizze, hogy minden olyan webhely helyreállítási eső felhőket nem hozzáadása a VMM virtuális. Ezzel biztosíthatja, hogy a Hyper-V replika beállítások nem bírálni webhely helyreállítási.
5. Szeretne létrehozni a helyreállítási tervek, ha meg kell adnia a forrás- és célwebhelyek ugyanazon VMM a kiszolgálón.

Hozzon létre egy tárolóból elemre, a kiszolgáló regisztrálása és védelem beállítása a [Ez a cikk](site-recovery-vmm-to-vmm.md) útmutatását.

### <a name="what-to-do-in-an-outage"></a>Mi a teendő az egy üzemszünetek

Ha egy teljes üzemszünetek fordul elő, és a másodlagos webhely üzemeltetéséhez szükséges, tegye a következőket:

1.  A Hyper-V kezelője konzolban kiegészítő helyén futtassa a VMM virtuális az elsődleges másodlagos webhely átadni feladatátvételhez.
2.  Győződjön meg arról, hogy a VMM virtuális lépéseket a másodlagos webhelyet.
3.  Futtassa a terhelést a VMs az elsődleges másodlagos felhőket szeretné átadni feladatátvételhez a helyreállítási szolgáltatások tárolóból elemre. A VMs tervezett feladatátvételének befejezéséhez véglegesítse a feladatátvételi, vagy válassza ki a különböző helyreállítási pont szükség szerint.
4.  A tervezett feladatátvételt befejeződése után a felhasználók hozzá tudnak férni a másodlagos webhelyen terhelést erőforrásokat.

Ha az elsődleges webhely megfelelően működik újra, tegye a következőket:

1.  A Hyper-V kezelője konzolban engedélyezése a VMM virtuális replikálása azt az elsődleges másodlagos indításához fordított replikáció.
2.  A Hyper-V kezelője konzolban futtassa a tervezett áttérni visszavétele a VMM virtuális az elsődleges webhelyre. Hajtsa végre a áttérni fejezze be. Engedélyeznie kell a fordított replikációs esetében, amelyek a VMM az elsődleges másodlagos indításához.
3.  A helyreállítási szolgáltatások tárolóra a engedélyezése a terhelést a VMs replikálása őket az elsődleges másodlagos indítása az, fordított replikáció.
4.  Futtassa a terhelést a VMs az elsődleges webhelyre visszavétele tervezett áttérni a helyreállítási szolgáltatások tárolóból elemre. Hajtsa végre a áttérni fejezze be. Engedélyeznie kell a fordított replikációs esetében, amelyek a terhelést a VMs az elsődleges másodlagos indításához.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Bizonyos webhelyek elnyújtva fürt egyetlen VMM kiszolgálón keresztül

![Csoportosított virtuális VMM kiszolgáló](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Helyett egy különálló VMM kiszolgáló telepít, a másodlagos webhelyre másolásához virtuális, akkor is használhatja VMM könnyen hozzáférhető üzembe helyezése a egy Windows Feladatátvevőfürt egy virtuális. Ezzel a megoldással terhelést ellenállóképességére és a hardver hiba elleni védelem. Webhely helyreállítási történő telepítéséről a VMM virtuális telepítésének Nyújtás fürt földrajzilag külön webhelyek közötti. Ennek módja:

1. Telepítse a Windows Feladatátvevőfürt a virtuális gépre VMM, és jelölje ki a beállítást, a kiszolgáló futtatni erősen érhető el, a telepítés során.
2. VMM által használt SQL Server-példányt az SQL Server AlwaysOn rendelkezésre állási csoportok kell replikált, hogy az adatbázis, a másodlagos webhelyen.
3. Hozzon létre egy tárolóból elemre, a kiszolgáló regisztrálása és védelem beállítása a [Ez a cikk](site-recovery-vmm-to-vmm.md) útmutatását. Regisztrálnia kell minden VMM kiszolgáló a fürt a a helyreállítási szolgáltatások tárolóból elemre. Ehhez a szolgáltató telepítése egy aktív csomópontra, és a VMM kiszolgáló regisztrálása. Kattintson a szolgáltató telepíthető többi csomópontot.

### <a name="what-to-do-in-an-outage"></a>Mi a teendő az egy üzemszünetek

Egy üzemszünetek fordul elő, amikor a VMM kiszolgáló és a megfelelő SQL Server-adatbázis keresztül nem sikerült és a másodlagos webhelyen elérhető.


## <a name="next-steps"></a>Következő lépések

[Tudjon meg többet](site-recovery-vmm-to-vmm.md) is megtudhat a részletes webhely helyreállítási telepítésének VMM való replikáció VMM.
