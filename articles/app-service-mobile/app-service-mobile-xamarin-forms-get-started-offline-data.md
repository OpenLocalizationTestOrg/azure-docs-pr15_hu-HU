<properties
    pageTitle="Kapcsolat nélküli szinkronizálás engedélyezése az Azure mobilalkalmazás (Xamarin.Forms) |} Microsoft Azure"
    description="Alkalmazás szolgáltatás mobilalkalmazás használata a Xamarin.Forms alkalmazásban offline adatok gyorsítótár és szinkronizálása"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>A Xamarin.Forms mobilalkalmazásának offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>– Áttekintés
Ebben az oktatóanyagban Azure Mobile-alkalmazások a kapcsolat nélküli szinkronizálás szolgáltatása számára Xamarin.Forms vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi a felhasználóknak használata mobilalkalmazásban – megtekintése, hozzáadása vagy módosítása az adatok –, akkor is, ha nincs hálózati kapcsolat. Módosítások tárolódnak helyi adatbázist. Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli szolgáltatás.

Ebben az oktatóanyagban alapján a Xamarin.Forms quickstart útmutató megoldást hoz létre, amikor befejezte az oktatóprogram [egy Xamarin iOS-alkalmazás létrehozása] Mobile-appokról. A quickstart útmutató megoldás Xamarin.Forms támogatása a kapcsolat nélküli szinkronizálás, amely csak az engedélyezésük van szüksége a kódot tartalmazza. Ebben az oktatóanyagban frissítse a kapcsolat nélküli funkciók bekapcsolása a Azure Mobile-alkalmazások quickstart útmutató megoldás. Azt is kiemelése az alkalmazásban a kapcsolat nélküli-specifikus kódot. Ha nem használja a letöltött quickstart útmutató megoldást, hozzá kell adnia az adatokat az access bővítmény csomagok a projekthez. További információt a kiszolgáló bővítmény csomagok [használata]című rész tartalmaz a .NET kódmentes server Azure Mobile-appokról SDK[1].

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd: a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Kapcsolat nélküli szinkronizálás funkcióhoz a quickstart útmutató megoldásban

A kapcsolat nélküli szinkronizálás kódot is tartalmazni fogja a projekt C# előfeldolgozó irányelvek használatával. Ha a **OFFLINE\_SZINKRONIZÁLÁSI\_engedélyezve** szimbólum van megadva, az kód elérési utak összeállítása szerepelnek. A Windows-alkalmazásokat telepítenie kell a SQLite platform.

1. A Visual Studióban, kattintson a jobb gombbal a megoldást > **NuGet csomagok kezelése megoldás...**, majd keresse meg és a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet csomag az összes projekt esetében a megoldás telepítése.

2. A megoldás Explorer megnyitni a TodoItemManager.cs fájlt a projekt **hordozható** a nevében, amely hordozható osztálytár projekt, majd vegye ki a megjegyzésjeleket a következő előfeldolgozó irányelvet:

        #define OFFLINE_SYNC_ENABLED

