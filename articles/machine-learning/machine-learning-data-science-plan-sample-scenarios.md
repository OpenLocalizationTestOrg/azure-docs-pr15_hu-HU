<properties
    pageTitle="A fejlett analitikai forgatókönyvek folyamat és technológia Azure gépi tanulási |} Microsoft Azure"
    description="Jelölje ki a megfelelő forgatókönyvek ennek a csapat adatok tudományos művelet prediktív analytics speciális."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na" 
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Az Azure gépi tanulási fejlett analitikai módszerei

Ez a cikk ismerteti a különféle példaadatforrások és kezelhetik a [Csapat adatok tudományos folyamat (TDSP)](data-science-process-overview.md)cél forgatókönyvet. A TDSP biztosít az intelligens alkalmazások létrehozásába kapcsolatos együttműködésre lehetőséget nyújt a csoportoknak a rendszeres megközelítés. Az alábbi bemutatott forgatókönyvek elérhető beállítások a adatfeldolgozás munkafolyamatban az adatok jellemzői, a forrás helyek és a cél tárházakban Azure-ban függő szemléltetése

A **döntési fa** kijelölése az minta jelenik meg, amely megfelel az adatokat, és a cél az utolsó szakasz jelenik meg.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Egy példa bemutatja a következő szakaszok. A minden esetben egy lehetséges adatok tudományos vagy fejlett analitikai folyamat és Azure erőforrások támogató sorolja fel.

>[AZURE.NOTE]**Összes az alábbi helyzetekben a kell:**
><br/>
>* [Tárterület-fiók létrehozása](../storage/storage-create-storage-account.md)
><br/>
>* [Azure gépi tanulási-munkaterület létrehozása](machine-learning-create-workspace.md)


## <a name="smalllocal"></a>Forgatókönyv \#1: kicsi, közepes táblázatos adatkészlet egy helyi fájlok

![Kicsi, közepes helyi fájlok][1]

#### <a name="additional-azure-resources-none"></a>Azure további források: nincs

1.  Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

2.  Egy adatkészlet feltöltéséhez.

3.  Hozzon létre egy Azure gépi tanulási kísérlet folyamat feltöltött adathalmaz kezdve.

## <a name="smalllocalprocess"></a>Forgatókönyv \#2: kicsi, közepes adatkészlet feldolgozás igénylő helyi fájlok

