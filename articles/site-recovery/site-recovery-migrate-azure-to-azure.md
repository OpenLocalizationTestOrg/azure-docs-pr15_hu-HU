<properties
    pageTitle="Azure régióból, egy másik, a webhely helyreállítási áttelepítendő Azure IaaS virtuális gépeken futó |} Microsoft Azure"
    description="Azure webhely helyreállítási segítségével Azure IaaS virtuális gépeken futó áttelepítése egy Azure régió között."
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
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Azure IaaS virtuális gépeken futó Azure régió az Azure-webhely helyreállításhoz közötti áttelepítése

## <a name="overview"></a>– Áttekintés

Üdvözli a Azure webhely helyreállítási! Ez a cikk használja, ha Azure VMs áttelepítendő Azure területek között. Mielőtt elkezdené, vegye figyelembe, hogy:

- Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása. Az áttelepítés az alapvető lépések megegyeznek, hogy éppen konfigurálása webhely helyreállítási, az erőforrás-menedzserek vagy klasszikus. Azonban a felhasználói felület útmutatók és a jelen cikkben képernyőképek fontosak az Azure-portálra.
- **Jelenleg csak áttelepítheti egy területről a másikra. Sikertelen lehet a másikra Azure régióból VMs fölé, de Ön nem visszavétele őket újra.**
- Az áttelepítési útmutatást a jelen cikkben alapuló esetében, amelyek egy fizikai számítógépre Azure vonatkozó lépéseket. Hivatkozások a lépésekkel [bizonyos VMware VMs vagy a fizikai Azure-kiszolgálók](site-recovery-vmware-to-azure.md), amely bemutatja, hogy miként való replikáció az Azure-portálon fizikai kiszolgáló tartalmazza.
- A klasszikus portálon állít be a webhely helyreállítás, ha kövesse az [Ebben](site-recovery-vmware-to-azure-classic.md)a cikkben részletes útmutatót. **Már nem kell használni** a [régi cikk](site-recovery-vmware-to-azure-classic-legacy.md)utasításait.

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.


## <a name="prerequisites"></a>Előfeltételek

Az alábbiakban a telepítéshez szükséges:

- **Konfigurációs kiszolgálója**: egy helyszíni virtuális fut a Windows Server 2012 R2 működik-e a kiszolgáló. Telepíti a webhely helyreállítási más összetevőket (beleértve a folyamat kiszolgáló, valamint fő cél) a virtuális is. Tudjon meg többet a [forgatókönyv architektúrája](site-recovery-vmware-to-azure.md#scenario-architecture) és [konfigurációs kiszolgáló Előfeltételek](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS virtuális gépeken futó**: A VMs áttelepítendő. Ezek VMs kezelni fizikai gépek áttelepítéséhez.

## <a name="deployment-steps"></a>Telepítési lépéseket.

Ebben a szakaszban az új Azure portál telepítési lépéseit ismerteti. Ha a klasszikus portálon webhely helyreállításhoz szükséges alábbi telepítési lépéseket, olvassa el az [Ebben](site-recovery-vmware-to-azure-classic.md)a cikkben.

1. [Create a tárolóból elemre](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [A konfigurációs kiszolgálója Deploy](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Után már telepítette a fiókkonfigurációs kiszolgáló, ellenőrizze, hogy akkor tud kommunikálni a VMs áttelepítendő, amelyet a.
4. [Replikációs beállításai beállítása](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Replikációs házirend létrehozása és hozzárendelése a fiókkonfigurációs kiszolgáló.
5. [Telepítse a mobilitás szolgáltatást](site-recovery-vmware-to-azure.md#step-6-replication-application). Minden egyes védelemmel ellátni kívánt virtuális mobilitás telepítve van szüksége. Ez a szolgáltatás elküldi a folyamat kiszolgáló adatokat. A mobilitás szolgáltatás telepíthető manuálisan vagy tolni és automatikusan telepíti a folyamat kiszolgálón Ha engedélyezve van a védelmet a virtuális. Az áttelepíteni kívánt VMs tűzfal szabályok kell beállítania, hogy leküldéses telepítés Ez a szolgáltatás lehetővé teszi.
6. A [Replikáció engedélyezése](site-recovery-vmware-to-azure.md#enable-replication). Engedélyezze az áttelepítendő VMs a replikáció. A magánjellegű IP-cím, a virtuális gépeken futó Azure áttelepítése kívánt IaaS virtuális gépeken futó felfedezheti. Keresse meg a címét a virtuális gép irányítópulton Azure-ban. Ha engedélyezi a replikáció, akkor be a gép típusa a VMs az fizikai gépeken futó.
7. A [Futtatás feladatátvételhez](site-recovery-failover.md#run-an-unplanned-failover). Kezdeti replikációs befejeződése után futtathatja feladatátvételhez Azure területről a másikra. Tetszés szerint helyreállítási terv létrehozása és futtatása több virtuális gépeken futó területek között áttelepítendő feladatátvételhez. [Tudjon meg többet](site-recovery-create-recovery-plans.md) a helyreállítási tervek.

## <a name="next-steps"></a>Következő lépések

További tudnivalók a más replikációs esetben [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)
