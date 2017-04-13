<properties
    pageTitle="Microsoft Azure-tárolóban lévő feldolgozási kezelése"
    description="Feldolgozási a Blob, várólista, táblázat vagy fájl szolgáltatások kezelése"
    services="storage"
    documentationCenter=""
    authors="jasonnewyork"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jahogg"/>

# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure-tárolóban lévő feldolgozási kezelése

## <a name="overview"></a>– Áttekintés

Modern Internet alapú alkalmazások általában a megtekintése és szerkesztése az adatok egyszerre több felhasználó van. Ehhez az alkalmazások fejlesztői körültekintően ahhoz, hogy egy kiszámíthatóan élmény a felhasználóknak, különösen ha több felhasználó ugyanazokat az adatokat frissíti esetek a olvashat. Vannak három fő adatok feldolgozási stratégiák általában veszi figyelembe a fejlesztők:  


1.  Optimista feldolgozási – az alkalmazás elvégzéséhez frissítés való frissítés részeként ellenőrzi Ha megváltoztatja az óta az alkalmazás utolsó olvassa el az adatokat. Például ha két felhasználók megtekintés wikilap frissítés ugyanazon az oldalon kattintson a Wikilapok platform biztosítania kell, hogy a második frissítés nem írja felül az első frissítés – és, hogy mindkét felhasználók megértéséhez, hogy a frissítés sikeres volt-e vagy sem. Ez a stratégia webalkalmazások leggyakrabban használatos.
2.  Pesszimista feldolgozási – az alkalmazás keresése frissítés elvégzéséhez veszi lakat megakadályozza, hogy más felhasználók az adatok frissítését, amíg a zárolás objektum. Például ha csak a fő frissítések végrehajtása főadat/kisegítő adatok replikációs helyzetben a fő általában ráfér-kizáró zárolása hosszabb ideig ideje, annak érdekében, hogy senki frissítheti az adatokat.
3.  Utolsó író wins –, amely lehetővé teszi, hogy a frissítés műveletek ellenőrzése, ha más alkalmazás az adatok óta frissített az alkalmazás első nélkül folytatásához megközelítés olvasni az adatokat. Stratégia (vagy egy hivatalos stratégia hiánya) általában használatos hol van formázva adatok úgy, hogy van-e nem valószínű, hogy több felhasználó fér ugyanazokat az adatokat. Is lehet hasznos, ha rövid életű adatfolyamok feldolgozása alatt.  

Ez a cikk áttekintést nyújt a hogyan a tárhely Azure platform leegyszerűsíti az e verzió-ellenőrzési stratégiák mindhárom első osztályú támogatásával fejlesztési.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure tároló – egyszerűbbé teszi a felhőben fejlesztési
Az Azure tárhelyszolgáltatáshoz három stratégiák támogatja a teljes támogatást nyújt optimista, pesszimista és feldolgozási, mert a erős konzisztencia modell, amely biztosítja, hogy a tárhelyszolgáltatáshoz véglegesíti a adatok Beszúrás vagy az összes frissítési művelet további fér hozzá az, hogy adatokat jelenik meg a legújabb frissítésben kibővül készült képessége megkülönböztető ugyan. Az esetleges konzisztencia adatmodellt használó tároló platformokon egy közötti eltérés áll elő, amikor írási végrehajtott egy felhasználó, és a frissített adatoknak a végfelhasználók érintő inkonzisztenciák megelőzése érdekében ügyfélalkalmazásokban fejlesztésének így része a mások által is láthatja.  

Mellett egy megfelelő feldolgozási stratégia kijelölése a fejlesztők kell is érdemes szem előtt tartania, hogy hogyan tároló platformot elkülöníti a módosításai – különösen ugyanazon objektumhoz tranzakciók keresztül. A tároló Azure szolgáltatás lehetővé teszi az olvasási műveletek párhuzamosan írási műveletek belül egy partíciót történjen pillanatkép elkülönítési használja. Eltérően más elkülönítési szintek pillanatkép elkülönítési garantálja, hogy az összes olvasás lásd: az adatok egységes pillanatképét történnek a frissítések – lényegében visszaadó a frissítés során lekötött utolsó értékek tranzakció feldolgozása közben is.  

