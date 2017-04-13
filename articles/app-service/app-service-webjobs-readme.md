<properties
    pageTitle="Azure App szolgáltatásban WebJobs"
    description="Megtudhatja, hogyan hozhat létre WebJobs háttér vizsgálatok futtatása, szolgáltatásai, például tárhely és a szolgáltatás Bus használata és létrehozása ütemezett tevékenységek."
    services="app-service"
    documentationCenter=""
    authors="christopheranderson"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="chrande"/>

# <a name="using-webjobs-in-azure-app-service"></a>Azure alkalmazás szolgáltatás WebJobs használata

Ez a cikk forrásokra mutató hivatkozásokat dokumentáció Azure WebJobs és a Azure WebJobs SDK csomagjában talál tájékoztatást. Azure WebJobs futtatandó programok telepítése és parancsfájlok háttérfolyamatok [szolgáltatás webes](http://go.microsoft.com/fwlink/?LinkId=529714)alkalmazásindítónalkalmazások ugyanígy biztosítanak. Töltse fel és a cmd karaktereket, például egy végrehajtható fájl futtatása Bát, exe (.NET), ps1, sh, php, másolása, js és üveg. Ezeket a programokat futtatása WebJobs időközönként (cron) vagy folyamatosan.

A WebJobs SDK megkönnyíti az Azure tárolás használja. A WebJobs SDK csomagjában talál egy kötés, és a kiváltó ok mező rendszerben, mely működik-e a Microsoft Azure tárolás BLOB, sorok és táblázatok, valamint szolgáltatás Bus sorban várakozó tartalmaz.

Létrehozás, üzembe helyezése, WebJobs kezelése az a Visual Studio integrált szerszámok zökkenőmentesen elvégezhető. WebJobs létrehozása sablonokból, közzététele és kezelése (Futtatás/leállítása/monitor/hibakeresési) őket.

Az WebJobs irányítópultot az Azure-portálon biztosít hatékony szolgáltatásairól, amelyek segítségével a teljes hozzáféréssel WebJobs, beleértve az azt jelenti, hogy egyes függvények belül WebJobs meghívása végrehajtását. Az irányítópult is szükséges függvény és a naplózás kimeneti jeleníti meg.

[AZURE.INCLUDE [app-service-blueprint-webjobs](../../includes/app-service-blueprint-webjobs.md)]
