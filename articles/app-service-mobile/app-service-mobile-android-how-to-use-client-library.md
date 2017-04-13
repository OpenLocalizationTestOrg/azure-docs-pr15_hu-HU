<properties
    pageTitle="Az Android Mobile alkalmazások ügyfél-tár használata"
    description="Hogyan lehet Azure Mobile-alkalmazások használata Android-ügyfél SDK csomagjában talál."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Az Android-ügyfél-tár használata Mobile-appokról

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató megtudhatja, hogy miként használhatja a Android Mobile-appokról SDK esetei, végrehajtásához:

- Lekérdezés-adatok (beszúrását, frissítése és törlése).
- Hitelesítés.
- Hibák kezelése.
- Az ügyfél testreszabása. 

A legtöbb mobilalkalmazások használt közös ügyfél kódot a mély merülési is tartalmaz.

Ez az útmutató az ügyféloldali Android SDK koncentrál.  További tudnivalók a kiszolgálóoldali SDK Mobile-appokról című témakörben talál [a .NET kódmentes SDK használata] [ 10] vagy [használatáról a Node.js kódmentes SDK][11].

## <a name="reference-documentation"></a>Hivatkozási dokumentáció

A [Javadocs API hivatkozás] található[ 12] az Android-ügyfél tárának GitHub.

## <a name="supported-platforms"></a>Támogatott platformok

Az Azure Mobile alkalmazások Android SDK API szintet 24 (KitKat nugát keresztül) – 19 támogat.  

A "server-adatfolyam" hitelesítés a felhasználói felület bemutatott egy webnézet használja. Ha az eszköz nem tudja webnézet felhasználói Felületet bemutatására, más hitelesítési módszer van szükség, amely a termék hatókörén kívüli.  Ez az SDK nem alkalmas Figyelőpont típusú vagy hasonló módon korlátozott eszközöket.

## <a name="setup-and-prerequisites"></a>A telepítő és vonatkozó követelmények

A [Mobile-alkalmazások quickstart útmutató](app-service-mobile-android-get-started.md) oktatóprogram elvégzéséhez.  Ebben a feladatban biztosítja, hogy teljesülnek-e minden előtti követelmény Azure Mobile-alkalmazások fejlesztésével.  A quickstart útmutató is segítségével konfigurálhatja a fiókot, és az első mobilalkalmazás kódmentes létrehozása.

Ha úgy dönt, nem a quickstart útmutató oktatóprogram elvégzéséhez, az alábbi feladatok elvégzésére:

