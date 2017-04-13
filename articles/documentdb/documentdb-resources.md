<properties 
    pageTitle="DocumentDB hierarchikus erőforrás modell és fogalmak |} Microsoft Azure" 
    description="Tudjon meg többet az adatbázisok, a gyűjtemények, a felhasználó által definiált függvény (UDF), a dokumentumok, a forrást és további kezeléséhez szükséges engedélyek hierarchikus mintája DocumentDB meg."
    keywords="Hierarchikus modell, documentdb azure, a Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>DocumentDB hierarchikus erőforrás modell és fogalmak

Az adatbázis szervezetek DocumentDB kezelő **erőforrások**néven. Minden erőforrás egy logikai URI egyedileg azonosítja. Szabványos HTTP-műveletek, a kérés/válasz élőfejek és az állapot kódok erőforrásokkal használhatók interaktív módon. 

Ez a cikk olvasásával is az alábbi kérdésekre választ:

- Mit nevezünk DocumentDB tartozó erőforrás modellnek?
- Mik azok a rendszer definiált erőforrásokat, nem pedig a felhasználó által definiált erőforrások?
- Hogyan a erőforrás cím?
- Működése a gyűjtemények
- Hogyan működik a tárolt eljárások, eseményindítók és a felhasználó által definiált függvények (UDF)?

## <a name="hierarchical-resource-model"></a>Hierarchikus erőforrás modell
Az alábbi ábra mutatja be, mint a DocumentDB hierarchikus **erőforrás modell** beállítása alatt egy adatbázis-fiókot, logikai és állandó URI keresztül minden címmel rendelkező erőforrások áll. Erőforrások halmazának továbbiakban néven a **hírcsatorna** ebben a cikkben. 

>[AZURE.NOTE] DocumentDB kínál erősen hatékony TCP-protokoll hozhat létre, amely szintén RESTful a kommunikációt a modellben a [.NET-ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)keresztül érhető el.

![DocumentDB hierarchikus erőforrás modell][1]  
**Hierarchikus erőforrás modell**   

Erőforrások használata kezdéséhez kell [DocumentDB adatbázis-fiók létrehozása](documentdb-create-account.md) az Azure-előfizetést használ. Egy adatbázis-fiókot is állnak az **adatbázisok**, több **gyűjtemények**, viszont, amelyek tartalmazzák tartalmazó **tárolt eljárások, eseményindítók, UDF, dokumentumok** és a kapcsolódó **mellékletek** (előzetes verzió funkció). Adatbázis van társítva **felhasználók**, minden egyes webhelycsoportok, tárolt eljárások, eseményindítók, UDF, dokumentumok vagy mellékletek hozzáférési **engedélyek** tartalmazó. Habár adatbázisok, a felhasználók, az engedélyek és a webhelycsoportok csak a jól ismert sémák erőforrások rendszer által definiált, dokumentumokat és a mellékletek tartalmaz tetszőleges, a felhasználó által definiált JSON tartalom.  

