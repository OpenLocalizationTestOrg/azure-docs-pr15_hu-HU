## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Hogyan hozhat létre egy VNet az Azure CLI használatával

Az Azure CLI az Azure erőforrások kezelése a parancssorból bármely Windows, Linux vagy OSX rendszerű számítógépről is használhatja. Egy VNet az Azure CLI létrehozásához kövesse az alábbi lépéseket.

1. Ha még soha nem használta az Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Szükség esetén futtassa az **azure csoport létrehozása** hozhat létre egy új erőforráscsoport alább látható módon. Figyelje meg a parancs. A kimenet után megjelenik a paramétereket ismerteti. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (vagy--neve)**. Az új erőforráscsoport nevét. A példában *TestRG*.
    - **-l (vagy--helyről)**. Azure terület, ahol az új erőforráscsoport létrejön. A példában *centralus*.

4. A parancsot **azure hálózati vnet hozzon létre** egy VNet és alhálózat, létrehozásához alább látható módon. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (vagy – erőforrás-csoport)**. Az erőforráscsoport, ahol a VNet létrejön neve. A példában *TestRG*.
    - **-n (vagy--neve)**. A létrehozandó VNet neve. A példában *TestVNet*
    - **-(vagy – cím-prefixumokban)**. A VNet címterületet használt CIDR blokkok listája. A példában *192.168.0.0/16*
    - **-l (vagy--helyről)**. Azure terület, ahol a VNet létrejön. A példában *centralus*.

5. Futtassa a alhálózat létrehozása az alább látható módon **azure alhálózathoz vnet létrehozása** parancsot. Figyelje meg a parancs. A kimenet után megjelenik a paramétereket ismerteti.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (vagy--vnet neve**. A VNet, ahol az alhálózathoz létrejön neve. A példában *TestVNet*.
    - **-n (vagy--neve)**. Az új alhálózat neve. A példában *FrontEnd*.
    - **-(vagy – cím-előtag)**. Alhálózat CIDR blokk. Négy a forgatókönyv, *192.168.1.0/24*.

6. Ismételje meg a fenti létrehozása más alhálózat, 5 szükség esetén. A példában a *Kódmentes* alhálózat létrehozásához az alábbi parancsot.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Futtassa az **azure hálózati vnet megjelenítése** parancsot az új vnet tulajdonságainak megtekintése, alább látható módon.

        azure network vnet show -g TestRG -n TestVNet

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
