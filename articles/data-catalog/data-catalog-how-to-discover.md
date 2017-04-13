<properties
   pageTitle="Hogyan végezhető el adatforrások feltárása |} Microsoft Azure"
   description="Ennek a kiemelés felfedezése regisztrált adatok eszközök Azure adatkatalógusban, beleértve a Keresés és szűrés és a Kiemelés a Azure adatkatalógus portál funkciók találatot használatával hogyan cikk."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Hogyan adatforrások felfedezése

## <a name="introduction"></a>– Bevezetés
**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. Más szóval **Azure adatkatalógus** az összes ezzel megkönnyítve a személyek Fedezze fel, megértését és adatforrásokból, és használata segítve a szervezet több értéket a meglévő adatokból. Miután az adatforrás **Azure**adatkatalógusban van regisztrálva, a metaadatait indexelt a szolgáltatás által, hogy a felhasználók egyszerűen megkereshetik a szükséges adatok felderítésére.

## <a name="searching-and-filtering"></a>Keresés és -szűrés

**Azure adatkatalógus** felderítése használja a két elsődleges mechanizmusok: keresés és a szűrést.

Keresés lett tervezve intuitív és a hatékony – alapértelmezés szerint, a keresési kifejezések teljesül a katalógus, beleértve a felhasználó által megadott jegyzetek bármely tulajdonság alapján.

Szűréssel lett tervezve kiegészítése keresése. Felhasználók választhatnak például szakértőket, adatforrástípus, objektum típusa és a címkék, csak egyező adatok eszközök megtekintése és korlátozhatja a találatokat, hogy a megfelelő eszközök, valamint adott jellemzőiket befolyásolják.

Keresés és -szűrés kombinációi használatával a felhasználók gyorsan megkeresheti az **Azure adatkatalógus** fel az adatforrások szükségük van regisztrálva adatforrások.

## <a name="search-syntax"></a>Keresési szintaxisa

Bár az alapértelmezett szabad szöveges keresés egyszerű, magától értetődő, felhasználók is használhatja **Azure adatkatalógus**keresési szintaxisa, hogy jobban kézben tarthatók a keresési eredmények között. **Azure adatkatalógus** keresési támogatja a következőket:

| A módszer                 | Használata                                                                                                                                     | Példa                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Egyszerű keresési              | Egyszerű keresési egy vagy több keresési kifejezések használatával. Eredménye bármely eszközök, amelyek megegyeznek bármely tulajdonság egy vagy több megadott feltételek alapján. | értékesítési adatok                                                |
| Hatókör meghatározása a tulajdonság          | Csak adja vissza az adatforrások, ahol a keresett kifejezést a megadott tulajdonság megegyezik                                                   | név: Pénzügy                                              |
| Logikai operátorok         | Bővítéséhez vagy logikai műveleteket használatával keresés szűkítéséhez                                                                                     | Pénzügy nem vállalati                                     |
| Csoportosítás a zárójel | A lekérdezés csoport részeinek zárójeleket használ logikai elkülönítési, különösen a logikai operátorokat együtt elérése              | név: Pénzügy és (címkék: V1 vagy címkék: v2) |
| Összehasonlító operátorok      | Nem egyenlő összehasonlításokat numerikus és dátum típusú adatokat tartalmazó tulajdonságok használata                                                | modifiedTime > "11/05/2014-es"                                 |

**Azure adatkatalógus** keresési kapcsolatos további tudnivalókért olvassa el a [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx)című témakört.

## <a name="hit-highlighting"></a>Kattintson a kiemelés
Keresési találatok megtekintésekor minden megjelenített tulajdonság, amelyek megfelelnek a megadott keresési kifejezések – például az adatok eszköz neve, leírás és címkék – kiemelten jelenik meg, hogy könnyebben azonosítható az oka annak, hogy egy adott adatok tárgyi eszköz egy adott keresési által visszaadott.

> [AZURE.NOTE] Kikapcsolhatja, hogy a felhasználók találati kiemelésének kikapcsolása tetszés szerint a "Kiemelés" kapcsolóval az **Azure adatkatalógus** portálon.

Keresési találatok megtekintésekor, előfordulhat, hogy mindig nem egyértelmű oka annak, hogy egy adatok eszköz megtalálható, még akkor is kiemeléssel találati engedélyezve van. Minden tulajdonság alapértelmezés szerint keressen a program, mert a adatok eszköz oszlop szintű tulajdonság alapján egyező miatt előfordulhat, hogy visszaadandó. És a több felhasználó regisztrált adatok eszközök saját címkék és a leírásokat széljegyzetelheti, mert nem az összes metaadatok jelennek meg a keresési eredmények listájában.

Az alapértelmezett nézet csempe, minden mozaik jelenik meg a keresési eredmények között megtalálható lesz egy "nézetben keresőkifejezést megfelelő" ikon, amely lehetővé teszi, hogy a felhasználó tekintheti meg gyorsan az egyező és a helyükre számát, és szükség esetén ugorhat.

 ![Találati kiemelése és a Keresés az adatkatalógusban Azure-portálon megfelelő](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Összefoglalás
Adatforrás regisztrálása **Azure** adatkatalógusban könnyebb az adatforrás felderítése és megértéséhez, másolásával szerkezeti és leíró metaadatokat az adatforrásból a katalógus helyezését. Miután az adatforrás van regisztrálva, felhasználók, észleljék használatával a szűrés és a Keresés a belül az **Azure adatkatalógus** portálon.

## <a name="see-also"></a>Lásd még:
- [Első lépések az Azure adatkatalógus](data-catalog-get-started.md) oktatóprogram adatforrások felderítésére részletesen olvashat.
