<properties
   pageTitle="Több IP-címek virtuális gépeken futó - PowerShell |} Microsoft Azure"
   description="Megtudhatja, hogy miként több IP-címek hozzárendelése egy virtuális gép Azure PowerShell használatával."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Virtuális gépeken futó több IP-címek hozzárendelése

Azure virtuális gép (virtuális) beállíthatja, hogy egy vagy több hálózati kapcsolat (hálózati) kapcsolódik. Bármelyik hálózati beállíthatja, hogy egy vagy több nyilvános vagy magánjellegű IP-címek rendelve. Ha nem ismeri a IP-címek Azure-ban, olvassa el a [IP-címek Azure-ban](virtual-network-ip-addresses-overview-arm.md) a cikkben többet megtudhat őket. Ez a cikk ismerteti, hogyan Azure PowerShell-lel több IP-címek hozzárendelése az erőforrás-kezelő Azure telepítési modell egy virtuális.

Több IP-címek hozzárendelése egy virtuális lehetővé teszi, hogy az alábbi funkciókat:

- Webhelyek vagy a szolgáltatások külön IP-címek és egy kiszolgálón SSL-tanúsítványok szolgáltatója.
- A hálózati virtuális készülék, például a tűzfalat lesz, vagy terheléselosztó.
- Bármely, a NICS bármely, a személyes IP-címek hozzáadása egy Azure terheléselosztó háttéradatbázist készlet lehetőséget. Múltbeli csak az elsődleges IP-címet az elsődleges hálózati rendszer automatikusan hozzáadja a háttéradatbázist készletébe.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

A betekintő rögzítése, küldjön e-mailt [Több IP -címei](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) az előfizetés azonosítójú és szánt használatát.

## <a name="scenario"></a>Eset

Ebben a cikkben a hálózati felhasználói felülete három IP konfigurációk rendelje hozzá.
A következő példa konfigurációk létrejön és egy három saját IP-címek és a rendelve egy nyilvános IP-címmel rendelkező hálózati rendelt:

- IPConfig-1: A névvel ellátott PIP1 nyilvános IP-cím erőforrás egy dinamikus magánjellegű IP-cím (alapértelmezett) és a egy nyilvános IP-cím.
- IPConfig-2: Egy privát statikus IP-címet, és nincs nyilvános IP-cím.
- IPConfig-3-as: A dinamikus magánjellegű IP-címet, és nincs nyilvános IP-cím.

    ![A kép helyettesítő szöveg](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Ebben az esetben feltételezi, hogy van-e egy úgynevezett *RG1* , amelyeken belül van egy úgynevezett *VNet1* VNet és *Alhalozat_1*nevű alhálózat erőforráscsoport. További azt feltételezi egy virtuális *VM1*nevű, a hálózati kapcsolat *VM1-NIC1* nevű kapcsolódó és a nyilvános IP-cím nevű *PIP1*.

Az alábbiakban [ismertetjük](./virtual-machines/virtual-machines-windows-ps-create.md ) az imént említett lekérdezéstípusokról abban az esetben, ha nem hozott létre őket, mielőtt erőforrások létrehozása.

## <a name = "create"></a>Hozzon létre egy virtuális több IP-címmel

1. Nyisson meg egy PowerShell parancssort, és töltse ki a ebben a szakaszban a hátralévő lépéseket belül egy PowerShell-munkamenetet. Ha még nincs telepítve és beállítva PowerShell, hajtsa végre a [miként telepítheti és Azure PowerShell beállítása](../powershell-install-configure.md) című témakörben.

2. A következő $Variables "értékek" módosítása az Azure virtuális hálózatához, akkor a nevét, az [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups), a VNet belül az erőforráscsoport, az alhálózathoz, amelyhez csatlakozni szeretne a hálózati kártya és a adaptert nevét [helye](https://azure.microsoft.com/regions) Több IP-címek hozzáadása bármelyik hálózati kártya egy virtuális, csatolva van szüksége, lépéseit.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Ha nem tudja, hogy a név erőforráscsoport vagy egy meglévő Azure helyét, írja be az alábbi parancsokat:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>A hálózati kártya egy meglévő Azure virtuális hálózati (VNet) belüli alhálózat kell csatlakoznia. A három összetevőket: hálózati kártya alhálózat és VNet, az összes léteznie kell a terület és az [előfizetés](../azure-glossary-cloud-terminology.md#subscription).  Ha nem ismeri a VNets, olvassa el a [virtuális hálózati – áttekintés](virtual-networks-overview.md) a cikkből megtudhatja, hogy róluk, vagy [Hozzon létre egy VNet](virtual-networks-create-vnet-arm-ps.md) cikkből megtudhatja, hogy miként hozhat létre egyet.

    Ha nem tudja, hogy egy meglévő VNet nevét, írja be a következő parancsot, és *VNet1* az előző változóban cserélhet egy VNet neve:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Ha a lista vissza üres, a VNet létrehozásához szükséges. Megtudhatja, hogyan, olvassa el a [virtuális hálózat létrehozása](virtual-networks-create-vnet-arm-ps.md) .

    Írja be a meglévő alhálózat belül a VNet nevének, és a *Alhalozat_1* feletti cserélhet alhálózat nevét az alábbi parancsokat:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Írja be a következő parancsot a alhálózat beolvasásához, és rendelje hozzá a változó.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>A hálózati szeretne hozzárendelni kívánt IP-konfigurációk meghatározása Minden egyes kereséskonfigurációs beállíthatja, hogy egy statikus vagy dinamikus magánjellegű IP-címet, és egy nyilvános IP-cím erőforráshoz társított egy statikus vagy dinamikus címet.

    Hozzáadhat vagy eltávolíthat tetszőleges számú a attól függően, hogy hány IP-címek a hálózati kártya és a beállítások társítani kívánt be szeretné állítani az alábbi beállításokat.

    **IPConfig-1**

    Módosítsa a *PIP1* értékét adja meg egy meglévő nyilvános IP-cím erőforrás, amely készít a a hálózati hely létezik a nevét, és, amely nem egy másik adaptert jelenleg társított Az erőforráscsoport a nyilvános IP-cím erőforrás nevét a *RG1* módosítása szerepel. Módosítsa az első IP-konfiguráció adni kívánt nevet *IPConfig-1* . Írja be az alábbi parancsokat:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Megjegyzés: a *-elsődleges* válthat. Ha több IP-konfigurációjának rendel egy hálózati, egy konfigurációs, az *elsődleges*e kiosztani. Ha nem tudja, hogy egy meglévő nyilvános IP-cím erőforrás nevét, írja be a következő parancsot: Get-AzureRMPublicIPAddress |} Táblázat formázása neve, hely, IP-cím, IpConfiguration

    A **IPConfiguration** oszlop nem tartalmaz értéket az eredményt ad vissza, ha a nyilvános IP-cím erőforrás nem társított hálózati egy meglévő kártya és használhatók. Ha a lista nem üres, vagy nem érhető el, nyilvános IP-cím erőforrások, létrehozhat egyet a **New-AzureRmPublicIPAddress** paranccsal.

    >[AZURE.NOTE] Nyilvános IP-címek a névleges díjat kell. IP-cím árak kapcsolatos további információért olvassa el az [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses) lapot.

    **IPConfig-2**

    Módosítsa *IPConfig-2* meg szeretné adni a második IP-konfigurációja, és módosítsa *10.0.0.5* még nem használt érvényes IP-címének a oszt ki szeretné a hálózati alhálózat nevére. Írja be az alábbi parancsokat:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    A következő parancsot, adja meg, ha nem tudja, hogy az IP cím alhálózathoz rendelt tartomány:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3-as**

    *IPConfig-3-as* módosítsa a nevet szeretne adni a harmadik IP-konfigurációja, és írja be az alábbi parancsokat:

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Legfeljebb 250 saját IP-cím hozzárendelése egy adaptert A nyilvános IP-címek belül előfizetés használható száma korlátozva van. További információért olvassa el a [beállító Azure](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) cikket.

6. Hozzon létre a hálózati kártya az előző lépésben megadott IP-konfigurációk használatával.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. A hálózati kártya csatolni, egy virtuális létrehozásakor a [Create a virtuális](../virtual-machines/virtual-machines-windows-ps-create.md) cikk utasításait követve. Bár ez a cikk a Windows Server operációs rendszert futtató virtuális hoz létre, a lépések megegyeznek egy Linux virtuális kívül más operációs rendszerű kijelölése. Kövesse a lépéseket 1-3 – a cikk. Ugorja át a 4-es és 5 lépést majd a teljes 6 egy virtuális cikk létrehozása.

    >[AZURE.WARNING] Lépés 6 egy virtuális cikk létrehozása sikertelen lesz, ha módosítani szeretne valami mást a 6: Ez a cikk $nic nevű változó, vagy még nem befejeződött, a jelen cikk az előző lépéseket.

8. Megtekintése a személyes IP-címei, hogy a hálózati kártya rendelt Azure DHCP és a következő parancs megadásával a hálózati kártya rendelt nyilvános IP-cím erőforrás:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Az összes a saját IP-címek (beleértve az elsődleges) manuális felvétele a TCP/IP konfiguráció az operációs rendszerben. 

**A Windows**

1. A parancssorba írja be a *ipconfig/összes*.  Csak az *elsődleges* magánjellegű IP-címet (keresztül DHCP) látható.
2. Ezután írja be a parancssorablakban *ncpa.cpl* . Ekkor megnyílik egy új ablakban.
3. Nyissa meg a Tulajdonságok **Helyi kapcsolat**.
4. Kattintson duplán a IPv4 (IPv4)
5. Jelölje be **az alábbi IP-cím használata** , és írja be az alábbi értékeket:
    - **IP-címe**: Adja meg az *elsődleges* magánjellegű IP-címe
    - **Alhálózat maszk**: Set alhálózathoz alapján. Ha például az alhálózathoz egy /24 alhálózat, majd az alhálózathoz maszk 255.255.255.0.
    - **Alapértelmezett átjáró**: az első IP-cím a alhálózat. Ha az alhálózathoz 10.0.0.0/24, az átjáró IP-cím 10.0.0.1.
    - Kattintson a **használja az alábbi DNS-kiszolgáló címét** , és írja be az alábbi értékeket:
        - **Elsődleges DNS-kiszolgáló:** Adja meg a 168.63.129.16, ha nem saját DNS-kiszolgálót használ.  Ha, írja be a DNS-kiszolgáló IP-címét.
    - Kattintson a **Speciális** gombra, és további IP-címek hozzáadása. Minden ugyanahhoz az alhálózathoz adni az elsődleges IP-cím a hálózati kártya 8 lépésben megjelenik a másodlagos saját IP-címek hozzáadása
    - Kattintson **az OK gombra** kattintva zárja be a TCP/IP beállításait, majd ismét az **OK gombra** a kártya beállításai bezárásához. Ez csak akkor állítja az RDP kapcsolat majd.

6. A parancssorba írja be a *ipconfig/összes*. Felvett összes IP-címek láthatók, és DHCP ki van kapcsolva.


**Linux (Ubuntu)**

1. Nyisson meg egy Terminálszolgáltatások ablakot.
2. Győződjön meg róla, hogy a legfelső szintű felhasználói. Ha nem, végezze el ezt a következő parancs használatával:

            sudo -i
3. Frissítse a konfigurációs fájl a hálózati kapcsolat (feltételezve, hogy a "eth0").
    - A meglévő sortétel DHCP megtartani. Ez lehet korábbi szolgál, konfigurálja az elsődleges IP-címét.
    - A konfiguráció az alábbi parancsokkal további statikus IP-cím felvétele:

                cd /etc/network/interfaces.d/
                ls

        Meg kell jelennie egy .cfg fájlt.
4. Nyissa meg a fájlt: vi *fájlnevet*.

    Meg kell jelennie a következő sort a fájlban végén:

            auto eth0
            iface eth0 inet dhcp
5. Helyezzen el a következő sort a fájl megtalálható a sorok után:

            iface eth0 inet static
            address <your private IP address here>
6. Mentse a fájlt a következő parancs használatával:

            :wq
7.  Állítsa alaphelyzetbe a hálózati kapcsolaton a következő parancsot:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Futtatása ifdown és a ifup ugyanabban a sorban Ha távoli kapcsolaton keresztül.
8. Ellenőrizze, hogy az IP-cím bekerül a hálózati kapcsolaton a következő parancsot:

            ip addr list eth0

    Meg kell jelennie a lista részeként felvett IP-címét.

**Linux (Redhat CentOS és mások)**

1. Nyisson meg egy Terminálszolgáltatások ablakot.
2. Győződjön meg róla, hogy a legfelső szintű felhasználói. Ha nem, végezze el ezt a következő parancs használatával:

            sudo -i
3. Írja be a jelszót, és kövesse az utasításokat, gombra. Ha van a legfelső szintű felhasználó, nyissa meg a hálózati parancsfájlok mappát az alábbi paranccsal:

            cd /etc/sysconfig/network-scripts
4. A lista a kapcsolódó ifcfg fájlokat a következő parancsot:

            ls ifcfg-*

    Ekkor megjelennek a *ifcfg-eth0* egyikeként a fájlokat.
5. Másolja a *ifcfg-eth0* fájlt, és nevezze el *ifcfg-eth0:0* , az alábbi parancsot:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Szerkesztés a *ifcfg-eth0:0* fájlt a következő parancsot:

            vi ifcfg-eth1
7. Az eszköz a fájlban; a megfelelő név módosítása *eth0:0* ebben az esetben az alábbi paranccsal:

            DEVICE=eth0:0
8. Módosítása a *IPADDR = YourPrivateIPAddress* sor megfelelően az IP-címet.
9. Mentse a fájlt a következő parancsot:

            :wq
10. Indítsa újra a hálózati szolgáltatásokkal, és ellenőrizze, hogy a módosítások sikeresek a következő parancs futtatásával:

            /etc/init.d/network restart
            Ipconfig

    Az IP-cím vett fel, *eth0:0*, a visszaadott listában láthatók.

## <a name="add"></a>Egy meglévő virtuális IP-címek hozzáadása

További IP-címek hozzáadása egy meglévő hálózati kártya az alábbi lépések elvégzésével:

1. Nyisson meg egy PowerShell parancssort, és töltse ki a ebben a szakaszban a hátralévő lépéseket belül egy PowerShell-munkamenetet. Ha még nincs telepítve és beállítva PowerShell, hajtsa végre a [miként telepítheti és Azure PowerShell beállítása](../powershell-install-configure.md) című témakörben.

2. A következő $Variables "értékek" nevét, valamint a hálózati kártya fel szeretné venni az IP-címek és az erőforráscsoport szerepel-e a hálózati hely módosítása:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Ha nem tudja, hogy a hálózati kártya szeretné módosítani, adja meg a következő parancsokat a nevét, majd módosítsa az értékeket az előző változót:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Hozzon létre egy változó, és állítsa be a meglévő hálózati írja be az alábbi parancsot:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Lekérdezi az alhálózathoz Azonosítóját, a hálózati kártya [3 lépés](#subnet) egy virtuális létrehozása a több IP-címek című szakaszát követve csatlakozik.

5. Az [5](#ipconfigs) létrehozása az utasításokat követve hozzáadása a hálózathoz szeretne IP-konfigurációk egy virtuális létrehozása több IP-címek című szakaszát.

6. *$IPConfigName4* módosítsa az IP-beállítását az előző lépésben létrehozott nevére. A konfiguráció hozzáadásához írja be a következő parancsot:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. A hálózati az IP-beállításokkal mezőben adja meg a következő parancsot:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Adja hozzá az IP-címek adott hozzá a hálózati virtuális operációs rendszerre egy virtuális több IP-címek szakasszal a jelen cikk létrehozása a [9-es](#os) lépésben utasításait követve.
