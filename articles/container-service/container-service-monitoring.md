<properties
   pageTitle="Az Azure tároló szolgáltatás fürtre Datadog figyelése |} Microsoft Azure"
   description="Figyelje az Azure tároló szolgáltatás fürtre Datadog. Használatával az Adatközpont/OS webes felület a Datadog anyagokkal a fürthöz."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Tárolók, Adatközpont/OS Docker méhrajt, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Az Azure tároló szolgáltatás fürtre Datadog figyelése

Ez a cikk azt fog üzembe Datadog ügynökök az Azure tároló szolgáltatás fürt ügynök csomópontok. Ebben a konfigurációban szüksége lesz egy Datadog fiókot. 

## <a name="prerequisites"></a>Előfeltételek 

[Deploy](container-service-deployment.md) és [Csatlakozás](container-service-connect.md) a fürtre Azure tároló szolgáltatás állítható be. Ismerje meg a [Marathon felhasználói felület](container-service-mesos-marathon-ui.md). Nyissa meg a [http://datadoghq.com](http://datadoghq.com) egy Datadog fiók beállításához. 

## <a name="datadog"></a>Datadog 

Datadog olyan felügyeleti szolgáltatás, amely a megfigyeléssel adatait gyűjti össze a tárolók az Azure tároló szolgáltatás fürt belül. Datadog Itt láthatja adott mértékek belül a tárolók Docker integrációs irányítópult tartalmaz. A tárolók gyűjtött mértékek Processzor, a memóriahasználat, a hálózati és I/O szerint vannak rendezve. Datadog mértékek osztja tárolók és a képeket. Példa a felhasználói felület néz ki az processzorhasználata nem éri el.

![Datadog felhasználói felület](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Egy Datadog telepítésének konfigurálásához az Marathon

Ezeket a lépéseket kell követnie beállítása és helyezhetnek üzembe a fürtre Marathon Datadog alkalmazásokat. 

Az Adatközpont/operációs rendszer felhasználói felület keresztül hozzáférés [http://localhost:80 /](http://localhost:80/). Egyszer Adatközpont/operációs rendszer felhasználói felület nyissa meg a "Univerzumban" Ez a párbeszédpanel bal alsó sarkában, és keresse meg a "Datadog", és kattintson a "Telepítés"

![Az Adatközpont/OS Univerzumhoz belül Datadog csomag](./media/container-service-monitoring/datadog1.png)

Most már a konfigurálás befejezéséhez szüksége lesz egy Datadog vagy ingyenes próba-fiókkal. Miután a Datadog webhely megjelenésének balra van bejelentkezve, és nyissa meg a integrációs -> majd API. 

![Datadog API-ja kulcs](./media/container-service-monitoring/datadog2.png)

Ezután írja be azt az API-ja kulcs a Datadog konfiguráció belül az Adatközpont/OS Univerzumhoz be. 

![Az Adatközpont/OS univerzumban Datadog konfigurálása](./media/container-service-monitoring/datadog3.png) 

A fenti konfiguráció példányok 10000000 vannak beállítva, amikor egy új csomópont bekerül a fürtre Datadog automatikusan üzembe ügynökkel, hogy csomópontra. Ez egy ideiglenes megoldás. Miután telepítette a csomagot kell vissza szeretne térni a Datadog webhelyet, és keresse meg a "Irányítópultok." Innen látni fogja a egyéni és integráció irányítópultok. Docker integrációs irányítópult lesz szüksége a fürtre figyelemmel kísérésére tároló mértékek. 
