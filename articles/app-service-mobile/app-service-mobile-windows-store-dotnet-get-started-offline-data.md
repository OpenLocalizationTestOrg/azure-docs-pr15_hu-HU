<properties
    pageTitle="Engedélyezze a kapcsolat nélküli szinkronizálás az univerzális Windows platformon (UWP) számára a Mobile-alkalmazások |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként gyorsítótár és szinkronizálási kapcsolat nélküli adatok Azure Mobile alkalmazás használata az univerzális Windows platformon (UWP) alkalmazást."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>A Windows-alkalmazás offline szinkronizálás engedélyezése

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy hogyan az offline munka támogatása hozzáadása egy univerzális Windows platformon (UWP) alkalmazást az Azure mobilalkalmazás kódmentes. Kapcsolat nélküli szinkronizálás lehetővé teszi a felhasználóknak használata mobilalkalmazásban – megtekintése, hozzáadása vagy módosítása az adatok –, akkor is, ha nincs hálózati kapcsolat. Módosítások tárolódnak helyi adatbázist. Ha az eszköz vissza online állapotban, ezek a változások a rendszer szinkronizálja a távoli kódmentes.

Ebben az oktatóanyagban frissítse a UWP alkalmazás projekt Azure Mobile-alkalmazások offline funkciók támogatása [a Windows-alkalmazás létrehozása] oktatóanyag. Ha nem használja a letöltött rövid útmutató az első kiszolgáló projekt, hozzá kell adnia az adatokat az access bővítmény csomagok a projekthez. További információt a kiszolgáló bővítmény csomagok [a .NET kódmentes server Azure Mobile-appokról SDK használata](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)című rész tartalmaz.

