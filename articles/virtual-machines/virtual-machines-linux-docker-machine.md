<properties
    pageTitle="Hozzon létre Docker hosts Docker gépi az Azure-ban |} Microsoft Azure"
    description="Docker gépi docker hosts létrehozása az Azure használatát ismerteti."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Az Azure illesztőprogram Docker gépi használata

[Docker](https://www.docker.com/) egyike, a leggyakrabban használt virtualizációs megközelítés, amely a virtuális gépeken futó, hanem Linux tárolók használja, mint a megosztott erőforrásokat a számítások és az alkalmazás adatok elkülönítése lehetőséget. Ez a témakör leírja, mikor és hogyan [Docker gép](https://docs.docker.com/machine/) használata (a `docker-machine` parancs) hozhat létre új Linux VMs engedélyezve van a Linux tárolók docker fogadó Azure-ban.


## <a name="create-vms-with-docker-machine"></a>Hozzon létre VMs Docker gépi

Host VMs docker létrehozása az Azure-ban a `docker-machine create` parancs használata a `azure` illesztőprogram argumentum illesztőprogram beállítással (`-d`) és bármely más argumentumokat. 

Az alábbi példa az alapértelmezett értékeket támaszkodó, de azt megnyitni a portjához 80 a virtuális kipróbálni egy nginx tároló, az megkönnyíti az internethez `ops` a bejelentkezett felhasználó SSH és a hívásokat a új virtuális `machine`. 

Típus `docker-machine create --driver azure` tekintheti meg a beállítások és alapértelmezett értékeik; is erről a [Docker Azure-illesztőprogram dokumentációjában találhat](https://docs.docker.com/machine/drivers/azure/). (Tájékoztatjuk, hogy engedélyezve varianciaanalízis hitelesítés esetén kéri a második tényező azonosítania.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

A kimenet hasonlóan kell kinéznie, attól függően, hogy van-e a fiókban beállított kétfaktoros hitelesítés.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>A docker rendszerhéj konfigurálása

Ezután írja be a `docker-machine env <VM name>` kattintva megtekintheti, hogy mit kell tennie a rendszerhéj konfigurálása. 

```bash
docker-machine env machine
```

A környezet adatait, amelyek néz ki nyomtat, amely. Megjegyzés: az IP-cím van rendelve, amely a virtuális tesztelése kell.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Futtathatja az ajánlott konfiguráció parancsot, vagy beállíthatja, hogy a környezeti változók saját magának. 

## <a name="run-a-container"></a>A tároló futtatása

Most már egy egyszerű webkiszolgáló tesztelése futtatását is lehetővé teszi hogy minden megfelelően működik. Itt azt egy szabványos nginx képet, adja meg, hogy kell-e meghallgatása a 80-as port és, ha a virtuális újraindul a tárolót kell indítani, valamint (`--restart=always`). 

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

És ellenőrizze, hogy a futó tároló, írja be a `docker-machine ip <VM name>` keresése az IP-cím (ha az elfelejtette a `env` parancs):

![A futó ngnix tároló](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Következő lépések

Ha érdekli, megpróbálhatja ki az Azure [Docker virtuális bővítmény](virtual-machines-linux-dockerextension.md) az azonos művelet végrehajtása az Azure CLI vagy az Azure erőforrás-kezelő sablon használatával. 

További példák Docker való használatáról című témakörben talál [Docker használata](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) a [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 csatlakozás. További QuickStarts HealthClinic.biz bemutatója a csomagban a [Azure fejlesztői eszközökkel QuickStarts csomagban](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)című témakör tartalmaz.

