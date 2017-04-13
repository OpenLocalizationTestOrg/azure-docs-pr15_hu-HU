<properties
    pageTitle="Kapcsolat nélküli szinkronizálás engedélyezése az Azure mobilalkalmazás (Xamarin Android)"
    description="Megtudhatja, hogy miként alkalmazás szolgáltatás mobilalkalmazás használatával az Xamarin Android alkalmazásban offline gyorsítótár és szinkronizálási adatok"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>A Xamarin.Android mobilalkalmazásának offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban Azure Mobile-alkalmazások a kapcsolat nélküli szinkronizálás szolgáltatása számára Xamarin.Android vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi a felhasználóknak használata mobilalkalmazásban – megtekintése, hozzáadása vagy módosítása az adatok –, akkor is, ha nincs hálózati kapcsolat. Módosítások tárolódnak helyi adatbázist.
Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli szolgáltatás.

Ebben az oktatóprogramban [egy Xamarin Android-alkalmazás létrehozása] oktatóanyag Azure Mobile-alkalmazások offline funkciók támogatása a ügyfél projekt frissítése. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, hozzá kell adnia az adatokat az access bővítmény csomagok a projekthez. További információt a kiszolgáló bővítmény csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd: a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások].

## <a name="update-the-client-app-to-support-offline-features"></a>Kapcsolat nélküli szolgáltatásait támogató az ügyfél-alkalmazás frissítése

Azure mobilalkalmazás offline szolgáltatások lehetőséget biztosítanak kommunikálni a helyi adatbázis-kapcsolat nélküli forgatókönyv közben. Ezek a funkciók használni az alkalmazást, inicializálni egy [SyncContext] egy helyi tárolóba. A táblázat a [IMobileServiceSyncTable], [IMobileServiceSyncTable] felületen majd hivatkozást. A helyi tárolóba az eszközön SQLite használják.

1. A Visual Studióban nyissa meg azt a projektet, amely az [egy Xamarin Android-alkalmazás létrehozása] oktatóprogram elvégezte NuGet csomag szolgáltatásának.  Keresse meg és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet csomagot.

2. Nyissa meg a ToDoActivity.cs fájlt, és vegye ki a megjegyzésjeleket a `#define OFFLINE_SYNC_ENABLED` definition.

3. A Visual Studióban nyomja le az **F5** billentyűvel újjáépítése, és futtassa az ügyfél-alkalmazást. Az alkalmazás ugyanúgy működik, mint ha engedélyezte a kapcsolat nélküli szinkronizálás előtt is. A helyi adatbázis azonban most töltve adatokkal egy offline esetben használható.

## <a name="update-sync"></a>A háttér-kiszolgálói kapcsolat bontása az alkalmazás frissítése

Ebben a részben megszakítja a kapcsolatot a mobilalkalmazás kódmentes hasonlóan egy offline helyzet szeretne. Adatok elemek hozzáadásakor a kivétel kezelő tájékoztat, hogy az alkalmazás a kapcsolat nélküli üzemmódban van-e. Új elemek ezt az állapotot, a helyi tárolóba adott hozzá, és a rendszer szinkronizálja a mobilalkalmazásban kódmentes, amikor csatlakoztatott állapotban végrehajtása leküldéses.

1. A megosztott projekt ToDoActivity.cs szerkesztése A **applicationURL** mutasson az érvénytelen URL-címének módosítása:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Kapcsolat nélküli viselkedés igazolni letiltása, WiFi-csatlakozás és a mobil hálózatok az eszközre, és repülőgép üzemmód használata.

2. Nyomja le az **F5** létre, és futtassa az alkalmazást. Figyelje meg a szinkronizálás a frissítés sikertelen, amikor elindítja az alkalmazást.

3. Írja be az új elemet, és figyelje meg, hogy leküldéses sikertelen, [CancelledByNetworkError] állapotban minden alkalommal, amikor, kattintson a **Mentés**gombra. Jó helyen jár, az új teendő elemek találhatók a helyi tárolóba mindaddig, amíg a mobilalkalmazásban kódmentes tolni is lehet.  Egy gyártási alkalmazásban Ha ezek a kivételek megjegyzéscímkék az ügyfél alkalmazás úgy működik, mintha továbbra is csatlakoztatva van a mobilalkalmazásban kódmentes.

4. Bezárja az alkalmazást, és indítsa újra a ellenőrizze, hogy az új elemek létrehozott állandó vannak-e a helyi tárolóba.

5. (Nem kötelező) A Visual Studióban nyissa meg a **Kiszolgáló Intézőt**. Nyissa meg az adatbázis **Azure**-ban->**SQL-adatbázisait**. Kattintson a jobb gombbal az adatbázist, és válassza a **Megnyitás az SQL Server-objektum Explorer**. Most már megkeresheti az SQL-adatbázis tábla és annak tartalmát. Győződjön meg arról, hogy az adatok, adatbázisszerű, kódmentes nem változott.

