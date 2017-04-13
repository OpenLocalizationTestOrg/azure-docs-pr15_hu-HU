<properties
    pageTitle="Mobil tetszés szerint elmélyedhet fogalmak |} Microsoft Azure"
    description="Azure Mobile tetszés szerint elmélyedhet fogalmak"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile tetszés szerint elmélyedhet fogalmak

Mobil tetszés szerint elmélyedhet néhány gyakori fogalmak az összes támogatott platformok határozza meg. Ez a cikk röviden bemutatja a fogalmak.

Ez a cikk a jó kezdő esetén nem Mobile részvételét. Győződjön meg arról is a platformot használja, az adott dokumentációjában olvasható, akkor fogja finomíthatja a fogalmak további részletes példák, valamint lehetséges korlátozások a jelen cikkben ismertetett módon.

## <a name="devices-and-users"></a>Eszközök és a felhasználók
Mobil tetszés szerint elmélyedhet létrehozása egy egyedi azonosítót az egyes felhasználók azonosítja. Az azonosító neve a eszközazonosítót (vagy `deviceid`). Oly módon, hogy az összes alkalmazások működését ugyanarra az eszközre megosztása az azonos eszközazonosítót jön létre.

Implicit módon az azt jelenti, hogy Mobile tetszés szerint elmélyedhet úgy véli, pontosan egy felhasználóhoz tartozó egy eszközt, és így felhasználó és eszköz egyenértékű fogalmak.

## <a name="sessions-and-activities"></a>Munkamenetek és a tevékenységek
A munkamenet az alkalmazás a felhasználó által végzett használatát, az időpontot a felhasználó elkezdi használni kezdi, amíg a felhasználó leáll.

Egy tevékenység az alkalmazás végrehajtott egy felhasználó által megadott alszint részét használatát (általában a képernyőn, de semmi az alkalmazás megfelelő lehet).

A felhasználó csak egyszerre egy tevékenység végre.

Egy tevékenység azonosítjuk (64 karakter korlátozódik) nevét, és tetszés szerint ágyazhatja be néhány további adatok (a korlát 1024 bájt).

Munkamenetek a program automatikusan a felhasználók által végzett tevékenységek sorozatának számítja ki. A munkamenet elindul, amikor a felhasználó elindítja az első tevékenységét, és leállítja a ő utolsó tevékenységét befejeződésekor. Ez azt jelenti, hogy a munkamenet, nem szükséges külön elindul, illetve leáll. Ehelyett tevékenységek kifejezetten lépések vagy leállt. Ha nincs tevékenység jelenteni, nincs munkamenet jelenteni.

## <a name="events"></a>Események
Események használatával jelentés azonnali műveletek (például gomb megnyomva, vagy olvassa el a felhasználók által cikkek).

Esemény is kapcsolódik az aktuális munkamenet futó feladatot, vagy egy különálló esemény lehet.

Esemény azonosítjuk (64 karakter korlátozódik) nevét és tetszőlegesen ágyazhatja be néhány további adatok (1024 bájt korlát).

## <a name="error"></a>Hibaüzenet
Hibát megfelelően az alkalmazás (például nem a megfelelő felhasználói műveletek, vagy API-hívás hibák) által talált hibákat használhatók.

Hiba is kapcsolódik az aktuális munkamenet futó feladatot, vagy egy különálló hiba lehet.

Hiba azonosítjuk (64 karakter korlátozódik) nevét, és tetszés szerint ágyazhatja be néhány további adatok (a korlát 1024 bájt).

## <a name="job"></a>Feladat
Feladatok használt műveletek időtartammal rendelkező jelentés (például API-hívások időtartama megjelenítése hirdetések időpontja, a háttér tevékenységek időtartamát vagy a felhasználói műveletek időtartama).

A feladat nem kapcsolódik munkamenet, mert egy tevékenység lehet végrehajtani a háttérben felhasználói beavatkozás nélkül.

