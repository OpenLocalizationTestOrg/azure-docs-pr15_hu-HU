<properties
   pageTitle="Böngészési erőforrások és kezelése tároló kiszolgáló Intézővel |} Microsoft Azure"
   description="Böngészéséről és a kiszolgáló Intézővel tárolási erőforrások kezelése"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Böngészési és tároló kiszolgáló Explorer erőforrások kezelése

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>– Áttekintés
Ha telepítette az Azure-eszközök a Microsoft Visual Studio, blob, várólista, és megtekintése táblázatok adatait a tárterület-fiókjából az Azure. A kiszolgáló Explorer Azure tároló csomópontra a helyi tároló irányító fiókja és a más fiókjaiból Azure tároló adatok jeleníti meg.

Kiszolgáló Explorer a menüsor megjelenítése a Visual Studióban, megtekinteni, kattintson a **Nézet**, **Server Explorer**. A tároló csomópontot látható az összes a tárhely fiókok megtalálható az egyes Azure előfizetés/tanúsítvány kapcsolódik. Ha nem látható a tárterület-fiókját, felveheti az utasításokat követve [a témakör későbbi](#add-storage-accounts-by-using-server-explorer).

Az Azure SDK 2.7 indulva is Intézővel az új felhő megtekintése és kezelése az Azure erőforrások. [Azure erőforrások kezelése a felhőben Intézővel](./vs-azure-tools-resources-managing-with-cloud-explorer.md) talál további információt.


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Megtekintheti és kezelheti a Visual Studióban tároló-erőforrások

Kiszolgáló Explorer automatikusan megjelenik az BLOB, a sorok és a táblák listájának a tárhely irányító-fiókjában. A tároló irányító fiók van feltüntetve Server Explorer csoportban a tárhely csomópontot a **fejlesztés** csomópont.

A irányító fiók erőforrások megtekintéséhez bontsa ki a **fejlesztés** csomópontot. Ha a tárhely irányító hiába elindítva, amikor bővíti a **fejlesztés** csomópontot, automatikusan elindul. Néhány másodpercig jelképező. Munka más területeken a Visual Studio, amíg a tárhely irányító elindul is.

Tárterület-fiókjában erőforrások megtekintéséhez bontsa ki a kiszolgáló Explorer a tárterület-fiók csomópontot. A következő alszint csomópontok jelennek meg:

- BLOB

- Sorok

- Táblák

## <a name="work-with-blob-resources"></a>Az Blob-erőforrások kezelése

A BLOB csomópontot a tároló kijelölt fiók tárolók listáját jeleníti meg. Tárolók BLOB blob-fájlok tartalmaznak, és szervezheti, így ezek BLOB mappa és almappák. Lásd: [a .NET Blob-tárolóhoz használatáról](./storage/storage-dotnet-how-to-use-blobs.md) további információt.

### <a name="to-create-a-blob-container"></a>Blob-tároló létrehozása

1. Nyissa meg a helyi menü a **BLOB** -csomópont, és válassza a **Blob-tárolóhoz létrehozása**.

1. Adja meg az új tároló nevét a **Blob-tárolóhoz létrehozása** párbeszédpanelen, és válassza az **Ok**

    >[AZURE.NOTE] A blob-tárolóhoz neve szám (0 – 9-ig) vagy (a – z) kisbetű kell kezdődnie.

### <a name="to-delete-a-blob-container"></a>Blob-tároló törlése

- Nyissa meg a helyi menü a szeretné eltávolítani, és válassza a **Törlés**blob-tároló.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Blob-tárolóban lévő elemeket listájának megjelenítéséhez

- Nyissa meg a helyi menü egy blob-tároló nevet a listában, és válassza a **View Blob-tárolóhoz**.

    Amikor blob-tároló tartalmát, megjelenik egy lapon a blob-tárolóhoz nézet neve.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    A blob-tárolóhoz nézet jobb felső sarkában található gombokkal BLOB meg az alábbi műveleteket végezheti el:

    - Adja meg a szűrő értékét, és alkalmazása

    - BLOB-tárolóban listájának frissítése

    - Fájl feltöltése

    - Blob törlése

      >[AZURE.NOTE] A fájlok törlését egy blob-tárolóból nem törli a háttérben levő fájl; Ez csak eltávolítja a blob-tárolóhoz.

    - Nyissa meg a blob

    - A helyi számítógép blob mentése

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Blob-tároló mappa vagy almappa létrehozása

1. Válassza a blob-tároló kiszolgáló Explorer. A tároló ablakában a **Blob feltöltése** gombra.

    ![Egy fájl feltöltése egy blob-mappába](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. **Új fájl feltöltése** párbeszédpanelen válassza a **Tallózás** gombra kattintva adja meg a feltölteni kívánt fájlt, és a **mappa (nem kötelező)** mezőbe írja be a mappa nevét.

    Ugyanezt az eljárást követve hozzáadhat almappák tároló mappában. Ha nem ad meg a mappa nevét, a fájlt a blob-tárolóhoz a legfelső szintű feltöltés. A fájl a megadott a tároló mappa jelenik meg.

    ![Mappához blob-tárolóhoz](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Kattintson duplán arra a mappára, vagy nyomja le a mappa tartalmát meg az ENTER billentyűt. A tároló mappa közben megkeresheti vissza a legfelső szintű a tároló az **Open szülő Directory** (fel nyíl) gomb kiválasztásával.

### <a name="to-delete-a-container-folder"></a>A tároló mappa törlése

 - Az összes a mappában lévő fájlok törlése

    >[AZURE.NOTE] Mivel blob tárolók mappák virtuális mappák, nem hozható létre a mappa ürítése, és nem is törli a fájl tartalmának törlése egy mappát. Ha a törölni kívánt mappát egy mappa teljes tartalmának törléséhez.

### <a name="to-filter-blobs-in-a-container"></a>BLOB-tárolóban szűrése

Az egy közös előtag megadásával megjelenített BLOB is szűrheti.

Ha például az előtag beírt `hello` szűrő szöveg mezőbe, és válassza a **végrehajtás** (****!) gomb, a csak a "hello" kezdetű BLOB jelenik meg.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] A szűrőmezőt kis-és nagybetűk, és nem támogatja a helyettesítő karakterekkel szűrést. BLOB csak szűrhetők előtag használatával. Az előtag elválasztó tartalmazhatnak, elválasztó BLOB virtuális hierarchikusan rendszerezheti használatakor. Például az előtag HelloFabric szűrés / karakterlánc kezdődő összes BLOB adja eredményül.

### <a name="to-download-blob-data"></a>Blob-adatok letöltése

- A **Kiszolgáló Explorer**nyissa meg a helyi menü egy vagy több BLOB- és majd **megnyitása**, válassza válassza a blob nevét, és majd válassza a **Megnyitás** gombra, vagy kattintson duplán a blob.

    A **Tevékenységnapló Azure** ablakban blob letöltés végrehajtásának jelenik meg.

    Az alapértelmezett szerkesztő az adott fájltípus a blob nyílik meg. Ha az operációs rendszer felismeri a fájl típusa, nyílik meg a helyileg telepített alkalmazások; egyéb esetben rákérdez, válassza ki a blob fájl típusának megfelelő alkalmazást. A helyi blob letöltésekor létrehozott fájl csak olvashatóként van-e jelölve.

    BLOB-adatainak helyben és veti össze a blob legutóbbi módosítás idejét a Blob szolgáltatásban. Ha a blob frissült, mivel a legutóbbi letöltésének, akkor letölt újra; egyéb esetben a blob a helyi lemezről töltődnek. Alapértelmezés szerint a blob letöltődik egy ideiglenes mappába. Töltse le egy adott könyvtár BLOB, nyissa meg a helyi menü a kijelölt blob nevek, és válassza a **Mentés másként**parancsot. Blob ily módon történő mentésekor a blob-fájl nem nyílik meg, és a helyi fájl írási és olvasási attribútumokkal rendelkező jön létre.

### <a name="to-upload-blobs"></a>Töltse fel a BLOB

- Amikor a tároló nyissa meg a blob-tárolóhoz nézetben megjelenített, válassza a **Blob feltöltése** gombra.

    Megadhatja, hogy töltse fel egy vagy több fájlt, és bármilyen típusú fájlokat is feltölthet. A **Tevékenységnapló Azure** látható az a feltöltés. Hogyan blob-adatokkal végzett munkához, akkor olvassa el [a Azure Blob-tároló szolgáltatás a .NET használatáról](http://go.microsoft.com/fwlink/p/?LinkId=267911)további információt.

### <a name="to-view-logs-transferred-to-blobs"></a>BLOB átkerülnek naplók megtekintése

- Ha Azure diagnosztika segítségével jelentkezzen be az adatokat az Azure-alkalmazásokból, és a tárterület-fiók van átkerülnek naplók, látni fogja a tárolók az alábbi naplók Azure által létrehozott. Ezek a naplófájlok megtekintése a kiszolgáló Explorer legegyszerűbben az alkalmazással kapcsolatos problémák azonosítása, különösen akkor, ha az Azure lett telepítve. Azure diagnosztika kapcsolatos további tudnivalókért olvassa el a [Naplózás adatok összegyűjtése Azure diagnosztika segítségével](https://msdn.microsoft.com/library/azure/gg433048.aspx)című témakört.

### <a name="to-get-the-url-for-a-blob"></a>Az URL-cím beszerzése blob

- Nyissa meg a blob helyi menüt, és válassza a **Másolás URL-CÍMÉT**.

### <a name="to-edit-a-blob"></a>Blob szerkesztése

- Jelölje ki a a blob, és kattintson a **Megnyitás Blob** gombra.

    A fájl letöltése az ideiglenes helyre és a helyi számítógépen megnyitott. Módosítások elvégzése után újra kell feltölteni a a blob.

## <a name="work-with-queue-resources"></a>Az várólista erőforrások kezelése

Tárterület szolgáltatások sorban várakozó Azure tárterület-fiókjában tárolt, és engedélyezni a felhőalapú szolgáltatás szerepkörök egymással és más szolgáltatásokhoz kommunikáció mechanizmusa átadása üzenet használhatja őket. Hozzáférhet a várólista programozás útján keresztül egy felhőalapú szolgáltatásba, és a külső ügyfélalkalmazások webszolgáltatás fölé. A várakozási sorban található közvetlenül a Visual Studio Server Explorer használatával is elérheti.

Ha egy felhőalapú szolgáltatásba sorban várakozó használó fejleszt, érdemes lehet Visual Studio segítségével sorban várakozó létrehozása és használata őket interaktív közben fejlesztése, és tesztelje a kód.

A kiszolgáló Intézőben, is megtekintése a sorokat egy tárterület-fiókban, létrehozása és sorok törlése, várólista az üzenetek megtekintéséhez nyissa meg és üzenetek hozzáadása egy várólista. Amikor megnyit egy várólista megtekintését, megtekintheti, hogy az egyes üzeneteket, és végezhet az alábbi műveletek a sor bal felső sarokban található gombokkal:

- Frissítheti a nézetet, a sor

- Üzenet a sor hozzáadása

- A legfelső üzenet feldolgozásához.

- A teljes sor törlése

Az alábbi képen látható, amely tartalmazza a két üzenetek várólista.

![Egy várólista megtekintése](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Tárterület kapcsolatos további tudnivalók a sorok Services, lásd: [Útmutató: a várólista tároló szolgáltatás használata](http://go.microsoft.com/fwlink/?LinkID=264702). A webszolgáltatás tároló szolgáltatások sorban várakozó tudni olvassa el a [Várólista szolgáltatást fogalmak](http://go.microsoft.com/fwlink/?LinkId=264788)című témakört. További információt a tárhely szolgáltatások várólista Visual Studio segítségével üzeneteket küldeni, című témakörben olvashat a [Tárhely szolgáltatások várólista üzenetek küldése](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Tárterület szolgáltatások sorban várakozó erősen különböznek bus sorokból. Szolgáltatás bus sorban várakozó kapcsolatos további tudnivalókért olvassa el a szolgáltatás Bus sorban várakozó, témák és előfizetések című témakört.

## <a name="work-with-table-resources"></a>Az táblázat erőforrások kezelése

Az Azure-táblából tárhelyszolgáltatáshoz nagy mennyiségű strukturált adatokat tárolja. A szolgáltatás nem egy NoSQL adattárhoz, amely elfogad hitelesített belüli és kívüli Azure felhőbeli érkező hívások. Azure táblák ideálisak strukturált, nem relációs adatok tárolására szolgáló.

### <a name="to-create-a-table"></a>Tábla létrehozása

1. A kiszolgáló Intézőben jelölje ki a tárterület-fiók a **táblázatok** csomópontot, és válassza a **Create Table**.

1. **Táblázat létrehozása** párbeszédpanelen írja be a tábla nevét.

### <a name="to-view-table-data"></a>Táblázat-adatok megtekintése

1. A kiszolgáló Intézőben nyissa meg az **Azure** csomópontot, és nyissa meg a **tárhely** csomópontot.

1. Nyissa meg a tárhely fiók csomópontot, amelyek érdeklik, és nyissa meg a **táblák** csomópontot a tárterület-fiókom táblák listájának megtekintéséhez.

1. Nyissa meg a helyi menü egy táblához, és válassza a **Nézet táblából**.

    ![Megoldás Explorer Azure-táblázat](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

A táblázat személyek (sorok látható), és tulajdonságok (az oszlopok látható) szerint vannak rendezve. Ha például az alábbi ábrán a **Táblatervező**felsorolt személyek:

### <a name="to-edit-table-data"></a>Táblázat-adatok szerkesztése

1. A **Táblatervező**nyissa meg a helyi menü egy entitás (egy sorban) vagy egy tulajdonságot (egyetlen cella), és válassza a **Szerkesztés**.

    ![Hozzáadása vagy módosítása egy táblázat egység](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Személyek egy táblában nem kötelező kitölteni, hogy ugyanazok a Tulajdonságok (oszlopokat). Tartsa szem előtt az alábbi korlátozások a táblázat-adatok megtekintésére és szerkesztésére.
    - Tekinthető meg és szerkesztheti a bináris adatokat (típusú byte[]), de tárolhatja azt egy táblázatban.

    - A **PartitionKey** vagy **RowKey** értéket, nem szerkeszthető, mert az Azure táblatárolóhoz nem támogatja ezt a műveletet.

    - Nem hozhatók létre időbélyeg nevű tulajdonságot, Azure tárolása tulajdonsággal egy ilyen nevű.

    - Ha megad egy DateTime típusú érték, hajtsa végre a számítógép területi és nyelvi beállítások megfelelő formátumban (például hh/nn/éééé óó [AM |} DU] az Amerikai Egyesült Államok-angol).

### <a name="to-add-entities"></a>Személyek hozzáadása

1. A **Táblatervező**válassza a **Szervezet hozzáadása** gombra, amely a táblázat nézet jobb felső sarkának közelében.

    ![Személy hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. **Személy hozzáadása** párbeszédpanelen írja be az értékeket a **PartitionKey** és **RowKey** tulajdonság.

    ![Személy szolgáló párbeszédpanel](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Írja be az értékeket gondosan mivel már nem módosíthatja őket a párbeszédpanel bezárásához, kivéve, ha a szervezet törlése, és állítsa be újra.

### <a name="to-filter-entities"></a>Személyek szűrése

Testre szabhatja a táblázatban a lekérdezéskészítő használatakor megjelenő szervezetek készlete.

1. Nyissa meg a Lekérdezésszerkesztő, nyissa meg a tábla megjelenítése.

1. Kattintson a táblázat nézet eszköztár jobb szélső gombjára.

    A **Lekérdezésszerkesztő** párbeszédpanel jelenik meg. Az alábbi ábrán egy lekérdezést, amely éppen elkészült a Lekérdezésszerkesztőben.

    ![Lekérdezésszerkesztő](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Ha végzett a lekérdezéshez, épület a párbeszédpanel bezárásához. WCF Data Services szűrőként a lekérdezés megjelenő szöveg képernyőn jelenik meg szövegdoboz.

1. Futtassa a lekérdezést, válassza a zöld háromszögikonra.

    Megjelenik a **Tábla Tervező** Ez esetben közvetlenül a szűrő mezőben WCF Data Services szűrő karakterlánc entitás adatokat is szűrheti. A karakterlánc típusú hasonlít egy SQL WHERE záradékot, de a kiszolgálón, a HTTP-kérelem küld. Szűrő karakterláncok Egyenletszerkesztővel olvashat lásd: a [Szűrő karakterláncok hozhat létre, amely a tábla Tervező](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Az alábbi ábrán egy példa egy érvényes szűrő karakterláncot:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Tárterület-adatok frissítése

Kiszolgáló Explorer csatlakozik, vagy adatok lekérése a tárterület-fiók esetén igénybe vehet a művelet elvégzéséhez perccé alakít. Ha azt nem lehet csatlakozni, a művelet előfordulhat, hogy időkorlátja. Rendezettnek, miközben a munkát a Visual Studio többi részén is. A művelet megszakítja, ha túl sokáig tart, válassza ki a kiszolgáló Explorer eszköztáron a **Frissítés leállítása** gombra.

#### <a name="to-refresh-blob-container-data"></a>Blob-tároló adatok frissítése

- Jelölje ki a **BLOB** - **tároló** alatt csomópontot, és válassza ki a kiszolgáló Explorer eszköztáron a **frissítés** gombra.

- Megjelenített BLOB listájának frissítéséhez kattintson az **végrehajtás** gombjára.

#### <a name="to-refresh-table-data"></a>Táblázat-adatok frissítése

- Jelölje ki a **táblázatok** csomópontot **tároló** alatt, és kattintson a **frissítés** gombra.

- Megjelenik a **Tábla Tervező**személyek listájának frissítéséhez kattintson a **Tábla Tervező** **végrehajtás** gombjára.

#### <a name="to-refresh-queue-data"></a>A várakozási megjelenített adatok frissítése

- Jelölje ki a **sorban várakozó** csomópontot, és kattintson a **frissítés** gombra.

#### <a name="to-refresh-all-items-in-a-storage-account"></a>A tároló fiók összes eleme frissítése

- Válassza a fiók nevét, és válassza a **frissítés** gombra az eszköztáron a kiszolgáló Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Tárterület-fiókok felvétele kiszolgáló Explorerrel

Kétféleképpen tárterület-fiók hozzáadása a kiszolgáló Intézővel. Új tároló fiók létrehozhat az Azure-előfizetése, vagy egy meglévő tárterület-fiókot csatolhat.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Tárterület új fiók létrehozása a kiszolgáló Explorerrel

1. A kiszolgáló Intézőben nyissa meg a helyi menü a tárhely csomópont, és válassza a tárterület-fiók létrehozása.

    ![Azure tároló új fiók létrehozása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Jelölje ki, vagy írja be az alábbi adatokat az új tárterület-fiók a **Tárterület-fiók létrehozása** párbeszédpanel.

    - A Azure előfizetést, amelyhez a tárterület-fiók hozzáadásához.

    - Az új tároló fiók használni kívánt neve.

    - A terület vagy affinitás csoport (például a nyugati USA-beli vagy kelet-ázsiai).

    - A replikáció tárterület-fiók esetén például Geo felesleges használni kívánt típusát.

1. Válassza ki a **létrehozni**.

    Az új tároló fiók megjelenik a megoldást Explorer **tároló** listájában.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>Egy meglévő tárterület-fiókot csatolhat kiszolgáló Explorerrel

1. A kiszolgáló Intézőben az Azure tárolás csomópont helyi menüjének megnyitása, és válassza a **Külső tároló csatolni**.

    ![Egy meglévő tárterület-fiók hozzáadása](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Jelölje ki, vagy írja be az alábbi adatokat az új tárterület-fiók a **Tárterület-fiók létrehozása** párbeszédpanel.

    - A csatolni kívánt meglévő tárterület-fiók neve. Írjon be egy nevet, vagy jelölje ki a listából.

    - A kijelölt tároló fióknak billentyűt. Ez az érték a szokásos megadva, amikor kijelöl egy tárterület-fiókkal. Ha azt szeretné, hogy a Visual Studio a tárhely fiókkulcs emlékezni fog, jelölje be az emlékeztető fiók mezőbe.

    - A Protocol (protokoll) szeretne csatlakozni a tárterület-fiókot, például a HTTP, HTTPS vagy egy egyéni végpontot. Megtudhatja, [hogy miként konfigurálása kapcsolati karakterláncot](https://msdn.microsoft.com/library/azure/ee758697.aspx) az egyéni végpontok kapcsolatban további tudnivalókat.

### <a name="to-view-the-secondary-endpoints"></a>A másodlagos végpontok megtekintése

- Ha létrehozott egy **Olvasásra Geo felesleges** replikációs parancs tárterület-fiókkal, a másodlagos végpontjait megtekintheti. Nyissa meg a helyi menü a fiók nevét, és válassza a **Tulajdonságok parancsot**.

    ![Másodlagos tárterület-végpontok](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>A tárterület-fiók eltávolítása Server Explorer

- A kiszolgáló Intézőben a fióknév helyi menüjének megnyitása, és válassza a **Törlés**gombra. Ha töröl egy tárterület-fiókkal, fiókhoz tartozó mentett fontos információkat is eltávolítja.

    >[AZURE.NOTE] Tárterület fiók törlése a kiszolgáló Intézőből, ha nincs hatással a tárterület-fiók vagy a kívánt adatokat tartalmaz; a kiszolgáló Intézőből a hivatkozás egyszerűen eltávolítja. Véglegesen töröl egy tárterület-fiókot, használja az [Azure klasszikus portálon](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Következő lépések

További tudnivalókat a Azure tároló szolgáltatások használatára című témakörben [elérése a Azure tároló szolgáltatást](https://msdn.microsoft.com/library/azure/ee405490.aspx).
