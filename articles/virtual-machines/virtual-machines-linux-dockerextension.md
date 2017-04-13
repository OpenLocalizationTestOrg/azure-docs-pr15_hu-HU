<properties
   pageTitle="Azure Docker virtuális bővítményében |} Microsoft Azure"
   description="Megtudhatja, hogy miként használatával a Docker virtuális bővítmény egyszerűen és biztonságos módon egy Docker környezet erőforrás-kezelő sablonok használata Azure-ban."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Hozzon létre egy Docker környezet Docker virtuális bővítményében Azure-ban
Docker pedig a népszerű tároló management Képkezelő platform, amely lehetővé teszi, hogy gyorsan használata tárolók Linux (és a Windows, valamint). Az Azure-módon különböző Docker telepítheti az igényeinek megfelelően. Ez a cikk koncentrál Docker virtuális kiterjesztése és Azure erőforrás-kezelő sablonok használatával. 

A különböző telepítési módszereit, beleértve a Docker gépi és Azure tároló szolgáltatások használatával kapcsolatos további tudnivalók az alábbi cikkekben talál:

- Gyorsan prototípusának alkalmazás egy Docker állomáshoz [Docker gépi](./virtual-machines-linux-docker-machine.md)használatával hozhat létre.
- Nagyobb, több stabil környezetben is támogat a [Docker írása](https://docs.docker.com/compose/overview/) egységes tároló telepítések létrehozásához bővítménnyel Azure Docker virtuális is használhatja. Ez a cikk részletei az Azure Docker virtuális bővítmény használatával.
- Adja meg a további ütemezés- és felügyeleti eszközök gyártási kész, méretezhető környezetek összeállításához telepítheti a [Docker Swarm fürt Azure tároló Services](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Azure virtuális Docker kiterjesztés – áttekintés
Az Azure Docker virtuális bővítmény telepítése, és konfigurálja a Docker démon, Docker ügyfél, valamint Docker írása a Linux virtuális gépen (virtuális). Az Azure Docker virtuális kiterjesztés használatával, ha további vezérlők, és funkciók, mint egyszerűen Docker gépi használ, vagy a Docker host létrehozása saját kezűleg. Ezen további funkciókat, például [Docker írása](https://docs.docker.com/compose/overview/), ellenőrizze az Azure Docker virtuális bővítmény megbízhatóbb fejlesztő vagy gyártási környezetekben meghatározáshoz.

Azure erőforrás-kezelő sablonok a környezet teljes szerkezetének megadása Sablonok létrehozása és konfigurálása az erőforrások, például a Docker host VMs, a tárhely, a szerepköralapú hozzáférés vezérlők (RBAC) és a diagnosztika teszi lehetővé. Ezek a sablonok létrehozása további telepítések következetesen újra felhasználhatja. További információt az Azure erőforrás-kezelő és a sablonok az [erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat. 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Az Azure Docker virtuális kiterjesztéssel sablon terjesztése
Quickstart útmutató meglévő sablon használatával hozzon létre egy Ubuntu virtuális az Azure Docker virtuális bővítmény telepítése és beállítása a Docker host használó. Megtekintheti a sablon itt: [egy Ubuntu virtuális Docker az egyszerű telepítését](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Telepítve van, és az erőforrás-kezelő-üzemmód használata az alábbi képlettel történik-e jelentkezve a [Legújabb Azure CLI](../xplat-cli-install.md) lesz szüksége:

```
azure config mode arm
```

A sablon használatával az Azure CLI, adja meg a sablon URI üzembe. Az alábbi példa létrehoz egy névvel ellátott erőforráscsoport `myResourceGroup` a a `WestUS` helyét. A saját erőforráscsoport nevét és helyét használja az alábbiak szerint:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Válaszolja meg a képernyőn megjelenő utasításokat a tárterület-fiók neve, felhasználónév és jelszó megadására, és küldje el a DNS-nevét. A kimenet szövege a következőhöz hasonló:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Az Azure CLI beolvasása a kérdésre, csupán néhány másodpercet, de a Docker szolgáltató után továbbra is létrehozott és a az Azure Docker virtuális bővítmény állítható be. A telepítés befejezéséhez néhány percet vesz igénybe. A Docker host állapot használatával kapcsolatos részleteket megtekintheti a `azure vm show` parancsot.

Az alábbi példa a virtuális nevű állapotának ellenőrzése `myDockerVM` (az alapértelmezett nevet a sablonból – ne módosítsa a név) az erőforráscsoport nevű `myResourceGroup`. Írja be a csoport nevét, az erőforrás az előző lépésben létrehozott:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

A kimenet: a `azure vm show` parancs hasonlít az alábbi példában:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

A kimenet tetején látható a `ProvisioningState` a virtuális gép. Amikor megjelenik `Succeeded`, a telepítés befejeződött, és a virtuális gép SSH is.

A kimenet, vége felé `FQDN` megjeleníti a Docker szolgáltató teljesen minősített tartománynév nevét. A teljesen minősített tartománynév, mit használatával SSH a Docker állomáshoz a hátralévő lépéseket.


## <a name="deploy-your-first-nginx-container"></a>Az első nginx tároló terjesztése
Amikor a telepítő befejeződik, SSH az új Docker állomáshoz a helyi számítógépről. Írja be a saját felhasználónevét és a teljesen minősített tartománynév

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Miután bejelentkezett a Docker host, nézzük futtatása egy nginx tároló:

```
sudo docker run -d -p 80:80 nginx
```

A kimenet hasonlít az alábbi példa a nginx kép letöltése és tároló lépések:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

A következőképpen a Docker állomáson futó tárolók állapotának ellenőrzése:

```
sudo docker ps
```

A kimenet hasonlít az alábbi példában, mely bemutatja, hogy a nginx tároló fut, és a 80 és 443-as TCP-portokat és a továbbított:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

A művelet tároló megtekintéséhez nyissa meg a böngészőben, és adja meg a Docker szolgáltató FQDN nevét:

![A futó ngnix tároló](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure virtuális Docker bővítmény sablon hivatkozása
Az előző példában meglévő quickstart útmutató sablon. A saját erőforrás-kezelő sablonok az Azure Docker virtuális kiterjesztést is telepítheti. Kéri, adja hozzá a következő az erőforrás-kezelő sablonok, meghatározása a `vmName` a virtuális gép megfelelően:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Erőforrás-kezelő sablonok használatáról az [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)olvasásával részletes útmutató található.


## <a name="next-steps"></a>Következő lépések
Merülnek fel, [állítsa be a Docker démon portot](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), [Docker biztonsági](https://docs.docker.com/engine/security/security/), amelyeket ismer, vagy üzembe tárolók [Docker írása](https://docs.docker.com/compose/overview/)használatával. Az Azure Docker virtuális bővítmény, magát a további tudnivalókért lásd: a [GitHub project](https://github.com/Azure/azure-docker-extension/).

Olvassa el a további információt a Docker további telepítési beállítások Azure-ban:

- [Az Azure illesztőprogram Docker gépi használata](./virtual-machines-linux-docker-machine.md)  
- [Első lépések a Docker és írása meghatározása és Azure virtuális-gépen a több elem tároló alkalmazásnak a futtatására](virtual-machines-linux-docker-compose-quickstart.md).
- [Az Azure-tároló szolgáltatás fürt terjesztése](../container-service/container-service-deployment.md)
