<properties
   pageTitle="Első lépések megtételében szemben lévő klasszikus telepítési modell használata az Azure CLI terheléselosztó internetes |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó klasszikus telepítési modell az Azure CLI használatával"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Első lépések megtételében egy internetes az Azure CLI terheléselosztó (klasszikus)

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó Azure erőforrás-kezelő használatával](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Szemben lévő CLI használatával terheléselosztó internetes lépésről lépésre létrehozása

Ez az útmutató szemlélteti, hogyan hozhat létre egy internetes terheléselosztó alapján a forgatókönyv a fenti.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Alább látható módon, futtassa a Váltás a klasszikus mód, az **azure konfiguráció mód** parancsot.

        azure config mode asm

    Várt kimenet:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Végpont létrehozásához, és töltse be a terheléselosztó beállítása

A példa feltételezi, hogy a virtuális gépeken futó "weben 1" és a "weben 2" lettek létrehozva.
Ez az útmutató a betöltés terheléselosztó beállítása meg nyilvános porttal 80-as port és a helyi port mint 80-as port hoz létre. Vizsgálati port is 80-as port konfigurált és a betöltés terheléselosztó beállítása "lbset" nevű.


### <a name="step-1"></a>Lépés: 1

Hozza létre az első végpontot, és használatával terheléselosztó `azure network vm endpoint create` virtuális gép "weben 1".

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

A paraméterek használja:

**k –** – helyi virtuális gép port<br>
**-o** – Protocol (protokoll)<BR>
**-t** - vizsgálati port<BR>
**-b** - terhelés terheléselosztó neve<BR>

## <a name="step-2"></a>Lépés: 2

Adhat hozzá egy második számítógépre virtuális "weben 2" a betöltés terheléselosztó.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>3 lépés

Ellenőrizze, hogy a betöltés terheléselosztó konfiguráció használata `azure vm show` .

    azure vm show web1

A kimenet lesz:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Virtuális gép távoli asztali végpont létrehozása

Létrehozhat egy távoli asztali végpontot nyilvános port hálózati forgalmának engedélyezésére továbbítja egy adott virtuális gépet használ egy helyi portot `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Virtuális gép eltávolítása terheléselosztó

Ha a megadása a virtuális gép terheléselosztó társított végpont törlése. A végpont eltávolítása után a virtuális gép a terheléselosztó beállítása a továbbiakban nem tartozik.

 A fenti példa használ, eltávolíthatja a "weben 1" virtuális gép létrehozott végpont terheléselosztó "lbset" paranccsal `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Ismerje meg a további beállítások kezelése paranccsal a végpontok`azure vm endpoint --help`


## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

