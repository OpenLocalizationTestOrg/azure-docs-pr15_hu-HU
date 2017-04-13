<properties 
    pageTitle="Értékáram-elemzés feladatokhoz kimenet adatok beállítása |} Microsoft Azure" 
    description="Értékáram-elemzés feladatok kimeneti értékeket beállítása |} tanulási javaslat szakaszában."
    keywords="adatok kimeneti, adatok mozgás"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Értékáram-elemzés feladatokhoz kimenet adatok konfigurálása

Azure Értékáram-elemzés feladatok egy vagy több adat kimeneti értékeket, amely definiálja egy kapcsolatot egy meglévő adatok gyűjtő kapcsolhat össze. A Értékáram-elemzés feladatok folyamatok és átalakítások bejövő adatokat, az adatok kimeneti események adatfolyam írja be a feladatok kimeneti.

Adatfolyam Analytics adatainak kimeneti értékeket használható valós idejű irányítópultok vagy a riasztások, a forrás indulnak el adatok mozgását munkafolyamatok, vagy egyszerűen csak a köteg feldolgozásához később adatok archiválása. Értékáram-elemzés első osztályú integrációs szolgáltatásokkal több Azure, amelyek részletesen itt ismertetett tartalmaz.

Egy kimenet hozzáadása a Értékáram-elemzés feladatok:

1. Az Azure klasszikus portálon kattintson a **kimeneti értékeket** , és válassza **A kimeneti hozzáadása** a Értékáram-elemzés feladatok.

    ![Kimeneti értékeket hozzáadása](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Az Azure-portálon kattintson a **kimeneti értékeket** csempére, a megjelenítő Értékáram-elemzés feladat.

    ![Azure Porta kimeneti értékeket hozzáadása](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Adja meg a kimenet:

    ![Válassza a mozgás adattípus](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure portálon válassza a mozgás adattípus](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Adja meg egy rövid nevet, a **Kimeneti Alias** mezőbe a nyomtatáshoz. Ez a név használható lekérdezésben a feladatok később a kimenet hivatkozik.  
    
    Töltse ki a többi a szükséges kapcsolat tulajdonságai parancsra, a kimeneti csatlakozni.  Ezeket a mezőket típusától függően változnak kimeneti és határozzák meg részletesen.  

    ![Adatok kimeneti tulajdonságok hozzáadása](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. A kimenet típusától függően előfordulhat, megadhatja, hogyan az adatok szerializálásának vagy formázott. A minden kimeneti típus adott szerializálási beállításait az alábbi ismerteti.

    Töltse ki a többi szükséges kapcsolat tulajdonságainak szeretne csatlakozni az adatforráshoz. Ezeket a mezőket beviteli és az adatforrás típusa típusától függően változnak, és részletesen definiált [Itt](stream-analytics-create-a-job.md).  

    ![Esemény központi kimeneti adatok hozzáadása](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Esemény-hubon keresztül csatlakozott kimeneti Azure portál adatok](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Bármely kimeneti elem hozzáadása a projekthez léteznie kell, mielőtt a a feladat van és események megkezdése halad. Például ha használja a Blob-tárolóhoz olyan eredménye, a feladat nem hoz létre egy tároló fiók automatikusan. A felhasználó által létrehozott, a ASA feladat megkezdése előtt szükséges.

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
