<properties
    pageTitle="Docker virtuális bővítmény használatának Linux |} Microsoft Azure"
    description="Docker és az Azure virtuális gépeken futó bővítmények, és hogy miként hozhat létre, amelyek használatával az Azure CLI klasszikus telepítési modell docker hosts Azure virtuális gépeken futó ismerteti."
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
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Az Azure klasszikus portált a Docker virtuális bővítmény használata

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) egyike, a leggyakrabban használt virtualizációs megközelítés, amely a virtuális gépeken futó, hanem [Linux tárolók](http://en.wikipedia.org/wiki/LXC) használja, mint a megosztott erőforrásokat a számítások és az adatok elkülönítése lehetőséget. Egy tetszőleges számú tárolók az Azure-alkalmazásokkal üzemeltető Docker virtuális létrehozása az [Azure Linux ügynök] által felügyelt Docker virtuális kiterjesztést is használhatja.

> [AZURE.NOTE] Ez a témakör ismerteti, hogyan hozhat létre egy Docker virtuális az Azure klasszikus portálról. Megtudhatja, hogy miként hozhat létre egy Docker virtuális a parancssorból, olvassa el a [a Docker a az Azure parancssori kezelőfelületről Azure virtuális bővítmény használatáról]című témakört. Tárolók és azok előnyeiről magas szintű vitafórum-témakörökben talál további [Docker magas szintű rajztábla](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Hozzon létre egy új virtuális a kép gyűjteményből
Az első lépés az Azure virtuális, amely támogatja a Docker virtuális bővítmény használatával Ubuntu 14.04 LTS képet a kép gyűjteményből Ubuntu 14.04 asztali, valamint egy példát kiszolgálóra képe egy ügyfél Linux képből szükséges. A portálon kattintson a **+ Új** hozzon létre egy új virtuális példányát, és jelöljön ki egy Ubuntu 14.04 LTS képet, a választható vagy a teljes kép gyűjteményből bal alsó sarokban alább látható módon.

> [AZURE.NOTE] Jelenleg csak Ubuntu 14.04 LTS képek újabb július 2014-es, mint támogatja a Docker virtuális bővítmény.

![Hozzon létre egy új Ubuntu képe](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker tanúsítványok létrehozása

A virtuális létrehozása után győződjön meg arról, hogy Docker az ügyfélszámítógépen telepítve van. (Részletekért lásd: [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation).)

A tanúsítvány és a billentyűk fájlokat Docker kommunikáció megfelelően [Fut Docker a https] létrehozása, és helyezze el őket a a **`~/.docker`** könyvtárban az ügyfélgépen beállított.

> [AZURE.NOTE] A Docker virtuális a portálon jelenleg meghosszabbításához hitelesítő adatait, amelyek kódolt base64.

A parancssorból használata **`base64`** eszközzel vagy más kedvenc kódolási base64 kódolású témakörök létrehozásához. Ezzel az egyszerű tanúsítvány és a billentyűk fájlokat tartalmazó néz ki:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>A Docker virtuális bővítmény hozzáadása
A Docker virtuális bővítmény hozzáadása, keresse meg a virtuális példány hozott létre, és görgessen le a **bővítményeket** , és rákattintva jelenítse meg a virtuális bővítmények, alább látható módon.
> [AZURE.NOTE] Ez a funkció csak az előnézeti portálon támogatott: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Egy bővítmény hozzáadása
Kattintson a **+ Hozzáadás** a lehetséges virtuális bővítmények vehet fel a virtuális megjelenítéséhez.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Jelölje ki a Docker virtuális bővítmény
Válassza ezt a lehetőséget választva a Docker leírás és a fontos hivatkozásokat, Docker virtuális bővítmény, és kattintson a telepítés megkezdése az alul a **Létrehozás** gombra.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>A tanúsítvány és a fő fájlok hozzáadása:

Írja be a hitelesítésszolgáltató tanúsítványát a kiszolgálói tanúsítvány és a kiszolgáló használatával base64 kódolású verziója az űrlap mezők az alábbi ábrán látható módon.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Figyelje meg, hogy (ahogy a fenti képet) a 2376 ki van töltve alapértelmezés szerint. Bármely végpont Itt adhatja meg, de a következő lépésben kattintva nyissa meg a megfelelő végpont lesz. Ha módosítja az alapértelmezett, feltétlenül kattintva nyissa meg a következő lépésben az egyező végpontot.

## <a name="add-the-docker-communication-endpoint"></a>A Docker kommunikációs végpont hozzáadása
Az erőforráscsoport létrehozott megtekintésekor jelölje ki a virtuális társított hálózati biztonsági csoport, és kattintson a **Bejövő szabályok** a szabályok megtekintésére, mint az ábrán.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Kattintson a **+ Hozzáadás** egy másik szabály hozzáadása gombra, és az alapértelmezett lehetőséget választja, írja be a nevét a végpont (a példában a **Docker**), és 2376 "cél porttartományt". Adja meg a protokoll oszlopbeli értéket megjelenítő **TCP**, és kattintson **az OK gombra** kattintva hozza létre a szabályt.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>A Docker ügyfél és Azure Docker Host tesztelése
Keresse meg és másolja a nevet a virtuális tartománynevet, és a parancssorban az ügyfélszámítógépen, írja be a `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (ahol *dockerextension* helyettesíti be az altartomány a virtuális).

Az eredmény jelenjen meg ehhez hasonló:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

A fenti lépések elvégzése után az Azure virtuális, csatlakoznia kell távolról más ügyfelektől konfigurált futó teljes funkcionalitású Docker állomás most már rendelkezik.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Nyissa meg a [Docker felhasználói útmutatóban] , és használja a Docker virtuális készen áll. Automatikus létrehozása Docker állomása Azure VMs parancssor keresztül, című témakörben olvashat [a Docker a az Azure parancssori kezelőfelületről Azure virtuális bővítmény használatáról]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[A Docker a az Azure parancssori kezelőfelületről Azure virtuális bővítmény használatáról]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Https Docker rendszerrel]: http://docs.docker.com/articles/https/
[Docker útmutatója]: https://docs.docker.com/userguide/
