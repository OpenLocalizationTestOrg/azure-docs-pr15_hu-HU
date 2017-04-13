<properties
    pageTitle="Hogy miként állíthatja be a vállalati szószedet, a címkézés szabályozott |} Microsoft Azure"
    description="Ennek a cikk a vállalati szószedet Azure adatkatalógusában kiemelése a definiálása és használata egy gyakori üzleti tanuláskor fontos címke az adatok eszközök regisztrálva."
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

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Hogy miként állíthatja be a vállalati szószedet a szabályozott címkézéshez

## <a name="introduction"></a>– Bevezetés

Azure adatkatalógus biztosít a forrás felhőbeli adatfeltárási, funkciókhoz engedélyezése a felhasználóknak egyszerű felderítése és a szükséges döntéseket és az elemzést végezhet adatforrások megértéséhez. Ezek dokumentumfelfedezési funkcióját legyen a legnagyobb hatás felhasználók megkeresése és az elérhető adatforrásokat a lehető tartomány megértéséhez.

Egy olyan adatkatalógus szolgáltatás, amely elősegíti az eszközök adatok nagyobb ismertetése címkézés van. Címkézés lehetővé teszi a felhasználóknak kulcsszavak társítása tárgyi eszköz vagy az oszlop, az megkönnyíti a keresést, illetve böngészési eszköz fel, és lehetővé teszi, hogy a felhasználók könnyebben megérthető, a környezet és az eszköz szándékának.

Azonban címkézés előfordul, hogy problémákat okozhatnak saját. Néhány példa a címkézés bevezetett problémákhoz a következők:

1.  A felhasználók rövidítéseket használata egyes eszközök és a kibontott szöveg mások címkézés közben. A ellentmondás hátráltatja eszközök felfedezése, akkor is, ha a szándék elérését segítő eszközök címkézéséhez az azonos címkével ellátott volt.
2.  Címkék, amely a különböző környezetekben különböző dolgot jelenti. Például "Bevételek" nevű a egy ügyfél adathalmaz címke jelentheti ügyfelenként bevétel, de az adott címkét a negyedéves értékesítési adatkészletet jelentheti a negyedéves bevétel a vállalat számára.  

Súgó és ezek, és más hasonló kihívásokkal kapcsolatban, adatkatalógus egy üzleti szószedet tartalmaz.

Az adatok katalógus üzleti szószedet lehetővé teszi a dokumentum kulcsfontosságú üzleti adatokra szervezetek és azok definíciók létrehozása egy gyakori üzleti szószedet. Ez az irányítási lehetővé teszi, hogy az adatok használatát konzisztencia a szervezeten belül. A vállalati szószedet kifejezéseket határozzák meg, miután őket tevékenységekhez hozzárendelhető adatok eszközök a katalógus, ugyanezt a megközelítést a címkézés, használják, ezáltal a _címkézés szabályozott_.

> [AZURE.NOTE] A jelen cikkben ismertetett funkciók csak a Standard Edition az Azure adatkatalógus érhetők el. Az ingyenes Edition nem nyújt irányadó címkézéshez vagy a vállalati szószedet funkciókhoz.

## <a name="glossary-availability-and-privileges"></a>Szószedet elérhetőségét és jogosultságok

*A vállalati szószedet a Standard Edition az Azure adatkatalógusában érhető el. Az ingyenes Edition az adatkatalógusban nem tartalmaz egy szószedet.*

A vállalati szószedet az adatkatalógusban portál navigációs menü az "Szószedet" beállítás a keresztül is elérhető.  

![A vállalati szószedet elérése](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Adatok katalógus rendszergazdák és a szószedet Rendszergazdák szerepkör tagjai is létrehozása, szerkesztése és törlése a vállalati szószedet szószedet feltételeit. Adatkatalógus a minden felhasználó megtekintheti a szerződési időszak definíciók és eszközök szószedet adatokkal is nyomon követése.

![Szószedet új kifejezés hozzáadása](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Szószedet kifejezések létrehozása

Adatok katalógus és szószedet rendszergazdák hozhat létre új címszó kattintson az új kifejezéshez "címszó létrehozásához az alábbi mezők gomb:

* A kifejezés üzleti meghatározása
* Egy leírást, amely a használatra vagy üzleti szabályok az eszköz oszlop rögzítése
* A résztvevők, akiknek a legtöbb tudnivalók a kifejezés lista
* A szülő kifejezés, amely meghatározza a hierarchia, amelyben a kifejezés vannak rendezve


## <a name="glossary-term-hierarchies"></a>Szószedet kifejezés hierarchiák

Az üzleti adatkatalógus szószedet lehetővé teszi a ismertetik a vállalati tanuláskor fontos kifejezések hierarchia szerint. Ezzel a szervezetek hozhat létre kifejezéseket besorolása, amely a vállalati osztályozási jobban képviseli.

Az a szerződési időszak nevének egyedinek kell lennie egy adott a hierarchia szintjén – ismétlődő nevek nem használhatók. A hierarchia szintek száma nem korlátozott, de a hierarchia gyakran könnyebben értendő három szintre vagy kevesebb esetén.

A vállalati szószedet a hierarchiák használata nem kötelező. Kifejezés a mező üres a szülő elhagyása szószedet adatokra hoz létre kifejezéseket sima (nem hierarchikus) listája a szószedet.  

## <a name="tagging-assets-with-glossary-terms"></a>Szószedet adatokkal címkézési eszközök

Miután címszó belül a katalógus definiálva van, a felület a címkézés eszközök, a felhasználó gépel, a címkék a gyűjteményt kereséséhez van optimalizálva. Az adatkatalógus portál szószedet feltételek, a felhasználó választhatja ki a megfelelő listáját jeleníti meg. Ha egy felhasználó kijelöli szószedet kifejezés a mező bekerül az eszköz címkeként (más néven listában szószedet címke). Hozzon létre egy új címke olyan kifejezés, amely nem szerepel a szószedet (más néven beírásával is dönthet a felhasználó felhasználói címke).

![Egyetlen felhasználó címkével és a két szószedet címkék címkézett adatok eszköz](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Felhasználói címkék a címke, melyet az ingyenes Edition az adatkatalógusban csak típusát.

### <a name="hover-behavior-on-tags"></a>Vigye az egérmutatót a címkék viselkedés
Az adatkatalógus portálon címkék kétféle dolgozó vizuálisan, különböző hover működő. Egy felhasználó címkét a felhasználó való rámutatáskor akkor láthatják a címke szöveget, és a felhasználó vagy felhasználók, akik felvették a címkét. Való rámutatáskor a felhasználók szószedet címke, azok is megjelenik a szószedet kifejezés és a hivatkozás a vállalati szószedet az időszak teljes meghatározása megtekintéséhez nyissa meg a definícióját.

### <a name="search-filters-for-tags"></a>Keresés címkék szűrők
Szószedet címkék és a felhasználó címkék kereshető, és a keresés szűrőként alkalmazhatók.

## <a name="summary"></a>Összefoglalás
A vállalati szószedet az Azure adatkatalógus, és lehetővé teszi az irányadó címkézés lehetővé teszi az adatok eszközök azonosíthatók, felügyelt és következetesen talált. A vállalati szószedet a felhasználók a szervezet egyik ablaktábláról a másikra az üzleti szószedet tanulási előléptetheti és értelmes metaadatai rögzítése, hogy kezdeményezhet eszköz feltárás és támogat egy Gyerekjáték ismertetése.

## <a name="see-also"></a>Lásd még:

- [REST API-dokumentáció üzleti szószedet műveletekhez](https://msdn.microsoft.com/library/mt708855.aspx)
