<properties
   pageTitle="Hozzon létre egy virtuális több"
   description="Megtudhatja, hogy miként hozhat létre, és több vms konfigurálása"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Hozzon létre egy virtuális több

Azure virtuális gépeken futó (VMs) létrehozása és a csatolása a VMs mindegyike több hálózati kapcsolaton (NIC). Több hálózati kártya sok hálózati virtuális készülékek, például az alkalmazás szállítási és a WAN-optimalizálási megoldásokért felvétele szükséges. Több hálózati kártya is biztosít további hálózati forgalmat kezelési funkciók, beleértve elkülönítési forgalom elöl közötti hálózati kártya befejezéséhez, és záró hálózati kártya vagy a sík adatforgalom szétválasztása biztonsági kezelési sík forgalmat a.

![Több hálózati virtuális](./media/virtual-networks-multiple-nics/IC757773.png)

A fenti ábrán egy virtuális három NIC, az egyes csatlakozik egy másik alhálózat.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Internetes virtuális (klasszikus telepítések) csak támogatott hálózati adapteren "alapértelmezés" Csak egy virtuális IP-, az alapértelmezett adaptert van
- Ebben az esetben a példány szint nyilvános IP (LPIP) címek (klasszikus telepítések) nem támogatott több hálózati kártya VMs.
- A virtuális belül a hálózati adapterrel sorrendjének véletlen lesz, és Azure infrastruktúra frissítések közötti is változhat. Azonban az IP-címek és a megfelelő ethernet MAC címek fog változatlan marad. Tegyük fel, hogy **Eth1** van 10.1.0.100 és MAC címet 00-0D-3A-B0-39-0D; az Azure infrastruktúrájának és után indítsa újra a rendszert **Eth2**, de a IP sikerült módosítani, és MAC párosítás változatlan marad. Ha a számítógép újraindítása ügyfél által kezdeményezett, a hálózati kártya sorrendben változatlan marad.
- Minden egyes virtuális a hálózati címét alhálózat kell elhelyezni, kattintson egy egyetlen virtuális több NIC minden rendelhetők címet, amely ugyanahhoz az alhálózathoz a.
- A virtuális méretét a szám, amely egy virtuális hozhat létre NICS határozza meg. Útmutató a [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) és a [Linux](../virtual-machines/virtual-machines-linux-sizes.md) virtuális méretű cikkek megállapíthatja, hogy hány NIC támogatja a minden virtuális méretét. 

## <a name="network-security-groups-nsgs"></a>Hálózati biztonsági csoportok (NSGs)
Erőforrás-kezelő környezetben egy virtuális meg bármelyik hálózati az egy hálózati biztonsági csoport (NSG), beleértve minden NIC egy virtuális, amely több NIC engedélyezve van a társított lehet. Ha egy hálózati kártya van hozzárendelve egy címet, ahol az alhálózathoz társítva az NSG alhálózat, majd a szabályok az alhálózat NSG is alkalmazása adott adaptert Nemcsak a alhálózat társítása NSGs, is társíthat egy hálózati kártya egy NSG.

Ha alhálózat hozzá van rendelve egy NSG, és a hálózati alhálózat belül egy NSG egyenként társított, a társított NSG szabály **folyamat sorrend** szerint átadott és elhalványítása a hálózati forgalmat irányát:

- **A bejövő forgalom** akiknek Céltéma a szóban forgó hálózati folyik először az alhálózat el a alhálózat NSG szabályok be a hálózati kártya átadása, majd a hálózati NSG szabályok elindítása előtt.
- **Kimenő forgalmának** amelynek forrását a szóban forgó hálózati szövegdobozról először meg a hálózati el a hálózati NSG szabályok az alhálózathoz áthaladó, majd a alhálózat NSG szabályok elindítása előtt.

További információk a [Hálózati biztonsági csoportokat](virtual-networks-nsg.md) , és hogyan alkalmazza a társítások alhálózat, VMs és NIC alapján.

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>A többszörös hálózati kártya virtuális konfigurálásáról klasszikus környezetben

Az alábbi útmutatást segítséget hozzon létre egy hálózati kártya virtuális 3 NIC tartalmazó több: egy alapértelmezett hálózati kártya és a két további kártyát. Beállítási lépések hoz létre egy virtuális a szolgáltatás konfigurációs fájl részlet az alábbi megfelelően konfigurált:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


A példában a PowerShell-parancsait futtatása előtt meg kell az alábbi előfeltételek.

- Egy Azure-előfizetést.
- A beállított virtuális hálózati. Lásd: a [Virtuális hálózati áttekintés](virtual-networks-overview.md) VNets kapcsolatban további tudnivalókat.
- A legújabb Azure PowerShell letöltött, és telepítve van. [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)tájékozódhat.

Több egy virtuális létrehozásához kövesse az alábbi lépéseket:

1. Válassza ki a virtuális képként Azure virtuális kép gyűjteményből. Figyelje meg, hogy a képek gyakran módosulnak, és régió szerint. A képet, az alábbi példában a megadott módosítása vagy előfordulhat, hogy nem kell az adott régióban, ezért mindenképpen adja meg a képet, akkor.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Hozzon létre egy virtuális konfigurációt.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Az alapértelmezett rendszergazdai bejelentkezés létrehozása.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. További NIC adja hozzá a virtuális konfiguráció.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Adja meg a alhálózat és IP-címet az alapértelmezett adaptert

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Hozzon létre a virtuális a virtuális hálózat.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] Az itt megadott VNet már léteznie kell, (mint az Előfeltételek említett). Az alábbi példában egy virtuális hálózati **MultiNIC-VNet**nevű határozza meg.

## <a name="limitations"></a>Korlátozások

Az alábbi korlátozások vonatkoznak olyan esetben, amikor a többszörös hálózati funkció használata:

- Több hálózati kártya VMs Azure virtuális hálózatok (VNets) kell létrehozni. Nem-VNet VMs nem állítható be a több NIC.
- Az elérhetőség beállítása az összes VMs kell használnia, vagy több hálózati vagy egyetlen adaptert Több hálózati kártya VMs és egyetlen hálózati kártya VMs az elérhetőség belül nem lehet. Az egy felhőalapú szolgáltatásba VMs ugyanazokat a szabályokat alkalmazni.
- Az egyetlen hálózati kártya egy virtuális nem konfigurálhatók a többszörös hálózati kártyák (és a fordítva) Miután telepítené, törlése és újbóli létrehozásával nélkül.


## <a name="secondary-nics-access-to-other-subnets"></a>Más alhálózathoz másodlagos NIC hozzáférés

Alapértelmezés szerint a másodlagos NIC lesz nincs konfigurálva alapértelmezett átjárót, amelyek miatt a forgalom adatfolyam a másodlagos NIC a korlátozott lesz ugyanahhoz az alhálózathoz belül. Ha a felhasználókat szeretne beszélgetni kívül saját alhálózat másodlagos NIC engedélyezése, azok kell szöveg beszúrása a útválasztási táblázatban az átjáró beállítása az alábbiakban leírtak szerint.

>[AZURE.NOTE] Előfordulhat, hogy július 2015 az összes NIC beállítja az alapértelmezett átjárót előtt létrehozott VMs. Az alapértelmezett átjáró másodlagos NIC nem távolítja el, e VMs újraindítása. Az operációs rendszerek, például Linux rendszerhez, a gyenge host útválasztási modell használata internetkapcsolat megszakítható különböző NIC használatakor a be- és kilépési forgalmat.

### <a name="configure-windows-vms"></a>A Windows VMs konfigurálása

Tegyük fel, hogy a két kártyát egy Windows virtuális az alábbi képlettel történik:

- Elsődleges hálózati kártya IP-címe: 192.168.1.4
- Másodlagos hálózati kártya IP-címe: 192.168.2.5

A IPv4-útvonal táblázat a virtuális így néz ki:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Figyelje meg, hogy az alapértelmezett útvonal (0.0.0.0) csak az elsődleges adaptert számára elérhető Nem kívül az alhálózathoz erőforrások elérheti a másodlagos hálózati kártya, az alább látható módon:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

A másodlagos hálózati kártya alapértelmezett útvonal hozzáadásához kövesse az alábbi lépéseket:

1. A parancssorból, futtassa a sorszámát azonosítja a másodlagos hálózati kártya az alábbi parancsot:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Figyelje meg, az index 27 (példánkban) a tábla a második tételt.
3. A parancssorból a **útvonal hozzáadása** parancs futtatásával alább látható módon. Ebben a példában megadása esetén 192.168.2.1 az átjárót, a másodlagos hálózati esetében:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Kapcsolat teszteléséhez térjen vissza a parancssor ikonra, és próbálja meg a ping a másodlagos hálózati kártya egy másik alhálózat mint megjelenített int eh az alábbi példában:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Érdemes is útvonal táblázatot szeretne, jelölje be az újonnan hozzáadott útvonal alább látható módon:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Linux VMs konfigurálása

Az Linux VMs mivel az alapértelmezett működés útválasztás, a gyenge host használja azt javasoljuk, hogy a másodlagos NIC csak forgalom csak ugyanahhoz az alhálózathoz belül. Azonban ha bizonyos esetekben az alhálózathoz kívüli kapcsolódási igényelnek, felhasználók engedélyeznie kell a csoportházirend-alapú annak érdekében, hogy a be- és kilépési forgalom használja-e az azonos adaptert továbbítása

## <a name="next-steps"></a>Következő lépések

- [Erőforrás-kezelő környezetben 2 szintű alkalmazás helyzetben MultiNIC VMs](virtual-network-deploy-multinic-arm-template.md)üzembe.
- Üzembe [MultiNIC VMs klasszikus környezetben 2 szintű alkalmazás használatával](virtual-network-deploy-multinic-classic-ps.md).
