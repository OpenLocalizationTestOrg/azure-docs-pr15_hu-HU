<properties
    pageTitle="Azure SQL-adatbázis JSON szolgáltatások |} Microsoft Azure"
    description="Azure SQL-adatbázis elemzési, lekérdezések és adatok formázása a JavaScript objektum jelölés (JSON) jelöléssel lehetővé teszi."
    services="sql-database"
    documentationCenter=""
    authors="jovanpop-msft"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/17/2016"
    ms.author="jovanpop"
   ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Azure SQL-adatbázisban JSON szolgáltatások – első lépések

Azure SQL-adatbázis segítségével elemezni JavaScript Objektumjelrendszer [(JSON)](http://www.json.org/) formátumban adatok a lekérdezés és a relációs adatok exportálása JSON szövegként.

JSON az adatcsere modern webes és mobilalkalmazások használt népszerű adatok formátum. JSON félig strukturált adatok tárolására, naplófájlok vagy NoSQL adatbázisok, például [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)is szolgál. Sok többi webszolgáltatásokhoz eredmények JSON a szövegként formázott vagy fogadja el az adatok JSON formátumú. Leginkább Azure szolgáltatások, például a [Azure keresés](https://azure.microsoft.com/services/search/), [Azure-tárhely](https://azure.microsoft.com/services/storage/)és [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) vissza, vagy JSON kimerítheti többi végpontok van.

Azure SQL-adatbázis segítségével egyszerűen JSON-adatok használata és az adatbázis integráció a modern szolgáltatásaival.

## <a name="overview"></a>– Áttekintés

Azure SQL-adatbázisban az alábbi funkciókat nyújt a JSON-adatok használata:

![JSON függvények](./media/sql-database-json-features/image_1.png)

Ha JSON szöveg, adatok kinyerése JSON, vagy ellenőrizze, hogy JSON megfelelően formázott [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx) [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx)és [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx)a beépített függvények használatával. A [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) függvény lehetővé teszi a belül JSON szöveges érték frissítése. További speciális lekérdezési és elemzési, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) függvény is alakíthat ki egy tömb JSON-objektumok sorok csoportját. Bármely SQL-lekérdezés eredménye beállítása futtatható. Végül pedig a [Vonatkozó JSON](https://msdn.microsoft.com/library/dn921882.aspx) záradékot, amely lehetővé teszi, hogy JSON szövegként a relációs táblákban tárolt adatok formázása.

## <a name="formatting-relational-data-in-json-format"></a>Relációs adatok formázása JSON formátumban
Ha egy webes szolgáltatás, hogy tart az adatbázisból származó adatok réteg, és itt választ a JSON formázhatja, vagy ügyféloldali JavaScript keretek vagy tárat, amellyel adatokat fogadjon, formázása: JSON, közvetlenül az SQL-lekérdezés az adatbázis tartalmát formázhatja, JSON. Már nem van való kódírás alkalmazás, amely formátumok Azure SQL-adatbázissal, JSON származó találatok jelenjenek meg, vagy néhány JSON szerializációs tár átalakítása táblázatos lekérdezés eredményeit, és majd szerializálni JSON formátum objektumok tartalmazza. Ehelyett is használhatja a vonatkozó JSON záradék SQL-lekérdezés eredménye formázza a JSON Azure SQL-adatbázisban, és közvetlenül az alkalmazásban használható.

A következő példában a Sales.Customer tábla sorainak formázott JSON a vonatkozó JSON záradék használatával:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

A JSON ELÉRÉSI záradék JSON szöveget formázza a lekérdezés eredményét. Oszlopnevek használják kulcsok, miközben a cellaértékek JSON értékként jönnek létre:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Az eredményhalmaz van formázva, ahol minden sorban van formázva, külön JSON objektumként JSON tömbként.

Elérési út, az azt jelenti, hogy a JSON eredménye, a kimeneti formátum szabhatja oszlop aliasokban pont jelölés segítségével. A következő lekérdezés a "Ügyfélneve" billentyűt a kimeneti JSON formátum neve megváltozik, és helyezi a telefon-és a "Névjegy" alszint objektumban:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

A lekérdezés eredményét néz ki:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Ez a példa azt a [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) beállítás megadásával helyett a tömb egyetlen JSON objektum adja vissza. Használhatja ezt a lehetőséget, ha tudja, hogy egyetlen objektummá lekérdezés eredményeként eredményül adása.

A fő a vonatkozó JSON záradék értéke, hogy lehetővé teszi összetett hierarchikus adatokat vissza az adatbázisból, beágyazott JSON-objektumok vagy tömböknek formázott. A következő példa bemutatja, hogyan lehet felvenni az ügyfélnek rendelések beágyazott tömbként tartozó rendelések:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Helyett elválaszthatja egymástól a felhasználói adatok beolvasása lekérdezések és majd-ból kapcsolódó rendelések listáját, elérheti a szükséges adatok egyetlen lekérdezéssel küldéséről, a következő példa eredménye látható módon:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>JSON-adatok használata

Ha nincs feltétlenül strukturált adatok, ha összetett alszint objektumok, tömbök vagy hierarchikus adatokat, vagy a adatstruktúrák verzióinformációk, a JSON formátum segíthetnek bármely összetett adatstruktúra ábrázolásához.

JSON formátuma egy szöveges használható, mint bármely más karakterlánc típusú Azure SQL-adatbázisban. Küldhet és JSON adattárolásra, mint a szokásos NVARCHAR:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Ebben a példában használt JSON adatok jelöli a NVARCHAR(MAX) típus használatával. JSON is lehet a tábla beszúrt vagy a tárolt eljárás, használja a szabványos Transact-SQL-szintaxisa a következő példában látható módon az argumentumként megadott:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Bármely ügyféloldali nyelvi vagy egy dokumentumtárba, amelynek Azure SQL-adatbázis adatainak karakterlánc esetét is működnek JSON adatokat. Minden olyan táblázat, amely támogatja az NVARCHAR típusú, például a memória optimalizálása vagy rendszer verziószámmal táblázat JSON tárolhatók. JSON bármely kényszer az ügyféloldali kódot vagy az adatbázis réteget nem vezet be.

## <a name="querying-json-data"></a>JSON adatok lekérdezése

Ha formázott JSON Azure SQL-táblákban tárolt adatok, JSON függvényekkel intervallumokat bármely SQL-lekérdezés használja ezeket az adatokat.

Azure SQL-adatbázis legyen a használható funkciók JSON kezelik az adataimat JSON, mint bármely más SQL adattípus formázva. Egyszerűen értékek kinyerése a JSON szöveget, és bármilyen lekérdezés JSON-adatok használata:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

A JSON_VALUE függvény egy érték olvas adatok oszlopban tárolt JSON szöveget. Ez a funkció JavaScript-szerű elérési utat egy értéket a JSON szöveg kibontása hivatkozni szeretne. A kinyert érték SQL-lekérdezés bármely részében használható.

A JSON_QUERY függvény hasonlít JSON_VALUE. Eltérően JSON_VALUE Ez a funkció kibontása összetett alszint objektumként, például a tömbök vagy objektumok, amelyek JSON szöveg kerül.

A JSON_MODIFY függvény lehetővé teszi a JSON-szöveget, amelyet a módosuljon, valamint egy új értéket, amely felülírja a régit elérési útját az érték adja meg. Ezzel a módszerrel egyszerűen hozzáigazítható JSON szöveg teljes struktúrájának reparsing nélkül.

Mivel a szabványos szöveget JSON van tárolva, garanciát nem jelentenek, hogy megfelelően formázott szöveg oszlopokban tárolt értékeket. Ellenőrizheti, hogy a szöveg JSON oszlopban tárolt megfelelően van formázva szabványos Azure SQL-adatbázis ellenőrző korlátozások és a ISJSON függvény használatával:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Ha a beírt szöveg megfelelően van formázva JSON, a ISJSON függvény a visszaadott érték 1. Minden Beszúrás vagy a frissítés JSON oszlop ezt a korlátot ellenőrzi, hogy új szöveges érték nem hibás JSON.

## <a name="transforming-json-into-tabular-format"></a>JSON átalakítása táblázatos formátumba

Azure SQL-adatbázishoz is lehetővé teszi a táblázatos formátum és a betöltés vagy a lekérdezés JSON adatokat JSON gyűjtemények átalakítása.

OPENJSON fut. egy táblázat értékű funkció elemzi a JSON szöveg JSON-objektumok tömbben keresi, a függvény a tömbben elemei között, és egy sor a kimeneti eredménye a tömb összes elemét adja eredményül.

![Táblázatos JSON](./media/sql-database-json-features/image_2.png)

A fenti példában azt is adja meg, hol helyezi el a JSON tömb meg kell nyitni (forintban. Rendelések elérési út), mely oszlopok vissza kell eredmény és a hol találhatók a cellaként visszaküldött JSON értékeket.

A JSON tömbben átalakíthatja azt is a @orders be egy sor olyan sorokat, elemezheti az eredményhalmaz, vagy sorok beszúrása a szabványos táblázatba:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
A rendelések gyűjteménye JSON tömbként formázott és, ahogyan a paramétert a tárolt eljárás elemzi, és a Rendelések tábla beszúrt.

## <a name="next-steps"></a>Következő lépések

Integrálhatja a JSON az alkalmazást, olvassa el az alábbi forrásokban olvashat:

- [TechNet-Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
- [Dokumentáció az MSDN webhelyen](https://msdn.microsoft.com/library/dn921897.aspx)
- [Csatorna 9 videó](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

A JSON integrálása az alkalmazás különböző forgatókönyvekben kapcsolatos további tudnivalókért lásd: Ez a [Videó csatorna 9](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) a bemutatók, vagy keresse meg a használati eset [JSON blogbejegyzéseket](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/)a megfelelő példa.
