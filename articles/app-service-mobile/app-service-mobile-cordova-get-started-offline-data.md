<properties
    pageTitle="Kapcsolat nélküli szinkronizálás engedélyezése az Azure mobilalkalmazás (Cordova) |} Microsoft Azure"
    description="Alkalmazás szolgáltatás mobilalkalmazás használata a Cordova alkalmazásban offline adatok gyorsítótár és szinkronizálása"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>A Cordova mobilalkalmazásának offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Ebben az oktatóanyagban Azure Mobile-alkalmazások a kapcsolat nélküli szinkronizálás szolgáltatása számára Cordova vezet be. Kapcsolat nélküli szinkronizálás lehetővé teszi, hogy a végfelhasználók vezérléséhez mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat. Változások tárolódnak helyi adatbázisban; Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli szolgáltatás.

Ebben az oktatóanyagban alapján a Cordova quickstart útmutató megoldást hoz létre, amikor befejezte az oktatóprogram [Apache Cordova gyors indítása]Mobile-appokról. Ebben az oktatóanyagban frissíti a quickstart útmutató megoldást offline Mobile-alkalmazások Azure-szolgáltatást. Azt is kiemeli az alkalmazásban a kapcsolat nélküli-specifikus kódot.

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további tudnivalókért lásd: a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások]. Az API felülettel, című cikkben olvashat a beépülő modul a [fontos] fájlban.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Kapcsolat nélküli szinkronizálás hozzáadása a quickstart útmutató megoldás

A kapcsolat nélküli szinkronizálás kódot hozzá kell adnia az alkalmazást. Kapcsolat nélküli szinkronizálás cordova-sqlite-tárhely a beépülő modul, amely kap amikor automatikusan bekerülnek az alkalmazást az Azure Mobile-alkalmazások beépülő modul szerepel a projekt szükséges. A quickstart útmutató projekt ezek a bővítmények egyikét tartalmazza.

1. A Visual Studio Solution Explorer nyissa meg a index.js, és a következő kódot cseréje

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    Ezzel a kóddal:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Ezután cserélje le a következő kódot:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Ezzel a kóddal:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Az előző kód bővítheti a helyi tárolóba inicializálni, valamint definiálhatók a helyi táblázat, amely megfelel az oszlopértékeket, használja az Azure-ban biztonsági célból. (, Nem kell minden oszlopértékek szerepeltetni kód.)

    Hívja fel a **getSyncContext**kap a szinkronizálási környezetben hivatkozni. A szinkronizálási helyi segítségével megőrizheti a táblának megfelelő kapcsolatokat nyomon követése és terjesztése módosításokat egy ügyfél alkalmazás módosította **leküldéses** hívásakor táblázatokban.

3. Frissítse az alkalmazás URL-címet a mobilalkalmazás alkalmazás URL-címe.

4. Ezután cseréje kód:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Ezzel a kóddal:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Az előző kód előkészíti a szinkronizálási környezetben, és getSyncTable (helyett getTable) hívásainak hivatkozni kell a helyi tábla első.

    A kódot használ, az összes a helyi adatbázis létrehozása, olvasása, frissítése és törlése (CRUD) táblázat műveletek.

    Ez a Példa egyszerű hibakezelési szinkronizálási ütközések hajt végre. A valós alkalmazások szeretné kezelni a különböző hibákat, például hálózati feltételek, a kiszolgáló ütközések és mások. Kód Példák című témakörben talál a [kapcsolat nélküli szinkronizálás minta].

5. Ezután adja hozzá a tényleges szinkronizálás végezze el ezt a funkciót.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Hogy mikor kell küldeni a módosításokat a mobilalkalmazás kódmentes hívja fel a **leküldéses** az ügyfél által használt **syncContext** objektum. Például hozzáadhatja **syncBackend** hívás gomb eseménykezelő az alkalmazásban, például egy új szinkronizálás gombra, vagy hozzáadhat szinkronizálni, valahányszor új elem hozzáadása a **addItemHandler** függvény a hívást.

##<a name="offline-sync-considerations"></a>Kapcsolat nélküli szinkronizálás előtt megfontolandó kérdések

A visszahívási függvény, jelentkezzen be az alkalmazás indításkor csak meghívni a a mintában **syncContext** **leküldéses** metódusát.  A valós életből alkalmazásban is teheti a szinkronizálási funkció, amelyen manuálisan vagy a hálózati állapot megváltozásakor.

Végrehajtásakor egy ki egy táblát, amely magában függőben lévő helyi frissítések nyomon szemben a környezet alapján, a művelet ki automatikusan elindítani előző helyi leküldéses. Frissítésekor, hozzáadása és ebben a példában szereplő elemek befejezése kihagyhatja a közvetlen **leküldéses** hívás, mivel lehet, hogy felesleges (első ellenőrizze a szolgáltatás állapota a [Fontos fájl] ).

A megadott kódot, az összes rekordot a távoli todoItem táblázatban a rendszer megkérdezi, de a rekordok szűréséhez megkerülhetők egy lekérdezés azonosító és a lekérdezés **leküldéses**lehetőség arra is. További információ című *Növekményes szinkronizálása* a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások].

## <a name="optional-disable-authentication"></a>(Nem kötelező) Hitelesítés letiltása

