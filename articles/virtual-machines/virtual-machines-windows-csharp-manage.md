<properties
    pageTitle="Az erőforrás-kezelő Azure és C# VMs kezelése |} Microsoft Azure"
    description="Az erőforrás-kezelő Azure és C# virtuális gépeken futó kezelése."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Azure virtuális gépeken futó Azure erőforrás-kezelő és a C kezelése#  

Ez a cikk a tevékenységek kezelése a virtuális gépeken futó, például indítása, leállítása és frissítése megjelenítése. A virtuális gép léteznie kell, egy erőforrás csoportban, a jelen cikkben a feladatokat.

Ebben a cikkben a feladatok elvégzéséhez az alábbiakra van szükség:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Egy hitelesítési jogkivonat](../resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Visual Studio projekt létrehozása és a csomagok telepítése

NuGet csomagok módon a legegyszerűbben a tárak, ez a cikk a tevékenységek befejezéséhez az kell telepíteni. A tárak, ez a cikk a telepített, hogy az Azure Active Directory Authentication Library és erőforrás-szolgáltató kiszámítania függvénytár. Hajtsa végre az alábbi módon nyithatja meg a tárakat a Visual Studio:

1. Kattintson a **fájl** > **Új** > **Projekt**.

2. A **sablonok** > **Visual C#**, jelölje be a **New**, írja be a projekt helyét és nevét, és kattintson **az OK**gombra.

3. Kattintson a jobb gombbal a projekt nevét a megoldást Intéző, és kattintson a **NuGet csomagok kezelése**gombra.

4. Írja be *Az Active Directory* a keresőmezőbe az Active Directory Authentication Library csomag a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

5. A lap tetején válassza a **Előzetes tartalmazza**. Típus *Microsoft.Azure.Management.Compute* a keresőmezőbe a kiszámítania .NET tárak kattintson a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

Most már készen áll a virtuális gépeken futó kezelése a tárak használatának megkezdése.

## <a name="set-up-the-project"></a>A projekt beállítása

Most, hogy az alkalmazás hoz létre, és a tárak telepítve van, hozzon létre egy token alkalmazás információk felhasználásával. Ez a token hitelesítést végezni az Azure erőforrás-kezelő kérések használják.

1. Nyissa meg az Ön által létrehozott projekthez a Program.cs fájlt, és adja hozzá ezeket a fájl tetején kimutatások használatával:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
        
2. Változók hozzáadása a fő módja a Program osztály adja meg az erőforrás-csoport nevét, és a nevét, valamint a virtuális gép az előfizetés azonosítója:

        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";

    Előfizetés azonosítója Get-AzureRmSubscription futtatásával is megkeresheti.
    
3. Úgy juthat a hitelesítő adatok létrehozásához szükséges jogkivonat, a Program osztály ezt a módszert hozzáadása:

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
    
4. A fő módszer Program.cs kód hozzáadása a hitelesítő adatok létrehozásához:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

5. Mentse a Program.cs fájlt.

## <a name="display-information-about-a-virtual-machine"></a>A virtuális gép adatainak megjelenítése

1. Ez a módszer hozzá a Program osztály a projekt, amely a korábban létrehozott:

        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");

          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);

          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);

          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);

          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);

          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }

          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }

          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }

          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
          
        }

2. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
    
3. Mentse a Program.cs fájlt.

4. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

    Ez a módszer futtatásakor meg kell jelennie Ez a példa hasonló:
    
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0

        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite

          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True

          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1

          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM

          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM

          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus

          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>A virtuális gép leállítása

A virtuális gép kétféleképpen leállíthatja. Virtuális gép leállítása és annak a beállításokat, de továbbra is azt kell fizetnie, vagy egy virtuális gép leállíthatja, és azt felszabadítása. Virtuális gép felszabadítása, amikor hozzárendelt összes erőforrás is felszabadítása és számlázási véget ér rá.

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }

    Ha azt szeretné, felszabadítása a virtuális gép, módosítsa a kód kikapcsolás-hívás:

        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

    Meg kell jelennie a virtuális gép módosítás leállítva állapotát. Ha futtatta a hívó Deallocate, az állapot leállítása módszer (felszabadítása).

## <a name="start-a-virtual-machine"></a>Indítsa el a virtuális gépen

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

    Meg kell jelennie a virtuális gépen futó átállítása állapotát.

## <a name="restart-a-running-virtual-machine"></a>Indítsa újra a futó virtuális gép

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

## <a name="resize-a-virtual-machine"></a>A virtuális gép átméretezése

Ez a példa bemutatja, hogyan futó virtuális gép méretének módosítása.

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

    Meg kell jelennie a virtuális gép változtatások Standard_A1 méretét.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Adatok lemezen hozzáadása egy virtuális számítógépre

Ez a példa bemutatja, hogyan futó virtuális gép adatok lemezen hozzáadni.

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

## <a name="delete-a-virtual-machine"></a>A virtuális gép törlése

1. Új megjegyzést kódrészleteket meg, hogy a korábban hozzáadott a fő módszerrel, kivéve a kód kinyeri a hitelesítő adatokat.

2. Ez a módszer hozzá a Program osztály:

        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();

4. Mentse a Program.cs fájlt.

5. Kattintson a **Start** a Visual Studióban, és jelentkezzen be az ugyanazon felhasználónév és jelszó, az előfizetés használt használatával Azure AD.

## <a name="next-steps"></a>Következő lépések

Ha vannak a telepítéssel kapcsolatos problémák, akkor előfordulhat, hogy tekintse meg [Hibaelhárítás erőforrás csoport telepítések Azure Portal segítségével](../resource-manager-troubleshoot-deployments-portal.md)