- [Hozzon létre egy mobilalkalmazás kódmentes] [ 13] használata az Android-alkalmazást.
- Az Android Studio [build Gradle fájlok frissítése](#gradle-build).
- [Engedélyezze az internet engedéllyel](#enable-internet).

###<a name="gradle-build"></a>Frissítés a Gradle fájl összeállításához

Mindkét **build.gradle** fájlok módosítása:

1. Ez a kód hozzáadása *a *buildscript* címkére szintű **build.gradle** projektfájl* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Ez a kód hozzáadása a *modul alkalmazás* szintű **build.gradle** fájlt a *függőségeket* címkére:

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    A legújabb verzióra jelenleg 3.1.0. A verziók jelennek meg [az alábbi][14].

###<a name="enable-internet"></a>Internetes jogosultsági engedélyezése

Azure eléréséhez az alkalmazás engedéllyel kell rendelkeznie az internetes engedélyezve van. Ha még nincs engedélyezve, a következő sort a kód hozzáadása a **AndroidManifest.xml** fájlhoz:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Az alapvető mély merülési

Ez a szakasz vázolja vonatkozik az Azure Mobile-alkalmazások használata a quickstart útmutató alkalmazásban a kódot.  

###<a name="data-object"></a>Ügyfél adatok osztályok meghatározása

SQL Azure-táblából származó adatok eléréséhez ügyfél adatok osztályok, amelyek megfelelnek a mobilalkalmazás kódmentes táblázataira határozza meg. Példák a jelen témakör tegyük fel, **ToDoItem**, amely mellett az alábbi oszlopokat nevű tábla:

- azonosító
- szöveg
- befejezése

A megfelelő beírt ügyféloldali objektum:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

A kód **ToDoItem.java**nevű fájlban található.

Ha az SQL Azure-táblából több oszlopot tartalmaz, vegyen fel a megfelelő mezőket az osztály.  Ha például ha a DTO (adatok átviteli objektumot) kellett egy egész szám prioritás oszlop, akkor előfordulhat, hogy ez a mező annak lekérdező és beállító metódus módszerek hozzáadása:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

További táblák létrehozása az a Mobile-alkalmazások kódmentes című témakörben talál [hogyan: megadása egy táblázat vezérlő] [ 15] (.NET kódmentes) vagy a [táblázatok definiálása dinamikus sémával] [ 16] (Node.js kódmentes). Egy Node.js kódmentes a az [Azure portálon]is használhatja a **könnyen táblák** beállítást.

###<a name="create-client"></a>Útmutató: a ügyfélkörnyezet létrehozása

Ez a kód használatával érheti el a mobilalkalmazás kódmentes **MobileServiceClient** objektum hoz létre. Ugrás a kódot a `onCreate` olyan **fő** művelet és az **ALKALMAZÁSINDÍTÓ** kategória *AndroidManifest.xml* megadott **tevékenység** osztály metódusát. Quickstart útmutató kódban kerül a **ToDoActivity.java** fájlban.

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

Ez a kód cserélje le `MobileAppUrl` a mobilalkalmazás kódmentes URL-címmel, amely megtalálható a az [Azure portálon] a lap esetében a mobilalkalmazás kódmentes. Az ebben a sorban összeállításához kód is kell hozzáadnia az alábbi **importálása** utasítás:

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Útmutató: táblázat hivatkozás létrehozása

A lekérdezés vagy a kódmentes adatainak módosítása legegyszerűbben a *beírt programozási modell*, mivel Java erősen beírt nyelvek használatával.. Ez a modell nyújt zökkenőmentes JSON szerializálási és használatáról a [gson] deszerializálás[ 3] a háttér Azure SQL-kiszolgálói ügyfél-objektumok és a táblák közötti adatok küldésekor tárban.

Táblázat használatához először hozzon létre egy [MobileServiceTable] [ 8] objektum hívja fel a **getTable** módszer a [MobileServiceClient][9].  Ez a módszer két túlterhelések foglalja magában:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

A következő kódrészlet **mClient** a MobileServiceClient objektumra mutató hivatkozás.  Az első információval szolgál, ahol az a osztály nevét és a táblázat neve megegyeznek és az egyik használják a quickstart Útmutató:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

A második információval szolgál, ha a táblázat neve eltér az osztálynév: az első paraméterként áll a tábla neve.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Útmutató: adatok kötni a felhasználói felület

Adatok kötés három összetevő foglalja magában:

- Az adatforrás
- A képernyő-elrendezés
- A kártya, amely a két közös kötelékek.

A minta kódban azt vissza az adatokat a Mobile alkalmazások SQL Azure-táblából **ToDoItem** tömbképletet be. Ez a tevékenység adatok alkalmazások közös mintájának.  Az adatbázis-lekérdezések gyakran vissza, amely az ügyfél kapja a listában vagy a tömb sorainak gyűjteménye. Az ebben a példában a tömb az adatforrásban.

A kód, amely definiálja az adatokat, a megjelenő nézete az eszközön képernyő elrendezés határozza meg.  A két kötött-adaptert, amely kód bővítménye együtt, a **ArrayAdapter&lt;ToDoItem&gt; ** osztály.

#### <a name="layout"></a>Útmutató: elrendezésének megadása

Az elrendezés XML-kódot több kódrészletek határozza meg. Egy meglévő elrendezések megadva, az alábbi kódot szeretnénk feltöltése a kiszolgáló adatokat tartalmazó **Listanézet** jelöli.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Az előző kód a *listitem* attribútum Megadja, hogy a listában az egyes sorok elrendezésének azonosítója. Ez a kód megadja egy jelölőnégyzetet, és a hozzá tartozó szöveg és egyszer kap megjelenítésre a minden elemet. Ebben az elrendezésben nem jelenít meg az **azonosító** mezőt, és további, összetettebb elrendezés volna meg a további mezők jelennek meg a. Ez a kód a **row_list_to_do.xml** fájl található.

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Útmutató: a kártya megadása

Mivel az adatforrás a nézet **ToDoItem**, tömbje azt alosztály a adaptert egy **ArrayAdapter&lt;ToDoItem&gt; ** osztály. Ez a alosztály nézet minden **ToDoItem** a **row_list_to_do** elrendezésben az hoz létre.

A saját kód azt határozza meg, amely a kiterjesztése a következő osztály a **ArrayAdapter&lt;E&gt; ** osztály:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

A kártyák **getView** módszer felülbírálása. Példa:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Hozzunk létre egy példánya az osztály a tevékenység az alábbi képlettel történik:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

A második a ToDoItemAdapter konstruktor paraméter elrendezését mutató hivatkozás. Hogy most hozható létre a **Listanézet** és a kártya hozzárendelése a **listanézetben**.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>Az API-struktúra

Aszinkron hogy a Mobile-alkalmazások táblázat műveleteket és egyéni API-hívások. Az aszinkron módszerek érintő lekérdezéseket, beszúrása, frissítések és törlése a [jövőbeli] és [AsyncTask] objektumok használata. Határidő használatával könnyebben végezze el a háttér szálon több műveletet anélkül, hogy több beágyazott visszahívás foglalkozik.

Tekintse át a **ToDoActivity.java** fájlt az Android quickstart útmutató projektben az [Azure portál] példát.

#### <a name="use-adapter"></a>Útmutató: a kártya használata

Most már készen áll adatok kötés használni. A következő kódot elemek beszerzése a táblázatban látható, és a helyi kártya beírja a visszaadott elemek.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Hívja fel a kártya bármikor módosíthatja a **ToDoItem** táblában. Mivel a módosításokat végzett egy rekord által rekord alapon, kezelheti a helyett egy webhelycsoport egyetlen sor. Elemek beszúrásakor hívás metódus **hozzáadása** a kártyán; törlésekor, hívja fel a **eltávolítása** módszerrel.

##<a name="querying"></a>Útmutató: a mobilalkalmazás kódmentes adatainak lekérdezése

Ez a szakasz ismerteti a lekérdezések ki a mobilalkalmazás kódmentes, amely tartalmazza az alábbi műveleteket:

- [Az összes elemek megjelenítése eredményként]
- [Visszaadott adatok szűrése]
- [A visszaadott adatok rendezése]
- [Lépjen vissza az adatokat a lapokon]
- [Egyedi oszlopok kijelölése]
- [ÖSSZEFŰZ lekérdezési módszerek](#chaining)

### <a name="showAll"></a>Útmutató: lépjen vissza az összes elem törlése egy táblázatról

Az alábbi lekérdezés összes elem a **ToDoItem** táblázatban adja eredményül.

    List<ToDoItem> results = mToDoTable.execute().get();

Az *eredmények* változó a listában a lekérdezésből az eredményhalmaz adja eredményül.

### <a name="filtering"></a>Útmutató: szűrés visszaadott adatok

A következő lekérdezés-végrehajtási adja eredményül elemeket a **ToDoItem** táblából hol **teljes** eredménye **hamis**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** a mobilszolgáltatás táblázat, amely a korábban létrehozott hivatkozik.

Adja meg a **Hol** módszer hívás használata a táblázathivatkozásokban szűrő. A **Hol** módszert követ, amely meghatározza a logikai predikátumok módszer **mező** módszerrel követik. Lehetséges predikátumalakzat módszerek például **eq** (egyenlő), a **ne** (nem egyenlő), a **gt** (nagyobb, mint), a **ge** (nagyobb vagy egyenlő), a **lt** (kisebb, mint), **le** (kisebb vagy egyenlő). Ezeket a módszereket adhatja meg a számot és a karakterlánc adott értékek mezők összehasonlítása.

Szűrheti a dátumokat. Az alábbi módszerek segítségével összehasonlíthatja a teljes dátummező vagy a dátum részei: **év**, **hónap**, **nap**, **órát**, **percet**és **második**. Az alábbi példa összeadja az elemek, amelyek a *határidő* egyenlő 2013 szűrő.

    mToDoTable.where().year("due").eq(2013).execute().get();

Az alábbi módszerek a karakterlánc-mezők támogatja az összetettebb szűrők: **startsWith**, **endsWith**, **összefűzés**, **karakterlánc**, **indexOf**, **cseréje**, **toLower**, **toUpper**, **vágása**és **hossza**. A következő példa szűrők Táblázatsorok, ahol a *szövegoszlop* kezdődik "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

Numerikus mezők az alábbi operátorok módszerek támogatottak: **adja hozzá**, **sub**, **MUL számú**, **div**, **mod**, **padló**, **plafon**és **kerekíteni**. A következő példa szűrők táblázatsorok hol található az **időtartamot** a páros szám.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Logikai módszerekben predikátumok kombinálhatja: **és**, **illetve** , és **nem**. Ez a példa két az előző példában egyesíti.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Csoportosítás és a függvények egymásba logikai operátorokat:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Részletesebb és szűrés Példák című témakörben talál [a összetettségű Android ügyfél lekérdezés modell feltárása](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Útmutató: Rendezés visszaadott adatok

A következő kódot a **ToDoItems** táblázat összes elemet növekvő sorrend szerint *a szövegmezőben* adja eredményül. *mToDoTable* az imént létrehozott kódmentes táblázat a hivatkozás:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Az első **orderBy** metódus paraméter egy karakterlánc egyenlő a rendezni kívánt mezőt a nevére. A második paraméter a **QueryOrder** felsorolás segítségével adja meg, hogy növekvő vagy csökkenő sorrendbe rendezheti.  A ***Hol*** módszerrel szűr, ha a ***Hol*** módszer a ***orderBy*** metódus előtt kell hívható meg.

### <a name="paging"></a>Útmutató: adatok visszatérési a lapokon

Az első példa bemutatja, hogyan jelölje ki az első öt elem egy táblában. A lekérdezés az elemek **ToDoItems**táblázatát adja eredményül. **mToDoTable** az imént létrehozott kódmentes táblázat a hivatkozás:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Íme egy lekérdezést, amely az első öt elemek átugrása, és a következő öt adja vissza:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Útmutató: egyedi oszlopok kijelölése

A következő kódot szemlélteti, hogyan visszatérni az összes elem **ToDoItems**táblázatát, de csak jeleníti meg a **teljes** és a **szöveg** típusú mezők. **mToDoTable** a kódmentes táblázat, amely a korábban létrehozott hivatkozik.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

A select függvény paraméterei vissza szeretné Táblázatoszlopok azoknak a karakterlánc.

**Válassza a** módszer kell hajtsa végre a módszereket, például a **Ha** és a **orderBy**. Lapozás módszerek, például a **felső**követhetnek.

### <a name="chaining"></a>Útmutató: lekérdezési módszerek összefűzése

Lekérdezés kódmentes táblák módszerre is fűzhetők. Lekérdezési módszerek láncolás lehetővé teszi, jelölje be a szűrt sorokat, amelyeket rendezett és lapozható adott oszlopainak. Összetett logikai szűrők hozhat létre.
A lekérdezés módszer egy lekérdezés objektum adja eredményül. Módszerek a sorozat befejezéséhez, és a ténylegesen futtatja a lekérdezést, hívja fel a metódus **végrehajtása** . Példa:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

A lekérdezési láncolt módszerek a következőképpen kell rendelni:

1. Szűrés módszer (**Amennyiben**).
2. Rendezési módszereket (**orderBy**).
3. (**Válassza a**) kijelölési módszereket.
4. lapozás (**ugorja át** és **felső**) módszerek.

##<a name="inserting"></a>Útmutató: adatok beillesztése a kódmentes

Hozható létre a *ToDoItem* osztály egy példányát, és adja meg annak tulajdonságait.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

**Insert()** segítségével objektum beszúrása:

    ToDoItem entity = mToDoTable.insert(item).get();

A szervezet visszaadott találatok az azonosító, és minden más érték a kódmentes meg az adatokat a kódmentes tábla beszúrt tartalmazza.

Mobile-alkalmazások táblák neve **id**elsődleges kulcsoszlop szükség. Alapértelmezés szerint ez az oszlop egy olyan karakterlánc. Az azonosító oszlopot alapértelmezett értéke egy globálisan egyedi azonosítója.  Megadhatja, hogy más egyedi értékek, például az e-mail címeket vagy felhasználónevét. Egy beszúrt rekord nem szerepel a karakterlánc-azonosító érték, a kódmentes hoz létre egy új globálisan egyedi azonosítója.

Karakterlánc azonosító értékét adja meg a következő előnyökkel jár:

+ Azonosítók anélkül, hogy egy üzenetváltási az adatbázis hozhatók létre.
+ A rekordok egyesítése különböző táblákból vagy adatbázisok könnyebben fognak.
+ Azonosító értékét jobban integrálása az alkalmazások összefüggés.

Karakterlánc azonosító értékei **szükséges** kapcsolat nélküli szinkronizálás ügyfélszolgálatával.

##<a name="updating"></a>Útmutató: adatok mobilalkalmazásban frissítése

A táblázatban lévő adatok frissítéséhez adják át az új objektum a **update()** módszerrel.

    mToDoTable.update(item).get();

*Ebben a példában az elem egy-egy sor a *ToDoItem* táblázat néhány módosításainak volt mutató hivatkozás.*
Az **azonosító** egy sor frissül.

##<a name="deleting"></a>Útmutató: adatok mobilalkalmazásban törlése

A következő kódot megtudhatja, hogy miként adatok törlése egy táblázatból a adatobjektumok megadásával.

    mToDoTable.delete(item);

Az **azonosító** mezőt a sor törlése megadásával is töröl egy elemet.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Útmutató: adott elem keresése

Egyedi **azonosító** mezőt tartalmazó elem keresése **lookUp()** módszer:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Útmutató: Típusos adatokkal végzett munkához

A típusos programozási modell lépve JSON szerializálási pontosan szabályozható.  Létezik néhány gyakori alkalmazási területek hol lehet használni kívánt egy típusos programozási modell. Ha például a kódmentes táblázat sok oszlopot tartalmaz, és csak akkor van szüksége az oszlopok csak egy részhalmazát hivatkozni szeretne.  A beírt modell megköveteli a mobilalkalmazások táblázat összes oszlopok meghatározása az adatok osztályához.  

A legtöbb eléréséhez API-hívások hasonlóak a beírt programozási hívások. A fő különbség, hogy a típusos modell, meghívása **MobileServiceJsonTable** objektumon helyett a **MobileServiceTable** objektum módszereket.

### <a name="json_instance"></a>Útmutató: típusossá példányának létrehozása

A beírt modell hasonló, indítsa el az első tábla hivatkozást, de ebben az esetben egy **MobileServicesJsonTable** objektum. A hivatkozás beszerzése hívja fel a **getTable** módszer az ügyfél-példány:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Miután létrehozott egy példányát a **MobileServiceJsonTable**, rajta gyakorlatilag az azonos API-val és a beírt programozási modellt, amelyekhez. Bizonyos esetekben a módszereket, hogy egy Típusos paraméter beírt paraméter helyett.

### <a name="json_insert"></a>Útmutató: típusossá beillesztése

A következő kódot végezze el a beszúrási mutatja. Az első lépésként hozzon létre egy [JsonObject][1], amely a [gson] része[ 3] tárat.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Ezután **insert()** segítségével típusos objektum beszúrásához a táblázatba.

    mJsonToDoTable.insert(jsonItem).get();

Ha a beszúrt objektum ID azonosító beszerzése kell módszert a **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Útmutató: típusossá törlése

A következő kódot megtudhatja, hogy miként példány törlése, ebben az esetben az előzetes *beszúrása* példában létrehozott **JsonObject** ugyanazt a példányát. A kód ugyanaz, mint a beírt esethez, de a módszer különböző aláírások rendelkezik, mivel az egy **JsonObject**hivatkozik.

         mToDoTable.delete(jsonItem);

Egy példány a Azonosítójával felhasználásával közvetlenül is törölheti:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Útmutató: minden sor bevétel a típusossá

A következő kód beolvasása a teljes táblázat mutatja be. Mivel a JSON táblázat használja, szelektív meghallgathatja csak egy részét a táblázatok oszlopait.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Ugyanazok a szűrést, szűrés és a beírt modell elérhető módszerek személyhívó elérhetők a típusos modellben üléstermet is.

##<a name="custom-api"></a>Útmutató: egyéni API-val hívása

Egy egyéni API lehetővé teszi, hogy az egyéni végpontok, amely az jelenítik meg a kiszolgáló funkciókat, amelyek nem egy Beszúrás megfeleltetése, módosítása, törlése vagy olvassa el a művelet meghatározása. Egy egyéni API segítségével is további beállítási lehetőségekre üzenetküldés, többek között az olvasási és állítsa a HTTP üzenetfejlécek és definiáló JSON kívül üzenet törzsén formátumot.

Android-ügyfélről hívja fel a **invokeApi** módszer, ha fel szeretne hívni az egyéni API-végpontot. A következő példa bemutatja, hogyan hívás API zárólap nevű **completeAll**, amely **MarkAllResult**nevű webhelycsoport osztály adja vissza.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

A **invokeApi** módszer az ügyfélszámítógépen, amely POST kérelem küld az új egyéni API neve. Az egyéni API által visszaadott eredménye megjelenik egy üzenet párbeszédpanel, a hibák. Más verzióit **invokeApi** segítségével tetszés szerint objektum küldése összehívás törzsébe, a HTTP-módszer és a lekérdezés paraméterei kérelem elküldése. **InvokeApi** típusos verziója is találhatók.

##<a name="authentication"></a>Útmutató: hitelesítés felvétele az alkalmazásba

Oktatóanyagok már részletesen leírja a következőképpen adhatja meg ezeket a funkciókat.

Alkalmazás szolgáltatás lehetővé teszi, hogy, [app felhasználói hitelesítő](app-service-mobile-android-get-started-users.md) számos külső Identitásszolgáltatók használata: Facebook, a Google, a Microsoft Account, a Twitteren és a Azure Active Directory. Engedélyeket állíthat be a meghatározott műveletekhez csak hitelesített felhasználóknak a hozzáférés korlátozására táblázatokban. A hitelesített felhasználók azonosítása engedélyezési szabályok megvalósítását a a kódmentes is használhatja.

Két hitelesítési flow támogatottak: egy **kiszolgáló** továbbításához és egy **ügyfél** továbbításához. A kiszolgáló folyamat nyújt a legegyszerűbb hitelesítési felület identitás szolgáltatók webes felületén támaszkodik.  Nincsenek további SDK kiszolgáló folyamat hitelesítési végrehajtásához szükséges. Kiszolgáló folyamat hitelesítése nem tartalmaz a mély integrálása a mobileszközön, és fogalmat esetek igazolása csak ajánlott.

Az ügyfél áramlás lehetővé teszi, hogy az eszköz-specifikus lehetőségek, például az egyszeri bejelentkezés mélyreható integrációja, ez az identitás-szolgáltató által biztosított SDK támaszkodik.  A Facebook-SDK integrálhatja például a mobil alkalmazásba.  A mobil ügyfélprogram felcseréli a Facebook-alkalmazásba, és a bejelentkezés előtt a mobilalkalmazásban való lecserélése nyugtázza.

Négy lépésből ahhoz, hogy az alkalmazás-hitelesítés van szükség:

- Regisztráljon az alkalmazás hitelesítéshez tel.
- Állítsa be az alkalmazás szolgáltatás kódmentes.
- Csak az alkalmazás szolgáltatás kódmentes a hitelesített felhasználók táblában engedélyek korlátozása.
- Hitelesítés kód hozzáadása az alkalmazást.

Engedélyeket állíthat be a meghatározott műveletekhez csak hitelesített felhasználóknak a hozzáférés korlátozására táblázatokban. Egy hitelesített felhasználó biztonsági azonosítója segítségével kérések módosítása.  További tudnivalókért tekintse át az [első lépések a hitelesítés] , valamint a kiszolgáló SDK útmutató dokumentum.

### <a name="caching"></a>Útmutató: az alkalmazás hitelesítési kód hozzáadása

A következő kódot folyamatának egy kiszolgáló folyamat jelentkezzen be a Google-szolgáltató használata:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Szerezze be a **getUserId** módszerrel **MobileServiceUser** a bejelentkezett felhasználó azonosítója. Példát, hogyan használhatja a határidő hívja fel a aszinkron bejelentkezési API-hoz olvassa el a [hitelesítési – első lépések]című témakört.

### <a name="caching"></a>Útmutató: a gyorsítótárban hitelesítési tokenek

Hitelesítési tokenek gyorsítótárazás megköveteli a felhasználói azonosító, és a hitelesítési jogkivonat a helyben történő tárolás az eszközön. A következő alkalommal, amikor elindítja az alkalmazást, akkor ellenőrizze a gyorsítótár, és ha ezeket az értékeket, ugorja át a napló eljárás, és ezeket az adatokat az ügyfél rehydrate. Azonban ezeket az adatokat bizalmas, és azt kell tárolja titkosítva biztonsági abban az esetben, ha a telefon biztosítása.

Megjelenik egy teljes példát, hogy miként gyorsítótár hitelesítési tokenek [gyorsítótár hitelesítési tokenek]szakasz[7].

Egy lejárt jogkivonat használatakor kap egy *401 jogosulatlan* választ. Szűrők használata hitelesítési hibákat képes kezelni.  Szűrők az alkalmazás szolgáltatás kódmentes kérelmek metsz. A szűrő kód azt vizsgálja, a válasz egy 401, a bejelentkezési folyamat eseményindítók és majd folytatja a kérelmet, a 401 létrehozott. 

## <a name="adal"></a>Útmutató: az Active Directory Authentication Library rendelkező felhasználók hitelesítő

Active Directory Authentication tár (ADAL) segítségével a felhasználók jelentkezzen be az Azure Active Directory segítségével az alkalmazás. Egy ügyfél folyamat login használatával célszerű gyakran használ a `loginAsync()` formában módszerek tartalmaz egy további natív UX működését, és lehetővé teszi, hogy további testreszabási.

1. A mobilalkalmazásban kódmentes az AAD bejelentkezés konfigurálása az [Active Directory jelentkezzen be az alkalmazás szolgáltatás konfigurálása](app-service-mobile-how-to-configure-active-directory-authentication.md) oktatóprogram követve. Ellenőrizze, hogy a natív ügyfélalkalmazás vételének nem kötelező lépés befejezéséhez.

2. ADAL telepítse a build.gradle fájl mentését a következő definíciók módosítása:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Az alkalmazás, így a következő javaslatok hozzá a következő kódot:

* **Beszúrás-szervezet – itt** cserélje le a bérlő, amelyben az alkalmazás kiépítéstől nevét. A formátum https://login.windows.net/contoso.onmicrosoft.com kell lennie. Ezt az értéket a [Azure klasszikus portálon] az Azure Active Directory tartományi lapjának másolható.

* **Beszúrás – erőforrás-azonosító – Itt** cserélje le a mobilalkalmazásban kódmentes az ügyfél-azonosító. Az ügyfél-azonosító a **Speciális** lapon az **Azure Active Directory-beállításai** a portálon szerezhet be.

* **Beszúrás-ügyfél-azonosító – Itt** cserélje le az ügyfél-azonosító másolta a natív ügyfele alkalmazásból.

* **Beszúrás – ÁTIRÁNYÍTÁS-URI-itt** cserélje le a webhely _/.auth/login/done_ végpontot, a HTTPS rendszert használ. Ezt az értéket kell _https://contoso.azurewebsites.net/.auth/login/done_hasonló.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Útmutató: az alkalmazás leküldéses értesítést hozzáadása

[Olvassa el a áttekintést] is[ 6] , amely bemutatja, hogyan támogatja a Microsoft Azure értesítés hubok a számos különböző típusú leküldéses értesítéseket.  [Ebben]az oktatóanyagban[5], a leküldéses értesítést küld az összes eszköz minden alkalommal, amikor a program beszúrja a rekord.

## <a name="how-to-add-offline-sync-to-your-app"></a>Útmutató: kapcsolat nélküli szinkronizálás az alkalmazás hozzáadása

A quickstart útmutató oktatóprogram hajtja végre a kapcsolat nélküli szinkronizálás kódot tartalmazza. Keresse meg a megjegyzések előtaggal kódot:

    // Offline Sync

A következő kódsorokat uncommenting alkalmazhat kapcsolat nélküli szinkronizálás, és más Mobile-alkalmazások kódot is hozzáadhat hasonló kódot.

##<a name="customizing"></a>Útmutató: az ügyfél testreszabása

Többféle módon testre az alapértelmezett működés az ügyfél számára.

### <a name="headers"></a>Útmutató: testreszabása fejlécek kérése

Állítsa be a **ServiceFilter** minden kérés egyéni HTTP élőfej hozzáadása:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Útmutató: szerializálási testreszabása

Az ügyfél feltételezi, hogy a táblázatnevek, oszlopnevek és adatok be kell írnia a az összes megfelelően, hogy pontosan a az ügyfél meghatározott data objects kódmentes. Tetszőleges számú miért nem egyeznek a kiszolgáló és az ügyfél nevét oka lehet. Az Ön esetében érdemes lehet hajtsa végre a következő típusú testreszabása:

- Az oszlopnevek, használja az alkalmazás szolgáltatás táblában nem egyeznek a használja az ügyfél nevét.
- Egy alkalmazás szolgáltatás táblát, amely tartalmaz egy másik nevet, mint az osztály megfelelteti az ügyfélprogramban, használhatja.
- Kapcsolja be a hibás nagybetűhasználat automatikus tulajdonság.
- Összetett tulajdonságok hozzáadása objektumhoz.

### <a name="columns"></a>Útmutató: ügyfél és a kiszolgáló neve más megfeleltetése

Tegyük fel, hogy a Java-ügyfél kód használja a szabványos Java-stílus nevét a **ToDoItem** objektum tulajdonságai, például a következő tulajdonságokat:

- Közép
- mText
- mComplete
- Mkamatérz

Az ügyfél nevét szerializálható egyezni a table_expression1 táblázatkifejezésben a **ToDoItem** táblázat a kiszolgálón JSON nevekké. A következő kódot használ a [gson] [ 3] tár széljegyzetekkel lássanak el az a tulajdonságokat:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Útmutató: feleltesse meg az ügyfél és a kódmentes közötti különböző táblázatnevek

Az ügyfelek tábla neve feleltesse meg a [getTable()] felülbírálása használatával különböző mobil szolgáltatások táblázatnév[ 4] módszer:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Útmutató: oszlop kiszolgálónév-megfeleltetések automatizálása

Megadhatja, hogy a konvertálás stratégia, amely minden oszlopra vonatkozik a [gson] használatával[ 3] API-val. Az Android-ügyfél tár használja [gson] [ 3] Bepillantás a színfalak mögé szerializálni JSON adatok Java-objektumok, az adatok Azure alkalmazás szolgáltatás elküldése előtt.  A következő kódot a **setFieldNamingStrategy()** módszert használja a stratégia beállításához. Ez a példa törli az első karakter ("m"), majd kisbetűs és a következő karakter, minden mező nevét. Például azt szeretné alakítani "mId" "azonosítót."

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Ez a kód használata az **MobileServiceClient**előtt végre kell hajtani.

### <a name="complex"></a>Útmutató: az objektum vagy tömbben tulajdonságot tárolása táblába

A szerializálási példák eddig egyszerű típusokhoz, például az egészérték részt veszi figyelembe és karakterláncok van szó.  Egyszerű típusokhoz egyszerűen szerializálható JSON be.  Ha azt szeretné, amely nem automatikusan szerializálni összetett objektum felvétele JSON, adja meg a JSON szerializálási módszer szükség.  Egyéni JSON szerializálási megadására példa látható, tekintse át a blogbejegyzés [a gson tárat a mobil szolgáltatások Android ügyfélprogram használatával testreszabása szerializálási][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Az összes elemek megjelenítése eredményként]: #showAll
[Visszaadott adatok szűrése]: #filtering
[A visszaadott adatok rendezése]: #sorting
[Lépjen vissza az adatokat a lapokon]: #paging
[Egyedi oszlopok kijelölése]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portál]: https://portal.azure.com
[Első lépések a hitelesítés]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Jövőbeli]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html