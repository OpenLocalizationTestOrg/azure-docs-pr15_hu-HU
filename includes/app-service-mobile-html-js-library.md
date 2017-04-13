##<a name="create-client"></a>Ügyfél-kapcsolat létrehozása

Ügyfél-kapcsolat létrehozása: hozzon létre egy `WindowsAzure.MobileServiceClient` objektumot.  Csere `appUrl` a mobilalkalmazásban URL-címét.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Táblázatok használata

Az access vagy a frissítés adatokat hozzon létre hivatkozást a kódmentes táblában. Csere `tableName` annak a táblának a nevére a

```
var table = client.getTable(tableName);
```

Ha befejezte a táblázathivatkozásokban, webhelyeken is megtekintheti a táblázatot további:

* [Lekérdezési táblázat](#querying)
  * [Adatok szűrése](#table-filter)
  * [Lapozás adatai között](#table-paging)
  * [Adatok rendezése](#sorting-data)
* [Adatok beszúrása](#inserting)
* [Adatok módosítása](#modifying)
* [Adatok törlése](#deleting)

###<a name="querying"></a>Útmutató: táblázat hivatkozást a lekérdezés

Ha befejezte a táblázathivatkozásokban, használhatja a kiszolgálóhoz adatok lekérdezéséhez.  Lekérdezések "LINQ-like" nyelven történik.
Az összes adat a táblából kiszámításához használja a következő:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

A sikeres függvény neve az eredményeket.   Ne használjon `for (var i in results)` sikerének módon, amely információkat az eredmények a ismétlés fog működni amikor más lekérdezés függvényekkel (például: `.includeTotalCount()`) használják.

A lekérdezés szintaxisát a további tudnivalókért olvassa el a [lekérdezés objektum dokumentációját].

####<a name="table-filter"></a>Adatok szűrése a kiszolgálón

Használhatja a `where` záradék a táblázat hivatkozást:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Szűri az objektum függvényt is használhatja.  Ebben az esetben a `this` változó az aktuális objektum szűrve van-e hozzárendelve.  Az alábbiakban látható megegyezik az előző példában:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Lapozás adatai között

A take() és skip() módszerek csatlakozást.  Ha például, hogy a táblázat felosztása 100 sor rekordok kíván:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

A `.includeTotalCount()` módszert totalCount mező felvétele az eredmények objektumra.  A totalCount mező van töltve vissza: nincs lapozási használatakor rekordok száma.

Majd használhatja a lapok változó és néhány a felhasználói felület gombjainak megadására, oldal lista; loadPage() segítségével töltse be az új rekordok lapot.  Rekordok már betöltött sebesség elérésének gyorsítótárazásának funkciója végre kell hajtania.


####<a name="sorting-data"></a>Útmutató: adatok rendezése

Használja a .orderBy() vagy .orderByDescending() lekérdezés módszereket:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

A lekérdezés objektum további tudnivalókért olvassa el a [lekérdezés objektum dokumentációját].

###<a name="inserting"></a>Útmutató: adatok beillesztése

JavaScript-objektum létrehozása a megfelelő dátuma, és hívja fel a table.insert() aszinkron:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

A sikeres beszúrási a további mezők szükséges, a szinkronizálási műveletek a beszúrt elemet adja vissza.  A későbbi frissítések ezt az információt a saját gyorsítótár frissítenie kell.

Figyelje meg, hogy támogatja-e az Azure Mobile alkalmazások Node.js Server SDK dinamikus séma fejlesztési célra.
Dinamikus séma, amíg a táblázat a séma menet, lehetővé téve a hasábok hozzáadása a táblázat csak megadásával őket az insert vagy frissítése művelet frissül.  Azt javasoljuk, hogy kikapcsolja dinamikus séma mielőtt gyártási az alkalmazást.

###<a name="modifying"></a>Útmutató: adatok módosítása

Hasonló a .insert() módszerrel, érdemes hozzon létre egy frissítési objektumot és hívhatja .update().  A frissítés objektum tartalmaznia kell frissíteni kell a rekord azonosítója – ez kapott, amikor a rekord olvasási vagy .insert() hívásakor.

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Útmutató: adatok törlése

Hívja fel a .del() módszer, ha rekordot szeretne törölni.  Az azonosító hozzáférési egy objektum hivatkozások:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
