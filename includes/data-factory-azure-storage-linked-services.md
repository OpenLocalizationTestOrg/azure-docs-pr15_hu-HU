## <a name="azure-storage-linked-service"></a>Tárterület Azure szolgáltatás csatolva

**Azure tároló csatolt szolgáltatás** lehetővé teszi az Azure adatok gyári a **fiókkulcs**Azure tároló ügyfél csatolása. Ez az Azure tároló globális hozzáféréssel rendelkező biztosít az adatok gyári. Az alábbi táblázat ismerteti a JSON elemeit adott Azure csatolt tárhelyszolgáltatáshoz leírását.

| A tulajdonság | Leírás | Szükséges |
| :-------- | :----------- | :-------- |
| típus | Meg kell a típusa tulajdonság: **AzureStorage** | igen |
| connectionString | Adja meg a connectionString tulajdonság Azure tárterület való csatlakozáshoz szükséges információkat. | igen |

A következő témakör, a lépések megtekintése és másolása a fiókkulcs az Azure tárolására: [nézet, a másolás, és a hívóbetűk újragenerálása tárterület](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Példa:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure tárolási társítások csatolt szolgáltatás  
Aláírás (Társítások) átengedése a tárterület-fiókjában erőforrások delegált hozzáférést biztosít. Ez azt jelenti, hogy adhat az anélkül, hogy a fiók hívóbetűk megosztása egy ügyfél korlátozott az időt és a megadott engedélyek tartalmazó adott időszakra vonatkozóan a tárterület-fiókjában objektumok engedélyeit. A Társítások, amely magában foglalja a lekérdezés paraméterei URI egy tároló erőforráshoz való hozzáférés az összes szükséges információ hitelesített. Férni a Társítások a tároló erőforrásokat, az ügyfél csak a megfelelő konstruktor vagy módszer a Társítások átadni van szüksége. Talál részletes információt a Társítások [a megosztott Access aláírások: ismertetése a Társítások modell](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
A csatolt Azure tárolási Társítások szolgáltatás Azure tárolási ügyfél csatolása az Azure adatok gyári megosztott Access aláírás (Társítások) használatával teszi lehetővé. Az adatok gyári nyújtó minden/adott erőforrások (blob/tároló) az tárolója korlátozott/idő-kötött elérését. Az alábbi táblázat ismerteti a JSON elemek Azure tároló Társítások csatolt szolgáltatásra leírását. 

| A tulajdonság | Leírás | Szükséges |
| :-------- | :----------- | :-------- |
| típus | Meg kell a típusa tulajdonság: **AzureStorageSas**  | igen |
| sasUri | Adja meg a megosztott Access aláírás URI például blob, tároló vagy táblázat Azure tároló forrásokat. Lásd: további információt az alábbi megjegyzésekben. | igen | 


**Példa:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Egy **Társítások URI**létrehozásakor figyelembe a következőket:  

- Azure Data Factory csak **Szolgáltatás Társítások**nem fiók Társítások támogatja. Lásd: [Típusa a megosztott Access aláírások](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) részletes tájékoztatást a következő két típusa.
- Megfelelő olvasási/írási **engedélyekkel** kell hogyan (olvasható, írási, olvasási/írási) a csatolt szolgáltatást fogja használni a adatok gyári alapján objektumokon állítható be.
- **Lejárati idő** kell megfelelően be kell állítani. Győződjön meg arról, hogy az access Azure tárolási objektumok nem jár le a folyamat aktív időszakon belül.
- URI jobb tároló/blob vagy táblázat szinten van szükség alapján létre kell hozni. Az Azure blob-való Társítások Uri lehetővé teszi, hogy az adatok gyári szolgáltatás, hogy a blob eléréséhez. Társítások Uri-Azure blob-tárolóhoz eszközébe BLOB Végigléptetés az adatok gyári szolgáltatás lehetővé teszi. Ha kell hozzáférést biztosít több program vagy annál kevesebb objektumok később, frissítése és a Társítások URI, ne felejtse el az új URI frissítse a csatolt szolgáltatás.   
