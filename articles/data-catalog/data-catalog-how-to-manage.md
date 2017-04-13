<properties
   pageTitle="Adatok eszközök kezelése |} Microsoft Azure"
   description="Hogyan szabályozható a láthatóság és adatok eszközök tulajdonjogának kiemelés cikkben található útmutató Azure adatkatalógus regisztrálni."
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


# <a name="how-to-manage-data-assets"></a>Adatok eszközök kezelése

## <a name="introduction"></a>– Bevezetés

**Azure adatkatalógus** biztosít a forrás felhőbeli adatfeltárási, funkciókhoz engedélyezése a felhasználóknak egyszerű felderítése és a szükséges döntéseket és az elemzést végezhet adatforrások megértéséhez. Ezek dokumentumfelfedezési funkcióját legyen a legnagyobb hatása az összes felhasználó megkeresése és az elérhető adatforrásokat a lehető tartomány megértéséhez. Ezzel szem előtt adatkatalógus alapértelmezett viselkedésének látható – és ismerjék fel minden regisztrált adatforrások – felhasználók a katalógus összes.

Adatkatalógus nem felhasználók hozzáférésének biztosítása az adatok magát. Az adatforrás tulajdonosa szabályozza adatokhoz való hozzáférés. Adatkatalógus lehetővé teszi, hogy a felhasználók adatforrások felderítésére, valamint a metaadat-alapú kapcsolódó regisztrált be a forrás megtekintése.

Lehetnek esetek, amikor, azonban hol adatforrások csak akkor látható, az adott felhasználókhoz, illetve a bizonyos csoportok tagjai. Ez a helyzet adatkatalógus lehetővé teszi a felhasználóknak a katalógus belül regisztrált adatok eszközök tulajdonlásának átvétele, és majd szabályozhatja az eszközök láthatóságát tulajdonosa.

> [AZURE.NOTE] A jelen cikkben ismertetett funkciók csak a Standard Edition az Azure adatkatalógus érhetők el. Az ingyenes Edition nem nyújt tulajdonjogot és korlátozásáról adatok eszköz láthatóság funkciókhoz.

## <a name="managing-ownership-of-data-assets"></a>Adatok eszközök tulajdonjogának kezelése
Alapértelmezés szerint tulajdonos; olyan adatkatalógusában regisztrált adatok eszközök minden felhasználó, aki a katalógus eléréséhez szükséges engedély képesek felderíteni és a jegyzetelés ezek az eszközök. Felhasználók is igénybe vehet a tulajdonos adatok eszközök tulajdonjogát, és korlátozhatja az eszközök őket a saját olvashatóságát.

Amikor egy adatok eszköz adatkatalógusában tulajdonában van, csak azok a felhasználók által a tulajdonosok engedélyezett, az eszköz felfedezése, és megtekintheti a metaadat-alapú és a tulajdonosok csak törölheti az eszköz a katalógusról.

> [AZURE.NOTE] Tulajdonjogot adatkatalógusában csak be a metaadat-alapú hatással van. Nem rendelkezik az alapul szolgáló adatforrás engedélyeket.

### <a name="taking-ownership"></a>Tulajdonjogot írása
Felhasználók által a "Készítése tulajdonjogot" lehetőséget választva az adatkatalógusban portálon adatok eszközök is tulajdonosává. Nincs speciális engedélyek szükségesek tulajdonos adatok tárgyi eszköz; tulajdonlásának átvétele bármelyik felhasználó is igénybe vehet a tulajdonos adatok tárgyi eszköz tulajdonjogát.

### <a name="adding-owners-and-co-owners"></a>Tulajdonosok és közös tulajdonosok hozzáadása
Ha egy adatok eszköz már tulajdonában van, a felhasználók egyszerűen nem tulajdonosává – őket hozzá kell adnia közös tulajdonos meglévő tulajdonos. Bármely tulajdonosa felveheti a többi felhasználó vagy biztonsági csoportokat közös tulajdonosai.

> [AZURE.NOTE] Egy javasoljuk az legalább két magánszemélyek bármely tulajdonában lévő adatok eszköz tulajdonosai.

### <a name="removing-owners"></a>Tulajdonosok eltávolítása
Ugyanúgy, mint bármely eszköz tulajdonosa közös tulajdonosok vehet fel, bármilyen eszköz tulajdonosa bármely társtulajdonos távolíthatja el.

Ha digitáliseszköz-tulajdonos távolítja el a saját maga tulajdonos, ő kezelheti az eszköz már nem. Ha egy digitáliseszköz-tulajdonos eltávolítja a saját maga tulajdonos és nincs más közös tulajdonosok, az eszköz visszaáll tulajdonos nélküli állapotba.

## <a name="visibility"></a>A láthatóság
Adatok eszköz tulajdonosai szabályozhatja, hogy az adatok eszközök őket a saját olvashatóságát. Az alapértelmezett – a láthatóság korlátozására hol összes adatkatalógus felhasználó képesek felderíteni és megtekintheti az adatok eszköz – az eszköz tulajdonosa válthat a láthatósági beállítás a "mindenki" a "Tulajdonosok és a felhasználók" eszköz tulajdonságok. Tulajdonosok hozzáadhatja a megadott felhasználók és a biztonsági csoportokat.

> [AZURE.NOTE] Amikor csak lehetséges, eszköz tulajdonjogot és láthatóság engedélyekkel kell-e kiosztani biztonsági csoportokat, és nem az egyes felhasználók számára.

## <a name="catalog-administrators"></a>Katalógus rendszergazdák
Adatok katalógus rendszergazdák rendszer implicit módon közös tulajdonosai a katalógus összes eszközök. Digitáliseszköz-tulajdonosok láthatóság nem távolítható el katalógus rendszergazdák, és a rendszergazdák kezelhetik tulajdonjogot és a katalógus összes adatok eszközök láthatóságát.

## <a name="summary"></a>Összefoglalás
Metaadat-alapú és az adatok eszköz feltárás adatok katalógus crowdsourcing modell lehetővé teszi az összes katalógus közreműködés és felfedezése. A Standard Edition az adatkatalógusban a szeretné korlátozni a láthatóság és eszközök meghatározott adatok felhasználása tulajdonjogot és kezelésére szolgáló szolgáltatásokat nyújt.