## <a name="managing-concurrency-in-blob-storage"></a>Feldolgozási Blob-tároló kezelése
Választhatja a BLOB és tárolók a blob szolgáltatásban való hozzáférés kezelése a bármelyik optimista, pesszimista vagy feldolgozási modellek használatával. Ha nem ad kifejezetten stratégia utolsó írások wins az alapértelmezett érték.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimista feldolgozási BLOB és tárolók  

A tároló szolgáltatás rendel tárolt összes objektum azonosítót. Az azonosító frissül, minden alkalommal, amikor egy frissítés műveletet végzi el a kívánt objektumra. Az azonosító a rendszer az ügyfél HTTP GET választ a ETag (entitás címkét) fejlécet a HTTP protokoll belül meghatározott részét képező adja vissza. Frissítés végrehajtása egy objektumon felhasználó annak érdekében, hogy frissítés akkor történik, ha egy bizonyos feltétel teljesül: Ebben az esetben a feltétel-e egy "Ha, hol.van" fejléc, amely előírja a Tárhelyszolgáltatáshoz annak érdekében, hogy a megadott a frissítési kérést ETag értéke megegyezik a tárhely szolgáltatásban tárolt küldhet az eredeti ETag feltételes fejlécet együtt.  

Ez a folyamat körvonalának van az alábbi képlettel történik:  

1.  Blob beolvasni a tárhely szolgáltatásból, a válasz egy HTTP-ETag fejléc érték, amely azonosítja az objektumot a tárhelyszolgáltatáshoz aktuális verziójának tartalmazza.
2.  Amikor frissíti a blob, bele az lépés: 1 az **If-egyező** feltételes fejlécében a kérelem küld a szolgáltatás kapott ETag értékét.
3.  A szolgáltatás a ETag értéket, a kérelem ETag aktuális értékét a blob hasonlítja össze.
4.  Ha az aktuális ETag a blob értéke egy másik verzióját, mint a ETag az **If-egyező** feltételes fejlécében a kérelem, a szolgáltatás 412 hiba az ügyfélnek adja eredményül. Ez mutatja az ügyfél számára, hogy egy másik folyamat frissítette a blob, mivel az ügyfél visszakeresése.
5.  Ha az aktuális ETag a blob értéke, az **If-egyező** feltételes fejlécében a kérelem a ETag azonos verzióban, a szolgáltatás a kért művelet végrehajtása, és frissíti a blob kattintva jelenítse meg, hogy hozott létre egy új verzió aktuális ETag értékét.  

A következő C# kódtöredékének (használatával az ügyfél tároló tár 4.2.0) jeleníti meg egy egyszerű példa egy **If-egyezés AccessCondition** Egyenletszerkesztővel, amely érhető el, amely korábban vagy beolvasott vagy beszúrt blob tulajdonságlapjáról ETag értéke alapján. Ezután használja a a **AccessCondition** objektum amikor azt a blob frissítése: a **AccessCondition** objektum az **If-egyezés** élőfej hozzáadása a kérést. Ha egy másik folyamat frissítette a blob, az a blob-szolgáltatás egy HTTP 412 (előfeltétel nem sikerült) állapotüzenet adja eredményül. Töltse le a teljes minta: [Azure-tárolót használó kezelése feldolgozási](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

    // Retrieve the ETag from the newly created blob
    // Etag is already populated as UploadText should cause a PUT Blob call
    // to storage blob service which returns the etag in response.
    string orignalETag = blockBlob.Properties.ETag;

    // This code simulates an update by a third party.
    string helloText = "Blob updated by a third party.";

    // No etag, provided so orignal blob is overwritten (thus generating a new etag)
    blockBlob.UploadText(helloText);
    Console.WriteLine("Blob updated. Updated ETag = {0}",
    blockBlob.Properties.ETag);

    // Now try to update the blob using the orignal ETag provided when the blob was created
    try
    {
        Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
        blockBlob.UploadText(helloText,accessCondition:
        AccessCondition.GenerateIfMatchCondition(orignalETag));
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        {
            Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
            // TODO: client can decide on how it wants to handle the 3rd party updated content.
        }
        else
            throw;
    }  

