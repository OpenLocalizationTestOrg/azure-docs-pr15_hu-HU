<properties
    pageTitle="Az alkalmazás szolgáltatás Mobile-alkalmazások felügyelt ügyfél-tár használata (Windows |} Xamarin) |} Microsoft Azure"
    description="Tudnivalók a .NET-ügyfél használata Azure alkalmazás szolgáltatás Mobile-Appok Windows és Xamarin alkalmazással."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>A felügyelt ügyfél használata Azure Mobile-appokról

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a felügyelt ügyfél tár Azure alkalmazás szolgáltatás alkalmazásokat a Windows Mobile és a Xamarin alkalmazások használt tipikus esetei. Ha először a Mobile-alkalmazások, vegye figyelembe az [Azure Mobile-alkalmazások quickstart útmutató] először befejezése[ 1] oktatóprogram. Ebből az útmutatóból azt kiemelése a ügyféloldali felügyelt SDK csomagjában talál. Ha többet szeretne tudni a kiszolgálóoldali SDK, Mobile-appokról, dokumentációjában a [.NET-kiszolgáló SDK] [ 2] vagy a [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Hivatkozási dokumentáció

Az ügyfél SDK dokumentációját található itt: [Azure Mobile alkalmazások .NET ügyfél hivatkozás][4].
Több ügyfél mintát is megtalálhatók az [Azure-minták GitHub tárházba][5].

## <a name="supported-platforms"></a>Támogatott platformok

A .NET-Platform támogatja a következő platformokon:

* 24 (KitKat nugát keresztül) keresztül az API 19 Xamarin Android-verziókban
* Xamarin iOS elengedi az iOS 8.0-s vagy újabb verziók
* Univerzális Windows platformra
* Windows Phone 8.1
* Windows Phone 8.0, kivéve a Silverlight-alkalmazások

A "server-adatfolyam" hitelesítés a felhasználói felület bemutatott egy webnézet használja.  Ha az eszköz nem tudja webnézet felhasználói Felületet bemutatására, más hitelesítési módszer van szükség.  Ez az SDK így nem alkalmas megtekintés típusú vagy hasonló módon korlátozott eszközöket.

##<a name="setup"></a>A telepítő és vonatkozó követelmények

Feltételezzük, hogy már létrehozott és közzétenni a mobilalkalmazás kódmentes projekt, amelynek legalább egy táblázatot tartalmaz.  Ez a témakör kódjába a táblázat neve `TodoItem` , és ennek a következő oszlopok: `Id`, `Text`, és `Complete`. Az alábbi táblázat a táblázatból a [Azure Mobile-alkalmazások quickstart útmutató] befejezésekor létrehozott.

A megfelelő beírt ügyféloldali típusát a C# eredetű a következő:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

A [JsonPropertyAttribute] [ 6] definiálja, a *tulajdonságnév* hozzárendelés az ügyféltípus és a tábla között.

Megtudhatja, hogyan hozhat létre a Mobile-alkalmazások kódmentes táblákat, lásd: az információkat a [.NET-kiszolgáló SDK témakör] [ 7] vagy a [Node.js Server SDK témakör][8]. A mobilalkalmazás kódmentes az Azure-portálon használja a quickstart útmutató hozta létre, ha az [Azure portálon]is a **könnyen táblák** beállítást is használhatja.

###<a name="how-to-install-the-managed-client-sdk-package"></a>Útmutató: a felügyelt SDK ügyfélcsomag telepítése

A felügyelt SDK ügyfélcsomag telepítése [NuGet]Mobile alkalmazások használatával az alábbi módszerek egyikét[9]:

+ **Visual Studio** Kattintson a jobb gombbal a projekt, kattintson a **NuGet csomagok kezelése**, kereshet a `Microsoft.Azure.Mobile.Client` csomag, majd kattintson a **telepítés**gombra.

+ **Xamarin Studio** Kattintson a jobb gombbal a projektet, kattintson a **Hozzáadás** > **NuGet csomagok hozzáadása**, keressen a `Microsoft.Azure.Mobile.Client `csomag, és kattintson a **Csomag hozzáadása**gombra.

A fő tevékenység fájl ne felejtse el az alábbi **használatával** utasítás hozzáadása:

    using Microsoft.WindowsAzure.MobileServices;

###<a name="symbolsource"></a>Útmutató: a Visual Studio hibakeresési szimbólumok használata

A szimbólumok a Microsoft.Azure.Mobile névtér érhetők el a [SymbolSource][10].  Olvassa el az [utasításokat SymbolSource] [ 11] SymbolSource integrálása a Visual Studio.

##<a name="create-client"></a>A Mobile-alkalmazások ügyfél létrehozása

A következő kódot hoz létre a [MobileServiceClient] [ 12] objektumra, amely a mobilalkalmazás kódmentes eléréséhez használt.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Az előző kód, a csere `MOBILE_APP_URL` a mobilalkalmazás kódmentes URL-címmel, amely megtalálható esetében a mobilalkalmazás kódmentes az [Azure portálon]a lap. A MobileServiceClient objektum egyszeres kell lennie.

## <a name="work-with-tables"></a>Táblázatok használata

A következő szakasz adatai keresése és rekordjával, és módosítsa az adatokat a táblázaton belül.  Az alábbi témaköröket:

* [Egy táblázat hivatkozást hoz létre.](#instantiating)
* [Adatok lekérdezése](#querying)
* [Visszaadott adatok szűrése](#filtering)
* [A visszaadott adatok rendezése](#sorting)
* [Lépjen vissza az adatokat a lapokon](#paging)
* [Egyedi oszlopok kijelölése](#selecting)
* [Azonosító szerint rekord keresése](#lookingup)
* [Típusos lekérdezések kezelése](#untypedqueries)
* [Adatok beszúrása](#inserting)
* [Adatok frissítése](#updating)
* [Adatok törlése](#deleting)
* [Ütközések feloldása és optimista feldolgozási](#optimisticconcurrency)
* [Windows felhasználói felület kötése](#binding)
* [Az oldalméret módosítása](#pagesize)

###<a name="instantiating"></a>Útmutató: táblázat hivatkozás létrehozása

A kód, amely segítségével érheti el, vagy módosítja a kódmentes táblázatban lévő adatok függvények felhívja a a `MobileServiceTable` objektumot. Szerezze be a táblázat hivatkozás hívja fel a [GetTable] módszer a következőképpen:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

A visszaadott objektum a beírt szerializálási modellt használja. Egy típusos szerializálási modell is támogatott. A következő példa [típusossá egy hivatkozást hoz létre]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

Típusos lekérdezésekben meg kell adnia a mögöttes OData lekérdezési karakterlánc.

###<a name="querying"></a>Útmutató: a Mobile-alkalmazás adatait a lekérdezés

Ez a szakasz lekérdezések ki a mobilalkalmazás kódmentes, amely tartalmazza a következő funkciókat ismerteti:

- [Visszaadott adatok szűrése](#filtering)
- [A visszaadott adatok rendezése](#sorting)
- [Lépjen vissza az adatokat a lapokon](#paging)
- [Egyedi oszlopok kijelölése](#selecting)
- [Táblázatadatok azonosító szerint](#lookingup)

>[AZURE.NOTE]Minden sor megakadályozásához visszaküldött kiszolgáló-alapú oldalméret lép érvénybe.  Lapozás a negatív érintő a szolgáltatás továbbra is nagy adatkészletek alapértelmezett kérelem.  Több, mint 50 sora kiszámításához használja a `Skip` és `Take` módszer, [a lapokon visszaadott adatok] leírt módon.

###<a name="filtering"></a>Útmutató: szűrés visszaadott adatok

A következő kódrészlet szemlélteti, hogyan kell adatok szűrése, beleértve a `Where` záradék egy lekérdezésben. Akkor adja vissza az összes elemet `todoTable` amelynek `Complete` tulajdonság értéke `false`. A [Ha] függvénnyel szemben a táblázatot a lekérdezésre predikátumok szűrése sor vonatkozik.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

A kérelem küld ki a kódmentes szoftverrel üzenet vizsgálati, például a böngésző Fejlesztőeszközök vagy [Fiddler]az URI tekintheti meg. Tekintse meg a kért URI, ha figyelje meg, hogy a lekérdezési karakterlánc módosult:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Az OData-összehívást fordítani SQL-lekérdezés által a kiszolgáló SDK csomagjában talál:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

A függvény átadott a `Where` módszer beállíthatja, hogy feltételeket egy tetszőleges számú.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

Ez a példa volna értelmezhető SQL-lekérdezés, a kiszolgáló SDK szerint:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

A lekérdezés is feloszthat több záradékot:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

A két módszer között nem tesz különbséget és azonos értelemben használható.  A korábbi beállítás&mdash;az egy lekérdezésben több predikátumok szereplő&mdash;tömörebb és ajánlott.

A `Where` záradék műveletek, amely támogatja az OData-Alkészlet fordítani. A következők műveleteket:

* Relációs operátorok (==,! =, <, <, >, > =),
* Számtani operátorok (+, -, /, *, %),
* Szám pontosságát (Math.Floor, Math.Ceiling)
* Karakterláncfüggvények (hosszúságú karakterlánc, a csere, IndexOf, StartsWith, EndsWith),
* A Dátumtulajdonságok (év, hónap, nap, órát, percet és második),
* Az objektum tulajdonságai elérése és
* A kifejezések egyesítése ezek a műveletek közül.

Amikor a kiszolgáló SDK támogatja, megpróbálhatja [OData v3 dokumentációt].

###<a name="sorting"></a>Útmutató: Rendezés visszaadott adatok

A következő kódrészlet szemlélteti, hogyan kell adatok rendezése egy [OrderBy] vagy [OrderByDescending] függvény együtt a lekérdezésben. Elemek vissza `todoTable` növekvő sorrend szerint a `Text` mező.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="paging"></a>Útmutató: adatok visszatérési a lapokon

Alapértelmezés szerint a kódmentes csak az első 50 sora adja eredményül. Hívja fel a [készítése] módszer növelheti a visszaadott sorok számát. Használat `Take` együtt a [Kihagyás] módszer egy "oldalán" az összes adatkészlet a lekérdezés által visszaadott adott kérhet. Az alábbi lekérdezés futtatásakor, az első három elem a táblázatban adja eredményül.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

A következő módosított lekérdezés az első három eredmények átugrása, és a következő három eredményt ad vissza. Ezt a lekérdezést hoz létre, a második "lap" az adatokat, az oldalméret három elem esetén.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

A [IncludeTotalCount] módszer kéri a darabszáma az _összes_ volna visszaküldött figyelmen kívül hagyja a bármely lapozási/korlát záradékban megadott rekordokat:

    query = query.IncludeTotalCount();

Az egy valós alkalmazást az előző példához hasonló lekérdezések használhat személyhívó vezérlőelem vagy más hasonló felhasználói felület oldalai között.

>[AZURE.NOTE]Az 50-sor határérték egy mobilalkalmazás kódmentes felülbírálásához is kell a [EnableQueryAttribute] alkalmazása a nyilvános GET-metódus és adja meg a lapozási viselkedését. Ha a módszerrel egyenként alkalmazza őket, az alábbi állítja be 1000 visszaadott sorok maximális:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>Útmutató: egyedi oszlopok kijelölése

Megadhatja, amely tulajdonságainak beállítása felvenni a találatok között a lekérdezést egy [kiválasztása] záradékot hozzáadásával. A következő kódot például csak egy mező kijelölése, és válassza ki, és több mező formázása jeleníti meg:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Az eddigi leírt összes függvények additívak, így azt is megtartása láncolás őket. További, a lekérdezés minden láncolt hívás hatással van. Egy további példa:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>Útmutató: Táblázatadatok azonosító szerint

A [LookupAsync] függvény használható az adatbázis egy adott azonosítót tartalmazó objektumok kereséséhez

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="untypedqueries"></a>Útmutató: Típusos lekérdezések végrehajtása

A lekérdezések típusos táblázat objektum végrehajtásakor meg kell adnia a OData lekérdezési karakterlánc hívja fel a [ReadAsync], ahogy a következő példában:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Akkor visszatérhet JSON értékek, például egy tulajdonság között használható. További információt a JToken és Newtonsoft Json.NET látogasson el a [Json.NET] .

### <a name="inserting"></a>Útmutató: adatok beillesztése egy mobilalkalmazás kódmentes

Lekérdezésiügyfél-típusok neve **Id**, egy karakterlánc alapértelmezés szerint tartoznia kell tartalmaznia. Az **azonosító** CRUD műveletek elvégzéséhez szükséges és kapcsolat nélküli szinkronizálás. A következő kódrészlet szemlélteti, hogyan kell a [InsertAsync] módszernek az új sor beszúrása egy táblázatba. A paraméter .NET objektumként beilleszteni kívánt adatokat tartalmazza.

    await todoTable.InsertAsync(todoItem);

Ha egyéni egyedi azonosító érték nem szerepel a `todoItem` GUID során insert, a kiszolgáló által generált.
A létrehozott azonosító meghallgathatja az objektum megvizsgálva, miután a hívás adja eredményül.

Típusos adatok beszúrni, akkor előfordulhat, hogy kihasználhatja Json.NET:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Íme egy példa karakterlánc egyedi azonosító e-mail cím használatával:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Területiazonosító-értékek használata

Mobile-alkalmazások egyéni karakterlánc egyedi értékeket támogatja a táblázat **azonosító** oszlopában. Szöveges értékként lehetővé teszi, hogy az alkalmazásokat, például az e-mail címeket vagy az azonosító nevével egyéni értékek használata  Karakterlánc azonosítók adja meg a következő előnyökkel jár:

* Azonosítók jönnek létre anélkül, hogy egy üzenetváltási az adatbázishoz.
* A rekordok egyesítése különböző táblákból vagy adatbázisok könnyebben fognak.
* Az alkalmazások logika a jobban integrálhatja azonosítók értékeket.

Azonosító karakterláncérték nincs beállítva egy beszúrt rekordot, a mobilalkalmazás kódmentes hoz létre egy egyedi érték a azonosítóját. Saját Területiazonosító-értékek, az ügyfél vagy a kódmentes a létrehozásához a [Guid.NewGuid] módszerrel is használhatja.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="modifying"></a>Útmutató: a mobilalkalmazás kódmentes adatainak módosítása

A következő kódrészlet szemlélteti, hogyan kell a [UpdateAsync] módszernek az alkalmazását egy meglévő rekord frissítése az új információkkal azonos azonosító. A paraméter .NET objektumként frissíteni kell adatokat tartalmazza.

    await todoTable.UpdateAsync(todoItem);

Típusos Ha adatokat szeretné frissíteni, akkor előfordulhat, hogy kihasználhatja [Json.NET] az alábbi képlettel történik:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Egy `id` mezőt meg kell adni, amikor frissítés végrehajtása. A kódmentes használja a `id` azonosítják milyen sorát frissítése mezőt. A `id` mező eredményét szerezheti be a `InsertAsync` hívja. Egy `ArgumentException` akkor következik be, ha megpróbálja frissíteni elem anélkül, hogy a `id` értéket.

###<a name="deleting"></a>Útmutató: a mobilalkalmazás kódmentes adatainak törlése

A következő kódrészlet szemlélteti, hogyan kell a [DeleteAsync] módszernek az alkalmazását egy meglévő példány törlése. A példány azonosítjuk a `id` megadása mezőjében a `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Típusos adatokat szeretné törölni, akkor előfordulhat, hogy kihasználhatja Json.NET az alábbi képlettel történik:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Törlés kérést, hogy, az azonosító meg kell adni. Egyéb tulajdonságait nem át a szolgáltatást a vagy függvény figyelmen kívül hagyja a szolgáltatást a. Eredménye egy `DeleteAsync` hívás van általában `null`. Az azonosító átadni beszerezhető eredményét a `InsertAsync` hívja. A `MobileServiceInvalidOperationException` törlése nélkülire elő a `id` mezőben.

###<a name="optimisticconcurrency"></a>Útmutató: optimista feldolgozási mód ütközések feloldása

Két vagy több ügyfél írhat módosítások ugyanazon az elemen egy időben. Ütközés észlelése, nélkül az utolsó írás felülírja az előző frissítések. **Optimista feldolgozási vezérlő** feltételezi, hogy tranzakciók is véglegesítése és ezért nem használja bármely erőforrás zárolása.  Tranzakció elvégzése, előtt optimista feldolgozási vezérlő ellenőrzi, hogy nincs más tranzakció módosította az adatokat. Az adatok módosításakor a végrehajtása tranzakció visszaáll.

Mobile-alkalmazások optimista feldolgozási vezérlő támogatja a változások követése minden elem használatával a `version` rendszer tulajdonság oszlop minden táblázatában a mobilalkalmazás kódmentes a meghatározott. Minden alkalommal, amikor a rekord frissítése Mobile-alkalmazások állítja be a `version` tulajdonság, egy új értéket a rekordot. Minden egyes frissítés kérés során a `version` tulajdonság a rekord a kérelem részét képező összehasonlítja az azonos tulajdonság a rekord a kiszolgálón. Ha az átadott a verziót a kérelem nem egyezik meg a kódmentes, majd az ügyfél tár hatványra egy `MobileServicePreconditionFailedException<T>` kivétel. A kivételt képező típusa: a rekord kiszolgáló verziója tartalmazó kódmentes rekordja. Az alkalmazás segítségével ezt az információt hajtsa végre a frissítési kérést a megfelelő döntés `version` mezőbe írja be a módosítások véglegesítése kódmentes.

Egy oszlop definiálása a táblázat osztály a `version` optimista feldolgozási ahhoz, hogy a rendszer tulajdonság. Példa:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Alkalmazások használatával típusos táblák optimista feldolgozási engedélyezése megadásával a `Version` a megjelölés a `SystemProperties` a táblázat az alábbiak szerint.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Nemcsak a optimista feldolgozási engedélyezése, meg kell is elfog a `MobileServicePreconditionFailedException<T>` a kód [UpdateAsync]hívásakor kivétel.  A megfelelő alkalmazásával az ütközésfeloldási `version` a frissített rekordot, és a hívás [UpdateAsync] a megoldott rekordhoz. A következő kódot egyszer észlelt írási ütközés feloldása mutatja be:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

További információ a [Kapcsolat nélküli adatok szinkronizálása az Azure Mobile-alkalmazások] témakörben talál.

###<a name="binding"></a>Útmutató: adatok kötés Mobile-alkalmazások Windows felhasználói felület

Ebben a részben a Felhasználóifelület-elemek használata a Windows-at, a visszaadott adatok objektumok megjelenítését.  A következő példa kódot a listában az elemek hiányos lekérdezés forrásának köti. A [MobileServiceCollection] mobil alkalmazások-et kötés gyűjtemény hoz létre.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

A felügyelt futtatókörnyezet egyes vezérlőelemei [ISupportIncrementalLoading]nevű felületet támogatja. A kapcsolat lehetővé teszi, hogy a felesleges adatok kérni, amikor a felhasználó görgetése vezérlők. Nincs beépített támogatása a felület-univerzális Windows alkalmazások [MobileServiceIncrementalLoadingCollection], amely automatikusan kezeli a vezérlők hívásait keresztül. Használat `MobileServiceIncrementalLoadingCollection` a Windows-alkalmazásokat az alábbiak szerint:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Az új webhelycsoport Windows Phone 8 és a "Silverlight" alkalmazások használatához a `ToCollection` kiterjesztés módszerek a `IMobileServiceTableQuery<T>` és `IMobileServiceTable<T>`. Adatok betöltése, hívja a `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

A hívó által létrehozott gyűjtemény használatakor `ToCollectionAsync` vagy `ToCollection`, kap, amelyek a felhasználói felület vezérlők kötni gyűjteménye.  Ebben a gyűjteményben lapozási-et.  Mivel a webhelycsoport adatok betöltése a hálózatról, időnként betöltése sikertelen lesz. E hibák kezelendő felülbírálása az `OnException` módszer `MobileServiceIncrementalLoadingCollection` kezelje a hívásokat eredő kivételek `LoadMoreItemsAsync`.

Fontolja meg, ha a táblázat sok mezőket tartalmaz, de csak egy részüket a vezérlőben megjelenítendő szeretné. Használhatja az útmutató ",[Jelölje be az adott oszlop](#selecting)" az előző szakaszban jelölje ki az adott oszlop felhasználói felület megjelenítéséhez.

###<a name="pagesize"></a>Az oldalméret módosítása

Azure Mobile-alkalmazások legfeljebb 50 elemet egy kérelemre alapértelmezés szerint adja eredményül.  Módosíthatja a lapozási méretének növelésével a maximális oldalméretet, az ügyfél és a kiszolgálón.  Ha növelni szeretné a kívánt oldalméretet, adja meg a `PullOptions` használatakor `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Feltételezve, hogy a módosításokkal a `PageSize` egyenlő vagy a kiszolgálón belül 100-nál nagyobb, a kérelem legfeljebb 100 elemet adja vissza.

##<a name="#offlinesync"></a>Kapcsolat nélküli táblázatok használata

Offline táblázatok használata kapcsolat nélküli módban használata egy helyi SQLite áruházból való típusú adatokat tárolja.  Műveletek szemben a helyi végzett összes táblázat SQLite tárolni a távoli kiszolgáló áruházból helyett.  Az offline táblázatot létrehozni, készítse elő a projekthez:

1. A Visual Studióban, kattintson a jobb gombbal a megoldást > **NuGet csomagok kezelése megoldás...**, majd keresse meg és a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet csomag az összes projekt esetében a megoldás telepítése.

2. (Nem kötelező) Támogatja a Windows-eszközöket, telepítse a következő SQLite futtatókörnyezet csomagokban egyikét:

    * **Windows 8.1 futtatókörnyezet:** Telepítse [a Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Telepítse [a Windows Phone 8.1 SQLite][4].
    * **Univerzális Windows platformra** Telepítse [az univerzális Windows univerzális az SQLite][5].

3. (Nem kötelező). Windows-eszközök, kattintson a **hivatkozás** > **Hivatkozás hozzáadása**, bontsa ki a **Windows** -mappába > **bővítmények**, majd a megfelelő **SQLite a Windows** SDK együtt a **Vizuális C++ 2013 futtatókörnyezet Windows** SDK engedélyezése.
    A SQLite SDK neveket egyes Windows platformon kissé eltérő lehet.

Bármely táblázathivatkozás létrehozható, mielőtt a helyi tárolóba kell készíteni:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Tár inicializálni a szokásos módon történik, az ügyfél létrehozása után közvetlenül.  A **OfflineDbPath** kell egy Ön által támogatott összes platformon alkalmas fájlnevet.  Ha egy teljes elérési utat (Ez azt jelenti, hogy kezdődik perjel), akkor az elérési út használják.  Az elérési út nem teljesen minősített, ha a fájlt platform-specifikus helyre kerül.

* Az iOS és az Android-eszközön az alapértelmezett elérési út a "Személyes fájlok" mappát.
* A Windows-eszközök esetén az alapértelmezett elérési út az alkalmazás-specifikus "AppData" mappát.

Bármely táblázathivatkozás felhasználásával lehet beszerezni a `GetSyncTable<>` módszer:

    var table = client.GetSyncTable<TodoItem>();

Nem kell használni egy offline tábla hitelesítést végezni.  Csak kell hitelesítés, ha a kódmentes szolgáltatással kommunikálni szeretne.

###<a name="syncoffline"></a>Az Offline tábla szinkronizálása

Kapcsolat nélküli táblák nem szinkronizálódnak a kódmentes alapértelmezés szerint.  Szinkronizálás két részre oszlik.  Az új elemek letöltése külön-külön közzétehetik módosításokat.  Íme egy tipikus szinkronizálási módszer:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
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

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
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

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Ha az első argumentum az `PullAsync` üres, akkor növekményes szinkronizálási nem használatos.  Minden egyes szinkronizálási műveletet az összes rekordot ad vissza.

A SDK hajt végre implicit `PushAsync()` rekordok adatok előtt.

Ütközés kezelése történik meg a `PullAsync()` módot.  Ütközések feloldása az online táblák megegyező módon is foglalkozik.  Az ütközés készül amikor `PullAsync()` alatt a Beszúrás, a frissítés, vagy a törlés helyett nevezik. Több ütközések fordulhat elő, ha azokat a egyetlen MobileServicePushFailedException be vannak csoportosítva.  Minden egyes hiba külön-külön kezelni.

##<a name="#customapi"></a>Egy egyéni API használata

Egy egyéni API lehetővé teszi, hogy az egyéni végpontok, amely az jelenítik meg a kiszolgáló funkciókat, amelyek nem egy Beszúrás megfeleltetése, módosítása, törlése vagy olvassa el a művelet meghatározása. Egy egyéni API segítségével is további beállítási lehetőségekre üzenetküldés, többek között az olvasási és állítsa a HTTP üzenetfejlécek és definiáló JSON kívül üzenet törzsén formátumot.

Egy egyéni API hívja fel az [InvokeApiAsync] módszerekkel az ügyfélgépen hívható meg. A következő sort a kód például bejegyzés kérést küld a **completeAll** API a kódmentes meg:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Ezen az űrlapon beírt módszer hívást, és csak a, hogy a **MarkAllResult** visszatérési típus van megadva. Gépelt és a típusos módszerek támogatottak.

##<a name="authentication"></a>Felhasználók hitelesítő

Mobile-alkalmazások támogatja a hitelesítése és különböző, külső Identitásszolgáltatók használata app felhasználói engedélyezése: Facebook, a Google, a Microsoft Account, a Twitteren és a Azure Active Directory. Engedélyeket állíthat be a meghatározott műveletekhez csak hitelesített felhasználóknak a hozzáférés korlátozására táblázatokban. Hitelesített felhasználói azonosítója segítségével engedélyezési szabályok megvalósítását a kiszolgálóoldali parancsfájlokat. További tudnivalókért lásd: az oktatóprogram [az alkalmazás hozzáadása a hitelesítés].

Két hitelesítési flow támogatottak: _ügyfél által kezelt_ és a _kiszolgáló által kezelt_ áramlás. A kiszolgáló által kezelt folyamat nyújt a legegyszerűbb hitelesítési változat, a szolgáltató webes hitelesítési felületén támaszkodik. Ahogy azt támaszkodik szolgáltatófüggő eszköz-specifikus SDK mélyreható-integráció a eszköz nyelvspecifikus funkciókat az ügyfél által kezelt áramlás tesz lehetővé.

>[AZURE.NOTE] Azt javasoljuk, hogy egy ügyfél által kezelt folyamat használata a termelési-alkalmazásokat.

Hitelesítés beállítása regisztrálnia kell az alkalmazást az egy vagy több Identitásszolgáltatók.  A identitásszolgáltató létrehoz egy ügyfél-azonosító, és az alkalmazás egy ügyfél titkos.  Ezeket az értékeket a kódmentes a majd állítsa a hitelesítési/engedélyezési Azure alkalmazás szolgáltatás engedélyezéséhez.  További információt az oktatóprogram során [az alkalmazás hozzáadása a hitelesítés]hajtsa végre a részletes útmutatást.

Ebben a szakaszban az alábbi témakörök közül:

+ [Ügyfél által kezelt hitelesítés](#clientflow)
+ [Kiszolgáló által kezelt hitelesítés](#serverflow)
+ [A hitelesítési jogkivonat gyorsítótárazás](#caching)

###<a name="clientflow"></a>Ügyfél által kezelt hitelesítés

Az alkalmazás független fordulhat a identitásszolgáltató, és majd küldje el a visszaadott jogkivonat bejelentkezés során a kódmentes. Ebben az ügyfél adatfolyamban lehetővé teszi, hogy egy egyszeri bejelentkezéses megoldást adnak vagy további felhasználói adatok visszakeresése a identitásszolgáltató. Ügyfél továbbításához hitelesítés az elsődleges, a kiszolgáló továbbításához használatával, a identitásszolgáltató SDK tartalmaz egy további natív UX működését, és lehetővé teszi, hogy további testreszabási.

Példák a következő ügyféleszközök-adatfolyam hitelesítési mintázatok áll rendelkezésre:

+ [Az Active Directory Authentication Library](#adal)
+ [Facebook vagy a Google](#client-facebook)
+ [Élő SDK](#client-livesdk)

#### <a name="adal"></a>Az Active Directory Authentication Library rendelkező felhasználók hitelesítő

Az Active Directory hitelesítési tár (ADAL) segítségével elindíthatja a felhasználói hitelesítés, az ügyféltől az Azure Active Directory-hitelesítés használatával.

1. A mobilalkalmazásban kódmentes az AAD bejelentkezéshez konfigurálása az [Active Directory jelentkezzen be az alkalmazás szolgáltatás konfigurálása] oktatóprogram követve. Ellenőrizze, hogy a natív ügyfélalkalmazás vételének nem kötelező lépés befejezéséhez.
2. Visual Studio vagy Xamarin Studio, nyissa meg a projekt és mutató hivatkozás hozzáadása a `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet csomagot. A keresés előzetes verzió tartalmazza.
3. Az alkalmazás szerint a platformot használja fel a következő kódot. Mindegyik győződjön meg az alábbi javaslatok:

    * **Beszúrás-szervezet – itt** cserélje le a bérlő, amelyben az alkalmazás kiépítéstől nevét. A formátum https://login.windows.net/contoso.onmicrosoft.com kell lennie. Ez az érték az [Azure klasszikus portál]az Azure Active Directory tartományi lapjának másolható.
    * **Beszúrás – erőforrás-azonosító – Itt** cserélje le a mobilalkalmazásban kódmentes az ügyfél-azonosító. Az ügyfél-azonosító a **Speciális** lapon az **Azure Active Directory-beállításai** a portálon szerezhet be.
    * **Beszúrás-ügyfél-azonosító – Itt** cserélje le az ügyfél-azonosító másolta a natív ügyfele alkalmazásból.
    * **Beszúrás – ÁTIRÁNYÍTÁS-URI-itt** cserélje le a webhely _/.auth/login/done_ végpontot, a HTTPS rendszert használ. Ezt az értéket kell _https://contoso.azurewebsites.net/.auth/login/done_hasonló.

    A kód platformokon szükséges a következőképpen:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="client-facebook"></a>Egyszeri bejelentkezés a Facebook vagy a Google egy token használatával

Az ügyfél áramlás is használhatja, Facebook vagy a Google-e kódtöredékének látható módon.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="client-livesdk"></a>Egyszeri bejelentkezés Microsoft Account használata a Live SDK

A program hitelesíti a felhasználókat, regisztrálnia kell az alkalmazás, a Microsoft-fiók Developer Center. Állítsa be a regisztráció részletei a mobilalkalmazás kódmentes. Hozzon létre egy Microsoft-fiók regisztrálása, és csatlakoztassa a mobilalkalmazás kódmentes, hajtsa végre [az alkalmazást egy Microsoft-fiókkal bejelentkezni használandó]nyilvántartásban. Ha az alkalmazás Windows áruház és a Windows Phone 8/Silverlight verziója, regisztráljon először a Windows áruházból verziót.

A következő kódot hitelesíti Live SDK használatával, és a visszaadott jogkivonat segítségével jelentkezzen be az-a mobilalkalmazás kódmentes.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

További információ a [Windows Live SDK] dokumentációjában olvasható.

###<a name="serverflow"></a>Kiszolgáló által kezelt hitelesítés

Miután regisztrálta a identitásszolgáltató, hívja fel a [MobileServiceClient] [LoginAsync] módszer a szolgáltató [MobileServiceAuthenticationProvider] értékével. A következő kódot például egy kiszolgáló folyamat bejelentkezés kezdeményez, a Facebook szolgáltatásból.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Ha nem a Facebook-identitásszolgáltató használata esetén a értékének módosítása [MobileServiceAuthenticationProvider] értékre tartozó.

A kiszolgáló folyamat Azure alkalmazás szolgáltatás jelennek meg a bejelentkezési lapja a kijelölt szolgáltató a OAuth hitelesítési folyamat kezeli.  Miután a identitás szolgáltató által visszaadott, Azure alkalmazás szolgáltatás létrehoz egy alkalmazás szolgáltatás hitelesítési jogkivonat. A [LoginAsync] módszer egy [MobileServiceUser], amely magában foglalja a [felhasználóazonosító] a hitelesített felhasználó, mind a [MobileServiceAuthenticationToken]JSON webes jogkivonat (JWT), adja eredményül. A token is gyorsítótárazott, és újra felhasználni, amíg a lejárna. További tudnivalókért olvassa el a [gyorsítótárazás a hitelesítési jogkivonat](#caching)című témakört.

###<a name="caching"></a>A hitelesítési jogkivonat gyorsítótárazás

Bizonyos esetekben a hívást a bejelentkezési módszer elkerülhető az első sikeres hitelesítés után a szolgáltató a hitelesítési jogkivonat tárolásához.  A Windows áruházból, és UWP alkalmazások [PasswordVault] segítségével az aktuális hitelesítési jogkivonat gyorsítótár egy sikeres bejelentkezés után, az alábbi képlettel történik:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

A felhasználóazonosító érték tárolása a hitelesítő adatok felhasználónév és a token tárolt, a jelszót. A későbbi kezdő vállalkozások számára jelölje be a **PasswordVault** gyorsítótárban tárolt hitelesítő adatokat. Az alábbi példában gyorsítótárban tárolt hitelesítő adatok találhatók, és egyéb próbál meg ismét a fájlok:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

A felhasználó kijelentkezéskor is el kell távolítania a tárolt hitelesítő adatokat, az alábbi képlettel történik:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

Xamarin alkalmazások [Xamarin.Auth] API segítségével biztonságosan hitelesítő adatainak tárolása a **fiók** objektumban. A következő API-khoz használatára példát a [ContosoMoments képmegosztó minta] [AuthStore.cs] kód fájl megtekintéséhez.

Ügyfél által kezelt hitelesítés használatakor gyorsítótárba helyezi a hozzáférési jogkivonat, például a Facebook vagy Twitter szolgáltatójától kapott. Ez a token is kell juttatni egy új hitelesítési jogkivonat kérése a kódmentes, az alábbi képlettel történik:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="pushnotifications"></a>Leküldéses értesítések

A következő témakörök a leküldéses értesítések terjed ki:

* [Regisztrálás a leküldéses értesítések](#register-for-push)
* [A Windows áruházból csomag biztonsági azonosító beszerzése](#package-sid)
* [Regisztráció a platformok sablonok](#register-xplat)

###<a name="register-for-push"></a>Útmutató: Regisztrálás a leküldéses értesítések

A Mobile-alkalmazások ügyfél lehetővé, hogy a leküldéses értesítéseket, az Azure értesítés hubok regisztrálni. Amikor regisztrál, fog kapni fog kapni a a platform-specifikus leküldéses értesítést szolgáltatás (PNS) egy leíró. Ezután meg kell adnia ezt az értéket, címkéket és a regisztrációs létrehozásakor. A következő kódot regisztrálja a leküldéses értesítések Windows-alkalmazás a Windows értesítési szolgáltatás (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Ha vannak terjesztése, hogy WNS, majd meg kell [biztonsági AZONOSÍTÓK a Windows áruházból csomagot](#package-sid).  További információt a Windows-alkalmazásokat, például hogy miként regisztrálhat sablon regisztrációk olvassa el a [Hozzáadás leküldéses értesítéseket, hogy az alkalmazás]című témakört.

Az ügyféltől a címkék kérő nem támogatott.  Címke kérések csendes megszakadnak a regisztráció.
Ha szeretné az eszközön regisztrálása címkék, hozzon létre egy egyéni API az Ön nevében a regisztráció elvégzéséhez az értesítési hubok API-t használó.  [Az egyéni API](#customapi) helyett a `RegisterNativeAsync()` módot.

###<a name="package-sid"></a>Útmutató: a Windows áruházból csomag biztonsági azonosító beszerzése

Leküldéses értesítések Windows áruház-alkalmazás engedélyezése egy csomag biztonsági AZONOSÍTÓK van szükség.  Szeretne kapni a biztonsági AZONOSÍTÓK csomagolása, regisztrálja az alkalmazást a Windows áruházból.

Ez az érték beszerzése:

1. A Visual Studio megoldás Intézőben kattintson a jobb gombbal a Windows Áruházbeli alkalmazás projektet, kattintson a **tár** > **... Áruházzal társítása alkalmazást**.
2. A varázslóban kattintson a **Tovább**gombra, jelentkezzen be Microsoft-fiókjával, **Foglalás új alkalmazás nevét**írja be az alkalmazás nevét, majd kattintson a **Foglalás**parancsra.
3. Az alkalmazás regisztrációs sikeres létrehozását követően jelölje be az alkalmazás nevére, kattintson a **Tovább**gombra, és válassza a **társítani**.
4. Jelentkezzen be a Microsoft Account [Windows fejlesztői központ] . **Saját alkalmazások**csoportban kattintson a létrehozott alkalmazás regisztráció.
5. Kattintson az **alkalmazás management** > **alkalmazás identitás**és majd lefelé görgetve keresse meg a **Csomag biztonsági AZONOSÍTÓK**.

A csomag biztonsági AZONOSÍTÓK számos felhasználási tekinti azt URI, ebben az esetben a használni kívánt _ms-alkalmazás: / /_ , a rendszer. Jegyezze fel a biztonsági AZONOSÍTÓK ezt az értéket, mint egy előtagot összefűzésével formázott csomag verziójának.

Xamarin alkalmazások csak néhány további kód engedélyezni szeretné, hogy regisztráljon az iOS és Android platformokon futó alkalmazást. További információ a platform az témakörben:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="register-xplat"></a>Útmutató: külső.FÜGGV leküldéses sablonok platformok értesítések küldésére

Sablonok regisztrálásához használhatja a `RegisterAsync()` módszer sablonokat, az alábbiak szerint:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

A sablonok kell `JObject` típusok és is tartalmazhat, az alábbi formátumban JSON több sablonok:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

A módszer **RegisterAsync()** is fogadja el a másodlagos Csempék:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Az összes címke hivatkozásból biztonsági regisztráció során nem vagyok a gépnél vannak. Címkék hozzáadása telepítéseihez vagy sablonok telepítéseihez belül, a [munka a .NET kódmentes kiszolgálóval SDK Azure Mobile-appokról] című témakör tartalmaz.

Ezek regisztrált a sablonok felhasználásával értesítéseket küldeni, tanulmányozza az [Értesítési hubok API -khoz].

##<a name="misc"></a>Egyéb témaköröket

###<a name="errors"></a>Útmutató: hibák kezelése

Ha hiba történik a kódmentes, az ügyfél SDK hatványra egy `MobileServiceInvalidOperationException`.  A következő példa bemutatja a kódmentes által visszaküldött kivételt kezelése:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Hiba feltételek foglalkozik egy másik példa a [Mobile alkalmazások fájlok minta]találhatók. A [LoggingHandler] példa egy naplózás meghatalmazott kezelő (következő) a kódmentes történik a kérések naplózását biztosít.

###<a name="headers"></a>Útmutató: testreszabása fejlécek kérése

Az adott alkalmazás forgatókönyvet támogatja, szükség lehet a mobilalkalmazás kódmentes való kommunikáció testreszabása. Például érdemes egyéni élőláb hozzáadása minden kimenő kérelem vagy akár válaszok állapot kódok. Egy egyéni [DelegatingHandler], ahogy a következő példában is használhatja:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Hitelesítés felvétele az alkalmazásba]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Kapcsolat nélküli adatok szinkronizálása az Azure mobilalkalmazások]: app-service-mobile-offline-data-sync.md
[Leküldéses értesítések felvétele az alkalmazásba]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Regisztráljon az alkalmazást egy Microsoft-fiókkal bejelentkezni használata]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hogyan alkalmazás szolgáltatás konfigurálása az Active Directory bejelentkezés]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[típusossá egy hivatkozást hoz létre]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Jegyzetelés]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Válassza a]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Kihagyása]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Felhasználóazonosító]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[Ha]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure portál]: https://portal.azure.com/
[Azure klasszikus portál]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[A Windows fejlesztői központ]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Értesítés hubok API-hoz]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobilalkalmazások fájlok minta]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentáció]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[ContosoMoments fénykép megosztási minta]: https://github.com/azure-appservice-samples/ContosoMoments