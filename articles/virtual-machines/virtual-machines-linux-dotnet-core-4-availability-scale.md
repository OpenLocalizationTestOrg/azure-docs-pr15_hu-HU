<properties
   pageTitle="Elérhetőség és Azure erőforrás-kezelő sablonok skála |} Microsoft Azure"
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

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Elérhetőség és Azure erőforrás-kezelő sablonok skála

Elérhetőség és arányának olvassa el az üzemidőt és az azt jelenti, hogy eleget igény szerint. Ha az alkalmazás az idő 99,9 %-át kell lennie, azt, amely lehetővé teszi, hogy több egyidejű számítási erőforrás-architektúra kell szerepelnie. Ahelyett, hogy egyetlen webhely, például a konfiguráció elérhetősége magasabb szintű ugyanazon a helyen, a terheléselosztás elé őket technológia több példányával tartalmazza. Ebben a konfigurációban az alkalmazás egy példánya tehetők karbantartás, miközben a fennmaradó továbbra is működnek. Méretezés azonban hivatkozik-alkalmazások azt jelenti, hogy az igény szerinti szolgálnak. A terhelést kiegyensúlyozott alkalmazást, hozzáadásával és eltávolításával példányok a készletből lehetővé teszi, hogy egy alkalmazást, ha át kívánja méretezni, hogy megfeleljenek az igény szerinti.

A dokumentum adatai a zene áruházból minta példányban elérhetősége és skála beállításaitól. Függőségek és a egyedi konfigurációk kiemelt. Az optimális használat érdekében léptetheti egy példánya az Azure előfizetést és a munka az erőforrás-kezelő Azure-sablon együtt a megoldást. A teljes sablon megtalálható itt – [Zenét áruházból telepítési a Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Elérhetőség beállítása

Az elérhetőség beállítása logikailag terjed ki Azure virtuális gépeken futó fizikai hosts és a más infrastrukturális összetevők, például power éppen a Kellékek és fizikai hálózati eszközt. Elérhetőség készletek győződjön meg arról, hogy közben a karbantartás, az eszköz nem vagy más idő lefelé, nem az összes virtuális gépeken futó történik. Az erőforrás-kezelő Azure sablon Visual Studio hozzáadása új erőforrás varázsló segítségével vagy érvényes JSON beszúrásával sablon egy elérhetőségének beállítása bővíthető.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Elérhetőségének beállítása](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387)a JSON minta megjelenítéséhez.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Az elérhetőség beállítása egy virtuális gép erőforrás tulajdonság deklarálva. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [virtuális gép elérhetőségének beállítása társítás](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313)belül a JSON minta megjelenítéséhez.


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Megadása esetén az Azure portál elérhető. Minden egyes virtuális gép és a konfigurációs olvashat az alábbi részletes.

![Elérhetőség beállítása](./media/virtual-machines-linux-dotnet-core/aset.png)

