<properties
   pageTitle="Az Excel csatlakozni a struktúra ODBC-illesztőprogram Hadoop |} Microsoft Azure"
   description="Megtudhatja, hogy miként állíthatja be, és a Microsoft-struktúra ODBC-illesztő lekérdezés adatok HDInsight fürt az Excel használatával."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Az Excel hadoop kapcsolatba a Microsoft-struktúra ODBC-illesztőprogram

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

A Microsoft nagy adatok megoldás az Azure hdinsight szolgáltatáshoz által telepített Apache Hadoop fürt integrálódik a Microsoft Business Üzletiintelligencia-összetevők. Példa az együttműködés az azt jelenti, hogy a struktúra adatraktár Hadoop fürt HDInsight a Microsoft struktúra Open Database Connectivity (ODBC) illesztőprogram használata az Excel csatlakozni.

Akkor is az adatokat egy HDInsight fürthöz és más adatforrásokhoz, beleértve a más (nem HDInsight) Hadoop fürt, az Excel alkalmazásból, az Excel a Microsoft Power Query bővítmény használatával kapcsolódó csatlakozni. Információt telepítésével és a Power Query használatával című témakörben talál [Hdinsight szolgáltatáshoz a Power Query az Excel csatlakozni][hdinsight-power-query].

> [AZURE.NOTE] Ez a cikk lépéseinek Linux vagy Windows-alapú HDInsight fürt kínál, miközben Windows az ügyfél-munkaállomás szükség.

**Előfeltételek**:

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Egy HDInsight fürt**. [Első lépések az Azure hdinsight szolgáltatáshoz]lásd hozzon létre egyet,[hdinsight-get-started].
- **A munkaállomás** az Office 2013 Professional Plus, Office 365 Pro Plus, az Excel 2013 önálló verzió vagy Office 2010 Professional Plus


##<a name="install-microsoft-hive-odbc-driver"></a>Telepítse a Microsoft-struktúra ODBC-illesztő

Töltse le és telepítse a Microsoft struktúra ODBC-illesztőprogram a [Letöltőközpontból][hive-odbc-driver-download].

Ez az illesztőprogram is telepítve van a Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 vagy Windows Server 2012 32 bites vagy 64 bites verziója, és lehetővé teszi a kapcsolat Azure hdinsight szolgáltatáshoz (1,6 vagy újabb verzió) és Azure hdinsight szolgáltatáshoz irányító (v.1.0.0.0 és újabb verzióiban). Telepíteni kell a verzió megegyezzen a az alkalmazás, ahol az ODBC-illesztőprogram használni fog. Ebben az oktatóprogramban az illesztőprogram az Office Excel lesz használható.

##<a name="create-hive-odbc-data-source"></a>A struktúra ODBC-adatforrás létrehozása

Az alábbi lépéseket a struktúra az ODBC adatforrás létrehozásának módját mutatják.

1. A Windows 8 vagy Windows 10-es nyissa meg a kezdőképernyőn a Windows billentyűt, és írja be a **adatforrások**gombra.
2. Kattintson a **források (32 bites) ODBC-adatok beállítása** vagy **ODBC-adatforrásokból (64 bites) beállítása** az Office verziójától függően. Ha Windows 7 használata esetén **Felügyeleti eszközök**válasszon **ODBC-adatforrásokból (32 bites)** vagy **ODBC-adatforrásokból (64 bites)** . Ez elindítja az **ODBC adatforrás-felügyelő** párbeszédpanel.

    ![Adatforrás-felügyelő OBDC][img-hdi-simbahiveodbc-datasource-admin]

3. Felhasználói DNS-ben kattintson a **Hozzáadás gombra** kattintva nyissa meg az **Új adatforrás létrehozása** varázsló elemre.
4. Jelölje ki a **Microsoft struktúra ODBC-illesztőprogram**, és kattintson a **Befejezés gombra**. **A Microsoft struktúra ODBC illesztőprogram DNS az Oldalbeállítás** párbeszédpanel fog elindulni.