4. (Nem kötelező) Támogatja a Windows-eszközöket, telepítse a következő SQLite futtatókörnyezet csomagokban egyikét:

    * **Windows 8.1 futtatókörnyezet:** Telepítse [a Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Telepítse [a Windows Phone 8.1 SQLite][4].
    * **Univerzális Windows platformra** Telepítse [az univerzális Windows univerzális az SQLite][5].

    Bár a quickstart útmutató univerzális Windows projekt nem tartalmaz, az univerzális Windows platformra Xamarin űrlapok támogatott.

5. (Nem kötelező) Minden egyes Windows alkalmazás projektben kattintson a jobb gombbal **hivatkozások** > **Hivatkozás hozzáadása**, bontsa ki a **Windows** -mappába > **bővítmények**.
    Engedélyezze a megfelelő **SQLite a Windows** SDK **Vizuális C++ 2013 futtatókörnyezet Windows** SDK együtt.
    A SQLite SDK neveket egyes Windows platformon kissé eltérő lehet.

## <a name="review-the-client-sync-code"></a>Tekintse át az ügyfél szinkronizálási kódot.

Az oktatóprogram kód belül már található rövid áttekintését az alábbiakban a `#if OFFLINE_SYNC_ENABLED` irányelvek. A kapcsolat nélküli szinkronizálás funkció a TodoItemManager.cs projektfájlba a hordozható osztálytár projekt. A szolgáltatás elvi áttekintése, [Az Azure Mobile-alkalmazások Offline adatszinkronizálás]című[2].

* Minden olyan tábla műveletek végrehajtása előtt inicializálni kell a helyi tárolóba. A helyi tárolóba adatbázist a **TodoItemManager** osztálykonstruktor van inicializálni, az alábbi kód használatával:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Ez a kód adatbázist hoz létre új helyi SQLite a **MobileServiceSQLiteStore** osztály használatával.

    A **DefineTable** módszer táblát hoz létre egy a helyi tárolóba, amely megfelel a megadott írja be a mezőket.  A típus nem található meg a távoli adatbázis szereplő összes oszlop felvenni. Oszlopok csak egy részhalmazát tárolásához lehetőség.

* A **TodoItemManager** **todoTable** mező helyett **IMobileServiceTable** **IMobileServiceSyncTable** típus nem. A class használ, az összes a helyi adatbázis létrehozása, olvasása, frissítése és törlése (CRUD) táblázat műveletek. Úgy dönt, amikor a változtatásokat a mobilalkalmazás kódmentes vannak tolódik **PushAsync** meghívásával a **IMobileServiceSyncContext**a. A szinkronizálási helyi segítségével megőrizheti a táblának megfelelő kapcsolatokat nyomon követése és terjesztése módosításokat egy ügyfél alkalmazás módosította **PushAsync** hívásakor táblázatokban.

    A következő **SyncAsync** módszer nevezzük a mobilalkalmazás kódmentes szinkronizálni:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Ez a példa az alapértelmezett szinkronizálási kezelő az egyszerű hibakezelési használja. A valós alkalmazások volna kezelheti a különböző hibákat, például a hálózati feltételek és kiszolgáló ütközések egy egyéni **IMobileServiceSyncHandler** végrehajtása.

## <a name="offline-sync-considerations"></a>Kapcsolat nélküli szinkronizálás előtt megfontolandó kérdések

A mintában a **SyncAsync** módszer csak neve indítási, és ha szükséges, a szinkronizálást a.  Az elemek listában; lehúzható a szinkronizálást, Android és IOS rendszerű alkalmazásban kezdeményezéséhez a Windows használja a **szinkronizálás** gombra. A valós életből alkalmazásban meg is elhelyezheti a szinkronizálási eseményindító a hálózati állapot megváltozásakor.

A leküldéses végrehajtásakor ellen függőben lévő táblázat helyi frissítések nyomon által megadott környezetben, húzás művelet automatikusan indítók előző helyi leküldéses. Ha frissítése, Hozzáadás és a következő példában az elemek befejezése, a közvetlen **PushAsync** hívás elhagyható.

A megadott kódot, az összes rekordot a távoli TodoItem táblázatban a rendszer megkérdezi, de a rekordok szűréséhez megkerülhetők egy lekérdezés azonosító és a lekérdezés **PushAsync**lehetőség arra is. További információ a részben *Növekményes szinkronizálási* [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások][2].

## <a name="run-the-client-app"></a>Futtassa az ügyfél alkalmazást

A kapcsolat nélküli szinkronizálás most már engedélyezni futtassa az ügyfélalkalmazás legalább egyszer minden platformon a helyi tárolóba adatbázis kitöltéséhez. Később hasonlóan egy offline faxolás, és a helyi tárolóba adatainak módosítása, a app kapcsolat nélküli módban.

## <a name="update-the-sync-behavior-of-the-client-app"></a>A szinkronizálási viselkedése az ügyfél-alkalmazás frissítése

Ebben a szakaszban módosítsa az ügyfél projekt hasonlóan egy offline forgatókönyv a kódmentes URL-címének érvénytelen alkalmazás használatával. Azt is megteheti kikapcsolhatja a hálózati kapcsolatok áthelyezésével, az eszköz "Repülőgép módra."  Hozzáadásakor vagy adatelemeket módosítása, ezek a változások a helyi tárolóba tartani, de nem szinkronizálja a kódmentes adatokat tároló addig, amíg helyreáll a kapcsolat.

1. A megoldás Intéző nyissa meg a Constants.cs projektfájl a **hordozható** projektből és értékének módosítása `ApplicationURL` mutasson az érvénytelen URL-címe:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. A **hordozható** projektből a TodoItemManager.cs fájlra, majd egy **tájékozódást segíti** az alap **kivétel** osztály hozzáadása a **SyncAsync** **próbálja... tényleges** tiltás. A **tényleges** blokk ír a kivételhiba üzenete a konzol, az alábbi képlettel történik:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Építse fel, és futtassa az ügyfél alkalmazást.  Adja hozzá az egyes új elemek. Figyelje meg, hogy a kivétel be van jelentkezve a konzol minden kísérlet során a fájlok szinkronizálása. Ezek az új elemek létezik csak a helyi tárolóba mindaddig, amíg a mobil kódmentes tolni is lehet. Az ügyfél alkalmazás úgy működik, mintha a kódmentes, támogatására, az összes létrehozni, olvassa el, frissítés, Törlés (CRUD) műveletek csatlakozik.

4. Bezárja az alkalmazást, és indítsa újra a ellenőrizze, hogy az új elemek létrehozott állandó vannak-e a helyi tárolóba.

5. (Nem kötelező) Visual Studio segítségével tekintheti meg, hogy az adatok, adatbázisszerű, kódmentes nem változott, hogy a Azure SQL-adatbázis táblát.

    A Visual Studióban nyissa meg a **Kiszolgáló Intézőt**. Nyissa meg az adatbázis **Azure**-ban->**SQL-adatbázisait**. Kattintson a jobb gombbal az adatbázist, és válassza a **Megnyitás az SQL Server-objektum Explorer**. Most már megkeresheti az SQL-adatbázis tábla és annak tartalmát.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Újracsatlakozás a mobil kódmentes az ügyfél-alkalmazás frissítése

Ebben a részben újra az alkalmazást a mobil kódmentes, az alkalmazás visszatérésének online állapotba alkalmazást. A frissítés kézmozdulat végrehajtásakor adatok szinkronizálva van a mobil kódmentes.

1. Nyissa meg újra Constants.cs. Javítsa ki a `applicationURL` mutasson a helyes URL-címet.

2. Újjáépítése, és futtassa az ügyfél alkalmazást. Az alkalmazás megkísérli indítását követően a mobilalkalmazásban kódmentes szinkronizálása. Győződjön meg arról, hogy a kivételek a hibakeresési konzolban bejelentkezve.

3. (Nem kötelező) Az SQL Server-objektum Explorer vagy a többi eszköz például Fiddler vagy [Postman]frissített adatok megtekintéséhez[6]. Figyelje meg, az adatok szinkronizálása adatbázisszerű, kódmentes és a helyi tárolóba között.

    Figyelje meg az adatok szinkronizálása az adatbázis és a helyi tárolóba között, és az alkalmazás forráshoz hozzáadott elemeket tartalmazza.

## <a name="additional-resources"></a>További források

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások][2]
* [Azure mobilalkalmazások .NET SDK útmutató][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md