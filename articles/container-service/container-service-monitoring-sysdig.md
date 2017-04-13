<properties
   pageTitle="Az Azure tároló szolgáltatás fürtre Sysdig figyelése |} Microsoft Azure"
   description="Figyelje az Azure tároló szolgáltatás fürtre Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Tárolók, Adatközpont/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Az Azure tároló szolgáltatás fürtre Sysdig figyelése

Ez a cikk azt fogja üzembe Sysdig ügynökök az Azure tároló szolgáltatás fürt ügynök csomópontok. Ebben a konfigurációban Sysdig rendelkező fiók szükséges. 

## <a name="prerequisites"></a>Előfeltételek 

[Deploy](container-service-deployment.md) és [Csatlakozás](container-service-connect.md) a fürtre Azure tároló szolgáltatás állítható be. Ismerje meg a [Marathon felhasználói felület](container-service-mesos-marathon-ui.md). Nyissa meg a [http://app.sysdigcloud.com](http://app.sysdigcloud.com) egy Sysdig felhő fiók beállításához. 

## <a name="sysdig"></a>Sysdig

Sysdig felügyeleti szolgáltatás, amely lehetővé teszi, hogy figyelheti a tárolók belül a fürt. Sysdig ismert hibaelhárítási segítség, de az egyszerű felügyeleti mértékek Processzor, hálózatok, memóriafoglalási és I/O is rendelkezik. Sysdig egyszerűen tekintheti meg, mely tárolók dolgozik, a hardest vagy lényegében használja a legtöbb memóriafoglalási és Processzor. Ebben a nézetben van, a béta használatban van "Áttekintése" szakaszban. 

![Sysdig felhasználói felület](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Egy Sysdig telepítésének konfigurálásához az Marathon

Ezeket a lépéseket kell követnie beállítása és helyezhetnek üzembe a fürtre Marathon Sysdig alkalmazásokat. 

Az Adatközpont/operációs rendszer felhasználói felület keresztül hozzáférés [http://localhost:80 /](http://localhost:80/) egyszer Adatközpont/operációs rendszer felhasználói felület nyissa meg a "Univerzumban", amelyben be van kapcsolva a párbeszédpanel bal alsó sarkában, és keresse meg a "Sysdig."

![Sysdig Adatközpont/OS univerzumban](./media/container-service-monitoring-sysdig/sysdig1.png)

Most már a konfigurálás befejezéséhez szükséges egy Sysdig felhő vagy ingyenes próba-fiókkal. Be van jelentkezve az Sysdig felhő webhelyre, kattintson a felhasználónevét, és az oldalon meg kell jelennie a "hívóbetű." 

![Sysdig API-ja kulcs](./media/container-service-monitoring-sysdig/sysdig2.png) 

Ezután írja be azt a hívóbetű belül az Adatközpont/OS Univerzumhoz a Sysdig konfiguráció be. 

![Az Adatközpont/OS univerzumban Sysdig konfigurálása](./media/container-service-monitoring-sysdig/sysdig3.png)

Most beállítása a 10000000 példányokat, amikor egy új csomópont bekerül a fürt Sysdig automatikusan üzembe ügynökkel, hogy új csomópontot. Ez egy ideiglenes megoldás, hogy a fürt belül minden új anyagokkal telepítésének Sysdig. 

![Az az Adatközpont/OS Univerzumban-példányok Sysdig konfigurálása](./media/container-service-monitoring-sysdig/sysdig4.png)

Telepítése után a csomag lépjen vissza a felhasználói felület Sysdig, és is feltárása a tárolók belül a fürt a különböző használatát mérési módja miatt. 