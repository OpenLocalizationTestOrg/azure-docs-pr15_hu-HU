<properties
   pageTitle="Hadoop oktatóprogram: első lépések a Windows Hadoop |} Microsoft Azure"
   description="Első lépések a Hadoop a hdinsight szolgáltatásból lehetőségre. Megtudhatja, hogy miként Hadoop fürt létrehozása a Windows, adatok struktúra lekérdezést futtat és elemzése az Excelben kimeneti."
   keywords="megtudhatja, hadoop-oktatóanyagban hadoop hadoop fürt, a windows rendszeren hadoop, struktúra lekérdezés"
   services="hdinsight"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/07/2016"
   ms.author="nitinme"/>


# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight-on-windows"></a>Hadoop oktatóprogram: Hadoop használata a Windows HDInsight első lépések

> [AZURE.SELECTOR]
- [Linux-alapú](../hdinsight-hadoop-linux-tutorial-get-started.md)
- [Windows-alapú](../hdinsight-hadoop-tutorial-get-started-windows.md)

Segít a Windows Hadoop megtudhatja, és kezdje el használni a HDInsight, ebből az oktatóanyagból megtudhatja, hogy miként struktúra lekérdezést futtat egy Hadoop fürt strukturálatlan adatokat, és kattintson a Microsoft Excel elemeznie.

