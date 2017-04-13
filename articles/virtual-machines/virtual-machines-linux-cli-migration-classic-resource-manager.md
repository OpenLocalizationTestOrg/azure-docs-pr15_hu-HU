<properties
    pageTitle="Áttelepítése IaaS erőforrások klasszikus az Azure erőforrás-kezelő Azure CLI használatával |} Microsoft Azure"
    description="Az alábbiakban ismertetjük az erőforrások platform támogatott áttelepítést a klasszikus az Azure erőforrás-kezelő Azure CLI használatával"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Áttelepítése IaaS erőforrások klasszikus az Azure erőforrás-kezelő Azure CLI használatával

Ezeket a lépéseket az Azure parancssori kezelőfelületről parancsok használata erőforrásként egy szolgáltatást (IaaS) a klasszikus telepítési modellből Azure erőforrás-kezelő telepítési adatmodellhez infrastruktúra áttelepítendő megjelenítése. A cikk az [Azure CLI](../xplat-cli-install.md)szükséges.

>[AZURE.NOTE] Az alábbiakban ismertetjük az összes művelet idempotent. Ha a probléma nem nem támogatott funkciók vagy konfigurációs hiba, azt javasoljuk, hogy meg újra az előkészítés, megszakítása vagy művelet végrehajtása. A platform fog majd próbálkozzon újra.

## <a name="step-1-prepare-for-migration"></a>Lépés: 1: Felkészülés áttelepítésre

Íme néhány gyakorlati tanácsokat, amely szerint felmérése a áttelepítése IaaS erőforrások klasszikus az erőforrás-kezelő javasoljuk:

- Olvassa el a [nem támogatott konfigurációk vagy a szolgáltatások listája](virtual-machines-windows-migration-classic-resource-manager.md). Ha virtuális gépeken futó, amelyek nem támogatott konfigurációk vagy funkcióit, azt javasoljuk, hogy várja meg a szolgáltatás/konfigurációs támogatási jelenti be. Azt is megteheti távolítsa el ezt a szolgáltatást, illetve ki, hogy a konfiguráció áthelyezése áttelepítési engedélyezése, ha az igényeknek.
-   Ha a infrastruktúra és az alkalmazások terjesztése ma parancsprogramok rendelkezik automatikus, próbálja meg hozzon létre egy hasonló próba-beállítást a parancsfájlok áttelepítésre használatával. Azt is megteheti akkor beállíthatja minta környezetekben a Azure portál használatával.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Lépés: 2: Az előfizetés beállítása és a szolgáltató regisztrálásához

Az áttelepítési forgatókönyvek, be kell állítania a környezet két klasszikus és erőforrás-kezelő. [Telepítse az Azure CLI](../xplat-cli-install.md) , és [Jelölje ki azt az előfizetést](../xplat-cli-connect.md).

Bejelentkezés a fiókjába.
    
    azure login

Jelölje ki az Azure előfizetés az alábbi parancsot.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Regisztráció az egy idő lépést, de kell végrehajtania egyszer az áttelepítés előtt. Anélkül, hogy regisztrált látni fogja az alábbi hibaüzenet 

>   *BadRequest: Előfizetési nem regisztrált az áttelepítésre.* 

Az áttelepítési erőforrás szolgáltató regisztrálhatja a következő parancs használatával. Figyelje meg, hogy bizonyos esetekben ez a parancs időtúllépés történik. A regisztráció azonban sikeres lesz.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Várjon öt percig, amíg a bejegyzés befejezéséhez. A jóváhagyási állapotának ellenőrizheti a következő parancs használatával. Győződjön meg róla, hogy RegistrationState `Registered` folytatás előtt.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Most váltás CLI, hogy a `asm` módban.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>3 lépés: Ellenőrizze, hogy van elegendő Azure erőforrás-kezelő virtuális gép magmintákat az aktuális környezetben vagy VNET Azure régióban

Az ebben a lépésben váltás kell `arm` módban. Ehhez az alábbi paranccsal.

```
azure config mode arm
```

