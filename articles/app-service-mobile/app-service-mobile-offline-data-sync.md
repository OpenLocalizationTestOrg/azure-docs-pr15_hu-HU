<properties
    pageTitle="Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások |} Microsoft Azure"
    description="Magyarázó rész hivatkozás és a kapcsolat nélküli szinkronizálás szolgáltatás Azure Mobile-alkalmazások áttekintése"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások

## <a name="what-is-offline-data-sync"></a>Mi az kapcsolat nélküli szinkronizálás?

Kapcsolat nélküli adatszinkronizálás egy ügyfél- és kiszolgálóoldali SDK szolgáltatása, amely megkönnyíti a fejlesztők számára is készíthet alkalmazást, amelyek a hálózati kapcsolat nélkül funkcionális Azure Mobile-alkalmazások.

Ha az alkalmazás kapcsolat nélküli módban van, felhasználók továbbra is létrehozhat és módosítani az adatokat, amelyeket a program menti a helyi tárolóba. Amikor az alkalmazás vissza online állapotban, hogy szinkronizálhatja helyi módosítások az Azure mobilalkalmazás kódmentes. A funkció is támogatja az ütközések észlelését, ugyanazt a rekordot az ügyfél és a kódmentes megváltozásakor. Ütközések lehet kezelni, vagy a kiszolgálón vagy az ügyfélnek.

Kapcsolat nélküli szinkronizálás számos előnyét:

* Alkalmazás válaszidő növelheti gyorsítótár-kiszolgálóadatok helyileg az eszközön
* A továbbra is hasznos, ha hálózati problémák lépnek fel robusztus alkalmazások létrehozása
* Végfelhasználók hozhat létre, és módosíthatják az adatokat, akkor is, ha nincs eseteket támogató kis vagy nincs kapcsolat a hálózati hozzáférés engedélyezése
* Adatok szinkronizálása minden több eszközön, és ütközés észleli, ha a két eszköz módosított ugyanazt a rekordot
* Max-késés vagy a forgalmi díjas hálózatok hálózati használatának korlátozása

Az alábbi oktatóanyagok megjelenítése a mobilügyfelek az Azure Mobile-alkalmazások használata kapcsolat nélküli szinkronizálás hozzáadása:

* [Android: Kapcsolat nélküli szinkronizálás engedélyezése]
* [iOS: kapcsolat nélküli szinkronizálás engedélyezése]
* [Xamarin iOS: kapcsolat nélküli szinkronizálás engedélyezése]
* [Android-alapú Xamarin: Kapcsolat nélküli szinkronizálás engedélyezése]
* [Xamarin.Forms: Engedélyezése kapcsolat nélküli szinkronizálás](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Univerzális Windows platformon: Kapcsolat nélküli szinkronizálás engedélyezése]

## <a name="what-is-a-sync-table"></a>Mi az szinkronizálási táblázat?

A "/ táblázatok" végpont eléréséhez az Azure mobil ügyfélprogram SDK nyújt felületek például `IMobileServiceTable` (.NET ügyfél SDK) vagy `MSTable` (iOS-ügyfél). Ezek az API-khoz közvetlenül csatlakozhat az Azure mobilalkalmazás kódmentes, és meghiúsul, ha az ügyfél eszköz nincs hálózati kapcsolaton.

Kapcsolat nélküli használatát támogatja, az alkalmazás helyett használjon a *szinkronizálási táblázat* API-hoz, például `IMobileServiceSyncTable` (.NET ügyfél SDK) vagy `MSSyncTable` (iOS-ügyfél). Azonos CRUD műveletek (létrehozás, Olvasás, frissítés, Törlés) munka ellen szinkronizálási táblázat API-hoz, azzal a különbséggel most fog olvasnak vagy írja be a *helyi tárolóba*. Táblázat szinkronizálási műveletek végrehajtása előtt inicializálni kell a helyi tárolóba.

## <a name="what-is-a-local-store"></a>Mi az a helyi tárolóba?

A helyi tárolóba az adatok adatmegőrzési réteg ügyfél eszközre. Az Azure Mobile-alkalmazások ügyfél SDK nyújtani alapértelmezett helyi tárolóba. A Windows, a Xamarin és az Android-alapú alapul SQLite; iOS, kattintson rá Core adatok alapján.

Vagy történő használatáról SQLite-alapú végrehajtása Windows Phone 8.1 a Windows áruházból, egy SQLite bővítmény telepítéséhez szükséges. További részletekért lásd: [univerzális Windows platformon: kapcsolat nélküli szinkronizálás engedélyezése]. Android és iOS kiszállítása SQLite változata az eszköz operációs rendszerben magát, így nem saját verziójának SQLite hivatkozni szeretne.

A fejlesztők saját helyi tárolóba is alkalmazhat. Például ha szeretne egy titkosított formában jelenik meg a mobil ügyfélprogram tárolja az adatokat, ablakban adhatja meg a helyi tárolóba titkosításhoz SQLCipher használó.

## <a name="what-is-a-sync-context"></a>Mi az szinkronizálási környezetben?

A *szinkronizálási környezetben* társítva a mobil ügyfélprogram objektum (például: `IMobileServiceClient` vagy `MSClient`), és nyomon követi a szinkronizálási táblázatokkal elvégzett módosításokat. A szinkronizálási helyi tart fenn egy *művelet várólista* , így az megmarad CUD műveletek (létrehozás, frissítés, Törlés) olyan rendezett listát, hogy később küld a kiszolgáló.