A kapcsolat nélküli szinkronizálás szolgáltatással kapcsolatos további információért olvassa el a a témakör [Offline adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.

## <a name="requirements"></a>Követelmények

Ebben az oktatóanyagban igényel, a következő előtti követelményei:

* Visual Studio 2013 Windows 8.1 vagy újabb.
* [A Windows-alkalmazás létrehozása][a windows-alkalmazás létrehozása]befejezését.
* [Azure Mobile Services SQLite áruház][sqlite store nuget]
* [Univerzális Windows platformra fejlesztési SQLite](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Kapcsolat nélküli szolgáltatásait támogató az ügyfél-alkalmazás frissítése

Azure mobilalkalmazás offline szolgáltatások lehetőséget biztosítanak kommunikálni a helyi adatbázis-kapcsolat nélküli forgatókönyv közben. Ezek a funkciók használni az alkalmazást, a [SyncContext] inicializálni[ synccontext] egy helyi tárolóba. A táblázat a [IMobileServiceSyncTable][IMobileServiceSyncTable] felületen majd hivatkozást. A helyi tárolóba az eszközön SQLite használják.

1. Telepítse [a Windows univerzális platform SQLite futtatókörnyezet](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. A Visual Studióban nyissa meg a NuGet csomag kezelő, amely az [egy Windows-alkalmazás létrehozása] oktatóprogram elvégezte a UWP alkalmazás projekthez.
    Keresse meg és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet csomagot.

4. A megoldás Intézőben kattintson a jobb gombbal **hivatkozások** > **Hivatkozás hozzáadása**  >  **Univerzális Windows** > **bővítmények**, majd engedélyezze a **univerzális Windows platformra SQLite** és **Univerzális Windows platformra alkalmazások vizuális C++ 2015 Runtime**is.

    ![SQLite UWP hivatkozás hozzáadása][1]

5. Nyissa meg a MainPage.xaml.cs fájlt, és vegye ki a megjegyzésjeleket a `#define OFFLINE_SYNC_ENABLED` definition.

6. A Visual Studióban nyomja le az **F5** billentyűvel újjáépítése, és futtassa az ügyfél alkalmazást. Az alkalmazás ugyanúgy működik, mint ha engedélyezte a kapcsolat nélküli szinkronizálás előtt is. A helyi adatbázis azonban most töltve adatokkal egy offline esetben használható.

## <a name="update-sync"></a>A háttér-kiszolgálói kapcsolat bontása az alkalmazás frissítése

Ebben a részben megszakítja a kapcsolatot a mobilalkalmazás kódmentes hasonlóan egy offline helyzet szeretne. Adatok elemek hozzáadásakor a kivétel eseménykezelő Ez az, hogy az alkalmazás a kapcsolat nélküli üzemmódban van-e. Új elemek ezt az állapotot, a helyi tárolóba adott hozzá, és alkalmazás szinkronizálja a mobilalkalmazásban kódmentes futtatásakor leküldéses következő csatlakoztatott állapotban.

1. A megosztott projekt App.xaml.cs szerkesztése Megjegyzések hozzáfűzése a inicializálni a **MobileServiceClient** ki, és adja meg a következő parancsot, és használja az érvénytelen mobilalkalmazás URL:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Kapcsolat nélküli viselkedés bemutatják, WiFi-csatlakozás és a mobil hálózatok az eszközön letiltásával vagy repülőgép üzemmód használata is.

2. Nyomja le az **F5** létre, és futtassa az alkalmazást. Figyelje meg a szinkronizálás a frissítés sikertelen, amikor elindítja az alkalmazást.

3. Írja be az új elemet, és figyelje meg, hogy leküldéses nem sikerül [CancelledByNetworkError] állapotban minden alkalommal, amikor, kattintson a **Mentés**gombra. Jó helyen jár, az új teendő elemek találhatók a helyi tárolóba mindaddig, amíg a mobilalkalmazásban kódmentes tolni is lehet.  Egy gyártási alkalmazásban Ha ezek a kivételek megjegyzéscímkék az ügyfél alkalmazás úgy működik, mintha továbbra is csatlakoztatva van a mobilalkalmazásban kódmentes.

4. Bezárja az alkalmazást, és indítsa újra a ellenőrizze, hogy az új elemek létrehozott állandó vannak-e a helyi tárolóba.

5. (Nem kötelező) A Visual Studióban nyissa meg a **Kiszolgáló Intézőt**. Nyissa meg az adatbázis **Azure**-ban->**SQL-adatbázisait**. Kattintson a jobb gombbal az adatbázist, és válassza a **Megnyitás az SQL Server-objektum Explorer**. Most már megkeresheti az SQL-adatbázis tábla és annak tartalmát. Győződjön meg arról, hogy az adatok, adatbázisszerű, kódmentes nem változott.

6. (Nem kötelező) A mobil kódmentes, a űrlap beolvasása lekérdezéssel lekérdezés például Fiddler vagy Postman többi eszköz segítségével `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Újracsatlakozás a mobilalkalmazás kódmentes az alkalmazás frissítése

Ebben a részben újra az alkalmazást a mobilalkalmazásban kódmentes. Ezek a változások hasonlóan egy hálózati újracsatlakozás az alkalmazásba.

Az alkalmazás első futtatásakor a `OnNavigatedTo` eseménykezelő-hívások `InitLocalStoreAsync`. Ez a módszer meghívja `SyncAsync` szinkronizálhatja a helyi tárolóba, adatbázisszerű, kódmentes együtt. Az alkalmazás megkísérli indításkor szinkronizálása.

1. Nyissa meg a App.xaml.cs a megosztott projektben, és vegye ki a megjegyzésjeleket a korábbi inicializálni `MobileServiceClient` használni a megfelelő mobilalkalmazás URL-CÍMÉT.

2. Nyomja le az **F5** billentyűvel újjáépítése, és futtassa az alkalmazást. Az alkalmazás és a helyi módosítások szinkronizál a az Azure mobilalkalmazás kódmentes leküldéses és ki műveletek használata esetén a `OnNavigatedTo` eseménykezelő hajt végre.

3. (Nem kötelező) Az SQL Server-objektum Explorer vagy a többi eszköz például Fiddler frissített adatok megtekintése. Értesítés az adatok szinkronizálása Azure mobilalkalmazás, adatbázisszerű, kódmentes és a helyi tárolóba között.

4. Kattintson az alkalmazást, néhány dolog, hajtsa végre ezeket a helyi tárolóba mellett található jelölőnégyzetet.

  `UpdateCheckedTodoItem`hívások `SyncAsync` mobilalkalmazás-fájlok szinkronizálása minden kész elemre. `SyncAsync`hívások leküldéses és ki is. Jó helyen jár **Amikor egy ki, hogy az ügyfél módosította, táblákon végrehajtása leküldéses mindig automatikusan végrehajtása**. Ez a jelenség biztosítja, hogy a helyi tárolóba együtt kapcsolatok szereplő összes táblázat továbbra is egységes. Ez a jelenség egy váratlan leküldéses vonhat.  További információt a problémáról olvassa el a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások]című témakört.


##<a name="api-summary"></a>API összegzése

Mobil szolgáltatások offline funkciók támogatása, hogy a [IMobileServiceSyncTable] felületet használja, és [MobileServiceClient.SyncContext] inicializált[ synccontext] a SQLite helyi adatbázist. Kapcsolat nélküli módban, a normál CRUD műveletek Mobile-appokról működik, mintha az alkalmazás továbbra is csatlakozik, amíg a műveletek fordulhat elő, szemben a helyi tárolóba. Az alábbi módszerek segítségével a helyi tárolóba szinkronizálja a kiszolgálóval:

*  **[PushAsync]** Mivel ez a módszer [IMobileServicesSyncContext]tagja, a módosítások minden olyan tábla között a kódmentes vannak tolódik. Csak azok a rekordok, helyi módosításokkal elküldi a kiszolgálóra.

* **[PullAsync] ** 
   egy ki egy [IMobileServiceSyncTable]az azonnal elindul. Ha a táblázat a nyomon követett változások, egy implicit leküldéses futtatásával győződjön meg arról, hogy a helyi tárolóba együtt kapcsolatok szereplő összes táblázat továbbra is egységes. A *pushOtherTables* paraméter határozza meg, hogy a többi táblázat környezetében egy implicit leküldéses vannak tolni. Megnyitja a *lekérdezési* paraméter egy [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   vagy OData lekérdezési karakterláncban, a visszaadott adatok szűréséhez. A *queryId* paraméter megadása növekményes szinkronizálási szolgál. További tudnivalókért olvassa el a  [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások](app-service-mobile-offline-data-sync.md#how-sync-works)című témakört.

* **[PurgeAsync]** Az alkalmazás rendszeres kell hívnia ezt a módszert, a helyi tárolóba elavult adatainak törlése. *Kényszerített* paraméter használatával, ha még nem szinkronizált által végzett esetleges módosítások véglegesen kell.

E fogalmak kapcsolatos további tudnivalókért olvassa el a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások](app-service-mobile-offline-data-sync.md#how-sync-works)című témakört.

## <a name="more-info"></a>További információ

Az alábbi témakörökben további tudnivalókat a Mobile-alkalmazások a kapcsolat nélküli szinkronizálás funkciót:

* [Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]
* [Azure mobilalkalmazások .NET SDK útmutató][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md
[a windows-alkalmazás létrehozása]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
