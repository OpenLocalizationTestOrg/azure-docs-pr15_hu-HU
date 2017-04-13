<properties
    pageTitle="Mobil tetszés szerint elmélyedhet exportálás API – áttekintés"
    description="Alapvető tudnivalók a nyers, a felhasználó eszközén használhatja azt az egyéni eszközök által generált adatok exportálása"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Mobil tetszés szerint elmélyedhet exportálás API – áttekintés

## <a name="introduction"></a>– Bevezetés

A jelen dokumentum megtanulhatja az alapvető tudnivalók a felhasználói eszközök használhatja azt az egyéni eszközök által generált nyers adatait exportálni.

## <a name="pre-requisites"></a>Előzetes követelmények

A nyers adatok exportálása a Mobile tetszés szerint elmélyedhet van szükség:

- Használhatja az API-khoz (lásd: a [hitelesítési kézi beállítás](mobile-engagement-api-authentication-manual.md)), API hitelesítési beállítás
- Használja a REST API-hoz vagy a [.net SDK](mobile-engagement-dotnet-sdk-service-api.md)
- Azure tároló fiókkal.

>[AZURE.NOTE] Is véleményét kiváló [Microsoft Azure tároló Explorer](http://storageexplorer.com/)legalább az fejlesztési fázisban egy könnyen használható felhasználói felület Azure tároló személlyel lehetővé teszi.

## <a name="what-can-be-exported"></a>Mi lehet exportálni?

Mobil tetszés szerint elmélyedhet lehetővé teszi a felhasználóknak számos különböző típusú adatok összegyűjtése és így ezek különböző adattípusú igazodó exportálása különböző típusú.
Exportálás 2 alapvető típusát különböztethetjük meg:

- Pillanatkép: használt általában adatok exportálása, amely jelöl egy állam és, amelynek Mobile tetszés szerint elmélyedhet nincs előzményeit. Címkék (app – útmutatók) tokenek vagy leküldéses marketingkampány visszajelzések például tartalmazó beállítás. Exportálás az ilyen típusú következtében nem kapcsolódnak dátummá.
- korábbi: exportálás típus, például összegzi például események vagy tevékenységek időbeli adatok szolgál.

Az alábbi táblázat ismerteti a teljes körűen összes lehetséges export:

| Exportálás típusa | Adattípus | Leírás                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Pillanatkép    | Leküldéses      | Létrehoz egy deviceid/felhasználóazonosító alapon leküldéses kampányok visszajelzések exportálása                                                              |
| Pillanatkép    | Címke       | Hozza létre az egyes eszközök társított címkék (app – útmutatók) exportálása                                                                       |
| Pillanatkép    | Eszköz    | Hozza létre a legtöbb az eszközöket, például a technicals (modell, területi beállítások, időzóna,...), a címkék, először látható az adatok exportálása... |
| Pillanatkép    | Jogkivonat     | Hozza létre az érvényes tokenek exportálása                                                                                                 |
| Korábbi  | Tevékenység  | Létrehoz egy adott időszakban lévő egyes eszközök esetén minden tevékenység exportálása                                                           |
| Korábbi  | Esemény     | Létrehoz egy adott időszakban lévő egyes eszközök esetén minden tevékenység exportálása                                                           |
| Korábbi  | Feladat       | Létrehoz egy adott időszakban lévő egyes eszközök esetén a feladatok exportálása                                                                 |
| Korábbi  | Hibaüzenet     | Létrehoz egy adott időszakban lévő egyes eszközök esetén a hibák exportálása                                                               |

## <a name="how-does-it-work"></a>Hogyan működik?

Exportnak hosszúak futó feladatok alakulhat ki a nagy adatfájlok. Ezért ezeket nem hívható való visszatéréshez közvetlenül egy fájl letöltésére.
Annak érdekében, hogy az adatok exportálása Mobile tetszés szerint elmélyedhet, be kell hozzon létre egy **Feladat exportálása** API segítségével megadhatja a általában:

- Milyen típusú exportálás (pillanatfelvétel vagy historikus)
- Az adattípus
- Az **Azure tároló tároló** (beleértve a egy érvényes Társítások írási hozzáféréssel) hol kell írni az Exportálás eredményét.

Felhívjuk a figyelmét arra, hogy a feladat indítható el néhány percig tart, és azt előfordulhat, hogy futtassa a kisméretű alkalmazások néhány másodpercig-felhasználóknak vagy a tevékenység nagyszámú tartalmazó alkalmazások több órát.

Amikor létrejött a feladatot, akkor lehet szeretné látni, ha még mindig fut, vagy ha elvégezte a állapotának ellenőrzéséhez.

Miután a feladat sikeres van, az eredményül kapott adatfájl érhető el a megadott tároló tároló.
