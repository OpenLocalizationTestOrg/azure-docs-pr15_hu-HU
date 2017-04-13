<properties
   pageTitle="Adatok betöltése az SQL Server Azure SQL adatok raktári (SSIS) be |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy SQL Server Integration Services (SSIS) csomag számos különböző adatforrásból származó adatok áthelyezése SQL adatraktár."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Adatok betöltése az SQL Server Azure SQL adatok raktári (SSIS) be

> [AZURE.SELECTOR]
- [AZ SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Adatok betöltése az SQL Server Azure SQL-adatraktár az SQL Server Integration Services (SSIS) csomag létrehozása. Másik lehetőségként átalakítása, átalakítási és tisztítható az adatokat, mint az SSIS-adatfolyam áthaladt rajta.

Ebben az oktatóanyagban lesz:

- A Visual Studióban, hozzon létre egy új SSIS-projektet.
- Csatlakozás adatforrásokhoz, beleértve az SQL Server-(forrás) és az SQL adatraktár (célként).
- Az SSIS-csomagok betöltő adatok a forrásból származó be a cél tervezése.
- Futtassa az SSIS-csomag töltse be az adatokat.

Ebben az oktatóanyagban SQL Server használja, mint az adatforráshoz. SQL Server sikerült fut, a helyszíni, vagy az Azure virtuális gépen.

## <a name="basic-concepts"></a>Alapfogalmak

A csomag módosítása munkamennyiség az SSIS. Kapcsolódó csomagok projektekben vannak csoportosítva. Az SQL Server Data Tools Visual Studio projektek és tervezés csomagok hoz létre. A tervezési folyamat, vizuális, amelyben, húzása összetevők az eszköztárról a tervezési felületre, kösse össze őket, és tulajdonságaik beállításához. Miután végzett a csomagot, tetszés szerint telepítheti azt az SQL Server teljes kezelése, a figyelés és a biztonsági.

## <a name="options-for-loading-data-with-ssis"></a>Az SSIS adatok betöltése lehetőségei

SQL Server Integration Services (SSIS) rugalmas eszközök csoportját, amelybe csatlakozik, és az SQL adatraktár adatok betöltése több módon is ellenőrizhető.

1. Az ADO nettó cél használata SQL adatraktár. Ebben az oktatóprogramban egy ADO nettó cél használ, mert a legkevesebb beállítási lehetőségek.
2. Az OLE DB cél használata SQL adatraktár. Ez a beállítás rendelkezhetnek kissé jobb teljesítményt, mint a ADO nettó célját.
3. Az Azure Blob-feltöltése tevékenység segítségével az Azure Blob-tárolóban lévő adatok szakasz. Az SSIS SQL végrehajtása tevékenység használatával indítsa el az adatokat az SQL adatraktár betöltő Polybase parancsfájl. Ezt a lehetőséget biztosít a legjobb teljesítményt az itt felsorolt három lehetőség közül választhat. Úgy juthat az Azure Blob-feltöltése tevékenység, töltse le a [Microsoft SQL Server 2016 Integration Services funkciócsomag az Azure][]. Ha többet szeretne tudni Polybase, a [PolyBase][]útmutatójában.

## <a name="before-you-start"></a>Előzetes teendők

Ebben az oktatóanyagban lépéseinek, van szükség:

1. **SQL Server Integration Services (SSIS)**. Az SSIS SQL Server összetevője és próbaverziója vagy az SQL Server licenccel rendelkező verziója szükséges. Az SQL Server 2016 előzetes verzió próbaverziója című témakörben kaphat [Az SQL Server értékelést][].
2. **Visual Studio**. Az ingyenes Visual Studio 2015 közösségi Edition című témakörben kaphat [A Visual Studio közösségi][].
3. **Az SQL Server Data Tools for Visual Studio (SSDT)**. Az SQL Server Data Tools for Visual Studio 2015 című témakörben kaphat [Letöltése az SQL Server Data Tools (SSDT)][].
4. **Mintaadatok**. Ebben az oktatóanyagban használja a mintaadatokat az SQL Serveren tárolt a AdventureWorks adatbázisban forrásadatait kell SQL adatraktár betölteni. A AdventureWorks adatbázisban című témakörben kaphat [AdventureWorks 2014-es minta adatbázisok][].
5. **A SQL adatraktár adatbázis és az engedélyeket**. Ebben az oktatóprogramban egy SQL adatraktár példány csatlakozik, és betölti az adatokat a mikrofonba. Táblázat létrehozása és adatok betöltése engedélyek kell rendelkeznie.
6. **A tűzfal szabályok**. Tűzfal szabály létrehozása a SQL adatraktár az IP-címet a helyi számítógép előtt adatokat is feltölthet a SQL adatraktár van.

## <a name="step-1-create-a-new-integration-services-project"></a>Lépés: 1: Hozzon létre egy új SSIS-projektet.

1. Indítsa el a Visual Studio 2015.
2. Kattintson a **fájl** menü **Új kijelölése |} Projekt**.
3. Nyissa meg azt a **telepített |} Sablonok |} Üzleti intelligenciával kapcsolatos funkciók |} Integrációs szolgáltatások** projekttípusok.
4. Jelölje ki az **Integration Services Project**. Adjon meg értéket a **helyét**és **nevét** , és kattintson **az OK**gombra.

