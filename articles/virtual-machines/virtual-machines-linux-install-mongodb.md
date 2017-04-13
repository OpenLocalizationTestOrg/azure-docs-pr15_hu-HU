<properties
   pageTitle="Egy Linux virtuális MongoDB telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként telepítheti, állíthatja MongoDB Linux virtuális gépen használja, az erőforrás-kezelő telepítési modell Azure-ban."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Telepítse és állítsa be a MongoDB a egy Linux virtuális Azure-ban
[MongoDB](http://www.mongodb.org) népszerű Megnyitás-forrást, a nagy teljesítményű NoSQL adatbázis. Ez a cikk bemutatja, hogyan telepítheti, állíthatja MongoDB egy Linux virtuális Azure-ban kattintson az erőforrás-kezelő telepítési modellt használja. Példák, amelyek részletesen hogyan szeretné:

- [Manuális telepítése és beállítása egy egyszerű MongoDB példány](#manually-install-and-configure-mongodb-on-a-vm)
- [Hozzon létre egy egyszerű MongoDB-példányt, erőforrás-kezelő sablon használatával](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Hozzon létre egy összetett MongoDB replikával sharded fürt állítja be az erőforrás-kezelő sablon használatával](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk az alábbiakra van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).
- bejelentkezett az [Azure CLI](../xplat-cli-install.md)`azure login`
- az Azure CLI *kell lennie* az erőforrás-kezelő Azure üzemmód használata`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Telepítse kézzel és egy virtuális MongoDB konfigurálása
MongoDB a [telepítési útmutatást](https://docs.mongodb.com/manual/administration/install-on-linux/) Linux distros, beleértve a piros kalap / CentOS, SUSE, Ubuntu és Debian. Az alábbi példa létrehoz egy `CoreOS` SSH kulccsal-on tárolt virtuális `.ssh/azure_id_rsa.pub`. Válaszoljon a képernyőn megjelenő utasításokat a tároló fióknév, a DNS-név és a rendszergazdai hitelesítő adataival:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Jelentkezzen be a virtuális használata a nyilvános IP-cím, az előző lépést a virtuális létrehozási végén jelenik meg:

```bash
ssh ops@40.78.23.145
```

A telepítési forrásokban MongoDB hozzáadásához hozzon létre egy `yum` tárházba fájlt az alábbiak szerint:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Nyissa meg szerkesztésre a MongoDB repó fájlt. Adja hozzá a következő sort:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Telepítse a MongoDB használatával `yum` az alábbi képlettel történik:

```bash
sudo yum install -y mongodb-org
```

Alapértelmezés szerint SELinux lép érvénybe, hogy megakadályozza, hogy MongoDB elérése CentOS képek. Csoportházirend-kezelő eszközök telepítése és engedélyezése az alapértelmezett TCP port 27017 a következőképpen működtetéséhez MongoDB SELinux konfigurálása. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Indítsa el a MongoDB szolgáltatást a következőképpen:

```bash
sudo service mongod start
```

Ellenőrizze a MongoDB telepítés használata a helyi útján `mongo` ügyfél:

```bash
mongo
```

Most már tesztelje a MongoDB példány újabb adatokat, és kattintson a keresés:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Ha azt szeretné, állítsa be a MongoDB automatikus indítása a rendszer újraindítása:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Egyszerű MongoDB példány létrehozása sablon használatával CentOS
Egy egyetlen CentOS virtuális Github a következő Azure quickstart útmutató-sablon segítségével létrehozhat egy egyszerű MongoDB példány. Ezzel a sablonnal kiterjesztése az egyéni parancsfájl-Linux hozzáadása egy `yum` az újonnan létrehozott CentOS virtuális és telepítse MongoDB tárházba.

- [Egyszerű MongoDB példányára CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Az alábbi példa létrehoz egy erőforráscsoport nevű `myResourceGroup` a a `WestUS` régió. Adja meg a saját értékek a következők szerint:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Az Azure CLI térhet vissza egy kérdés a telepítési létrehozásának néhány másodpercet belül, de a telepítésének és konfigurálásának néhány percig tart. A környezet, amelyben állapotának ellenőrzése `azure group deployment show myResourceGroup`, a csoport nevét, az erőforrás megfelelően beírásával. Várja meg, amíg a `ProvisioningState` jeleníti meg a "Sikerült" mielőtt megpróbálja a virtuális gép SSH.

A telepítés befejezése után, SSH a virtuális gép. Szerezze be a használatáról virtuális IP-címét a `azure vm show` parancs a következő példának megfelelően:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

A kimenet, vége közelében a `Public IP address` jelenik meg. A virtuális a virtuális IP-címét a gép SSH:

```bash
ssh ops@138.91.149.74
```

Ellenőrizze a MongoDB telepítés használata a helyi útján `mongo` ügyfél az alábbiak szerint:

```bash
mongo
```

Most már tesztelje a példány bizonyos adatok hozzáadásával, és a Keresés az alábbi képlettel történik:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Összetett MongoDB Sharded fürt létrehozása a sablon használatával CentOS
Létrehozhat egy összetett sharded MongoDB fürt Github a következő Azure quickstart útmutató-sablon segítségével. Ez a sablon a [MongoDB sharded fürt gyakorlati tanácsok](https://docs.mongodb.com/manual/core/sharded-cluster-components/) a redundancia és magas elérhetősége megadására követi. A sablon két shards, minden kópiakészletben három csomópontok hoz létre. Egy config server replikakészlethez három csomópontokat is létrehozott, két plusz `mongos` útválasztó kiszolgálók az alkalmazások konzisztencia révén a shards.

- [MongoDB Sharding fürt CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] A komplex MongoDB sharded fürt telepítése szükséges több, mint 20 egyes vagyis általában az alapértelmezett core száma egy előfizetési régió. Nyissa meg a növelheti a core darab Azure támogatási kérelmet.

Az alábbi példa létrehoz egy erőforráscsoport nevű `myResourceGroup` a a `WestUS` régió. Adja meg a saját értékek a következők szerint:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Az Azure CLI térhet vissza egy kérdés a telepítési létrehozásának néhány másodpercet belül, de a telepítésének és konfigurálásának átveheti óránként befejezéséhez. A környezet, amelyben állapotának ellenőrzése `azure group deployment show myResourceGroup`, a csoport nevét, az erőforrás megfelelően kiigazítására. Várja meg, amíg a `ProvisioningState` jeleníti meg a "Sikerült" a VMs való csatlakozás előtt.


## <a name="next-steps"></a>Következő lépések
Ezek a példák kapcsolódni MongoDB példány helyileg a virtuális a. Ha más virtuális vagy hálózati MongoDB példány csatlakozni szeretne, győződjön meg róla, a megfelelő [hálózati biztonsági csoport szabályok hozhatók létre](virtual-machines-linux-nsg-quickstart.md).

Sablonok használata létrehozásával kapcsolatos további tudnivalókért lásd: az [erőforrás-kezelő Azure áttekintése](../azure-resource-manager/resource-group-overview.md).

Az erőforrás-kezelő Azure-sablonok letöltése, és hajtsa végre a VMs parancsfájlokat az egyéni parancsfájl bővítmény használatával. További tudnivalókért lásd: [az Azure egyéni parancsfájl kiterjesztés Linux virtuális gépeken futó használatával](virtual-machines-linux-extensions-customscript.md).