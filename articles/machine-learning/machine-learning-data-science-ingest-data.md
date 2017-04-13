<properties 
    pageTitle="Adatok betöltése tárterület-verziót tartalmazó környezetek elemzéséhez |} Microsoft Azure" 
    description="Adatok áthelyezése és az Azure Blob-tárolóhoz" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Adatok betöltése tárterület-verziót tartalmazó környezetek elemzéséhez

A csoportwebhely adatok tudományos folyamat szükséges adatokat keresztül a szervezetbe vagy más-más tárolási környezetekben feldolgozása, vagy a folyamat minden egyes szakaszban a leginkább megfelelő módon elemzett számos betöltött. Adatok célok feldolgozás a gyakran használt Azure Blob-tárolóhoz, az SQL Azure-adatbázisokhoz, SQL Server Azure virtuális, HDInsight (Hadoop) és Azure gépi tanulási tartalmazzák. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Ez a **menü** , amelyek adatok ingest ezek cél környezetekben, ahol az adatokat tárolja és feldolgozása témakörökre mutató hivatkozásokat tartalmaz.

Műszaki és az üzleti igények, valamint a kezdeti hely, formázhatja, és az adatok méretének meghatározza, hogy a célhely környezetben, amelybe az adatokat kell keresztül kell a szervezetbe az elemzés a kitűzött célok eléréséhez. Még nem ritka, hogy a példa az adatok áthelyezésére között több környezetekben a különböző prediktív modell létrehozásához szükséges feladatokat eléréséhez szükséges. A feladatok sorrendjének tartalmazhatnak, például adatok feltárása, előtti feldolgozás, az eltávolítás, lefelé-mintavételnél, és modell oktatás.
