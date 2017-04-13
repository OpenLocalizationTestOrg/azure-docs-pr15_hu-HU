<properties
   pageTitle="Az első szolgáltatás háló-alkalmazás létrehozása használatával C# Linux |} Microsoft Azure"
   description="Hozzon létre, és egy C használ szolgáltatási háló alkalmazások telepítése#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Az első Azure Service háló-alkalmazás létrehozása

> [AZURE.SELECTOR]
- [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Szolgáltatás háló SDK nyújt a szolgáltatások támaszkodva .NET Core és Java Linux rendszerhez. Ebben az oktatóanyagban megnézi a Linux-alkalmazás létrehozása és használata a C# (.NET Core) szolgáltatás összeállítása.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt hozzákezdene, győződjön meg arról, hogy [a Linux fejlesztői környezet beállítása](service-fabric-get-started-linux.md). Ha a Mac OS X rendszer használata esetén is [Vagrant használatáról virtuális gépen Linux egy beépített környezet beállítása](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Az alkalmazás létrehozása

A szolgáltatás háló alkalmazások egy vagy több szolgáltatás, az adott jogosultság az alkalmazás működésének előadásához az egyes is tartalmazhat. A szolgáltatás háló SDK Linux tartalmaz egy [Yeoman](http://yeoman.io/) nyilvántartás-készítő alkalmazás, amely megkönnyíti az első szolgáltatás hozzon létre, és további később szeretne felvenni. Yeoman segítségével egyetlen szolgáltatás hozzon létre egy alkalmazást.

1. A terminált írja be a következő parancs, amellyel a állványon létrehozása:`yo azuresfcsharp`

2. Adjon nevet az alkalmazás.

3. Válassza ki az első szolgáltatás típusát, és nevezze el. Ebben az oktatóanyagban alkalmazásában válassza azt a megbízható szereplő szolgáltatás.

  ![Szolgáltatás háló Yeoman létrehozója c#][sf-yeoman]

>[AZURE.NOTE] A beállításokkal kapcsolatos további tudnivalókért [szolgáltatás háló programozási modell áttekintése](service-fabric-choose-framework.md)című témakörben találhat.

## <a name="build-the-application"></a>Az alkalmazás összeállítása

A szolgáltatás háló Yeoman sablonok (után az alkalmazás mappára lépés) az alkalmazás a terminált létrehozásához használható build parancsfájl tartalmazza.

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Indítsa el a próba-ügyfelet, és hajtsa végre a feladatátvevő

Szereplő projektek nem bármit önállóan. Másik szolgáltatásra vagy ügyfél üzeneteket küld van szükségük. A szereplő sablon tartalmaz egy egyszerű próba parancsfájlt, a szereplő szolgáltatás vezérléséhez használható.

1. Futtassa a parancsfájl a Megtekintés segédprogrammal a szereplő szolgáltatás a kimenet jelenik meg.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. A szolgáltatás háló Intézőben keresse meg a csomópont-szolgáltatója, az elsődleges replika szereplő szolgáltatás. Az alábbi képernyőképen 3 csomópontot.

    ![Az elsődleges replika szolgáltatás háló Explorer keresése][sfx-primary]

3. Kattintson a csomópontra, akkor az előző lépésben található, majd a Műveletek menüből válassza az **Inaktiválás (újraindítás)** . Ez a művelet a helyi fürtre feladatátvevő kényszerítése egy másik csomóponton futó másodlagos replikával egyet az öt csomópontok újraindul. Ez a művelet végrehajtásakor, figyelmet a kimenet a próba-ügyfél és látható, hogy a számláló továbbra is a feladatátvételi ellentétben növelése.


## <a name="next-steps"></a>Következő lépések

- [További tudnivalók a megbízható szereplők](service-fabric-reliable-actors-introduction.md)
- [Az Azure CLI használ szolgáltatási háló fürt használata](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
