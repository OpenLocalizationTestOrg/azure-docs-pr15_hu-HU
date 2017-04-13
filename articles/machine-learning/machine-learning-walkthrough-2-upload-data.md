<properties
    pageTitle="Lépés: 2: Adatok feltölteni egy gépi tanulási kísérlet |} Microsoft Azure"
    description="Lépés: 2, az elektronikus oktatóanyagok elkészítése prediktív megoldás Útmutató: feltöltés tárolt nyilvános adatokban Azure gépi tanulási Studio be."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Áttekintése a következő lépés: 2: Azure gépi tanulási kísérlet feltölteni a meglévő adatok

Ez a forgatókönyv, [az Azure gépi tanulási prediktív analytics megoldást fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md) , a második lépés


1.  [Gépi tanulási munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Töltse fel a meglévő adatok**
3.  [Hozzon létre egy új kísérlet](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Képzése és a modellek felmérése](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [A webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)
6.  [A webszolgáltatás elérésére](machine-learning-walkthrough-6-access-web-service.md)

----------

Hitelképesség-kockázat prediktív modell fejleszt, szükségünk van, amely azt segítségével képzése és tesztelje a modell adatait. Ez a forgatókönyv a UCI gépi tanulási tárat a "UCI Statlog (német hitelképesség-adatok) adatkészlet" használjuk. Megtalálhatja a következőképpen:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

A **german.data**nevű fájlt használjuk. A helyi merevlemez-meghajtó töltse le ezt a fájlt.  

Ez az adatkészlet 1000 múltbeli kérelmezőinek hitelképesség 20 változót sorokat tartalmazza. Ezek a 20 változók azonosító jellemzők nyújt a minden hitelképesség-bejelentő az adatkészlet szolgáltatás vektoros jelenítik meg. Minden egyes sor egy újabb oszlopot a bejelentő számított hitelkártya kockázat, a magas kockázatú alacsony hitelképesség kockázat és 300 elkülönítésének 700 pályázókkal jelöli

A UCI webhely pénzügyi adatait, hitelkártyáinak, alkalmazási állapotát és személyes információkat a szolgáltatás vektoros tulajdonságainak leírását tartalmazza. Az egyes bejelentő, a bináris minősítés lett adott, jelezve, hogy-e az alacsony vagy magas követel a kockázat.  

Az adatok hozunk láthatja el ismeretekkel a cserélendő analytics modell. Amikor végzett azt, a modell látnia kell fogadja el az új személyek adatainak és előrejelzésére, hogy-e alacsony vagy magas hitelképesség kockázat.  

Íme egy érdekes jelölés. Az adatkészlet leírása ebből a cikkből megtudhatja, hogy egy személy misclassifying, alacsony hitelképesség kockázat ténylegesen magas hitelképesség kockázat közül 5 megszorzása További költséges pénzügyi intézmény, mint magas, alacsony hitelképesség kockázat misclassifying formátumát. Egy egyszerű a kísérlet során figyelembe vennie ezt módja (5 megszorzása) e tételek megjelenítő rendelkező magas hitelképesség kockázat másolásával. Ezután a modell misclassifies a lehető legalacsonyabb magas hitelképesség kockázat, ha azt hajt végre, téves besorolás 5 megszorzása, egyszer az egyes ismétlődő. Ez a hiba a oktatás között a költség megnöveli.  

##<a name="convert-the-dataset-format"></a>Konvertálás az adatkészlet formátum
Az eredeti adatkészlet egy üres elválasztott formátumot használ. Gépi tanulási Studio jobban működik, vesszővel tagolt (CSV)-fájl segítségével, így azt fogja átalakítani az adatkészlet szóközöket helyettesítve a vessző.  

Számos módon konvertálása ezeket az adatokat. Egyik módja az alábbi Windows PowerShell-parancs használatával:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Úgy, a Unix sed parancs használatával:  

    sed 's/ /,/g' german.data > german.csv  

Bármelyik lehetőséget választja azt az adatokat a kísérlet használjuk **german.csv** nevű fájlban vesszővel tagolt verziójában hozott létre.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Töltse fel az adatkészlet gépi tanulási Studio

Miután az adatok az a CSV formátum alakított, töltse fel a gépi tanulási Studio szükséges. Gépi tanulási Studio – első lépések kapcsolatos további tudnivalókért olvassa el a [Microsoft Azure gépi tanulási Studio kezdőlap](https://studio.azureml.net/)című témakört.

1.  Nyissa meg a gépi tanulási Studio ([https://studio.azureml.net](https://studio.azureml.net)). Jelentkezzen be a program, amikor a fiókot, a munkaterület tulajdonosaként megadott használja.
1.  Kattintson az ablak tetején a **Studio** fülre.
1.  Válassza a **+ Új** az ablak alján.
1.  Jelölje ki az **ADATKÉSZLET**.
1.  Jelölje be a **Feladó helyi fájl**.
1.  Az **új adatkészletet feltöltése** párbeszédpanelen kattintson a **Tallózás gombra** , és keresse meg a létrehozott **german.csv** fájlt.
1.  Adja meg az adatkészlet nevét. Ez a forgatókönyv azt fogja hívja meg "UCI német hitelkártya-adatok".
1.  Adattípus, jelölje ki a **fejléc nélküli általános CSV-fájl (. nh.csv)**.
1.  Megadhat egy leírást, ha szeretné.
1.  Kattintson az **OK gombra**.  

![Az adatkészlet feltöltéséhez][1]  


Ez az adatok ábrázolásakor a kísérlet az adatkészlet modulba feltöltések megjelenítése.

> [AZURE.TIP] Studio feltöltött adatkészleteket kezeléséhez kattintson a Studio ablak bal oldalán a **ADATKÉSZLETEKET** fülre.

További információt a különböző típusú adatokat importál egy kísérlet lásd: [az adatimportálás képzés az Azure gépi tanulási Studio](machine-learning-data-science-import-data.md).

**Tovább: [egy új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
