### <a name="prerequisites"></a>Előfeltételek
- Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre
- [Azure Blob-tárolóhoz fiókhoz](../articles/storage/storage-create-storage-account.md) többek között a tárterület-fiók nevét, és a hívóbetű. Ezt az információt a tárterület-fiókot az Azure-portálon tulajdonságainak szerepel. Olvasson bővebben az [Azure-tárterületet](../articles/storage/storage-introduction.md).

Az Azure Blob-tárolóhoz fiókkal összefüggés-alkalmazásban, mielőtt csatlakozni Azure Blob-tárolóhoz. Ezt megteheti egyszerűen a logika alkalmazásból, az Azure portálon.  

Az Azure Blob-tárolóhoz használatával csatlakozhat fiókjához az alábbi lépéseket:  

1. Hozzon létre egy logikai alkalmazást. A logikai alkalmazások tervezőben hozzáadása az eseményindító, és adja hozzá az művelet. A legördülő listában válassza a **Microsoft megjelenítése felügyelt API-hoz** , és írja be a keresőmezőbe a "blob". A műveletek közül választhat:  

    ![Kapcsolat létrehozása a lépést Azure Blob-tárolóhoz](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Ha még nem korábban létrehozott minden Azure tárterület-kapcsolatot, megjelenik egy kérdés a kapcsolat adatai:   

    ![Kapcsolat létrehozása a lépést Azure Blob-tárolóhoz](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Adja meg a tárhely fiók adatait. A tulajdonságok egy csillag szükség.

    | A tulajdonság | Részletek |
|---|---|
| A kapcsolat neve * | Írja be a kapcsolatot egy tetszőleges nevet. |
| Azure tároló fióknév * | Írja be a tárterület-fiók nevére. A tároló fióknak a nevét az Azure-portálon tároló tulajdonságok jelenik meg. |
| Azure tároló Access Fiókkulcs * | Írja be a tárhely fiókkulcs. A hívóbetűk az Azure-portálon tároló tulajdonságok jelennek meg. |

    Ezek a hitelesítő adatok használatával csatlakozhat, és az adatok eléréséhez az logikájának alkalmazást engedélyezni. 

4. Jelölje ki a **létrehozása**.

5. Értesítés a kapcsolat létrejött. Ezután folytassa a lépéseket a logikájának alkalmazásban: 

    ![Kapcsolat létrehozása a lépést Azure Blob-tárolóhoz](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