|Erőforrás   |Leírás
|-----------|-----------
|Adatbázis-fiók   |Egy adatbázis-fiókja társítva adatbázisok és a mellékletek (előzetes verzió funkció) blob-tárolóhoz rögzített méretű. Létrehozhat egy vagy több adatbázis fiókok Azure-előfizetése segítségével. További tudnivalókért látogassa meg a [árak, oldal](https://azure.microsoft.com/pricing/details/documentdb/).
|Adatbázis   |Adatbázis a dokumentum tárolási keresztül gyűjtemények particionálva logikai tároló. Érdemes emellett felhasználók tároló.
|Felhasználói   |Hatókör meghatározása engedélyek logikai névtere. 
|Engedély |Egy jóváhagyási jogkivonat, a felhasználó hozzáférést az adott erőforráshoz társított.
|Webhelycsoport |Egy webhelycsoport JSON dokumentumokat és a kapcsolódó JavaScript-alkalmazás logika tároló. A webhelycsoport még egy számlázható egységek, ha a [költség](documentdb-performance-levels.md) határozza meg a webhelycsoport társított teljesítményével. Webhelycsoportok egy vagy több partíciót/kiszolgálón is kiterjedhet, és tároló vagy átviteli gyakorlatilag korlátlan mennyiségű kezelésére is méretezhető.
|Tárolt eljárás   |Alkalmazás logika JavaScript, amely regisztrált egy webhelycsoport és a adatbázismotor belül tranzakción keresztül végre nyelven íródott.
|Eseményindító    |Alkalmazás logikájának végrehajtása előtt vagy után mindkét Beszúrás JavaScript nyelven íródott cseréje vagy a törlési művelet.
|UDF    |Alkalmazás logika JavaScript nyelven íródott. UDF engedélyezze, hogy egy egyéni lekérdezési operátor modell, és egyúttal kiterjesztése az alapvető DocumentDB lekérdezési nyelv.
|Dokumentum   |Felhasználó által definiált (tetszőleges) JSON tartalmat. Alapértelmezés szerint nincs séma kell definiálhatók, sem a másodlagos indexek szükség kell adni egy webhelycsoport felvett összes dokumentumot.
|(Előzetes verzió) Melléklet   |Mellékletet tartalmazó hivatkozások és a külső blob/médiaformátum társított metaadatok speciális dokumentum. A Fejlesztőeszközök választhat a blob DocumentDB kezeli, illetve tárolja azt egy külső blob-szolgáltatónál, például OneDrive, Dropbox stb. 


## <a name="system-vs-user-defined-resources"></a>A rendszer a felhasználó által definiált erőforrások összehasonlítása
Erőforrások adatbázis fiókok, adatbázisok, webhelycsoportok, felhasználók, engedélyek, tárolt eljárások, eseményindítók és UDF – például egy fix séma rendelkeznek, és nevezzük a rendszer-erőforrásokat. Erőforrások, például a dokumentumok és a mellékletek viszont a séma nincsenek korlátozások rendelkezik, és látható, felhasználó által definiált erőforrások. A rendszer és a felhasználó által definiált DocumentDB, az erőforrások jelölt és standard-kompatibilis JSON kezelése. Összes erőforrás, a rendszer vagy felhasználó által definiált, az alábbi közös tulajdonságok van.

> [AZURE.NOTE] Megjegyzés: a rendszer az összes erőforrás tulajdonságok hozza létre az aláhúzásjel _ a JSON ábrázolása a elé.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>A tulajdonság</strong></p></td>
            <td valign="top"><p><strong>A felhasználó állítható be vagy a rendszer által létrehozott?</strong></p></td>
            <td valign="top"><p><strong>Cél</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>A rendszer által létrehozott, az erőforrás egyedi és hierarchikus azonosítója</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>az erőforrás optimista feldolgozási ellenőrzéshez szükséges ETag</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Az erőforrás utolsó frissített időbélyeg</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Az erőforrás egyedi címmel rendelkező URI</p></td>
        </tr>
        <tr>
            <td valign="top"><p>azonosító</p></td>
            <td valign="top"><p>Rendszer által létrehozott</p></td>
            <td valign="top"><p>Felhasználó által definiált (azonos értékű partíciót kulcs) egyedi nevét. Ha a felhasználó nem adja meg a azonosítóját, a azonosító lesz-e a rendszer által létrehozott</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Erőforrások vezetékes ábrázolása
DocumentDB határozza meg, hogy a saját bővítések szeretne a JSON normál vagy speciális kódolások; szabványos kompatibilis JSON dokumentumok működik.  
 
### <a name="addressing-a-resource"></a>Erőforrás megcímezheti
Minden erőforrás címmel rendelkező URI. Az érték egy erőforrás **_self** tulajdonság a relatív, az erőforrás URI jelöli. A URI formátumának áll a /\<hírcsatorna\>/ {_rid} elérési út szegmensek:  

|A _self értéke |Leírás
|-------------------|-----------
|/dbs   |Az adatbázisok adatbázis-fiókot a hírcsatorna
|{dbName} /dbs/  |{DbName} értékével egyező azonosítójú adatbázis
|{dbName} /dbs/ /colls/   |Webhelycsoportok adatbázis a hírcsatornában
|{collName} {dbName} /dbs/ /colls/ |{CollName} értékével egyező azonosítójú gyűjtemény
|{collName} {dbName} /dbs/ /colls/ / dokumentumok    |Dokumentumok gyűjtemény a hírcsatornában
|/dbs/ {dbName} {collName} /colls/ /docs/ {dokumentumazonosító}    |{Dokumentum} értékével egyező azonosítót tartalmazó dokumentum
|{dbName} /dbs/ /users/   |A felhasználók adatbázis a hírcsatorna
|{Felhasználóazonosító} {dbName} /dbs/ /users/   |{Felhasználó} értékével egyező azonosítójú felhasználói
|{Felhasználóazonosító} {dbName} /dbs/ /users/ és engedélyek   |A felhasználó engedélyeinek hírcsatorna
|/dbs/ {dbName} {felhasználóazonosító} /users/ /permissions/ {permissionId}    |{Jogosultsági} értékével egyező azonosítójú engedély
  
Minden erőforrás egy egyedi a felhasználó által definiált nevet az id tulajdonság keresztül elérhetővé tett tartalmaz. Megjegyzés: a dokumentumok, ha a felhasználó nem adja meg a azonosítóját, a támogatott SDK automatikusan létrehozza a dokumentum egy egyedi azonosítót. A azonosítóra a felhasználó által definiált karakterlánc, egy adott szülő erőforrás környezetén belül egyedi legfeljebb 256 karaktert. 

Az egyes erőforrások szintén a rendszer által létrehozott hierarchikus erőforrás-azonosító (más néven egy RID), amely olyan, a _rid tulajdonság keresztül érhető el. A RID kódolja egy adott erőforrás teljes hierarchiáját és a hivatkozási integritás megőrzése elosztott módon használt kényelmes belső megadott. A RID egyedi belül egy adatbázis-fiókot, és belső használja DocumentDB anélkül, hogy több partíciót keresések hatékony útválasztás. A _self és _rid tulajdonságainak értékei alternatív és a kanonikus egy erőforráshoz megadott. 

A DocumentDB REST API-k megcímezheti az erőforrások, és az azonosítóját, illetve a _rid tulajdonságok kérelmek útválasztás támogatja.

## <a name="database-accounts"></a>Adatbázis-fiókok
Egy vagy több DocumentDB adatbázis fiókok Azure-előfizetése segítségével is kiépítése.

Azt is megteheti, [DocumentDB adatbázis-fiókok létrehozása és kezelése](documentdb-create-account.md) az Azure-portált a [http://portal.azure.com/](https://portal.azure.com/)keresztül. Hozhat létre és kezelhet az adatbázis-fiókot rendszergazdai hozzáférése van szükség, és csak az Azure-előfizetése hajtható végre. 

### <a name="database-account-properties"></a>Adatbázis-fiók tulajdonságai
Kiépítési és egy adatbázis-fiókjának kezelését részeként konfigurálása, és olvassa el a következő tulajdonságokat:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Tulajdonság neve</strong></p></td>
            <td valign="top"><p><strong>Leírás</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Egységesebb házirend</p></td>
            <td valign="top"><p>A tulajdonság beállítása csoportban az adatbázis-fiókot a gyűjtemények alapértelmezett konzisztencia szintjét. A konzisztencia szint az [x ms-konzisztencia-szintű] kérelem fejlécet kérés / alapon felülbírálhatja. <p><p>Figyelje meg, hogy ezt a tulajdonságot csak vonatkozik a <i>felhasználó által definiált erőforrásokat</i>. Definiált erőforrások vannak támogatására konfigurált összes rendszer olvasás/lekérdezés erős konzisztencia.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Engedély kulcsok</p></td>
            <td valign="top"><p>Ezek az elsődleges és másodlagos diaminta és a csak olvasható kulcsok, amely az összes az adatbázis-fiókot az erőforrás felügyeleti hozzáférést biztosít.</p></td>
        </tr>
    </tbody>
</table>

Figyelje meg, hogy kiépítési, kívül beállításáról és kezeléséről az adatbázis-fiókot az Azure portálról programozás útján is létrehozhat és DocumentDB adatbázis fiókokat az [Azure DocumentDB REST API-hoz](https://msdn.microsoft.com/library/azure/dn781481.aspx) , valamint a [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)használatával.  

## <a name="databases"></a>Adatbázisok
Egy DocumentDB adatbázisa logikai tároló egy vagy több webhelycsoportok és a felhasználók, a következő ábrán látható módon. Tetszőleges számú ajánlat korlátai DocumentDB adatbázis fiókon adatbázisokat hozhat létre.  

![Fiók és gyűjtemények hierarchikus adatbázismodell][2]  
**A felhasználók és webhelycsoportok logikai tároló egy adatbázis**

Adatbázis szerint gyűjtemények, amelyek a bennük lévő dokumentumok tranzakció tartományához particionálva gyakorlatilag korlátlan dokumentumok tárolására is tartalmazhat. 

### <a name="elastic-scale-of-a-documentdb-database"></a>DocumentDB adatbázis skála rugalmas
Egy DocumentDB adatbázisa rugalmas alapértelmezésben – petabytes biztonsági SSD dokumentumok tárolása és kiépített átviteli néhány GB kezdve. 

Az egyetlen számítógépen nem korlátozódik a hagyományos RDBMS adatbázison eltérően DocumentDB az adatbázis. Az DocumentDB az alkalmazás skála kell a nagyobb, mint készíthet több gyűjtemények, adatbázisok vagy mindkettőt. Valójában különböző első alkalmazásait belül a Microsoft használták DocumentDB fogyasztói méretarány szerint a dokumentum tárolási terabájt az egyes webhelycsoportok tartalmazó ezer nagyon nagy méretű DocumentDB az adatbázisok létrehozása. A nagyobb, vagy kisebb adatbázis hozzáadásával és eltávolításával gyűjtemények az alkalmazás skála követelményeknek. 

Tetszőleges számú gyűjteményt belül egy adatbázist, amelyekre az ajánlat hozhat létre. Minden egyes webhelycsoport biztonsági SSD tárterület és a kiépítve várja, attól függően, hogy a kijelölt teljesítmény réteg átviteli tartalmaz.

A felhasználók tároló DocumentDB adatbázis is. A felhasználóhoz, a bekapcsolása, egy logikai névteret engedélyek csoportja, amely külön hitelesítés és webhelycsoportok, a dokumentumok és a mellékletek hozzáférést biztosít.  
 
A DocumentDB erőforrás modell más források, az adatbázisok hozhatók létre, helyett, törölt, olvassa el vagy felsorolásos egyszerűen használatával [Azure DocumentDB REST API -k](https://msdn.microsoft.com/library/azure/dn781481.aspx) és az [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)közül. DocumentDB garantálja az olvasásra vagy a metaadatok egy adatbázis erőforrás lekérdezése erős konzisztencia. Adatbázis automatikus törlése biztosítja, hogy nem tud hozzáférni a webhelycsoportok vagy a felhasználók benne.   

## <a name="collections"></a>Webhelycsoportok
A JSON-dokumentumait tároló egy DocumentDB gyűjteménye. Egy webhelycsoport tranzakciók és lekérdezések skála időegység is. 

### <a name="elastic-ssd-backed-document-storage"></a>Rugalmas SSD biztonsági dokumentumok tárolása
Egy webhelycsoport alapvetően rugalmas - automatikusan nő, és zsugorodik össze, mint szeretne hozzáadni vagy eltávolítani a dokumentumokat. Webhelycsoportok logikai erőforrásokat, és egy vagy több fizikai partíciót vagy a kiszolgálón is kiterjedhet. Egy webhelycsoport belül partíciók száma DocumentDB tárhely méretét és a webhelycsoport kiépített átviteli alapján határozza meg. DocumentDB az összes partíciót van társítva SSD biztonsági tároló rögzített méretű, és az elérhetőség magas replikált van. Partition management Azure DocumentDB teljesen kezeli, és nem rendelkezik összetett kódíráshoz, és kezelheti a partíciók. Webhelycsoportok DocumentDB **gyakorlatilag korlátlan** tárhely és a teljesítmény. 

### <a name="automatic-indexing-of-collections"></a>Automatikus indexelés gyűjtemények
DocumentDB rendszer igaz séma ingyenes adatbázis. Tegyük fel, vagy nem minden séma megkövetelése a JSON dokumentumokat. Dokumentumok elhelyezése a gyűjteményt, mint DocumentDB automatikusan indexek őket, és azok érhetők el, hogy a lekérdezés. Automatikus indexelés dokumentumok séma vagy másodlagos indexek nélkül a fő videofunkcióinak DocumentDB és írási optimalizált, a zárolás ingyenes és a napló strukturált index karbantartási technikákkal engedélyezve van. DocumentDB tartós mennyiségű rendkívül gyors írás közben egységes lekérdezések továbbra is támogat. Dokumentum és a tárgymutató tárolása kiszámítása az elfogyasztott minden egyes webhelycsoport tárolási szolgálnak. Megadhatja, hogy a tárhely és a teljesítmény kompromisszumok indexelés konfigurálása a webhelycsoport indexelési házirendjét társított. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Az indexelési gyűjtemény házirend beállítása
Az indexelési házirendet az egyes lehetővé teszi az indexelés társított teljesítmény és a tárhely kompromisszumok. Az alábbi lehetőségek állnak rendelkezésre Önnek az indexelő konfigurációs részeként:  

-   Válassza ki, hogy a webhelycsoport automatikusan indexek összes dokumentumot vagy sem. Alapértelmezés szerint minden dokumentum automatikusan indexelve vannak. Megadhatja, hogy kapcsolja ki az Automatikus indexelés és a szelektív hozzáadása csak bizonyos dokumentumtípusok esetén a tárgymutató. Viszont szelektív megadhatja kizárása csak bizonyos dokumentumtípusok esetén. Ez a igaz vagy hamis, a gyűjtemény a indexingPolicy kell automatikus tulajdonság beállításával, és az [x-ms-indexingdirective] kérelem élőfej beszúrása, cseréje vagy törlése a dokumentum közben érhet el.  
-   Válassza ki, hogy részösszegekbe bizonyos elérési utak vagy a minta a dokumentumokban az indexből. Érhet el a beállítás includedPaths, és kattintson a gyűjtemény indexingPolicy excludedPaths rendre. A tárhely és a teljesítmény kompromisszumok tartomány- és kivonat lekérdezések adott elérési út mintákat is beállítható. 
-   Válassza a szinkronizált között (egységes) és aszinkron (Lusta) index frissítéseket. Alapértelmezés szerint az index frissítése szinkron minden Beszúrás, cseréje vagy törlése a webhelycsoport egy dokumentumot. Lehetővé teszi, hogy a dokumentum olvasása konzisztencia azonos szintű elfogadja a lekérdezések. DocumentDB optimalizált írási és támogatja a dokumentum írások szinkron index karbantartási és egységes lekérdezések szolgáló tartós mennyiségű, miközben az index lazily frissítéséhez egyes webhelycsoportok is beállíthatja. Lusta indexelés a hanghatások írási további és tömeges bevitel esetek elsősorban olvasási-nehéz gyűjtemények ideális.

Az indexelési házirend végrehajtása egy helyezése a gyűjtemény a módosíthatják. Ez a érhető el vagy az [ügyfél SDK csomagjában talál](https://msdn.microsoft.com/library/azure/dn781482.aspx), az [Azure portál](https://portal.azure.com) vagy az [Azure DocumentDB REST API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Egy webhelycsoport lekérdezésére.
A dokumentuma egy webhelycsoport beállíthatja, hogy a sémák tetszőleges és anélkül, hogy minden olyan séma vagy upfront másodlagos tárgymutatók használata egy webhelycsoport tárolt dokumentumokra is lekérdezhetők. A gyűjtemény a [DocumentDB SQL-szintaxisa](https://msdn.microsoft.com/library/azure/dn782250.aspx), amely biztosítja rich hierarchikus, relációs, és a térbeli operátorok és a JavaScript-alapú UDF keresztül bővítési is lekérdezhetők. JSON nyelvhelyesség-ellenőrzés lehetővé teszi, hogy a fastruktúrájú csomópontok adatfeliratokkal JSON fák dokumentumokat modellezési. Ez lehet kihasználni egyaránt DocumentDB's Automatikus indexelés technikák, valamint a DocumentDB's SQL helyesírását. A DocumentDB lekérdezésnyelv három fő szempont áll:   

1.  Lekérdezési műveletek a fastruktúra, többek között a hierarchikus lekérdezések és előrejelzést természetesen megfeleltetése kis csoportja. 
2.  Relációs műveletek, beleértve a kialakítási, szűrése, előrejelzések, összegzések és saját illesztések egy részét. 
3.  Csak a JavaScript függvényekben használható (1) és (2) alapján.  

A DocumentDB lekérdezés modell megkísérli a használható funkciók körét, a hatékonyság és az egyszerűség kedvéért kiegyensúlyozzák. A DocumentDB adatbázismotor natív módon lefordítja, és végrehajtja a lekérdezés SQL-utasításait. Egy webhelycsoport az [Azure DocumentDB REST API-hoz](https://msdn.microsoft.com/library/azure/dn781481.aspx) , vagy az [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)használatával is lekérdezhetők. A .NET SDK LINQ szolgáltató megtalálható.

> [AZURE.TIP] Próbálja ki az DocumentDB, és SQL-lekérdezések futtatása a a [Lekérdezés játszótéri](https://www.documentdb.com/sql/demo)az adatkészlet ellen.

### <a name="multi-document-transactions"></a>Több dokumentum tranzakciók
Adatbázis-tranzakciók programozási csökkentett és kiszámítható modell biztosít az adatok egyidejű módosítása foglalkozik. RDBMS, az üzleti logikai funkcióinak írni a hagyományos módon is írása a **tárolt eljárás** , illetve **Eseményindítók** , és töltse tranzakció alapú végrehajtásához az adatbázis-kiszolgáló. A RDBMS az alkalmazás programozási a két elemezve programozási nyelven kezeléséhez szükséges: 

- (Nem-tranzakció alapú) alkalmazásprogramozási nyelvet (például JavaScript, Python, C#, Java stb.)
- Kétmintás T-SQL nyelvben, a tranzakció alapú programnyelv, amely az adatbázis natív módon végrehajtása

Alapján a mély lekötési JSON-és JavaScript közvetlenül a adatbázismotor belül DocumentDB biztosít egy ötletes programozási modell JavaScript-alapú alkalmazás logikájának végrehajtása közvetlenül a tárolt eljárások és indítók gyűjtemények. Ez lehetővé teszi a következők valamelyikét:

- Feldolgozási hatékony végrehajtásának szabályozni, a JSON objektum sokféle közvetlenül a adatbázismotor indexeléséhez az automatikus helyreállítás
- Természetesen a control flow kifejező, változó hatókör meghatározása, hozzárendelés és kezelése az adatbázis-tranzakciók közvetlenül a JavaScript programozási nyelven értelmez primitívek kivétel integrációja

A JavaScript logika a webhelycsoport szintjén regisztrált majd állíthatnak az adott webhelycsoport adatbázis-műveletek a dokumentumokon. DocumentDB implicit módon fussa a JavaScript-alapú tárolt eljárások és -pillanatfelvétel elkülönítési környezeti savas tranzakciók belül indítók belül egy webhelycsoport dokumentumok között. A végrehajtás során Ha a JavaScript okoz a kivétel, majd a teljes tranzakció megszakad. Az eredményül kapott programozási modell rendkívül egyszerű még hatékony. JavaScript fejlesztők számára beolvasása a "tartós" programozási modell továbbra is a már jól ismert nyelvi elem és a tár primitívek használata közben.   

Az azt jelenti, hogy hajtsa végre a JavaScript közvetlenül elérhető jelentését az azonos címterület a pufferelési készletre, az adatbázis-vezérlő lehetővé tevő performant tranzakció alapú gyűjtemény szemben a dokumentumok adatbázis-műveletek végrehajtása. Továbbá DocumentDB adatbázismotort lehetővé teszi a mély lekötési a JSON-ba, és a JavaScript megszünteti a típus rendszerek, az alkalmazás és az adatbázis bármelyik impedancia nem egyezik.   

Miután létrehozott egy webhelycsoport, rögzítheti tárolt eljárások, eseményindítók és UDF kötött az [Azure DocumentDB REST API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx) vagy bármely, az [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)gyűjteménye. Regisztráció után hivatkozást, és hajtsa végre azokat. Vegye figyelembe az alábbi tárolt eljárás csupán a JavaScript írt, alábbi kód megnyitja a két argumentuma (a címjegyzék neve és a szerző neve), és létrehoz egy új dokumentumot, dokumentum lekérdezése és majd frissíti – implicit savas tranzakción belül az összes. A végrehajtás során bármely pontján a JavaScript kivétel, ha a teljes tranzakció megszakítja.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

Az ügyfél is "kiszállítása" a fenti JavaScript logika tranzakció alapú végrehajtás keresztül HTTP-BEJEGYZÉST az adatbázist. HTTP módszerekkel kapcsolatos további tudnivalókért olvassa el a [DocumentDB erőforrásokkal RESTful kapcsolati](https://msdn.microsoft.com/library/azure/mt622086.aspx)című témakört. 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


Figyelje meg, hogy az adatbázis natív módon JSON és a JavaScript értelmezésére képes, mert nem áll fenn nincs rendszer nem egyezik, "OR megfeleltetés" vagy kód generációs mágikus szükséges.   

Tárolt eljárások és eseményindítók használata egy gyűjtemény és a dokumentumokat egy webhelycsoport pontosan meghatározott objektummodell, amelyek az aktuális környezetben a webhelycsoport közzététele keresztül.  

Webhelycsoportok DocumentDB a hozhatók létre, a törölt, olvassa el vagy felsorolásos egyszerűen a következők egyikét használja, az [Azure DocumentDB REST API-hoz](https://msdn.microsoft.com/library/azure/dn781481.aspx) , vagy az [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB mindig itt erős konzisztencia olvasásra vagy a metaadatok egy webhelycsoport lekérdezésére. Automatikus törlése a webhelycsoport biztosítja, hogy nem tud hozzáférni a dokumentumok, mellékleteket, tárolt eljárások, eseményindítók, és UDF benne.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Tárolt eljárások, eseményindítók és a felhasználó által definiált függvények (UDF)
Az előző szakaszban leírtak alkalmazás logika futtatásához a adatbázismotor belül tranzakción belül közvetlenül is írhat. Az alkalmazás logika csupán a JavaScript írhatók, és a tárolt eljárás, eseményindító vagy egy UDF lehet modellezni. A tárolt eljárás vagy az eseményindító JavaScript-kód beszúrása, cseréje, törölni, a webhelycsoport olvasási vagy lekérdezés dokumentumaiban. Kézzel a JavaScript belül egy UDF nem beszúrása, cseréje vagy törlése dokumentumokat. UDF számba veheti a webhely egy lekérdezés eredményhalmazában dokumentumokat, és egy másik eredményhalmaz kiszámítására. A több elem bérleti DocumentDB kényszeríti a szigorú foglalás alapú erőforrás irányítási. Minden egyes tárolt eljárás, eseményindító vagy egy UDF kapja a rögzített kvantum az operációs rendszer erőforrások végezze el a munkát. Továbbá tárolt eljárások, eseményindítók vagy UDF nem csatolhat külső JavaScript-tárak ellen, és is a számukra kiosztott erőforrás költségvetést meghaladó feketelistára teszi. Regisztráció, tárolt eljárások, eseményindítók vagy UDF unregister a gyűjtemény a REST API-k használatával.  Regisztráció után a tárolt eljárás, eseményindító vagy egy UDF előre lefordított és bájt kód, amely később hajtsa végre tárolja. A következő szakasz bemutatják, hogyan használhatja a DocumentDB JavaScript SDK regisztrálásához, hajtsa végre, és a tárolt eljárás, a kiváltó ok mező és egy UDF. A JavaScript SDK csomagjában talál egy egyszerű csomagolópapír, a [DocumentDB REST API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx)fölé. 

### <a name="registering-a-stored-procedure"></a>Regisztrálás a tárolt eljárás
Regisztráció a tárolt eljárás új tárolt eljárás erőforrás egy webhelycsoport keresztül HTTP POST hoz létre.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>A tárolt eljárás végrehajtása
A tárolt eljárás végrehajtását azáltal szemben a tárolt eljárás meglévő erőforrás HTTP POST paraméterek átadása összehívás törzsébe az eljárás szerint történik.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>A tárolt eljárás regisztrációjának
A tárolt eljárás regisztrációjának azáltal, szemben a tárolt eljárás meglévő erőforrás-HTTP törlése egyszerűen történik.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Az eseményindító előtti rögzítése
Eseményindító regisztráció: hozzon létre egy új eseményindító erőforrás keresztül HTTP POST gyűjtemény a történik. Megadhatja, ha az eseményindító egy előtti vagy utáni eseményindító és a művelet típusát (például létrehozás, a csere, törlése vagy az összes) társított lehet.   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Az eseményindító előtti végrehajtása
Az eseményindító végrehajtását meglévő eseménykód nevét a bejegyzés és helyezése/törlése kérelem egy dokumentum erőforrás keresztül a kérés fejléce kibocsátó időben megadásával történik.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Az eseményindító előtti regisztrációjának
Az eseményindító regisztrációjának keresztül kiállító ellen eseményindító meglévő erőforrás-HTTP törlése egyszerűen történik.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Egy UDF rögzítése
Regisztráció a udf: hozzon létre egy új UDF erőforrás keresztül HTTP POST gyűjtemény a történik.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>A lekérdezés részeként egy UDF végrehajtása
Egy UDF adható meg az SQL-lekérdezés részeként, és lesz az alapvető [SQL lekérdezési nyelv DocumentDB a](https://msdn.microsoft.com/library/azure/dn782250.aspx)kiterjesztése lehetőséget.

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Egy UDF regisztrációjának 
Egy UDF regisztrációjának ellen UDF meglévő erőforrás-HTTP törlése azáltal egyszerűen történik.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Bár a fenti kódrészletek mutatott a regisztrációs (KÜLDÉS), rendszerleíró adatbázisból való (helyezése), (GET) listaolvasás és végrehajtása (KÜLDÉS) a [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js)keresztül, a [REST API-hoz](https://msdn.microsoft.com/library/azure/dn781481.aspx) vagy a többi [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)is használhatja. 

## <a name="documents"></a>Dokumentumok
Beszúrása, cseréje, törölheti, olvassa el, számba veheti a webhely és a lekérdezés egy webhelycsoport tetszőleges JSON dokumentumokat. DocumentDB határozza meg, hogy minden olyan séma, és nincs szüksége a másodlagos indexek támogatja a fölé dokumentumok egy webhelycsoport lekérdezésére.   

Hogy valóban megnyitott adatbázis szolgáltatás, DocumentDB nem készlet bármelyik speciális adattípusok (például dátum-idő) vagy az adott kódolások JSON dokumentumok. DocumentDB nincs szükség a különböző dokumentumok; közötti kapcsolatok kodifikálni bármely speciális JSON szabályok az SQL-szintaxisa DocumentDB lekérdezés és a project dokumentumokhoz speciális széljegyzetek és dokumentumok közötti kapcsolatok kodifikálni kell nélkül operátorok megkülönböztető tulajdonságok nagyon nagy teljesítményű hierarchikus és relációs lekérdezés biztosít.  
 
Az összes más forrásokból dokumentumokat hozhat létre, lecserélve, törölt, olvasási, felsorolásos és lekérdezett egyszerűen a REST API-k és az [ügyfél SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)bármelyikét használja. Azonnali törlése a dokumentum szabadítja fel a megfelelő összes beágyazott mellékletet kvóta. Dokumentumok olvasási konzisztencia szintjét konzisztencia házirend következik az adatbázis-fiókot. Ezzel a házirenddel attól függően, hogy az alkalmazás konzisztencia adatszolgáltatási kérelem alapon felülírható. Lekérdezés dokumentumokat, ha az olvasási következetességét az indexelési mód beállítása a webhelycsoport követi. Az "egységes" követi a fiók konzisztencia házirend. 

## <a name="attachments-and-media"></a>Mellékletek és médiaklipek
>[AZURE.NOTE] Melléklet- és erőforrások előzetes funkciók állnak.
 
DocumentDB lehetővé teszi, hogy bináris BLOB/media DocumentDB vagy a saját távoli media áruház tárolhatja. Lehetővé teszi a metaadatok egy mellékletek nevű speciális dokumentum értelmez a médiafájlok képviseli. A DocumentDB egy mellékletet máshol tárolt media/blob hivatkozó speciális (JSON) dokumentum. Melléklet egyszerűen speciális dokumentum, amely jellemzi a médiafájlok egy távoli media a tárolóban lévő metaadatok (például a hely, a szerző stb.). 

Fontolja meg egy közösségi olvasási alkalmazást használó DocumentDB szabadkézi széljegyzetek tárolására, és megjegyzések, beleértve a metaadat-alapú kiemeli, könyvjelzőket, a minősítések, a tetszésnyilvánítások/dislikes az adott felhasználó egy e-címjegyzék tartozó stb.   

-   A tartalom a könyv magát megtalálható a multimédia-tárolóban vagy egy távoli media tároló vagy DocumentDB adatbázis-fiókot részeként használható. 
-   Az alkalmazás a distinct dokumentumként minden felhasználó metaadatokat tárolhatnak előfordulhat, hogy – például a Munka1 Mintaügyvezető a metaadat-alapú /colls/joe/docs/book1 által hivatkozott dokumentum tárolja. 
-   Mutasson a tartalmi lapok az adott felhasználó könyv mellékletek csoportban a megfelelő dokumentum, például az /colls/joe/docs/book1/chapter1 vannak tárolva /colls/joe/docs/book1/chapter2 stb. 

Figyelje meg, hogy a fenti példákban rövid azonosítók az erőforrás-hierarchia bajlódnia. Erőforrások érhetők el a REST API-k egyedi erőforrás-azonosítók keresztül keresztül. 

A _media tulajdonságot, a melléklet adathordozóját DocumentDB kezeli hivatkoznak médiafájlokat a URI. DocumentDB biztosítja médiafájlokat szemét való összegyűjtése, ha minden függőben lévő hivatkozásokat megszakadnak. DocumentDB automatikusan létrehozza a melléklet új médiafájlokat feltöltésekor és feltölti a _media, mutasson az újonnan hozzáadott multimédiás. Ha úgy dönt, hogy tárolja a médiafájlokat a távoli blob-tárolóban (például OneDrive, Azure tárolására, DropBox stb) Ön által felügyelt, továbbra is használható a mellékletek médiafájlokat hivatkozni szeretne. Ebben az esetben fognak létrehozni a mellékletet, és _media tulajdonsága feltöltéséhez.   

Más erőforrások, mint a mellékletek hozhatók létre, helyett, a törölt, olvassa el vagy felsorolásos egyszerűen a REST API-k és az ügyfél SDK bármelyikét használja. Dokumentumok, a mellékletek olvasási konzisztencia szintjét követi a konzisztencia házirend meg az adatbázis-fiókot. Ezzel a házirenddel attól függően, hogy az alkalmazás konzisztencia adatszolgáltatási kérelem alapon felülírható. Mellékletek lekérdezése, amikor az olvasási következetességét követi az indexelési mód beállítása a webhelycsoport. Az "egységes" követi a fiók konzisztencia házirend. 
 
## <a name="users"></a>Felhasználók
Egy DocumentDB felhasználói engedélyek csoportosítása egy logikai névteret jelöli. Egy identitáskezelő rendszer vagy egy előre definiált alkalmazás szerepkör DocumentDB felhasználó egy felhasználóhoz, előfordulhat, hogy megfelelnek. DocumentDB egy felhasználói engedélyek területen az adatbázis csoportja csoportosításához absztrakciója egyszerűen jelenti.   

Az alkalmazás több bérleti végrehajtásához felhasználók létrehozhat az DocumentDB, amely megfelel a tényleges felhasználóknak vagy a bérlők az alkalmazás. Létrehozhat az adott felhasználó engedélyeinek, amelyek megfelelnek a hozzáférés-vezérlés keresztül különféle gyűjtemények, dokumentumok, mellékletek stb.   

Az alkalmazások kell a felhasználói NÖV méretezése, mint az adatok shard különféle módokon is elfogadja. Mindegyik felhasználónak a következőképpen modellezheti:   

-   Minden olyan felhasználóhoz rendeli hozzá adatbázis.
-   A webhelycsoport minden olyan felhasználóhoz rendeli hozzá. 
-   Több felhasználó tartozó dokumentumok egy dedikált gyűjtemény megnyitásához. 
-   Több felhasználó tartozó dokumentumok gyűjtemények halmazának megnyitásához.   

Függetlenül attól, kiválaszthatja, hogy az adott sharding stratégia a tényleges felhasználók modell felhasználóként DocumentDB adatbázisban, és minden felhasználó számára jól nyomtatott engedélyek hozzárendelése.  

![Felhasználói gyűjtemények][3]  
**Sharding stratégiák és modellezése felhasználók**

Más erőforrások, például DocumentDB felhasználók hozhatók létre, helyett, a törölt, olvassa el vagy felsorolásos egyszerűen a REST API-k és az ügyfél SDK bármelyikét használja. DocumentDB mindig az olvasásra vagy a metaadatok az erőforrás-felhasználói lekérdezés erős konzisztencia biztosít. Érdemes mutató, hogy nem tud hozzáférni az engedélyek, benne biztosítja, hogy automatikusan a felhasználó törlése. Annak ellenére, hogy a DocumentDB reclaims kvóta az engedélyek, a háttérben a törölt felhasználói részeként, a törölt engedélyek érhető el azonnali újra szeretne használni.  

## <a name="permissions"></a>Engedélyek
A egy access vezérlő szempontjából erőforrások, például az adatbázis-fiók esetén adatbázisok, a felhasználók és engedélyek számítanak *felügyeleti* erőforrások, mivel ezek a rendszergazdai engedélyek szükségesek. Kézzel források, többek között a webhelycsoportok, dokumentumok, mellékleteket, tárolt eljárások, eseményindítók és UDF hatóköre egy adott adatbázis csoportban, és *alkalmazás erőforrások*tekinteni. Erőforrások és a szerepköröket elérheti őket (azaz a rendszergazda és a felhasználó) két típusának megfelelő, a engedélyezési modell határozza meg, kétféle *hívóbetűk*: *minta kulcs* és *erőforrás-kulcs*. A fő része: az adatbázis-fiókot és billentyű kapja meg a Fejlesztőeszközök (vagy a rendszergazda) ki van kiépítési az adatbázis-fiókot. A diaminta kulcs rendszergazda szemantikáját, tartalmaz, annak, hogy használható felügyeleti és a alkalmazás erőforrások elérésének engedélyezése. Egy erőforrás kulcsa viszont részletes hívóbetű, amely lehetővé teszi az *adott* alkalmazás erőforráshoz való hozzáférés. Tehát azt a felhasználót az adatbázis és az engedélyeket, a felhasználónak van egy adott erőforrás (pl. webhelycsoport, dokumentum, melléklet, tárolt eljárás, eseményindító vagy UDF) közötti kapcsolatra rögzíti.   

Jelenleg csak egy erőforrás kulcs beszerzése van az adott felhasználó jogosultsági erőforrás létrehozásával. Megjegyzendő, hogy a létrehozásához az jogosultsági beolvasásához, vagy egy minta kulcsot be kell mutatni a engedélyezési fejlécében. A jogosultsági erőforrás kötelékek az erőforrás, az access és a felhasználó. Miután létrehozott egy jogosultsági erőforrás, a felhasználó csak be kell mutatnia a hozzárendelt erőforrás billentyűt annak érdekében, hogy a megfelelő erőforrás eléréséhez szükséges. Egy erőforrás kulcs így megtekintheti, logikai és kisméretű ábrázolása a jogosultsági erőforrás.  

Egyéb erőforrást, az engedélyek DocumentDB az hozhatók létre, helyett, a törölt, olvassa el vagy felsorolásos egyszerűen a REST API-k és az ügyfél SDK bármelyikét használja. DocumentDB mindig itt erős konzisztencia olvasásra vagy jogosultsági metaadatait lekérdezésére. 

## <a name="next-steps"></a>Következő lépések
További tájékoztatás az erőforrások HTTP [DocumentDB erőforrásokkal RESTful kapcsolati](https://msdn.microsoft.com/library/azure/mt622086.aspx)paranccsal.


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

