<properties
    pageTitle="Gépi tanulási munkaterület kezelése |} Microsoft Azure"
    description="Azure gépi tanulási munkaterületek, való hozzáférés kezelése és üzembe helyezéséhez és Machine Learning API webes szolgáltatások kezelése"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Az Azure gépi tanulási munkaterület kezelése

>[AZURE.NOTE] A jelen cikkben ismertetett eljárások szempontjából fontos Azure gépi tanulási klasszikus webszolgáltatásokhoz. Kezelésével kapcsolatos tudnivalók a gépi tanulási Web Services portál webszolgáltatásokhoz a [webszolgáltatás az Azure gépi tanulási webszolgáltatásokhoz portálon kezelése](machine-learning-manage-new-webservice.md)című cikkben talál.

Az Azure klasszikus portál használatával, hogy gépi tanulási munkaterületek kezelése:

- A munkaterület használatának figyelése
- A munkaterület engedélyezze vagy elutasítsa konfigurálása
- A munkaterület létrehozott webes szolgáltatások kezelése
- A munkaterület törlése

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Ezeken kívül az irányítópult lap biztosít a munkaterület használatát áttekintése és a munkaterületi adatokhoz a kiadványok.  

> [AZURE.TIP] Azure gépi tanulási Studio alkalmazásban, a **WEBES szolgáltatások** lap hozzáadása, módosítása vagy gépi tanulási webszolgáltatás törlése.

Munkaterület kezelése:

1.  Jelentkezzen be a Microsoft Azure-fiók használatával [Azure klasszikus portál](https://manage.windowsazure.com/) – használja a fiókot, amely az Azure előfizetéshez van társítva.
2.  A Microsoft Azure szolgáltatások panelen kattintson a **Gépi TANULÁSI**.
3.  Kattintson a munkaterület kezelni szeretné.

A munkaterület lapon három fül foglalja magában:

- **IRÁNYÍTÓPULT** - munkaterület-kihasználtság megtekintése és más információk segítségével
- **KONFIGURÁLÁSA** - lehetővé teszi a munkaterületre való hozzáférés kezelése
- **WEBSZOLGÁLTATÁSOK** - lehetővé teszi a munkaterületről közzétett webes szolgáltatások kezelése

## <a name="to-monitor-how-the-workspace-is-being-used"></a>A munkaterület használatának figyelése

Kattintson az **IRÁNYÍTÓPULT** fülre.

Az irányítópult lapon megtekintheti a munkaterület általános használatát és a munkaterület adatainak kiadványok első.

- A **számítása** diagramra a munkaterület által használt számítási erőforrások jeleníti meg. Módosíthatja a űrlapnézetet relatív és abszolút érték, és módosíthatja az időkeret megjelenik a diagramon.
- **Használatát áttekintése** a munkaterület által használt Azure tároló jeleníti meg.
- **Fontos** munkaterület adatainak és hasznos hivatkozások biztosít.

> [AZURE.NOTE] A **bejelentkezési kísérleteket Machine Learning Studio** hivatkozás nyitja meg, hogy be van jelentkezve a Microsoft-Account gépi tanulási Studio. Jelentkezzen be az Azure klasszikus portálra munkaterület létrehozásához használt Microsoft-Account automatikusan nincs engedélye, hogy a munkaterület megnyitása. A munkaterület megnyitása, kell bejelentkeznie Microsoft-Account a munkaterület tulajdonosaként megadott vagy meghívót kap a tulajdonos, ha be szeretne kapcsolódni a munkaterület kell.


## <a name="to-grant-or-suspend-access-for-users"></a>Engedélyezheti vagy felfüggesztése a felhasználók hozzáférésének ##

Kattintson a **beállítás** fülre.

A konfiguráció lapon a következőkre van lehetősége:

- Hozzáférés a gépi tanulási munkaterületre felfüggeszti a MEGTAGADÁS gombra kattintva. Felhasználók már nem tudja megnyitni a munkaterületet gépi tanulási Studio alkalmazásban. Az access visszaállításához kattintson az Engedélyezés gombra.

Kezeléséhez a munkaterület gépi tanulási Studio hozzáféréssel rendelkező további fiókokat kattintson **bejelentkezni a Machine Learning Studio** az **IRÁNYÍTÓPULT** lapon (lásd a portáladatbázis előző kapcsolatos **bejelentkezési kísérleteket Machine Learning Studio**). Ekkor megnyílik a munkaterület gépi tanulási Studio alkalmazásban. Itt kattintson a **Beállítások** fülre, majd a **felhasználók**. Választhatja a **További felhasználók MEGHÍVÁSA** a felhasználók hozzáférésének a munkaterületre, vagy válassza ki azt a felhasználót, és kattintson az **ELTÁVOLÍTÁS**gombra.


## <a name="to-manage-web-services-in-this-workspace"></a>A munkaterület webes szolgáltatások kezelése

Kattintson a **WEB SERVICES** fülre.

Ez a munkaterület közzétett webes szolgáltatások listáját jeleníti meg.
Webszolgáltatás kezeléséhez kattintson a nevére a listában, és nyissa meg azt a weblapot szolgáltatás.

Előfordulhat, hogy a webszolgáltatás definiált egy vagy több végpontok.

- Az "Alapértelmezett" végpont mellett további végpontok határozhatja meg. A végpont hozzáadásához kattintson a **Végpontok kezelése** a képernyő alján az irányítópult nyissa meg az Azure gépi tanulási webszolgáltatásokhoz portált.

- (Nem törölhető, az "Alapértelmezett" végpontot) zárólap törlése jelölje be a jelölőnégyzetet, a végpontok sor elejére, és kattintson a **Törlés**gombra. A végpont eltávolítása a webes szolgáltatás.

    > [AZURE.NOTE] Ha az alkalmazás a végpont törlésekor a webes szolgáltatás végpontjának használ, az alkalmazás hibaüzenetet kap a következő alkalommal, amikor megkísérli elérni a szolgáltatást.

Kattintson a nevére a megnyitáshoz, egy webes szolgáltatás végpontjának. 

Az irányítópult lapon megtekintheti a webszolgáltatás általános használatát egy időszakra vetített állapotával. Kattintson a időszakokat tartalmazó legördülő menü jobb felső sarokban a használatát diagramok megtekintése az időszak kijelölhet. Az irányítópult jeleníti meg az alábbi adatokat:

- **Kérések fölé idő** lépés grafikonhoz kérések száma a kijelölt időszakban jeleníti meg. Segítheti a azonosítása, ha a használat kiugrásainak megfelelő tapasztalja.
- **Kérés-válasz kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány sikertelen kérés-válasz hívások száma.
- **Átlagos kérelem-számítja ki válaszidő** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Köteg kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány nem sikerült köteg kérések száma.
- **Átlagos feladat-időtartam** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Hibák** a hívások átirányítása a webszolgáltatás jeleníti meg, hogy van-e hibák összesített száma.
- **Szolgáltatások költségeket** jeleníti meg a számlázási csomagot, a szolgáltatásokhoz kapcsolódó díjai.

A beállítás lapon frissítheti a következő tulajdonságokat:

* **Leírást** adjon meg egy leírást a webszolgáltatás teszi lehetővé. Leírás megadása kötelező.
* **Naplózás** lehetővé teszi engedélyezése vagy letiltása a hiba jelentkezik be a végpontot. További információt a naplózást olvassa el a engedélyezése [gépi tanulási webszolgáltatásokhoz naplózás](machine-learning-web-services-logging.md)című témakört.
* **Mintaadatok engedélyezése** lehetővé teszi, hogy meg kell adnia a mintaadatokat, tesztelje a kérelem-válasz szolgáltatás használható. Ha a webszolgáltatás gépi tanulási Studio hozta létre, a mintaadatok kérdezi le az adatokat a modell betanítása a használt. A szolgáltatás programozás útján hozta létre, ha az adatok forrása a mintaadatokat, a JSON-csomag részeként megadott.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