Visual Studio megnyílik, és létrehoz egy új Integration Services (SSIS)-projektet. Kattintson a Visual Studio nyílik meg a tervező a egyetlen új SSIS-csomag (Package.dtsx) a projekt. A következő képernyőn területek jelenik meg:

- A bal oldali, az **eszközkészlet lapon** az SSIS-összetevők.
- A középső, a tervezési felületre több fül. A szokásos használhatja legalább **Control Flow** és a **Data Flow** lapot.
- Kattintson a jobb oldalán, a **Megoldást Explorer** és a **Tulajdonságok** ablaktábla.

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Lépés: 2: Az alapszintű adatfolyam létrehozása

1. Adatok Flow tevékenység húzása az eszköztárról a tervezési felületre közepén (a **Control Flow** lapon).

    ![][02]

2. Kattintson duplán az adatok Flow tevékenység kattintson a Data Flow fülre.
3. A más forrásokból listából az eszközkészlet húzza a tervezési felületre egy ADO.NET forrás. A kijelölt adatforrás adaptert, és módosítsa a nevét az **SQL Server adatforrás** a **Tulajdonságok** ablaktáblában.
4. A más helyre listából az eszközkészlet húzza az ADO.NET cél a tervezési felületre ADO.NET forrás alatt. A cél kártya legyen kijelölve módosítsa a nevét az **SQL-DW cél** a **Tulajdonságok** ablaktáblában.

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Lépés 3: Állítsa be a forrás kártya

1. Kattintson duplán kattintva nyissa meg a **ADO.NET-Forrásszerkesztő**a forrás adaptert.

    ![][03]

2. A **ADO.NET Forrásszerkesztő** **Kapcsolatkezelő** fülre kattintson a **ADO.NET kapcsolatkezelő** listát a **ADO.NET kapcsolatkezelő beállítása** párbeszédpanel megnyitásához, és hozhat létre a kapcsolatbeállításokat az SQL Server-adatbázishoz, amelyből ebben az oktatóanyagban betölti az adatokat az **Új** gombra.

    ![][04]

3. **ADO.NET kapcsolatkezelő beállítása** párbeszédpanelen kattintson az **Új** gombra, nyissa meg a **Kapcsolatkezelő** párbeszédpanelt, és hozzon létre egy új adatkapcsolat.

    ![][05]

4. **Kapcsolatkezelő** párbeszédpanelen hajtsa végre a következő műveleteket.

    1. **Szolgáltató**lehetőségnél jelölje be a SqlClient adatszolgáltató.
    2. A **kiszolgáló nevét**adja meg az SQL Server nevét.
    3. **Jelentkezzen be a kiszolgáló** szakaszban jelölje ki vagy adja meg a hitelesítési adatait.
    4. **Kapcsolódás adatbázis** csoportjában jelölje be a AdventureWorks adatbázisban.
    5. **A kapcsolat tesztelése**gombra.
    
        ![][06]
    
    6. Amely a kapcsolat tesztelése eredményeinek jelentések párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **Kapcsolatkezelő** párbeszédpanel.
    7. **Kapcsolatkezelő** párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **ADO.NET kapcsolatkezelő beállítása** párbeszédpanel.
 
5. **ADO.NET kapcsolatkezelő beállítása** párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **ADO.NET Forrásszerkesztő**.
6. **ADO.NET-Forrásszerkesztő**a **a táblázat vagy a nézet neve** listában jelölje ki a **Sales.SalesOrderDetail** táblázatot.

    ![][07]

7. Kattintson a **betekintő** gombra a forrástáblához a **Lekérdezési eredmény megjelenítése** párbeszédpanel az első 200 adatsorok.

    ![][08]

8. A **Lekérdezési eredmény megjelenítése** párbeszédpanelen kattintson a **bezárása gombra** kattintva visszatérhet a **ADO.NET Forrásszerkesztő**.
9. **ADO.NET Forrásszerkesztő**kattintson **az OK gombra** az adatforrás konfigurálása befejezéséhez.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Lépés: 4: A forrás kártya csatlakoztatása a cél kártya

1. Jelölje ki az adatforrás-adaptert, a tervezési területen.
2. Jelölje be a forrás kártyán nyúló kék nyílra, és húzza a cél-szerkesztő amíg nem simul tetszőleges helyére.

    ![][10]

    Egy tipikus az SSIS package használatával az SSIS-eszköztárról legyen a forrás- és a más összetevő számos átalakítása, átalakítás és az adatok tisztítható, mint az SSIS-adatfolyam áthaladt rajta. Ahhoz, hogy ez a példa egyszerűen, lehetséges, hogy kapcsolódik a forrás közvetlenül a cél.

## <a name="step-5-configure-the-destination-adapter"></a>5 lépés: Állítsa be a cél kártya

1. Kattintson duplán a cél adaptert a **ADO.NET cél szerkesztő**megnyitása.

    ![][11]

