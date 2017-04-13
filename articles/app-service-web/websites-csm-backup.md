<properties
    pageTitle="Biztonsági mentése és visszaállítása az alkalmazás szolgáltatás alkalmazások többi használatával"
    description="Megtudhatja, hogy miként RESTful API-hívások biztonsági mentése és visszaállítása az Azure alkalmazás szolgáltatás alkalmazás használata"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Biztonsági mentése és visszaállítása az alkalmazás szolgáltatás alkalmazások többi használatával

> [AZURE.SELECTOR]
- [A PowerShell](../app-service/app-service-powershell-backup.md)
- [REST API-VAL](websites-csm-backup.md)

[Alkalmazás szolgáltatás alkalmazások](https://azure.microsoft.com/services/app-service/web/) tárolója Azure BLOB-szerint is biztonsági másolatot. A biztonsági másolat is tartalmazhat, az alkalmazás-adatbázisok. Ha az alkalmazás minden eddiginél véletlenül törölné, vagy ha az alkalmazás vissza kell állítani egy korábbi verzióját, akkor minden korábbi biztonsági mentés állíthatók vissza. Biztonsági másolatok igény bármikor el tud végezni, vagy biztonsági másolatok ütemezhető legyen a megfelelő időközönként.

Ez a cikk ismerteti, hogyan biztonsági mentése és visszaállítása az alkalmazás RESTful API kéréseivel. Ha létrehozása és kezelése az Azure portal segítségével grafikusan alkalmazás másolatok kívánt, olvassa el a [biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Első lépések
TÖBBI kérések elküldéséhez kell, hogy az alkalmazás **nevét**, **erőforráscsoport**és **Előfizetés azonosítója**. Ezek az információk találhatók az **Alkalmazás szolgáltatás** lap a [Azure portál](https://portal.azure.com)alkalmazását gombra kattintva. Ez a cikk példákat akkor állítja be a webhely **backuprestoreapiexamples.azurewebsites.net**. Ez az alapértelmezett-Web-WestUS erőforráscsoport tárolja, és fut, az azonosító 00001111-2222-3333-4444-555566667777 előfizetés.

![Példa webhely információk][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Biztonsági mentési és visszaállítási REST API-val
Néhány példa a REST API segítségével biztonsági mentési és visszaállítási alkalmazás most foglalkozunk lesz. Minden egyes példa tartalmazza a egy URL-CÍMEK és a HTTP összehívás törzsébe. A minta URL-cím kapcsos zárójelek, például {előfizetés azonosítója} csomagolt helyőrzők tartalmazza. A helyőrzők helyére a megfelelő adatokat, az alkalmazás. Hivatkozás az alábbiakban magyarázatot egyes URL-példákat megjelenő helyőrzőtípust.

* Előfizetés azonosítója – az Azure előfizetést tartalmazó az alkalmazás azonosítója
* erőforrás-csoportnevet – az erőforráscsoport tartalmazó az alkalmazás neve
* név – az alkalmazás neve
* biztonsági másolat azonosító – az alkalmazás biztonsági másolat azonosító

A teljes dokumentációt a API, beleértve a HTTP-összehívásban szereplő egyéb paramétereket az [Azure erőforrás Explorer](https://resources.azure.com/)című témakör tartalmaz.

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Biztonsági másolat igény alkalmazás
Egy **bejegyzés** kérelem küldése azonnal készítsen biztonsági másolatot az alkalmazás, **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Az alábbiakban az URL-cím néz ki a példát webhelyet használ. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Adjon meg egy JSON-objektum kérését tároló fiók tárolására szolgál a biztonsági másolat megadása törzsében. A JSON-objektum nevű **storageAccountUrl**, betöltő [Társítások URL-CÍMÉT](../storage/storage-dotnet-shared-access-signature-part-1.md) az Azure tároló tároló, amely a biztonsági másolat blob-írási hozzáférés engedélyezése tulajdonság kell rendelkeznie. Ha szeretne készítsen biztonsági másolatot az adatbázisról, meg kell adnia a neveket, -típusok és készülni az adatbázisok kapcsolati karakterláncot tartalmazó lista is.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Biztonsági másolatot az alkalmazás azonnal elkezdődik, amikor a kérelem érkezik. A biztonsági mentés hosszú időt vesz igénybe vehet. A HTTP-válasz használható Azonosítót tartalmaz egy másik összehívásban a biztonsági mentés állapotának megtekintéséhez. Íme egy példa a HTTP-válasz a biztonságimásolat-összehívás törzsében.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Hibaüzenetek jelennek meg a HTTP-válasz tulajdonságlapján a napló találhatók.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Az automatikus biztonsági mentés ütemezése
Nemcsak a biztonsági másolatot az alkalmazás igény, történjen az automatikus biztonsági is ütemezése.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Egy új biztonsági ütemezésű beállítása
Biztonsági ütemezett beállításához összehívásokkal **HELYEZI el** a **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Az alábbiakban az URL-cím néz ki a példa webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

A kérelem szervezet egy JSON-objektum, amely meghatározza a biztonsági másolat konfiguráció kell rendelkeznie. Íme egy példa a szükséges paraméterekkel.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Ebben a példában az alkalmazás automatikusan mentésben hét naponta állítja be. A paraméterek **frequencyInterval** és **frequencyUnit** együtt határozza meg, milyen gyakran fordulhat elő, a biztonsági másolatok. **FrequencyUnit** érvényes értékei **órát** és **napot**. Például készítsen biztonsági másolatot az alkalmazás minden 12 óra, állítsa frequencyInterval 12 és frequencyUnit órára.

Régi biztonsági másolatok automatikusan törlődnek a tárterület-fiókból. Megadhatja, hogy hogyan régi a biztonsági másolatok lehet **retentionPeriodInDays** paraméterének beállításával. Ha azt szeretné, hogy mindig van mentve, függetlenül attól, hogy régi meg, értéke **keepAtLeastOneBackup** igaz, ha legalább egy biztonsági másolatot.

### <a name="get-the-automatic-backup-schedule"></a>Az Automatikus ütemezés beszerzése
Úgy juthat az alkalmazás a biztonsági mentés konfigurációja, az URL-cím **bejegyzés** -összehívás küldése **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Példa webhelyünkön URL-je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Biztonsági másolat állapotuk
Attól függően, hogy az alkalmazás mekkora biztonsági vehet igénybe. Biztonsági másolatok lehet is sikertelen, időkorlátja, vagy részlegesen sikerült. Az alkalmazás biztonsági másolatok állapotának megtekintéséhez az URL-cím **GET** -összehívás küldése **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Az URL-cím **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**GET kérésekben küld az adott biztonsági állapotának megtekintéséhez.

Az alábbiakban az URL-cím néz ki a példa webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

A válasz szervezet Ez a példa hasonló JSON objektumot tartalmaz.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Biztonsági másolat állapota sorszámozott típus. Az alábbiakban minden lehetséges állam.

* A 0 – esetbejegyzések: A biztonsági mentés elindult, de még nem fejeződött be.
* 1 – nem sikerült: A biztonsági mentés sikertelen volt.
* 2 – sikeres: A biztonsági mentés sikeres befejezését.
* 3 – időtúllépése: A biztonsági mentés nem fejeződött be az időt, és meg lett szakítva.
* 4 – létrehozott: A biztonsági másolat kérelem aszinkron van, de még nem kezdődött.
* 5 – kihagyott: A biztonsági mentés művelet nem folytathatja túl sok biztonsági másolatok el az ütemezett miatt.
* 6 – PartiallySucceeded: A biztonsági mentés sikerült, de egyes fájlok nem mentésben, mivel ezek nem tudja olvasni. Ez általában oka az, hogy-kizáró zárolása helyezte a fájlokat.
* 7 – DeleteInProgress: A biztonsági mentés kért törlődik, de még nem törölt.
* 8 – DeleteFailed: A biztonsági mentés nem lehet törölni. Ez akkor fordulhat elő, mert a biztonsági másolat létrehozásához használt Társítások URL-cím lejárt.
* 9 – a törölt: A biztonsági mentés törlése megtörtént.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Az alkalmazás visszaállítása biztonsági másolatból
Ha törli az alkalmazást, vagy vissza szeretné állítani az alkalmazás egy korábbi verzióját, visszaállíthatja az alkalmazás egy biztonsági másolatból. A visszaállítás meghívandó kérelmet küldhet egy **bejegyzés** az URL-cím **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Az alábbiakban az URL-cím néz ki a példa webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/restore**

Az összehívás törzsébe küldése egy JSON-objektum, amely tartalmazza a visszaállítási művelet tulajdonságait. Íme egy példa az összes szükséges tulajdonságokat tartalmazó:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Visszaállíthatja az új alkalmazás
Időnként előfordulhat, hogy kívánt új alkalmazás létrehozása, amikor visszaállít egy már meglévő alkalmazás felülírása helyett a biztonsági másolatot. Ehhez módosítása a kérelem URL-cím, mutasson az új alkalmazás létre szeretne hozni, és módosítsa a **felülírja a** tulajdonságot a JSON **hamis**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Törlés-alkalmazás biztonsági mentése
Ha szeretné törölni a biztonsági másolatot, egy **törlése** kérelmet küldhet az URL-cím **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Az alábbiakban az URL-cím néz ki a példa webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>A biztonsági másolatot Társítások URL-cím kezelése
Azure alkalmazás szolgáltatás megkísérli a biztonsági másolat törlése a kapott a biztonsági mentés létrehozásakor Társítások URL-címmel Azure-tárhelyről. Társítások URL-címe már nem érvényes, ha a biztonsági mentés keresztül a REST API-t nem lehet törölni. Azonban frissítheti a Társítások biztonsági társított **POST** kérelem küld az URL-cím URL-cím **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Az alábbiakban az URL-cím néz ki a példa webhelyhez. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/List**

Az összehívás törzsébe küldése egy JSON-objektum, amely tartalmazza az új Társítások URL-címet. Lássunk egy példát.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Biztonsági okokból biztonsági tartozó Társítások URL-címe nem jelenik meg a biztonsági másolat GET kérésekben küldésekor. Megjelenítendő biztonsági tartozó Társítások URL-CÍMÉT, ha egy bejegyzés kérelmet küldhet az azonos URL-címet a fenti. Üres JSON objektum bele az összehívás törzsébe. A válasz, a kiszolgálóról összes a biztonsági másolat adatait, beleértve az Társítások URL-CÍMÉT tartalmazza.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
