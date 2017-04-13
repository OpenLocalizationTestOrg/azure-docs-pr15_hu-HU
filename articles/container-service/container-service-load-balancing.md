<properties
   pageTitle="Azure tároló szolgáltatás fürt egyensúly tárolók betöltése |} Microsoft Azure"
   description="Egyensúly betöltése több tárolók Azure tároló szolgáltatás fürt keresztül."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Tárolók, Micro-szolgáltatások, Adatközpont/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Azure tároló szolgáltatás fürt egyenleg tárolók betöltése

Ez a cikk mutatunk be egy belső terheléselosztó létrehozása egy Adatközpont/OS felügyelt Azure tároló szolgáltatás Marathon-LB. Ezzel engedélyezi az alkalmazások vízszintesen méretezheti. Azt is lehetővé teszi a terheléselosztókkal helyez el a nyilvános fürt és az alkalmazás tárolók a magánjellegű fürt fürt ezeknek a funkcióknak a nyilvános és titkos ügynök.

## <a name="prerequisites"></a>Előfeltételek

[Azure tároló szolgáltatás egy példánya Deploy](container-service-deployment.md) orchestrator típusú Adatközpont/OS, és [biztosítani, hogy az ügyfelek a fürt csatlakozhat](container-service-connect.md). 

## <a name="load-balancing"></a>Terheléselosztás

A tároló szolgáltatás fürt azt gyűjt két terheléselosztási rétegek van: 

  1. Azure terheléselosztó nyilvános belépési pontról (a végfelhasználók fog találati lehetőségekből) tartalmaz. Ezzel automatikusan Azure tároló szolgáltatás által biztosított, és, alapértelmezés szerint be van, úgy beállítva, hogy a 80, 443-as és 8080 port nézetéhez.
  2. A Marathon terheléselosztó (marathon-lb), amely ezeket a szolgáltatáskérések tároló példányok bejövő kérések irányítja. Ahogy azt a tárolók kezeléséről a webes szolgáltatás méretezéséhez marathon-lb dinamikusan alkalmazkodik. A tároló szolgáltatás alapértelmezés szerint nem szerepel a terheléselosztó, de nagyon egyszerűen telepítése.

## <a name="marathon-load-balancer"></a>Marathon terheléselosztó

Marathon terheléselosztó dinamikusan átállítja magát a tárolók korábban telepített alapján. Érdemes emellett rugalmassá a fokú tároló vagy ügynökszoftvert – Ha ez akkor fordulhat elő, akkor Apache Mesos egyszerűen indítsa újra a tároló máshol marathon-lb alkalmazkodás fog.

Telepítheti a Marathon terheléselosztó is használhatja, vagy az Adatközpont/operációs rendszer felhasználói felület vagy a parancssorból webes.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Telepítse a Marathon-LB Adatközpont/OS webes felületének használata

  1. Kattintson a "Univerzumban"
  2. Keresés a "Marathon-LB"
  3. Kattintson a "Telepítés"

![Telepítésének marathon lb a Adatközpont/OS webes felületén keresztül](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Használatával az Adatközpont/OS CLI Marathon LB telepítése

Az Adatközpont/OS CLI telepítése, és biztosítása után a fürt, a következő parancsot a az ügyfélgép csatlakozhat:

```bash
dcos package install marathon-lb
```

Ez a parancs a nyilvános ügynökök fürt a terheléselosztó automatikusan telepíti.

## <a name="deploy-a-load-balanced-web-application"></a>A betöltés üzembe kiegyensúlyozott webalkalmazáshoz

Most, hogy elkészült a marathon-lb csomagot, azt az egy alkalmazás tároló terheléselosztó, hogy kívánt telepítheti. Ez a példa azt egyszerű webkiszolgálóra használatával az alábbi beállításokkal fog telepítése:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Értékének beállítása `HAProxy_0_VHOST` szeretne a munkatársak számára a terheléselosztó teljes Tartománynevét. Az űrlapon: az `<acsName>agents.<region>.cloudapp.azure.com`. Ha például a tároló szolgáltatás fürtre néven hozzon létre `myacs` régióban `West US`, a teljesen minősített tartománynév lenne `myacsagents.westus.cloudapp.azure.com`. Megtalálhatja a szerint keres a terheléselosztó "ügynök" a neve, ha azt szeretné megtudni, az Ön által létrehozott tároló szolgáltatás az [Azure portál](https://portal.azure.com)erőforráscsoport forrásokból.
  * Állítsa a servicePort olyan portot > = 10 000. Az ebben a tárolóban – futó szolgáltatás azonosítja marathon-lb használja a szolgáltatás, amely azt át kell egyenleg azonosítására.
  * Állítsa a `HAPROXY_GROUP` "külső" címke.
  * Beállítása `hostPort` pedig 0. Ez azt jelenti, hogy Marathon tetszőlegesen lefoglalhat egy használható port.
  * Beállítása `instances` létre szeretne hozni példányainak száma. Mindig méretezheti ezek felfelé és lefelé később.

Érdemes noing, hogy a személyes fürthöz telepítésének Marathon alapértelmezés szerint ez azt jelenti, hogy a fenti példányban csak akkor a terheléselosztó keresztül érhető el ez általában azt kívánja viselkedését.

### <a name="deploy-using-the-dcos-web-ui"></a>Üzembe Adatközpont/OS webes felületének használata

  1. Látogassa meg a Marathon a http://localhost/marathon (beállítása a [SSH alagutas](container-service-connect.md) és kattintás után`Create Appliction`
  2. Az a `New Application` párbeszédpanelen kattintson a `JSON Mode` jobb felső sarokban
  3. A fenti JSON illessze be a szerkesztő
  4. Kattintson a`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Használatával az Adatközpont/OS CLI terjesztése

Telepítéshez használni ehhez az alkalmazáshoz a Adatközpont/OS CLI egyszerűen a fenti JSON-fájlba másolása nevű `hello-web.json`, és futtatása:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure terheléselosztó

Alapértelmezés szerint az Azure terheléselosztó közzététele 80-as port 8080 és 443. Használata egy három portok (mint azt a fenti példán), majd a nincs semmilyen teendője. A ügynök terheléselosztó FQDN – találati láthatja, és minden alkalommal, amikor frissíti, fog gombra kattint a három webkiszolgálón ciklikus módon közül. Azonban egy másik port használata esetén szüksége a terheléselosztó használt portszámként ciklikus szabály és a vizsgálati hozzáadni. Ehhez az [Azure CLI](../xplat-cli-azure-resource-manager.md), a paranccsal a `azure network lb rule create` és `azure network lb probe create`. Szintén ezt megteheti az Azure-portálon.


## <a name="additional-scenarios"></a>További felhasználási területei

Ha másik tartományt használ kattintva jelenítse meg más szolgáltatások példa lehet. Példa:

Azure LB:80 mydomain1.com -> marathon ->-lb:10001 -> mycontainer1:33292  
Azure LB:80 mydomain2.com -> marathon ->-lb:10002 -> mycontainer2:22321

Cél, nézze meg [virtuális tárolja](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), amelyek adott marathon-lb elérési utakra tartományok társítása kínál.

Azt is megteheti esetleg elérhetővé tenni a különböző portokat és megfelelteti őket a megfelelő szolgáltatás marathon-lb mögött. Példa:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Következő lépések

További be [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)Adatközpont/OS dokumentációjában.
