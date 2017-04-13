<properties
   pageTitle="Azure erőforrás-kezelő sablonokkal erőforrások üzembe helyezése kiszámítania |} Microsoft Azure"
   description="Azure virtuális gép DotNet Core oktatóprogram"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Erőforrás-kezelő Azure sablonokkal alkalmazás architektúra

Az erőforrás-kezelő Azure környezetben fejlesztésekor számítási követelmények kell Azure erőforrásokat és szolgáltatásokat kell megfeleltetni. Ha egy alkalmazás több http végpontok, adatbázis és gyorsítótár-szolgáltatás adatok áll, az Azure erőforrások állomáson, minden összetevő kell rationalized kell. A minta zenét áruház alkalmazás például webalkalmazás virtuális gépen üzemelteti és SQL-adatbázishoz, amely Azure SQL-adatbázisban üzemelteti tartalmazza. 

A dokumentum adatai a zene tárolási számítási erőforrásokat a minta Azure erőforrás-kezelő sablonban beállításától. Függőségek és a egyedi konfigurációk kiemelt. Az optimális használat érdekében léptetheti egy példánya az Azure előfizetést és a munka az erőforrás-kezelő Azure-sablon együtt a megoldást. A teljes sablon megtalálható itt – [Zenét áruházból telepítési a Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="virtual-machine"></a>Virtuális gépen

A zene-tár alkalmazás hol ügyfelek és vásárolhat zenét webalkalmazás tartalmazza. Ebben a példában a webalkalmazások üzemeltetheti több Azure szolgáltatás várakozó virtuális gép használják. A minta zenét tár sablon használatára, egy virtuális számítógépre telepítve van, webkiszolgálóra telepítése, és a zene tároló webhelyen telepítette és beállította. Az Ez a cikk csak a virtuális gép példányban részletes. Az érintett webkiszolgálóra, és az alkalmazás beállításait egy újabb cikk részletesen.

A virtuális gép egy sablont, a Visual Studio, hozzáadása új erőforrás varázslóval, vagy a telepítési sablonba érvényes JSON beszúrásával adhatók meg. A virtuális gép telepítésekor több kapcsolódó erőforrásokat is van szükség. Ha a sablon létrehozása a Visual Studio segítségével, ezek az erőforrások jön létre. Ha manuális megépítése a sablont, ezek az erőforrások szükséges beszúrt és a beállítva.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Virtuális gép JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295)belül a JSON minta megjelenítéséhez.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
      ........<truncated>  
    }
```

Miután telepített, a virtuális gép tulajdonságai az Azure-portálon is láthatja.

![Virtuális gépen](./media/virtual-machines-linux-dotnet-core/vm.png)

## <a name="storage-account"></a>Tárterület-fiók

Tárterület-fiókok számos tároló beállítások és lehetőségek van. Az Azure virtuális gépeken futó környezetben egy tárterület-fiókkal rendelkezik a virtuális gép és bármilyen további adatokat lemezre virtuális merevlemezeken lévő. A zene áruházból minta virtuális gépeken virtuális merevlemez-meghajtó tartása a példányban fiókok tárolási tartalmazza. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Tárterület számla](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109)belüli JSON minta megjelenítéséhez.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

A tárterület-fiók társítása egy virtuális gép az erőforrás-kezelő sablon nyilatkozat a virtuális gép belül a. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [virtuális gép és tároló fiók társítási](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341)belül a JSON minta megjelenítéséhez.

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

A telepítés után a tárterület-fiók megtekinthetők az Azure-portálon.

![Tárterület-fiók](./media/virtual-machines-linux-dotnet-core/storacct.png)

Kattintás a tárolási fiók blob-tároló be, a merevlemez-meghajtó fájlt a sablonnal rendszerbe virtuális gépeken is láthatja.

![Merevlemez-meghajtók](./media/virtual-machines-linux-dotnet-core/vhd.png)

Azure tároló kapcsolatos további tudnivalókért [Azure tároló](https://azure.microsoft.com/documentation/services/storage/)dokumentációjában olvasható.

## <a name="virtual-network"></a>Virtuális hálózati

Ha egy virtuális számítógépre van szüksége, például az azt jelenti, hogy más virtuális gépeken futó és Azure erőforrások kommunikálni belső hálózati, az Azure virtuális hálózaton szükség.  A virtuális hálózati nem a virtuális gép elérhetővé tétele az interneten keresztül. Nyilvános egy nyilvános IP-címet, amelyeket később a sorozat részletes szükséges.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [virtuális hálózati és alhálózat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136)belül a JSON minta megjelenítéséhez.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Az Azure portálról a virtuális hálózat néz ki az alábbi képen. Figyelje meg, hogy a sablonnal rendszerbe virtuális gépeken futó a virtuális hálózat vannak csatolva.

![Virtuális hálózati](./media/virtual-machines-linux-dotnet-core/vnet.png)

## <a name="network-interface"></a>Hálózati kapcsolat

 A hálózati kapcsolat virtuális hálózaton, konkrétan a virtuális hálózaton definiált alhálózat virtuális gép csatlakozik. 
 
 Kattintson erre a hivatkozásra, a JSON minta belül az erőforrás-kezelő sablon – a [Hálózati kapcsolat](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166)megtekintéséhez.
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig1",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Az egyes virtuális gép erőforrások hálózati profil tartalmazza. A hálózati kapcsolaton társítva a profil virtuális gép.  

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Virtuális gép hálózati profil](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350)belül a JSON minta megjelenítéséhez.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Az Azure portálról a hálózati kapcsolaton néz ki az alábbi képen. A belső IP-cím és a virtuális gép társítás a hálózati kapcsolat erőforráson is láthatja.

![Hálózati kapcsolat](./media/virtual-machines-linux-dotnet-core/nic.png)

További információt a Azure virtuális hálózatokon [Azure virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/)dokumentációjában olvasható.

## <a name="azure-sql-database"></a>Azure SQL-adatbázis

Nemcsak a virtuális géphez üzemeltető a zene Store webhelyet Azure SQL-adatbázis telepíti, a zene Store-adatbázist. A használatának Azure SQL-adatbázis itt előnye, hogy virtuális gépeken futó második csoportja nem szükséges, és a szolgáltatás beépített méretezése és elérhetőség.

Az Azure SQL-adatbázis használata a Visual Studio hozzáadása új erőforrás varázsló, és a sablonba érvényes JSON beszúrásával adhatók meg. Az SQL Server-erőforrás tartalmaz felhasználónevet és jelszót, amelyet az SQL-példány a rendszergazdai jogokkal adják. Ezenkívül SQL tűzfal erőforrás kerül. Alapértelmezés szerint Azure-ban tárolt alkalmazások képesek az SQL-példány való csatlakozáskor kap. Ahhoz, hogy külső alkalmazás ilyen egy SQL Server Management studio csatlakozhat az SQL-példányt, a tűzfal kell beállítania. A zene-tár videoklip az alapértelmezett beállítás nem kell aggódnia. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő – az [Azure SQL-adatbázis](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401)sablon a JSON minta megjelenítéséhez.


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Az SQL server és az Azure-portálon naplójában MusicStore adatbázis nézete.

![Az SQL Server](./media/virtual-machines-linux-dotnet-core/sql.png)

További tájékoztatást a telepíti az SQL Azure-adatbázis dokumentációjában olvasható [Az SQL Azure-adatbázis](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Következő lépés

<hr>

[Lépés: 2 - elérhetőségének és biztonságának Azure erőforrás-kezelő sablonok](./virtual-machines-linux-dotnet-core-3-access-security.md)