>[AZURE.NOTE] A dokumentum adatai a Windows-alapú HDInsight fürt jellemző. Információ a Linux-alapú fürt [Hadoop oktatóprogram: használatba Linux-alapú Hadoop a HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

Tegyük fel, egy nagy strukturálatlan adathalmaz van, és a struktúra lekérdezést futtat, hogy néhány hasznos információkat kinyerni kívánt. Ez az pontosan mi fogja az ebben az oktatóanyagban. Az alábbiakban hogyan ennek elérése:

   !["Hadoop oktatóprogram: hozzon létre egy fiókot; Hozzon létre egy Hadoop fürt; küldje el a struktúra lekérdezést; adatok elemzése az Excelben.][image-hdi-getstarted-flow]

Ebből az oktatóanyagból megtudhatja, a HDInsight Hadoop a bemutató videó megtekintése:

![Videó egy első Hadoop az oktatóprogram: küldje el a struktúra lekérdezést a Hadoop fürthöz, eredményeinek és elemzése az Excelben.][img-hdi-getstarted-video]

**[Nézze meg a Hadoop oktatóprogram HDInsight-YouTube-on](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**

Microsoft Azure hdinsight szolgáltatáshoz a általános elérhetőségét együtt is tartalmaz HDInsight irányító az Azure- *Microsoft HDInsight Developer Preview*korábbi neve. A irányító Fejlesztőeszközök esetek észlelő, és csak akkor támogatja a telepítések egyetlen csomópontot. HDInsight irányító használatával kapcsolatos további tudnivalókért lásd: [Első lépések a HDInsight irányító][hdinsight-emulator].

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban Windows Hadoop-megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **A munkaállomáson** az Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 önálló verzió vagy Office 2010 Professional Plus.

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-hadoop-clusters"></a>Hadoop fürt létrehozása

Amikor létrehoz egy fürt, hozzon létre Azure számítási erőforrások Hadoop és kapcsolódó alkalmazások tartalmazó. Ebben a részben hozzon létre egy HDInsight-3,2 verzió fürthöz. Egyéb verzióihoz Hadoop fürt is létrehozhat. Útmutatásért lásd: az [egyéni beállításokkal létrehozása HDInsight fürt][hdinsight-provision]. HDInsight-verziók és azok SLA kapcsolatos további tudnivalókért lásd [HDInsight-összetevő verziószámozás](hdinsight-component-versioning.md).


**Hadoop fürt létrehozása**

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson az **Új**gombra, válassza az **Adatok analitika**, és válassza a **hdinsight szolgáltatásból lehetőségre**. A portál megnyílik egy **Új HDInsight fürt** lap.

    ![Az Azure-portálon új fürt létrehozása] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.1.png "Az Azure-portálon új fürt létrehozása")

3. Adja meg, és válasszon az alábbi:

    ![ENTER fürt név és típus] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.2.png "ENTER fürt név és típus")
    
  	|A mező neve| Érték|
  	|----------|------|
  	|Csoport neve| Egy egyedi nevet a fürt azonosítása|
  	|Fürt típusa| Ebben az oktatóanyagban **Hadoop** kijelölése |
  	|Operációs rendszer| Jelölje be **A Windows Server 2012 R2 adatközponthoz** ebben az oktatóanyagban.|
  	|HDInsight-verzió| Jelölje ki a legújabb verzióra az ebben az oktatóanyagban.|
  	|Előfizetés| Jelölje ki az Azure előfizetés a fürt használt.|
  	|Erőforráscsoport | Jelölje be a meglévő Azure erőforráscsoport, vagy hozzon létre egy új erőforráscsoport. Egy egyszerű HDInsight fürt fürt és annak az alapértelmezett tároló fiók tartalmazza.  Csoportosíthatja a két esetében könnyű kezelés egy erőforrás csoportba.|
  	|Hitelesítő adatok| Írja be a fürt bejelentkezési felhasználónév és jelszó. Windows-alapú fürt beállíthatja, hogy 2 felhasználói fiókok.  A fürt felhasználó (vagy a HTTP-felhasználó) a fürt kezeléséhez, és nyújtson be a feladatok szolgál.  Másik lehetőségként a távoli asztali (RDP) hozhat létre felhasználói fiókot távoli csatlakozzon a fürthöz. Ha úgy dönt, hogy engedélyezze a távoli asztal, a RDP felhasználói fiók létrehoznia.|
  	|Adatforrás| Kattintson az új alapértelmezett Azure tároló új fiók létrehozása gombra. A csoport nevét használja az alapértelmezett tároló nevet. Minden HDinsight fürt alapértelmezett Blob-tároló az Azure tároló szüntetnie van.  Hol található az alapértelmezett Azure tároló fiók határozza meg, hogy hol található a HDInsight fürt.|
  	|Réteg árak csomópont| 1 vagy 2 dolgozó csomópontok használata az alapértelmezett dolgozó csomópontot, és a címsor Megjegyzés réteg árak esetében ebben az oktatóanyagban.|
  	|Választható konfiguráció| Ebben a részben átugrása.|

9. Kattintson az **Új HDInsight fürt** lap győződjön meg arról, hogy a **PIN-kód Startboard** választógombot, és kattintson a **Létrehozás**gombra. Ezzel a fürt létrehozása és hozzáadása a mozaik, az Azure portálja Startboard. Az ikon jelzi, hogy a fürt hoz létre, és a HDInsight ikont jeleníti meg, létrehozási befejeződése után.

  	| Létrehozása során. | Teljes létrehozása |
  	| ------------------ | --------------------- |
  	| ![Mutató a startboard létrehozása](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioning.png) | ![A létrehozott fürt csempe](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioned.png) |

    > [AZURE.NOTE] Egy kis időt, a fürt hozható létre, általában körülbelül 15 percet fog tartani. Ellenőrizheti a létrehozási folyamat használata a Startboard vagy a lap bal szélén lévő **értesítések** tétel a csempére.

10. Ha befejeződött a létrehozását, kattintson a Startboard a fürt lap elindítani a fürt csempéje.


## <a name="run-a-hive-query-from-the-portal"></a>Struktúra lekérdezések futtatása a portálon
Most, hogy a létrehozott egy HDInsight fürthöz, következő lépésként a struktúra feladat struktúra mintatáblázat a lekérdezés futtatásához. *Hivesampletable*, amely megtalálható HDInsight fürt fogjuk használni. A tábla mobileszköz gyártók, rendszerek és modellek adatait tartalmazza. Az alábbi táblázat a struktúra lekérdezés az egy adott gyártó mobileszközökhöz adatokat ad vissza.

> [AZURE.NOTE] A .NET rendszerhez 2.5-ös vagy újabb verzió SDK csomagjában talál az Azure HDInsight Tools for Visual Studio megtalálható. Eszközeivel a Visual Studióban, HDInsight fürthöz csatlakozni, struktúra táblázatok létrehozása és struktúra lekérdezések futtatása. További tudnivalókért lásd: a [HDInsight Hadoop Tools for Visual Studio használatának első lépései][1].

**A struktúra feladat futtatása az fürt irányítópult**

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Az **Összes Tallózás** gombra, és válassza a **HDInsight fürt** fürt, beleértve a fürt az előző részben létrehozott listájának megtekintéséhez.
3. Kattintson a nevére, amelyet a struktúra feladat futtatásához a fürt, és válassza az **Irányítópult** elemre a lap tetején.
4. Weblapon egy másik böngésző lapon nyílik meg. Írja be a Hadoop felhasználói fiókot, és a jelszót. Az alapértelmezett felhasználóneve **felügyeleti**; a jelszó nem megadott a fürt létrehozásakor.
5. Az irányítópult lapon kattintson a **Szerkesztő struktúra** fülre. A következő lap megnyitása

    ![A HDInsight fürt irányítópulton Editor lapon a struktúra.][img-hdi-dashboard]

    Vannak olyan több lapot, kattintson a lap tetején. Az alapértelmezett lapot **Szerkesztő struktúra**, és a többi lapot **Korábbi** és a **Fájlt a böngészőben**. Irányítópultja használatával struktúra lekérdezéseket küldhet, Hadoop feladat naplókban és tárolt fájlok keresése.

    > [AZURE.NOTE] Megjegyzendő, hogy az URL-cím, a weblap * &lt;ClusterName&gt;. azurehdinsight.net*. Így nem kell megnyitnia az irányítópulton a portálról, megnyithatja az Irányítópulton egy webböngészőben az URL-cím használatával.

6. A **Struktúra Editor** lapon a **Lekérdezés nevét**, írja be a **HTC20**.  A lekérdezés neve a feladat címére. A lekérdezés ablaktáblában adja meg a struktúra lekérdezés, a képen látható módon:

    ![A lekérdezés ablaktáblában struktúra szerkesztő beírt lekérdezés struktúra.][img-hdi-dashboard-query-select]

4. Kattintson a **Küldés**gombra. Térjen vissza az eredményeket néhány percet vesz igénybe. A képernyő 30 másodperenként. **Frissítse** a képernyő frissítése is kattinthat.

    ![A képernyő alján az fürt irányítópult struktúra lekérdezés eredményei szerepel a listában.][img-hdi-dashboard-query-select-result]

5. Miután az Állapot oszlopban, hogy a feladat befejezése után, kattintson a lekérdezés nevére kattintva megtekintheti a kimenet képernyőn. Jegyezze fel a **Projekt indítása ideje (UTC) szerint**. Később lesz szüksége.

    ![Projekt kezdési idő HDInsight fürt irányítópult a korábbi lapon látható.][img-hdi-dashboard-query-select-result-output]

    A lapon látható a **Feladat kimeneti** és a **Feladat naplót**. Töltse le a kimeneti fájl lehetősége is van (\_stdout) és a naplófájl \(_stderr).


**Tallózással keresse meg a kimeneti fájl**

1. A fürt irányítópult lapon kattintson a **Fájl böngészőben**.
2. Kattintson a tárterület-fiókja nevére, kattintson a tároló nevét (Ez ugyanaz, mint a csoport neve), és válassza a **felhasználó**.
3. Kattintson a **rendszergazda** , és válassza a a globálisan egyedi azonosítója, amelynek a legutóbbi módosítás idejét (miután kissé korábban feljegyzett elindítása a feladatot). Másolja a vágólapra a globálisan egyedi azonosítója. Lesz szüksége a következő szakaszban.


    ![A struktúra lekérdezés a kimeneti fájl globálisan egyedi azonosítója szerepel a böngészőben a Fájl fülre.][img-hdi-dashboard-query-browse-output]


##<a name="connect-to-microsoft-business-intelligence-tools-for-excel"></a>Az Excelhez készült Microsoft üzletiintelligencia-eszközeinek csatlakoztatása

A Power Query bővítmény a Microsoft Excel használata az Excelben, ahol a Microsoft Üzletiintelligencia-eszközök segítségével ezek alapján további elemzésnek az eredmények a feladat eredménye importálása hdinsight szolgáltatásból lehetőségre.

Az Excel 2013-ban vagy 2010 ezt a részét az oktatóprogram elvégzéséhez telepítve kell rendelkeznie.

**A Microsoft Power Query az Excel letöltése**

- Töltse le a Microsoft Power Query a Microsoft Excel, a [Microsoft letöltőközpontból](http://www.microsoft.com/download/details.aspx?id=39379) , és telepítse.

**HDInsight-adatok importálása**

1. Nyissa meg az Excelt, és hozzon létre egy új munkafüzetet.
3. Kattintson a **Power Query** menüre, kattintson **A más forrásokból**, és válassza **A Azure hdinsight szolgáltatásból lehetőségre**.

    ![Az Excel PowerQuery importálása menüje Azure hdinsight szolgáltatásból lehetőségre.][image-hdi-gettingstarted-powerquery-importdata]

3. Írja be a **Fiók neve** a társított a fürt Azure Blob-tárolóhoz fiók, és kattintson **az OK**gombra. (Ez a a tárterület-fiókot az oktatóprogram során létrehozott.)
4. Írja be a **Fiókkulcs** Azure Blob-tárolóhoz fióknak, és kattintson a **Mentés**gombra.
5. A jobb oldali ablaktáblán kattintson duplán a blob nevére. Alapértelmezés szerint a blob neve megegyezik a csoport nevét.

6. Keresse meg a **név** oszlopban **stdout** . Győződjön meg arról, hogy a megfelelő **Mappa elérési útja** oszlopban a globálisan egyedi azonosítója megfelel-e az előbb másolt globálisan egyedi azonosítója. Egyező javasol, hogy az Ön által küldött feladat kimeneti adatai felel meg. Kattintson a **bináris** oszlop bal **stdout**.

    ![Az adatok kimenete globálisan egyedi azonosítója keresése a tartalom listában.][image-hdi-gettingstarted-powerquery-importdata2]

9. Kattintson a **Bezárás és betöltés** importálhatja az Excelbe kimeneti struktúra feladat bal felső sarokban.

##<a name="run-samples"></a>Példák futtatása

HDInsight fürt tartalmaz, amely tartalmaz egy első lépések dokumentumtárban közvetlenül a portálon minták futtatandó lekérdezés konzol. A minták segítségével megtudhatja, hogy miként HDInsight dolgozni útmutató néhány alapvető felhasználási helyzet alapján. Ezek a minták beépített minden szükséges összetevőjét, például az elemezni kívánt adatokat és a lekérdezések az adatokat a futtatására. Többet szeretne tudni a minták az első lépések gyűjteményben, lásd: [Hadoop ismerje meg, a az első lépések HDInsight gyűjtemény segítségével hdinsight szolgáltatásból lehetőségre](hdinsight-learn-hadoop-use-sample-gallery.md).

**A minta futtatása**

1. Kattintson az imént létrehozott fürt csempéje az Azure-portálon startboard.
 
2. Kattintson az új fürt lap **Irányítópult**. Amikor a rendszer kéri, írja be a rendszergazdai felhasználónevével és jelszavával a fürt.

    ![Indítsa el a fürt irányítópult] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.Cluster.Dashboard.png "Indítsa el a fürt irányítópult")
 
3. A megnyíló weblapról kattintson az **Első lépések gyűjtemény** fülre, és a **megoldások a mintaadatokat tartalmazó** kategóriában kattintson a futtatni kívánt minta. Az weblapon a minta befejezéséhez kövesse az utasításokat. Az alábbi táblázat néhány minták sorolja fel, és milyen minden egyes minta tartalmaz további információt tartalmaz.

Minta | Mire szolgál ez?
------ | ---------------
[Érzékelő adatelemzés][hdinsight-sensor-data-sample] | Megtudhatja, hogy miként készítheti el fűtési, szellőztetési és légkondicionáló (fűtés-és Légtechnikai) rendszerek, amelyek nem biztos, hogy a beállított hőmérséklet fenntartására képes rendszerek azonosításához által létrehozott korábbi adatfeldolgozás HDInsight használatával.
[Webhely napló elemzése][hdinsight-weblogs-sample] | Megtudhatja, hogy miként HDInsight segítségével elemzése a webhely-naplófájlok első, illetve a gyakoriság regisztráltak a webhelyen található külső webhelyekről a napon és webhely hibák, amelyek a felhasználói felület összefoglalását.
[Twitter trend-elemzés](hdinsight-analyze-twitter-data.md) | Megtudhatja, hogy miként trendek elemzése az Twitter HDInsight használatával.

##<a name="delete-the-cluster"></a>A csoport törlése

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Következő lépések
Az oktatóprogram Hadoop Hadoop fürt létrehozása a Windows az adatokat, és az eredmények az Excelben, ahol el további feldolgozott és üzletiintelligencia-eszközeinek grafikusan megjelenített importálása struktúra lekérdezést futtat, HDInsight megtanulta azt. További tudnivalókért lásd: az alábbi oktatóanyagok:

- [Használatba HDInsight Hadoop Tools for Visual Studio][1]
- [A HDInsight irányító – első lépések][hdinsight-emulator]
- [Azure Blob-tárolóhoz használata hdinsight szolgáltatáshoz][hdinsight-storage]
- [A PowerShell használatával HDInsight felügyelete][hdinsight-admin-powershell]
- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
- [HDInsight MapReduce használata][hdinsight-use-mapreduce]
- [HDInsight struktúra használata][hdinsight-use-hive]
- [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
- [HDInsight Oozie használata][hdinsight-use-oozie]
- [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz][hdinsight-develop-mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-versions]: hdinsight-component-versioning.md


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-emulator]: hdinsight-hadoop-emulator-get-started.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hadoop-hdinsight-intro]: hdinsight-hadoop-introduction.md
[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#step-1-install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png
 
