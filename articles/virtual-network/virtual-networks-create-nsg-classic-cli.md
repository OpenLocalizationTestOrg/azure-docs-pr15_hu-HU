<properties
   pageTitle="Hogyan hozhat létre NSGs klasszikus mód használata az Azure CLI |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre és üzembe helyezni a NSGs klasszikus mód használata az Azure CLI"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Hogyan hozhat létre az Azure CLI NSGs (klasszikus)

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Is [NSGs az erőforrás-kezelő telepítési modell létrehozása](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

A minta Azure CLI parancsok az alábbi fejlemények egy egyszerű környezet már létrehozott alapján a forgatókönyv a fenti. Ha szeretne a dokumentumban megjelenített parancsokat, először a tesztkörnyezetben összeállítása: [Hozzon létre egy VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Az előtér-alhálózat a NSG létrehozása
Az elnevezett **NSG-FrontEnd** alapján a forgatókönyv a fenti nevű NSG létrehozásához kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.

2. Futtassa a **`azure config mode`** parancs, amellyel váltson klasszikus mód, alább látható módon.

        azure config mode asm

    Várt kimenet:

        info:    New mode is asm

3. Futtassa a **`azure network nsg create`** egy NSG létrehozása parancsot.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Várt kimenet:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Paraméterek:

    - **-l (vagy--helyről)**. Azure terület, ahol az új NSG létrejön. A példában *westus*.
    - **-n (vagy--neve)**. Az új NSG nevét. A példában *NSG-FrontEnd*.

4. Futtassa a **`azure network nsg rule create`** hozzon létre egy szabályt, amely lehetővé teszi, hogy az access port 3389 (RDP) az internetről parancsot.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Várt kimenet:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Paraméterek:

    - **-(vagy--nsg neve)**. A NSG létrejön a szabály nevét. A példában *NSG-FrontEnd*.
    - **-n (vagy--neve)**. Az új szabály nevét. A példában *rdp-szabály*.
    - a **-c (vagy--művelet)**. Hozzáférési szintjének (tiltása vagy engedélyezése) a szabályt.
    - **-p (vagy--protocol)**. Protocol (Tcp, Udp, vagy *) a szabályhoz.
    - **-r (vagy--típusa)**. Kapcsolat (bejövő és kimenő) irányát.
    - **-y (vagy--prioritás)**. A szabály prioritását.
    - **az f-(vagy – adatforrás-cím – előtag)**. Forrás cím előtag CIDR vagy az alapértelmezett címkék segítségével.
    - **-o (vagy – adatforrás-porttartományt)**. Forrásport, vagy porttartományt.
    - **-e (vagy – cél-cím – előtag)**. Cél címe előtag CIDR vagy az alapértelmezett címkék segítségével.
    - **-u (vagy--port-céltartományban)**. Célport vagy porttartományt.

5. Futtassa a **`azure network nsg rule create`** parancs, amellyel hozzon létre egy szabályt, amely lehetővé teszi, hogy az access (HTTP) 80-as port az internetről.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Várható putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Futtassa a **`azure network nsg subnet add`** a NSG csatolása az előtér-alhálózat parancsot.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Várt kimenet:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>A visszahívási vége alhálózat a NSG létrehozása
Az elnevezett *NSG-Kódmentes* alapján a forgatókönyv a fenti nevű NSG létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a **`azure network nsg create`** egy NSG létrehozása parancsot.

        azure network nsg create -l uswest -n NSG-BackEnd

    Várt kimenet:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Paraméterek:

    - **-l (vagy--helyről)**. Azure terület, ahol az új NSG létrejön. A példában *westus*.
    - **-n (vagy--neve)**. Az új NSG nevét. A példában *NSG-FrontEnd*.

4. Futtassa a **`azure network nsg rule create`** hozzon létre egy szabályt, amely lehetővé teszi, hogy az access port 1433 (SQL) az előtér-alhálózat parancsot.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Várt kimenet:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Futtassa a **`azure network nsg rule create`** , hogy megtagadja a hozzáférést az internethez szabály létrehozása parancsot.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Várható putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Futtassa a **`azure network nsg subnet add`** , amelyre a hivatkozás a NSG vissza vége az alhálózathoz parancsot.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Várt kimenet:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
