<properties
   pageTitle="Hogyan lehet adatforrásokat a jegyzet |} Microsoft Azure"
   description="Útmutató a cikk a jegyzet adatok eszközök Azure adatkatalógusában, köztük a rövid nevet, a címkék, a leírás és a szakértői hogyan kiemelés."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Hogyan lehet a jegyzet adatforrások

## <a name="introduction"></a>– Bevezetés
**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. Más szóval adatkatalógus az összes ezzel megkönnyítve a személyek felfedezése, megértését és adatforrásokból, és használata segítve a szervezet több értéket a meglévő adatokból. Adatforrás regisztrálása adatkatalógusban esetén a metaadatait másolt és szolgáltatás indexelni, de a szövegegység ott nem befejezése. Adatkatalógus lehetővé teszi, hogy a felhasználók saját leíró metaadatainak – például a leírásokat és a címkék – a metaadatokat az adatforrásból kibontása kiegészítése, és végezze el az adatforrás több érthető további személyeket.

## <a name="annotation-and-crowdsourcing"></a>Jegyzet és crowdsourcing
Minden résztvevő rendelkezik a véleményét. És ez jó valamilyen dolog.
Adatkatalógus felismer, hogy rendelkezik-e a különböző felhasználók különböző perspektívák a vállalati adatforrásokból, és hogy ezek perspektívák mindegyike is értékes. Vegye figyelembe az alábbiakat:

* A rendszergazda tudja, hogy a szolgáltatásiszint-szerződés kiszolgálók vagy szolgáltatásokat tároló az adatforráshoz.
* Az adatbázis-adminisztrátorhoz tudja, hogy az ütemezés adatbázisonként, és a megengedett ETL feldolgozási windows.
* A rendszer tulajdonosa tudja, hogy a felhasználók a következőképpen igényelhet hozzáférést az adatforrás folyamat.
* Az adatgondnok tudja, hogy hogyan az eszközök és az adatforrás attribútumok leképezése vállalati az adatmodellhez.
* Az analitikus tudja, hogy hogyan az adatokat az üzleti folyamatok támogatja az általa környezetében használják.

Minden egyes e perspektívák jelentősége, és adatkatalógus használja egy crowdsourcing, amely lehetővé teszi, hogy rögzítse és a teljes kép regisztrált adatforrások elérésére használt egyenként metaadat-alapú megközelítés. Az adatkatalógus portál használatával, minden felhasználóhoz is hozzáadása és szerkesztése a saját jegyzetek, miközben is más felhasználók által biztosított jegyzetek megtekintése.

## <a name="different-types-of-annotations"></a>Különböző típusú széljegyzetek
Adatkatalógus támogatja a következő típusú széljegyzetek:

| Jegyzet     | Jegyzetek                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Rövid név  | Rövid nevek adatok eszköz szintre, de, hogy az adatok eszközök könnyebben érthető is kell megadni. Rövid nevek leginkább hasznosak, ha az alapul szolgáló objektum neve rejtélyes, rövidített vagy más módon nem jellemző felhasználók.                                                                                                                            |
| Leírás    | Leírások attribútum és lévő adatok eszköz lehet megadni / oszlop szintek. Leírások, amelyek bemutatják a felhasználó rövid szöveg szabadkézi széljegyzetek perspektíva az adatok eszköz vagy a használatát.                                                                                                                                                              |
| Címkék (felhasználói címkék)          | Címkék attribútum és lévő adatok eszköz lehet megadni / oszlop szintek. Felhasználói címkék olyan felhasználó által definiált címkék eszközök adatokat vagy attribútumokat kategorizálásához használható.                                                                                                                                                                                                    |
| Címkék (szószedet címkék)          | Címkék attribútum és lévő adatok eszköz lehet megadni / oszlop szintek. Szószedet címkék olyan központi definiált címszó adatok eszközök, illetve egy gyakori üzleti besorolás használatával attribútumok kategorizálásához is használható. További információ megtudhatja, [hogy miként állíthatja be a vállalati szószedet szabályozott címkézéshez](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Szakértői        | Szakértői is kell megadni, az eszköz szintjén. Szakértői azonosítása felhasználók vagy csoportok, az adatok szakértői perspektívák is lesz a felhasználók számára a regisztrált adatforrások felfedezése kapcsolattartási és kérdései vannak, amelyek nem érkezett válasz, a meglévő széljegyzeteket.  |
| Hozzáférés kérése | Access-adatok kérése az eszköz szintjén lehet megadni. Az információk a felhasználók, akik az adatforrás eléréséhez engedélyek még nem rendelkeznek felfedezése szolgál. Felhasználók adhat meg annak a felhasználónak vagy csoportnak, akik hozzáférést, a folyamaton, illetve hozzáférést igénylő felhasználók eszköz URL-CÍMÉT az e-mail címét, vagy megadhat olyan maga a folyamat szövegként. |
| Dokumentáció | Az eszköz szintjén is kell adni a dokumentációt. Digitáliseszköz-dokumentáció áll a rich text formátumú információ, hivatkozásokat és képeket is tartalmaz, és amelyek nem közvetített leírásokat és a címkék információkat tartalmaznak. |


## <a name="annotating-multiple-assets"></a>Több eszközök jegyzetet fűz
Amikor az adatkatalógusban portál több adat eszközök jelöl ki, a felhasználók készíthetnek széljegyzeteket a egy lépésben az összes kijelölt eszköz. Kijelölt Webhelyeszközök, így egyszerűen jelölje ki, és a szükséges egységes leírás és a címkék és szakértők készletek kapcsolódó adatok eszközök széljegyzetek érvényesek lesznek.

> [AZURE.NOTE] Címkék és szakértői is biztosítható, hogy amikor az adatkatalógusban adatokkal regisztrálása adatok eszközök forrás regisztrációs eszköz.

Ha több táblák és nézetek, csak oszlopot, hogy az összes kijelölt eszközök van közös adatok kijelölés jelennek meg az adatkatalógusban portálon. Ez lehetővé teszi a felhasználóknak, hogy adja meg a címkék és az oszlopok ismertetése az összes kijelölt eszköz azonos nevű.

## <a name="annotations-and-discovery"></a>A széljegyzetek és a feltárás
Ugyanúgy, mint a metaadat-alapú regisztráció során az adatforrás kiolvasott bekerül az adatkatalógusban keresési index, felhasználó által megadott metaadatokat is indexelt. Ez azt jelenti, hogy nem csak a széljegyzetek megkönnyítheti a felhasználóknak, hogy az adatokat azok felfedezése megértéséhez, széljegyzetek is megkönnyítheti a felhasználóknak, hogy az adatok jelekkel ellátott eszközök felfedezése őket hozzárendelésével kifejezésekkel kereséssel.

## <a name="summary"></a>Összefoglalás
Adatforrás regisztrálása az adatkatalógusban ellenőrzi, hogy az adatok feltárható átmásolásával szerkezeti és leíró metaadatokat az adatforrásból a katalógus helyezését. Miután az adatforrás van regisztrálva, felhasználók megadhatja az jegyzeteket, így könnyebben felderítése és belül az adatkatalógusban portált a megértéséhez.

## <a name="see-also"></a>Lásd még:
- [Első lépések az Azure adatkatalógus](data-catalog-get-started.md) oktatóprogram adatforrásokat a jegyzet részletesen olvashat.
