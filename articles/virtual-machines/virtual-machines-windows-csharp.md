<properties
    pageTitle="Azure erőforrásoknak C# telepítése |} Microsoft Azure"
    description="Olvassa el a C# és Azure erőforrás-kezelő létrehozása a Microsoft Azure erőforrásokat."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="deploy-azure-resources-using-c"></a>Azure erőforrásoknak C terjesztése# 

Ez a cikk bemutatja, hogyan hozhat létre az Azure erőforrásoknak C#.

Először ellenőrizze, hogy befejezte az alábbi műveleteket:

- Telepítse az [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- A [Windows Management Framework 3.0-s](http://www.microsoft.com/download/details.aspx?id=34595) vagy a [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) telepítésének ellenőrzéséhez
- A [hitelesítési jogkivonat](../resource-group-authenticate-service-principal.md) beszerzése

Kövesse az alábbi lépéseket körülbelül 30 percig tart.

## <a name="step-1-create-a-visual-studio-project-and-install-the-libraries"></a>Lépés: 1: Visual Studio projekt létrehozása, és a tárak telepítése

Azok a NuGet csomagok a legegyszerűbben a tárak, ebben az oktatóanyagban befejezéséhez szükséges telepítéséhez. Ha a tárakban, hogy szüksége van a Visual Studióban, kövesse az alábbi lépéseket:

1. Kattintson a **fájl** > **Új** > **Projekt**.

2. A **sablonok** > **Visual C#**, jelölje be a **New**, írja be a projekt helyét és nevét, és kattintson **az OK**gombra.

3. Kattintson a jobb gombbal a projekt nevét a megoldást Intéző, és kattintson a **NuGet csomagok kezelése**gombra.

4. Írja be *Az Active Directory* a keresőmezőbe az Active Directory Authentication Library csomag a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

5. A lap tetején válassza a **Előzetes tartalmazza**. Típus *Microsoft.Azure.Management.Compute* a keresőmezőbe a kiszámítania .NET tárak kattintson a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

6. Típus *Microsoft.Azure.Management.Network* a keresőmezőbe a hálózati .NET tárak esetében kattintson a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

7. Típus *Microsoft.Azure.Management.Storage* a keresőmezőbe a tárhely .NET tárak kattintson a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

8. A Keresés mezőbe írja be a *Microsoft.Azure.Management.ResourceManager* , az erőforrás-kezelés tárak kattintson a **telepítés** gombra.

Most már készen áll az alkalmazás létrehozása a tárakban használatának megkezdése.

## <a name="step-2-create-the-credentials-that-are-used-to-authenticate-requests"></a>Lépés: 2: Hozzon létre hitelesítést végezni kérések használt hitelesítő adatok

Most már formázhatja, de az alkalmazás adatai korábban létrehozott be használt hitelesítő adatokat is kérések az Azure erőforrás-kezelő hitelesítést végezni.

1. Nyissa meg az Ön által létrehozott projekthez a Program.cs fájlt, és adja hozzá ezeket a fájl tetején kimutatások használatával:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Azure.Management.Storage;
        using Microsoft.Azure.Management.Storage.Models;
        using Microsoft.Azure.Management.Network;
        using Microsoft.Azure.Management.Network.Models;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;

2. A szükséges jogkivonat létrehozásához a Program osztály ezt a módszert hozzáadása:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }

    {Ügyfél-azonosító} cserélje ki az azonosító az Azure Active Directory-alkalmazást, {ügyfél-titkos} az Active Directory-alkalmazás, és a {bérlői-azonosító} access kulccsal az előfizetéshez tartozó bérlői azonosítóval. A bérlői azonosító Get-AzureRmSubscription futtatásával is megkeresheti. Az access billentyűt az Azure portal segítségével is megkeresheti.

3. Hívja fel a korábban hozzáadott módszer, kód hozzáadása a fő módszer a Program.cs fájl:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Mentse a Program.cs fájlt.

## <a name="step-3-register-the-resource-providers-and-create-the-resources"></a>3 lépés: Az erőforrás-szolgáltatók regisztrálni, és a szükséges erőforrások létrehozása

### <a name="register-the-providers-and-create-a-resource-group"></a>Regisztráció a szolgáltatók, és az erőforrás csoport létrehozása

Az erőforráscsoport összes erőforrás szerepelnie kell. Erőforrások hozzáadása csoporthoz, mielőtt az előfizetés rendelkeznie kell regisztrált az erőforrás-szolgáltatókkal való.

1. A fő módja a Program osztály adja meg a neveket, amelyeket az erőforrásokhoz tartozó használni kívánt változók felvétele:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var location = "location name";
        var storageName = "storage account name";
        var ipName = "public ip name";
        var subnetName = "subnet name";
        var vnetName = "virtual network name";
        var nicName = "network interface name";
        var avSetName = "availability set name";
        var vmName = "virtual machine name";  
        var adminName = "administrator account name";
        var adminPassword = "administrator account password";
        
    A változók értékein cserélje a nevek és azonosító, amelyet használni szeretne. Előfizetés azonosítója Get-AzureRmSubscription futtatásával is megkeresheti.

2. Az erőforráscsoport létrehozásához, és regisztráljon a szolgáltatók, a Program osztály ezt a módszert hozzáadása:

        public static async Task<ResourceGroup> CreateResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location)
        {
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
            
          Console.WriteLine("Registering the providers...");
          var rpResult = resourceManagementClient.Providers.Register("Microsoft.Storage");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Network");
          Console.WriteLine(rpResult.RegistrationState);
          rpResult = resourceManagementClient.Providers.Register("Microsoft.Compute");
          Console.WriteLine(rpResult.RegistrationState);
          
          Console.WriteLine("Creating the resource group...");
          var resourceGroup = new ResourceGroup { Location = location };
          return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
        }

3. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módszer:

        var rgResult = CreateResourceGroupAsync(
          credential,
          groupName,
          subscriptionId,
          location);
        Console.WriteLine(rgResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

### <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása

A [tárterület-fiók](../storage/storage-create-storage-account.md) van szükség a virtuális gép jön létre virtuális merevlemez fájlt tárolja.

1. A Program osztály ezt a módszert hozzáadása a tárterület-fiók létrehozásához:

        public static async Task<StorageAccount> CreateStorageAccountAsync(
          TokenCredentials credential,       
          string groupName,
          string subscriptionId,
          string location,
          string storageName)
        {
          Console.WriteLine("Creating the storage account...");
          var storageManagementClient = new StorageManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await storageManagementClient.StorageAccounts.CreateAsync(
            groupName,
            storageName,
            new StorageAccountCreateParameters()
            {
              Sku = new Microsoft.Azure.Management.Storage.Models.Sku() 
                { Name = SkuName.StandardLRS},
              Kind = Kind.Storage,
              Location = location
            }
          );
        }

2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módja a Program osztály:

        var stResult = CreateStorageAccountAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          storageName);
        Console.WriteLine(stResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-public-ip-address"></a>A nyilvános IP-cím létrehozása

Egy nyilvános IP-cím használatával kommunikál a virtuális gépre van szükség.

1. A Program osztály ezt a módszert hozzáadása a nyilvános IP-címet a virtuális gép létrehozásához:

        public static async Task<PublicIPAddress> CreatePublicIPAddressAsync(
          TokenCredentials credential,  
          string groupName,
          string subscriptionId,
          string location,
          string ipName)
        {
          Console.WriteLine("Creating the public ip...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await networkManagementClient.PublicIPAddresses.CreateOrUpdateAsync(
            groupName,
            ipName,
            new PublicIPAddress
            {
              Location = location,
              PublicIPAllocationMethod = "Dynamic"
            }
          );
        }

2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módja a Program osztály:

        var ipResult = CreatePublicIPAddressAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          ipName);
        Console.WriteLine(ipResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-the-virtual-network"></a>A virtuális hálózat létrehozása

Virtuális hálózatot, hogy az erőforrás-kezelő telepítési modell jön létre virtuális gép kell lennie.

1. A Program osztály ezt a módszert hozzáadása alhálózat és egy virtuális hálózati létrehozásához:

        public static async Task<VirtualNetwork> CreateVirtualNetworkAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string vnetName,
          string subnetName)
        {
          Console.WriteLine("Creating the virtual network...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          
          var subnet = new Subnet
          {
            Name = subnetName,
            AddressPrefix = "10.0.0.0/24"
          };
          
          var address = new AddressSpace {
            AddressPrefixes = new List<string> { "10.0.0.0/16" }
          };
          
          return await networkManagementClient.VirtualNetworks.CreateOrUpdateAsync(
            groupName,
            vnetName,
            new VirtualNetwork
            {
              Location = location,
              AddressSpace = address,
              Subnets = new List<Subnet> { subnet }
            }
          );
        }
        
2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módja a Program osztály:

        var vnResult = CreateVirtualNetworkAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          vnetName,
          subnetName);
        Console.WriteLine(vnResult.Result.ProvisioningState);  
        Console.ReadLine();
        