5. Írja be vagy válassza ki a következő értékeket:

    A tulajdonság|Leírás
    ---|---
    Adatforrás neve szerint.|Adjon nevet az adatforrás
    A Host|Adja meg &lt;HDInsightClusterName >. azurehdinsight.net. Ha például myHDICluster.azurehdinsight.net
    Port|<strong>443-as</strong>használja. (Ez a port módosítását követően 563 a 443-as.)
    Adatbázis|Használja az <strong>alapértelmezett</strong>.
    Kiszolgálótípus struktúra|Jelölje be a <strong>kiszolgáló 2 struktúra</strong>
    Mechanizmusa|Jelölje ki az <strong>Azure HDInsight-szolgáltatás</strong>
    HTTP elérési út|Hagyja üresen.
    Felhasználónév|Írja be a HDInsight fürt felhasználó felhasználónév. Ez a felhasználónév a fürt rendelkezést folyamat során létre. A gyors létrehozása beállítást használta, az alapértelmezett felhasználónév esetén <strong>rendszergazda</strong>.
    Jelszó|Írja be a HDInsight fürt felhasználó jelszavát.
    </table>

    Létezik néhány fontos paraméter lényeges esetén kattintson a **Speciális beállítások**:

    Paraméter|Leírás
    ---|---
    Natív lekérdezés használata|Ha be van jelölve, az ODBC-illesztőprogram nem megpróbálja TSQL konvertálása HiveQL. Csak akkor, ha Ön a 100 %-os arról, hogy csak HiveQL kimutatások küld alkalmazzák. Amikor csatlakozik az SQL Server vagy Azure SQL-adatbázissal, akkor hagyja bejelölve.
    Sorok letiltása egy beolvasása|Rekordok nagy mennyiségű beolvasása, ha a paraméter finombeállítása írhatnak optimális teljesítmény biztosítása érdekében.
    Alapértelmezett karakterlánchossz oszlopban, a bináris oszlopot hossza, a decimális oszlopban a skála|Az adattípus hossza és hatással lehet szükséges, hogy adatokat ad vissza. Helytelen információkat és/vagy a pontosság csonkított elvesztése miatt vissza őket miatt.


    ![Speciális beállítások][img-HiveOdbc-DataSource-AdvancedOptions]

6. Kattintson a **tesztelje** az adatforrás tesztelése gombra. Az adatforrás megfelelően van konfigurálva, amikor látható *vizsgálatok sikeresen befejeződött!*.
7. Kattintson az **OK gombra** a tesztelése párbeszédpanel bezárásához. Az új adatforrás célszerű most az **ODBC adatforrás-felügyelő**lesz látható.
8. Kattintson **az OK gombra** kattintva zárja be a varázslót.

##<a name="import-data-into-excel-from-hdinsight"></a>Adatok importálása az Excel hdinsight

Az alábbi lépéseket a úgy egy struktúratáblával adatok importálása Excel-munkafüzet az ODBC adatforrás a fenti lépések létrehozott ismertetik.

1. Új vagy meglévő munkafüzet megnyitása az Excel programban.
2. Az **adatok** lapon kattintson **Az egyéb adatok adatforrásból**, és kattintson **Az Adatkapcsolat varázsló** az **Adatkapcsolat varázsló**indításához.

    ![Nyissa meg az Adatkapcsolat varázsló][img-hdi-simbahiveodbc.excel.dataconnection]

3. Jelölje ki a **ODBC DSN** adatforrásként, és kattintson a **Tovább gombra**.
4. ODBC-adatforrásokból jelölje be az adatforrás neve az előző lépésben létrehozott, és kattintson a **Tovább gombra**.
5. Írja be újra a jelszót a fürt varázsló, és szükség esetén kattintson a **vizsgálat** ellenőrizze még egyszer beállításait.
6. Kattintson az **OK gombra** a tesztelése párbeszédpanel bezárásához.
7. Kattintson az **OK gombra**. Várja meg az **adatbázis és tábla kijelölése** párbeszédpanel megnyitásához. Ez a is néhány másodpercet igénybe vehet.
8. Jelölje ki a táblázatot, amely az importálni kívánt, és kattintson a **Tovább**gombra. A *hivesampletable* egy minta struktúratáblával HDInsight fürt épített.  Ha még nem hozott létre egyet választhat. További információt a Futtatás struktúra lekérdezések és struktúra táblázatok létrehozása című témakörben [Használata a HDInsight struktúra][hdinsight-use-hive].
8. Kattintson a **Befejezés gombra**.
9. Az **Adatimportálás** párbeszédpanelen módosítsa, vagy adja meg a lekérdezést. Ehhez kattintson a **Tulajdonságok**parancsra. Ez a is néhány másodpercet igénybe vehet.
10. Kattintson a **definíció** fülre, és kattintson a struktúra select utasítás a **szöveg parancs** rendelés_részletei.mennyiség fűzze hozzá **korlát 200** . A módosítás korlátozza a visszaadott rekordkészlet 200.

    ![Kapcsolat tulajdonságai][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Kattintson az **OK gombra** a kapcsolat tulajdonságai párbeszédpanel bezárásához.
12. Kattintson **az OK gombra** kattintva zárja be az **Adatimportálás** párbeszédpanelen.  
13. Írja be újra a jelszót, és kattintson **az OK**gombra. Adatok importálása az Excel kap előtt néhány másodpercet vesz igénybe.

##<a name="next-steps"></a>Következő lépések

Ebben a cikkben szüksége a tanultakhoz használatáról a Microsoft-struktúra ODBC-illesztőprogram adatok beolvasásához a HDInsight szolgáltatásból az Excelbe. Hasonlóképpen meghallgathatja adatok HDInsight szolgáltatásból az SQL-adatbázisba. Akkor is feltölteni az adatokat egy HDInsight helyezését. További tudnivalókért lásd:

- [Adatelemzés nézetbeli késleltetés segítségével hdinsight szolgáltatáshoz][hdinsight-analyze-flight-data]
- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
- [HDInsight Sqoop használata] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
