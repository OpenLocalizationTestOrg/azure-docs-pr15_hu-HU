<properties
   pageTitle="Service háló és Linux a központi telepítés tárolók |} Microsoft Azure"
   description="A szolgáltatás és Docker tárolók e microservice alkalmazások telepítéséhez. Ez a cikk ismerteti a lehetőségeket nyújtó szolgáltatás háló tárolók és Docker tároló képként fürt való telepítéséről"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Szolgáltatás háló Docker tároló telepítése

> [AZURE.SELECTOR]
- [A Windows tároló terjesztése](service-fabric-deploy-container.md)
- [Docker tároló terjesztése](service-fabric-deploy-container-linux.md)

Ez a cikk végigvezeti a Docker tárolók indexelése services támaszkodva Linux.

Szolgáltatás háló segítséget nyújt a microservices, amelyek indexelése állnak alkalmazások létrehozásába több tároló-funkciókkal rendelkezik. Ezek az úgynevezett indexelése szolgáltatások.

A funkciókat tartalmazza;

- Tároló kép telepítési és aktiválási
- Erőforrás irányítási
- Tárházba hitelesítés
- A host port hozzárendelése tároló port
- A tároló-tároló feltárás és a kommunikáció
- Állítsa be, és a változók környezet beállítása


## <a name="packaging-a-docker-container-with-yeoman"></a>Docker tároló csomagolása, yeoman
Amikor a tároló Linux csomagolása, egy yeoman sablon vagy [Alkalmazáscsomag létrehozása manuálisan](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container)választhatja.

A szolgáltatás háló alkalmazások egy vagy több tárolók, az adott jogosultság az alkalmazás működésének előadásához az egyes is tartalmazhat. A szolgáltatás háló SDK Linux tartalmaz egy [Yeoman](http://yeoman.io/) nyilvántartás-készítő alkalmazás, amely megkönnyíti az alkalmazás létrehozása és tároló beállítása. Yeoman segítségével egyetlen Docker tároló *SimpleContainerApp*nevű új alkalmazás létrehozása. További szolgáltatások később a létrehozott szerkesztésével cikkét fájlokat is hozzáadhat.

## <a name="create-the-application"></a>Az alkalmazás létrehozása

1. Írja be a terminálablakba **yo azuresfguest**.

2. A keret **tároló** közül.

3. Nevezze el az alkalmazást, például SimpleContainerApp

4. A tároló képet egy DockerHub repó a szükséges URL-CÍMÉT. Ekkor az űrlap [repó] / [kép neve]

![Tárolók szolgáltatás háló Yeoman létrehozója][sf-yeoman]

## <a name="deploy-the-application"></a>Az alkalmazás telepítéséhez

Ha elkészült az alkalmazást, az Azure CLI használata a helyi fürthöz telepítheti azt.

1. A helyi szolgáltatás háló fürthöz csatlakozni.

    ```bash
    azure servicefabric cluster connect
    ```

2. Alkalmazáscsomag másolása a fürt kép áruházból, regisztráljon az alkalmazás típusától és az alkalmazás egy példányának létrehozása a sablon kapott telepítési parancsfájlt használatával.

    ```bash
    ./install.sh
    ```

3. Nyissa meg a böngészőt, és kattintson a szolgáltatás háló Explorer, a http://localhost:19080/Explorer (csere localhost a virtuális Vagrant használata Mac OS X rendszeren, a személyes IP-) elemre.

4. Bontsa ki az alkalmazások csomópontot, és figyelje meg, hogy már létezik egy bejegyzést a alkalmazás típusát, és egy másikat a az első példányt az adott típusú.

5. A sablonban kapott eltávolítás parancsfájlt használja az alkalmazás-példány törlése és az alkalmazás típusa.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Következő lépések

- [Szolgáltatás háló és tárolók – áttekintés](service-fabric-containers-overview.md)
- [Az Azure CLI használ szolgáltatási háló fürt használata](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

