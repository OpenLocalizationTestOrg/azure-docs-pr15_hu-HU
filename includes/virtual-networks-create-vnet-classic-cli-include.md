## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Hogyan hozhat létre egy klasszikus VNet Azure CLI használatával

Az Azure CLI az Azure erőforrások kezelése a parancssorból bármely Windows, Linux vagy OSX rendszerű számítógépről is használhatja. Egy VNet az Azure CLI létrehozásához kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../articles/xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. A parancsot **azure hálózati vnet hozzon létre** egy VNet és alhálózat, létrehozásához alább látható módon. A kimenet után megjelenik a paramétereket ismerteti.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Várt kimenet:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **– vnet**. A létrehozandó VNet neve. A példában *TestVNet*
    - **-e (vagy--- címterület)**. A címterület VNet. A példában *192.168.0.0*
    - **-i (vagy - cidr)**. Hálózati maszk CIDR formátumban. A példában *16*.
    - **- n (vagy--alhálózat neve**). Az első alhálózat neve. A példában *FrontEnd*.
    - **-p (vagy – ip-alhálózat-útmutató)**. Kezdő alhálózat vagy alhálózat címterület IP-címe. A példában *192.168.1.0*.
    - **-r (vagy – alhálózat-cidr)**. Hálózati maszk alhálózat CIDR formátumban. A példában *a 24*.
    - **-l (vagy--helyről)**. Azure terület, ahol a VNet létrejön. A példában *a központi USA -beli*.

3. Futtassa a alhálózat létrehozása az alább látható módon **azure alhálózathoz vnet létrehozása** parancsot. A kimenet után megjelenik a paramétereket ismerteti.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Az alábbiakban a várt eredménye a fenti parancs:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (vagy--vnet neve**. A VNet, ahol az alhálózathoz létrejön neve. A példában *TestVNet*.
    - **-n (vagy--neve)**. Az új alhálózat neve. A példában *Kódmentes*.
    - **-(vagy – cím-előtag)**. Alhálózat CIDR blokk. Négy a forgatókönyv, *192.168.2.0/24*.

4. Futtassa az **azure hálózati vnet megjelenítése** parancsot az új vnet tulajdonságainak megtekintése, alább látható módon.

            azure network vnet show

    Az alábbiakban a várt eredménye a fenti parancs:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
