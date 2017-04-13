<properties
    pageTitle="A webszolgáltatás az Azure gépi tanulási webes Serivces portálon kezelheti |} Microsoft Azure"
    description="Azure gépi tanulási munkaterületek, való hozzáférés kezelése és üzembe helyezéséhez és Machine Learning API webes szolgáltatások kezelése"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Az Azure gépi tanulási webszolgáltatásokhoz portálon webszolgáltatás kezelése

A gépi tanulási új és a klasszikus webes szolgáltatások a Microsoft Azure gépi tanulási webszolgáltatásokhoz portálon kezelheti. Klasszikus Web services és az új webhely-szolgáltatások különböző mögöttes technológiák alapján, mivel némileg eltérő szolgáltatásairól van az egyes őket.

A gépi tanulási Web Services portál a következőkre van lehetősége:

- Figyelje meg, hogyan a webszolgáltatás van használatban.
- Állítsa be a leírást, frissítse a billentyűk webes szolgáltatás (csak új), a tárhely fiók kulcs (csak új), naplózás engedélyezése, frissítése és engedélyezése vagy letiltása a mintaadatokat tartalmazó.
- A webszolgáltatás törlése.
- Létrehozása, törlése, illetve frissítést számlázási csomagok (csak új).
- Hozzáadása és törlése a végpontok (csak a klasszikus)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Új webes szolgáltatások kezelése

Az új webhely-szolgáltatások kezelése:

