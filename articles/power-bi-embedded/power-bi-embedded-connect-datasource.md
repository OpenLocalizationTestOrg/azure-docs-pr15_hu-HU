<properties
   pageTitle="Microsoft Power BI beágyazott - kapcsolódás egy adatforráshoz"
   description="A Power BI beágyazott, csatlakozás adatforrásokhoz"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="connect-to-a-data-source"></a>Kapcsolódás egy adatforráshoz

A **Power BI beágyazott**jelentések ágyazhatja be a saját alkalmazásba. Power BI jelentés beágyazása a az alkalmazást, ha a jelentés csatlakozik a mögöttes adatok **importálásával** az adatok vagy való **Csatlakozás közvetlenül** az adatforráshoz a **DirectQuery**másolatot.

Az alábbiakban az **Importálás** és a **DirectQuery**közötti különbségeket.

|Importálás | DirectQuery
|---|---
|Táblázatok, oszlopok, *és az adatok* importálása vagy a jelentés adatkészlet másolja. Ha látni szeretné, hogy a mögöttes adatok-e módosítások, frissítése, vagy importálja, a teljes, az aktuális adatkészlet újra.|Csak a *táblázatok és oszlopok* importált vagy a jelentés adatkészlet másolja. Mindig meg a legfrissebb adatok.
A Power BI beágyazott, felhőbeli adatforrások DirectQuery használhatja, de nem a helyszíni adatforrások jelenleg.

## <a name="benefits-of-using-directquery"></a>DirectQuery használatának előnyei

Van két legfontosabb előnye **DirectQuery**használata esetén:

   -    **DirectQuery** segítségével össze a képi megjelenítések fölé nagyon nagy adathalmazok, ahol egyéb lenne, amelyeknek az első importálás összes adatot.
   -    Alapjául szolgáló adatok módosítását is van szükség az adatok frissítését, és az egyes jelentések, az aktuális adatokat jelenítik meg kell is nagy adatátvitel, amelyeknek újra importáló adatok váljanak. Ezzel ellentétben **DirectQuery** jelentések mindig használja az aktuális adatokat.

## <a name="limitations-of-directquery"></a>DirectQuery korlátai

   Létezik néhány korlátozás, **DirectQuery**használata:

   -    Minden olyan tábla egy adatbázist kell származnia.
   -    Ha a lekérdezés nem túl bonyolult, hiba történik. A hiba orvoslására a lekérdezés kell refactor, így sokkal egyszerűbb. Ha a quuery összetett kell lennie, szüksége lesz **DirectQuery**használata helyett az adatok importálásához.
   -    Kapcsolat szűrése korlátozódik mindkét irányba helyett egy irányra.
   -    Egy oszlop adattípusát nem módosítható.
   -    Alapértelmezés szerint korlátozások mértékek használható DAX-kifejezés kerülnek. Lásd: [DirectQuery és mértékek](#measures).

<a name="measures"/>
## <a name="directquery-and-measures"></a>DirectQuery és mértékek alapján

Annak érdekében, hogy a mögöttes adatforrásban küldött lekérdezések rendelkezik a megfelelő teljesítmény, amelyek korlátozást a mértékek. Amikor a **Power BI Desktopot**, tapasztalt felhasználóknak eldöntheti, hogy megkerülheti ezt a korlátozást kiválasztása a **Fájl > Beállítások > Beállítások**. A **Beállítások** párbeszédpanelen válassza ki a **DirectQuery**, és jelölje ki a **DirectQuery módban korlátlan mértékek engedélyezése**lehetőséget. Ha ezt a beállítást választja, bármely érvényes egy mértéket DAX-kifejezés használható. Lehet, hogy a felhasználók tartsa szem előtt; azonban, hogy bizonyos kifejezések, hajtsa végre nagyon jól az adatok importálásakor vonhat nagyon lassan lekérdezések **DirectQuery** módban kódmentes forrását. 

## <a name="see-also"></a>Lásd még:
- [Ismerkedés a Microsoft Power BI beágyazott](power-bi-embedded-get-started.md)
- [Power BI-asztal](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
