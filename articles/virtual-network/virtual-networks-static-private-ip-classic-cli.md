<properties 
   pageTitle="Klasszikus módú ausing statikus magánjellegű IP-cím beállíthatja a CLI |} Microsoft Azure"
   description="Statikus magánjellegű IP-címei (immerzióban), és hogyan kezelheti őket a CLI használatával Klasszikus módú ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Hogyan kell beállítani egy privát statikus IP-cím (klasszikus) Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Kezelheti [az erőforrás-kezelő telepítési modell statikus magánjellegű IP-címet](virtual-networks-static-private-ip-arm-cli.md).

A minta Azure CLI parancsok az alábbi fejlemények már létrehozott egy egyszerű környezetben. Ha szeretne a dokumentumban megjelenített parancsokat, először összeállítása a [Hozzon létre egy vnet](virtual-networks-create-vnet-classic-cli.md)ismertetett tesztkörnyezetben.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>A magánjellegű statikus IP-cím megadása, egy virtuális létrehozásakor
Hozzon létre egy új virtuális *DNS01* nevű *TestService* alapján a forgatókönyv a fenti nevű új felhőalapú szolgáltatás, kövesse az alábbi lépéseket:

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
1. Futtassa a felhőbeli szolgáltatástól létrehozása az **azure szolgáltatás hozzon létre** parancsot.

        azure service create TestService --location uscentral

    Várt kimenet:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Futtassa a virtuális létrehozása az **azure virtuális létrehozása** parancsot. Figyelje meg az érték egy privát statikus IP-címet. A kimenet után megjelenik a paramétereket ismerteti.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Várt kimenet:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (vagy--helyről)**. Azure régió, ahol a virtuális létrejön. A példában *centralus*.
    - **-n (vagy – virtuális neve)**. A létrehozandó virtuális neve.
    - **-w (vagy – ezek olyan virtuális network name)**. A VNet, ahol a virtuális létrejön neve. 
    - **-S (vagy – statikus ip-)**. Statikus magánjellegű IP-címet a virtuális.
    - **TestService**. A felhőalapú szolgáltatást, ahová a virtuális létrejön neve.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage – a Windows-2012R2-x64-v14.2**. A virtuális létrehozásához használt kép.
    - **adminuser**. A Windows virtuális helyi rendszergazdai.
    - **AdminP@ssw0rd**. A Windows virtuális helyi rendszergazdai jelszavát.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan statikus magánjellegű IP-cím adatait egy virtuális beolvasása
Statikus magánjellegű IP-cím információit a fenti forgatókönyvvel létre virtuális megtekintéséhez futtassa a következő Azure CLI parancsot, és tekintse meg az az érték a *Hálózati StaticIP*:

    azure vm static-ip show DNS01

Várt kimenet:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>A magánjellegű statikus IP-cím eltávolítása egy virtuális
Ha el szeretné távolítani a magánjellegű statikus IP-cím hozzáadni a virtuális a fenti parancsfájl futtassa a következő Azure CLI parancsot:
    
    azure vm static-ip remove DNS01

Várt kimenet:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Statikus magánjellegű IP-cím felvétele egy meglévő virtuális
Statikus hozzáadni személyes IP-címet a fenti runt az parancsfájl használatával létrehozott virtuális a következő parancsot:

    azure vm static-ip set DNS01 192.168.1.101

Várt kimenet:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Következő lépések

- [Fenntartott nyilvános IP](virtual-networks-reserved-public-ip.md) -címekről bővebb ismertetőt.
- [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -címekről bővebb ismertetőt.
- Nézze meg a [fenntartott IP REST API-khoz](https://msdn.microsoft.com/library/azure/dn722420.aspx).
