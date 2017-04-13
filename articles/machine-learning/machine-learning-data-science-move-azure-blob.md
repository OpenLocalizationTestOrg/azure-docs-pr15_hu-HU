<properties
    pageTitle="Adatok áthelyezése és az Azure Blob-tárolóhoz |} Microsoft Azure"
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
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Adatok áthelyezése és az Azure Blob-tárolóhoz

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Adatok áthelyezése és/vagy az Azure Blob-tárolóhoz használt technológiák útmutatást a itt kapcsolódnak:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Az igényektől függ, hogy melyik módszert a legjobb Önnek. A [fejlett analitikai az Azure gépi tanulási forgatókönyvek a](machine-learning-data-science-plan-sample-scenarios.md) cikk segít határozza meg a szükséges adatok tudományos munkafolyamatok a fejlett analitikai folyamatban használt sokféle erőforrások.

> [AZURE.NOTE] Egy teljes – bevezetés Azure blob-tárolóhoz olvassa el a [Azure Blob](../storage/storage-dotnet-how-to-use-blobs.md) -alapokhoz és [Azure Blob](https://msdn.microsoft.com/library/azure/dd179376.aspx)-szolgáltatásba.

Alternatív megoldásként [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) való használható: 

- Hozzon létre, és a folyamat az Azure blob-tárolóhoz adatok letöltődő ütemezése 
- a közzétett Azure gépi tanulási webszolgáltatás adja át 
- a cserélendő analytics kapni és 
- Töltse fel az eredmények tárhelyet. 

További tudnivalókért olvassa el a [Létrehozás a prediktív folyamatok Azure Data Factory és Azure gépi tanulási](../data-factory/data-factory-azure-ml-batch-execution-activity.md)című témakört.

## <a name="prerequisites"></a>Előfeltételek

A dokumentum feltételezi, hogy rendelkezik-e az Azure előfizetéssel, a tárterület-fiók és a megfelelő tároló billentyűt az adott fiókhoz. Adatok feltöltése és letöltése, mielőtt ismernie kell a tároló Azure nevét és a fiók fiókkulcs.

- Szeretne beállítani egy Azure-előfizetést, olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.
- Útmutatást a tárterület-fiók létrehozása és a fiók és a fontos információkat ismertető [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók.
