<properties
   pageTitle="Miként dolgozhat az adatforrások "nagy-adatok" |} Microsoft Azure"
   description="Ennek a cikk az Azure adatkatalógus használata "nagy-adatok" adatforrások – köztük a Azure Blob-tárolóhoz Azure adatok tó és Hadoop Fájlrendszerhez mintázatok kiemelés."
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


# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Azure adatkatalógusában nagy adatforrások használata

## <a name="introduction"></a>– Bevezetés
**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. **Azure adatkatalógus** Ez azt jelenti, segítve a személyekre vonatkozó összes Fedezze fel, megértését és adatforrások és segítve a szervezetek használatával több érték beolvasása a meglévő adatforrásokhoz, beleértve a nagy adatokat is.

**Azure adatkatalógus** támogatja az Azure-Blog tárolóhoz BLOB és könyvtárak, valamint Hadoop Fájlrendszerhez fájl és a regisztráció. Ezek az adatforrások félig strukturált jellegét remek rugalmasságot biztosít, de azt is jelenti, hogy felhasználók figyelembe kell vennie hogyan vannak rendezve az adatforrás annak érdekében, hogy a legtöbb érték beolvasása a **Azure**adatkatalógusban regisztrálja őket.

## <a name="directories-as-logical-data-sets"></a>Könyvtárak logikai adatkészletek szerint

Nagy adatforrások rendezése közös mintájának is logikai adatkészletek könyvtárak tekinti. Legfelső szintű könyvtárak használatával definiálnak adatok, miközben az almappák meghatározása a partíciók, és a bennük fájlokat tárolja az adatokat, magát.

Példa a minta lehet:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Ebben a példában a vehicle_maintenance_events és location_tracking_events logikai adatkészletek képviseli. Ezen mappák mindegyikében tartalmazza az év és hónap szerint vannak tagolva az almappák adatfájlokat. Ezen mappák mindegyikében vélhetően tartalmazó száz vagy fájlok ezer.

Ezt a minta egyes fájlokat regisztrálása az **Azure adatkatalógus** valószínűleg nincs értelme. A könyvtárak, amely a felhasználóknak a adatkezelés értelmes adatkészletek megjelenítő ehelyett regisztrálni.

## <a name="reference-data-files"></a>Hivatkozás adatfájlok

Kiegészítő mintát is hivatkozás adatkészletek tárolása külön fájlként. Ezek adatkészletek is értelmezhető nagy adatok kis"oldalán, és gyakran hasonlóak analitikai adatmodellbeli méretét. Hivatkozás adatfájlok tartalmazzák a rekordok, amellyel a tömeges máshol a nagy adatok tárban tárolt adatfájlok segítik a szövegkörnyezet.

Példa a minta lehet:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Egy analitikus vagy adatok tudósok használatakor az adatokkal, a nagyobb directory szerkezetek található a hivatkozás fájlok adatainak részletesebb információkat, amelyek csak által hivatkozott nevet vagy Azonosítót (GB) nagyobb adatkészlet entitás használható.

Ez a mintázat fog értelme regisztrálni az egyes összefoglaló adatfájlok **Azure**adatkatalógusban. Fájlonként adatok csoportját képviseli, és mindegyik vonatkozóan kiegészítések értelmében, és egyenként talált.

## <a name="alternate-patterns"></a>Alternatív mintázatok

A fent leírt mintázatok csak két módszerrel lehetséges előfordulhat, hogy egy nagy adattár szervezett, de minden megvalósítása különböző lesz. Függetlenül attól, hogy az adatforrások szerkezete, nagyméretű adatforrások **Azure adatkatalógusban**történő regisztráláshoz kiemelése a fájlokat és a szervezeten belül másoknak érték lesz adatkészletek megjelenítő könyvtárak regisztrálása. Az összes fájlok és mappák regisztrálása a katalógus, így azokat a jegyzetfüzeteket, hogy a felhasználók nehezebb rendezetlen elemek is.

## <a name="summary"></a>Összefoglalás
Adatforrás regisztrálása **Azure** adatkatalógusban megkönnyíti a Fedezze fel, és megértette. Regisztrálása, és a nagy adatfájlok és logikai adatkészletek megjelenítő könyvtárak jegyzetet fűz, segítséget nyújthat a felhasználók megkeresése és használata a nagy adatforrások van szükségük.
