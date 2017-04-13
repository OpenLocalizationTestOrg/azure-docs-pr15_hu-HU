<properties
    pageTitle="A Windows virtuális gépeken futó áttelepítése Amazon webszolgáltatásokhoz webhely helyreállítási az Azure |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogy miként telepítheti át a Windows operációs rendszert futtató Amazon Web Services (AWA) az Azure-webhely helyreállítás Azure virtuális gépeken futó."
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
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>A Windows virtuális gépeken futó Amazon Web Services (AWS) áttelepítése az Azure-webhely helyreállításhoz Azure

## <a name="overview"></a>– Áttekintés

Üdvözli a Azure webhely helyreállítási. Ez a cikk segítségével áttelepítheti a Windows-példányok AWS az Azure webhely helyreállítási az operációs rendszert futtató. Mielőtt elkezdené, vegye figyelembe, hogy:

- Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása. Az áttelepítés az alapvető lépések megegyeznek, hogy éppen konfigurálása webhely helyreállítási, az erőforrás-menedzserek vagy klasszikus. Azonban a felhasználói felület útmutatók és a jelen cikkben képernyőképek fontosak az Azure-portálra.
- **Jelenleg csak áttelepítheti a AWS az Azure. Sikertelen lehet VMs AWS az Azure fölé, de Ön nem visszavétele őket újra. Nincs folyamatban lévő replikációs nem.**
- Az áttelepítési útmutatást a jelen cikkben alapuló esetében, amelyek egy fizikai számítógépre Azure vonatkozó lépéseket. Hivatkozások a lépésekkel [bizonyos VMware VMs vagy a fizikai Azure-kiszolgálók](site-recovery-vmware-to-azure.md), amely bemutatja, hogy miként való replikáció az Azure-portálon fizikai kiszolgáló tartalmazza.
- A klasszikus portálon állít be a webhely helyreállítás, ha kövesse az [Ebben](site-recovery-vmware-to-azure-classic.md)a cikkben részletes útmutatót. **Már nem kell használni** a [régi cikk](site-recovery-vmware-to-azure-classic-legacy.md)utasításait.

Megjegyzések vagy kérdések közzé a képernyő alján a jelen cikk vagy a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="prerequisites"></a>Előfeltételek

Mire van szükség a telepítéshez

- **Konfigurációs kiszolgálója**: egy helyszíni virtuális fut a Windows Server 2012 R2 működik-e a kiszolgáló. Telepíti a webhely helyreállítási más összetevőket (beleértve a folyamat kiszolgáló, valamint fő cél) a virtuális is. Tudjon meg többet a [forgatókönyv architektúrája](site-recovery-vmware-to-azure.md#scenario-architecture) és [konfigurációs kiszolgáló Előfeltételek](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **EC2 virtuális példányok**: A Windows operációs rendszert futtató példányok szeretné áttelepíteni.

## <a name="deployment-steps"></a>Telepítési lépéseket.

Ebben a szakaszban az új Azure portál telepítési lépéseit ismerteti. Ha a klasszikus portálon webhely helyreállításhoz szükséges alábbi telepítési lépéseket, olvassa el az [Ebben](site-recovery-vmware-to-azure-classic.md)a cikkben.

1. [Create a tárolóból elemre](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [A konfigurációs kiszolgálója Deploy](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. A kiszolgáló által üzembe helyezése, után érvényesítése kommunikáció is az áttelepíteni kívánt VMs együtt.
4. [Replikációs beállításai beállítása](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Replikációs házirend létrehozása és hozzárendelése a fiókkonfigurációs kiszolgáló.
5. [Telepítse a mobilitás szolgáltatást](site-recovery-vmware-to-azure.md#step-6-replication-application). Minden egyes védelemmel ellátni kívánt virtuális mobilitás telepítve van szüksége. Ez a szolgáltatás elküldi a folyamat kiszolgáló adatokat. A mobilitás szolgáltatás telepíthető manuálisan vagy tolni és automatikusan telepíti a folyamat kiszolgálón Ha engedélyezve van a védelmet a virtuális. Tűzfalszabályokat EC2 példányok, amelyet át szeretne a kell beállítania, hogy a leküldéses telepítés Ez a szolgáltatás engedélyezése. A biztonsági csoport EC2 előfordulását van, hogy a következő szabályokat:

    ![tűzfalszabályokat](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. A [Replikáció engedélyezése](site-recovery-vmware-to-azure.md#enable-replication). Engedélyezze az áttelepítendő VMs a replikáció. Felfedezheti az EC2 példányok, amely letölthető a EC2 konzol magánjellegű IP-cím használatával.
7. A [Futtatás feladatátvételhez](site-recovery-failover.md#run-an-unplanned-failover). Kezdeti replikációs befejeződése után futtathatja feladatátvételhez AWS az Azure minden virtuális. Tetszés szerint helyreállítási terv létrehozása és futtatása több virtuális gépeken futó áttelepítése AWS Azure feladatátvételhez. [Tudjon meg többet](site-recovery-create-recovery-plans.md) a helyreállítási tervek.

## <a name="next-steps"></a>Következő lépések

További tudnivalók a más replikációs esetben [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)
