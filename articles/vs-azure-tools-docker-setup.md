<properties
   pageTitle="Állítson be egy Docker Host VirtualBox |} Microsoft Azure"
   description="Egy alapértelmezett Docker példányára Docker gépi és VirtualBox konfigurálása című témakörben ismertetettek"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Állítson be egy Docker Host VirtualBox

## <a name="overview"></a>– Áttekintés
Ez a cikk végigvezeti Önt a Docker gépi és VirtualBox alapértelmezett Docker példány konfigurálása. A [Docker Windows béta](http://beta.docker.com/)használja, ha ez a beállítás nincs szükség.

## <a name="prerequisites"></a>Előfeltételek
A következő eszközök telepítésére van szükség.

- [Docker eszköztára](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>A Docker ügyfél beállítása a Windows PowerShell

Egy Docker ügyfélprogram konfigurálását, egyszerűen nyissa meg a Windows Powershellt, és hajtsa végre az alábbi lépéseket:

1. Hozzon létre egy alapértelmezett docker host példányt.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Ellenőrizze, hogy az alapértelmezett példány beállítva, és folytonos. (Meg kell jelennie egy példánya fut, az "alapértelmezett" nevű.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![docker-gép ls kimeneti][0]
 
1. Alapértelmezett beállítása az aktuális állomás, és állítsa be a rendszerhéj.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Az aktív Docker tárolók megjelenítéséhez. A lista üres kell lennie.

    ```PowerShell
    docker ps
    ```

    ![docker ps kimeneti][1]
 
> [AZURE.NOTE]Minden alkalommal, amikor a fejlesztés a számítógépen, indítsa újra kell indítani a helyi docker szolgáltató.
> Ehhez hiba a parancssorban az alábbi parancsot: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
