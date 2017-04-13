<properties
   pageTitle="Szűrő karakterláncok megépítése az a tábla Tervező |} Microsoft Azure"
   description="Szűrő karakterláncok megépítése az a tábla Tervező"
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
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Szűrő karakterláncok megépítése az a tábla Tervező

## <a name="overview"></a>– Áttekintés

Az Azure-táblázat a Visual Studio **Táblatervező**megjelenítendő adatok szűrése, szűrő karakterlánc össze, és megadható a szűrőmezőt. A karakterlánc szintaxist: a WCF Data Services által meghatározott és hasonlít egy SQL WHERE záradékot, de a rendszer elküldi a táblázat szolgáltatás egy HTTP elküldött kérésre. A **Táblatervező** kezeli a megfelelő kódolást, így a szűrni kívánt tulajdonságérték, szükséges csak adja meg, a tulajdonságnév, összehasonlító operátort, feltételérték, és tetszés szerint logikai operátorok a szűrő mezőben. Nem kell a $filter lekérdezési lehetőséget tartalmazza, ha meg lettek megépítése egy URL-CÍMÉT a táblázatot a [Tárhely szolgáltatások REST API-hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=400447)útján lekérdezés módon.

A [Megnyitás, adatok Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData-) a WCF Data Services alapulnak. A rendszer lekérdezés lehetőség (**$filter**) a részletekért olvassa el az [OData-URI szabályok specifikációja](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Összehasonlító operátorok

A következő logikai operátorokat diagramtípusokat tulajdonság támogatott:

|Logikai operátorok|Leírás|Példa szűrési karakterlánc.|
|---|---|---|
|EQ|Egyenlő|A város eq "Redmond"|
|gt|Nagyobb, mint|Ár gt 20|
|GE|Nagyobb vagy egyenlő|Ár ge 10|
|lt|Kisebb, mint|Ár lt 20|
|le|Kisebb vagy egyenlő|Ár le 100|
|Ne|Nem egyenlő|A város ne "London"|
|és|És|Ár le 200 és ár gt 3.5-ös|
|vagy|Vagy|Ár le 3.5-ös vagy ár gt 200|
|nem|Nem|nem isAvailable|

Szűrő karakterlánc kiszámításakor a következő szabályokat kell szem:

- A logikai operátorokat segítségével összehasonlíthatja egy tulajdonság, egy értéket. Ne feledje, hogy nem lehetséges a dinamikus érték; a tulajdonságot összehasonlítása a kifejezés egyik oldalának konstans kell legyen.

- A szűrő karakterlánc minden részének nagybetűk között.

- Az állandó értékét azonos típusú adatok ahhoz, hogy a szűrő tulajdonságként érvényes találatokra kell lennie. Támogatott tulajdonság típusok kapcsolatos további tudnivalókért olvassa el [a táblázat szolgáltatás-adatmodell ismertetése](http://go.microsoft.com/fwlink/p/?LinkId=400448)című témakört.

## <a name="filtering-on-string-properties"></a>Szűrés a karakterlánc-tulajdonságok

A karakterlánc-tulajdonságok szűrhet, amikor idézőjelek közé kell tenni a szöveges konstans szimpla idézőjelek közé.

A következő példa szűrők **PartitionKey** és **RowKey** tulajdonságok; a szűrő karakterlánc is hozzáadhatók további nem kulcs tulajdonságokat:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Akkor is minden szűrőkifejezés akkor zárójelek közé kell, de nem kötelező:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Ne feledje, hogy a táblázat szolgáltatás nem támogatja a helyettesítő lekérdezéseket, nem használhatók lévő a Táblatervező vagy. Azonban hajthatja végre a kívánt előtagot összehasonlító operátorokkal úgy egyező előtagot. Az alábbi példa eredménye "A" betűvel kezdődő Keresztnév tulajdonság rendelkező szervezetek:

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Szűrés a numerikus tulajdonságai

Egy egész szám vagy lebegőpontos szám szűréséhez adja meg, hány idézőjelek nélkül.

Ebben a példában az életkor tulajdonság, amelynek értéke nagyobb, mint 30 összes entitás adja eredményül:

    Age gt 30

Ez a példa egy AmountDue tulajdonság, amelynek az értéke kisebb vagy egyenlő 100.25 összes entitás adja eredményül:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Szűrés a logikai tulajdonságai

Olyan logikai értéket szűrni, adja meg a **Igaz** vagy **hamis** idézőjelek nélkül.

Az alábbi példa eredménye összes entitás, ahol a IsActive tulajdonság értéke **Igaz**:

    IsActive eq true

A filter kifejezésére a logikai operátorok nélkül is írhat. A következő példában a táblázat szolgáltatás is visszaadja a program minden entitás **Hol IsActive teljesül**:

    IsActive

Minden entitás vissza, ahol IsActive értéke HAMIS, a nem használható operátor:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Szűrés a DateTime tulajdonságai

Egy DateTime típusú érték szűréséhez adja meg, a **datetime** kulcsszót, a dátum/idő állandó aposztrófok közé foglalása. A dátum/idő állandó UTC kombinált-formátumban kell lennie, [Formázás DateTime tulajdonságértékeket](http://go.microsoft.com/fwlink/p/?LinkId=400449)leírt módon.

Az alábbi példa eredménye szervezetek, ahol a CustomerSince tulajdonság értéke 2008 július 10:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