### <a name="create-the-network-interface"></a>A hálózati kapcsolat létrehozása

Virtuális gép van szüksége a hálózati illesztő kapcsolatba lépni a virtuális hálózaton.

1. A hálózati kapcsolat létrehozásához a Program osztály ezt a módszert hozzáadása:

        public static async Task<NetworkInterface> CreateNetworkInterfaceAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string subnetName,
          string vnetName,
          string ipName,
          string nicName)
        {
          Console.WriteLine("Creating the network interface...");
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var subnetResponse = await networkManagementClient.Subnets.GetAsync(
            groupName,
            vnetName,
            subnetName
          );
          var pubipResponse = await networkManagementClient.PublicIPAddresses.GetAsync(groupName, ipName);

          return await networkManagementClient.NetworkInterfaces.CreateOrUpdateAsync(
            groupName,
            nicName,
            new NetworkInterface
            {
              Location = location,
              IpConfigurations = new List<NetworkInterfaceIPConfiguration>
              {
                new NetworkInterfaceIPConfiguration
                {
                  Name = nicName,
                  PublicIPAddress = pubipResponse,
                  Subnet = subnetResponse
                }
              }
            }
          );
        }

2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módja a Program osztály:

        var ncResult = CreateNetworkInterfaceAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          subnetName,
          vnetName,
          ipName,
          nicName);
        Console.WriteLine(ncResult.Result.ProvisioningState);  
        Console.ReadLine();

### <a name="create-an-availability-set"></a>Hozzon létre egy elérhetősége

Elérhetőség készletek megkönnyítik a kezelése a virtuális gépeken futó az alkalmazás által használt fenntartása.

1. Szeretne létrehozni az elérhetőség, a Program osztály ezt a módszert hozzáadása:

        public static async Task<AvailabilitySet> CreateAvailabilitySetAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId,
          string location,
          string avsetName)
        {
          Console.WriteLine("Creating the availability set...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          return await computeManagementClient.AvailabilitySets.CreateOrUpdateAsync(
            groupName,
            avsetName,
            new AvailabilitySet()
            {
              Location = location
            }
          );
        }

2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módja a Program osztály:

        var avResult = CreateAvailabilitySetAsync(
          credential,  
          groupName,
          subscriptionId,
          location,
          avSetName);
        Console.ReadLine();

### <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Most, hogy a támogató erőforrások hozta létre, létrehozhat egy virtuális számítógépre.