A helyi tárolóba társítva az szinkronizálási környezetben, például egy initialize módszerrel `IMobileServicesSyncContext.InitializeAsync(localstore)` a [.NET-ügyfél SDK csomagjában talál].

## <a name="how-sync-works"></a>Kapcsolat nélküli hogyan működik a szinkronizálás?

Szinkronizálási táblázatok használatakor az ügyfél kód szabályozza, amikor helyi módosítások szinkronizálódni fognak az Azure mobilalkalmazás kódmentes. Semmi sem küldi a kódmentes addig, amíg nincs hívást kezdeményez *leküldéses* változtatásokat. Hasonlóképpen a helyi tárolóba kitölti új adatokat csak akkor van egy hívás *lekéri* az adatokat.

* **Leküldéses**: leküldéses művelet, a szinkronizálási környezetben, és értesítést küld a minden CUD módosítás az utolsó leküldéses óta. Ne feledje, hogy nem lehet elküldeni csak egy adott tábla módosításokat, mert ellenkező műveletek lehetett elküldeni megfelelő sorrendben. Leküldéses végrehajtja a többi hívásokat az Azure mobilalkalmazás kódmentes, ami viszont módosítani a server-adatbázis fogja sorozata.

* **Lekérés**: ki táblázat alapon történik, és csak egy részét a kiszolgálóhoz adatok beolvasásához lekérdezéssel testre szabható. Az Azure mobil ügyfélprogram SDK helyezze az eredményül kapott adatok a helyi tárolóba.

* **Implicit verembe küldi**: a leküldéses végrehajtása szemben a függőben lévő helyi frissítések tartalmazó táblát, ha a leküldéses fog először hajtsa végre a leküldéses szinkronizálás keretében. Ezzel az elemzéssel kisméretűvé alakítása a módosításokat, amelyeket már vannak aszinkron és az új adatok a kiszolgáló közötti ütközéseket.

* **Növekményes szinkronizálás**: a leküldéses művelet az első paramétere a *lekérdezés nevét* , amely csak az ügyfélgépen. Ha használja a nem null lekérdezés nevét, az Azure Mobile SDK elvégez egy *növekményes szinkronizálása*.
  Minden alkalommal, amikor egy ki művelet lásson, amelyeket az eredmények, a legújabb `updatedAt` időbélyeg, hogy az eredményhalmaz a rendszer a SDK helyi rendszer táblákban tárolja. Leküldéses egymást követő műveletek rekordok csak beolvassa az időbélyeg után.

  Növekményes szinkronizálás használatára, a kiszolgáló kell visszaadnia értelmes `updatedAt` értékek és is támogatnia kell ezt a mezőt szerinti rendezést. Azonban a SDK saját rendezési a updatedAt mezőt veszi fel, mert nem ki lekérdezést használ, amelynek a saját `$orderBy$` záradék.

  A lekérdezés nevét, válassza a karakterlánc lehet, de az alkalmazás minden logikai lekérdezés egyedinek kell lennie.
  Ellenkező esetben a különböző ki műveletek sikerült felülírja az azonos növekményes szinkronizálási időbélyeg, és a lekérdezések helytelen eredményt térhet vissza.

  Ha a lekérdezés paraméter, egy adjon nevet a lekérdezés egyedi módja, amelyekkel beépíthetők a paraméter értéke.
  Például a felhasználói azonosítóval szűr, ha a lekérdezés neve lehet az alábbi képlettel történik (a C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Növekményes szinkronizálási lemondása szeretné, ha a sikeres `null` , a lekérdezés azonosítóját. Ebben az esetben rekordokat lesz visszakeresve, amely minden hívása `PullAsync`, amely olyan, esetleg nem hatékony.

* **Purging**: a helyi tárolóba használatával tartalmát törölheti `IMobileServiceSyncTable.PurgeAsync`.
  Erre akkor lehet szükség, ha elavult adatokat az ügyfél-adatbázisban, vagy ha minden függőben lévő módosítások elvetése szeretne.

  A végleges törlése a helyi tárolóba táblázat törli. Ha az adatbázishoz való szinkronizálás függőben lévő műveletek, a kiürítés fog kivételhibát kivéve, ha a *kötelező véglegesen* paraméter értéke.

  Az ügyfélgépen elavult adatok példaként tegyük fel, hogy a "teendőlista" példában Device1 csak gyűjti össze a elemek, amelyek nem fejeződött be. Ezt követően egy todoitem "Vásárlása tej" van-e jelölve a kiszolgálón kiegészíteni egy másik eszközt. Azonban Device1 továbbra is rendelkeznek a "Vásárlása tej" todoitem a helyi tárolóba, mert csak akkor van adatok nem a készként megjelölt elemek. A kiürítés törli az elavult elemet.

## <a name="next-steps"></a>Következő lépések

* [iOS: kapcsolat nélküli szinkronizálás engedélyezése]
* [Xamarin iOS: kapcsolat nélküli szinkronizálás engedélyezése]
* [Android-alapú Xamarin: Kapcsolat nélküli szinkronizálás engedélyezése]
* [Univerzális Windows platformon: Kapcsolat nélküli szinkronizálás engedélyezése]

<!-- Links -->
[.NET-ügyfél szoftverfejlesztői csomag]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Kapcsolat nélküli szinkronizálás engedélyezése]: app-service-mobile-android-get-started-offline-data.md
[iOS: kapcsolat nélküli szinkronizálás engedélyezése]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: kapcsolat nélküli szinkronizálás engedélyezése]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Android-alapú Xamarin: Kapcsolat nélküli szinkronizálás engedélyezése]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Univerzális Windows platformon: Kapcsolat nélküli szinkronizálás engedélyezése]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
