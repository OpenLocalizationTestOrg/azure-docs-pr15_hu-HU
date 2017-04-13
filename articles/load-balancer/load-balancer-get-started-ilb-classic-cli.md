<properties
   pageTitle="Hozzon létre egy belső terheléselosztó az Azure CLI használata a klasszikus telepítési modell |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy belső terheléselosztó az Azure CLI használata a klasszikus telepítési modell"
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

# <a name="get-started-creating-an-internal-load-balancer-classic-using-the-azure-cli"></a>Első lépések megtételében használatával az Azure CLI belső terheléselosztó (klasszikus)

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](load-balancer-get-started-ilb-arm-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


## <a name="to-create-an-internal-load-balancer-set-for-virtual-machines"></a>Hozzon létre egy belső terheléselosztó virtuális gépeken futó beállítása

Hozzon létre egy belső betöltés terheléselosztó beállítása és a kiszolgálókat, azt a forgalom küld, hajtsa végre a következőket:

1. Hozzon létre egy példányának belső terheléselosztás, amely a bejövő forgalom betöltése a kiszolgálóin terheléselosztás meg kell végpont lesz.

1. Adja meg, hogy fog kapni a bejövő forgalom virtuális gépeken futó megfelelő végpontok.

1. Állítsa be a kiszolgáló, amely a forgalmat a forgalom küldésére a belső terheléselosztás példány virtuális IP (virtuális) címének osztható küldi.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Egy belső terheléselosztó CLI használatával lépésről lépésre létrehozása

Ez az útmutató szemlélteti, hogyan hozhat létre egy belső terheléselosztó alapján a forgatókönyv a fenti.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Alább látható módon, futtassa a Váltás a klasszikus mód, az **azure konfiguráció mód** parancsot.

        azure config mode asm

    Várt kimenet:

        info:    New mode is asm


## <a name="create-endpoint-and-load-balancer-set"></a>Végpont létrehozásához, és töltse be a terheléselosztó beállítása

Az esetben azt feltételezzük, hogy a virtuális gépeken futó "D1" és "DB2" egy "mytestcloud" nevű felhőszolgáltatásában. A "testvnet" a "alhálózat-1" alhálózat nevű virtuális hálózat mindkét virtuális gépeken futó használják.

Ez az útmutató a helyi port megegyezik egy belső betöltés terheléselosztó beállítása privát port as port 1433 használata és 1433 hoz létre.

Ez a hol SQL virtuális gépeken futó, ha rendelkezik a háttér egy belső terheléselosztó használatával garantálja az adatbázis-kiszolgálók nem megjelenített közvetlenül a egy nyilvános IP-cím használata a közös példa.


### <a name="step-1"></a>Lépés: 1

Hozzon létre egy belső terheléselosztó használatával `azure network service internal-load-balancer add`.

     azure service internal-load-balancer add -r mytestcloud -n ilbset -t subnet-1 -a 192.168.2.7

A paraméterek használja:

**-r** - felhőalapú szolgáltatás neve<BR>
**-n** - belső betöltés terheléselosztó neve<BR>
**-t** - alhálózat nevét (azonos alhálózat a virtuális gépeken futó szeretne hozzáadni a belső terheléselosztó szerint)<BR>
**-a** - (nem kötelező) egy privát statikus IP-cím hozzáadása<BR>

Nézze meg `azure service internal-load-balancer --help` további információt.

A belső betöltés terheléselosztó tulajdonságai paranccsal ellenőrizheti `azure service internal-load-balancer list` *felhőalapú szolgáltatás neve*.

Az alábbi példa a kimenet a következőképpen:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


## <a name="step-2"></a>Lépés: 2

A belső betöltés terheléselosztó beállítás be kell állítania, ha a felvétele az első végpontot. A végpontot, virtuális gép és vizsgálati port, ebben a lépésben a belső betöltés terheléselosztó állítsa fog társítani.

    azure vm endpoint create db1 1433 -k 1433 tcp -t 1433 -r tcp -e 300 -f 600 -i ilbset

A paraméterek használja:

**k –** – helyi virtuális gép port<BR>
**-t** - vizsgálati port<BR>
**-r** - vizsgálati Protocol (protokoll)<BR>
**-e** – a másodpercben megadott vizsgálati intervallum<BR>
**az f-** - a másodpercben megadott időkorlát <BR>
**-i** - belső betöltés terheléselosztó neve <BR>


## <a name="step-3"></a>3 lépés

Ellenőrizze, hogy a betöltés terheléselosztó konfiguráció használata `azure vm show` *virtuális gép neve*

    azure vm show DB1

A kimenet lesz:

        azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK


## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Virtuális gép távoli asztali végpont létrehozása

Létrehozhat egy távoli asztali végpontot nyilvános port hálózati forgalmának engedélyezésére továbbítja egy adott virtuális gépet használ egy helyi portot `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Virtuális gép eltávolítása terheléselosztó

A virtuális gép távolíthat el egy belső terheléselosztó törli a társított végpont beállítása. A végpont eltávolítása után a virtuális gép a terheléselosztó beállítása a továbbiakban nem tartozik.

 A fenti példa használ, eltávolíthatja a virtuális gép "D1" a belső terheléselosztó "ilbset" a parancs használatával létrehozott végpont `azure vm endpoint delete`.

    azure vm endpoint delete DB1 tcp-1433-1433


Nézze meg `azure vm endpoint --help` további információt.


## <a name="next-steps"></a>Következő lépések

[Betöltési terheléselosztó terjesztési mód forrás IP affinitás konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)