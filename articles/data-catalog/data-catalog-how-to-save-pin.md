<properties
   pageTitle="Hogyan lehet menteni a keresést, majd adatok eszközök rögzítése |} Microsoft Azure"
   description="Ennek a cikk az Azure adatkatalógus funkciói kiemelés mentési adatforrások és az adatok eszközök későbbi használatra."
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
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Hogyan lehet menteni a keresést, majd adatok eszközök rögzítése

## <a name="introduction"></a>– Bevezetés

Microsoft Azure adatkatalógus forrás feltáráshoz szolgáltatásokat nyújt. Felhasználók gyorsan kereshet, és keresse meg az adatforrások és azok rendeltetését megérteni a katalógus szűrése, így könnyebben megtalálhatja a megfelelő adatokat a feladat kéznél.

De mit kell tudni a felhasználók rendszeresen használata ugyanazzal az adattal szükség esetén? Mi a helyzet, amikor felhasználók rendszeresen közreműködés ismereteiket a katalógus azonos adatforrásokhoz? Ezekben az esetekben, hogy az azonos keresések többször ki lehet hatékony – Ez az adott mentett keresés és a kiemelt eszközök segítségével adatokat.

## <a name="saved-searches"></a>Mentett keresések

Azure adatkatalógusában mentett keresés egy újrafelhasználható, felhasználónkénti keresési definíciója. Amikor a felhasználó definiálva van – ideértve a keresési kifejezések, címkéket és más szűrők – keresés ő későbbi felhasználásra mentheti. A mentett keresés definition majd ismételt futtatása későbbi időpontban való visszatéréshez bármely a keresési feltételeknek megfelelő adatokat eszközök.

### <a name="creating-a-saved-search"></a>Mentett keresés létrehozása

Mentett keresés létrehozásához írja be a keresési feltételek fel újra. Kattintson a "Mentés" hivatkozás "aktuális" keresőmezőbe az Azure adatkatalógus portálon.

 ![Select "Mentés" az aktuális beállítások](./media/data-catalog-how-to-save-pin/01-save-option.png)

Amikor a rendszer kéri, adja meg a mentett keresés nevét. Válassza ki, hogy értelmes és a keresés által visszaadott adatok eszközök leíró nevét.

 ![Nevezze el a mentett keresés](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Kezelése mentett keresések

Ha egy felhasználó mentette egy vagy több keresés a "Mentett keresés" lehetőség jelenik meg az Azure adatkatalógus portálon "aktuális" keresőmező alatt. Ha ki van bontva, keresések menti a teljes listájának jelennek meg.

 ![Mentett keresés listája](./media/data-catalog-how-to-save-pin/03-list.png)

Mentett keresés kiválaszthatja a listából futtatható a keresést miatt.

Jelölje ki a legördülő menü management választhatnak adja meg:

 ![Mentett keresés kezelésére szolgáló beállítások](./media/data-catalog-how-to-save-pin/04-managing.png)

Válassza a "Átnevezése" kérni fogják felhasználóiktól a felhasználót, hogy adja meg a mentett keresés új nevét. A keresés definition nem lehet módosítani.

Válassza a "delete műveletet" a felhasználó megerősítését kéri, és majd távolítsa el a mentett keresés a felhasználót a listából.

Jelölje ki a "Mentés másként alapértelmezés" megjelöli a választott keresési menti a felhasználó az alapértelmezett keresése. A felhasználó az "üres" Keresés az adatkatalógusban Azure kezdőlapról hajt végre, ha a felhasználó alapértelmezett keresés hajtani. Ezenkívül a keresés megjelölt alapértelmezett jelenik meg a mentett keresés lista tetején.

### <a name="organizational-saved-searches"></a>Szervezeti mentett keresések

Minden felhasználónak saját használatra keresések mentheti. Adatok katalógus rendszergazdák is mentheti a keresést az összes felhasználó számára a szervezeten belül. Keresés mentésekor a rendszergazdák egy beállítást választja, a vállalaton belül a mentett keresés megosztása jelennek meg. Ha ez a beállítás ki van jelölve, a mentett keresés a listában az összes felhasználó számára elérhető keresések szerepelni fog.

 ![Szervezeti mentett keresések](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Rögzített adatokat eszközök

Mentett keresés engedélyezése a felhasználóknak mentéséhez és újbóli keresési definíciókat; a keresés által visszaadott adatok eszközök a katalógus módosítása tartalmával idő után megváltoznak. Adatok eszközök rögzítése lehetővé teszi a felhasználóknak, így azok könnyebben access anélkül, hogy a keresés meghatározott adatokat eszközök kifejezetten azonosítása.

Egy adatok eszköz rögzítése nem túl bonyolult feladat – a felhasználók is egyszerűen kattintson a "" rajzszögikonra az adatok eszköz rögzített címzettet hozzáadni. Ez az ikon jelenik meg az eszköz csempére, a mozaik elrendezés nézetben, majd a listanézet az Azure adatkatalógus portál bal szélső oszlopában sarkában.

![A adatok eszköz rögzítése](./media/data-catalog-how-to-save-pin/05-pinning.png)

Egy tárgyi eszköz unpinning egyaránt egyszerű – felhasználója egyszerűen kattintson a "" ikonnal újra állítsa át a kijelölt eszköz beállítást.

![A adatok eszköz unpinning](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"A eszközök"
Az Azure adatkatalógus portál Kezdőlap lapján, amelyet a vizsgált eszközök az aktuális felhasználót a "Saját eszközök" szakasz tartalmazza. Ez a szakasz mindkét rögzített eszközöket is tartalmaz, és mentett keresések.

!["A eszközök" a Kezdőlap lapon](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Összefoglalás
Azure adatkatalógus funkciók, amelyek megkönnyítik a felhasználók az adatforrások van szüksége, fel, így azok is kevesebb időt keres adatokat, és azt használata több időt biztosít. Mentett kereséseket, és a kiemelt eszközök épülnek ezeket a alapvető műveleteket, így a felhasználók könnyen megtalálják adatforrásokat, amelyeknek ezek fognak működni, többször adatok.
