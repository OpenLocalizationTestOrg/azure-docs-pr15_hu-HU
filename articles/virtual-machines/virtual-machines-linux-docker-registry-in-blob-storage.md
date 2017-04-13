<properties 
  pageTitle="A saját személyes Docker beállításjegyzékének Azure telepítése |} Microsoft Azure"
  description="Ismerteti, hogyan használhatja a beállításjegyzék Docker tárolni az Azure Blob-tárolóhoz szolgáltatása tároló képeit."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>A saját személyes Docker beállításjegyzékének Azure üzembe helyezése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



A dokumentum mely egy Docker magánjellegű beállításjegyzék, és bemutatja, hogyan telepítheti a Docker rendszerleíró 2.0-s tároló képe egy Docker magánjellegű Azure Blob-tárolóhoz használata a Microsoft Azure beállításjegyzékének ismerteti.

A dokumentum feltételezi, hogy:

1. Megtudhatja, hogy hogyan Docker és Docker képeket tárolja. (Nem? [További tudnivalók: Docker](https://www.docker.com))
2. Ha egy kiszolgáló, amelyen telepítve Docker motor. (Nem? [Gyorsan tegye Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Mi az a magánjellegű Docker beállításjegyzék?

Annak érdekében, hogy a felhőbe indexelése alkalmazás kiszállítása, Docker tároló képként össze, és valahol tárolja azt, hogy saját maga és mások által is használható. 

A tároló kép létrehozásához, és a felhőbe szállítás Gyerekjáték, azt is biztos, hogy tárolja a létrejövő kép bonyolulttá. Éppen ezért Docker kínál [Docker központi] nevű központi szolgáltatás[ docker-hub] tárolásához tároló a képeket a felhő és a tárolók bármikor használatával ezekkel a képekkel létrehozását teszi lehetővé.

Bár a [Docker központi] [ docker-hub] a saját alkalmazások tároló tárolására szolgáló fizetett szolgáltatás képek, Docker veszi fejlesztők igényeinek és a képeket tárolja a saját személyes Docker beállításjegyzékben tűzfal mögött egy megnyitott-forrás toolset biztosít vagy a helyszíni szerezze meg a nyilvános internetkapcsolat nélkül.
Mivel a Azure Blob-tárolóhoz egyszerűen biztonságos, gyorsan felhasználhatja azt hozhat létre és használhat egy privát Docker beállításjegyzék, amely az Ön saját maga Azure-ban.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Miért érdemes Ön üzemelteti egy Docker beállításjegyzékének Azure?

A Microsoft Azure Docker beállításjegyzék példányára szolgáltatója és Azure Blob-tárolóhoz a képeket tárolja, akkor néhány előnye:

**Biztonsági:** A Docker képek ne hagyja Azure adatközpontokkal, így nem felveszik a nyilvános Internet ha használta Docker központi ugyanúgy.
  
**Teljesítmény:** Docker képeit a alkalmazásként ugyanabban a adatközponthoz vagy régióban találhatók. Ez azt jelenti, hogy gyorsabban és megbízhatóbb képest Docker-hubon keresztül csatlakozott a képek fogja húzni.

**Megbízhatóság:** Microsoft Azure Blob-tárolóhoz használatával teheti magas elérhetőségét, redundancia, prémium tároló (SSD), például sok tároló tulajdonságainak használata, és így tovább.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Azure Blob-tárolóhoz Docker beállításjegyzék konfigurálása

(Ajánlott a folytatás előtt [Docker beállításjegyzék 2.0 dokumentáció][beállításjegyzék-dokumentumokat] olvasni.)

[Állítsa be] is[ registry-config] a Docker rendszerleíró kétféle módon.
képes vagy:

1. Használja a `config.yml` fájlt. Ebben az esetben meg kell hozzon létre egy külön Docker kép fölé `registry` képe.
2. Bírálja felül az alapértelmezett konfigurációs fájl környezeti változók keresztül: nélkül létrehozása és fenntartása önálló Docker kép tevékenységeken kap.

Ez a témakör az egyszerűség kedvéért következik, 2, a környezeti változók használata lehetőséget.

Annak érdekében, hogy futtatni Docker beállításjegyzék például amelyek:

* az Azure tárterület-fiókot használnak a képek tárolására szolgáló
* a virtuális gép portot 5000 figyeli
* van konfigurálva hitelesítés nélkül (nem javasolt, lásd a lenti megjegyzést)

a következő parancsot Docker a bash terminálablakba kell (cseréje `<storage-account>` és `<storage-key>` a hitelesítő adatokkal együtt):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Miután kilép a parancsot, a tároló szolgáltatója a magánjellegű Docker beállításjegyzék-példány futtatásával megjelenik a `docker ps` parancs a szolgáltató:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Biztonság beállítása a Docker Registry nem vonatkozik a dokumentumban, és a beállításjegyzék bárki megnyithatja, alapértelmezés szerint hitelesítés nélkül lesz, ha nyissa meg a port a virtuális gép végpont registry portjához vagy terheléselosztó, ha a fenti telepítés parancs.
>
> Olvassa el a [Konfigurálása Docker beállításjegyzék] [ registry-config] dokumentáció megtudhatja, hogy miként biztonságossá a beállításjegyzék-példány és a képeket.

## <a name="next-steps"></a>Következő lépések

Ha befejezte a beállításjegyzék beállítása, pedig lépjen vele néhány további. Indítsa el a docker [beállításjegyzék-dokumentumok]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[beállításjegyzék-dokumentumok]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
