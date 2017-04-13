<properties
   pageTitle="A fejlesztői környezet Linux beállítása |} Microsoft Azure"
   description="Telepítse a futtatókörnyezet és SDK, és hozzon létre egy helyi fejlesztési fürt Linux rendszerhez. Ez a beállítás végeztével készen áll a készíthet alkalmazásokat fogja."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>A fejlesztői környezet Linux előkészítése


> [AZURE.SELECTOR]
-[A Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Üzembe helyezéséhez és [Azure Service háló alkalmazások](service-fabric-application-model.md) futtatásához a Linux fejlesztési számítógépen, telepítse a futtatókörnyezet és általános SDK csomagjában talál. Nem kötelező SDK Java és .NET Core is telepítheti.

## <a name="prerequisites"></a>Előfeltételek
### <a name="supported-operating-system-versions"></a>Támogatott operációs rendszerek
Az alábbi operációs rendszer verziója támogatott fejlesztői:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Frissítse a apt források

A SDK és a kapcsolódó futtatókörnyezet csomag apt get-e telepítendő frissítenie kell a apt forrásokból.

1. Nyissa meg a terminált.
2. A szolgáltatás háló repó hozzáadása a forrás listában.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Az új GPG kulcsának felvétele a apt kulcskarika.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Frissítse a csomag listákat, az újonnan hozzáadott tárházakban alapján.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Telepítse és állítsa be a SDK

Miután a források frissülnek, telepítheti a SDK csomagjában talál.

1. Telepítse a Service háló SDK csomagot. Akkor a rendszer kéri, erősítse meg a telepítést, és fogadja el a licencszerződést.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Futtassa a SDK telepítő.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Az Azure-platformok CLI beállítása

Az [Azure - platformok CLI] [ azure-xplat-cli-github] szolgáltatás háló szervezetek, beleértve a fürt és alkalmazások személlyel parancsokat tartalmaz. Node.js alapul Igen, [Ellenőrizze, hogy van-e telepítve csomópont] [ install-node] előtt hajtsa végre az alábbi utasításokat.

1. A fejlesztői számítógépen a github repó klónozhatja.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. A klónozott repó átváltani, és telepítse a CLI függőségek a csomópont-csomag kezelő (npm) segítségével.

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Hozzon létre egy symlink a klónozott repó, a bin és azure mappából /usr/bin/azure, hogy nem adja hozzá az elérési utat és a parancsok bármely címtárból érhetők el.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Végezetül engedélyezése automatikus-kiegészítési szolgáltatás háló parancsokat.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>A helyi fürtre beállítása

Ha mindent telepített sikeresen, láthatja, a helyi fürtre indításához.

1. Futtassa a fürt beállítása.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Nyisson meg egy webböngészőt, és kattintson a http://localhost:19080/Explorer. Ha a fürt elindult, meg kell jelennie a szolgáltatás háló Explorer irányítópult.

    ![Szolgáltatás háló Explorer Linux][sfx-linux]

Ezen a ponton tudunk üzembe beépített szolgáltatás háló alkalmazáscsomagok vagy újakat Vendég tárolók vagy Vendég végrehajtható alapján. Új szolgáltatások Java vagy .NET Core SDK létrehozásához kövesse a telepítéskor az alábbi.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Telepítse a Java SDK és Holdas Neonfény bővítményt (nem kötelező)

A Java SDK biztosít a tárak és a sablonok Java használ szolgáltatási háló szolgáltatások létrehozásához szükséges.

1. Telepítse a Java SDK csomagot.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Futtassa a SDK telepítő.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

A beépülő modul Holdas belül az Holdas Neonfény IDE a szolgáltatás háló telepítheti.

1. Holdas győződjön meg róla, hogy telepítve van a Buildship verzió 1.0.17 vagy később. Érdemes összetevőket változatának kiválasztása a **Súgó > telepítéssel kapcsolatos részleteket**. Frissítheti az útmutatást követve Buildship [Itt][buildship-update].

2. Válassza a szolgáltatás háló beépülő modul telepítéséhez **Súgó > új szoftver telepítése...**

3. Az "Használata" mezőbe írja be: http://dl.windowsazure.com/eclipse/servicefabric

4. Kattintson a Hozzáadás gombra.

    ![Holdas beépülő modul][sf-eclipse-plugin]

5. Válassza a szolgáltatás háló bővítményt, és kattintson a Tovább gombra.

6. Haladjon végig a telepítést, és fogadja el a végfelhasználói licencszerződésben találja.

## <a name="install-the-net-core-sdk-optional"></a>Telepítse a .NET Core SDK (nem kötelező)

A .NET Core SDK biztosít a tárak és a sablonok használatával platformok .NET Core szolgáltatás háló szolgáltatások létrehozásához szükséges.

1. Telepítse a .NET Core SDK csomagot.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Futtassa a SDK telepítő.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Következő lépések

- [Az első Java-alkalmazás létrehozása Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [A fejlesztői környezet a OSX előkészítése](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
