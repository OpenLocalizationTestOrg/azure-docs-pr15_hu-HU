<properties
 pageTitle="Importálás és exportálás IoT központi eszköz identitások |} Microsoft Azure"
 description="Fogalmak és a .NET-kód kódrészletek IoT központi eszköz identitások tömeges kezeléséhez"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>Tömeges IoT központi eszköz identitás kezelése

Minden egyes IoT hubhoz egy eszköz identitás beállításjegyzék eszköz erőforrások létrehozása a szolgáltatásban, például egy várólista repülés cloud-eszköz üzenetek tartalmazó is használhatja. Az eszköz identitás beállításjegyzék azt is lehetővé teszi, hogy az eszköz elérésű végpontjait való hozzáférés szabályozása. Ez a cikk leírja, hogy miként importálhat és exportálhat eszköz identitások tömegesen, és onnan egy eszköz identitás beállításjegyzék.

Importálása és exportálása műveletek történik a *feladatok* , amelyek lehetővé teszik, hogy egy IoT központi szemben a tömeges szolgáltatás műveletek végrehajtása környezetében.

A **RegistryManager** osztály a **ExportDevicesAsync** és a **feladat** keretében használt **ImportDevicesAsync** módszerek tartalmaz. Ezeket a módszereket lehetővé teszi azok exportálása importálása és szinkronizálása egy IoT központi eszköz beállításjegyzék egészét.

## <a name="what-are-jobs"></a>Mik azok a feladatok?

Eszköz identitás beállításjegyzék műveletek a **feladat** rendszer használata esetén a műveletet:

*  Rendelkezik a potenciálisan végrehajtás sokáig szabványos futtatókörnyezet műveleteket, és összehasonlítása vagy
*  A felhasználó nagy mennyiségű adat lekérdezése

Ezekben az esetekben egyetlen API-hívás Várakozás vagy blokkolja a művelet, az eredmény helyett a művelet aszinkron létrehoz egy adott IoT elosztót **feladatot** . Kattintson a művelet közvetlenül egy **JobProperties** objektum adja eredményül.

A következő C# kódrészletet szemlélteti, hogyan lehet az Exportálás feladat létrehozásához:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

Kattintson a visszaadott **JobProperties** metaadatok használatáról a **feladat** állapota lekérdezni **RegistryManager** osztály is használhatja.

A következő C# kódrészletet öt másodpercenként lekérdezik tekintheti meg, ha a projekt végrehajtásakor befejeződött mutatja be:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Exportálás eszközök

