<properties
    pageTitle="A C# és erőforrás-kezelő sablon segítségével virtuális telepítése |} Microsoft Azure"
    description="Olvassa el a C# és erőforrás-kezelő sablon használatához az Azure virtuális terjesztése."
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
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Az Azure virtuális gép C# és erőforrás-kezelő sablon használatával terjesztése

Erőforrás-csoportokat és a sablonok használatával Ön kezelő összes erőforrás össze, amelyek támogatják az alkalmazást. Ez a cikk bemutatja, hogyan Visual Studio és C# hitelesítés beállítása, hozzon létre egy sablont, és majd telepítse az Azure az erőforrások segítségével létrehozott sablont.

Először győződjön meg arról, megtette beállítási lépéseket:

- Telepítse az [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- A [Windows Management Framework 3.0-s](http://www.microsoft.com/download/details.aspx?id=34595) vagy a [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) telepítésének ellenőrzéséhez
- A [hitelesítési jogkivonat](../resource-group-authenticate-service-principal.md) beszerzése
- Hozzon létre egy erőforrás az [Azure PowerShell](../resource-group-template-deploy.md), [Azure CLI](../resource-group-template-deploy-cli.md)vagy [Azure portálon](../resource-group-template-deploy-portal.md).

Kövesse az alábbi lépéseket körülbelül 30 percig tart.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Lépés: 1: A Visual Studio projekt, a sablonfájlt és a paraméterek fájl létrehozása

### <a name="create-the-template-file"></a>A sablon fájl létrehozása

Az erőforrás-kezelő Azure-sablon segítségével a, hogy üzembe helyezéséhez és kezeléséhez az Azure erőforrások együtt. A sablon () az erőforrások és a kapcsolódó telepítési paraméterek JSON leírását.

A Visual Studióban hajtsa végre az alábbi lépéseket:

1. Kattintson a **fájl** > **Új** > **Projekt**.

2. A **sablonok** > **Visual C#**, jelölje ki a **New**, írja be a projekt helyét és nevét, és kattintson **az OK**gombra.

3. Kattintson a jobb gombbal a projekt nevét a megoldást Intézőben, kattintson a **Hozzáadás** > **Új elemet**.

4. Kattintson a webhely, JSON-fájl kiválasztása, *VirtualMachineTemplate.json* adja meg a nevét, és kattintson a **Hozzáadás**gombra.

5. A nyitó és záró szögletes zárójel VirtualMachineTemplate.json fájl adja hozzá a szükséges séma elemet, valamint a szükséges contentVersion elem:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Paraméterek](../resource-group-authoring-templates.md#parameters) nem mindig kötelező, de azok kínál bemeneti értékek, ha telepíti a sablont. A paraméterek elemet, valamint az annak alárendelt elemeinek felvétele a contentVersion elem után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Változók](../resource-group-authoring-templates.md#variables) értékek, amelyek gyakran módosíthatja és értékek, amelyek létre kívánja hozni az adatok kombinációjának paraméter megadása sablon használható. A változók elem hozzáadása a paraméterek szakasz után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. A sablon ezután definiálva [erőforrások](../resource-group-authoring-templates.md#resources) , például a virtuális gép, a virtuális hálózat és a tárterület-fiókot. Az erőforrások szakasz hozzáadása a változók szakasz után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Mentse az Ön által létrehozott sablonfájl.

### <a name="create-the-parameters-file"></a>A paraméterek fájl létrehozása

Az erőforrás-paraméterek a sablonban definiált értékei megadásához használatosak, amikor a sablon telepítik értékeket tartalmazó paraméterek fájl létrehozása. A Visual Studióban hajtsa végre az alábbi lépéseket:

1. Kattintson a jobb gombbal a projekt nevét a megoldást Intézőben, kattintson a **Hozzáadás** > **Új elemet**.

2. Kattintson a webhely, JSON-fájl kiválasztása, *Parameters.json* adja meg a nevét, és kattintson a **Hozzáadás**gombra.

3. Nyissa meg a parameters.json fájlt, és vegye fel a JSON-tartalom:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Ez a cikk a Windows Server operációs rendszer verziója fut virtuális gép hoz létre. Más képek kiválasztásával kapcsolatos további tudnivalókért lásd: a [Navigálás és a Windows PowerShell és az Azure CLI választó Azure virtuális gép képekkel](virtual-machines-linux-cli-ps-findimage.md).

4. Mentse a létrehozott paraméterek fájlt.

## <a name="step-2-install-the-libraries"></a>Lépés: 2: Telepítse a tárakban

Azok a NuGet csomagok a legegyszerűbben a tárak, ebben az oktatóanyagban befejezéséhez szükséges telepítéséhez. Szükséges erőforrások létrehozása az Azure erőforrás könyvtár és Azure Active Directory Authentication Library van szüksége. Úgy juthat az ilyen tárakat a Visual Studióban, kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal a projekt nevét a megoldást Intéző, **NuGet csomagok kezelése**gombra, és kattintson a Tallózás gombra.

2. Írja be *Az Active Directory* a keresőmezőbe az Active Directory Authentication Library csomag a **telepítés** gombra, és kövesse az utasításokat követve telepítse a csomagot.

4. A lap tetején válassza a **Előzetes tartalmazza**. Típus *Microsoft.Azure.Management.ResourceManager* a keresőmezőbe, kattintson a **telepítse** a Microsoft Azure erőforrás Management tárak, és kövesse az utasításokat követve telepítse a csomagot.

Most már készen áll az alkalmazás létrehozása a tárakban használatának megkezdése.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Lépés 3: Hozzon létre hitelesítést végezni kérések használt hitelesítő adatok

Az Azure Active Directory-alkalmazás hoz létre, és a hitelesítési tár telepítve van. Most már formázhatja, de az alkalmazás hitelesítést végezni az Azure erőforrás-kezelő kérések használt hitelesítő adatokat.

1. Nyissa meg az Ön által létrehozott projekthez a Program.cs fájlt, és adja hozzá ezeket a fájl tetején kimutatások használatával:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Ez a módszer hozzá a Program osztály első a jogkivonat, a hitelesítő adatok létrehozásához szükséges:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Ügyfél-azonosító} cserélje ki az azonosító az Azure Active Directory-alkalmazást, {ügyfél-titkos} az Active Directory-alkalmazás, és a {bérlői-azonosító} access kulccsal az előfizetéshez tartozó bérlői azonosítóval. A bérlői azonosító Get-AzureRmSubscription futtatásával is megkeresheti. Az access billentyűt az Azure portal segítségével is megkeresheti.

3. A fő módszer a Program.cs fájlban kód hozzáadása a hitelesítő adatok létrehozásához:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Mentse a Program.cs fájlt.

## <a name="step-4-deploy-the-template"></a>Lépés: 4: A sablon terjesztése

Ebben a lépésben a korábban létrehozott erőforráscsoport használja, de a [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) és a [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) osztályok erőforráscsoport is létrehozhat.

1. Változók hozzáadása a fő módja a Program osztály adja meg a korábban létrehozott erőforrásnevek, a telepítési nevét és az előfizetés azonosítója:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    A csoport nevét, az erőforrás csoportnév értékének cserélje. DeploymentName értékének cserélje le a telepítéshez használni kívánt nevére. Előfizetés azonosítója Get-AzureRmSubscription futtatásával is megkeresheti.

2. Ez a módszer hozzá a Program osztály való telepítéséhez az erőforrások erőforráscsoport a megadott sablon használatával:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    A sablon egy tárterület-fiókból üzembe megy végbe, ha a sablon tulajdonság lecserélheti a TemplateLink tulajdonságot.

3. Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>5 lépés: Az erőforrások törlése

Mivel az Azure-ban használt erőforrások előfizetést terhelő, tanácsos mindig egy információforrások, amelyek már nem szükséges törlése. Nem kell az egyes erőforrások erőforráscsoport külön-külön törlése. Az erőforráscsoport törlése és összes erőforrás automatikusan törlődnek.

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

2.  Hívja fel a módszerrel, közvetlenül a felvétele után, kód hozzáadása a fő módszer:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Lépés a 6: Futtassa a New

1.  A konzol alkalmazásnak a futtatására, kattintson a **Start** a Visual Studióban, és jelentkezzen be az Azure Active Directory, a hitelesítő adatait használó-előfizetése segítségével.

2.  Nyomja le az **Enter** után az elfogadott állapotjelző jelenik meg.

    A befejezéshez öt perc az elejétől teljesen futtatásához konzol alkalmazáshoz kell vennie. Indítsa el az erőforrások törlése az Enter billentyű lenyomása előtt, törölje őket előtt ellenőrizendő pontok létrehozása az Azure-portálon erőforrások néhány perc is eltelhet.

3. Az erőforrások állapotának megtekintéséhez nyissa meg azt a naplókat, az Azure-portálon:

    ![Tallózással keresse meg a naplókat az Azure-portálon](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Következő lépések

- A telepítési problémák volt, a következő lépésben akkor is megtekintheti a [Hibaelhárítás erőforrás csoport telepítések Azure portálján](../resource-manager-troubleshoot-deployments-portal.md).
- Megtudhatja, hogy miként kezelheti a virtuális gép megtekintésével [kezelése virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-csharp-manage.md)létrehozott.
