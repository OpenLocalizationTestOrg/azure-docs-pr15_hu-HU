<properties
    pageTitle="Lépés 3: Hozzon létre egy új gépi tanulási kísérlet |} Microsoft Azure"
    description="3 / a fejlesztése a cserélendő megoldás áttekintése a következő lépés: hozzon létre egy új oktatás kísérlet Azure gépi tanulási Studio alkalmazásban."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a>3 áttekintése a következő lépés: Hozzon létre egy új Azure gépi tanulási kísérlet

Ez a forgatókönyv, [az Azure gépi tanulási prediktív analytics megoldást fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md) , a harmadik lépés


1.  [Gépi tanulási munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Töltse fel a meglévő adatok](machine-learning-walkthrough-2-upload-data.md)
3.  **Hozzon létre egy új kísérlet**
4.  [Képzése és a modellek felmérése](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [A webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)
6.  [A webszolgáltatás elérésére](machine-learning-walkthrough-6-access-web-service.md)

----------

Az útmutató a következő lépés, ha egy kísérlet gépi tanulási Studio alkalmazásban, amely az adatkészlet feltölteni azt használja.  

1.  A Studióban válassza a **+ Új** az ablak alján.
2.  Jelölje be a **kísérlet**, és válassza a "Üres kísérlet". Válassza az alapértelmezett kísérlet nevet a vászonra tetején, és nevezze egy kifejező nevet

    > [AZURE.TIP] Tanácsos **összefoglalása** és a **Leírás** adja meg a kísérlet a **Tulajdonságok** ablaktáblában. A Tulajdonságok ad a dokumentumhoz a kísérlet, úgy, hogy bárki, aki vizsgálja meg, később a kitűzött célok és módszertan megértse hozzájuthat a szükséges.

3.  A modul paletta bal oldalán a kísérlet vászon, bontsa ki a **Mentett adatkészleteket**.
4.  Keresse meg az adatkészlet létrehozott **Saját adatkészleteket** csoportban, és húzza azt a vásznat. Az adatkészlet **a keresőmező fölött a színpalettán** a név beírásával is megtalálhatók.  

##<a name="prepare-the-data"></a>Az adatainak előkészítése
Az első 100 adatsorok és a teljes adatkészlet az egyes statisztikai adatokat, kattintson a kimeneti port az adatkészlet (a kis kör alul), és válassza a **Megjelenítés**megtekintése.  

Mivel az adatfájl nem jár, az oszlopfejléceket, Studio nyújtott általános címsorok (Oszlop1, Col2 *stb*.). Jó címsorok nem alapvető hozhat létre egy adatmodellt, de azok megkönnyítése a kísérlet az adatokkal végzett munkához. Is ha végül közzétesszük a modell be egy webszolgáltatásból, a címsorok segítséget nyújt az oszlopokat, a felhasználónak a szolgáltatás azonosítja.  

A [Metaadatok szerkesztése] oszlopfejlécek is hozzáadunk[ edit-metadata] modulra.
A [Metaadatok szerkesztése] használt[ edit-metadata] módosíthatja egy adatkészletet metaadatai modulra. Ebben az esetben ezáltal az oszlopfejléceket további rövid neve. 

[A metaadatok szerkesztése]használatának[edit-metadata], először megadhatja, melyik oszlopot a módosítandó (ebben az esetben az összes oldalról.) Ezután adja meg az oszlopok (ebben az esetben az oszlopfejléceket megváltoztatása.) a végrehajtandó művelet

1.  Írja be a modul paletta "metaadatok" a **Keresés** mezőbe. Látni fogja a [Metaadatok szerkesztése] [ edit-metadata] a modul listájában jelennek meg.
2.  Kattintással és húzással a [Metaadatok szerkesztése] [ edit-metadata] modul a vászonra az alakzatot, és engedje el alatt az adatkészlet, hogy a korábban hozzáadott.
3.  A [Metaadatok szerkesztése]az adatkészlet csatlakozás[edit-metadata]: kattintson a kimeneti port az adathalmaz (a az adatkészlet alján a kis kör.) Ezután húzza a [Metaadatok szerkesztése] a bemeneti port[ edit-metadata] (a a modul a felső részén a kis kör), majd engedje fel az egérgombot. Az adatkészlet és modul továbbra is csatlakoztatott még akkor is, ha Ön Navigálás vagy a vászonra.

    A kísérlet kell ekkor valami hasonló látható:  

    ![A metaadatok szerkesztése hozzáadása][2]
    
    A piros felkiáltójel jelenik meg, az azt jelzi, hogy azt még nem állította a modul tulajdonságainak előtt. Azt fogja végezze el a Tovább gombra.
    
    > [AZURE.TIP] Modulra megjegyzést fűzhet a modul duplán kattint, majd írja be a szöveget. Ez segítséget nyújthat egy pillantással mit a modul tesz a kísérlet az. Ebben az esetben kattintson duplán a [Metaadatok szerkesztése] [ edit-metadata] modul, és írja be a Megjegyzés "Hozzáadás oszlopfejléc". Kattintson bárhová kattintana, a vászonra, a szöveg párbeszédpanel bezárásához. A megjegyzést szeretne megjeleníteni, kattintson a lefelé mutató nyílra a modulra.

4.  Jelölje ki a [Metaadatok szerkesztése][edit-metadata], a **Tulajdonságok** ablaktábla jobb oldalán a vászonra, kattintson a **oszlopkijelölő indítása**.
5.  Az **Oszlopok kiválasztása** párbeszédpanelen jelölje ki az összes sort **Megjeleníthető oszlopok** , majd kattintson > **Kijelölt oszlopok**áthelyezheti őket.
A párbeszédpanel így néz ki: ![oszlopkijelölő az összes kijelölt oszlopok][4]
7.  Kattintson az **OK** be van jelölve.
8.  Vissza a **Tulajdonságok** ablaktáblában keresse meg az **Új oszlop neve** paraméter. Ebben a mezőben adja meg az adatkészlet, és az oszlopok sorrendjének vesszővel elválasztott 21 oszlopai nevek listája. Az oszlopok nevek letöltheti az adatkészlet dokumentáció a UCI webhelyen, vagy kényelmesebbé másolja és illessze be az alábbi listában:  

          Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    A Tulajdonságok ablaktábla fog kinézni:

    ![Szerkesztés metaadatok tulajdonságainak megadása][1]

> [AZURE.TIP] Ha szeretné ellenőrizni az oszlopfejlécekkel, futtassa a kísérlet (kattintson a **Futtatás** parancsra a kísérlet vászon alább). Amikor futása (zöld pipa jelzi megjelenik a [Metaadatok szerkesztése][edit-metadata]), kattintson a [Metaadatok szerkesztése] a kimeneti port[ edit-metadata] modult, és válassza a **Megjelenítés**. Megtekinthetők a kimenetének bármely modul módon, mint a kísérlet az adatoknak állapotának megtekintése

##<a name="create-training-and-test-datasets"></a>Oktatás létrehozása és adatkészleteket tesztelése

A következő lépés a kísérlet, külön adatkészleteket, képzés és tesztelése a modell használjuk létrehozásához.

Ehhez a [Szétválasztott adatok] használjuk[ split] modulra.  

1.  A [Szétválasztott adatok] keresése[ split] modul, húzza az alakzatot a vászonra, és csatlakoztassa az utolsó [Metaadatok szerkesztése] [ edit-metadata] modulra.
2.  Alapértelmezés szerint a megosztási arány 0,5, és a **Randomized felosztása** paraméter értéke. Azt jelenti, hogy az adatokat a véletlen felét keresztül egy, a [Szétválasztott adatok] port kimeneti[ split] modul és keresztül, a másik fél. Beállíthatja, hogy ezek, valamint a **véletlen rendező** paramétert, módosíthatja a felosztott képzés és az adatok vizsgálata között. Ebben a példában hagyja nyitva a azokat-van.
    
    > [AZURE.TIP] A tulajdonság **részét, az első kimeneti adatkészlet sorainak** azt határozza meg az adatok mennyiségét kimeneti a bal oldali kimeneti port keresztül. Például arányát 0,7 állítja be, majd az adatok 70 %-os esetén keresztül a bal oldali port kimeneti és a megfelelő port – 30 %-os.  
    
3. Kattintson duplán a [Szétválasztott adatok] [ split] modult, és írja be a megjegyzést, "adatok oktatás/tesztelése felosztása 50 %-os". 

A kimeneti értékeket, a [Szétválasztott adatok] ábrázolásakor[ split] modul azonban azt hasonló, de most használata mellett dönt a bal oldali kimeneti oktatóanyag adatok és a jobbra kimeneti a tesztelés adatok.  

Az említett a UCI webhelyén, a lehető legalacsonyabb magas hitelképesség kockázat misclassifying költségét öt alkalommal nagyobb, mint a magas, alacsony hitelképesség kockázat misclassifying költségét. A fiókot, akkor hozza létre egy új adatkészletet, amely a költség függvény tükrözi. Az új adathalmazban magas kockázatú példákban van replikált öt időpontok, miközben alacsony kockázat példákban nem replikált..   

Azt megteheti, hogy a replikáció R kód használatával:  

1.  Keresse meg és húzza az [R parancsfájl végrehajtása] [ execute-r-script] alakzatot a kísérlet modul vászon, és csatlakozás a bal oldali kimeneti port, a [Szétválasztott adatok] [ split] az első beviteli porthoz ("Dataset1") az [R parancsfájl végrehajtása] modul[ execute-r-script] modul.
2. Kattintson duplán az [R parancsfájl végrehajtása] [ execute-r-script] modult, és írja be a megjegyzést, "Költség helyesbítése beállítása".
2.  A **Tulajdonságok** ablaktáblában törölje az alapértelmezett szöveget, az **R parancsfájl** paraméter, és adja meg a parancsfájlt:

          dataset1 <- maml.mapInputPort(1)
          data.set<-dataset1[dataset1[,21]==1,]
          pos<-dataset1[dataset1[,21]==2,]
          for (i in 1:5) data.set<-rbind(data.set,pos)
          maml.mapOutputPort("data.set")


A azonos replikációs művelethez minden kimeneti a [Szétválasztott adatok] az ehhez szükséges[ split] modul, hogy a képzés és tesztelés adatokat a azonos költség módosításokat.

1.  Kattintson a jobb gombbal az [R parancsfájl végrehajtása] [ execute-r-script] modul és a választó **másolatot**.
2.  Kattintson a jobb gombbal a kísérlet vászonra, és válassza a **Beillesztés parancsot**.
3.  Csatlakozás az első beviteli port [R parancsfájl végrehajtása] [ execute-r-script] jobbra modul kimeneti port, a [Szétválasztott adatok] [ split] modulra. 
4.  A vászonra alján kattintson a **Futtatás**gombra. 

> [AZURE.TIP] A végrehajtás R parancsfájl modul másolatát a azonos parancsfájl, mint az eredeti modul tartalmazza. Másolja és illessze be a modul a vászonra, a másolatot megmaradnak az eredeti Tulajdonságok parancsot.  

A kísérlet most néz ki:

![A felosztott modul és R parancsfájlok hozzáadása][3]

A kísérletek R-parancsfájlokkal a további tudnivalókért lásd: [bővítése a kísérlet az R](machine-learning-extend-your-experiment-with-r.md).

**Tovább: [vonaton és az adatmodellek kiértékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
