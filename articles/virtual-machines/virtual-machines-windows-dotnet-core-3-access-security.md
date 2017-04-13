<properties
   pageTitle="A hozzáférési és Azure erőforrás-kezelő sablonok biztonsági |} Microsoft Azure" 
   description="Azure virtuális gép DotNet Core oktatóprogram"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Hozzáférés és Azure erőforrás-kezelő sablonok biztonsága

Alkalmazások valószínűleg Azure-ban tárolt kell hozzáférést az interneten vagy egy virtuális Magánhálózati / Azure Express útvonal kapcsolatot. A zene áruház alkalmazás mintával a webhely elérhetővé válik az interneten egy nyilvános IP-címet. Az Access-szel létrehozott az alkalmazás-kapcsolatot, és a virtuális gép erőforrásokhoz saját maguk titkosítani kell. Ez az access biztonsági a hálózati biztonsági csoport megadva. 

A dokumentum adatai a zene áruház alkalmazás a minta Azure erőforrás-kezelő sablonban titkosításának módját. Függőségek és a egyedi konfigurációk kiemelt. Az optimális használat érdekében léptetheti egy példánya az Azure előfizetést és a munka az erőforrás-kezelő Azure-sablon együtt a megoldást. A teljes sablon megtalálható itt – [Zenét áruházból telepítési Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).


## <a name="public-ip-address"></a>Nyilvános IP-cím

Az Azure-erőforrás nyilvános hozzáférést biztosít, egy nyilvános IP-cím erőforrás is használható. Nyilvános IP-cím egy statikus vagy dinamikus IP-címet is beállítható. Dinamikus címet használja, és a virtuális gép leállítása és felszabadítása, ha a címek törlődik. Ha a számítógép újra indítják, azt hozzárendelhető egy másik nyilvános IP-címet. IP-címének módosítása megakadályozásához egy fenntartott IP-cím használható. 

Egy nyilvános IP-cím felvehetők egy erőforrás-kezelő Azure sablon Visual Studio hozzáadása új erőforrás varázsló segítségével, illetve érvényes JSON beszúrásával sablonba. 

Hajtsa végre az erre a hivatkozásra kattintva megtekintheti a JSON minta belül az erőforrás-kezelő sablon – [Nyilvános IP-címet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Lehet, hogy egy nyilvános IP-címet, vagy egy virtuális hálózati kártya egy terheléselosztó társított. Ebben a példában mivel a zene tároló webhelyen betöltése több virtuális gépeken futó között a nyilvános IP-cím csatolva van a betöltése terheléselosztó.

Hajtsa végre az erre a hivatkozásra kattintva megtekintheti a JSON minta belül az erőforrás-kezelő sablon – [terheléselosztó nyilvános IP-cím társítva](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

A nyilvános IP-címet az Azure portálról látható módon. Figyelje meg, hogy a nyilvános IP-cím egy terheléselosztó és nem egy virtuális számítógépre van társítva. A következő dokumentumot a sorozat hálózati terheléselosztókkal részletezi.

![Nyilvános IP-cím](./media/virtual-machines-windows-dotnet-core/pubip-win.png)

Azure nyilvános IP-címek kapcsolatos további tudnivalókért olvassa el az [IP-címek Azure-ban](../virtual-network/virtual-network-ip-addresses-overview-arm.md)című témakört.

## <a name="network-security-group"></a>Hálózati biztonsági csoport

Miután access Azure erőforrások hoztak létre, a hozzáférés korlátozni kell. Az Azure virtuális gépeken futó biztonságos hozzáférés hajtható végre hálózati biztonsági csoport használata. A zene áruház alkalmazás mintával összes a virtuális gép teljes egészében kivéve a tiltott a http-elérés 80-as port vagy RDP access 3389 port fölé. Hálózati biztonsági csoport leendő egy erőforrás-kezelő Azure sablon Visual Studio hozzáadása új erőforrás varázsló segítségével, illetve érvényes JSON beszúrásával sablonba.

Kattintson erre a hivatkozásra, a JSON minta belül az erőforrás-kezelő sablon – a [Hálózat biztonsági csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57)megjelenítéséhez.

```none
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
},
```

Ebben a példában a hálózati biztonsági csoport az alhálózathoz objektummal deklarálva a virtuális hálózati erőforrás társítása. 

Hajtsa végre az erre a hivatkozásra kattintva megtekintheti a JSON minta az erőforrás-kezelő sablon – [hálózati biztonsági csoport szövetség virtuális hálózatán](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143)belül.


```none
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
```

Az alábbiakban a hálózati biztonsági csoport néz ki az Azure portálról. Figyelje meg, hogy egy NSG társíthat egy alhálózat és / vagy a hálózati kapcsolat. Ebben az esetben a NSG társítva alhálózat. Ebben a konfigurációban a bejövő szabályok vonatkoznak a alhálózathoz csatlakoztatott virtuális gépeken futó.

![Hálózati biztonsági csoport](./media/virtual-machines-windows-dotnet-core/nsg-win.png)

Részletesebb információkat a hálózati biztonsági csoportokat olvassa el a [Mi az hálózati biztonsági csoport]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/)című témakört.

## <a name="next-step"></a>Következő lépés

<hr>

[3 – elérhetőségéről és az Azure erőforrás-kezelő sablonok skála lépés](./virtual-machines-windows-dotnet-core-4-availability-scale.md)
