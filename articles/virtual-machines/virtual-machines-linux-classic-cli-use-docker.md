<properties
    pageTitle="A Azure Linux a Docker virtuális bővítmény használata"
    description="Docker és az Azure virtuális gépeken futó bővítmények mutatja be, és bemutatja, hogyan programozás útján létrehozása, amelyek használatával az Azure CLI a parancssorból docker hosts Azure virtuális gépeken futó."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Az Azure parancssori Docker virtuális kiterjesztésre használatával felület (Azure CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Ez a témakör ismerteti, hogy miként hozhat létre egy virtuális szolgáltatás felügyeleti (asm) üzemmódról az Azure CLI bármely platformon Docker virtuális kiterjesztésű. [Docker](https://www.docker.com/) egyike, a leggyakrabban használt virtualizációs megközelítés, amely a virtuális gépeken futó, hanem [Linux tárolók](http://en.wikipedia.org/wiki/LXC) használja, mint a megosztott erőforrásokat a számítások és az adatok elkülönítése lehetőséget. A virtuális Docker bővítmény és az [Azure Linux ügynök](virtual-machines-linux-agent-user-guide.md) hozhat létre egy tetszőleges számú tárolók az Azure-alkalmazásokkal üzemeltető Docker virtuális. Tárolók és azok előnyeiről magas szintű vitafórum-témakörökben talál további [Docker magas szintű rajztábla](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Azure virtuális Docker kiterjesztése használata
Azure virtuális Docker kiterjesztése használatához telepítenie kell az [Azure parancssori felület](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) nagyobb, mint 0.8.6-verziót (mint írásakor jelenlegi verziójával 0.10.0). Az Azure CLI telepíthető Windows, Mac és Linux.


A teljes folyamat Azure Docker használatáról a következő egyszerű:

+ Telepítse az Azure CLI és függőségét azon a számítógépen, amelyből szabályozni szeretné Azure (Windows, ez lesz egy virtuális gépen futó Linux terjesztési)
+ Az Azure CLI Docker parancsaival hozhat létre virtuális Docker állomás Azure-ban
+ A helyi Docker parancsaival a Docker tárolók az Azure-ban a Docker virtuális kezelése.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Telepítse az Azure parancssori kezelőfelületről Azure

Telepítse és állítsa be az Azure CLI, megtudhatja, [hogy miként telepítheti az Azure parancssort](../xplat-cli-install.md). A telepítés, írja be a `azure` a parancssorablakban, és meg kell jelennie a Azure CLI ASCII-elem rövid némi után, amely felsorolja azokat a legalapvetőbb gombokkal elérhető. A telepítés dolgozott helyesen, ha látnia kell beírásához `azure help vm` , és megtudhatja, hogy a listában szereplő parancsok egyikét "docker".

> [AZURE.NOTE] Docker vannak eszközök Windowshoz [Docker számítógépen](https://docs.docker.com/installation/windows/), amelyen automatizálásához használható készült Azure VMs mint docker hosts docker ügyfél kibocsátása is használhatja.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Az Azure CLI szeretne csatlakozni az Azure-fiók
Az Azure CLI használata előtt össze kell kapcsolnia a Azure-fiók hitelesítő adatait az Azure CLI a platformon. A szakasz [az Azure előfizetés keresztüli](../xplat-cli-connect.md) megtudhatja, hogyan letöltése és a **.publishsettings** -fájl importálása, vagy az Azure CLI társítása egy szervezeti fiókkal.

> [AZURE.NOTE] Nincsenek viselkedés eltérések a hitelesítési, olvassa el a fenti megérteni a különböző funkciókat dokumentum más módszert használja.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Docker telepítése és használata a Docker virtuális bővítmény Azure
Kövesse az [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation) Docker helyileg telepítése a számítógépen.

A virtuális használt Linux kép Docker az Azure virtuális gépen való használatához rendelkeznie kell a [Azure Linux virtuális ügynök](virtual-machines-linux-agent-user-guide.md) . Jelenleg képek, amelyekkel ez csak két típusa van:

+ Az Azure képgyűjtemény Ubuntu képet vagy

+ Egyéni Linux képe, amelyet az Azure Linux virtuális ügynök, telepítette és beállította az Ön által létrehozott. Lásd: az [Azure Linux virtuális ügynök](virtual-machines-linux-agent-user-guide.md) hogyan hozhat létre egy egyéni Linux virtuális Azure virtuális ügynök további információt.

### <a name="using-the-azure-image-gallery"></a>Az Azure képgyűjtemény használatával

Bash vagy munkamenet a következő Azure CLI parancs segítségével keresse meg a legutóbbi Ubuntu képet szeretne használni, írja be a virtuális gyűjtemény

`azure vm image list | grep Ubuntu-14_04`

és válasszon a kép nevét, például: `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, és hozzon létre egy új virtuális használja azt a képet a következő parancs segítségével.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Ha:

+ * &lt;virtuális-cloudservice neve&gt; * a szerkeszthetővé vált a Docker tároló gazdaszámítógép Azure virtuális neve

+  * &lt;felhasználónév&gt; * a felhasználónév a virtuális alapértelmezett legfelső szintű felhasználójának

+ * &lt;jelszó&gt; * az Azure szabványoknak összetettsége *felhasználónév* fiók jelszava

> [AZURE.NOTE] Arról, hogy a jelszó jelenleg kell lennie legalább 8 karakter, tartalmaz egy kisbetű és a egy nagybetű karakter, a szám és a speciális karakter például a következő karakterek közül: `!@#$%^&+=`. Nem, az időszak végén az előző mondatban található még nem speciális karakter.

Ha a parancsot sikerült, meg kell jelennie a következő, attól függően, hogy a pontos argumentumokat és beállítások a használt hasonló:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] A virtuális gépek létrehozására néhány percig is eltarthat, de után van kiépítve (az állapot érték `ReadyRole`) a Docker démon (Docker szolgáltatás) kezdődik, és a Docker tároló host csatlakozhat.

Írja be a Docker virtuális Azure-ban létrehozott teszteléséhez

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

Ha * &lt;virtuális neve-akkor-használt&gt; * a virtuális gép, akkor a hívás átirányítása az használt neve `azure vm docker create`. Meg kell jelennie a következő, amely azt jelzi, hogy a Docker Host virtuális be és Azure-ban futó és parancsok Várakozás hasonló. 

Most, érdemes csatlakozhat a docker ügyfél információkhoz juthat (az egyes Docker ügyfél, a Mac rendszeren, például beállítását használja is `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Minden egyes, hogy az összes munkaidő, hogy vizsgálja meg a virtuális Docker bővítményhez:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker Host virtuális Authentication

A Docker virtuális létrehozásán a `azure vm docker create` parancs is automatikusan létrehoz lehetővé teszi a Docker ügyfélszámítógép az Azure tároló állomás HTTPS használatával csatlakozhat a szükséges tanúsítványok, és a tanúsítványok vannak tárolva, szükség szerint az ügyfél és a host gépeken. A többször a meglévő tanúsítványok újrafelhasználható és az új állomás megosztva.

Alapértelmezés szerint a tanúsítványok helyezett `~/.docker`, és Docker fog állíthatók be a port **2376**futtatásához. Ha szeretné, hogy egy másik porthoz vagy könyvtár, akkor használhatja a következők valamelyikét `azure vm docker create` parancssori kapcsolók a Docker tároló konfigurálása szolgáltató használja egy másik port vagy a különböző tanúsítványok csatlakoztatás ügyfelek virtuális:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

A host Docker démont úgy van konfigurálva, hogy figyelje, és a hitelesítési ügyfélkapcsolatok az adott port által létrehozott tanúsítványokkal a `azure vm docker create` parancsot. Az ügyfélgép a Docker host eléréséhez szükséges tanúsítványok kell rendelkeznie.

> [AZURE.NOTE] Egy hálózatra kötött host futtatása nélkül tanúsítványok bárki lehet kapcsolódni a gép lesz. Csak utána módosítsa az alapértelmezett beállítás, győződjön meg arról, hogy megismeri a kockázatok, a számítógépek és alkalmazásokat.

## <a name="next-steps"></a>Következő lépések

* Nyissa meg a [Docker felhasználói útmutatóban] , és használja a Docker virtuális készen áll. Az új portálon egy Docker engedélyező virtuális létrehozásához tájékozódhat [a Docker virtuális kiterjesztést a portálon használja].

* Az Azure Docker virtuális kiterjesztést is támogat Docker írása, használó deklaráció YAML fájl bármely környezetben mindenütt életbe a egy Fejlesztőeszközök modellezni alkalmazást, és egy egységes telepítési létrehozni. Lásd: [Ismerkedés a Docker és írása meghatározása és Azure virtuális-gépen a több elem tároló alkalmazásnak a futtatására].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[A Docker virtuális bővítmény használatáról az portálján]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker útmutatója]: https://docs.docker.com/userguide/
 
[Első lépések a Docker és írása meghatározása és egy több tároló alkalmazás futtatásához Azure virtuális-gépen]:virtual-machines-linux-docker-compose-quickstart.md