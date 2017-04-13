<properties
    pageTitle="Értékáram-elemzés használata a hivatkozási adatok és keresési táblák |} Microsoft Azure"
    description="Értékáram-elemzés lekérdezés hivatkozási adatok használata"
    keywords="a keresési tábla, hivatkozási adatok"
    services="stream-analytics"
    documentationCenter=""
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Értékáram-elemzés beviteli adatfolyam hivatkozási adatok és keresési táblák használata

Hivatkozási adatok (más néven a keresési tábla) egy statikus függvényt adathalmaz vagy lassítása módosítása jellegű, végezze el a Keresés vagy a adatfolyam a összehangolására. Ellenőrizze, hogy az Azure Értékáram-elemzés feladat hivatkozási adatok felhasználása általában használandó [Hivatkozási adatok Bekapcsolódás](https://msdn.microsoft.com/library/azure/dn949258.aspx) a lekérdezés. A hivatkozási adatok Értékáram-elemzés használja, mint a tárhely réteg Azure Blob-tárolóhoz, és Azure Data Factory hivatkozást tartalmazó adatokat transzformált vagy [felhőalapú tetszőleges számú alapú és a helyszíni adatok tárolja](../data-factory/data-factory-data-movement-activities.md)átmásolt Azure Blob-tárolóhoz hivatkozás adatként való használatra. Hivatkozási adatok modellezni szerint növekvő sorrendben, a dátum/idő megadott a blob neve (a bemeneti konfigurációban definiált) BLOB sorozatát. Ez **csak** a hozzáadása a sorozat végére egy dátum/idő **nagyobb,** mint az utolsó blob sorrendben megadott támogatja.

## <a name="configuring-reference-data"></a>Hivatkozási adatok beállítása

Ha szeretné beállítani a hivatkozási adatok, először kell hozzon létre egy típusú **Hivatkozási adatok**bevitele. Az alábbi táblázat ismerteti a minden tulajdonság adja meg a leírását a bemeneti hivatkozási adatok létrehozásakor meg kell:

<table>
<tbody>
<tr>
<td>Tulajdonság neve</td>
<td>Leírás</td>
</tr>
<tr>
<td>Beviteli Alias</td>
<td>Egy rövid nevet, a bemeneti hivatkozni szeretne a feladat lekérdezésben használt.</td>
</tr>
<tr>
<td>Tárterület-fiók</td>
<td>Hol találhatók a blob-fájlok a tárterület-fiók neve. Ha a adatfolyam Analytics munka ugyanabban az előfizetésben, válassza azt a legördülő lefelé.</td>
</tr>
<tr>
<td>Tárterület Fiókkulcs</td>
<td>A titkos kulcs a tárhely partnernél. Ennek módja automatikusan kitölti a rendszer a tárterület-fiók esetén, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben.</td>
</tr>
<tr>
<td>Tárterület-tárolóhoz</td>
<td>Tárolók adja meg a Microsoft Azure Blob-szolgáltatásban tárolt BLOB-logikai csoportja. Amikor blob feltöltése a Blob-szolgáltatás, meg kell adnia, hogy a blob-tároló.</td>
</tr>
<tr>
<td>Elérési út mintával</td>
<td>A fájl elérési útja, a megadott tárolóban a BLOB megkeresésére. A PATH tudja kiválasztani az alábbi 2 változót egy vagy több példány megadása:<BR>{date}, {idő}<BR>Példa 1: products/{date}/{time}/product-list.csv<BR>Példa 2: products/{date}/product-list.csv
</tr>
<tr>
<td>A dátumformátum [választható]</td>
<td>Ha a megadott Path mintában használt {date}, majd válassza a dátumformátumot, amelyben a fájlok vannak rendezve a támogatott formátumok legördülő. Példa: YYYY/hh/nn</td>
</tr>
<tr>
<td>Időformátum [választható]</td>
<td>Ha a megadott Path mintában használt {idő}, majd válassza az időformátumot, ahol a fájlok vannak rendezve a támogatott formátumok legördülő. Példa: HH</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>A lekérdezések működjenek a kívánt módon, Értékáram-elemzés kell, hogy melyik szerializálási formátum használata bejövő adatfolyamok. A hivatkozási adatok a támogatott formátumok CSV és JSON.</td>
</tr>
<tr>
<td>Kódolás</td>
<td>UTF-8 jelenleg csak támogatott kódolás</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Hivatkozási adatok időközönként létrehozása

Adatkészlet lassan módosítása a hivatkozási adatok esetén akkor támogatja az adatok engedélyezve van a bemeneti konfigurációval {date} elérési út mintát megadásával hivatkozás frissítése, és helyettesítő tokenek {time}. Értékáram-elemzés program felveszi a frissített hivatkozási adatok definíciók e elérési_út minta alapján. Például a minta `sample/{date}/{time}/products.csv` az idő és a **"YYYY-MM-DD"** dátumformátummal **"Hh: mm"** formátumának arra utasítja adatfolyam elemzést emelje fel a frissített blob `sample/2015-04-16/17:30/products.csv` 17:30 között a április 16 2015 UTC a zóna időt.

> [AZURE.NOTE] Jelenleg Értékáram-elemzés feladatok keresse meg a blob-frissítés csak akkor, ha a számítógép ideje előlegek blob neve kódolt időt. Példa a feladatot fog keresni `sample/2015-04-16/17:30/products.csv` , amint lehetséges, de nincs korábbi mint 17:30 között a április 16 2015 UTC idő zónában. Program *soha nem* keresse meg a fájl az utolsó azzal, hogy az észlelt verziónál kódolt idő.
> 
> Pl. Miután a feladat megtalálja a blob `sample/2015-04-16/17:30/products.csv` akkor figyelmen kívül hagyja a fájlok 17:30 között április 16 2015-nél korábbi kódolt dátuma, ha egy késő érkező `sample/2015-04-16/17:25/products.csv` blob jön létre ugyanabban a tárolóban a feladat nem használja azt.
> 
> Hasonlóképpen ha `sample/2015-04-16/17:30/products.csv` du. 10:03 áprilisi 16 2015 csak készül, de nincs blob-korábbi dátumú nem tartalmaz adatokat a tárolóban, a feladat fogja használni ezt a fájlt 10:03 du áprilisi 16 2015 kezdve, és az előző hivatkozási adatok egészen addig.
> 
> Ez a kivétel vissza az időt újra folyamat adatokat kell a feladatot, vagy ha a feladat első azonnal elindul. A legutóbbi blob előtt a feladat előállított keres a projekt a kezdési időpontot a elindítása megadott. Ez történik, annak érdekében, hogy van egy **nem üres** hivatkozás adathalmaz a feladat indításakor. Ha egy nem található, a feladat jelenít meg a következő diagnosztikai: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) használható téve a frissített BLOB Értékáram-elemzés szükséges frissítést hivatkozási adatok jelez létrehozását. Adatok gyári orchestrates és a szállítási és adatok átalakítása automatizálja felhőalapú adatok integrációs szolgáltatás. Adatok gyári támogatja a [felhőalapú és a helyszíni adatok tárolja nagyszámú való csatlakozás](../data-factory/data-factory-data-movement-activities.md) és a mozgó adatok egyszerűen megadott rendszeres időközönként. További információk és hogy hogyan állíthatja be a Data Factory folyamat adatfolyam elemzéséhez, amely előre meghatározott időközönként frissítse hivatkozási adatok létrehozásához lépésenkénti útmutatást kövesse ezt a [minta GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tippek a hivatkozási adatok frissítése ##

1. Hivatkozás a BLOB-adatobjektumok nem felülírással adatfolyam elemzést ismételt betöltése a blob, és bizonyos esetekben okozhatják a feladat meghiúsító. Az ajánlott módszereket, módosíthatja a hivatkozási adatok hozzáadása egy új blob használata ugyanabban a tárolóban és elérési út mintázatot a feladat bemeneti definiálva, és használja a dátum/idő **nagyobb,** mint az utolsó blob sorrendben által megadott.
2.  Hivatkozási adatok BLOB nem vannak rendezve, a blob "Utoljára módosította" idő, hanem csak a blob megadott dátum és időpont nevet, {date} használ, és {time} helyettesítő.
3.  Néhány alkalommal egy feladatot kell térjen vissza az idő ezért hivatkozási adatok BLOB nem kell módosítani vagy törölni.

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Korábban már ismerje meg az adatfolyam Analytics, egy felügyelt szolgáltatást a folyamatos átvitelű analytics az internetes dolgot adataiból. Ezzel a szolgáltatással kapcsolatos további tudnivalókért lásd:

- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