Részletesebb információt a elérhetőségének beállítása című témakörben talál [virtuális gépeken futó elérhetőségének kezelése](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Hálózati terheléselosztó

Mivel az elérhetőség beállítása alkalmazás hibatűrést biztosít, egy terheléselosztó elérhetővé tétele az alkalmazás hány példánya egyetlen hálózat címen. Sok virtuális gépeken futó, egyenként csatlakozik egy terheléselosztó is szerepeltethetők több példányának egy alkalmazást. Érhető el az alkalmazást, mint a terheléselosztó irányítja a beérkező kérelem a végig a csatolt tagok. Egy terheléselosztó felvehetők a Visual Studio hozzáadása új erőforrás varázslóval, vagy beszúrásával megfelelően formázott JSON erőforrás az erőforrás-kezelő Azure sablonba.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – a [Hálózat terheléselosztó](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208)belül a JSON minta megjelenítéséhez.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

A minta alkalmazás van téve egy nyilvános IP-címet az internethez, mivel erre a címre a terheléselosztó társítva. 

Hajtsa végre az erre a hivatkozásra kattintva megtekintheti a JSON minta belül az erőforrás-kezelő sablon – a [hálózat terheléselosztó társítási nyilvános IP-címet](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Az Azure portálról a hálózati betöltés terheléselosztó áttekintése a nyilvános IP-címet a társítás jeleníti meg.

![Hálózati terheléselosztó](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Betöltési terheléselosztó szabály

Amikor egy terheléselosztó, szabályok, amelyek meghatározzák, hogy hogyan forgalmat a tervezett erőforrások kiegyensúlyozását megtörténik. A minta zenét áruház alkalmazás forgalom a 80-as port a nyilvános IP-címének megérkezik, és át az összes virtuális gépeken futó 80-as port van meghatározva. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Betöltés terheléselosztó szabály](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)belül a JSON minta megjelenítéséhez.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

A hálózati betöltés terheléselosztó szabály a portálról nézete.

![Hálózati betöltés terheléselosztó szabály](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Betöltési terheléselosztó vizsgálati

A terheléselosztó is kell figyelni a virtuális gépeken, hogy csak a rendszert futtató szolgáltatott kérések. Ez az ellenőrzés esedékes, állandó számlálása egy előre definiált port. 80-as port összes része virtuális gépeken probe a zene-tár példányban van beállítva. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Betöltés terheléselosztó Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257)belül a JSON minta megjelenítéséhez.


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

A betöltés terheléselosztó vizsgálati az Azure portálról láthatók.

![Hálózati betöltés terheléselosztó vizsgálati](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Bejövő hálózati Címfordítást szabályok

Amikor egy betöltése terheléselosztó, szabályok kell, amely nem betöltés kiegyensúlyozott hozzáférést biztosítanak virtuális gépeken bevezetett. Például SSH internetkapcsolattal rendelkező virtuális gépeken létrehozásakor a forgalom kell nem osztható, inkább egy előre meghatározott elérési utat kell beállítani. előre meghatározott elérési utak állíthatók be egy bejövő hálózati Címfordítást szabály erőforrás. Ez az erőforrás használ, adatforgalmat csatolható egyes virtuális gépeken futó. 

A zene áruház alkalmazás olyan portot 5000 karaktertől kezdve SSH access virtuális gépeken 22 portjához van rendelve. A `copyindex()` függvényt használja a bejövő port növelni, úgy, hogy a második virtuális gépen: 5001, a harmadik 5002-bejövő portja kap, és így tovább.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Hálózati Címfordítást szabályok bejövő](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270)eső JSON minta megjelenítéséhez. 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Egy példa a bejövő hálózati Címfordítást szabály, az Azure-portálon látható módon. A példányban virtuális gépeken létrejön egy SSH hálózati Címfordítást szabályt.

![Bejövő hálózati Címfordítást szabály](./media/virtual-machines-linux-dotnet-core/natrule.png)

Részletesebb információt a Azure hálózati terheléselosztó a című témakörben talál [az Azure infrastruktúrájának szolgáltatásai terheléselosztás](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Több VMs terjesztése

Végül a elérhetőségének beállítása vagy terheléselosztó hatékony működését, több virtuális gépeken futó szükség. Több VMs telepítheti az Azure erőforrás-kezelő sablon másolat funkcióval. A másolás funkció használata nem szükséges virtuális gépeken futó függvényt számos meghatározni, inkább ezt az értéket dinamikusan biztosítható, hogy a telepítés során. A másolás funkció a létrehozandó példányainak száma és üzembe helyezése a virtuális gépeken futó és kapcsolódó erőforrásokat megfelelő számának fogópontokkal fogyaszt.

Zene áruházból űrlapsablonok paraméter van megadva, hogy mi a-példányok száma. Ennek a számnak virtuális gépeken futó és a kapcsolódó források létrehozásakor használatos egész a sablont.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

A virtuális gép erőforráson másolás le kap nevét és a példányok paraméter használatával megadható, az eredményül kapott példányszámot számát.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Virtuális gép másolása funkció](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300)a JSON minta megjelenítéséhez. 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

A Másolás függvény az aktuális közelítését a is elérhető a `copyIndex()` függvény. A Másolás index függvény értéke virtuális gépeken futó és más erőforrások: a név használható. Például a virtuális gép két példánya van telepítve, ha szeretnék különböző neveket. A `copyIndex()` függvény használható a virtuális számítógépnév részeként hozzon létre egy egyedi nevet. Példa a `copyindex()` elnevezési célra használt függvény a virtuális gép erőforrás is láthatja. Ebben az esetben a számítógép neve-e egy összefűzése a `vmName` paraméter, és a `copyIndex()` függvény. 

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – a [Másolatot Index függvény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319)a JSON minta megjelenítéséhez. 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

A `copyIndex` függvény többször szerepel a zene-tár minta sablont. Erőforrások és a függvények felhasználásával `copyIndex` vegye fel az adott egy példányát a virtuális gép, például a hálózati kapcsolat, betöltés terheléselosztó szabályokat, hogy semmit, és bármelyik funkciók attól függ. 

A másolási funkciót a további tudnivalókért olvassa el a [létrehozása az Azure erőforrás-kezelő erőforrások több példányával](../resource-group-create-multiple.md)című témakört.

## <a name="next-step"></a>Következő lépés

<hr>

[Lépés: 4 – az alkalmazás környezet, amelyben az Azure erőforrás-kezelő sablonok](./virtual-machines-linux-dotnet-core-5-app-deployment.md)