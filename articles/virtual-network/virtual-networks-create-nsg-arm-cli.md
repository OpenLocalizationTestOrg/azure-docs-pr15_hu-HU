<properties 
   pageTitle="Hogyan hozhat létre NSGs ARM módban használja az Azure CLI |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, és a használja az Azure CLI ARM NSGs terjesztése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-in-the-azure-cli"></a>Hogyan hozhat létre az Azure CLI NSGs

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Is megtekintheti [a klasszikus telepítési modell NSGs létrehozása](virtual-networks-create-nsg-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

A minta Azure CLI parancsok az alábbi fejlemények egy egyszerű környezet már létrehozott alapján a forgatókönyv a fenti. A parancsokat a dokumentumban megjelenített szeretné, ha először hozza létre a tesztkörnyezetben úgy, hogy [Ez a sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd)üzembe helyezése, kattintson a **központi telepítés az Azure**, és cserélje le a alapértelmezett paraméterértékeket, ha szükséges, kövesse az utasításokat a portálon.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Az előtér-alhálózat a NSG létrehozása
Az elnevezett *NSG-FrontEnd* alapján a forgatókönyv a fenti nevű NSG létrehozásához kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Várt kimenet:

        info:    New mode is arm

3. Futtassa a hozhat létre egy NSG **azure hálózati nsg létrehozása** parancsot.

        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd

    Várt kimenet:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

    Paraméterek:
    - **-g (vagy – erőforrás-csoport)**. Az erőforráscsoport, ahol a NSG létrejön neve. A példában *TestRG*.
    - **-l (vagy--helyről)**. Azure terület, ahol az új NSG létrejön. A példában *westus*.
    - **-n (vagy--neve)**. Az új NSG nevét. A példában *NSG-FrontEnd*.

4. Futtassa a hozhat létre egy szabályt, amely lehetővé teszi, hogy az access port 3389 (RDP) internetes **azure hálózati nsg szabály létrehozása** parancsot.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Várt kimenet:

        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Paraméterek:

    - **-(vagy--nsg neve)**. A NSG létrejön a szabály nevét. A példában *NSG-FrontEnd*.
    - **-n (vagy--neve)**. Az új szabály nevét. A példában *rdp-szabály*.
    - a **-c (vagy – access)**. Hozzáférési szintjének (tiltása vagy engedélyezése) a szabályt.
    - **-p (vagy--protocol)**. Protocol (Tcp, Udp, vagy *) a szabályhoz.
    - **-r (vagy--irányba)**. Kapcsolat (bejövő és kimenő) irányát.
    - **-y (vagy--prioritás)**. A szabály prioritását.
    - **az f-(vagy – adatforrás-cím – előtag)**. Forrás cím előtag CIDR vagy az alapértelmezett címkék segítségével.
    - **-o (vagy – adatforrás-porttartományt)**. Forrásport, vagy porttartományt.
    - **-e (vagy – cél-cím – előtag)**. Cél címe előtag CIDR vagy az alapértelmezett címkék segítségével.
    - **-u (vagy--port-céltartományban)**. Célport vagy porttartományt.    

5. Futtassa a hozhat létre egy szabályt, amely lehetővé teszi, hogy az access (HTTP) 80-as port az internetről **azure hálózati nsg szabály létrehozása** parancsot.

        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Várható putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Futtassa a NSG csatolása az előtér-alhálózat **azure alhálózathoz vnet beállítása** parancsot.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd

    Várt kimenet:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>A visszahívási vége alhálózat a NSG létrehozása
Az elnevezett *NSG-Kódmentes* alapján a forgatókönyv a fenti nevű NSG létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a hozhat létre egy NSG **azure hálózati nsg létrehozása** parancsot.

        azure network nsg create -g TestRG -l westus -n NSG-BackEnd

    Várt kimenet:

        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK

4. Futtassa a hozhat létre egy szabályt, amely lehetővé teszi, hogy az access a porttal 1433 (SQL) az előtér-alhálózat az **azure hálózati nsg szabály létrehozása** parancsot.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Várt kimenet:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

5. Futtassa a hozhat létre egy szabályt, amely megtagadja a hozzáférést az internethez, az **azure hálózati nsg szabály létrehozása** parancsot.

        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *

    Várható putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Futtassa a NSG hivatkozni szeretne a hátsó vége alhálózat **azure alhálózathoz vnet beállítása** parancsot.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd

    Várt kimenet:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