A feladat azonosítjuk (64 karakter korlátozódik) nevét, és tetszés szerint ágyazhatja be néhány további adatok (a korlát 1024 bájt).

## <a name="crash"></a>Összeomlik
Összeomlik automatikusan a Mobile tetszés szerint elmélyedhet SDK alkalmazáshibák, ahol az alkalmazás által nem észlelt problémák legyen crash jelentés által kibocsátott.

## <a name="application-information"></a>Alkalmazás adatai
Alkalmazás adatai (vagy alkalmazás információ) felhasználója, ez azt jelenti, hogy címkézéséhez való társításához használatos bizonyos adatok (Ez a hasonló webes cookie-kat, azzal a különbséggel, hogy az alkalmazás információ tárolja az Azure Mobile tetszés szerint elmélyedhet platformon kiszolgálóoldalon) alkalmazás a felhasználóknak.

Alkalmazás információ lehet regisztrálni a Mobile tetszés szerint elmélyedhet SDK API vagy a Mobile tetszés szerint elmélyedhet platform eszköz API segítségével.

Alkalmazás információ eszközre tartozó kulcs/érték két. A kulcs az alkalmazás info (legfeljebb 64 ASCII betűket [a-hamza-Z], [0 – 9-es] számok és aláhúzás [_]) nevére. Az érték (1024 karakterre korlátozza) lehet olyan karakterlánc, egész szám, dátum (éééé-MM-dd) vagy logikai érték (IGAZ vagy hamis).

Tetszőleges számú app útmutatók társítható egy eszközt a Mobile tetszés szerint elmélyedhet árak kifejezéseket által meghatározott korlátok. Egy adott billentyű Mobile tetszés szerint elmélyedhet csak nyomon követi, hogy a legújabb érték beállítás (nincs Előzmények). Beállítása vagy módosítása az alkalmazás információ értékének kezd Mobile tetszés szerint elmélyedhet újra a közönség feltétel beállítása a alkalmazás info (ha van ilyen) ezekkel az információkkal alkalmazás azaz használható indíthatja el a valós idejű veremmutatót ki szeretné számítani.

## <a name="extra-data"></a>További adatok
További adatok (vagy extrák), amely kapcsolható eseményeket, a hibák, a tevékenységek és a feladatok tetszőleges adatokat.

Extrák hasonlóképpen szerkezete JSON objektumok: a kulcs/érték párokká fa kiemelhetők. Billentyűk korlátozódik 64 ASCII-betűk [a-hamza-Z], [0 – 9-es] számok és aláhúzás [_]) és a teljes extrák mérete legfeljebb 1024 karakter hosszúságúak lehetnek (egyszer kódolt JSON által a Mobile tetszés szerint elmélyedhet SDK).

Kulcs/érték párokká teljes fa JSON objektumként vannak tárolva. Azonban csak az első szintű kulcsok/értékek lebontott bizonyos speciális függvények, például szakaszokat közvetlenül elérhető lesz (például egyszerűen meghatározhatja a minden felhasználónak "content_viewed" nevű esemény legalább 10 alkalommal elküldését tevődnek "SciFi ventillátortól" nevű szakasz a felesleges kulcs "content_type" az elmúlt egy hónapban "scifi" értékre állítja a). Így erősen ajánlott küldése csak extrák készült kulcs/érték párokká skaláris érték (például karakterláncok, dátumok, egész számok vagy logikai érték) használatával egyszerű listája.

## <a name="next-steps"></a>Következő lépések

- [Univerzális SDK Windows Azure Mobile tetszés szerint elmélyedhet áttekintése](mobile-engagement-windows-store-sdk-overview.md)
- [Az Azure Mobile tetszés szerint elmélyedhet a Windows Phone Silverlight SDK – áttekintés](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS Azure Mobile tetszés szerint elmélyedhet SDK](mobile-engagement-ios-sdk-overview.md)
- [Azure mobil tetszés szerint elmélyedhet Android SDK](mobile-engagement-android-sdk-overview.md)
