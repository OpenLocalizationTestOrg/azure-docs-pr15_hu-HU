<properties
    pageTitle="Az adatok gyár Azure másolás varázsló |} Microsoft Azure"
    description="További tudnivalók az gyári Azure másolás varázsló segítségével adatok másolása a támogatott adatforrások mosdók."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="spelluru"/>

# <a name="azure-data-factory---copy-wizard"></a>Azure adatok Factory - példány varázsló
Az Azure gyár másolás varázsló megkönnyítése érdekében a folyamat ingesting adatok, amely általában az adatok végpont integrációs forgatókönyvek az első lépés. Az Azure gyári másolás varázsló keresztül fog, amikor szükségtelen megértéséhez az összes csatolt szolgáltatások, adatkészleteket és folyamatok JSON-definíciót. Miután a varázsló összes lépését, az a varázsló automatikusan létrehoz egy folyamat adatok másolása a kijelölt adatforrás a kijelölt helyre. Ezenkívül a másolás varázsló segítségével adatérvényesítés a szerzői, idején keresztül alatt a szervezetbe menti az Ön ideje nagy, különösen ha meg van ingesting adatok az első alkalommal az adatforrásból. A másolás varázsló indításához kattintson az **adatok másolása** csempe, az adatok factory kezdőlapján a.

![Másolja a varázsló](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Egy ötletes varázslót adatok másolása
A varázsló segítségével egyszerűen áthelyezését adatokat számos különböző forrásból helyekre perc alatt. Fog a varázsló lépéseit, miután egy folyamat egy másolatot a tevékenységhez automatikusan létrejön függő adatok gyár személyek (csatolt szolgáltatások és adatkészleteket) együtt. További lépések nem szükséges a során létre.   

![Válassza az adatforrás](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Lásd: a [Másolás varázsló oktatóprogram](data-factory-copy-data-wizard-tutorial.md) cikk másolni egy minta folyamat létrehozásához részletes útmutatást adatainak egy Azure blob-Azure SQL-adatbázis táblához. 

A varázsló szem előtt az elejétől nagy adatokkal szolgál. Célszerű az egyszerű és a szerzői adatok gyári folyamatok, helyezze át a mappát, fájlok vagy az adatok másolása varázslóval táblák több száz hatékony. A varázsló az alábbi három szolgáltatásokat támogatja: automatikus adatvillámnézet, a séma rögzítési és a hozzárendelés és a adatainak szűréséről. 

## <a name="automatic-data-preview"></a>Az automatikus adatvillámnézet 
A másolás varázsló lehetővé teszi, hogy tekintse meg a megfelelő adatokat a másolni kívánt kerül az adatok érvényesítése a kijelölt adatforrás adatainak egy részét. Ezenkívül a forrásadatok van egy szövegfájlt, ha a másolás varázsló elemzi a sor és oszlop határoló, és -sémafájlok automatikusan minderről a szövegfájlt. 

![Formátum beállításai](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Megfeleltetés és séma rögzítése 
Előfordulhat, hogy a bemeneti adatok séma nem egyezik meg a kimeneti adatok bizonyos esetekben séma. Ebben az esetben meg kell feleltesse meg a célként megadott séma oszlopot a forrás séma oszlopok. 

A másolás varázsló automatikusan rendeli hozzá a forrás sémában oszlopok oszlopokat a cél sémában. Felülbírálhatja a hozzárendelések a legördülő listák használatával (vagy) adja meg, hogy egy oszlopot kell kihagyja az adatok másolása során.   

![Séma-hozzárendelés](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Adatok szűrése  
A varázslóban jelölje be a csak az adatokat a cél/gyűjtő adatokat tároló másolható kell, hogy a forrás adatainak szűréséhez teszi lehetővé. Szűrés csökkenti a gyűjtő adatokat tároló másolandó adatok mennyisége, és ezért növeli a Másolás a kapacitásának. Szűrheti az adatokat egy relációs adatbázisban rugalmasan használatával SQL lekérdezési nyelv (vagy) fájlok az Azure blob-mappában [Data Factory függvények és a változók](data-factory-functions-variables.md)biztosít.   

### <a name="filtering-of-data-in-a-database"></a>Adatbázis-adatok szűrése  
A példában az SQL-lekérdezés használja a `Text.Format` függvény és `WindowStart` változó. 

![A kifejezések ellenőrzése](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Az Azure blob-mappában adatok szűrése
A mappa elérési útja változók segítségével adatainak másolása egy mappát, amely alapján a [rendszer változói](data-factory-functions-variables.md#data-factory-system-variables)futásidőben határozza meg. A támogatott változót: **{év}**, **{hónap}**, **{nap}**, **{óra}**, **{perc}**és **{egyéni}**. Példa: inputfolder / {év} / {hónap} / {nap}.

Tegyük fel, hogy van-e bemeneti mappákat az alábbi formátumban:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Kattintson a **Tallózás** gombra a **fájl**vagy mappa, tallózással keresse meg azt az alábbi mappák egyikére (például-2016 > 03 -> 01 -> 02), és kattintson a **Választás**gombra. Meg kell jelennie `2016/03/01/02` a szövegmezőbe. Most **2016** **03** **{**hónap}, **01** **{**nap}, **02** **{**óra}, **{year}**, és nyomja le az fülre. Meg kell jelennie a formátumot, az alábbi négy változók jelölje ki a legördülő listák:

![A rendszer változói használatával](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Ahogy az alábbi képernyőképen látható, egy **egyéni** változó és bármilyen [támogatott formátum-karakterláncot](https://msdn.microsoft.com/library/8kb3ddd4.aspx)is használhatja. Egy adott struktúra mappa kijelöléséhez használja először a **Tallózás** gombra. Kattintson egy értéket a **{egyéni}**, és nyomja le fülre, és nézze meg a szövegdobozt, ahol írhat a Formátum-karakterlánc.     

![Egyéni változó használatával](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Különböző adatok és objektumtípusok támogatása
A másolás varázsló használatával hatékonyan áthelyezheti mappák, fájlok vagy táblák több száz.

![Jelölje ki a táblázatok, amelyből az adatok másolása](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Az ütemezési beállítások
Futtathatja a Másolás egyszer vagy ütemezés (óránként naponta, és így tovább). Az alábbi lehetőségek is használható az összekötők szélessége egyszerre több helyszíni, a felhő és az asztali helyi példányt.

Egy egyszeri fájlmásolás lehetővé teszi, hogy a cél forrásból származó adatok mozgás csak egyszer. Adatok bármilyen méretű és bármilyen támogatott formátumú vonatkozik. Az ütemezett példány lehetővé teszi a előírt megismétlődését adatok másolása. Az ütemezett példány konfigurálása gazdag beállítások (például újrapróbálkozási időtúllépés és értesítések) is használhatja.

![Ütemezési tulajdonságai](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Következő lépések
Rövid ismertetését megtalálja az gyári másolás varázsló használatával hogyan hozhat létre egy folyamat másolás tevékenységet, lásd: [oktatóprogram: hozzon létre egy folyamat, a Másolás varázslóval](data-factory-copy-data-wizard-tutorial.md).
