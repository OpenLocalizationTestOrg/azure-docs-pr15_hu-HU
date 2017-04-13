<properties 
   pageTitle="Tó adattár – első lépések |} Azure" 
   description="A portál használatával tó adattár fiók létrehozásához, és végezze el az alapműveletek az adatok tó áruházban" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Első lépések az Azure tó adattár az Azure-portálon

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [A PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-VAL](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [NODE.js](data-lake-store-manage-use-nodejs.md)

További információ az Azure Portal segítségével tó adattár Azure-fiók létrehozása, és olyan mappában, létrehozásakor fájlok feltöltése és letöltése adatok alapvető műveleteket, törölje a fiókot, stb. Tó adattár kapcsolatos további tudnivalókért olvassa el a [Áttekintése az Azure tó adattár](data-lake-store-overview.md)című témakört.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Megismerheti egy videók tegye?

Az alábbi videókból tó adattár – első lépések.

* [Adatok tó Store-fiók létrehozása](https://mix.office.com/watch/1k1cycy4l4gen)
* [Az adatok Intézővel tó adattár adatok kezelése](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Azure tó adattár fiók létrehozása

1. Jelentkezzen be az új [Azure-portálon](https://portal.azure.com).

2. Kattintson az **Új**gombra, kattintson az **adatok + tárhely**, és válassza az **Azure tó adattár**. Olvassa el az **Azure tó adattár** lap, és kattintson a lap bal alsó sarkában kattintson a **Létrehozás** gombra.

3. Az **Új adatok tó tároló** lap adja meg az értékeket, a képernyőképet alább látható módon:

    ![Azure tó adattár új fiók létrehozása] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Azure adatok tó új fiók létrehozása")

    - **Előfizetés**. Jelölje ki az előfizetés meg szeretné hozzon létre egy új tó adattár fiókot.
    - **Erőforráscsoport**. Jelölje ki a meglévő erőforráscsoport, vagy hozzon létre egyet **az erőforrás csoport létrehozása** gombra. Erőforráscsoport olyan tároló, amely az alkalmazáshoz kapcsolódó erőforrásokat. További tudnivalókért lásd: az [Erőforrás-csoportok Azure-ban](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Hely**: jelöljön ki egy helyet, ahová a tó adattár fiók létrehozásához.

4. Jelölje ki a **Startboard PIN-kód** , ha azt szeretné, hogy a tó adattár fiók érhető el a Startboard.

5. Kattintson a **létrehozása**gombra. Ha azt választotta, a fiókot a startboard rögzít, visszakerül a startboard, és megjelenik a kiépítési tó adattár fiók állapotának. Miután a tó adattár fiók már kiépítve, a fiók lap jelenik meg.

6. Bontsa ki a **Essentials** legördülő megtudhatja, tó adattár-fiókja adatait, például az erőforrás csoportosítson része, a hely stb. Kattintson a **Quick Start** ikonra kattintva megtekintheti a többi tó adattár kapcsolatos erőforrásokra mutató hivatkozásokat.

    ![Az Azure tó adattár fiók] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Az Azure adatok tó fiók")

## <a name="createfolder"></a>Mappák létrehozása az Azure tó adattár fiók

Mappák kezeléséhez és adattárolásra tó adattár fiókhoz tartozó hozhat létre.

1. Nyissa meg az éppen létrehozott tó adattár fiókot. A bal oldali ablaktábláján kattintson a **Tallózás gombra**kattintson **Tó adattár**, és a az adatok tó áruházból lap válassza a fiók nevére, amelyben létre szeretné hozni a mappák. Ha, a kiemelt a startboard a fiókot, kattintson a fiók mozaik.

2. Kattintson a tó adattár fiók lap **Adatok Explorer**.

    ![Mappák létrehozása tó adattár fiók] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappák létrehozása tó adattár fiók")

3. A tó adattár fiók lap az **Új mappa**parancsra, írja be az új mappa nevét, és kattintson **az OK**gombra.
    
    ![Mappák létrehozása tó adattár fiók] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappák létrehozása tó adattár fiók")
    
    Az **Adatok Explorer** lap szerepelni fognak az újonnan létrehozott mappát. Egymásba ágyazott mappákat importálhat bármilyen szinten hozhat létre.

    ![Adatok tó fiók mappák létrehozása] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Adatok tó fiók mappák létrehozása")


## <a name="uploaddata"></a>Adatok feltöltése tó adattár Azure-fiók

Az adatok is feltölthet, közvetlenül a gyökérszinten Azure tó adattár fiók vagy a számla belüli létrehozott mappára. A, a képernyőfelvétel alatt kövesse a almappában fájl feltöltése a az **Adatok Explorer** lap. A képernyőkép rögzítése a feltöltött a fájlt (piros mezőben jelölt) tetejéhez látható almappában.

Ha tölthet fel mintaadatokat is tartalmazó keres, a az [Azure adatok tó mely számjegy tárházba](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)kattint a **Mentővel adatok** mappát.

![Adatok feltöltése] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Adatok feltöltése")


## <a name="properties"></a>Tulajdonságok és a gyorsítótárban tárolt adatokat elérhető műveletek

Kattintson az újonnan hozzáadott fájlra kattintva nyissa meg a **Tulajdonságok** lap. Ez a lap a fájlt, és a műveleteket végezheti el a fájlt a kapcsolódó tulajdonságainak érhetők el. Azure tó adattár fiókban, a lenti képernyőképet piros mezőjébe kiemelt fájl is másolhatja a teljes elérési útját.

![Az adatok tulajdonságai] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Az adatok tulajdonságai")

* Láthatja a fájlt, közvetlenül a böngészőből, a **kép** gombra. Megadhatja, valamint a betekintő formátumát. Kattintson az **Előnézet**gombra, és kattintson a **Fájl előnézet** lap **Formátum** az **Előnézeti fájlformátum** a lap adja meg a beállításokat, például a sorok számát a megjelenítéséhez használandó kódolás, elválasztó használata stb.

  ![Előzetes fájlformátum] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Előzetes fájlformátum")

* Kattintson a **Letöltés** a fájl letöltése a számítógépre.

* Kattintson a **fájl átnevezése** a fájl átnevezéséhez.

* Kattintson a **fájl törlése** a fájl törlése gombra.


## <a name="secure-your-data"></a>Az adatok védelme

Az Azure tó adattár fiókjához Azure Active Directory és a hozzáférés-vezérlés (hozzáférés-vezérlési listák) tárolt adatok biztonságát. Ennek módja, tanulmányozza [Azure tó adattár biztonságossá tétele adatok](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Fiók törlése Azure tó adattárhoz

A tó adattár lap Azure tó adattár fiók törléséhez kattintson a **Törlés**gombra. A művelet megerősítését kéri a törölni kívánt fiók nevét. Adja meg a fiók nevét, és kattintson a **Törlés**gombra.

![Törlés adatok tó fiók] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Törlés adatok tó fiók")


## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Az Access a diagnosztikai naplók adatok tó üzlet](data-lake-store-diagnostic-logs.md)
