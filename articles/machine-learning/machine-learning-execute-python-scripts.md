<properties 
    pageTitle="Hajtsa végre a Python gépi tanulási parancsfájlok |} Microsoft Azure" 
    description="Tagolás tervezése elvek alapjául szolgáló Python parancsfájlok Azure gépi tanulási alapvető felhasználási forgatókönyvek, funkciók és korlátozások támogatása." 
    keywords="Python gépi tanulási, pandas, python pandas, python parancsfájlok, python parancsfájlok végrehajtása"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Azure gépi tanulási Studio Python gépi tanulási parancsfájlok végrehajtása

Ez a témakör ismerteti a tervezés elvek alapjául szolgáló Azure gépi tanulási Python parancsfájlok aktuális támogatása. A főbb funkciók azt is, beleértve a importálása a meglévő kód, a képi megjelenítések exportálása támogatása, és végül a korlátozások, valamint a folyamatban lévő munka tárgyalt.

[Python](https://www.python.org/) elengedhetetlen eszköz a sok adatok tudósok eszköz mellmagasságban. Ezt foglalja magában:

-  egy elegáns és tömör szintaxisa 
-  platformok támogatás 
-  hatékony tárak döntő gyűjteménye és 
-  a Fejlesztőeszközök elért. 

Folyamatban van a Python használja a jellemző felhasználási módjuk látható gépi tanulási modellezése a munkafolyamat minden szakaszában, adatokból ingest és feldolgozása, szolgáltatás építés és modell képzés, és érvényességi vagy modellek telepítési. 

Azure gépi tanulási Studio beágyazási Python parancsfájlok támogatja a kísérlet tanulási és is zökkenőmentesen közzétételéről méretezhető, operationalized webes szolgáltatások a Microsoft Azure gép különböző részre.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Gépi tanulási Python parancsfájlok alapelvei tervezése
A [Végrehajtás Python parancsfájl] található Azure gépi tanulási Studio Python elsődleges felhasználói felületét[ execute-python-script] modul az 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Ábra 1. A **Végrehajtás Python parancsfájl** modulra.

A [Végrehajtás Python parancsfájl] [ execute-python-script] modul fogadja el a legfeljebb három ráfordítások és hoz létre egy vagy két kimeneti értékeket (lásd alább), hasonlóan az R analóg, az [R parancsfájl végrehajtása] [ execute-r-script] modulra. A paraméter mezőben kifejezetten elnevezett bekerül a Python kód futtatható bejegyzés pontos nyomtatáshoz `azureml_main`. Az alábbiakban a modul végrehajtásához használt kulcsfontosságú tervezés elvek:

1.  *Idiomatikus Python felhasználóknak kell lennie.* A legtöbb Python felhasználó tényező függvények belül modulokat, mint kódjukat, tehát a kimutatások végrehajtható sok elhelyez egy felső szintű modulban viszonylag ritka. Emiatt a parancsprogram mezőben is nem pedig a kimutatások a sorozatában kifejezetten elnevezett Python függvény veszi. A függvény megjelenített objektumai szabványos Python dokumentumtár-típus például [Pandas](http://pandas.pydata.org/) adatok keretek és [NumPy](http://www.numpy.org/) tömbök.
2.  *Rendelkeznie kell nagy hűségű közötti helyi és felhőbeli végrehajtások.* A Python kód végrehajtásához használja a kódmentes [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1 elterjedt platformok tudományos Python eloszlás alapján. A leggyakoribb Python csomagok 200 közelébe előtelepített azt. Ezért adatok tudósok hibakeresési és a helyi Azure gépi tanulási-kompatibilis Anaconda környezetre kódjukat jogosultak. Meglévő fejlesztési környezetben, például [IPython](http://ipython.org/) jegyzetfüzet vagy [Python Tools for Visual Studio](http://aka.ms/ptvs) segítségével magas biztonságos Azure gépi tanulási kísérlet részeként futtatni. További, a `azureml_main` belépési pontjához egy vanília Python függvény, és Azure gépi tanulási adott kód nélkül is többen vagy a SDK telepített.
3.  *Zökkenőmentes szerteágazó más Azure gépi tanulási modulok együtt kell lennie.* A [Végrehajtás Python parancsfájl] [ execute-python-script] modul elfogadja, mint a bemeneti adatok alapján és a kimeneti értékeket, szabványos Azure gépi tanulási adatkészleteket. Az alapul szolgáló keretrendszer átlátszó és hatékonyan kulcsösszetevő az Azure gépi tanulási és Python szükséges (támogató szolgáltatások, például a hiányzó értékeket). Python ezért használható együtt a meglévő Azure gépi tanulási munkafolyamatok, beleértve azokat, amelyek R és SQLite Betárcsázás. Egy is ezért számol azzal munkafolyamatok, amely:
  * adatok előfeldolgozása és törlése, Python és Pandas használata 
  * az adatok adatcsatorna SQL átalakítás, csatlakozás több adatkészleteket, űrlap-szolgáltatások 
  * Azure gépi tanulási, algoritmusok rengeteg használata modellek képzése és 
  * felmérése és az eredmények segítségével j utáni feldolgozása


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Egyszerű használat felhasználási helyzetei a gépi tanulási Python parancsfájl
Ebben a részben azt felmérésre, néhány alapvető felhasználási [Python parancsfájl végrehajtása] [ execute-python-script] modulra.
A korábban említett bármely bemenetben Python modulba Pandas adatok keretek vannak közzétéve. További információt a Python Pandas, és hogyan használható szeretné módosítani az adatokat, eredményesen és hatékonyan Nyugat McKinney megtalálható *az adatelemzés Python* (O'Reilly 2012). A függvény csomagolt belül Python [szekvencia –](https://docs.python.org/2/c-api/sequence.html) például egy sor bármelyik eleme, a lista vagy a NumPy tömb egyetlen Pandas adatok keret kell eredményül adnia. A sorozat első elemének kattintson az első kimeneti portot a modul a adja vissza. Ez a rendszer a 2 jelenik meg.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Ábra 2. Leképezése: bemeneti paramétereket portok, és a kimeneti porttal eredmény.

Hogyan határozza meg a bemeneti portokat első megfeleltetve szemantikáját részletesebb a `azureml_main` függvény jelennek meg az 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tartalomjegyzék 1. Függvényparaméterek beviteli portok megfeleltetésének.

A beviteli portokat és Függvényparaméterek közötti hozzárendelés pozíciójelző. Az első csatlakoztatott bemeneti port hozzá van rendelve, a függvény első paraméterként, és a második bemeneti (Ha a csatlakoztatott) a függvény a második paraméter van hozzárendelve.

## <a name="translation-of-input-and-output-types"></a>Fordítási bemeneti és kimeneti típusok
Című cikkben ismertetett módon korábbi, az Azure gépi tanulási beviteli adatkészleteket Pandas keretek és a kimeneti adatok keretek lesznek konvertálva vissza az Azure gépi tanulási adatkészleteket adatok lesznek konvertálva. Az alábbi módon végzi:

1.  Konvertálja a karakterlánc és numerikus oszlopok-van, és egy adatkészlet a hiányzó értékeket Pandas "Név" értékkel alakulnak. Az azonos átalakítás történik, ahogyan vissza (Pandas hiányzik értékek alakulnak át Azure gépi tanulási a hiányzó értékeket).
2.  Az Azure gépi tanulási a Pandas az index módszereket által nem támogatott. Az összes bemeneti adatok képkocka a Python függvény mindig indexűnek 64 bites numerikus 0-tól mínusz 1 sorok számát. 
3.  Azure gépi tanulási adatkészleteket ismétlődő oszlopnevek és oszlopnevek, amelyek nem karakterláncok nem lehet. Egy kimenet adatok keret nem numerikus oszlopot tartalmaz, ha az keretrendszer felhívja `str` oszlopnevei. Hasonlóképpen minden ismétlődő oszlopnevek automatikusan összekeveredése annak biztosítására, hogy a nevek egyediek. Az első ismétlődő, (3) a második ismétlődő, például hogy bekerül az utótag (2).

## <a name="operationalizing-python-scripts"></a>Parancsfájlok Python operationalizing
Bármely [Python parancsfájl végrehajtása] [ execute-python-script] egy pontozási kísérlet használt modulok úgynevezett mint webszolgáltatás közzétételkor. Ábra 3 például egy olyan egyetlen Python kifejezés kiértékelése a kódot tartalmazó pontozási kísérlet jeleníti meg. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Ábra 3. Egy Python kifejezés kiértékelésének webszolgáltatásból.

A kísérlet létrehozott webszolgáltatás tart a szövegbeviteli egy Python kifejezést (karakterláncként), a Python értelmező elküldi azt, és a kifejezés és a kiértékelt eredménye tartalmazó táblázatot ad vissza.

## <a name="importing-existing-python-script-modules"></a>Meglévő Python parancsfájl modulok importálása
A közös-használatieset-sok adatok tudósok meglévő Python parancsfájlok részévé Azure gépi tanulási kísérleteket. Kifejezések és az összes kód beillesztése egy parancsfájl-mezőbe a [Python parancsfájl végrehajtása] helyett[ execute-python-script] modul fogadja el a harmadik bemeneti port Python modulokat tartalmazó zip-fájl csatlakozhat. A fájl ezután kibontott futásidőben a végrehajtás keretrendszer és tartalmát bekerülnek a Python értelmező tár elérési útját. A `azureml_main` függvény importálhatja ezeket a modulokat közvetlenül belépési pontjához.

Példaként fontolja meg a fájlt egy egyszerű "Helló, világ" függvényt tartalmazó Hello.py.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Ábra 4. Felhasználó által definiált függvény.

Ezután hozzunk létre egy fájlt, amely tartalmazza a Hello.py Hello.zip:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Ábra 5. Felhasználó által definiált Python kódot tartalmazó zip-fájl.

Töltse ki egy adatkészletet, az Azure gépi tanulási Studio. Hozzon létre, és futtassa a egy egyszerű kísérlet a Python kódot használó a Hello.zip fájlban a harmadik bemeneti port végrehajtása Python parancsfájl csatolásával az alábbi ábrán látható módon.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

6 ábrán. Felhasználó által definiált Python kóddal minta kísérlet feltöltött zip-fájlként.

A modul kimeneti jeleníti meg, hogy a zip-fájl csomagolatlan többször és a függvény `print_hello` valóban futtatni.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Ábra 7. Felhasználó által definiált függvényt használja a [Végrehajtás Python parancsfájl] belül[ execute-python-script] modulra.

## <a name="working-with-visualizations"></a>Megjelenítések használata
A felvétel a böngészőben, amely megjeleníthetők MatplotLib a készült visszaadható a [Python parancsfájl végrehajtása][execute-python-script]. A felvétel átirányítás nem történik meg automatikusan képeket azok a j használatakor, de Így a felhasználó kifejezetten mentse minden felvétel PNG fájlok vissza kell vissza az Azure gépi tanulási hagyják. 

Annak érdekében, hogy a képek létrehozása a MatplotLib, meg kell versenyt az alábbi lépésekkel:

* Váltás a kódmentes "AGG" az alapértelmezett Qt-alapú leképezőt 
* Hozzon létre egy új ábra objektumot 
* a tengely felvenni, és abba a felvételt minden készítése 
* az ábrán PNG fájl mentése 

Ez a folyamat menetét az az alábbi ábrán 8, amely a scatter_matrix függvénnyel Pandas pont ábra mátrixok hoz létre.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Ábra 8. Képek mentése MatplotLib értékeit.



Ábra 9 jeleníti meg, a második kimeneti port keresztül ábra egy kísérlet, amely a korábban már megjelenített való visszatéréshez parancsfájlt használ.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Ábra 9. Felvétel Python kódból generált megjelenítésére.

Lehetséges különböző képek az ott több ábrák térni, az Azure gépi tanulási futtatókörnyezet felveszi az összes kép megjelenítő elem összefűzésével őket.


## <a name="advanced-examples"></a>Speciális példák
A telepített Azure gépi tanulási Anaconda környezet közös csomagok tartalmaz, például NumPy, SciPy, és Scikits – ismerje meg, és ezek hatékony használható különböző adatkezelési feladatok egy tipikus gépi tanulási során. Példaként parancsprogramot és a következő kísérlet ensemble tanulók Scikits – ismerje meg, hogyan számítja ki a szolgáltatás sürgős értékek egy adatkészlet számára az használatát mutatja. Az eredmények használható felügyelt szolgáltatásainak kiválasztása egy másik gépi tanulási modellbe töltését mutató előtti végrehajtásához.

A fontosság értékek és a rendelés alapuló szolgáltatások számítja ki a Python függvény nézhet ki:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Ábra 10. A funkciót sorszám szolgáltatások értékek szerint.
 A következő kísérlet függvény ki, majd a sürgős eredmények funkciói az "Pima indiai cukorbetegség" adatkészlet az Azure gépi tanulási adja eredményül:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Ábra 11. Kísérletezés a Pima indiai cukorbetegség adatkészlet a sorrendet megadó funkcióit.

## <a name="limitations"></a>Korlátozások 
A [Végrehajtás Python parancsfájl] [ execute-python-script] által jelzett az alábbi korlátozások vonatkoznak:

1.  *A végrehajtás védőfal mögé helyezett.* A Python futtatókörnyezet jelenleg védőfal mögé helyezett, és emiatt nem engedélyezi a hozzáférést a hálózathoz vagy a helyi fájlrendszerben tartósan. Helyi meghajtóra mentett összes fájl elszigetelt és törölni a modul befejezése után. A Python kód nem tud hozzáférni a számítógépen, akkor fut, a kivétel, az aktuális könyvtárat és alkönyvtárait a legtöbb könyvtárak.
2.  *Összetett fejlesztés és a támogatási hibakeresési hiánya.* A Python modul jelenleg nem támogatja a IDE szolgáltatások, például az intellisense és hibakeresés. Is ha a modul nem sikerül futásidőben, a teljes Python Papírhalom követés érhető el, de kell jeleníthetők meg a kimeneti napló a modul. Jelenleg javasoljuk, hogy fejlesztése, és olyan környezetben, például IPython azok Python parancsfájlok hibakeresése és majd importálja a kódot a modul.
3.  *Keret kimeneti egyetlen adatokat.* A Python belépési pontjához való visszatéréshez ezt az eredményt adatok egyetlen keret csak engedélyezett. Még nem jelenleg meg lehet például közvetlenül a képzett modellek tetszőleges Python objektumok térjen vissza az Azure gépi tanulási futtatókörnyezet. Például az [R parancsfájl végrehajtása][execute-r-script], amelynek van, hogy az azonos korlátozást, érdemes azonban lehetőség sok esetben körte objektumok egy bájt tömb és majd visszatérés, hogy adatokat keret belsejében.
4.  A *meggátoló Python telepítés testreszabásához*. Jelenleg csak úgy lehet egyéni Python modulok hozzáadása a ismertetett zip-fájl szerkezet található. Ez a valósítható meg kis modulokat, azt is lehető a nagy modulok (különösen a natív DLL-ekkel) vagy a sok modulokat. 


##<a name="conclusions"></a>Következtetéseket
A [Végrehajtás Python parancsfájl] [ execute-python-script] modul lehetővé teszi, hogy egy adatok tudósok meglévő Python kódot felhőben tárolt gépi tanulási munkafolyamatok részévé az Azure gépi tanulási, és zökkenőmentesen üzemeltető webszolgáltatás részeként őket. A Python parancsfájl modul természetesen együttműködik az Azure gépi tanulási más modulok, és előtti feldolgozás, szolgáltatás kinyerésének kiértékelés és az eredmények utáni feldolgozásával adatok feltárása a tevékenységek egy cellatartomány használhatók. A végrehajtás használt kódmentes futtatókörnyezet Anaconda, egy jól tesztelése és elterjedt Python eloszlás alapján. Ez a következőket egyszerű meg fedélzeti meglévő kód eszközökhöz be a felhőben.

További funkciókat nyújtson a [Végrehajtás Python parancsfájl] megakadályozási[ execute-python-script] képzése és az Python levő üzemeltető és hatékonyabb támogatása a fejlesztés és Azure gépi tanulási Studio kód hibakeresése hozzáadása lehetővé teszi például modulra.

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Python Developer Center](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