Ha még nem állította be a hitelesítés, és nem szeretné tesztelés offline szinkronizálás előtt hitelesítés beállítása, Megjegyzés meg a bejelentkezési visszahívás függvény, de a kód belül maradjon a visszahívási függvény uncommented.

A kód így néz ki, a bejelentkezési sorok megjegyzésekről után.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Most az alkalmazás szinkronizálja az Azure-fájlok az alkalmazást, ha bejelentkezés helyett futtatásakor.

## <a name="run-the-client-app"></a>Futtassa az ügyfél alkalmazást

A kapcsolat nélküli szinkronizálás most már engedélyezni most futtathatja az ügyfélalkalmazás legalább egyszer minden platformon a helyi tárolóba adatbázis kitöltéséhez. Később fogja hasonlóan egy offline faxolás, és a helyi tárolóba adatainak módosítása, a app kapcsolat nélküli módban.

## <a name="optional-test-the-sync-behavior"></a>(Nem kötelező) Tesztelje a szinkronizálási viselkedése

Ebben a szakaszban a az ügyfél projekt hasonlóan egy offline forgatókönyv a kódmentes az alkalmazás érvénytelen URL használatával módosítja. Hozzáadásakor vagy adatelemeket módosítása, ezek a változások fog tartani a helyi tárolóba, de nem szinkronizálja a kódmentes adatokat tároló addig, amíg helyreáll a kapcsolat.

1. A megoldás Explorer index.js projektfájl megnyitása, és módosítsa az alkalmazás URL-cím, mutasson az érvénytelen URL-cím, például a következőket:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. A index.html, frissítse a CSP `<meta>` az azonos érvénytelen URL-címmel elem.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Szerkesztés és futtassa az ügyfél-alkalmazást, és figyelje meg, hogy kivételt be van jelentkezve a konzol bejelentkezés után a fájlok szinkronizálása az alkalmazás alkalommal, amikor. Minden új elemek hozzáadása fog létezik csak a helyi tárolóba mindaddig, amíg a mobil kódmentes tolni is lehet. Az ügyfél alkalmazás úgy működik, mintha a kódmentes, támogatására, az összes létrehozni, olvassa el, frissítés, Törlés (CRUD) műveletek csatlakozik.

4. Bezárja az alkalmazást, és indítsa újra a ellenőrizze, hogy az új elemek létrehozott állandó vannak-e a helyi tárolóba.

5. (Nem kötelező) Visual Studio segítségével tekintheti meg, hogy az adatok, adatbázisszerű, kódmentes nem változott, hogy a Azure SQL-adatbázis táblát.

    A Visual Studióban nyissa meg a **Kiszolgáló Intézőt**. Nyissa meg az adatbázis **Azure**-ban->**SQL-adatbázisait**. Kattintson a jobb gombbal az adatbázist, és válassza a **Megnyitás az SQL Server-objektum Explorer**. Most már megkeresheti az SQL-adatbázis tábla és annak tartalmát.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Nem kötelező) A mobil kódmentes szeretne a újracsatlakozás tesztelése

Ebben a szakaszban a a mobil kódmentes, az alkalmazás visszatérésének online állapotba alkalmazást az alkalmazás újracsatlakozik. Bejelentkezés, amikor adatokat fog szinkronizálja a mobil kódmentes.

1. Nyissa meg újra a index.js, és javítsa ki az alkalmazás URL-cím, mutasson a helyes URL-címet.

2. Nyissa meg újra a index.html, és javítsa ki az alkalmazás URL-címet a CSP `<meta>` elemet.

3. Újjáépítése, és futtassa az ügyfél alkalmazást. Az alkalmazás kísérel meg szinkronizálni a mobilalkalmazásban kódmentes a bejelentkezés után. Győződjön meg arról, hogy a kivételek a hibakeresési konzolban bejelentkezve.

4. (Nem kötelező) Az SQL Server-objektum Explorer vagy a többi eszköz például Fiddler frissített adatok megtekintése. Figyelje meg, az adatok szinkronizálása, adatbázisszerű, kódmentes és a helyi tárolóba között.

    Figyelje meg az adatok szinkronizálása az adatbázis és a helyi tárolóba között, és az alkalmazás forráshoz hozzáadott elemeket tartalmazza.

## <a name="additional-resources"></a>További források

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]

* [Fedőlap cloud: Azure mobil szolgáltatások a kapcsolat nélküli szinkronizálás] \(Megjegyzés: a videó a mobil szolgáltatások, de kapcsolat nélküli szinkronizálás az Azure Mobile-alkalmazások hasonló módon működik\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Következő lépések

* Tekintse meg speciális kapcsolat nélküli szinkronizálás szolgáltatások, például a [kapcsolat nélküli szinkronizálás minta] ütközések feloldása
* A kapcsolat nélküli szinkronizálás API hivatkozás a [Fontos fájl] megtekintése

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Rövid útmutató az Apache Cordova]: app-service-mobile-cordova-get-started.md
[FONTOS FÁJL]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[kapcsolat nélküli szinkronizálás minta]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md
[Felhőalapú fedőlap: Kapcsolat nélküli szinkronizálás az Azure mobil szolgáltatások]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
