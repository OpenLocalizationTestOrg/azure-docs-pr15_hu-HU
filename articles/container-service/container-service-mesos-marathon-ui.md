<properties
   pageTitle="Azure tároló tároló Szolgáltatáskezelés révén a webes felhasználói felület |} Microsoft Azure"
   description="Tárolók az Azure tároló fürt szolgáltatás üzembe Marathon webes felhasználói felület használatával."
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
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>A felhasználói felület webes keresztül tároló kezelése

Adatközpont/operációs rendszer üzembe helyezése, és a csoportosított munkaterhelésekből, átméretezés közben a mögöttes hardver abstracting környezetet biztosít. Fölött Adatközpont/OS az ütemezés- és számítási feladatok végrehajtása kezelő keretet.

Keretek számos népszerű munkaterhelésekből érhetők el, miközben a dokumentum leírja, hogyan létrehozása és a Marathon tároló telepítések méretezni. Ezek a példák keresztül munka, előtt szüksége lesz az Azure tároló szolgáltatás konfigurált Adatközpont/OS fürt. Meg kell ehhez a fürthöz távoli kapcsolatban van. További információt a ezeket az elemeket az alábbi cikkekben talál:

- [Az Azure tároló szolgáltatás fürt terjesztése](container-service-deployment.md)
- [Csatlakozzon az Azure tároló szolgáltatás fürthöz](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>Ismerje meg az Adatközpont/OS a felhasználói felület

A biztonságos rendszerhéj (SSH) alagutas, létrehozott keresse meg azt a http://localhost/. Ez az Adatközpont/OS webes felület betölti, és a fürt, például használt erőforrások, aktív ügynökök és futó szolgáltatások vonatkozó információkat jelenít meg.

![ADATKÖZPONT/OPERÁCIÓS RENDSZER FELHASZNÁLÓI FELÜLET](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Ismerje meg a Marathon felhasználói felület

A felhasználói felület Marathon megtekintéséhez nyissa meg azt a http://localhost/Marathon. A képernyőn, elindíthatja új tároló vagy más alkalmazásba az Azure tároló szolgáltatás Adatközpont/OS fürt. Tárolók és alkalmazások futtatásával kapcsolatos információkat is láthatja.  

![Marathon felhasználói felület](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>A tároló Docker formázott terjesztése

Új tároló Marathon telepítéséhez kattintson az **Alkalmazás létrehozása** gombra, és írja be az alábbi adatokat az űrlapon:

A mező           | Érték
----------------|-----------
AZONOSÍTÓ              | nginx
Kép           | nginx
Hálózati         | Össze
A Host Port       | 80
Protocol (protokoll)        | TCP

![Új alkalmazás felhasználói felület – általános](media/dcos/dcos4.png)

![Új alkalmazás felhasználói felület – Docker tároló](media/dcos/dcos5.png)

![Új alkalmazás felhasználói felület – portokat és feltárása](media/dcos/dcos6.png)

Ha szeretne a tároló port statikus megfeleltetése olyan portot a Agent, JSON üzemmód szüksége. Ha igen, váltson az új alkalmazás varázsló **JSON mód** a kapcsoló használatával. Írja be a következő csoportban a `portMappings` az alkalmazásdefiníció szakaszát. Ebben a példában a tároló 80-as port 80-as port az Adatközpont/OS ügynök köti. Ez a módosítás elvégzése után a varázsló kívül JSON mód válthat.

```none
"hostPort": 80,
```

![Új alkalmazás felhasználói felület – például a 80-as port](media/dcos/dcos13.png)

Az Adatközpont/OS fürtre telepítve van a személyes és nyilvános ügynökök csoportja. A fürt fér hozzá az alkalmazások az internetről is telepíteni kell egy nyilvános ügynök az alkalmazásokat. Ehhez jelölje be a **választható** lap az új alkalmazás varázsló, és írja be az **Erőforrás szerepkörök elfogadott** **slave_public** .

![Új alkalmazás felhasználói felület – nyilvános ügynökök beállítása](media/dcos/dcos14.png)

Vissza a Marathon fő lapon megtekintheti a tároló telepítési állapotának.

![Marathon főoldalra felhasználói felület – tároló telepítési állapota](media/dcos/dcos7.png)

Amikor vált vissza az Adatközpont/OS webes felhasználói felület (http://localhost/), látni fogja, hogy egy tevékenység (ebben az esetben az Docker formázott tároló) az Adatközpont/OS fürt fut-e.

![Adatközpont/OS webes felület – a a fürthöz tevékenység](media/dcos/dcos8.png)

A tevékenység futtató csomópont is láthatja.

![Adatközpont/OS webes felület – tevékenység fürt csomópont](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>A tárolók méretezése

Ha át kívánja méretezni tároló példány számát a felhasználói felület Marathon is használhatja. Ehhez nyissa meg a **Marathon** lapot, jelölje be a tárolót, ha át kívánja méretezni, amelyet, és kattintson a **méret** gombra. **Méretezés alkalmazás** párbeszédpanelen adja meg a kívánt tároló példányok számát, és jelölje ki a **Méretezés-alkalmazás**.

![Marathon felhasználói felület – skála alkalmazás párbeszédpanel](media/dcos/dcos10.png)

A méretezés művelet befejezése után jelenik meg több példányával Adatközpont/OS anyagokkal között ugyanaz a feladat.

![Adatközpont/OS webes felhasználói felület irányítópult ügynökök végig a várható tevékenység](media/dcos/dcos11.png)

![Adatközpont/OS webes felület – csomópontok](media/dcos/dcos12.png)

## <a name="next-steps"></a>Következő lépések

- [Adatközpont/operációs rendszer és a Marathon API](container-service-mesos-marathon-rest.md)

Az Azure tároló szolgáltatása a Mesos mély merülési

> [AZURE. Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos videó]]