A következő CLI parancs használatával jelölje be a jelenlegi mérete magmintákat az Azure erőforrás-kezelő van. Alapvető kvóták kapcsolatos további információért lásd: a [korlátok és az Azure erőforrás-kezelő](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Amikor elkészült ebben a lépésben ellenőrzi, válthat vissza `asm` módban.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Lépés: 4: 1 beállítás – egy felhőalapú szolgáltatásba a virtuális gépeken futó áttelepítése 

Ismerkedés a felhőalapú szolgáltatások listáját a következő parancs használatával, és válassza ki az áttelepíteni kívánt felhőszolgáltatásba. Megjegyzendő, hogy ha virtuális hálózatban a VMs a felhőszolgáltatásában vagy web/dolgozó szerepkörök akkor jelenik meg hibaüzenet.

    azure service list

A következő parancsot a telepítési nevét a felhőbeli szolgáltatástól kinyerése a részletes kimenet. A legtöbb esetben a telepítési neve megegyezik a felhőalapú szolgáltatás neve.

    azure service show <serviceName> -vv

Felkészülés a virtuális gépeken futó a felhőszolgáltatásában az áttelepítésre. Ha két lehetőség közül választhat.

Ha szeretné áttelepíteni a VMs platform létre virtuális hálózati, használja a következő parancsot.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Ha azt szeretné, az erőforrás-kezelő telepítési modellben meglévő virtuális hálózat áttelepítése, használja a következő parancsot.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Az előkészítés művelet befejeződött, miután tekinthetik meg a részletes kimenet szeretne felvenni a VMs áttelepítési állapotát, és győződjön meg arról, hogy vannak-e a keresztül a `Prepared` állapota.

    azure vm show <vmName> -vv

Jelölje be az adatokat az elkészített erőforrások CLI vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja a következő parancsot.

    azure service deployment abort-migration <serviceName> <deploymentName>

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>4 lépés: 2 lehetőséget - virtuális gépeken futó virtuális hálózat áttelepítése

Válassza ki a virtuális hálózat áttelepítése kívánt. Figyelje meg, hogy ha a virtuális hálózat nem támogatott beállításokat tartalmazó webes/dolgozó szerepkörök vagy VMs tartalmaz, akkor kapja egy adatérvényesítési hibaüzenet.

Az előfizetés a virtuális hálózatok megnyithatja a következő parancs használatával.

    azure network vnet list
    
A kimenet jelenik meg az alábbihoz hasonló:

![Képernyőkép: a parancssorban kiemelt teljes virtuális hálózat nevét.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

A fenti példában a **virtualNetworkName** pedig a teljes neve **"Csoport classicubuntu16 classicubuntu16"**.

Készítsen lehetőség a virtuális hálózat áttelepítése a következő parancs használatával.

    azure network vnet prepare-migration <virtualNetworkName>

Jelölje be az adatokat az elkészített virtuális gépeken futó CLI vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja a következő parancsot.

    azure network vnet abort-migration <virtualNetworkName>

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>5 lépés: A tárterület-fiók áttelepítése

Amikor elkészült a virtuális gépeken futó áttelepítette, azt javasoljuk, a tárhely fiók áttelepítése.

A tárterület-fiók előkészületei áttelepítést a következő parancs használatával

    azure storage account prepare-migration <storageAccountName>

Jelölje be az adatokat az elkészített tároló fiók CLI vagy az Azure portal segítségével. Ha nem az áttelepítésre, és azt szeretné, ha szeretne visszatérni a régi állapot, használja a következő parancsot.

    azure storage account abort-migration <storageAccountName>

Ha az elkészített konfiguráció elégedett az eredménnyel, előrelépésre, és az erőforrások véglegesítse a következő parancs használatával.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Következő lépések

- [Áttelepítési platform támogatott IaaS erőforrások klasszikus az erőforrás-kezelő](virtual-machines-windows-migration-classic-resource-manager.md)
- [Műszaki mély merülési platform támogatott áttelepítési a klasszikus az erőforrás-kezelő](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