6. (Nem kötelező) A mobil kódmentes, a űrlap beolvasása lekérdezéssel lekérdezés például Fiddler vagy Postman többi eszköz segítségével `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Újracsatlakozás a mobilalkalmazás kódmentes az alkalmazás frissítése

Ebben a részben újra az alkalmazást a mobilalkalmazásban kódmentes. Az alkalmazás első futtatásakor a `OnCreate` eseménykezelő-hívások `OnRefreshItemsSelected`. Ez a módszer felhívja `SyncAsync` szinkronizálni, adatbázisszerű, kódmentes és a helyi tárolóba.

1. Nyissa meg a megosztott projekt ToDoActivity.cs, és a módosítás **applicationURL** tulajdonság visszaállítása.

2. Nyomja le az **F5** billentyűvel újjáépítése, és futtassa az alkalmazást. Az alkalmazás és a helyi módosítások szinkronizál a az Azure mobilalkalmazás kódmentes leküldéses és ki műveletek használata esetén a `OnRefreshItemsSelected` módszer hajt végre.

3. (Nem kötelező) Az SQL Server-objektum Explorer vagy a többi eszköz például Fiddler frissített adatok megtekintése. Értesítés az adatok szinkronizálása Azure mobilalkalmazás, adatbázisszerű, kódmentes és a helyi tárolóba között.

4. Kattintson az alkalmazást, néhány dolog, hajtsa végre ezeket a helyi tárolóba mellett található jelölőnégyzetet.

  `CheckItem`hívások `SyncAsync` mobilalkalmazás-fájlok szinkronizálása minden kész elemre. `SyncAsync`leküldéses és a leküldéses hívások. **Amikor egy ki egy olyan táblát, az ügyfél módosította, szemben végrehajtása leküldéses mindig automatikusan végrehajtása**. Ezzel biztosíthatja, hogy a helyi tárolóba együtt kapcsolatok szereplő összes táblázat továbbra is egységes. Ez a jelenség egy váratlan leküldéses járhat. További információt a problémáról olvassa el a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.

## <a name="review-the-client-sync-code"></a>Tekintse át az ügyfél szinkronizálási kódot.

A Xamarin ügyfél a projekt, letöltötte, ha már végzett az oktatóprogram [Xamarin Android-alkalmazás létrehozása] a kapcsolat nélküli szinkronizálás használata a helyi adatbázis SQLite támogató kódot tartalmaz. Az alábbiakban rövid áttekintését az oktatóprogram c ode már található. A szolgáltatás elvi áttekintése olvassa el a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.

* Minden olyan tábla műveletek végrehajtása előtt inicializálni kell a helyi tárolóba. A helyi tárolóba adatbázist inicializálni, akkor ha `ToDoActivity.OnCreate()` végrehajtja a `ToDoActivity.InitLocalStoreAsync()`. Ezzel a módszerrel hoz létre egy helyi SQLite adatbázis használja a `MobileServiceSQLiteStore` az Azure Mobile-alkalmazások ügyfél SDK által biztosított osztály.

    A `DefineTable` módszer táblát hoz létre egy a helyi tárolóba, amely megfelel a megadott típusú mezőinek `ToDoItem` ebben az esetben. A típus nem található meg a távoli adatbázis szereplő összes oszlop felvenni. Oszlopok csak egy részhalmazát tárolásához lehetőség.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* A `toDoTable` tagja `ToDoActivity` áll a `IMobileServiceSyncTable` írja be a helyett `IMobileServiceTable`. A IMobileServiceSyncTable arra utasítja az összes létrehozása, olvasása, frissítése és törlése (CRUD) táblázat műveletek a helyi tárolóba adatbázishoz.

    Úgy dönt, hogy mikor módosításokat az Azure mobilalkalmazás kódmentes meghívásával tolódik vannak `IMobileServiceSyncContext.PushAsync()`. A szinkronizálási helyi segítségével megőrizheti a táblának megfelelő kapcsolatokat nyomon követése és terjesztése módosításokat egy ügyfél alkalmazás módosította, ha az összes táblában lévő `PushAsync` nevezik.

    A megadott kódot hívások `ToDoActivity.SyncAsync()` való szinkronizálás a todoitem lista frissítése vagy egy todoitem ad hozzá vagy befejezett. A kód szinkronizálja minden helyi módosítás után.

    A megadott kódot, az összes a távoli rekordok `TodoItem` táblázat a rendszer megkérdezi, de a rekordok szűrése a lekérdezés azonosító megkerülhetők, és hogy a lekérdezés lehetőség arra is `PushAsync`. További információ című *Növekményes szinkronizálása* a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>További források

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]
* [Azure mobilalkalmazások .NET SDK útmutató][8]

<!-- URLs. -->
[Xamarin Android-alkalmazás létrehozása]: ../app-service-mobile-xamarin-android-get-started.md
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android-alkalmazás létrehozása]: app-service-mobile-xamarin-android-get-started.md
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
