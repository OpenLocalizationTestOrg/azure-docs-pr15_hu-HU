<properties
   pageTitle="Mac OS X rendszeren a fejlesztői környezet beállítása |} Microsoft Azure"
   description="A futási idejű, SDK és eszközök telepítése, és hozzon létre egy helyi fejlesztési fürt. Ez a beállítás végeztével készen áll a Mac OS X rendszeren alkalmazások fogja."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Mac OS X rendszeren a fejlesztői környezet beállítása

> [AZURE.SELECTOR]
-[A Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Mac OS X használatával Linux fürt szolgáltatás háló alkalmazásokat készíthet. Ez a cikk bemutatja, hogy miként állíthatja be a Mac fejlesztési.

## <a name="prerequisites"></a>Előfeltételek

Szolgáltatás háló nem futtatási OS X rendszeren. A helyi szolgáltatás háló fürtre futtatásához kínálunk, amely egy előre konfigurált Ubuntu virtuális gép Vagrant és VirtualBox használatával. Mielőtt hozzákezdene, lesz szüksége:

- [Vagrant (v1.8.4 vagy újabb verzió)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>A helyi virtuális létrehozása

A csomópont-5 szolgáltatás háló fürt tartalmazó helyi virtuális létrehozásához tegye a következőket:

1. A Vagrantfile repó másolása

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Nyissa meg a helyi adatfeliratsor a repó:

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Nem kötelező) Az alapértelmezett virtuális beállításainak módosítása

    Alapértelmezés szerint a helyi virtuális van beállítva az alábbi képlettel történik:

    - 3 GB memóriát kiosztott
    - Magánjellegű host hálózatnak IP 192.168.50.50 a forgalmat a Mac állomáswebhelyén átadó engedélyezése

    Ezek a beállítások bármelyikének módosítása, illetve egyéb konfigurációs hozzáadása a virtuális a Vagrantfile a. A teljes listáját a beállítási lehetőségek a [Vagrant dokumentációjában](http://www.vagrantup.com/docs) találhat.

4. A virtuális létrehozása

    ```bash
    vagrant up
    ```

    Ebben a lépésben előre beállított virtuális képe, rajta fürt helyi meghajtóra, és majd állítsa be a helyi szolgáltatás háló indítási letöltését. Érdemes a várt néhány percet igénybe vehet. Ha a telepítő sikeresen végrehajtotta, látni fogja az eredményt ad, hogy a fürt indítását jelző üzenet.

    ![A beállítás indítása követő virtuális kiépítési fürt][cluster-setup-script]

5. Tesztelje, hogy a fürt beállította megfelelően navigálással szolgáltatás háló Explorer http://192.168.50.50:19080/Explorer (feltételezve, hogy az alapértelmezett magánhálózati IP maradt) elemre.

    ![Szolgáltatás háló Explorer megtekintett Mac számítógépről][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>A szolgáltatás háló beépülő modul telepítése Holdas Neonfény (nem kötelező)

Szolgáltatás háló biztosít az Holdas Neonfény IDE, amely is egyszerűbbé összeállítását, és Java services telepítése egy bővítményt.

1. Holdas győződjön meg róla, hogy telepítve van a Buildship verzió 1.0.17 vagy később. Érdemes összetevőket változatának kiválasztása a **Súgó > telepítéssel kapcsolatos részleteket**. Frissítheti az útmutatást követve Buildship [Itt][buildship-update].

2. Válassza a szolgáltatás háló beépülő modul telepítéséhez **Súgó > új szoftver telepítése...**

3. Az "Használata" mezőbe írja be: http://dl.windowsazure.com/eclipse/servicefabric.

4. Kattintson a Hozzáadás gombra.

    ![A szolgáltatás háló Holdas Neonfény beépülő modul][sf-eclipse-plugin-install]

5. Válassza a szolgáltatás háló bővítményt, és kattintson a Tovább gombra.

6. Haladjon végig a telepítést, és fogadja el a végfelhasználói licencszerződésben találja.

## <a name="next-steps"></a>Következő lépések

- [Az első szolgáltatás háló alkalmazáson Linux létrehozása](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [A szolgáltatás háló fürtre létrehozása az Azure-portálon](service-fabric-cluster-creation-via-portal.md)
- [Az Azure erőforrás-kezelő használ szolgáltatási háló fürt létrehozása](service-fabric-cluster-creation-via-arm.md)
- [A szolgáltatás háló alkalmazásmodell ismertetése](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
