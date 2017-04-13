<properties 
   pageTitle="Az adatok tó eszközök csúcsra adatvégrehajtás nézet használata for Visual Studio |} Microsoft Azure" 
   description="A vizsgálat adatok tó Analytics feladatok csúcsra végrehajtási nézet használatának elsajátítása." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>A csúcs végrehajtás nézetében tó Adateszközök for Visual Studio használata

A vizsgálat adatok tó Analytics feladatok csúcsra végrehajtási nézet használatának elsajátítása.

## <a name="prerequisites"></a>Előfeltételek

- Néhány alapvető ismerete adatok tó Tools for Visual Studio segítségével U-SQL nyelvben parancsfájl fejlesztését.  Lásd: [oktatóanyag: adatok tó eszközeivel for Visual Studio U – SQL-parancsfájlok kidolgozása](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Nyissa meg a csúcs végrehajtás nézet

Egy adott feladathoz kattinthat a "csúcsra végrehajtás" nézethivatkozás bal alsó sarokban. Profilok első betöltése kérheti és eltarthat egy kis időt, attól függően, hogy a hálózati kapcsolat.

![Adatok tó Analytics eszközök csúcsra végrehajtási megtekintése](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Csúcspont végrehajtás nézet ismertetése

Miután beírta a csúcs végrehajtás nézetben, a következők három részből áll:

![Adatok tó Analytics eszközök csúcsra végrehajtás megtekintése](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Csúcspont Adatkijelölő: a bal oldalon a csúcs sorkijelölőre van.  Kijelölhet a csúcsok funkciók (például a 10 legjobb adatok olvassa el, és válassza a szakasz).

    A leggyakrabban használt szűrők egyik a kritikus úthoz tartozó csúcsok. Kritikus út pedig a leghosszabb feladat U-SQL nyelvben. Célszerű az optimalizálás, a feladatok bejelölésével, mely csúcsra a leghosszabb időt vesz igénybe.

- A felső középső ablaktáblán:

    ![Adatok tó Analytics eszközök csúcsra végrehajtás megtekintése](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Ez a nézet is látható minden csúcsok futó állapotát. Az idő megfelelően konvertálja a helyi számítógép zónában, és a különböző színű különböző állapotot jeleníti meg.

- Az alsó középső ablaktáblán:

    ![Adatok tó Analytics eszközök csúcsra végrehajtási megtekintése](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Folyamat neve: A csúcs-példány nevét. Navigálás a StageName áll |} VertexName |} VertexRunInstance. Például a SV7_Split [62] .v1 csúcsra jelentése a második futó példány (.v1, index, 0 kezdve) szakasz SV7_Split 62 csúcsra szám.
    - Teljes olvasási/írási: Az adatok volt, a csúcs által olvasott/írt.
    - Állam/kilépési állapota: A végleges állapot amikor befejeződik a kívánt pozícióba a csúcsot.
    - Kilépés kód/hiba típusa: A hibaüzenet, amikor nem sikerült a kívánt pozícióba a csúcsot.
    - Létrehozás oka: A csúcs létrehozásának okát.
    - Erőforrás késés/folyamat késés/c várólista késés: idő várniuk az erőforrásokhoz, a kívánt pozícióba a csúcsot adatainak feldolgozása, és a várakozási sorban található kapcsolatának.
    - Folyamat/készítő globálisan egyedi azonosítója: Az aktuális futó csúcsra vagy létrehozója GUID azonosítója.
    - Verzió: az N-ed példányát a futó csúcspont (a rendszer előfordulhat, hogy ütemezése a csúcs új példányát az számos oka lehet annak, például a feladatátvétel számítja ki a redundancia stb.)
    - Verzió idő jön létre.
    - Listaszerű folyamatábra létrehozása kezdési idő/folyamat várólistás idő/folyamat kezdési idő/folyamat teljes időt: a csúcs folyamat indításakor létrehozása; a csúcs folyamat indításakor sorba; az egyes csúcsra folyamat indításakor; Amikor az egyes csúcsra befejeződött.

## <a name="next-steps"></a>Következő lépések

- Adatok tó Analytics áttekinti a [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.
- Első lépésként U – SQL-alkalmazások fejlesztésével lásd: [adatok tó Tools for Visual Studio segítségével kidolgozása U – SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
- Az SQL-U című témakörben talál [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md).
- Teendőkről olvassa el az [Azure adatok kezelése tó Analytics Azure portál használatával](data-lake-analytics-manage-use-portal.md)című témakört.
- Diagnosztikai információk naplózása súgójának [elérése diagnosztikai naplók Azure adatok tó elemzéséhez](data-lake-analytics-diagnostic-logs.md)
- Összetett lekérdezés talál további [elemzés webhely naplók Azure adatok tó Analytics használatával](data-lake-analytics-analyze-weblogs.md).
- Feladat részletei megtekintéséhez, lásd: [használata feladat böngészőben és a feladat nézet Azure adatok tó Analytics feladatokhoz](data-lake-analytics-data-lake-tools-view-jobs.md)