A **ExportDevicesAsync** módszert a teljes egészében egy IoT központi eszköz beállításjegyzék exportálása egy [Tároló Azure](https://azure.microsoft.com/documentation/services/storage/) blob tároló [Megosztott Access-aláírás](https://msdn.microsoft.com/library/ee395415.aspx).

Ez a módszer hozhat létre megbízható biztonsági másolatait eszköz adatainak blob-tárolóban szabályozható.

A **ExportDevicesAsync** módszerhez két paraméterrel:

*  Egy *karakterlánc* , amely tartalmazza a blob-tároló URI. Ez a URI Társítások jogkivonat, amely írási hozzáféréssel a tároló hozzárendeli kell tartalmaznia. A feladat blokk blob a szerializált exportálás eszköz adatainak tárolására tároló hoz létre. A biztonsági jogkivonat tartalmaznia kell a következő engedélyeket:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  Olyan *logikai* , amely jelzi, hogy az adatok exportálása hitelesítési kulcsokat kizárása kívánja. Ha a **hamis**, az Exportálás szereplő kulcsok hitelesítési kibocsátás; egyéb esetben billentyűk exportálja **null**.

A következő C# kódrészletet szemlélteti, hogyan lehet az exportálási feladat, amely tartalmazza az adatok exportálása eszköz hitelesítési kulcsokat kezdeményezzen, és majd lekérdezik feltételei:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

A feladat a megadott blob-tárolóhoz az eredményt, a név **devices.txt**blokk blob-ként tárol. Kimeneti adatai soronként egy eszközzel szerializálásának JSON eszközadatok, áll.

A következő példa bemutatja a kimeneti adatai:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Ha ez a kód adatokhoz való hozzáférés van szüksége, egyszerűen a ezeket az adatokat a **ExportImportDevice** osztály deszerializálni meg. A következő C# kódrészletet mutatja be, hogy volt korábban exportált blokk blob eszköz az információkat olvashatja:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  A **GetDevicesAsync** módszer a **RegistryManager** osztály-ból az eszközök is használhatja. Ezt a megközelítést azonban 1000 a merevlemez vonalvég van eszköz olyan objektumok, amelyek a visszaadott számú. A várt használatieset **GetDevicesAsync** mód fejlesztési esetek szeretné hibakeresése támogatási és gyártási munkaterhelésekből nem ajánlott.

## <a name="import-devices"></a>Importálás eszközök

A **ImportDevicesAsync** módszer a **RegistryManager** osztály lehetővé teszi, hogy egy IoT központi eszköz beállításjegyzékben tömeges importálás és a szinkronizálási műveletek elvégzéséhez. A **ExportDevicesAsync** módszer, például a **ImportDevicesAsync** módszert használja a **feladat** keretében.

Gondoskodik a **ImportDevicesAsync** módszerrel, mert a mellett a eszköz identitás beállításjegyzékben új eszközök kiépítési, azt is frissítése és törlése a meglévő eszközök.

> [AZURE.WARNING]  Az importálási művelet nem vonható vissza. Mindig készítsen biztonsági másolatot az módszerrel **ExportDevicesAsync** egy másik blob-tárolóhoz módosítása előtt tanácsos lehet tömeges az eszköz identitás regisztrációs meglévő adatait.

A **ImportDevicesAsync** módszer két paraméterrel hajtja végre:

*  Egy *karakterlánc* , amely tartalmazza a *bemeneti* a feladat tartományként egy [Tároló Azure](https://azure.microsoft.com/documentation/services/storage/) blob-tároló URI. Ez a URI Társítások jogkivonat, amely a tároló olvasási hozzáférést biztosít a kell tartalmaznia. Ez a tároló, az eszköz identitás rendszerleíró adatbázisba történő importálásához szerializált eszköz adatokat tartalmazó neve **devices.txt** blob kell tartalmaznia. Az adatok importálása a **ExportImportDevice** feladat használ, amikor létrehoz egy **devices.txt** blob JSON formátumában eszközök adatainak kell tartalmaznia. A biztonsági jogkivonat tartalmaznia kell a következő engedélyeket:

    ```
    SharedAccessBlobPermissions.Read
    ```

*  *Karakterlánc* , amely tartalmaz egy [Tároló Azure](https://azure.microsoft.com/documentation/services/storage/) blob-tároló URI *kimeneti* a feladatot a fogja használni. A feladat blokk blob importálás **feladat**befejezése után bármilyen hiba adatainak tárolására tároló hoz létre. A biztonsági jogkivonat tartalmaznia kell a következő engedélyeket:
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  A két paraméterrel ugyanabban a blob-tárolóban is mutathat. A külön paraméterek egyszerűen engedélyezése további beállítási lehetőségekre az adatok, a kimeneti tároló további engedéllyel kell rendelkeznie.

A következő C# kódrészletet szemlélteti, hogyan lehet az importálás indítása:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Importálás viselkedése

A **ImportDevicesAsync** módszerrel is használhatja az alábbi tömeges műveletek elvégzését az eszköz identitás beállításjegyzékben:

-   Az új eszközök tömeges nyilvántartása
-   Meglévő eszközök tömeges törlése
-   Tömeges állapotváltozások (engedélyezése vagy letiltása a eszközök)
-   Tömeges hozzárendelését új eszköz hitelesítési kulcsok
-   Tömeges automatikus-újbóli eszköz hitelesítési kulcsok

Tetszőleges kombinációját egyetlen **ImportDevicesAsync** hívás belül az előző műveletek végrehajtása Például az új eszközök regisztrálni és törölni vagy meglévő eszközök frissítése egyszerre. A **ExportDevicesAsync** módszer együtt használva teljesen áttelepítheti minden eszközén egy IoT központból között.

Az egyes eszközök szerializálási adatok importálása a választható, **amelyben a importMode** tulajdonság használatával szabályozhatja az importálási folyamat per-eszköz. A **amelyben a importMode** a tulajdonságnak a következő beállításokat:

| amelyben a importMode |  Leírás |
| -------- | ----------- |
| **createOrUpdate** | Ha egy eszközt a megadott **azonosító**nem létezik, azt újonnan regisztrálva van. <br/>Már létezik az eszközt, ha meglévő adatokat a rendszer felülírja a megadott bemeneti adatok tekintet nélkül a **ETag** értéket. |
| **Hozzon létre** | Ha egy eszközt a megadott **azonosító**nem létezik, azt újonnan regisztrálva van. <br/>Ha már létezik az eszközt, a naplófájl írott hibát jelez. |
| **frissítés** | A megadott **azonosító**már létezik egy eszközt, ha meglévő adatokat a rendszer felülírja a megadott bemeneti adatok tekintet nélkül a **ETag** értéket. <br/>Ha az eszköz nem létezik, a naplófájl írott hibát jelez. |
| **updateIfMatchETag** | Ha a megadott **azonosító**már létezik egy eszközt, a meglévő adatokat a rendszer felülírja a megadott bemeneti adatok csak akkor, ha van egy **ETag** egyezés. <br/>Ha az eszköz nem létezik, a naplófájl írott hibát jelez. <br/>Ha egy **ETag** eltérés, a naplófájl írott hibát. |
| **createOrUpdateIfMatchETag** | Ha egy eszközt a megadott **azonosító**nem létezik, azt újonnan regisztrálva van. <br/>Ha már létezik az eszközt, a meglévő adatokat a rendszer felülírja a megadott bemeneti adatok csak akkor, ha van egy **ETag** egyezés. <br/>Ha egy **ETag** eltérés, a naplófájl írott hibát. |
| **törlése** | Ha a megadott **azonosító**már létezik egy eszközt, a program törli tekintet nélkül a **ETag** értéket. <br/>Ha az eszköz nem létezik, a naplófájl írott hibát jelez. |
| **deleteIfMatchETag** | Ha a megadott **azonosító**már létezik egy eszközt, csak akkor, ha van egy **ETag** egyezés törlődik. Ha az eszköz nem létezik, a naplófájl írott hibát jelez. <br/>Ha egy ETag eltérés, a naplófájl írott hibát. |

> [AZURE.NOTE] Ha a szerializációs adatokat nem kifejezetten határozza meg egy **amelyben a importMode** jelző eszköz, lesz az alapértelmezett **createOrUpdate** az importálási művelet során.

## <a name="import-devices-example--bulk-device-provisioning"></a>Példa eszközök importálása – tömegesen kiépítési eszköz 

A következő C# kód példa szemlélteti, hogyan kell készítése a több eszköz identitás, amely:

- Hitelesítési kulcsokat tartalmaznak.
- Az eszköz adatokat blokk blob írni.
- Az eszköz identitás beállításjegyzék eszközök importálhat.

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Eszközök példa – tömeges törlés importálása

A következő kódot minta megtudhatja, hogy hogyan használja az előző kód minta felvett eszközök törlése:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>A tároló Társítások URI első


A következő kódot minta megtudhatja, hogy miként olvasható, írási és törlése a blob-tároló engedélyeinek [URI Társítások](../storage/storage-dotnet-shared-access-signature-part-2.md) létrehozása:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben a eszköz identitás nyilvántartó ellen tömeges műveletek elvégzéséhez, IoT-központban megtanulta azt. Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure IoT központi kezelése:

- [Mértékek használata][lnk-metrics]
- [Műveletek figyelése][lnk-monitor]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md