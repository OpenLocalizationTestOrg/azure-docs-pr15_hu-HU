<properties
   pageTitle="Hozzon létre Docker hosts Docker gépi az Azure-ban |} Microsoft Azure"
   description="Docker gépi docker hosts létrehozása az Azure használatát ismerteti."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Hozzon létre Docker Hosts Docker-gép az Azure-ban

A szolgáltató fut a docker démon virtuális [Docker](https://www.docker.com/) tárolók futó szükséges.
Ez a témakör ismerteti, hogyan lehet a [docker-gép](https://docs.docker.com/machine/) paranccsal hozhat létre új Linux VMs, a Docker démon az Azure-ban futó konfigurált. 

**Megjegyzés:** 
- *Ez a cikk docker-gép verzió 0.7.0 függ-s vagy újabb*
- *A Windows tárolók keresztül támogatják docker-gép a közeljövőben*

## <a name="create-vms-with-docker-machine"></a>Hozzon létre VMs Docker gépi

A host VMs docker létrehozása az Azure-ban a `docker-machine create` parancs használata a `azure` illesztőprogramját. 

Az Azure illesztőprogram szükségük van az előfizetés azonosítójával. Az [Azure CLI](xplat-cli-install.md) vagy az [Azure Portal](https://portal.azure.com) segítségével lekérése az Azure-előfizetés. 

**Az Azure portál használatával**
- Válassza ki az előfizetések a bal oldali ablaktáblában, és másolja a vágólapra a előfizetés azonosítója.

**Az Azure CLI használatával**
- Típus ```azure account list``` és másolása az előfizetés azonosítója.

Típus `docker-machine create --driver azure` beállítások és alapértelmezett értékeik megtekintéséhez.
A [Docker Azure-illesztőprogram dokumentációjában találhat](https://docs.docker.com/machine/drivers/azure/) további információt a is láthatja. 

Az alábbi példa az alapértelmezett értékeket támaszkodó, de a másik lehetőségként nyissa meg az internet-hozzáférés a virtuális 80-as portjához. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Válassza ki a docker-gép docker állomás
Ha a szolgáltató már egy bejegyzést a docker-gép, beállíthatja, hogy az alapértelmezett állomás docker parancsok futtatásakor.
##<a name="using-powershell"></a>A PowerShell használatával

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Bash használata

```bash
eval $(docker-machine env MyDockerHost)
```

Most már futtatva docker parancsok a megadott állomás ellen

```
docker ps
docker info
```

## <a name="run-a-container"></a>A tároló futtatása

Konfigurált állomással most futtatását is lehetővé teszi egyszerű webkiszolgálóra, és ellenőrizze, hogy a szolgáltató megfelelően történt-e beállítva.
Itt azt egy szabványos nginx képet, adja meg, hogy kell-e meghallgatása a 80-as port és, hogy a fogadó virtuális újraindul, ha a tároló fog indítani, valamint (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

A kimenet hasonlóan kell kinéznie a következőket:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>A tároló tesztelése

Vizsgálja meg a futó tárolók használatával `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

És ellenőrizze, hogy a futó tároló, írja be a `docker-machine ip <VM name>` keresése az IP-címet a böngészőben:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![A futó ngnix tároló](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Összefoglalás
A docker-gép egyszerűen a az egyes docker host ellenőrzések is kiépítése docker hosts Azure-ban.
Gyártási a tárolók tárolására, lásd: az [Azure tároló szolgáltatás](http://aka.ms/AzureContainerService)

Visual Studio .NET Core alkalmazásokat fejleszt, lásd: [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)
