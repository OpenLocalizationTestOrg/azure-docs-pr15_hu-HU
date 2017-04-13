<properties
   pageTitle="Azure tároló szolgáltatás – bevezetés |} Microsoft Azure"
   description="Azure tároló szolgáltatás megoldást létrehozását, beállítása és kezelése, hogy be vannak állítva indexelése alkalmazások futtatásához virtuális gépeken futó fürt egyszerűsítése érdekében."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure tároló szolgáltatás – bevezetés

Azure tároló szolgáltatás egyszerűbbé teszi a hozhat létre, beállítása és kezelése, hogy be vannak állítva indexelése alkalmazások futtatásához virtuális gépeken futó fürt. Akkor használja a népszerű Megnyitás-forrás ütemezés- és üzletifolyamat-tervező eszközök egy optimalizált konfigurálása. Ez lehetővé teszi a meglévő ismeretek, illetve rajzolása, közösségi szakértelemmel üzembe helyezéséhez és kezeléséhez, a Microsoft Azure tároló-alapú alkalmazások nagy és növekvő törzsében.


![Azure tároló szolgáltatás lehetővé teszi a Azure több állomásokon indexelése alkalmazáskezelési.](./media/acs-intro/acs-cluster.png)


Azure tároló szolgáltatás használja annak érdekében, hogy az alkalmazás tárolók teljesen hordozható a Docker tároló formázása. A választott Marathon és Adatközpont/OS vagy Docker Swarm is, hogy ezeket az alkalmazásokat tárolók ezer, vagy akár tízesre ezres méretezheti támogat.

Azure tároló szolgáltatás segítségével kihasználhatja a vállalati szintű funkciói Azure-alkalmazás hordozhatóságát – többek között az üzletifolyamat-tervező rétegek a hordozhatóságát továbbra is megőrzésével.

<a name="using-azure-container-service"></a>Azure tároló szolgáltatás használatával
-----------------------------

Azure tároló szolgáltatással célunk üzemeltetési környezet Megnyitás-forrás eszközökkel és a ma ügyfeleink körében népszerű technológiák tároló adja meg. Emiatt azt jelenítik meg a szabványos API-végpontok esetében a választott orchestrator (Adatközpont/OS vagy Docker Swarm). Ezeket a végpontokat használatával olyan szoftvert, amely képes a beszélgetésben ezeket a végpontokat is élvezheti. Például az Docker Swarm végpont esetében lehet való használatra kiválasztott a Docker parancssori kezelőfelületről. Adatközpont/OS előfordulhat, hogy válassza a DCOS CLI használja.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Docker fürt létrehozása az Azure tároló szolgáltatás használatával
-------------------------------------------------------

Azure tároló szolgáltatás használatának megkezdéséhez rendszerbe keresztül a portálon ("Azure tároló szolgáltatás" keresése), az Azure tároló szolgáltatás fürtre használatával egy erőforrás-kezelő Azure-sablon ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) vagy [Adatközpont/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) vagy a [CLI](/documentation/articles/xplat-cli-install/). A megadott quickstart útmutató sablonok módosítható, hogy további vagy speciális Azure konfigurációs tartalmazza. Az Azure tároló szolgáltatás fürt telepítése a további tudnivalókért lásd: [az Azure tároló szolgáltatás fürt Deploy](container-service-deployment.md).

<a name="deploying-an-application"></a>Az alkalmazás telepítése
------------------------

Azure tároló szolgáltatása a választott Docker Swarm vagy Adatközpont/OS az üzletifolyamat-tervező. A választott orchestrator függ, hogy hogyan telepíti az alkalmazásokat.

### <a name="using-dcos"></a>Adatközpont/operációs rendszer használata

Adatközpont/OS elosztott operációs rendszerű Apache Mesos elosztott rendszerek kernelen alapuló. Apache Mesos van elhelyezni a Apache szoftver Foundation címen, és egy részét sorolja fel a [legfontosabb nevek informatikai](http://mesos.apache.org/documentation/latest/powered-by-mesos/) felhasználókként és a munkatársak.

![Azure tároló szolgáltatási ügynökök és mesteralakzatok méhrajt be van állítva.](media/acs-intro/dcos.png)

Adatközpont/OS és Apache Mesos tartalmazza az egy hatásos szolgáltatás beállítása:

-   Igazolt méretezhetőség:

-   Hibafa-alternatív replikált fő- és slaves Apache ZooKeeper használatával

-   Tárolók Docker formázott támogatása

-   Natív elkülönítési Linux jegyzettárolót tevékenységek közé

-   Multiresource ütemezési (memóriát, Processzor, a lemez és portok)

-   Java Python és új párhuzamos alkalmazások fejlesztésével C++ API-khoz

-   Egy webhely felhasználói felület megtekintésre fürt állam

Alapértelmezés szerint Adatközpont/OS Azure tároló szolgáltatás futó tartalmaz, a feladatok ütemezése Marathon üzletifolyamat-tervező platform. A ACS Adatközpont/OS központi részét képező azonban nem adhatók hozzá a szolgáltatást, ezek közé tartozik a külső, Hadoop, Cassandra és még sok más szolgáltatások Mesosphere definícióinak.

![Azure tároló szolgáltatásban Adatközpont/OS Univerzumban](media/dcos/universe.png)

#### <a name="using-marathon"></a>Marathon használata

Marathon pedig egy fürtre kiterjedő init vezérlő rendszer szolgáltatások cgroups – vagy az Azure tároló szolgáltatás, a tárolók Docker formázott esetében. Marathon ahonnan telepítheti az alkalmazások webes felhasználói Felületet biztosít. Érheti el ezt a egy URL-címet, a következőhöz hasonló `http://DNS_PREFIX.REGION.cloudapp.azure.com` hol DNS\_ELŐTAG és régió mindkét definiált telepítési időben. Természetesen is megadhat saját DNS-nevét. A Marathon webes felhasználói felületen tároló, bővebben lásd: [tároló kezelése a webes felhasználói felület keresztül](container-service-mesos-marathon-ui.md).

![Marathon alkalmazások lista](media/dcos/marathon-applications-list.png)

A REST API-k Marathon kommunikációhoz is használhatja. Számos elérhető az egyes eszközök ügyfél-tárakban. Számos nyelven – terjed ki, és természetesen használható a HTTP protokoll bármely nyelven. Ezenkívül számos népszerű DevOps eszközök Marathon nyújt támogatást. A maximális rugalmasságot biztosít a műveletek csoport egy Azure tároló szolgáltatás fürthöz használatakor. A Marathon REST API segítségével tároló futnak a további tudnivalókért lásd: [a REST API-val tároló kezelése](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Docker méhrajt használatával

Docker méhrajt itt Docker natív fürtözőszoftverét. A szokásos Docker API Docker Swarm szolgál, mert bármilyen eszközzel, amely már kommunikál Docker démon segítségével méhrajt Azure tároló szolgáltatása több állomások átlátszó méretezni.

![Azure tároló szolgáltatás Adatközpont/OS – jumpbox, ügynökök és mesteralakzatok használja.](media/acs-intro/acs-swarm2.png)

Tárolók méhrajt fürtre kezelésére szolgáló támogatott eszközök tartalmazzák, de nem korlátozódik, a következőket:

-   Dokku

-   Docker CLI és Docker írása

-   Krane

-   Jenkins

<a name="videos"></a>Videók
------

Első lépések az Azure tároló szolgáltatás (101):  

> [AZURE.VIDEO azure-container-service-101]

Építőelem-alkalmazásokat az Azure tároló szolgáltatással (Build 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