![Kicsi, közepes helyi fájlok feldolgozása][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure további források: Azure virtuális gép (IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gépen futó IPython jegyzetfüzetet.

2.  Az Azure tároló tároló feltölteni az adatokat.

3.  Előre folyamat, és az adatok eléréséhez Azure tároló tárolóból IPython jegyzetfüzetben adatok letisztázásának.

4.  Művelet, táblázatos adatok átalakítása.

5.  Azure BLOB-transzformált adatok mentése

6.  Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

7.  Az [Adatok importálása] használata Azure BLOB lévő adatok beolvasása[ import-data] modulra.

8. Hozzon létre egy Azure gépi tanulási kísérlet folyamat j.leny adathalmaz kezdve.

## <a name="largelocal"></a>Forgatókönyv \#3: nagy adatkészlet a helyi fájlok Azure BLOB célba juttatása

![Nagyméretű helyi fájlok][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Azure további források: Azure virtuális gép (IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gépen futó IPython jegyzetfüzetet.

2.  Az Azure tároló tároló feltölteni az adatokat.

3.  Előre folyamat, és az adatok elérése az Azure BLOB IPython jegyzetfüzetben adatok letisztázásának.

4.  Adatok művelet, táblázatos formátumban, átalakítás, szükség esetén.

5.  Adatok feltárása és funkciók létrehozása szükség szerint.

6.  Bontsa ki a kis és közepes adatok minta.

7.  Azure BLOB mentse a mintában szereplő adatokat.

8. Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

9. Az [Adatok importálása] használata Azure BLOB lévő adatok beolvasása[ import-data] modulra.

10. Azure gépi tanulási kísérlet folyamat j.leny adathalmaz kezdődő összeállítása.


## <a name="smalllocaltodb"></a>Forgatókönyv \#4: kicsi, közepes adatkészlet helyi fájlokat, SQL Server-Azure Virtal gépen célba juttatása

![Kicsi, közepes helyi fájlokat az Azure SQL-adatbázis][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure további források: Azure virtuális gép (SQL Server / IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gépen futó SQL Server + IPython jegyzetfüzet elemre.

2.  Az Azure tároló tároló feltölteni az adatokat.

3.  Előre folyamat, és IPython jegyzetfüzetet használó Azure tárterület-tárolóban lévő adatok letisztázásának.

4.  Adatok művelet, táblázatos formátumban, átalakítás, szükség esetén.

5.  Adatok mentése virtuális helyi fájlok (IPython jegyzetfüzet virtuális fut, helyi lévő virtuális meghajtók vonatkoznak).

6.  Az Azure virtuális futó SQL Server-adatbázishoz adat betöltése.

    A beállítás \#1: SQL Server Management Studio segítségével.

    - Jelentkezzen be az SQL Server virtuális
    - Futtassa az SQL Server Management Studio eszközben.
    - Adatbázis- és célwebhelyek táblázatok létrehozása.
    - Használja a tömeges módszerek az adatok betöltése a virtuális helyi fájlokból importálni.

    A beállítás \#2: segítségével IPython jegyzetfüzet – közepes és nagy adatkészletek nem tanácsos<!-- -->    
    - SQL Server virtuális eléréséhez használja az ODBC-kapcsolati karakterláncot.
    - Adatbázis- és célwebhelyek táblázatok létrehozása.
    - Használja a tömeges módszerek az adatok betöltése a virtuális helyi fájlokból importálni.

7.  Adatok feltárása, funkciók létrehozása szükség szerint. Fontos tudni, hogy a szolgáltatások nem lehet eseményre az adatbázis-táblázatokban. Megjegyzés: csak a a szükséges lekérdezést hozhat létre őket.

8. Ha szükséges, illetve a kívánt dönt egy adatok minta mérete.

9. Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

10. Olvassa el az adatokat közvetlenül az SQL Server használatával az [Adatok importálása] a[ import-data] modulra. Illessze be a szükséges lekérdezést, mely mezők kibontása, szolgáltatások hoz létre, és minták adatok, ha szükséges, közvetlenül az [Adatok importálása] [ import-data] lekérdezést.

11. Azure gépi tanulási kísérlet folyamat j.leny adathalmaz kezdődő összeállítása.

## <a name="largelocaltodb"></a>Forgatókönyv \#5: nagy adatkészlet helyi fájlok, az SQL Server Azure virtuális céltábladiagram

![Nagyméretű fájlok helyi az Azure SQL-adatbázis][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure további források: Azure virtuális gép (SQL Server / IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gépen futó SQL Server IPython jegyzetfüzet kiszolgáló.

2.  Az Azure tároló tároló feltölteni az adatokat.

3.  (Nem kötelező) Előre folyamat, és az adatok letisztázásának.

    egy.  Előre folyamat, és az adatok elérése az Azure BLOB IPython jegyzetfüzetben adatok letisztázásának.

    b.  Adatok művelet, táblázatos formátumban, átalakítás, szükség esetén.

    c billentyűkombinációt.  Adatok mentése virtuális helyi fájlok (IPython jegyzetfüzet virtuális fut, helyi lévő virtuális meghajtók vonatkoznak).

4.  Az Azure virtuális futó SQL Server-adatbázishoz adat betöltése.

    egy.  Jelentkezzen be az SQL Server virtuális.

    b.  Ha adatainak nincs már mentette, töltse le adatfájlok Azure tároló tároló helyi-virtuális mappába.

    c billentyűkombinációt.  Futtassa az SQL Server Management Studio eszközben.

    d.  Adatbázis- és célwebhelyek táblázatok létrehozása.

    e.  Használja a tömeges importálása módszereket, amelyekkel betöltheti az adatokat.

    f.  A JOIN szükség, ha illesztések felgyorsításához indexek létrehozása

     > [AZURE.NOTE] Gyorsabb betöltése a nagy méretű, az azt ajánljuk, hogy particionált táblák létrehozásakor és tömeges importálni az adatokat, párhuzamosan. További tudnivalókért lásd: az [SQL-partícionálású táblák párhuzamos adatok importálása](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Adatok feltárása, funkciók létrehozása szükség szerint. Fontos tudni, hogy a szolgáltatások nem lehet eseményre az adatbázis-táblázatokban. Megjegyzés: csak a a szükséges lekérdezést hozhat létre őket.

6.  Ha szükséges, illetve a kívánt dönt egy adatok minta mérete.

7.  Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

8. Olvassa el az adatokat közvetlenül az SQL Server használatával az [Adatok importálása] a[ import-data] modulra. Illessze be a szükséges lekérdezést, mely mezők kibontása, szolgáltatások hoz létre, és minták adatok, ha szükséges, közvetlenül az [Adatok importálása] [ import-data] lekérdezést.

9. Egyszerű Azure gépi tanulási kísérlet folyamat feltöltött adatkészlet kezdve

## <a name="largedbtodb"></a>Forgatókönyv \#6: nagy adatkészlet a egy SQL Server adatbázis helyszíni, SQL Server-Azure virtuális gép célba juttatása

![Nagy SQL DB helyszíni SQL DB Azure-ban][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure további források: Azure virtuális gép (SQL Server / IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gépen futó SQL Server IPython jegyzetfüzet kiszolgáló.

2.  Használja az adatok exportálása módszerek az adatok exportálása az SQL Server kiírása fájlokat.

    > [AZURE.NOTE] Ha úgy dönt, hogy az összes adat áthelyezése a helyszíni az adatbázis egy alternatív (gyorsabb) módszer a teljes adatbázis áthelyezése az SQL Server-példányt Azure-ban. Hagyja ki az adatok exportálása, adatbázis, hozzon létre és betöltés/adatimportálás céladatbázist, és kövesse az alternatív módszer a lépéseket.

3.  Azure tároló tároló kiírása fájlok feltöltése.

4.  Töltse be az adatokat az Azure virtuális gépen futó SQL Server-adatbázishoz.

    egy.  Jelentkezzen be az SQL Server virtuális.

    b.  Az Azure tároló tároló adatfájlok töltse le a helyi-virtuális mappába.

    c billentyűkombinációt.  Futtassa az SQL Server Management Studio eszközben.

    d.  Adatbázis- és célwebhelyek táblázatok létrehozása.

    e.  Használja a tömeges importálása módszereket, amelyekkel betöltheti az adatokat.

    f.  A JOIN szükség, ha illesztések felgyorsításához indexek létrehozása

    > [AZURE.NOTE] A nagy méretű, gyorsabb betöltése particionált táblázatok létrehozása és a tömeges párhuzamosan adatokat importál. További tudnivalókért lásd: az [SQL-partícionálású táblák párhuzamos adatok importálása](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Adatok feltárása, funkciók létrehozása szükség szerint. Fontos tudni, hogy a szolgáltatások nem lehet eseményre az adatbázis-táblázatokban. Megjegyzés: csak a a szükséges lekérdezést hozhat létre őket.

6.  Ha szükséges, illetve a kívánt dönt egy adatok minta mérete.

7.  Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

8. Olvassa el az adatokat közvetlenül az SQL Server használatával az [Adatok importálása] a[ import-data] modulra. Illessze be a szükséges lekérdezést, mely mezők kibontása, szolgáltatások hoz létre, és minták adatok, ha szükséges, közvetlenül az [Adatok importálása] [ import-data] lekérdezést.

9. Egyszerű Azure gépi tanulási kísérlet folyamat feltöltött adatkészlet kezdve.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternatív módszer a teljes adatbázis másolása egy helyszíni SQL Server Azure SQL-adatbázis

![Helyi adatbázis leválasztása és az Azure SQL-adatbázis csatolása][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Azure további források: Azure virtuális gép (SQL Server / IPython jegyzetfüzet server)

Való replikáció az SQL Server virtuális az egész SQL Server-adatbázishoz, másolja adatbázis egy helyen-kiszolgálóról egy másikra, feltéve, hogy az adatbázis tehetők ideiglenesen kapcsolat nélküli módban. Ehhez az SQL Server Management Studio objektum Explorer vagy az egyenértékű a Transact-SQL-parancsok használatával.

1. A forrás helyére az adatbázis leválasztása. További tudnivalókért olvassa el a [Leválasztás adatbázis](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx)című témakört.
2. A Windows Intéző vagy Windows parancssor ablakban másolja a leválasztott adatbázisfájl vagy a fájlok és a naplófájl vagy a fájl a célhely meg az SQL Server virtuális Azure-ban.
3. A másolt fájlok csatolása a cél SQL Server-példányt. További tudnivalókért olvassa el a [adatbázis csatolása](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx)című témakört.

[Ugrás egy adatbázis leválasztása és csatolása (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="largedbtohive"></a>Forgatókönyv \#7: a fájlok helyi nagy adatainak struktúra adatbázis Azure hdinsight szolgáltatáshoz Hadoop fürt céltábladiagram

![A helyi cél struktúra nagy adatok][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Azure további források: Azure hdinsight szolgáltatáshoz Hadoop fürt és Azure virtuális gép (IPython jegyzetfüzet server)

1.  Hozzon létre egy Azure virtuális gép IPython jegyzetfüzet Servert futtató.

2.  Hozzon létre egy Azure hdinsight szolgáltatáshoz Hadoop fürthöz.

3.  (Nem kötelező) Előre folyamat, és az adatok letisztázásának.

    egy.  Előre folyamat, és az adatok elérése az Azure BLOB IPython jegyzetfüzetben adatok letisztázásának.

    b.  Adatok művelet, táblázatos formátumban, átalakítás, szükség esetén.

    c billentyűkombinációt.  Adatok mentése virtuális helyi fájlok (IPython jegyzetfüzet virtuális fut, helyi lévő virtuális meghajtók vonatkoznak).

4.  Az alapértelmezett tároló 2 lépésben kiválasztott Hadoop-fürt feltölteni az adatokat.

5.  Azure hdinsight szolgáltatáshoz Hadoop fürt struktúra adatbázishoz adat betöltése.

    egy.  Jelentkezzen be a központi csomópontot a Hadoop fürt

    b.  Nyissa meg a Hadoop parancsot.

    c billentyűkombinációt.  Írja be a struktúra legfelső szintű könyvtár paranccsal `cd %hive_home%\bin` Hadoop parancssor.

    d.  Adatbázist és táblákat létrehozni a struktúra lekérdezések futtatása és adatok betöltése blob-tárolóhoz struktúra táblák.

    > [AZURE.NOTE] Ha az adatok nagy, felhasználók partíciót a struktúratáblával hozhat létre. Ezt követően a felhasználók használhatják a `for` hurok a Hadoop parancssori a központi csomópontra adat betöltése a struktúra táblázatba particionálva partíciót szerint.

6.  Adatok feltárása és funkciók létrehozása a Hadoop parancssori szükség szerint. Fontos tudni, hogy a szolgáltatások nem lehet eseményre az adatbázis-táblázatokban. Megjegyzés: csak a a szükséges lekérdezést hozhat létre őket.

    egy.  Jelentkezzen be a központi csomópontot a Hadoop fürt

    b.  Nyissa meg a Hadoop parancsot.

    c billentyűkombinációt.  Írja be a struktúra legfelső szintű könyvtár paranccsal `cd %hive_home%\bin` Hadoop parancssor.

    d.  A Hadoop parancssori az a struktúra lekérdezések futtatása a az adatok feltárása és funkciók létrehozása szükség szerint a Hadoop fürt központi csomópontra.

7.  Ha szükséges, illetve a kívánt számú Azure gépi tanulási Studio illesztése az adatokat.

8.  Jelentkezzen be az [Azure gépi tanulási Studio](https://studio.azureml.net/).

9. Olvassa el az adatokat közvetlenül a `Hive Queries` a az [Adatok importálása] [ import-data] modulra. Illessze be a szükséges lekérdezést, mely mezők kibontása, szolgáltatások hoz létre, és minták adatok, ha szükséges, közvetlenül az [Adatok importálása] [ import-data] lekérdezést.

10. Egyszerű Azure gépi tanulási kísérlet folyamat feltöltött adatkészlet kezdve.

## <a name="decisiontree"></a>Döntési fa forgatókönyv kijelölése
------------------------

Az alábbi diagram összefoglalja a fenti esetek és a speciális Analytics folyamat és technológia lehetőségek arról, hogy minden tételes esetet tárgyal kattintva juthat. Látható, hogy az adatkezelési, feltárás, szolgáltatás tervezés és mintavételnél eltarthat egy vagy több módszer/környezetben – a haladó, forrás és/vagy a Céllista környezetben – helyezze, és ismételt szükség szerint folytathatja. A diagram csak néhány lehetséges flow ábrája szolgál, és nem adja meg egy teljes körű számbavétel.

![Minta DS folyamat áttekintése a következő felhasználási területei][8]

### <a name="advanced-analytics-in-action-examples"></a>Speciális Analytics példák működésben

A speciális Analytics folyamat és technológia használata nyilvános adatforrást alkalmaznia Azure gépi tanulási forgatókönyvek végpont a következő cikkekben talál:


* [Csapat adatok tudományos folyamat működését: az SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).
* [Csapat adatok tudományos folyamat működését: HDInsight Hadoop fürt használatával](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
