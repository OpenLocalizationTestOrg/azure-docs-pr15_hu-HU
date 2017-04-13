## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM sablon telepítése az Azure CLI használatával

ARM meg a letöltött sablont Azure CLI használatával telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa a **`azure config mode`** erőforrás-kezelő módban, az alább látható módon válthat parancsot.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Ha szükséges, futtassa a **`azure group create`** hozhat létre egy új erőforráscsoport, alább látható módon. Figyelje meg a parancs. A kimenet után megjelenik a paramétereket ismerteti. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](../articles/resource-group-overview.md).

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

4. Futtassa a **`azure group deployment create`** az új VNet telepítse a sablon és a paraméter segítségével parancsmag fájlok letöltött és a fenti módosított. A kimenet után megjelenik a paramétereket ismerteti.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (vagy – erőforrás-csoport)**. Az új VNet erőforrás csoport nevére a létrejön.
    - **az f-(vagy – sablon-fájl)**. Fájl elérési útját a ARM sablont.
    - **-e (vagy – paraméterek-fájl)**. A ARM paraméterek fájl elérési útja.

5. Futtassa a **`azure network vnet show`** parancs, amellyel az új vnet tulajdonságainak megtekintése az alább látható módon.

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