2. A **ADO.NET cél szerkesztő** **Kapcsolatkezelő** fülre kattintson az **Új** gomb melletti a **Kapcsolatkezelő** listában a **ADO.NET kapcsolatkezelő beállítása** párbeszédpanel megnyitása és létrehozása az Azure SQL-adatraktár adatbázist, amelybe ebben az oktatóanyagban betölti az adatokat kapcsolatbeállításainak.
3. **ADO.NET kapcsolatkezelő beállítása** párbeszédpanelen kattintson az **Új** gombra, nyissa meg a **Kapcsolatkezelő** párbeszédpanelt, és hozzon létre egy új adatkapcsolat.
4. **Kapcsolatkezelő** párbeszédpanelen hajtsa végre a következő műveleteket.
    1. **Szolgáltató**lehetőségnél jelölje be a SqlClient adatszolgáltató.
    2. A **kiszolgáló nevét**az SQL adatraktár nevének megadása
    3. **Jelentkezzen be a kiszolgáló** csoportban jelölje be a **használata SQL Server-hitelesítés** , és adja meg a hitelesítési adatait.
    4. A **Csatlakozás az adatbázishoz** szakaszában jelölje be a meglévő adatraktár SQL-adatbázishoz.
    5. **A kapcsolat tesztelése**gombra.
    6. Amely a kapcsolat tesztelése eredményeinek jelentések párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **Kapcsolatkezelő** párbeszédpanel.
    7. **Kapcsolatkezelő** párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **ADO.NET kapcsolatkezelő beállítása** párbeszédpanel.
5. **ADO.NET kapcsolatkezelő beállítása** párbeszédpanelen kattintson **az OK gombra** kattintva visszatérhet a **ADO.NET cél szerkesztő**.
6. **ADO.NET cél szerkesztő**a **táblák és nézetek használata** listát hozhat létre új táblát, amely megfelel a forrástáblához oszlop listával a **Tábla létrehozása** párbeszédpanel megnyitásához kattintson az **Új** gombra.

    ![][12a]

7. **Táblázat létrehozása** párbeszédpanelen hajtsa végre a következő műveleteket.

    1. A céltábla nevének módosítása az **értékesítési rendelési adatot**.
    2. Távolítsa el a **rowguid** oszlopban. A **uniqueidentifier** adattípus az SQL adatraktár nem támogatott.
    3. **Pénz**módosítsa a **LineTotal** oszlop adattípusát. A **decimális** típusú adat az SQL adatraktár nem támogatott. Támogatott adattípusok című témakörben [CREATE TABLE (Azure SQL-adatraktár, a párhuzamos adatraktár)][].
    
        ![][12b]
    
    4. Kattintson **az OK gombra** a tábla létrehozása, és térjen vissza a **ADO.NET cél szerkesztő**.

8. **ADO.NET cél szerkesztő**jelölje ki a **hozzárendelések** fülre kattintva megtekintheti, hogyan a cél oszlop a forrás oszlop vannak megfeleltetve.

    ![][13]

9. Kattintson **az OK gombra** az adatforrás konfigurálása befejezéséhez.

## <a name="step-6-run-the-package-to-load-the-data"></a>Lépés a 6: Futtassa a csomagot, töltse be az adatokat

Futtassa a csomagot, kattintson a **Start** gombra az eszköztáron, vagy jelöljön ki egy a **hibakeresési** menü **Futtatás** közül.

A csomag kezdődik el, láthatók a sárga frissítésjelző kerekek, hogy a tevékenységet, valamint az eddigi feldolgozott sorok számát.

![][14]

A csomag futó befejezéskor zöld pipajelölések, hogy a sikeres, valamint az adatokat a cél a forrásból származó betöltött sorok száma látható.

![][15]

Gratulálok! Adatok betöltése Azure SQL-adatraktár használta az SQL Server Integration Services sikeresen.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók az SSIS-adatfolyam. Kezdje itt: [Data Flow][].
- Megtudhatja, hogy miként hibakeresési és elhárítása a csomagok jobb oldalán található a tervezési környezetet. Kezdje itt: [Hibaelhárítási eszközök csomag fejlesztéséhez][].
- Megtudhatja, hogy miként üzembe az SSIS-projektjeit és a csomagok Integration Services kiszolgálóra vagy más tárolási helyére. Kezdje itt: [telepítési a projektek és csomagok][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase útmutató]: https://msdn.microsoft.com/library/mt143171.aspx
[Töltse le az SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[TÁBLA (Azure SQL-adatraktár, a párhuzamos adatraktár) létrehozása]: https://msdn.microsoft.com/library/mt203953.aspx
[Adatfolyam]: https://msdn.microsoft.com/library/ms140080.aspx
[A csomag fejlesztéshez hibaelhárítási eszközök]: https://msdn.microsoft.com/library/ms137625.aspx
[Projektek és csomagok]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[A Microsoft SQL Server 2016 Integration Services Azure funkciócsomag]: http://go.microsoft.com/fwlink/?LinkID=626967
[Az SQL Server értékelése]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio-Közösség]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014-es minta adatbázisok]: https://msftdbprodsamples.codeplex.com/releases/view/125550
