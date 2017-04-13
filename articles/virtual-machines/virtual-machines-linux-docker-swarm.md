<properties
   pageTitle="Első lépések a Azure méhrajt docker használata"
   description="Megtudhatja, hogy miként VMs beállításcsoport a Docker virtuális kiterjesztésű tűntek méhrajt Docker fürt létrehozásához."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Méhrajt docker használata

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ez a témakör bemutatja használata [docker](https://www.docker.com/) [swarm](https://github.com/docker/swarm) méhrajt felügyelt fürt létrehozása az Azure a rendkívül egyszerű lehetőséget. Négy virtuális gépeken futó az Azure-egy a használni kívánt a méhrajt manager és a docker hosts gombcsoport részeként három hoz létre. Amikor elkészült, a fürt megjelenítéséhez, és kezdje el használni docker rajta méhrajt is használhatja. Ezeken kívül az Azure CLI hívások ebben a témakörben használja a a szolgáltatás felügyeleti (asm) módot. 

> [AZURE.NOTE] Ez a témakör docker méhrajt és az Azure CLI *nélkül* **docker-gép** annak érdekében, hogy hogyan működik a különböző eszközök együtt megjelenítése, de továbbra is a független használ. **docker** számítógépben **– swarm** kapcsolók, amelyek lehetővé teszik, hogy közvetlenül a csomópontok hozzáadása egy méhrajt **docker-gép** használatával. Példa a [docker-gép](https://github.com/docker/machine) dokumentációjában olvasható. Ha Ön kihagyott **docker-gépen** futó Azure VMs, megtudhatja, [hogy miként Azure docker készülék használja](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Docker hosts Azure virtuális gépeken futó létrehozása

Ez a témakör a négy VMs hoz létre, de minden számot is használhatja. Hívja fel az alábbi műveleteket * &lt;jelszó&gt; * váltja fel a kiválasztott jelszót.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Amikor elkészült kell **azure virtuális lista** használata a Azure VMs megtekintéséhez:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>A méhrajt fő virtuális méhrajt telepítése

Ez a témakör használja, a [tároló modell a docker-telepítés swarm dokumentáció](https://github.com/docker/swarm#1---docker-image) –, de Ön is is SSH **méhrajt – minta**. Ebben a modellben **swarm** letöltődik méhrajt futó docker elhelyezése. Az alábbi azt végrehajtása a lépés *távolról a saját laptopján docker használatával* csatlakozhat a virtuális **méhrajt-fő** , és megállapítani, hogy a paranccsal fürt azonosító létrehozása, **méhrajt létrehozása** A fürt azonosítóra, hogy hogyan a méhrajt csoport tagjai **swarm** találja. (Is a tárházba klónozhatja, és azt készítése saját magát, amely ad teljes hozzáférés és hibakeresés engedélyezése.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Utolsó sorban az a fürt azonosító; másolja a vágólapra valahol mivel később is használni szeretné, ha a csomópont VMs be a méhrajt mesteroldalra hozhat létre a "méhrajt". Ebben a példában a fürt azonosító **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Világosság, hogy azt használják a helyi docker telepítése kapcsolódni a **méhrajt-fő** virtuális Azure és utasítás **méhrajt-fő** letöltése, telepítése, és futtassa az **létrehozása** parancs, amely a fürt azonosítója, amely használjuk feltárás céljából később.
<!-- -->
> Ennek ellenőrzéséhez futtatása `docker -H tcp://` * &lt;hostname (állomásnév)&gt; * ` images` (azért, mert az előző méhrajt parancs merültek a **--erőforrás-kezelő** kapcsolóval, a tároló el lett távolítva, Miután befejeződött volna, alkalmazással **docker ps – egy** nem visszatérési semmit) a tároló folyamatok **méhrajt-fő** gépre és egy másik csomóponton összehasonlításra céllista.:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Ha ismeri a **docker**, tudni fogja, hogy a többi csomópont van-e a bejegyzés sem, mert nincsenek képek letöltött és futtatása még.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>A csomópont VMs csatlakoztatása a saját docker fürthöz

Az egyes csomópontra sorolja fel az végpont adatait az Azure CLI használatával. Az alábbi azt ehhez az **méhrajt-csomópont-1** docker állomáshoz a csomópont docker port beszerzése érdekében.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


**Docker** használatával és a `-H` lehetőséget a csomópont virtuális, mutasson a docker ügyfél csomópontra csatlakozzon a hoz létre az fürt azonosítóját, illetve a csomópont docker port megkerülhetők méhrajt (az utóbbi használatával **– cím**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Hogy néz ki jó. Kattintva erősítse meg, hogy **swarm** **méhrajt-csomópont-1** , akkor írja be van kapcsolva:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Ismételje meg a fürt minden más csomópontot. Ebben az esetben azt megtennie **méhrajt-csomópont-2** és **méhrajt-csomópont-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Kezdje el, a méhrajt fürt kezelése

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

és ezután jeleníthet meg a csomópontok a fürt:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Nyissa meg a méhrajt dolog, amit futtatni. Ötleteket keres, [https://github.com/docker/swarm/](https://github.com/docker/swarm/)vagy talán [Videó](https://www.youtube.com/watch?v=EC25ARhZ5bI)című témakört.

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
