<properties
   pageTitle="Azure gépi tanulási adatok elemzése |} Microsoft Azure"
   description="Azure gépi tanulási létrehozásához használandó a cserélendő gépi tanulási modell Azure SQL-adatraktár tárolt adatok alapján."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Azure gépi tanulási adatok elemzése

> [AZURE.SELECTOR]
- [A Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure gépi tanulási](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Ebben az oktatóanyagban Azure gépi tanulási létrehozásához használ a cserélendő gépi tanulási modell Azure SQL-adatraktár tárolt adatok alapján. Kifejezetten ez épít célzott marketingkampányok az Adventure Works, a kerékpárhoz szalon által előrejelzésére, ha egy ügyfél valószínűleg egy kerékpárhoz vásárol, vagy sem.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban lépéseinek, van szükség:

- A AdventureWorksDW mintaadatokat tartalmazó előre betöltött egy SQL adatraktár. Hozhatók létre, ez, [egy SQL adatraktár létrehozása][] című témakör tartalmaz, és válassza a mintaadatok betöltéséhez. Ha már van egy adatraktár, de nem rendelkezik a mintaadatokat, akkor [Töltse be manuálisan a mintaadatok][].

## <a name="1-get-data"></a>1. a adatok beolvasása
Az adatokat az AdventureWorksDW adatbázis dbo.vTargetMail nézetben van. Az adatok olvasható:

1. Jelentkezzen be az [Azure gépi tanulási studio][] , majd kattintson a kísérletek.
2. Kattintson a **+ Új** , és válassza az **Üres kísérlet**.
3. Adjon meg egy nevet a kísérlet: Marketing célzott.
4. Húzza a **olvasó** modul a modulokat ablakból a vásznat.
5. Adja meg az SQL adatraktár adatbázis részleteit a Tulajdonságok ablaktáblában.
6. Adja meg az adatbázis- **lekérdezés** olvasható a vizsgált adatokat.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Futtassa a kísérlet a kísérlet vászon a **Futtatás** gombra kattintva.
![A kísérlet futtatása][1]


Sikeresen fut a kísérlet befejezése után a kimeneti port a olvasó modul alján kattintson, és válassza a **Megjelenítés** az importált adatok megjelenítéséhez.
![Az importált adatok megtekintése][3]


## <a name="2-clean-the-data"></a>2. a adatok letisztázásának
Az adatok letisztázásának, engedje el az egyes oszlopok, amelyek nem megfelelő a modell. Ennek módja:

1. A **Projekt hasábok** modulhoz húzhat a vásznat.
2. Kattintson a **Launch oszlopkijelölő** megadhatja, melyik húzhatja kívánt oszlopot a Tulajdonságok ablaktáblában.
![Projekt oszlopok][4]

3. Két oszlopok kizárása: CustomerAlternateKey és a geographykey nevet.
![Szükségtelen oszlopok eltávolítása][5]


## <a name="3-build-the-model"></a>3. a modell összeállítása
Azt az adatok 80-20 felbontja: 80 %-a gépi tanulási, az adatmodelleket és a modell tesztelése 20 %-os képzése. Fog VÁLLALUNK "Két szintű" algoritmusok használható bináris besorolás probléma megoldásához.

1. A **felosztott** modul húzhat a vásznat.
2. Írja be 0.8 részét, a Tulajdonságok ablaktáblában az első kimeneti adatkészlet sorainak.
![Adatok felosztása képzés és tesztelése][6]
3. A **Második szintű csillapítja döntési fa** modul húzhat a vásznat.
4. A **Modell vonaton** modul húzhat a vászonra, és adja meg a bemeneti adatok alapján. Kattintson a **oszlopkijelölő indítsa el** a Tulajdonságok ablaktáblában.
      - Először a szövegbeviteli: Machine Learning algoritmus.
      - Második bemeneti: láthatja el ismeretekkel a algoritmus lévő adatokat.
![Csatlakozás a vonaton modell modul][7]
5. Jelölje ki **Kerékpárvásárló** előrejelzésére oszlopként.
![Előre oszlop kijelölése][8]


## <a name="4-score-the-model"></a>4. a modell pontszám
Most azt fogja teszteli, hogyan a modell tesztadatokat hajt végre. Egy másik algoritmus tekintheti meg, amelyek jobban hajtja végre a kiválasztott algoritmus összehasonlítja azt.

1. Húzza a vászonra **Pontszámhoz modell** modulra.
    Először a szövegbeviteli: modell második beviteli képzett: adatok vizsgálata ![pontszám a modell][9]
2. A **Második szintű Bayes pont gép** húzhat a kísérlet vásznat. Azt összehasonlítja hogyan az algoritmus hajt végre, a második szintű csillapítja döntési fa összehasonlítva.
3. Másolja a vágólapra, és illessze be a modulokat vonaton az adatmodelleket és pontszámhoz modell a vásznat.
4. Húzza a **Modell felmérése** modul két algoritmusokról összehasonlítása a vásznat.
5. **Futtassa** a kísérlet.
![A kísérlet futtatása][10]
6. Kattintson a kimeneti port a modell felmérése modul alján, majd a Megjelenítés parancsra.
![Kiértékelés eredmények ábrázolása][11]

A mértékek, feltéve a ROC görbe, a pontosság-visszahívási ábra és felvonó görbe. Ezek a mértékek megnézve is láthatja, hogy az első modell elvégzett jobban, mint a második lehetőséget. Tekintse meg a Mi az első modell előre jelzett, kattintson a kimeneti port pontszámhoz modell, majd kattintson a Megjelenítés gombra.
![Pontszámhoz eredmények ábrázolása][12]

A próba adatkészlet hozzáadott két további hasábok jelenik meg.

- A valószínűség pontszáma: annak valószínűsége, hogy egy ügyfél-e a kerékpárhoz-vásárló.
- A címkék pontszáma: az osztályozás elkészült a modell – kerékpárhoz-vásárló (1)-e (0). A valószínűség küszöbértéknél parancsot az 50 %-os van állítva, és a módosítható.

Az oszlop (előrejelzési) pontszáma adatfeliratokkal (tényleges) Kerékpárvásárló összehasonlítása, megtekintheti, mennyire végezte el a modell. Következő lépések, mint a modell előrejelzések új vevők és a modellt, webes szolgáltatás közzététele vagy eredmények vissza írni SQL adatraktár is használhatja.

## <a name="next-steps"></a>Következő lépések

Prediktív gépi tanulási az adatmodellek létrehozásával kapcsolatos további információért olvassa el a [gépi tanulási Azure a bemutatása][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure gépi tanulási studio]:https://studio.azureml.net/
[Tanulási Azure a gép – bevezetés]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[mintaadatok manuálisan betöltése]: sql-data-warehouse-load-sample-databases.md
[Hozzon létre egy SQL adatraktár]: sql-data-warehouse-get-started-provision.md
