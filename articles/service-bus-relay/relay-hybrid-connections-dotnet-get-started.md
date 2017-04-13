<properties
    pageTitle="Első lépések a továbbítási hibrid kapcsolatok |} Microsoft Azure"
    description="A C# console-alkalmazások szövegalkotás hibrid kapcsolatok"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Első lépések a továbbítási hibrid kapcsolatok

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Mit fog elvégezhető

Hibrid kapcsolatok igényel egy ügyfél és a kiszolgáló-összetevő, mert azt a két konzol alkalmazás ebben az oktatóanyagban hoz létre. A lépések a következők:

1. Hozzon létre egy továbbítási névtér, az Azure portálon.

2. Az Azure portálon hibrid kapcsolat létrehozásához.

3. Írja be a kiszolgáló üzenetek fogadására New.

4. Írjon egy ügyfél New üzeneteket küldhet.

## <a name="prerequisites"></a>Előfeltételek

1. [Visual Studio 2013 vagy a Visual Studio 2015](http://www.visualstudio.com). Ebben az oktatóanyagban példákban a Visual Studio 2015.

2. Egy Azure-előfizetést.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. a Azure portálon névtér létrehozása

Ha már létrehozott továbbítási névteret, [hibrid kapcsolat létrehozása az Azure portál használatával](#2-create-a-hybrid-connection-using-the-azure-portal) című ugorhat.

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. a Azure portálon hibrid kapcsolat létrehozása

Ha már létrehozott hibrid kapcsolatot, [a kiszolgáló-alkalmazás létrehozása](#3-create-a-server-application-listener) szakaszban ugorhat.

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. (figyelő) kiszolgáló-alkalmazás létrehozása

Figyelje, és a továbbítási érkező üzenetek fogadására, a C# console-alkalmazások használata a Visual Studio írja azt.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. (Feladó) ügyfél-alkalmazás létrehozása

A továbbítási üzeneteket küldeni, a C# console-alkalmazások használata a Visual Studio írja azt.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Indítsa el az alkalmazások

1. A kiszolgáló alkalmazásnak a futtatására.

2. Futtassa az ügyfélalkalmazás, és írjon be valamilyen szöveget.

3. Győződjön meg arról, hogy a kiszolgáló alkalmazás konzol exportálja a az ügyfélalkalmazás beírt szöveget.

![futó-alkalmazások](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Gratulálunk, a végpontok közötti hibrid kapcsolatok alkalmazás létrehozott.

## <a name="next-steps"></a>A következő lépéseket:

- [Továbbítás – gyakori kérdések](relay-faq.md)
- [Névtér létrehozása](relay-create-namespace-portal.md)
- [Csomópont – első lépések](relay-hybrid-connections-node-get-started.md)