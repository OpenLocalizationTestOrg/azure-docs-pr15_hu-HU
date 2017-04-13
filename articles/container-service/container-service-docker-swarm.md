<properties
   pageTitle="Azure tároló tároló Szolgáltatáskezelés a Docker Swarm |} Microsoft Azure"
   description="Tárolók telepítse az Azure tároló szolgáltatás egy Docker Swarm"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>A Docker Swarm tároló kezelése

Docker méhrajt üzembe helyezése a indexelése munkaterhelésekből teljes Docker hosts készletezett halmazának környezetet biztosít. Docker méhrajt a natív Docker API-t használja. A munkafolyamat egy Docker Swarm a tárolók kezelésére szolgáló megegyezik szinte milyen lenne a egyetlen tároló állomáson. Ezt a dokumentumot egyszerű példákat indexelése munkaterhelésekből Docker Swarm példány Azure tároló szolgáltatás üzembe helyezése. További részletes dokumentációja Docker Swarm [A Docker.com Swarm Docker](https://docs.docker.com/swarm/)című témakör tartalmaz.

A dokumentumban a gyakorlatok előfeltételei:

[Méhrajt fürt létrehozása az Azure tároló szolgáltatás](container-service-deployment.md)

[Csatlakozás a méhrajt fürthöz Azure tároló szolgáltatásban](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Új tároló terjesztése

Létrehoz egy új tároló a Docker Swarm, használja a `docker run` (biztosítva, hogy megnyitotta a mesteralakzatok a fenti előfeltételek szerint szeretne egy SSH alagutas) parancsot. Ez a példa létrehoz egy tárolótól a `yeasy/simple-web` képe:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

A tároló létrehozása után a `docker ps` a tároló kapcsolatos adatok visszaadására. Itt figyelje meg, hogy szerepel-e a méhrajt agent, amelyen a tároló:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Most már hozzáférhet az alkalmazás ebben a tárolóban keresztül a méhrajt ügynök terheléselosztó a nyilvános tartománynév-kiszolgáló. Ezt az információt az Azure-portálon találja meg:  


![Tényleges keresse fel az eredmények](media/real-visit.jpg)  

Alapértelmezés szerint a terheléselosztó rendelkezik portok 80, 8080 és 443 megnyitása. Ha szeretne kapcsolódni, akkor nyissa meg, hogy az Azure terheléselosztó portjához ügynök gyűjtő kell egy másik port.

## <a name="deploy-multiple-containers"></a>Több tárolók terjesztése

Több tárolók elindított, hajtja végre "docker Futtatás" többször, mint is használhatja a `docker ps` parancs látható, amely tárolja a tárolók futó. Az alábbi példában a három tárolók vannak egyenletesen a három méhrajt anyagokkal keresztül:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Tárolók üzembe Docker írása használatával

Docker írása használatával automatizálhatja a telepítési és több tárolók beállításait. Ha igen, győződjön meg arról, hogy egy biztonságos rendszerhéj (SSH) alagutas létrehoztak és, hogy a DOCKER_HOST változó beállítva (lásd a fenti előtti követelmények).

A helyi számítógépen docker-compose.yml fájl létrehozása. Ehhez használja ezt a [minta](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Futtatása `docker-compose up -d` a tároló telepítések indítása:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Végül a listában, a tárolók futó visszatér. Ebben a listában a tárolók Docker írása használatával bevezetett tükrözi:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Természetesen használható `docker-compose ps` csak meghatározott tárolók megvizsgálni a `compose.yml` fájlt.

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a Docker Swarm](https://docs.docker.com/swarm/)
