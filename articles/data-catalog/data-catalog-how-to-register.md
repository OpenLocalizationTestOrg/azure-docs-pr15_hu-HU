<properties
   pageTitle="Hogyan lehet rögzíteni az adatforrások |} Microsoft Azure"
   description="Hogyan lehet rögzíteni az adatforrások Azure adatkatalógusban, beleértve a kibontott regisztráció során metaadatmezőket kiemelés útmutató cikk."
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


# <a name="how-to-register-data-sources"></a>Hogyan lehet rögzíteni az adatforrások

## <a name="introduction"></a>– Bevezetés
**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. Más szóval **Azure adatkatalógus** az összes ezzel megkönnyítve a személyek Fedezze fel, megértését és adatforrásokból, és használata segítve a szervezet több értéket a meglévő adatokból. És az első lépés, hogy az adatforrás feltárható **Azure adatkatalógus** keresztül regisztrálhatja adott adatforráshoz.
## <a name="registering-data-sources"></a>Adatforrás regisztrálása
Regisztráció az kiolvasó metaadatokat az adatforrásból, és másolja az adatokat az **Azure adatkatalógus** szolgáltatás folyamatát. Az adatok marad, ahol jelenleg található, és a a rendszergazdák és a jelenlegi rendszer házirendek alatt marad.

Adatforrás regisztrálása, egyszerűen indítsa el az **Azure adatkatalógus** adatok forrás regisztrálási eszköze az **Azure adatkatalógus** portálról. Jelentkezzen be a munkahelyi vagy (a Azure Active Directory hitelesítő adatait használatával jelentkezzen be a portálra) iskolai fiókját, és válassza ki a rögzíteni kívánt adatforrás.
Lásd: az [Első lépések az Azure adatkatalógus](data-catalog-get-started.md) oktatóprogram további részletes információt.

Az adatforrás van regisztrálva, a katalógus nyomon követi a helyéről, és a metaadat-alapú indexek, így a felhasználók keresése, böngészése, és Fedezze fel az adatforrás, és helyére használatával csatlakozhat az alkalmazás vagy tetszés szerinti eszköz használatával.

## <a name="sources-supported"></a>Támogatott adatforrások
Kérjük, olvassa el az [Adatok katalógus DSR](data-catalog-dsr.md) jelenleg támogatott adatforrások listája.
<br/>


## <a name="structural-metadata"></a>Szerkezeti metaadatok
Adatforrás regisztrálásakor esetén a regisztrációs eszköz kiveszi az objektumok, akkor jelölje be a szerkezet információt, – ez az a szerkezeti említett metaadat-alapú.

Objektumok a szerkezeti metaadatok tartalmazni fogja az objektum helye, hogy a felhasználók, akik az adatok felderítésére is csatlakozhat ezeket az információkat az ügyféleszközök elől tetszés szerinti az objektumot. Többi szerkezeti metaadatokat tartalmaz objektum név és típus pedig a attribútum/oszlop nevét és adattípusát.

## <a name="descriptive-metadata"></a>Leíró metaadatok
Mellett a core szerkezeti metaadatokat az adatforrásból kibontása az adatok forrása regisztrációs eszköz is kibontása, valamint a leíró metaadat. Az SQL Server Analysis Services és az SQL Server Reporting Services Ez az alábbi szolgáltatások által elérhetővé tett leírásában származik. Az SQL Server használja a kiterjesztett tulajdonságként ms_description megadott értékek bemutatókban program használja. Az Oracle-adatbázishoz az adatok forrása regisztrációs eszköz fog bontsa ki a megjegyzések oszlop ALL_TAB_COMMENTS nézetben.

A leíró metaadatokat az adatforrásból kibontása, mellett felhasználók is megadhatja az adatok forrása regisztrációs eszközzel leíró metaadatok. A felhasználók adhat hozzá címkéket, és az objektumokhoz, hogy regisztrált szakértői azonosítani tudja. Az összes a leíró metaadatok másolja a program az **Azure adatkatalógus** szolgáltatás a szerkezeti metaadatok együtt.

## <a name="including-previews"></a>Többek között a előnézete

Alapértelmezés szerint csak metaadat-alapú adatforrások kivonjuk, és másolja a **Adatkatalógus Azure** szolgáltatás, de az adatforrás gyakran történik könnyebben szolgáltatók egy mintája szerepel az adatok megismerése tartalmazza.

Az **Azure adatkatalógus** adatok source regisztrációs eszköz lehetővé teszi a felhasználóknak az adatok pillanatkép előnézetét szerepeltetni minden táblánál és regisztrált megtekintése. Ha a felhasználó mellett dönt – a regisztráció során előnézetek felvenni, a regisztrációs eszköz minden táblánál és nézet legfeljebb 20 rekordjait tartalmazza. Ez a pillanatkép majd a katalógus a szerkezeti és leíró metaadatok együtt másolja a program.


> [AZURE.NOTE]  Előfordulhat, hogy az oszlopok nagyszámú széles tábla 20-nál kevesebb rekordok szerepelnek az előnézeti.


## <a name="including-data-profiles"></a>Többek között a profilok

Ahogyan beleértve előnézetek is adnak értékes helyi adatforrásokat **Azure**adatkatalógusában keresése, többek között az adatok profil is megkönnyítheti észlelt adatforrások megértéséhez.

Az **Azure adatkatalógus** adatok source regisztrációs eszköz lehetővé teszi a felhasználóknak minden táblánál és regisztrált nézet profil adatait tartalmazza. A felhasználó úgy dönt, hogy a profil adatait tartalmazza a regisztráció során, ha a regisztrációs eszköz tartalmazni fogja az adatok összesítése statisztikájának minden táblánál és a nézetet, például:

* Sorok és az adatokat az objektum méretét száma
* A dátumot, az adatok és az objektum séma a legutóbbi frissítés
* Null rekordok és az oszlopok különböző értékek száma
* A minimum, maximum, az átlag és szórását érték oszlopokban

Ezeket a statisztikákat majd bemásolja a katalógus a szerkezeti és leíró metaadatok együtt.

> [AZURE.NOTE]  Szöveg és dátum oszlop nem fogja tartalmazni átlagos vagy a szórás statisztikai adatok profiljukat.

## <a name="updating-registrations"></a>Regisztráció frissítése

Adatforrás regisztrálása fog tételére azt a metaadat-alapú és a kibontott regisztráció során választható előzetes **Azure adatkatalógus** . Ha a katalógus frissítése (Ha például az objektum sémájának megváltozott, táblák eredetileg kizárt szerepeljenek vagy előnézete szereplő adatok frissíteni szeretné a felhasználó) az adatforrás van szüksége az adatok forrása regisztrálási eszköz futtassa újra kell.

Egyesítés "upsert" műveletet már bejegyezte adatforrás újraregisztrálása hajt végre: meglévő objektumok frissülnek, miközben létrejön az új objektumokat. Bármely az **Azure adatkatalógus** portálon keresztül a felhasználó által megadott metaadatokat maradnak.

## <a name="summary"></a>Összefoglalás
Adatforrás regisztrálása **Azure** adatkatalógusban könnyebb az adatforrás felderítése és megértéséhez, másolásával szerkezeti és leíró metaadatokat az adatforrásból a katalógus helyezését. Miután az adatforrás van regisztrálva, azt is majd magyarázó jegyzete, felügyelt és észlelt az **Azure adatkatalógus** portálon.

## <a name="see-also"></a>Lásd még:
- [Első lépések az Azure adatkatalógus](data-catalog-get-started.md) oktatóprogram adatforrások rögzítése részletesen olvashat.
