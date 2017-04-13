<properties
   pageTitle="Azure adatkatalógus terminológia |} Microsoft Azure"
   description="Ebben a cikkben fogalmak és a kifejezések a Azure adatkatalógus dokumentációt."
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

# <a name="azure-data-catalog-terminology"></a>Azure adatkatalógus kifejezések

## <a name="catalog"></a>Katalógus

Az Azure adatkatalógus tárháza felhőalapú metaadat-alapú adatok az adatok és adatforrások eszközök lehet regisztrálni. A katalógus egy központi tárolási helye az adatforrások kiolvasott szerkezeti metaadatok és a leíró metaadatokat felhasználók által hozzáadott szolgál.

## <a name="data-source"></a>Adatforrás

Adatforrás egy rendszer vagy adatok eszközök kezelő tároló. Többek között az SQL Server-adatbázisok, az Oracle-adatbázisok, SQL Server Analysis Services-adatbázisok (táblázatos vagy többdimenziós) és az SQL Server Reporting Services-kiszolgálókon.

## <a name="data-asset"></a>Adatok eszköz

Adatok eszközök objektumai lehet regisztrálni a katalógus az adatforrások található. SQL Server-táblák és nézetek, az Oracle-táblák és nézetek, SQL Server Analysis Services mértékek, dimenziók és KPI-k többek között, és az SQL Server Reporting Services-jelentések.

## <a name="data-asset-location"></a>Adatok eszköz helye

A katalógus adatforrás vagy adatok tárgyi eszköz használható a forrás használatával ügyfélalkalmazás csatlakoztatása helyét tárolja. Kattintson az adatforrás típusa függően változnak, a formátum és a hely részleteit. Például SQL Server-tábla azonosítható a négy rész nevén – kiszolgáló nevét, az adatbázis neve, séma nevet, objektum –, miközben egy SQL Server Reporting Services-jelentés azonosítható az URL-CÍMÉT.

## <a name="structural-metadata"></a>Szerkezeti metaadatok

A metaadat-alapú adatok eszköz a szerkezet leíró adatforrás kiolvasott metaadatai szerkezeti. Ide tartoznak a eszközök helyét, az objektum neve és a típusa és a további típus-specifikus jellemzők. Táblák és nézetek szerkezeti metaadatait például nevét és az objektum oszlopok típusú adatokat tartalmazza.

## <a name="descriptive-metadata"></a>Leíró metaadatok

Leíró metaadatai, amely leírja a cél vagy a szándék elérését segítő adatok eszköz metaadatok. Általában az leíró metaadatok az Azure adatkatalógus portálon katalógus felhasználók hozza létre, de azt is ki lehet venni az adatforrásból regisztráció során. Például az Azure adatkatalógus regisztrálási eszköz kivágja leírások a Leírás tulajdonság az SQL Server Analysis Services és az SQL Server Reporting Services és az SQL Server-adatbázisok a [kiterjesztett tulajdonságként ms_description](https://technet.microsoft.com/library/ms190243.aspx) , ha a Tulajdonságok van töltve értékű.

## <a name="request-access"></a>Hozzáférés kérése

A adatok eszköz leíró metaadatok is elhelyezhet adatok eszköz vagy adatforráshoz hozzáférést kérni olvashat. Ezt az információt az adatok eszköz hellyel mutatják be, és is tartalmazhat, az alábbi lehetőségek közül:

- Felhasználó vagy csoport megadása az adatforrás eléréséhez felelős annak az e-mail címét.
- Az adatforrás eléréséhez szükséges követniük kell a felhasználókat a dokumentált folyamat URL-CÍMÉT.
- Az identitás- és a hozzáférés kezelése eszköz (például Microsoft Identitáskezelő) az adatforrás eléréséhez használható URL-CÍMÉT.
- Szabad-bejegyzések, amely bemutatja, hogyan felhasználó is hozzáférhet az adatforráshoz.

## <a name="preview"></a>Előzetes verzió

Azure adatkatalógus megjelenítése az adatforrás kiolvasott regisztráció során, és be a az adatok eszköz metaadatokkal legfeljebb 20 rekordokat pillanatképét. Az előnézet segítségével, hogy a felhasználók, akik a adatok eszköz felfedezése jobban megértheti függvény és a célra. Más szóval megjelenik a mintaadatok lehet több értékes, mint megjelenik a csak az az oszlop nevét és adattípusát.
Előnézetek táblák és nézetek csak támogatottak, és kell lennie explicit módon a felhasználó által választott regisztráció során.

## <a name="data-profile"></a>Adatok profil

Azure adatkatalógusában adatok profil egy regisztrált adatok eszköz, amely az adatforrás kiolvasott regisztráció során, és az adatok eszköz metaadatokkal be a tábla- és oszlop szintjének metaadatait pillanatképét. Az adatok profil segíthet a felhasználók, akik a adatok eszköz felfedezése jobban megértheti függvény és a célra. Hasonló nevű szoftver előzetes verzióihoz profilok kell kell kifejezetten a felhasználó által választott regisztráció során.

> [AZURE.NOTE] Adatok profil kiolvasó lehet egy nagy táblák és nézetek költséges műveletet, és jelentősen növelheti a adatforrás regisztrálásához szükséges időt.

## <a name="user-perspective"></a>Felhasználói szempontjából

Azure adatkatalógusában bármelyik felhasználó egy olyan regisztrált adatai eszköznél biztosíthat leíró metaadatok. Minden felhasználó rendelkezik az adatokkal és tartalmaz különböző perspektíva. Ha például a felelős a kiszolgáló rendszergazdája előfordulhat, hogy részletesen ismertetik a szolgáltatásiszint-szerződés (SLA) vagy a biztonsági másolat Windows rendszerben, adatgondnok biztosíthatják hivatkozásokat dokumentációjában találhat az üzleti folyamatok az adatok támogatja; és -elemző előfordulhat, hogy leírást is, amelyek más elemzők legrelevánsabbnak, és ez lehet legértékesebb azon felhasználóknak, akik Fedezze fel, és megértette az adatokat kell feltételekkel.

A perspektívák mindegyike eleve értékes, és Azure adatkatalógusban minden felhasználó információval szolgálhat a találó őket, amíg minden felhasználó használhatja ezeket az információkat megtudhatja, hogy az adatok és célját.

## <a name="expert"></a>Szakértői szintig

Egy szakértővel a felhasználók, akik az adatok eszköz egy tájékoztatni "szakértői" perspektíva azzal azonosították. Bármelyik felhasználó magukat vagy egy másik felhasználó felveheti az eszköz terület szakértője. Aktív állapotúként szakértő nem bajlódnia további jogosultságokra Azure adatkatalógusában; lehetővé teszi a felhasználóknak, hogy egyszerűen keresse meg azokat, amelyek lehetnek hasznosak, ha egy eszköz leíró metaadatok megtekintésével nagy valószínűséggel perspektívák.

## <a name="owner"></a>Tulajdonos

Tulajdonos az Azure adatkatalógusában egy adatok eszköz kezelésével kapcsolatos további jogosultságokkal rendelkező felhasználó. Felhasználók is tulajdonosává regisztrált adatok eszközök, és a tulajdonosok felveheti mások közös tulajdonosai. További információ [adatok eszközök kezelése](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Tulajdonjogot és kezelésére csak a Standard Edition az Azure adatkatalógus érhetők el.

## <a name="registration"></a>Regisztráció

Regisztráció az eszköz metaadat-alapú adatok kinyerése egy adatforráshoz, és az Azure adatkatalógus szolgáltatás másolás. Adatok eszközök regisztrált ezután vonatkozóan kiegészítések értelmében és talált.

## <a name="see-also"></a>Lásd még:

- [Mi az Azure adatkatalógus?](data-catalog-what-is-data-catalog.md) – Ez a cikk áttekintést nyújt az Azure adatkatalógus szolgáltatás, az érték biztosít és az esetek támogat.

- Egy végpont oktatóanyag arról, hogy miként Azure adatkatalógus használt forrás adatfeltárási [Azure adatkatalógus – első lépések](data-catalog-get-started.md) – Ez a cikk tartalmaz.  