A tároló szolgáltatás is támogatja további feltételes fejlécek például **If-módosított-óta**, **Ha-változatlanul-óta** és **If-nincs – hol.van** , valamint kombinációi. További információt találhat az MSDN [Feltételes fejlécek Blob-szolgáltatási műveletek megadása](http://msdn.microsoft.com/library/azure/dd179371.aspx) kapcsolatban.  

Az alábbi táblázat összefoglalja a tároló műveletek, amely elfogadja a kérést például **Ha-egyező** feltételes fejlécek, és a válasz, amely ETag értéket adnak vissza.  

| Művelet                | A tároló ETag értéket ad vissza | Fogadja el a feltételes fejlécek |
|:-------------------------|:-----------------------------|:----------------------------|
| A tároló létrehozása         | igen                          | nem                          |
| A tároló tulajdonságainak lekérése | igen                          | nem                          |
| A tároló metaadatok beszerzése   | igen                          | nem                          |
| A tároló metaadatok beállítása   | igen                          | igen                         |
| Tároló vezérlés beszerzése        | igen                          | nem                          |
| Tároló vezérlés beállítása        | igen                          | Igen (*)                     |
| Tároló törlése         | nem                           | igen                         |
| Bérleti tároló          | igen                          | igen                         |
| Lista BLOB               | nem                           | nem                          |

(*) Gyorsítótárban SetContainerACL által meghatározott engedélyeket, és terjesztése 30 másodperc, amely időszakban frissítések nem garantált biztosítani szeretné konzisztens módosítása a módosítások alkalmazása ezeket az engedélyeket.  

Az alábbi táblázat összefoglalja a blob-műveletek, amely elfogadja a kérést például **Ha-egyező** feltételes fejlécek, és a válasz, amely ETag értéket adnak vissza.

| Művelet           | ETag értéket ad vissza | Fogadja el a feltételes fejlécek           |
|:--------------------|:-------------------|:--------------------------------------|
| Helyezze a Blob            | igen                | igen                                   |
| Blob beszerzése            | igen                | igen                                   |
| Első Blob-tulajdonságok | igen                | igen                                   |
| Blob-tulajdonságainak beállítása | igen                | igen                                   |
| Első Blob-metaadat   | igen                | igen                                   |
| Blob-metaadatok beállítása   | igen                | igen                                   |
| Bérleti Blob (*)      | igen                | igen                                   |
| Pillanatkép Blob       | igen                | igen                                   |
| Blob másolása           | igen                | Igen (forrás- és céltáblák blob) |
| Másolás Blob megszakítása     | nem                 | nem                                    |
| Blob törlése         | nem                 | igen                                   |
| Továbbfejlesztett fájlblokkolás helyezése           | nem                 | nem                                    |
| Blokkokból álló lista helyezése      | igen                | igen                                   |
| Blokkokból álló lista beolvasása      | igen                | nem                                    |
| Helyezze a lapon            | igen                | igen                                   |
| Első oldal tartományok     | igen                | igen                                   |


(*) A ETag blob a bérleti Blob nem változik.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pesszimista feldolgozási BLOB-
Blob-kizáró használata rögzítéséhez szerezhet a [bérleti](http://msdn.microsoft.com/library/azure/ee691972.aspx) rajta. Amikor egy bérleti finomítása, megadhatja, hogy mennyi ideig kell a bérleti: Ez lehet az 15-60 másodperc között vagy végtelen amely-kizáró zárolása összege. Megújíthatja függvényt bérleti, ha ki szeretné terjeszteni azt, és bármelyik bérleti is engedje fel, ha végzett az általa. A blob-szolgáltatás automatikusan elengedi függvényt bérletek, amikor járnak.  

Lízingek engedélyezése a különböző szinkronizálási stratégiák véve, beleértve a kizárólagos írási és olvasási, kizárólagos írási megosztott / kizárólagos olvasási és írási megosztott / kizárólagos olvasható. Ha egy bérleti létezik a tároló szolgáltatás kényszeríti kizárólagos írások (elhelyezni, beállítása és törlése a műveletek) érvényes bérleti azonosítóval azonban olvasási műveletek kizárólagosság biztosítva van szükség a biztosítja, hogy az összes ügyfélalkalmazásokban a bérleti azonosító, és csak egy ügyfél által egyszerre a fejlesztői rendelkezik. További műveletek, amely nem szerepel a megosztott olvasás bérleti azonosító eredményt.  

A következő C# kódtöredékének 30 másodperc a blob-kizáró bérleti beszerző, és a blob tartalmának frissítése, majd felengedi a bérleti példa. Ha már van egy érvényes bérleti a blob szerezheti be egy új bérleti használatakor, a blob-szolgáltatás egy "HTTP (409) ütközés" állapot eredményt adja. Az alábbi kódtöredékének **AccessCondition** objektum beágyazása a bérleti adatokat, ha azt a tárhely szolgáltatásban a blob frissítése kérést használja.  Töltse le a teljes minta: [Kezelése verzió-ellenőrzési Azure tároló használatával](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    // Acquire lease for 15 seconds
    string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
    Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

    // Update blob using lease. This operation will succeed
    const string helloText = "Blob updated";
    var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
    blockBlob.UploadText(helloText, accessCondition: accessCondition);
    Console.WriteLine("Blob updated using an exclusive lease");

    //Simulate third party update to blob without lease
    try
    {
        // Below operation will fail as no valid lease provided
        Console.WriteLine("Trying to update blob without valid lease");
        blockBlob.UploadText("Update without lease, will fail");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
            Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
        else
            throw;
    }  

Ha megpróbálja a bérelt blob írási művelet anélkül bérleti azonosítója, a kérelem 412 hibával sikertelen lesz. Figyelje meg, hogy ha a bérleti lejár, mielőtt a **UploadText** metódus meghívása, de továbbra is adja meg a bérleti azonosítója, a kérelem is sikertelen **412** hibával. Bérleti lejárati idő és a bérleti azonosítók kezelésével kapcsolatos további tudnivalókért lásd: a [Bérleti Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) többi dokumentációt.  

A következő blob-műveletek bérletek segítségével pesszimista feldolgozási kezelése:  


-   Helyezze a Blob
-   Blob beszerzése
-   Első Blob-tulajdonságok
-   Blob-tulajdonságainak beállítása
-   Első Blob-metaadat
-   Blob-metaadatok beállítása
-   Blob törlése
-   Továbbfejlesztett fájlblokkolás helyezése
-   Blokkokból álló lista helyezése
-   Blokkokból álló lista beolvasása
-   Helyezze a lapon
-   Első oldal tartományok
-   Pillanatkép Blob - választható, ha létezik a bérleti bérleti azonosító
-   Blob - ID szükséges, ha egy bérleti megtalálható-e a cél blob bérleti másolása
-   Megszakítás másolás Blob - bérleti azonosító kötelező, ha a célként megadott blob-végtelen bérleti létezik
-   Bérleti Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Tárolók pesszimista feldolgozási
A tárolók bérletek engedélyezése a azonos szinkronizálási stratégiák BLOB a véve (kizárólagos írási és olvasási, kizárólagos írási megosztott / kizárólagos olvasási és írási megosztott kizárólagos olvasási /) azonban BLOB eltérően a tároló szolgáltatás csak kényszeríti törlési műveletekre vonatkozó kizárólagosság. Az az aktív bérleti tároló törléséhez ügyfél tartalmaznia kell a törlés kérésével aktív bérleti azonosítója. Minden más tároló művelet sikerült, a bérelt tároló nélkül, beleértve a bérleti azonosító ebben az esetben is megosztott műveletek. Frissítés (helyezése vagy a beállítása) vagy az olvasási műveletek kizárólagosság szükségessége esetén kattintson a fejlesztők biztosítania kell összes ügyfelet használja a bérleti azonosító, és csak egy ügyfél egyszerre rendelkezik-e egy érvényes bérleti azonosítót.  

A következő tároló műveletek bérletek segítségével pesszimista feldolgozási kezelése:  

-   Tároló törlése
-   A tároló tulajdonságainak lekérése
-   A tároló metaadatok beszerzése
-   A tároló metaadatok beállítása
-   Tároló vezérlés beszerzése
-   Tároló vezérlés beállítása
-   Bérleti tároló  

További információ:  

- [Feltételes fejlécek megadása Blob-szolgáltatási műveletek](http://msdn.microsoft.com/library/azure/dd179371.aspx)
- [Bérleti tároló](http://msdn.microsoft.com/library/azure/jj159103.aspx)
- [Bérleti Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>A táblázat szolgáltatásban feldolgozási kezelése
A táblázat szolgáltatás által optimista feldolgozási ellenőrzi, az alapértelmezett működés személyekkel eltérően a blob szolgáltatás, ahol kifejezetten választania kell az optimista feldolgozási ellenőrzések végrehajtásához használatakor. A táblázat és blob-szolgáltatások más különbségét, hogy csak kezelheti feldolgozási működése a szervezetek, mivel a blob-szolgáltatás a tárolók és a BLOB feldolgozási kezelheti.  

Optimista feldolgozási használata és ellenőrzése, ha egy másik folyamat módosította entitás beolvasni a táblázat tároló szolgáltatásból, a ETag értéket, hogy kap, ha a táblázat szolgáltatás entitás adja vissza is használhatja. Ez a folyamat körvonalát van az alábbi képlettel történik:  

1.  Entitás beolvashatja a táblázat tároló szolgáltatás, a válasz egy ETag értéket, amely azonosítja az aktuális azonosító, hogy a tárhelyszolgáltatáshoz entitás társított tartalmazza.
2.  Ha a szervezet, akkor bele az lépés: 1 a kérelem a szolgáltatás elküldi a kötelező **If-egyezés** fejlécében kapott ETag értékét.
3.  A szolgáltatás összehasonlítja ETag a kérelem entitás aktuális ETag értékkel.
4.  Ha az aktuális ETag érték entitás ugyanaz, mint a ETag a kötelező **If-hol.van** fejlécben a kérést, a szolgáltatás 412 hiba az ügyfélnek adja eredményül. Ez mutatja az ügyfél számára, hogy egy másik folyamat frissítette a szervezet, mivel az ügyfél visszakeresése.
5.  Ha az aktuális ETag a szervezet értéke megegyezik a ETag a kötelező **If-hol.van** fejlécben a kérelmet, vagy az **If-egyezés** élőfej tartalmaz a helyettesítő karakter (*), a szolgáltatás hajtja végre a kívánt művelet, és frissíti a személy kattintva jelenítse meg, hogy azt frissült az aktuális ETag értéket.  

Figyelje meg, hogy a blob-szolgáltatás eltérően a táblázat szolgáltatás kell lennie az ügyfél- **If-egyezés** élőfej szerepeltetni kérések. Azonban ajánlatos kényszerítése egy feltétel (utolsó író wins stratégia) frissítése és feldolgozási szempontú vizsgálatok kikerülheti, ha az ügyfél az **If-egyezés** élőfej állítja a helyettesítő karakter (*) a kérést.  

A következő C# kódtöredékének egy ügyfél entitás vagy korábban létrehozott, vagy olvassa be a személy e-mail címét frissíteni problémákat jeleníti meg. A kezdeti beszúrása vagy művelet tárolja az ügyfél objektum ETag érték beolvasása, és a minta objektum példányt használ, a csere művelet végrehajtásakor, mert azt automatikusan visszaküldi ETag értékét a táblázat szolgáltatás, engedélyezi a szolgáltatást a feldolgozási megsértése ellenőrzéséhez. Egy másik folyamat frissítette a szervezet táblatárolóban lévő, ha a a szolgáltatás állapota (előfeltétel nem sikerült) HTTP 412 üzenetben adja eredményül.  Töltse le a teljes minta: [Kezelése verzió-ellenőrzési Azure tároló használatával](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

    try
    {
        customer.Email = "updatedEmail@contoso.org";
        TableOperation replaceCustomer = TableOperation.Replace(customer);
        customerTable.Execute(replaceCustomer);
        Console.WriteLine("Replace operation succeeded.");
    }
    catch (StorageException ex)
    {
        if (ex.RequestInformation.HttpStatusCode == 412)
            Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
        else
            throw;
    }  

Explicit módon letiltása a feldolgozási jelölőnégyzetet, beállíthatja a **alkalmazotthoz** objektum **ETag** tulajdonsága "*", a csere művelet végrehajtása előtt.  

    customer.ETag = "*";  

Az alábbi táblázat összefoglalja, hogyan a táblázat entitás műveletek ETag értékek használata:

| Művelet                | ETag értéket ad vissza | Ha-egyezés kérés fejléce igényel |
|:-------------------------|:-------------------|:---------------------------------|
| Lekérdezés személyek           | igen                | nem                               |
| Entitás beszúrása            | igen                | nem                               |
| Személy frissítése            | igen                | igen                              |
| Személy egyesítése             | igen                | igen                              |
| Személy törlése            | nem                 | igen                              |
| Beszúrás vagy entitás cseréje | igen                | nem                               |
| Beszúrás vagy entitás egyesítése   | igen                | nem                               |

Megjegyzendő, hogy az **Insert vagy cseréje entitás** és **Beszúrás vagy entitás egyesítése** műveletek *nem* bármely verzió-ellenőrzési ellenőrzi, mert azok ne küldjön egy ETag értéket a táblázat szolgáltatás.  

Az általános tábláinak használatával fejlesztők kell támaszkodhat optimista feldolgozási méretezhető alkalmazások fejlesztéséhez. Pesszimista zárolás van szükség, ha egy megközelítés fejlesztők táblák elérése esetén minden táblánál egy kijelölt blob rendelni, és próbálja meg a táblázatban működő előtt tanulmányozza a bérleti a blob vehet igénybe. Ez a módszer annak érdekében, hogy minden adat access elérési út előtt működő a táblázatban a bérleti szerezze be az alkalmazás kell-e. Is vegye figyelembe, hogy a minimális bérleti idő Ez 15 másodperc, amely előírja méretezhetőség ügyeljen figyelembe.  

További információ:  

- [A személyek műveletek](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>A várakozási szolgáltatásban feldolgozási kezelése
Melyik verzió-ellenőrzési a fontos a várólista szolgáltatásban egy helyzetleírás, ahol több ügyfél keres üzeneteket a sorból. Amikor egy üzenetet a program beolvassa a sorból, a válasz tartalmazza, az üzenet- és pop visszaigazolás érték, amelynek van szükség, ha törli az üzenetet. Az üzenet nem automatikusan törli a sorból, de miután megtörtént fogadta, még nem látható más ügyfelek számára a visibilitytimeout paraméterben megadott időközönként. Az ügyfél, amely beolvassa az üzenet várhatóan törlése az üzenet, amelynek a feldolgozása, és a TimeNextVisible által megadott időpontjában előtt kiszámítása a válasz elem visibilitytimeout paraméter értéke alapján. Az idő, amelynél az üzenet TimeNextVisible értékének meghatározása beolvasott bekerül visibilitytimeout értékét.  

Optimista, vagy pesszimista feldolgozási támogatása nem rendelkezik a várólista szolgáltatást, és ennek oka ügyfelek számára beolvasása várólista üzenetek feldolgozásával biztosítania kell az üzenetek idempotent módon feldolgozása. A legutóbbi író wins stratégia frissítési műveletek, például SetQueueServiceProperties, SetQueueMetaData, SetQueueACL és UpdateMessage szolgál.  

További információ:  

- [Várólista szolgáltatást REST API-val](http://msdn.microsoft.com/library/azure/dd179363.aspx)
- [Üzenetek lekérése](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>A fájl szolgáltatásban feldolgozási kezelése
A szolgáltatás két különböző protokoll végpontok – a kis-és Középvállalatok és a többi azok webböngészőn. A többi szolgáltatás nincs optimista zárolás vagy pesszimista zárolás támogatása, és az összes frissítés kövesse az utolsó író wins stratégia. Kis-és Középvállalatok ügyfélprogramokat sorolja fel fájlmegosztások csatlakoztatása is kihasználhatja a fájl rendszer zárolási mechanizmusok kezelheti a megosztott fájlok – többek között az azt jelenti, hogy végezze el a pesszimista zárolás elérésére. Ha egy kis-és Középvállalatok ügyfél megnyit egy fájlt, adja meg a fájlt az access és a megosztás módban. Fájl Access "Írása" vagy "Olvasási/írási" együtt a fájlmegosztás mód, "Nincs" beállítás a fájlt, amíg a fájl meg van nyitva egy kis-és Középvállalatok ügyfél által zárolva eredményezi. Ha a többi művelet pró egy fájlon, ha egy kis-és Középvállalatok ügyfél rendelkezik-e a fájl zárolt állapotban van a többi szolgáltatás az állapotkód 409 (Ütközés) SharingViolation hibakóddal ad vissza.  

Törlés fájlok megnyitásakor a egy kis-és Középvállalatok ügyfél kész megjelöléssel látja el a fájlt, addig, amíg az összes többi kis-és Középvállalatok ügyfél törlés függőben lévő fájl megnyitása fogópontja be van zárva. Törlés függőben be van jelölve egy fájlt, miközben a fájl a megadott többi műveletet állapotkód 409 (Ütközés) SMBDeletePending hibakóddal ad vissza. Állapotkód 404 (nem található) függvény nem ad vissza, mivel előfordulhat, hogy a kis-és Középvállalatok ügyfél, a fájl bezárása előtt törlés függőben jelölő eltávolításához. Más szóval állapotkód 404 (nem található) csak várhatóan a fájl eltávolításakor. Figyelje meg, hogy amikor egy fájlt a kis-és Középvállalatok állapot törlés függőben, akkor nem jelennek a lista fájlok között. Tartsa szem előtt, hogy a többi fájl törlése és a többi törlése címtár művelet lekötött atomically, nem állapot törlés függőben eredményeznek.  

További információ:  

- [Zárolja fájl kezelése](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Összefoglaló és a következő lépések
A Microsoft Azure tárhelyszolgáltatáshoz verziójához meg a fejlesztők számára behatolhatnak vagy kulcs tervezés feltételezések feldolgozási és az adatok konzisztencia nyújtott hajtson végre származnak, például rethink mellőző igényeknek leginkább összetett online alkalmazások.  

A teljes minta alkalmazás blogban hivatkozott:  

- [Azure tároló – minta alkalmazás használatával feldolgozási kezelése](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Azure tároló lásd: további információt:  

- [Microsoft Azure tároló kezdőlapja](https://azure.microsoft.com/services/storage/)
- [Azure tároló – bevezetés](storage-introduction.md)
- Első lépések az [Blob](storage-dotnet-how-to-use-blobs.md), a [táblázat](storage-dotnet-how-to-use-tables.md), a [sorok](storage-dotnet-how-to-use-queues.md)és a [fájlok](storage-dotnet-how-to-use-files.md) tárolására
- Tárterület-architektúra – [Azure tároló: egy könnyen hozzáférhető felhőalapú Tárhelyszolgáltatáshoz az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
