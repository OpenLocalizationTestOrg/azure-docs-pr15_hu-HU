<properties
    pageTitle="Az Azure portálon szolgáltatás Bus névtér létrehozása |} Microsoft Azure"
    description="Annak érdekében, hogy az első lépések a szolgáltatás Bus, szüksége lesz egy névtér. Az alábbi módszerrel hozhat létre egyet az Azure portálon."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Az Azure portálon szolgáltatás Bus névtér létrehozása

Névtér az összes üzenetben összetevőhöz közös tároló. Sorok és a témakörök egyetlen névteret találhatók, és névtér gyakran alkalmazás tárolók lesz. Kétféleképpen jelenleg különböző szolgáltatás Bus névtér létrehozása.

1.  Azure portal (Ez a cikk)

2.  [Erőforrás-kezelő sablonok][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Névtér létrehozása az Azure-portálon

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gratulálok! Ezzel létrehozott egy szolgáltatás Bus üzenetküldés névtér.

## <a name="next-steps"></a>Következő lépések

Nézze meg a [GitHub minták] [ github-samples] amelyek megjelenítése az Azure Bus üzenetküldés speciális funkciók közül egyesek.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples