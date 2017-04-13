<properties
   pageTitle="Tó adattár található adatok elemzése a Power BI segítségével |} Microsoft Azure"
   description="Használni a Power BI Azure adatok tó tárban tárolt adatok elemzése céljából"
   services="data-lake-store" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Tó adattár található adatok elemzése a Power BI segítségével

Ez a cikk megtanulhatja, hogyan elemzése és Azure adatok tó tárban tárolt adatok ábrázolása a Power BI Desktop segítségével.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure tó adattár fiókot**. Kövesse az utasításokat az [első lépések az Azure tó adattár az Azure-portálon](data-lake-store-get-started-portal.md). Ez a cikk tartalma feltételezi, hogy már **mybidatalakestore**, nevű tó adattár fiók létrehozása és adatok mintafájl (**Drivers.txt**) feltölteni azt. A mintafájl [Azure adatok tó mely számjegy tárházba](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)letölthető érhető el.

- **A power BI Desktop**. Ez letöltheti a [Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=45331)letöltőközpontból. 


## <a name="create-a-report-in-power-bi-desktop"></a>Jelentés létrehozása a Power BI-asztal

1. Indítsa el a Power BI Desktop a számítógépen.

2. Az **otthoni** menüszalagján kattintson az **Adatok beolvasása**, és válassza a további. **Adatok átvétele** párbeszédpanelen kattintson az **Azure** **Azure tó adattár**kattintson, és kattintson a **Csatlakozás**.

    ![Csatlakozás tó adattárhoz] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Csatlakozás tó adattárhoz")

3. Ha megjelenik egy párbeszédpanel, az összekötő a egy fejlesztési szakasz kapcsolatban, kiválaszthatják, hogy továbbra is.

4. A **Microsoft Azure adattár tó** párbeszédpanel tó adattár fiókja URL-címét, és kattintson **az OK**gombra.

    ![Adatok tó tár URL-címe] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "Adatok tó tár URL-címe")

5. A következő párbeszédpanelen kattintson a **Bejelentkezés** jelentkezzen be az tó adattár fiók elemre. A szervezet bejelentkezési lapra irányítja. Kövesse az útmutatást követve jelentkezzen be a fiókjába.

    ![Jelentkezzen be az adatok tó áruházból] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Jelentkezzen be az adatok tó áruházból")

6. Miután sikeresen aláírták, kattintson a **Csatlakozás**gombra.

    ![Csatlakozás tó adattárhoz] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Csatlakozás tó adattárhoz")

7. A következő párbeszédpanel megjeleníti a feltöltött tó adattár fiókját a fájlt. Ellenőrizze az információ, és kattintson a **Betöltés**gombra.

    ![Adatok betöltése tó tárból] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Adatok betöltése tó tárból")

8. Az adatok sikeres betöltése a Power BI, után jelenik meg a következő mezők a **mezők** fülre.

    ![Imported mezők] (./media/data-lake-store-power-bi/imported-fields.png "Imported mezők")

    Áttekintéséhez és elemezheti az adatokat, hogy szívesebben érhető el az alábbi mezők használati adatok

    ![Kívánt mezők] (./media/data-lake-store-power-bi/desired-fields.png "Kívánt mezők")

    A következő lépésekkel a konvertálni kívánt formátumban az importált adatok a lekérdezés frissíteni fogjuk.

9. Menüszalagról a **Kezdőlap fülre** kattintson a **Lekérdezés szerkesztése**elemre.

    ![Lekérdezés szerkesztése] (./media/data-lake-store-power-bi/edit-queries.png "Lekérdezés szerkesztése")

10. A Lekérdezésszerkesztőben, a **Content** oszlopot, kattintson a **bináris**.

    ![Lekérdezés szerkesztése] (./media/data-lake-store-power-bi/convert-query1.png "Lekérdezés szerkesztése")

11. Ekkor megjelenik a fájl ikonra, amely a feltöltött **Drivers.txt** fájlt. Kattintson a jobb gombbal a fájlra, majd kattintson a **CSV**.  

    ![Lekérdezés szerkesztése] (./media/data-lake-store-power-bi/convert-query2.png "Lekérdezés szerkesztése")

12. Ekkor megjelennek a kimeneti, alább látható módon. Az adatok már formátumot használó képi megjelenítések létrehozása érhető el.

    ![Lekérdezés szerkesztése] (./media/data-lake-store-power-bi/convert-query3.png "Lekérdezés szerkesztése")

13. Az **otthoni** menüszalagján kattintson a **Bezárás gombra, és az alkalmazás**elemet, és kattintson a **Bezárás gombra, és alkalmazása**gombra.

    ![Lekérdezés szerkesztése] (./media/data-lake-store-power-bi/load-edited-query.png "Lekérdezés szerkesztése")

14. A lekérdezés frissül, amint a **mezők** lap az új képi megjelenítés rendelkezésre álló mezők jelennek meg.

    ![Frissített mezők] (./media/data-lake-store-power-bi/updated-query-fields.png "Frissített mezők")

15. Tudassa velünk hozzon létre egy kördiagram valamelyik, az egyes városaihoz egy adott ország-illesztőprogramok ábrázolására. Kéri, adja meg a következő beállításokat.

    1. A képi megjelenítések lapon kattintson a tortadiagram jelét.

        ![Kördiagram létrehozása] (./media/data-lake-store-power-bi/create-pie-chart.png "Kördiagram létrehozása")

    2. Az oszlopok fogjuk használni, hogy **oszlop 4** (a város neve) és **7 oszlop** (ország neve). Húzza ezeket az oszlopokat a **mezők** lap **megjelenítések** lap alább látható módon.

        ![Megjelenítések létrehozása] (./media/data-lake-store-power-bi/create-visualizations.png "Megjelenítések létrehozása")

    3. A diagram a célszerű most hasonló olyanra, mint az alább látható módon:

        ![Kördiagram] (./media/data-lake-store-power-bi/pie-chart.png "Megjelenítések létrehozása")

16. A lap szintű szűrőket elemre kattintva egy adott országhoz, most megtekintheti a számot az egyes városaihoz a kijelölt ország-illesztőprogramok. A **Megjelenítés** lap, **oldal szint szűrők**, például csoportban **Brazília**.

    ![Ország kiválasztása] (./media/data-lake-store-power-bi/select-country.png "Ország kiválasztása")

17. A diagram automatikusan frissül a illesztőprogramok jelennek meg a város Brazília.

    ![Illesztőprogramok országban] (./media/data-lake-store-power-bi/driver-per-country.png "Az egyes országok illesztőprogramok")

18. A **fájl** menüben kattintson a **Mentés** fájlként való mentéséhez a Megjelenítés a Power BI Desktop.

## <a name="publish-report-to-power-bi-service"></a>Jelentés közzététele Power BI szolgáltatás

Miután létrehozta a képi megjelenítések a Power BI Desktop, megoszthatja másokkal a Power BI szolgáltatás közzétéve. Ennek módja, tanulmányozza [a közzététel a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Lásd még:

* [Adatok elemzése az adatok tó Analytics használatával tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
