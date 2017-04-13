<properties 
   pageTitle="Hálózati biztonsági csoportok – PowerShell elhárítása |} Microsoft Azure"
   description="További információ az erőforrás-kezelő Azure telepítési modell Azure PowerShell használatával kapcsolatos hibák elhárítása hálózati biztonsági csoportokat."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Azure PowerShell használatával hálózati biztonsági csoportok – problémamegoldás

> [AZURE.SELECTOR]
- [Azure portálon](virtual-network-nsg-troubleshoot-portal.md)
- [A PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Ha megadott hálózati biztonsági csoportok (NSGs) a virtuális gépen (virtuális), és virtuális csatlakozási problémákat tapasztal, ez a cikk áttekintést nyújt további hibaelhárítási NSGs diagnosztika funkciókhoz.

NSGs lehetővé teszi a forgalom típusú szabályozhatja, hogy a munkafolyamat és kijelentkezés a virtuális gépeken futó (VMs). Az Azure virtuális hálózati (VNet) vagy hálózati kapcsolatok (hálózati) alhálózat NSGs alkalmazhatók. A hatékony szabályokat alkalmazza a hálózati kártya is megtalálható a NSGs egy hálózati kártya alkalmazott szabályok és az alhálózathoz csatlakoztatva van egy összesítést. Szabályok át ezeket a NSGs időnként ütköznek egymással, és hatással lehet a virtuális a hálózati kapcsolat.  

A virtuális NIC alkalmazva az összes hatékony biztonsági szabály megtekintheti a NSGs, a. Ez a cikk bemutatja, hogyan szabályok használatával a Azure erőforrás-kezelő telepítési modell virtuális kapcsolódási problémák elhárítása. Ha nem ismeri a VNet és NSG fogalmak, olvassa el a [virtuális hálózati](virtual-networks-overview.md) és a [hálózati biztonsági csoportok](virtual-networks-nsg.md) – áttekintés cikkeket.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Hatékony biztonsági szabályok segítségével virtuális forgalom folyamat kapcsolatos hibák elhárítása

Az alkalmazási példát, amely követi a leggyakoribb csatlakozási probléma példája:

A virtuális *VM1* nevű *Alhalozat_1* belül egy névvel ellátott *WestUS-VNet1*VNet nevű alhálózat része. A virtuális portot 3389 felett RDP használatával csatlakozhat kísérlet sikertelen lesz. A hálózati kártya *VM1-NIC1* , mind az alhálózathoz *Alhalozat_1*NSGs alkalmazzák. TCP-port 3389 forgalom engedélyezett a NSG társított a hálózati kapcsolaton *VM1-NIC1*, azonban a TCP VM1 a port 3389 sikertelen a ping.

Ez a példa olyan portot 3389, miközben az alábbi lépéseket minden olyan portot határozza meg a bejövő és kimenő csatlakozási hibák használható.

## <a name="detailed-troubleshooting-steps"></a>Részletes hibaelhárítási lépések
Hajtsa végre az alábbi lépések végrehajtásával kapcsolatos hibák elhárítása NSGs egy virtuális:

1. Indítsa el az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure. Ha nem ismeri a Azure PowerShell, olvassa el a [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md) használatával.

2. Írja be a következő parancsot a hálózati kártya elnevezett *VM1-NIC1* az erőforráscsoport *RG1*alkalmazott összes NSG szabály vissza:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Ha nem tudja, hogy egy hálózati kártya nevét, írja be a lekérdezni az összes hálózati kártyákat erőforráscsoport a nevét a következő parancsot: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Az alábbi szöveget egy mintája szerepel a hatékony szabályok kimenet *VM1-NIC1* hálózati kártya esetén a visszaadott:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Megjegyzés: a eredménye a következő adatokat:

    - Két **NetworkSecurityGroup** szakasz: egy alhálózat (*Alhalozat_1*) kapcsolódik, és egy társított egy hálózati kártya (*VM1-NIC1*). Ebben a példában egy NSG alkalmazása minden.
    - **A társítási** jeleníti meg az erőforrás (alhálózat vagy hálózati kártya) egy adott NSG van társítva. Ha a NSG erőforrás áthelyezve/társítás megszüntetve közvetlenül a parancs futtatása előtt, előfordulhat parancs kimeneti megfelelően a változtatások néhány másodperc várakozás. 
    - A szabály neveket is tartalmazni a *defaultSecurityRules*: amikor egy NSG hoz létre, benne készült különböző alapértelmezett biztonsági szabályok. Alapértelmezett szabályok nem távolítható el, de magasabb prioritás szabályokkal felülírható.
     Ha többet szeretne tudni a NSG alapértelmezett biztonsági szabályok a [NSG áttekintése](virtual-networks-nsg.md#default-rules) a cikket elolvasva.
    - A cím prefixumokban NSG alapértelmezett címkék **ExpandedAddressPrefix** kibontása Címkék több címet prefixumokban jelenítik meg. A címkék kiterjesztett akkor lehet hasznos, ha és az adott cím prefixumokban virtuális minőséggel kapcsolatos problémák elhárítása. Például a VNET peering, VIRTUAL_NETWORK címke kibővíti kattintva jelenítse meg az előző eredményben peered VNet prefixumokban.

        >[AZURE.NOTE] A parancs csak megjelenítése hatékony szabályokat, ha egy NSG alhálózat, a hálózati vagy mindkettőt társítva. A virtuális előfordulhat, hogy a különböző NSGs alkalmazott több NIC. Ha a hiba elhárításához, futtassa a parancsot a az egyes adaptert
        
3. Megkönnyítése érdekében a szűrés NSG szabályok nagyobb számú fölé, írja be a következő parancsokat, ha további: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    RDP-forgalmat (TCP port 3389), a szűrő van alkalmazva a Rács nézet, a következő képen látható módon:

    ![Dokumentumszabályok listája:](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. A rács nézetben látható, mint vannak-e mindkét engedélyezése és RDP-megtagadási a szabályokat. Lépés: 2 kimenetét azt mutatja, hogy a *DenyRDP* szabályt alkalmazza az alhálózathoz a NSG a. A bejövő szabályok az alhálózathoz alkalmazott NSGs dolgozza fel először. Ha van találat, a hálózati kapcsolaton alkalmazza a NSG feldolgozása nem. Ebben az esetben a *DenyRDP* szabály az alhálózathoz blokkolja RDP, hogy a virtuális (**VM1**).

    >[AZURE.NOTE] Előfordulhat, hogy egy virtuális több NIC csatolva. Minden lehetséges csatlakoznia kell egy másik alhálózat. Mivel a parancsok az előző lépésekben szemben a hálózati kártya futtat, célszerű győződjön meg arról, hogy adja meg a hálózati kapcsolat elmulasztják problémákat. Ha nem biztos benne, mindig futtathatja a parancsok egyes a virtuális csatlakoztatott hálózati szemben.

5. RDP VM1 be módosítsa a *Megtagadja RDP (3389)* szabály *RDP(3389) lehetővé teszi* a **Alhalozat_1-NSG** NSG. Győződjön meg arról, hogy a TCP port 3389 meg nyitva egy RDP-kapcsolat a virtuális megnyitása vagy a PsPing eszközzel. A [PsPing letöltési oldalon](https://technet.microsoft.com/sysinternals/psping.aspx) olvasásával talál további információt a psping parancs

    Is, vagy egy NSG információk segítségével a következő parancsot a kimeneti szabályok eltávolítása:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Megfontolandó szempontok

Kapcsolódási problémák elhárítása során, vegye figyelembe az alábbiakat:

- Alapértelmezett NSG szabályok tiltható le a bejövő hozzáférése az internetről, és azt csak akkor VNet engedélyezik bejövő forgalom. Szabályok kell kifejezetten hozzá bejövő elérésének engedélyezése az internetről, szükség szerint.
- Nincsenek egy virtuális a hálózati kapcsolat lefagyását okozza NSG biztonsági szabályok, ha a probléma oka az, hogy lehet meg:
    - A virtuális operációs rendszerben futó tűzfal szoftver
    - Virtuális készülékek vagy a forgalmat a helyszíni konfigurálva útvonalak. Internetes forgalmat a helyszíni keresztül kényszerített tunneling átirányítható. Ezt a beállítást, attól függően, hogy a forgalmat a helyszíni hálózati eszközök kapacitása kezelésének nem működik a virtuális egy RDP/SSH kapcsolat az internetről. Olvassa el a [Hibaelhárítás útvonalak](virtual-network-routes-troubleshoot-powershell.md) a cikkből megtudhatja, hogy miként diagnosztizálása útvonal, előfordulhat, hogy a forgalmat és kijelentkezés a virtuális kell késleltetése. 
- Ha VNets, alapértelmezés szerint van peered, a a VIRTUAL_NETWORK címke automatikusan kiterjesztése peered VNets prefixumokban tartalmazzák. Megtekintheti a prefixumokban **ExpandedAddressPrefix** listájában VNet peering kapcsolódási kapcsolatos problémák megoldásához. 
- Hatékony biztonsági szabályok csak van-e egy társított a virtuális hálózati kártya és vagy alhálózat NSG jelennek meg. 
- Ha nem a hálózati kártya társított NSGs vagy alhálózat és rendelkezik egy nyilvános IP-címet a virtuális rendelve, az összes olyan portot nyissa meg a bejövő és kimenő hozzáférés lesz. Ha a virtuális van egy nyilvános IP-címet, a hálózati és alhálózat NSGs alkalmazásával ajánlott.  
