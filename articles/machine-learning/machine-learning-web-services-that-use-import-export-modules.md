<properties
    pageTitle="Azure Machine Learning webszolgáltatásokhoz adatimportálás és adatexportálás modulok használó telepítése |} Microsoft Azure"
    description="Megtudhatja, hogyan küldhet és fogadhat adatokat webszolgáltatásból az adatok importálása és az adatok exportálása modulok segítségével."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Azure Machine Learning webszolgáltatásokhoz adatimportálás és adatexportálás modulok használó üzembe helyezése 

Amikor létrehoz egy prediktív kísérlet, általában hozzáadhat egy webes szolgáltatás bemeneti és kimeneti. Amikor rendszerbe állítják a kísérlet, fogyasztói küldhet és fogadhat a adatok át a ráfordítások és a kimeneti értékeket webszolgáltatásból. Az egyes alkalmazások a fogyasztói adatok adatcsatornát elérhetők legyenek vagy egy külső adatforrásból, például Azure Blob-tárolóhoz már találhatók. Ebben az esetben webes szolgáltatás ráfordítások és a kimeneti értékeket adatainak olvasása és írása nem szükséges. Azokat is, ehelyett a köteg végrehajtás szolgáltatás (BES) segítségével adatokat az adatforrásból, az adatok importálása modulról olvasni és írási pontozási eredménye-adatok exportálása modulról különböző helyre.

Az adatok importálása és exportálása adatok modulokat, olvassák és hány helyekre, például egy webes URL-cím HTTP struktúra lekérdezés, egy Azure SQL-adatbázis, Azure táblatárolóhoz, Azure blobtárolóhoz, Adatcsatornáról keresztül a szükséges adatokat, vagy egy helyszíni SQL-adatbázis írni.

Ez a témakör használja a "minta 5: bináris osztályozási vonaton, vizsgálat kiértékelés: felnőtt adatkészlet" mintát, és azt feltételezi, hogy az adatkészlet már be van töltve egy Azure SQL censusdata nevű táblába.

## <a name="create-the-training-experiment"></a>A tanfolyam kísérlet létrehozása 
 
Megnyitásakor a "minta 5: bináris osztályozási vonaton, vizsgálat kiértékelés: felnőtt adatkészlet" minta, akkor használja a minta bináris besorolás felnőtt népszámlálás bevétel adatkészlet. És a kísérlet az a vászonra az alábbi képhez hasonló fog kinézni.

![Kiindulási konfiguráció a kísérlet.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Az adatok olvasható az Azure SQL-táblából:

1.  Törölje az adatkészlet modulra.
2.  Összetevők Keresés mezőbe írja be az importálás.
3.  A kísérlet vászon- *Adatok importálása* modul hozzáadása az eredménylistából.
4.  Az *Adatok importálása* modul a bemeneti a *Hiányzó adatok karbantartása* modul kimeneti csatlakozni.
5.  A Tulajdonságok ablaktáblában jelölje ki a **Azure SQL-adatbázis** **Adatforrás** a legördülő.
6.  Az **adatbázis-kiszolgáló nevét** **az adatbázis neve**, **felhasználónév**és **jelszó** mezők, adja meg az adatbázis a megfelelő adatokat.
7.  Az adatbázis lekérdezés mezőjébe írja be az alábbi lekérdezés.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  A képernyő alján a kísérlet vászon, kattintson a **Futtatás**parancsra.

## <a name="create-the-predictive-experiment"></a>A cserélendő kísérlet létrehozása

Ezután a cserélendő kísérlet, amelyből telepíti a webszolgáltatás beállítása.

1.  A kísérlet vászon alján kattintson a **Webes szolgáltatás beállítása** , és válassza ki a **Cserélendő webszolgáltatás [ajánlott]**.
2.  A *Web Service beviteli* és a *webes szolgáltatás kimeneti modulok* eltávolítása a cserélendő kísérlet. 
3.  Összetevők Keresés mezőbe írja be az exportálás.
4.  A kísérlet vászon- *Adatok exportálása* modul hozzáadása az eredménylistából.
5.  Csatlakozás a *Pontszámhoz modell* modul a bemeneti *Adatok exportálása* modul kimeneti. 
6.  Tulajdonságok ablaktáblában jelölje ki az adatokat a cél legördülő **Azure SQL-adatbázis** .
7.  Az **adatbázis-kiszolgáló nevét** **az adatbázis neve**, a **kiszolgáló felhasználói fiók nevét**és a **kiszolgáló felhasználói fiók jelszavának** mezők, adja meg az adatbázis a megfelelő adatokat.
8.  **Vesszővel elválasztott menteni kívánt oszlopok listája** mezőbe írja be a címkék pontszáma.
9.  Az **adatokat a táblázat neve mezőben**írja be dbo. ScoredLabels. Ha a táblázat nem létezik, létrejön a kísérlet fut, vagy a webszolgáltatás nevezik.
10. Írja be a **vesszővel elválasztott adattábla oszlopok listája** mező ScoredLabels.

Ha olyan alkalmazás, amely felhívja a végleges webszolgáltatás ír, előfordulhat, hogy szeretne megadni egy másik beviteli lekérdezést vagy a céltábláról futásidőben. A ráfordítások és a kimeneti értékeket konfigurálásához a webes szolgáltatás paraméterei funkcióval az *Adatok importálása* modul *adatforrás* tulajdonság és az *Adatok exportálása* mód adatok destination tulajdonság beállítása.  Webes szolgáltatás paraméterei a további tudnivalókért lásd: a [AzureML Web Service paraméterek bejegyzés](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) a Cortana üzletiintelligencia-és gépi tanulási Blog.

A Web Service paramétereinek importálása lekérdezési és a céltábla konfigurálása:

1.  Az *Adatok importálása* modulhoz a Tulajdonságok ablaktáblában kattintson a ikonra a felső sarkában látható az **adatbázis-lekérdezés** mezőben, és a **webszolgáltatási paraméter beállítás**kiválasztása.
2.  Az *Adatok exportálása* modulhoz a Tulajdonságok ablaktáblában kattintson a ikonra a felső sarkában látható az **adatokat a táblázat neve** mezőben, és a **webszolgáltatási paraméter beállítás**kiválasztása.
3.  A képernyő alján az *Adatok exportálása* modul Tulajdonságok ablaktábla, **Webes szolgáltatás paraméterei** csoportban kattintson az adatbázis-lekérdezés, és nevezze át a lekérdezést.
4.  Kattintson az **adatokat a táblázat neve** , majd nevezze át a **táblázatot**.

Ha végzett, a kísérlet az alábbi képen láthatóhoz hasonlóan kell kinéznie.
 
![Kísérlet végleges kinézetét.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Most már telepítheti a kísérlet webes szolgáltatásként.

## <a name="deploy-the-web-service"></a>A webszolgáltatás terjesztése 
Klasszikus vagy új webszolgáltatás telepítheti.

### <a name="deploy-a-classic-web-service"></a>Klasszikus webszolgáltatás terjesztése

Klasszikus webes szolgáltatásként telepíthető, és felhasználja azt az alkalmazás létrehozása:

1.  A kísérlet vászon alján kattintson a Futtatás gombra.
2.  A Futtatás befejezése kattintson a **Web szolgáltatás üzembe** , és jelölje be a **Webes szolgáltatás üzembe [klasszikus]**.
3.  Webes szolgáltatás irányítópult lapon keresse meg a API használatával. Másolás és későbbi felhasználás céljából menteni.
4.  Kattintson az **Alapértelmezett végpont** táblát a **Köteg végrehajtás** hivatkozásra kattintva nyissa meg a API súgó lapon.
5.  A Visual Studióban, C# console-alkalmazás létrehozása.
6.  A lap alján a **Példakódot** szakaszban keresse meg a API súgó lapon.
7.  Másolja és illessze be a C# példakódot Program.cs fájlját, és távolítson el minden hivatkozást a blob-tárolóhoz.
8.  Frissítse a *apiKey* változó értékét a korábban mentett API billentyűt.
9.  Keresse meg a kérelem nyilatkozat és frissíti a webes szolgáltatás paraméterei az *Adatok importálása* és az *Adatok exportálása* modulok átadott értékét. Ebben az esetben meg fog az eredeti lekérdezéssel, de egy új tábla nevét adja meg.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Futtassa az alkalmazást. 

Új tábla a Futtatás befejeztével bekerül a pontozási eredményeket tartalmazó adatbázis.

### <a name="deploy-a-new-web-service"></a>Új webes szolgáltatás telepítése

Új webes szolgáltatásként telepíthető, és felhasználja azt az alkalmazás létrehozása:

1.  A képernyő alján a kísérlet vászon, kattintson a **Futtatás**parancsra.
2.  A Futtatás befejezése **Webes szolgáltatás üzembe** és **Üzembe webszolgáltatás [Új]**válassza.
3.  A kísérlet üzembe lapon írjon be egy nevet a webszolgáltatás válasszon egy árak csomagot, majd kattintson a **központi telepítés**.
4.  **Quickstart útmutató** lapon kattintson a **felhasználás**.
5.  A **Minta kód** csoportban kattintson a **köteget**.
6.  A Visual Studióban, C# console-alkalmazás létrehozása.
7.  Másolja és illessze be a C# példakódot Program.cs fájljait.
8.  Frissítse a *apiKey* változó értékét az **Elsődleges kulcs** **alapvető felhasználási információ** szakaszában található.
9.  Keresse meg a *scoreRequest* nyilatkozatot és frissíti a webes szolgáltatás paraméterei az *Adatok importálása* és az *Adatok exportálása* modulok átadott értékét. Ebben az esetben meg fog az eredeti lekérdezéssel, de egy új tábla nevét adja meg.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Futtassa az alkalmazást. 
 

