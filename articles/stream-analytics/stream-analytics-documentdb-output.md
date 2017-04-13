<properties
    pageTitle="Értékáram-elemzés JSON kimeneti |} Microsoft Azure"
    description="Megtudhatja, hogyan Értékáram-elemzés is Megcélozhat Azure DocumentDB JSON kibocsátás, az adatok archiválása és a kis-késés lekérdezések strukturálatlan JSON adatok."
    keywords="JSON kimeneti"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Cél Azure DocumentDB a JSON kimenetét Értékáram-elemzés

Értékáram-elemzés az archiválás és a kis-késés adatlekérdezések strukturálatlan JSON adatok engedélyezése kimeneti, JSON is Megcélozhat [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . Megtudhatja, hogy miként integrálás legjobb végrehajtásához.

Azok, akik nem ismerik DocumentDB tanulmányozza az első lépések a [DocumentDB a tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/documentdb/) .

## <a name="basics-of-documentdb-as-an-output-target"></a>Egy kimenet célként DocumentDB alapjai
Értékáram-elemzés a Azure DocumentDB kimenetét lehetővé teszi, hogy írni az eredmények feldolgozása JSON kimenetként be a DocumentDB collection(s) adatfolyam. Értékáram-elemzés létrehozása gyűjtemények az adatbázisban, helyette megkövetelése, hogy upfront hozzon létre. Ez az, hogy a számlázási DocumentDB gyűjtemények költségei, átlátszó, és így is finomhangolása a teljesítményt, egységesebb és a webhelycsoportok közvetlenül használatával a [DocumentDB API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx)kapacitása. Azt javasoljuk, hogy egy DocumentDB adatbázis használata adatfolyam Feladatonkénti logikailag külön a webhelycsoportok adatfolyam feladathoz.

Egyes DocumentDB webhelycsoport beállításokat részletezi alatt.

## <a name="tune-consistency-availability-and-latency"></a>Egységesebb, elérhetőségét és időtartama finomhangolása

Alkalmazás igényeinek megfelelően, DocumentDB lehetővé teszi, hogy miként optimalizálhatja az adatbázis és webhelycsoportok, és végezze el a kompromisszumok konzisztencia elérhetőségét, és a késés között. Attól függően, hogy milyen mértékű olvasási konzisztencia forgatókönyv igényeinek megfelelően szemben és olvasási késés, a konzisztencia szint választania az adatbázis-fiókot. Is alapértelmezés szerint DocumentDB lehetővé teszi, hogy a webhelycsoport minden CRUD művelet szinkron indexelés. Ez a beállítás egy másik hasznos szabályozhatja DocumentDB írható/olvasható teljesítményét. Ez a témakör a további tudnivalókért tekintse át a [az adatbázis és a lekérdezés konzisztencia szintek módosítása](../documentdb/documentdb-consistency-levels.md) .

## <a name="choose-a-performance-level"></a>Válassza a teljesítmény szint

A 3 különböző teljesítményszint (S1, S2 vagy S3), amely megállapításához, hogy a rendelkezésre álló átviteli CRUDs az adott webhelycsoport DocumentDB gyűjtemények hozhat létre. Ezenkívül teljesítmény hatással van a webhelycsoport a indexelés/konzisztencia frissítésére. [Ez a cikk](../documentdb/documentdb-performance-levels.md) részletesen e teljesítményszint megértéséhez olvassa el.

## <a name="upserts-from-stream-analytics"></a>Értékáram-elemzés a Upserts

Adatfolyam Analytics-integráció a DocumentDB beszúrása meg és frissítheti a rekordokat a megadott oszlop szerint dokumentumazonosító DocumentDB gyűjteménybe teszi lehetővé. Ez akkor is nevezik egy *Upsert*.

Értékáram-elemzés optimista Upsert megközelítés, ahol frissítések csak fejezni Ha beszúrás dokumentumazonosító ütközés miatt nem sikerül használja. A frissítés végzi Értékáram-elemzés javítás, mint így lehetővé teszi, hogy a dokumentumot, azaz új tulajdonságai, vagy egy meglévő tulajdonság fokozatosan végrehajtott cseréje hozzáadásával részleges frissítéseit. Figyelje meg, hogy a változások a JSON-dokumentum eredményében a tömbben teljes felülírja az első, azaz a tömb tömb tulajdonságok értékei nem egyesített.

## <a name="data-partitioning-in-documentdb"></a>Adatok DocumentDB a szétválasztás

DocumentDB gyűjtemények lehetővé teszi az adatok a lekérdezés mintázatok és a teljesítmény az alkalmazás igényeinek megfelelően alapján partition. Minden egyes webhelycsoport tartalmazhat legfeljebb 10GB adatot (legfeljebb), és jelenleg nincs lehetőség van egy webhelycsoport méretezése (vagy túlcsordulás). Méretezés kifelé, Értékáram-elemzés teszi lehetővé írni egy adott előtaggal több gyűjtemények (lásd: használatát részleteit alább). Értékáram-elemzés kimeneti rekordjai partition használja az egységes [Kivonat partíciót feloldó](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) stratégia, a felhasználó által megadott PartitionKey oszlop alapján. A továbbított feladat indításakor a megadott előtaggal gyűjtemények számformátum, amelyhez a feladat ír párhuzamosan kimeneti partíciót száma (DocumentDB gyűjtemények = partíciók kibocsátás). Egy egyetlen S3 az lusta indexelési tenni a webhelycsoport csak szúrja be, MB/s írási átviteli várhatók 0,4 kapcsolatban. Több gyűjtemények használatával lehetővé teszi a nagyobb teljesítmény és a megnövelt kapacitás eléréséhez.

Ha azt szeretné, a jövőben a partíciót számának növelése érdekében, szükség lehet a feladat leállítása, a meglévő gyűjtemények új csoportokba adatainak particionálnia, és indítsa újra a Értékáram-elemzés feladatot. "Elintézendő" bejegyzésbe szerepelni fog PartitionResolver használ, és újra szétválasztás együtt példakódot, további tájékoztatást. A részletek [DocumentDB a szétválasztás](../articles/documentdb-partition-data.md#developing-a-partitioned-application) cikk is tartalmaz.

## <a name="documentdb-settings-for-json-output"></a>JSON kimeneti DocumentDB beállításai

Információ az alább látható módon a egy figyelmeztető üzenet létrehozása DocumentDB adatfolyam Analytics-kimenetként hoz létre. Ez a témakör a Tulajdonságok meghatározása szakaszok egyenként ismertetik.

![documentdb adatfolyam analytics kimenet képernyőn](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Kimeneti Alias** – egy alias olvassa el a kimenet ASA lekérdezésbe  
-   **Fióknév** – a nevét vagy URI DocumentDB fiók végpontot.  
-   **Fiókkulcs** – a megosztott hívóbetű DocumentDB fiók.  
-   Az **adatbázis** – DocumentDB az adatbázis nevét.  
-   **A webhelycsoport neve mintát** – a gyűjtemények használandó webhelycsoport neve mintájának. A webhelycsoport névformátum is alakítható ki a {partíciót} token használ, ahol partíciót a 0-tól indítása. Az alábbiakban minta érvényes bemeneti adatok alapján:  
   1\) MyCollection – egy webhelycsoport "MyCollection" nevű léteznie kell.  
   2\) – ilyen gyűjtemények léteznie kell, – MyCollection {partíciót} "MyCollection0", "MyCollection1", "MyCollection2", és így tovább.  
-   **Partition kulcs** – Itt adhatja meg a kimeneti szétválasztás keresztül gyűjtemények billentyűjét kimeneti események a mező nevét. A kimenet egyetlen webhelycsoport bármely tetszőleges kimeneti oszlopban lehet például PartitionId használt.  
-   **Dokumentumazonosító** – nem kötelező. Az Itt adhatja meg az elsődleges kulcsot, mely Beszúrás vagy a frissítés műveletek alapul kimeneti események a mező neve.  
