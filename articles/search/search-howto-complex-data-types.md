<properties
    pageTitle="Hogyan összetett adattípusok Azure keresés modell |} Microsoft Azure-keresés"
    description="Beágyazott vagy hierarchikus adatszerkezetek is modellezni sík sorhalmazon és webhelycsoportok adattípus Azure keresési index."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Hogyan modell összetett adattípusok az Azure-keresés

Azure keresési index néha feltöltéséhez használt külső adatkészleteket, amely nem szétválasztani szépen táblázatos sorhalmazt hierarchikus vagy beágyazott alépítményeit tartalmazzák. Példák a ilyen szerkezetek előfordulhat, hogy több helyek és telefonszámok tartalmazzák az egyetlen ügyfél több színét és méretét több szerző a egyetlen füzet egyetlen Termékváltozatot, és így tovább. Modellezési kifejezéseket, az a továbbiakban *összetett adattípusok*, *összetett adattípusok*, *az összetett adattípusok*vagy *adattípusok összesítése*, néhány neve szerkezetek jelenhet meg.

Összetett adattípusok natív módon nem támogatottak az Azure keresési, de a kerülő megoldást kipróbált tartalmaz a szerkezet összeolvasztása, és a **webhelycsoport** adattípus használatával a belső szerkezet pótlására lépéssel. A jelen cikkben ismertetett technika követő lehetővé teszi a tartalom szeretne keresni, síklapos, rendezhetők és szűrhetők.

## <a name="example-of-a-complex-data-structure"></a>Példa összetett adatstruktúra

Általában a szóban forgó tárolt adatokhoz JSON vagy XML-dokumentumok formájában, valamint DocumentDB például egy NoSQL tárolóban található elemek. A kérdés szerkezetileg több alárendelt elemeket szeretne keresni és a szűrt kell bejelentkezésen törzs.  A bemutató a megoldáshoz kiindulási pontként a következő JSON-dokumentumot, amely felsorolja a partnerek halmazának példaként vennie:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Miközben a mezők neve "azonosító", "név" és "társaság" egyszerűen csatolható belül Azure keresési index mezőként egy az egyhez, és a "helyek" mező tartalmazza a helyek, hogy mindkét egy csoportja hely azonosítók, valamint a hely leírását tömb. Tekintve, hogy az Azure-keresés nem rendelkezik, amely támogatja ezt adattípus, egy másik módszert ez Azure keresési modell szükséges. 

> [AZURE.NOTE] Ez a módszer is le Kirk Evans egy blogbejegyzésbe [Indexelés DocumentDB az Azure keresés](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), mely megjelenítése technika neve "összeolvasztása az adatok", amellyel azt szeretné, hogy egy nevű mező `locationsID` és `locationsDescription` , amelyek mindkét [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok tömbje).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>1 rész: A tömb összeolvasztása az egyes mezők

Azure keresési index, amely a adatkészlet alkalmazkodik, hozzon létre a beágyazott alépítményhez az egyes mezők: `locationsID` és `locationsDescription` [gyűjtemények](https://msdn.microsoft.com/library/azure/dn798938.aspx) (vagy karakterláncok tömbje) adattípusú. Ebben a mezőben volna indexelést állít be az "1" és "2" értéket be a `locationsID` be Ambrus Zsolt és a "3" & "4" értékek mezőben az `locationsID` Jen Campbell mezőt.  

Az adatok belül Azure keresési így néz ki: 

![mintaadatok, 2 sorból](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>2 rész: A tárgymutató-definícióban webhelycsoport mező felvétele

A tárgymutató sémában a mező definíciók néz ebben a példában.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Keresés működésének ellenőrzése, és tetszés szerint a az index bővítése

Feltételezve, hogy a létrehozta az indexet, és az adatok betöltése, most tesztelheti keresési lekérdezés-végrehajtási szemben az adatkészlet ellenőrizze a megoldást. Minden egyes **webhelycsoport** írjuk **kereshető**, **szűrhető** és **facetable**. Látnia kell például lekérdezések futtatása:

* Keresse meg az Adventureworks központjában dolgozó összes személy.
* A Kezdőlap Office dolgozó személyek száma megszámlálása.  
* A Kezdőlap Office dolgozó személyek milyen más irodák dolgoznak együtt minden olyan helyen, a személyek számának megjelenítése  

Ha ez a módszer is széthúzhatja esik esetén kell tennie a keresés, mind a helyét azonosító, valamint a tartózkodási hely kombináló. Példa:

* Összes személy megkeresése, ahol rendelkeznek a Kezdőlap Office és az egy helyét azonosító 4.  

Ha visszahívja az eredeti tartalom volt a jelennek meg:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Jó helyen jár, most, hogy azt van elválasztva az adatokat külön mezőkbe, azt nem áll módjukban afelől, hogy ha az otthoni Office Jen Campbell vonatkozik a `locationsID 3` vagy `locationsID 4`.  

Ebben az esetben kezelendő definiálása egy másik mező az indexet, amely az összes adatot egyesít egyetlen gyűjteménye.  Példa azt visszahívja ebben a mezőben `locationsCombined` azt elválasztja a tartalmat, és egy `||` de megadható, amelyek úgy gondolja, hogy minden olyan elválasztó karaktereket a tartalom számára a szülőwebhelyhez lenne. Példa: 

![mintaadatok, 2 sorból az elválasztó](./media/search-howto-complex-data-types/sample-data-2.png)

Ennek használatával `locationsCombined` mező, akkor most rendezhetők még több lekérdezések, például:

* Látható a "Kezdőlap Office" helyét azonosító "4" dolgozó személyek száma.  
* A Kezdőlap Office dolgozó személyek keresése helyét azonosító "4". 

## <a name="limitations"></a>Korlátozások

Ez a módszer akkor hasznos, ha olyan esetek számát, de még nem minden esetben alkalmazható.  Példa:

1. Ha nem rendelkezik egy statikus set mezők az összetett adattípus, és feleltesse meg a lehetséges típusok egyetlen mezőből nincs mód volt. 
2. Néhány további munkát határozza meg, hogy mit kell frissíteni kell a Azure keresési index frissítése a beágyazott objektumok igényel.

## <a name="sample-code"></a>Minta kódot.

Példa egy összetett JSON adathalmaz index Azure keresés és a lekérdezések száma végezhet az ebben a [GitHub repó](https://github.com/liamca/AzureSearchComplexTypes)a adatkészlet megjelenik.

## <a name="next-step"></a>Következő lépés

[Összetett adattípusok natív támogatása szavazzon](https://feedback.azure.com/forums/263029-azure-search) az Azure keresési UserVoice lapon, és bármilyen további adatbevitelt, amelyeket szeretne nekünk szolgáltatás végrehajtásával kapcsolatos szempontok. Akkor is is keresse meg a rólam közvetlenül a Twitteren, a @liamca.


 