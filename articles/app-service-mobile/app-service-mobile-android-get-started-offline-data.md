<properties
    pageTitle="Kapcsolat nélküli szinkronizálás engedélyezése az Azure mobilalkalmazás (Android)"
    description="Alkalmazás szolgáltatás Mobile-alkalmazások használata az Android alkalmazásban offline adatok gyorsítótár és szinkronizálása"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Az Android mobilalkalmazásának offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban a kapcsolat nélküli szinkronizálás funkció az Azure Mobile alkalmazások Android-alapú terjed ki. Kapcsolat nélküli szinkronizálás lehetővé teszi a felhasználóknak használata mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat. Módosítások tárolódnak helyi adatbázist. Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli kódmentes.

Ha az első használatának Azure Mobile alkalmazással, először töltse ki az oktatóprogram [-Android-alkalmazás létrehozása]. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, hozzá kell adnia az adatokat az access bővítmény csomagok a projekthez. További információt a kiszolgáló bővítmény csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további információért olvassa el a a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.

## <a name="update-the-app-to-support-offline-sync"></a>A kapcsolat nélküli szinkronizálás támogatási az alkalmazás frissítése

A kapcsolat nélküli szinkronizálás olvassa be, és írjon egy *szinkronizálási táblázat* (a felhasználói felületén *IMobileServiceSyncTable* ), amelynek eszközön **SQLite** adatbázis része.

Leküldéses, és húzza a módosításokat, az eszköz és Azure Mobile szolgáltatások között, a *szinkronizálási környezetet* (*MobileServiceClient.SyncContext*), amely inicializálni a helyi adatok tárolására helyi adatbázis-kezelő használja.

1. A `TodoActivity.java`, a meglévő meghatározását, Megjegyzés `mToDoTable` , és vegye ki a megjegyzésjeleket a szinkronizálási tábla verziója:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. Az a `onCreate` módszer, Megjegyzés ki a meglévő inicializálni `mToDoTable` , és vegye ki a megjegyzésjeleket a meghatározás:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. A `refreshItemsFromTable` definícióját, Megjegyzés `results` , és vegye ki a megjegyzésjeleket a meghatározása:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Megjegyzés definícióját, `refreshItemsFromMobileServiceTable`.

5. Vegye ki a megjegyzésjeleket definícióját `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Vegye ki a megjegyzésjeleket definícióját `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Az alkalmazás tesztelése

Ebben a szakaszban a vezérlő viselkedése, WiFi tesztelje a, és kapcsolja ki a WiFi-kapcsolat nélküli eset létrehozása.

Adatok elemek hozzáadásakor azokat a helyi SQLite tároló tartani, de nem szinkronizálja a mobilszolgáltatás mindaddig, amíg a **frissítés** gombra. Előfordulhat, hogy más alkalmazások eltérő követelményeik kapcsolatban, amikor adatokat kell a rendszer szinkronizálja, de a bemutató céljára ebben az oktatóanyagban van a felhasználónak kifejezetten kérhetik azt.

A gomb megnyomásakor háttér az új feladat elindul. Először a helyi tárolóba szinkronizálási környezetet, majd a helyi tábla minden megváltozott illesztéssel Azure adatok használata az összes módosításai verembe küldi.

### <a name="offline-testing"></a>Kapcsolat nélküli tesztelése

1. Helyezze az eszköz vagy a simulatort használja *Repülőgép*módban. Ezzel létrehoz egy offline forgatókönyv.

2. Néhány *Teendők* elem hozzáadása, illetve az egyes elemeket teljes jelölheti. Lépjen ki az eszközt, vagy simulatort használja (vagy az alkalmazás kényszerített bezárása), majd indítsa újra. Ellenőrizze, hogy a módosítások van már állandó az eszközön, mert tárolják őket a helyi SQLite áruházban.

3. Az SQL dolgozhat eszközzel, például *SQL Server Management Studio*és a többi ügyfél, például *Fiddler* vagy *Postman*Azure *TodoItem* táblázat tartalmának megtekintéséhez. Ellenőrizze, hogy az új elemek a kiszolgáló _nem_ szinkronizált

    + Egy Node.js kódmentes válassza az [Azure-portálra](https://portal.azure.com/), és a Mobile alkalmazásban kódmentes kattintva **Egyszerűen táblázatokat** > **TodoItem** tartalmának megtekintéséhez a `TodoItem` táblázat.
    + A .NET kódmentes olvassa el a táblázatok tartalmának a SQL-például *SQL Server Management Studio*eszközben, vagy a többi ügyfél, például *Fiddler* vagy *Postman*.

4. Kapcsolja be a WiFi az eszköz vagy a simulatort használja. Ezután kattintson a **frissítés** gombra.

5. Az TodoItem adatok ismét megtekintése az Azure-portálon. Az új és megváltozott TodoItems jelenjenek meg.

## <a name="additional-resources"></a>További források

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]

* [Fedőlap cloud: Azure mobil szolgáltatások a kapcsolat nélküli szinkronizálás] \(Megjegyzés: a videó a mobil szolgáltatások, de kapcsolat nélküli szinkronizálás az Azure Mobile-alkalmazások hasonló módon működik\)


<!-- URLs. -->

[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md

[Az Android-alkalmazás létrehozása]: app-service-mobile-android-get-started.md

[Felhőalapú fedőlap: Kapcsolat nélküli szinkronizálás az Azure mobil szolgáltatások]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

