<properties 
    pageTitle="Adatok áthelyezése és az Azure Blob-tárolóhoz Azure tároló Intézővel |} Microsoft Azure" 
    description="Adatok áthelyezése és az Azure tároló Intézővel Azure Blob-tárolóhoz" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Adatok áthelyezése és az Azure tároló Intézővel Azure Blob-tárolóhoz

Azure tároló Explorer, amely lehetővé teszi, hogy Windows macOS és Linux Azure tároló adatok használata a Microsoft ingyenes eszköz. Ez a témakör ismerteti, hogyan tölthet fel, és az adatok letöltése a Azure blob-tárolóhoz használatához. Az eszköz letölthető a [Microsoft Azure tároló Intézőből](http://storageexplorer.com/).

Adatok áthelyezése és/vagy az Azure Blob-tárolóhoz használt technológiák útmutatást a itt kapcsolódnak:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Ha használ, úgy lett beállítva az [Azure-ban adatok tudományos virtuális gépeken futó](machine-learning-data-science-virtual-machines.md)által biztosított parancsfájlok virtuális, majd Azure tároló Explorer már telepítve van a virtuális.
 
> [AZURE.NOTE] Azure blob-tárolóhoz teljes bevezetéséhez olvassa el az [Azure Blob-alapjai](../storage/storage-dotnet-how-to-use-blobs.md) és [Azure Blob-szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Előfeltételek

A dokumentum feltételezi, hogy rendelkezik-e az Azure előfizetéssel, a tárterület-fiók és a megfelelő tároló billentyűt az adott fiókhoz. Adatok feltöltése és letöltése, mielőtt ismernie kell a tároló Azure nevét és a fiók fiókkulcs. 

- Szeretne beállítani egy Azure-előfizetést, olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.
- Útmutatást a tárterület-fiók létrehozása és a fiók és a fontos információkat ismertető [kapcsolatos Azure tároló fiókok](../storage/storage-create-storage-account.md)témakörben találhatók. Jegyezze fel a hívóbetű a tárterület-fiókjához, hogy a kulcs az Azure tároló Explorer eszközzel a fiókhoz való csatlakozáshoz.
- Az Azure tároló Explorer tool letölthető a [Microsoft Azure tároló Intézőből](http://storageexplorer.com/). Fogadja el az alapértelmezett telepítése során.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure tároló Intézővel 

A dokumentum feltöltése és letöltése Azure tároló Intézővel adatok az alábbi lépéseket. 

1.  Indítsa el a Microsoft Azure tároló Explorer.
2.  Jelenítse meg a **Bejelentkezés a fiókjába a...** varázsló, jelölje be az **Azure-fiók beállításai** ikonra, majd a **fiók hozzáadása** , és írja be hitelesítő adatait. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Jelenítse meg a **Csatlakozás az Azure-tároló** varázsló, jelölje be a **Csatlakozás az Azure tároló** ikonra. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Adja meg a hívóbetű Azure tároló fiókjából az **Azure tároló csatlakozás** varázsló, és **Tovább**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Írja be a tárhely partner nevét a **fióknév** mezőbe, és válassza a **Tovább gombra**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Most már hozzáadott tárterület-fiók szerepelnie kell. Tárterület-fiókjában blob-tároló létrehozásához kattintson a jobb gombbal az adott fiókban a **Blob tárolók** csomópontot, jelölje ki a **Blob-tárolóhoz létrehozása**, és írjon be egy nevet.
7. A tároló feltölteni az adatokat, jelölje be a cél tároló, és kattintson a **Feltöltés** gombra.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Kattintson a mező jobb oldalán a **fájlok** **…** , jelöljön ki egy vagy több fájlt a fájlrendszerből feltöltését, és **Töltse fel** a fájlok feltöltése a kezdéshez kattintson.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Adatok, a megfelelő tároló töltheti le, és kattintson a **Letöltés**parancs kiválasztása a blob letöltése. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