1.  Jelentkezzen be a [Microsoft Azure gépi tanulási webszolgáltatásokhoz](https://services.azureml.net/quickstart) portálra a Microsoft Azure-fiókjával – használja a fiókot, amely az Azure előfizetéshez van társítva.
2.  A menüben kattintson a **Webes szolgáltatások**.

Ez az előfizetéshez tartozó telepített webszolgáltatásokhoz listáját jeleníti meg. 

Webszolgáltatás kezeléséhez kattintson a webes szolgáltatások. A webes szolgáltatások lapon a következőkre van lehetősége:

- Kattintson a webszolgáltatás történő kezeléséhez.
- Kattintson a számlázás megtervezése a webszolgáltatás frissítéséhez.
- Törlése egy webszolgáltatásból.
- Másolja a webszolgáltatás, és üzembe egy másik területére.

Amikor a webszolgáltatás gombra kattint, a szolgáltatás quickstart útmutató lap nyílik meg. A szolgáltatás quickstart útmutató lap menü lehetőségei, amelyek lehetővé teszik a webszolgáltatás kezelése foglalja magában:

- **IRÁNYÍTÓPULT** - megtekintése, webes szolgáltatás használatát teszi lehetővé.
- **KONFIGURÁLÁSA** - leíró szöveget, frissítse a kulcsot a tárterület-fiókom a webszolgáltatás társított és engedélyezése vagy letiltása a mintaadatok teszi lehetővé.

### <a name="monitoring-how-the-web-service-is-being-used"></a>A webszolgáltatás használatának figyelése ###

Kattintson az **IRÁNYÍTÓPULT** fülre.

Az irányítópult lapon megtekintheti a webszolgáltatás általános használatát egy időszakra vetített állapotával. Kattintson a időszakokat tartalmazó legördülő menü jobb felső sarokban a használatát diagramok megtekintése az időszak kijelölhet. Az irányítópult jeleníti meg az alábbi adatokat:

- **Kérések fölé idő** lépés grafikonhoz kérések száma a kijelölt időszakban jeleníti meg. Segítheti a azonosítása, ha a használat kiugrásainak megfelelő tapasztalja.
- **Kérés-válasz kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány sikertelen kérés-válasz hívások száma.
- **Átlagos kérelem-számítja ki válaszidő** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Köteg kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány nem sikerült köteg kérések száma.
- **Átlagos feladat-időtartam** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Hibák** a hívások átirányítása a webszolgáltatás jeleníti meg, hogy van-e hibák összesített száma.
- **Szolgáltatások költségeket** jeleníti meg a számlázási csomagot, a szolgáltatásokhoz kapcsolódó díjai.

### <a name="configuring-the-web-service"></a>A webszolgáltatás beállítása ###

Kattintson a **KONFIGURÁLÁS** menü lehetőségre.

A következő tulajdonságokat módosíthatja:

* **Leírást** adjon meg egy leírást a webszolgáltatás teszi lehetővé.
* **Cím** lehetővé teszi a webszolgáltatás címének megadása
* **Billentyűparancsok** az elsődleges és másodlagos API-kulcsok elforgatása teszi lehetővé.
* **Tárterület fiókkulcs** frissítse a kulcsot a tárterület-fiókom a webes szolgáltatásváltozásokról társított teszi lehetővé. 
* **Mintaadatok engedélyezése** lehetővé teszi, hogy meg kell adnia a mintaadatokat, tesztelje a kérelem-válasz szolgáltatás használható. Ha a webszolgáltatás gépi tanulási Studio hozta létre, a mintaadatok kérdezi le az adatokat a modell betanítása a használt. A szolgáltatás programozás útján hozta létre, ha az adatok forrása a mintaadatokat, a JSON-csomag részeként megadott.

### <a name="managing-billing-plans"></a>Számlázási csomagok kezelése ###

Kattintson a weblapról szolgáltatások quickstart útmutató a **tervek** menü lehetőségre. A terv adott webszolgáltatás társított kezelése, hogy a terv is kattinthat.

* **Új** lehetővé teszi, hogy hozzon létre egy új csomag.
* **Hozzáadás/Eltávolítás terv példány** "méretezése" kapacitás felvenni egy meglévő terv teszi lehetővé.
* **Frissítés/visszalépéssel kapcsolatos** "szerkezetének kialakítása" kapacitás felvenni egy meglévő terv teszi lehetővé.
* **Törlése** lehetővé teszi a terv törlése.

Kattintson az irányítópult megtekintése egy tervre. Az irányítópult lépve pillanatfelvétel vagy terv használatát egy kijelölt időszakra vetített állapotával. Az időszak megtekintéséhez kijelöléséhez kattintson az **időszak** legördülő menü a képernyő jobb felső sarkában irányítópult. 

Az csomagra Irányítópulton az alábbi információk találhatók:

* **Terv leírás** a költségek és a terv társított kapacitás információkat jelenít meg.
* **Terv használatát** a tranzakciók és a terv ellen alapdíjával számítási órák számát jeleníti meg.
* **Webszolgáltatások** webszolgáltatásokhoz használják a terv számát jeleníti meg.
* **Felső webes szolgáltatás által hívások** vannak híváskezdeményezési szemben a terv alapdíjával felső négy webes szolgáltatások jeleníti meg.
* **Felső webszolgáltatásokhoz által óra számítja ki** a felső négy webszolgáltatásokhoz használják a terv ellen alapdíjával számítási erőforrások jeleníti meg.

## <a name="manage-classic-web-services"></a>Klasszikus webes szolgáltatások kezelése

> [AZURE.NOTE] Ez a szakasz lépéseit szempontjából fontos klasszikus webszolgáltatásokhoz keresztül az Azure gépi tanulási Web Services portál kezelése. Kezelésével kapcsolatos tudnivalók a gépi tanulási Studio és az Azure klasszikus portálon keresztül klasszikus webszolgáltatásokhoz a [az Azure gépi tanulási munkaterület kezelése](machine-learning-manage-workspace.md)című cikkben talál.

A klasszikus webes szolgáltatások kezelése:

1.  Jelentkezzen be a [Microsoft Azure gépi tanulási webszolgáltatásokhoz](https://services.azureml.net/quickstart) portálra a Microsoft Azure-fiókjával – használja a fiókot, amely az Azure előfizetéshez van társítva.
2.  A menüben kattintson a **Klasszikus webszolgáltatásokhoz**.

Klasszikus webszolgáltatás kezeléséhez kattintson a **Klasszikus webszolgáltatásokhoz**. A klasszikus webszolgáltatásokhoz lapon a következőkre van lehetősége:

- Kattintson a webes szolgáltatással társított végpontjait megtekintheti.
- Törlése egy webszolgáltatásból.

Klasszikus webszolgáltatás felügyeletét látja el, ha Ön kezeli minden, a végpontok külön-külön. Amikor a webes szolgáltatások lap webszolgáltatás gombra kattint, a listában, a szolgáltatásokhoz kapcsolódó végpontok nyílik meg. 

A klasszikus webszolgáltatás végponttípust lapon hozzáadása, és törölje a szolgáltatást a végpontok. Végpontok hozzáadásával kapcsolatos további tudnivalókért olvassa el a [Végpontok létrehozása](machine-learning-create-endpoint.md)című témakört.

Kattintson a szolgáltatás quickstart útmutató weblap megnyitása a végpontok. Quickstart útmutató lapon két lehetőség menü, amelyek lehetővé teszik a webszolgáltatás kezelése:

- **IRÁNYÍTÓPULT** - megtekintése, webes szolgáltatás használatát teszi lehetővé.
- **KONFIGURÁLÁSA** - lehetővé teszi leíró szöveget, kapcsolja be a hibák naplózása be- és kikapcsolása, frissítés, a kulcs tárolására szolgáló a webszolgáltatás társított fiók engedélyezése és letiltása a mintaadatokat.

### <a name="monitoring-how-the-web-service-is-being-used"></a>A webszolgáltatás használatának figyelése ###

Kattintson az **IRÁNYÍTÓPULT** fülre.

Az irányítópult lapon megtekintheti a webszolgáltatás általános használatát egy időszakra vetített állapotával. Kattintson a időszakokat tartalmazó legördülő menü jobb felső sarokban a használatát diagramok megtekintése az időszak kijelölhet. Az irányítópult jeleníti meg az alábbi adatokat:

- **Kérések fölé idő** lépés grafikonhoz kérések száma a kijelölt időszakban jeleníti meg. Segítheti a azonosítása, ha a használat kiugrásainak megfelelő tapasztalja.
- **Kérés-válasz kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány sikertelen kérés-válasz hívások száma.
- **Átlagos kérelem-számítja ki válaszidő** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Köteg kérések** jeleníti meg a szolgáltatás megkapta az adott időszakra és őket, hogy hány nem sikerült köteg kérések száma.
- **Átlagos feladat-időtartam** a kapott kérések végrehajtásához szükséges időt átlagát jeleníti meg.
- **Hibák** a hívások átirányítása a webszolgáltatás jeleníti meg, hogy van-e hibák összesített száma.
- **Szolgáltatások költségeket** jeleníti meg a számlázási csomagot, a szolgáltatásokhoz kapcsolódó díjai.

### <a name="configuring-the-web-service"></a>A webszolgáltatás beállítása ###

Kattintson a **KONFIGURÁLÁS** menü lehetőségre.

A következő tulajdonságokat módosíthatja:

* **Leírást** adjon meg egy leírást a webszolgáltatás teszi lehetővé. Leírás megadása kötelező.
* **Naplózás** lehetővé teszi engedélyezése vagy letiltása a hiba jelentkezik be a végpontot. További információt a naplózást olvassa el a engedélyezése [gépi tanulási webszolgáltatásokhoz naplózás](machine-learning-web-services-logging.md)című témakört.
* **Mintaadatok engedélyezése** lehetővé teszi, hogy meg kell adnia a mintaadatokat, tesztelje a kérelem-válasz szolgáltatás használható. Ha a webszolgáltatás gépi tanulási Studio hozta létre, a mintaadatok kérdezi le az adatokat a modell betanítása a használt. A szolgáltatás programozás útján hozta létre, ha az adatok forrása a mintaadatokat, a JSON-csomag részeként megadott.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Engedélyezheti vagy felfüggesztése webszolgáltatásokhoz felhasználók számára a hozzáférést a portálon

Az Azure klasszikus portál használatával, engedélyezi, vagy megtagadja a hozzáférést az adott felhasználókhoz.

### <a name="access-for-users-of-new-web-services"></a>Új webes szolgáltatások felhasználók hozzáférésének

Ahhoz, hogy más felhasználók számára a webes szolgáltatások az Azure gépi tanulási Web Services portál használata, hozzá kell adnia őket, további-rendszergazdák az Azure-előfizetésben.

Jelentkezzen be a Microsoft Azure-fiók használatával [Azure klasszikus portál](https://manage.windowsazure.com/) – használja a fiókot, amely az Azure előfizetéshez van társítva.

1. A navigációs ablakban kattintson a **Beállítások**gombra, majd kattintson a **rendszergazdák**elemre.
2. Az ablak alján kattintson a **Hozzáadás**gombra. 
3. Hozzáadás A közös rendszergazda párbeszédpanelen írja be annak a személynek, vegye fel a közös közé, és válassza ki az előfizetést, amelyhez a közös rendszergazda eléréséhez az e-mail címet.
4. Kattintson a **Mentés**gombra.

### <a name="access-for-users-of-classic-web-services"></a>Klasszikus webszolgáltatások felhasználók hozzáférésének

Munkaterület kezelése:

Jelentkezzen be a Microsoft Azure-fiók használatával [Azure klasszikus portál](https://manage.windowsazure.com/) – használja a fiókot, amely az Azure előfizetéshez van társítva.

1. A Microsoft Azure szolgáltatások panelen kattintson a **Gépi TANULÁSI**.
1. Kattintson a munkaterület kezelni szeretné.
1. Kattintson a **beállítás** fülre.

A konfiguráció lapon a gépi tanulási munkaterületre való hozzáférést is felfüggesztése **MEGTAGADÁS**gombra kattintva. Felhasználók már nem tudja megnyitni a munkaterületet gépi tanulási Studio alkalmazásban. Az access visszaállításához kattintson az **Engedélyezés**gombra.

Az adott felhasználókhoz:

További fiókok a munkaterület gépi tanulási Studio hozzáféréssel rendelkező kezeléséhez kattintson **bejelentkezni a Machine Learning Studio** az **IRÁNYÍTÓPULT** lapon. Ekkor megnyílik a munkaterület gépi tanulási Studio alkalmazásban. Itt kattintson a **Beállítások** fülre, majd a **felhasználók**. Választhatja a **További felhasználók MEGHÍVÁSA** a felhasználók hozzáférésének a munkaterületre, vagy válassza ki azt a felhasználót, és kattintson az **ELTÁVOLÍTÁS**gombra.

> [AZURE.NOTE] A **bejelentkezési kísérleteket Machine Learning Studio** hivatkozás nyitja meg, hogy be van jelentkezve a Microsoft-Account gépi tanulási Studio. Jelentkezzen be az Azure klasszikus portálra munkaterület létrehozásához használt Microsoft-Account automatikusan nincs engedélye, hogy a munkaterület megnyitása. A munkaterület megnyitása, kell bejelentkeznie Microsoft-Account a munkaterület tulajdonosaként megadott vagy meghívót kap a tulajdonos, ha be szeretne kapcsolódni a munkaterület kell.
