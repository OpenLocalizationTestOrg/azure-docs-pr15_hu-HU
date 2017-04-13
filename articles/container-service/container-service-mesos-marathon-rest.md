<properties
   pageTitle="Azure tároló tároló Szolgáltatáskezelés az REST API-k |} Microsoft Azure"
   description="A Marathon REST API telepítse az Azure tároló szolgáltatás Mesos fürthöz tárolók."
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

# <a name="container-management-through-the-rest-api"></a>Az REST API-k tároló kezelése

Adatközpont/operációs rendszer üzembe helyezése, és a méretezés csoportosított munkaterhelésekből, az alapul szolgáló hardver abstracting közben környezetet biztosít. Fölött Adatközpont/OS az ütemezés- és számítási feladatok végrehajtása kezelő keretrendszert.

Bár a keretek számos népszerű munkaterhelésekből érhetők el, a jelen dokumentum hogyan létrehozhat és méretezése tároló telepítések Marathon használatával ismerteti. Munka – ezek a példák, előtt Azure tároló szolgáltatásban konfigurált Adatközpont/OS fürt szüksége van. Meg kell ehhez a fürthöz távoli kapcsolatban van. További információt a ezeket az elemeket az alábbi cikkekben talál:

- [Az Azure tároló szolgáltatás fürt telepítése](container-service-deployment.md)
- [Kapcsolódás egy Azure tároló szolgáltatás fürthöz](container-service-connect.md)

Miután az Azure tároló szolgáltatás fürtre kapcsolódik, az Adatközpont/operációs rendszer és a kapcsolódó REST API-k elérheti http://localhost:local nyílású keresztül. Példák a dokumentumban feltételezik, hogy a 80-as port tunneling. Ha például a Marathon végpont érhető el a `http://localhost/marathon/v2/`. A különböző API-k kapcsolatos további tudnivalókért olvassa el a Mesosphere dokumentációjában találhat a [Marathon API -val](https://mesosphere.github.io/marathon/docs/rest-api.html) és a [Chronos API -val](https://mesos.github.io/chronos/docs/api.html)és a Apache dokumentációjában találhat a [Mesos ütemező API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)című témakört.

## <a name="gather-information-from-dcos-and-marathon"></a>Információk összegyűjtése az Adatközpont/OS és Marathon

Mielőtt beállítaná a Adatközpont/OS fürt tárolók, gyűjtse össze a Adatközpont/OS fürt, például a nevek és aktuális állapotát az Adatközpont/OS ügynökök információkat. Ehhez a lekérdezés a `master/slaves` végpontját az Adatközpont/OS REST API-t. Ha mindent mellé kerül, láthatja a Adatközpont/OS anyagokkal, és több tulajdonságának minden egyes.

```bash
curl http://localhost/mesos/master/slaves
```

Most, használja a Marathon `/apps` végpontot, jelölje be a Adatközpont/OS fürthöz aktuális alkalmazás telepítésekhez. Ha egy új fürt, üres tömböt-alkalmazások jelenik meg.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>A tároló Docker formázott terjesztése

Tárolók Docker formázott Marathon keresztül telepíteni, amely leírja a tervezett példányban JSON-fájl használatával. Az alábbi példa a Nginx tároló, a 80-as port kötés, a tároló 80-as port az Adatközpont/OS ügynök telepíteni fog. Tartsa szem előtt, hogy a "acceptedResourceRoles" tulajdonság értéke "slave_public". A tároló telepítésének a nyilvános elérésű ügynök skála megadása ügynökszoftvert.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Docker formázott tároló telepítéséhez saját JSON-fájl létrehozása, vagy használja a megadott [Azure tároló szolgáltatás bemutatása](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)a minta gombra. Az elérhető helyen tárolja azt. Ezután a tároló telepítéséhez futtassa a következő parancsot. Adja meg a JSON-fájl nevét.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

A kimenet a következőhöz hasonló lesz:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Ezután ha alkalmazásokhoz a Marathon lekérdezés az új alkalmazás jeleníti meg az eredményben.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>A tárolók méretezése

A Marathon API is használhatja méretezheti vagy átméretezheti a alkalmazás környezetekben. Az előző példában egy példánya az alkalmazás telepítését. Vegyük méretezze át a ki az alkalmazások három példányát. Ehhez a JSON-fájl létrehozása a következő JSON szöveg használatával, és elérhető helyen tárolja azt.

```json
{ "instances": 3 }
```

Futtassa a következő parancsot, ha át kívánja méretezni, az alkalmazás.

>[AZURE.NOTE] A URI lesz http://localhost/marathon/v2/apps/, majd az alkalmazás, ha át kívánja méretezni azonosítója. Ha a Nginx minta biztosított ebben az esetben a URI lenne http://localhost/marathon/v2/apps/nginx használja.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Végül a lekérdezés alkalmazások Marathon végpontot. Láthatja, hogy jelenleg három a Nginx tárolók.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Ebben a gyakorlatban PowerShell-lel: PowerShell Marathon REST API-interakció

Windows rendszeren PowerShell-parancsok használatával ezeket a műveleteket végezhet.

Az Adatközpont/OS fürt, például ügynök-agent állapotát, és információt gyűjthet a következő parancsot.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Tárolók Docker formázott Marathon keresztül telepíteni, amely leírja a tervezett példányban JSON-fájl használatával. Az alábbi példa a Nginx tároló, a 80-as port kötési az Adatközpont/OS ügynök 80-as port a tároló való telepítésének.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Hozzon létre saját JSON-fájlt, vagy használja a megadott [Azure tároló szolgáltatás bemutatása](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)a minta. Az elérhető helyen tárolja azt. Ezután a tároló telepítéséhez futtassa a következő parancsot. Adja meg a JSON-fájl nevét.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

A Marathon API is használhatja méretezheti vagy átméretezheti a alkalmazás környezetekben. Az előző példában egy példánya az alkalmazás telepítését. Vegyük méretezze át a ki az alkalmazások három példányát. Ehhez a JSON-fájl létrehozása a következő JSON szöveg használatával, és elérhető helyen tárolja azt.

```json
{ "instances": 3 }
```

Futtassa a következő parancsot, ha át kívánja méretezni, az alkalmazás.

> [AZURE.NOTE] A URI lesz http://localhost/marathon/v2/apps/, majd az alkalmazás, ha át kívánja méretezni azonosítója. A megadott Nginx minta itt használja, az URI akkor http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Következő lépések

- [További információk: a Mesos HTTP végpontok]( http://mesos.apache.org/documentation/latest/endpoints/).
- [További információk: a Marathon REST API -t]( https://mesosphere.github.io/marathon/docs/rest-api.html).