1. A virtuális gép létrehozásához a Program osztály ezt a módszert hozzáadása:

        public static async Task<VirtualMachine> CreateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName,
          string subscriptionId,
          string location,
          string nicName,
          string avsetName,
          string storageName,
          string adminName,
          string adminPassword,
          string vmName)
        {
          var networkManagementClient = new NetworkManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var nic = networkManagementClient.NetworkInterfaces.Get(groupName, nicName);

          var computeManagementClient = new ComputeManagementClient(credential);
          computeManagementClient.SubscriptionId = subscriptionId;
          var avSet = computeManagementClient.AvailabilitySets.Get(groupName, avsetName);

          Console.WriteLine("Creating the virtual machine...");
          return await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(
            groupName,
            vmName,
            new VirtualMachine
            {
              Location = location,
              AvailabilitySet = new Microsoft.Azure.Management.Compute.Models.SubResource
              {
                Id = avSet.Id
              },
              HardwareProfile = new HardwareProfile
              {
                VmSize = "Standard_A0"
              },
              OsProfile = new OSProfile
              {
                AdminUsername = adminName,
                AdminPassword = adminPassword,
                ComputerName = vmName,
                WindowsConfiguration = new WindowsConfiguration
                {
                  ProvisionVMAgent = true
                }
              },
              NetworkProfile = new NetworkProfile
              {
                NetworkInterfaces = new List<NetworkInterfaceReference>
                {
                  new NetworkInterfaceReference { Id = nic.Id }
                }
              },
              StorageProfile = new StorageProfile
              {
                ImageReference = new ImageReference
                {
                  Publisher = "MicrosoftWindowsServer",
                  Offer = "WindowsServer",
                  Sku = "2012-R2-Datacenter",
                  Version = "latest"
                },
                OsDisk = new OSDisk
                {
                  Name = "mytestod1",
                  CreateOption = DiskCreateOptionTypes.FromImage,
                  Vhd = new VirtualHardDisk
                  {
                    Uri = "http://" + storageName + ".blob.core.windows.net/vhds/mytestod1.vhd"
                  }
                }
              }
            }
          );
        }

    >[AZURE.NOTE] Ebben az oktatóprogramban egy virtuális gép, a Windows Server operációs rendszer verziója fut hoz létre. Más képet kiválasztásával kapcsolatos további tudnivalókért lásd: a [Navigálás és a Windows PowerShell és az Azure CLI választó Azure virtuális gép képekkel](virtual-machines-linux-cli-ps-findimage.md).

2. Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módszer:

        var vmResult = CreateVirtualMachineAsync(
          credential,
          groupName,
          subscriptionId,
          location,
          nicName,
          avSetName,
          storageName,
          adminName,
          adminPassword,
          vmName);
        Console.WriteLine(vmResult.Result.ProvisioningState);
        Console.ReadLine();

##<a name="step-4-delete-the-resources"></a>Lépés: 4: Az erőforrások törlése

Mivel az Azure-ban használt erőforrások előfizetést terhelő, tanácsos mindig egy információforrások, amelyek már nem szükséges törlése. Ha törölni szeretné a virtuális gépeken futó és a támogató erőforrások, kell elvégeznie mindössze az erőforráscsoport törlése.

1.  A Program osztály ezt a módszert hozzá az erőforráscsoport törléséhez:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Hívja fel a korábban hozzáadott módszer, a kód hozzáadása a fő módszer:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

## <a name="step-5-run-the-console-application"></a>5 lépés: A konzol alkalmazásnak a futtatására

1. A New futtatásához kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

2. Nyomja le az **ENTER billentyűt** minden állapotkód jelez minden erőforrás létrehozása után. A virtuális gép létrehozása után hajtsa végre a következő lépésben lenyomja az Enter billentyűt minden erőforrások törlése előtt.

    A befejezéshez öt perc az elejétől teljesen futtatásához konzol alkalmazáshoz kell vennie. Indítsa el az erőforrások törlése az Enter billentyű lenyomása előtt, törölje őket előtt ellenőrizendő pontok létrehozása az Azure-portálon erőforrások néhány perc is eltelhet.

3. Az erőforrások állapotának megtekintéséhez nyissa meg azt a naplókat, az Azure-portálon:

    ![Tallózással keresse meg a naplókat az Azure-portálon](./media/virtual-machines-windows-csharp/crpportal.png)
    
## <a name="next-steps"></a>Következő lépések

- Információk [az Azure virtuális gép Deploy](virtual-machines-windows-csharp-template.md)a C# és erőforrás-kezelő sablon segítségével virtuális gép létrehozása sablon használatával előnyeit.
- Megtudhatja, hogy miként kezelheti a virtuális gép megtekintésével [kezelése virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-csharp-manage.md)létrehozott.
