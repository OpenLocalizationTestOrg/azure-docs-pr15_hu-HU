<properties
   pageTitle="Docker és írása virtuális gépre |} Microsoft Azure"
   description="Azure-ban Linux virtuális gépeken írása és Docker használata rövid ismertetése"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Első lépések a Docker és írása meghatározása és egy több tároló alkalmazás futtatásához Azure virtuális-gépen

Használatba Docker és [írása](http://github.com/docker/compose) meghatározása és Azure-ban Linux virtuális gépen egy összetett alkalmazás futtatásához. Írása akkor egy egyszerű szövegfájl definiálja az alkalmazást, több Docker tárolók álló használata. Majd a léptetőnyíl fel az alkalmazást, amely tartalmaz minden, a megadott környezetben üzembe egyetlen paranccsal. 

Példaként Ez a cikk bemutatja, hogyan gyorsan állíthat be egy-egy Ubuntu virtuális MariaDB SQL-adatbázis fájlok WordPress blogbejegyzésbe. Írása segítségével állíthatja be az összetettebb alkalmazásokat.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Lépés: 1: Egy Linux virtuális Docker fogadó beállítása

Segítségével különböző Azure eljárások és az elérhető képek vagy az erőforrás-kezelő sablonok a Microsoft Azure piactéren található hozzon létre egy Linux virtuális és Docker fogadó beállítása. [A Docker virtuális bővítmény telepítése a környezet használatával](virtual-machines-linux-dockerextension.md) gyorsan létrehozhat egy Ubuntu virtuális az Azure Docker virtuális kiterjesztéssel [quickstart útmutató sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)például látható. 

A virtuális Docker bővítmény használata esetén a virtuális automatikusan be van állítva egy Docker állomással, és írása már telepítve van. Ebben a cikkben a példa bemutatja, hogyan [for Mac, Linux, és a Windows Azure parancssori felület](../xplat-cli-install.md) (Azure CLI) létrehozása a virtuális erőforrás-kezelő módban.

Az egyszerű parancs az előző dokumentumból hoz létre nevű erőforráscsoport `myResourceGroup` a a `West US` helyét és üzembe helyezése a virtuális az Azure Docker virtuális kiterjesztéssel telepítve:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Lépés: 2: Győződjön meg arról, hogy telepítve van-e a írása

A telepítésének befejeződése után az új Docker állomáshoz a DNS-sel SSH adjon nevet az Ön által megadott a telepítés során. Használható `azure vm show -g myDockerResourceGroup -n myDockerVM` megtekintése a virtuális, beleértve a DNS-név adatait.

Annak ellenőrzéséhez, hogy írása telepítve van a virtuális, futtassa a következő parancsot:

```bash
docker-compose --version
```

A kimenet hasonló módon jelenik meg `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Ha a hozzon létre egy Docker host és írása saját kezűleg kell telepítenie egy másik módszert használt, olvassa el a [írása dokumentációt](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Lépés 3: Docker-compose.yml konfigurációs fájl létrehozása

Ezután hoz létre egy `docker-compose.yml` fájlt, amely csak egy szöveges konfigurációs fájl, a Docker tárolók a virtuális futtatni szeretne meghatározni. A fájlt, adja meg a képet, futtassa az egyes tárolóra (vagy egy Dockerfile a építés lehet), szükséges környezeti változók és a függőségek, a portokat és a tárolók közötti kapcsolatokat. A yml fájl szintaxis részletekért olvassa el [írása fájlhivatkozás](http://docs.docker.com/compose/yml/).

Hozzon létre a `docker-compose.yml` fájl az alábbi képlettel történik:

```bash
touch docker-compose.yml
```

A kedvenc szövegszerkesztővel bizonyos adatok hozzáadása a fájlhoz. Az alábbi példában a `vi` szerkesztő:

```bash
vi docker-compose.yml
```

Illessze be az alábbi példa a szövegfájlt. Ebben a konfigurációban használja a képek a [Beállításjegyzék DockerHub](https://registry.hub.docker.com/_/wordpress/) a WordPress (a Megnyitás blogírás és a tartalom kezelése rendszer) és a csatolt kódmentes MariaDB SQL-adatbázishoz való telepítéséhez. Írja be a saját `MYSQL_ROOT_PASSWORD` az alábbi képlettel történik:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Lépés: 4: A tárolók Kiindulás írása

Ugyanabban a mappában a `docker-compose.yml` fájlt, a következő parancs futtatása (a környezettől függően előfordulhat, hogy futtatásához szükséges `docker-compose` használatával `sudo`.):

```bash
docker-compose up -d

```

Ez a parancs elindítja a megadott Docker tárolók `docker-compose.yml`. Néhány percig is az ebben a lépésben befejezéséhez szükséges időt. Ez a példa hasonló kimenet jelenik meg:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Feltétlenül a **kétdimenziós** indítási beállítást, hogy a tárolók folyamatosan futtatása a háttérben.

Győződjön meg róla, hogy a tárolók be, írja be a következőt `docker-compose ps`. Meg kell jelennie hasonló:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Közvetlenül a 80-as port a virtuális WordPress is csatlakozhat. Nyisson meg egy webböngészőt, és adja meg a virtuális DNS nevét (például: `http://myresourcegroup.westus.cloudapp.azure.com`). Ekkor megjelenik a WordPress kezdőképernyőn, ahol végezhetik el a telepítést, és első lépések az alkalmazást.

![WordPress kezdőképernyőre][wordpress_start]


## <a name="next-steps"></a>Következő lépések

* Nyissa meg a további beállítások konfigurálása a Docker virtuális Docker és írása a [Docker virtuális bővítmény útmutatója](https://github.com/Azure/azure-docker-extension/blob/master/README.md) . Például egy, hogy a írása yml fájlt (JSON konvertálva) közvetlenül a konfigurációban a Docker virtuális bővítmény.
* Nézze meg a [írása parancssori hivatkozás](http://docs.docker.com/compose/reference/) és a [felhasználói útmutatóban](http://docs.docker.com/compose/) összeállítását, és több tároló alkalmazások telepítése további példákat.
* Az erőforrás-kezelő Azure sablont, vagy használja a saját vagy egy járult a [közösségi](https://azure.microsoft.com/documentation/templates/)az Azure virtuális Docker és állítva írása az alkalmazás telepítéséhez. Például a [Deploy WordPress blog Docker a](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) sablont használja Docker és írása gyors üzembe WordPress egy olyan Ubuntu virtuális a MySQL-fájlok.
* Próbálja meg a [Docker Swarm](virtual-machines-linux-docker-swarm.md) fürtre Docker írása integrálása. Lásd: [A méhrajt használatával írhat](https://docs.docker.com/compose/swarm/) esetek.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
