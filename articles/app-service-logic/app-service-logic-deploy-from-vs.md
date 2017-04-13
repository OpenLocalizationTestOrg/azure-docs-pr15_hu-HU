<properties 
    pageTitle="Logika alkalmazások Visual Studio build |} Microsoft Azure" 
    description="Projekt létrehozása a Visual Studióban hozhat létre, és telepítse az logika alkalmazást." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Építse fel és telepítse az Visual Studio összefüggés-alkalmazások

Bár az [Azure-portálon](https://portal.azure.com/) lépve egyszerűen tervezhet és kezelheti logika alkalmazásait, előfordulhat, hogy is érdemes tervezése és üzembe inkább a Visual Studio a logika alkalmazás.  Logika alkalmazások gazdag Visual Studio toolset, amely lehetővé teszi, hogy egy logikai alkalmazást a Tervező összeállítása, és konfigurálása a megfelelő telepítési telepített és automatizálási sablont, és bármelyik környezetbe üzembe megtalálható.  

## <a name="installation-steps"></a>Telepítési lépései

Alatti lépések, telepítse és állítsa be a Visual Studio eszközök-logika alkalmazást.

### <a name="prerequisites"></a>Előfeltételek

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Legújabb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Az Access a beágyazott tervező használata esetén a weben

### <a name="install-visual-studio-tools-for-logic-apps"></a>Visual Studio eszközök összefüggés-alkalmazások telepítése

Ha már van telepítve, a előfeltételekről 

1. Nyissa meg a Visual Studio 2015 az **eszközök** menüre, és válassza a **bővítmények és frissítések**
1. Válassza az **Online** kategóriát keresése az interneten
1. Jelenítse meg a **Azure logika alkalmazások Tools for Visual Studio** **Logika alkalmazások** keresése
1. A **Letöltés** gombra kattintva töltse le és telepítse a bővítmény
1. Telepítése után indítsa újra a Visual Studio

> [AZURE.NOTE] A bővítmény töltse le [ezt a hivatkozást](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9) közvetlenül

Miután telepített fogja tudni az Azure erőforráscsoport használhatja a logika alkalmazás tervezővel.

## <a name="create-a-project"></a>Projekt létrehozása

1. Nyissa meg a **fájl** menüre, és válassza az **Új** >  **Project** (vagy válassza a **Hozzáadás gombra** , és válassza az **Új projekt** fel szeretne venni egy meglévő megoldás):  ![fájl menüje](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. A párbeszédpanelen keresse meg a **felhőben**, és válassza az **Azure erőforráscsoport**. Írjon be egy **nevet** , és kattintson **az OK**gombra.
    ![Új projekt hozzáadása](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Jelölje ki a **logika** alkalmazássablon. Egy üres logika alkalmazássablon telepítési induljon ez hoz létre.
    ![Jelölje ki az Azure-sablon](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Miután kiválasztotta a **sablont**, kattintson az **OK gombra**.

    Most már a logika alkalmazás projekt bekerül a megoldás. Meg kell jelennie a telepítési fájlt az megoldás Explorer:  

    ![Telepítési](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>A logikai alkalmazás Designer alkalmazással

Ha befejezte az Azure erőforráscsoport projektnél logika alkalmazást tartalmaz, a Visual Studio, amely segít a munkafolyamat létrehozása a Tervező is megnyithatja.  A Tervező folyamatos internetkapcsolatra annak érdekében, hogy az összekötők elérhető tulajdonságok és az adatok a lekérdezés (például a Dynamics CRM Online-összekötő használata esetén a tervező lesz a lekérdezés a rendelkezésre álló egyéni és az alapértelmezett tulajdonságainak megjelenítéséhez CRM-példány).

1. Kattintson a jobb gombbal a `<template>.json` fájl, és válassza a **Megnyitás logika alkalmazás tervezővel** (vagy `Ctrl+L`)
1. Válassza az előfizetés, erőforráscsoport és helyét a telepítési sablon
    - Fontos tudni, hogy egy logikai alkalmazás tervezése hoz létre **API-kapcsolat** erőforrások tervezés során tulajdonságainak lekérdezésére.  Az erőforráscsoport kijelölve a tervezés során ezeket a kapcsolatok létrehozásához használt erőforráscsoport lesz.  Megtekintheti vagy minden API-kapcsolatot módosítani az Azure-portálra, és az **API-kapcsolatot**böngészési.
    ![Előfizetés-választó](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. A Tervező megjelenítik alapján meghatározása a `<template>.json` fájlt.
1. Most már létrehozhat és tervezése a logika alkalmazást, és módosításokat frissülnek a telepítési sablonban.
    ![A Visual Studióban tervezőben](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Akkor is látható `Microsoft.Web/connections` hozzáadódjanak az erőforrásfájl bármely kapcsolatok függvény logika alkalmazáshoz szükséges források.  A kapcsolat tulajdonságai állítja be, amikor rendszerbe, és felügyelt **API-kapcsolatokat** az Azure-portálon telepítését követően.

### <a name="switching-to-the-json-code-view"></a>A JSON-kód-nézetre váltás

A **Kódnézet** lap alján a Tervező szeretne váltani a logika alkalmazás JSON ábrázolása kijelölhet.  Ha vissza szeretne váltani a teljes erőforrás JSON, kattintson a jobb gombbal a `<template>.json` fájlt, és válassza a **Megnyitás**.

### <a name="saving-the-logic-app"></a>A logikai alkalmazás mentése

Mentheti a logika alkalmazást bármikor keresztül a **Mentés** gombra, vagy `Ctrl+S`.  Ha mentette időben a hibák az összefüggés-alkalmazást, azok jelennek meg a Visual Studio a **kimeneti értékeket** ablakban.

## <a name="deploying-your-logic-app"></a>A logikai alkalmazás telepítése

Végül Miután beállította az alkalmazást, telepítheti a közvetlenül a Visual Studio mindössze néhány lépésben. 

1. Kattintson a jobb gombbal az megoldás Explorer a projekten, és válassza a **központi telepítés** > **Új telepítési...** 
     ![Új telepítési](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Jelentkezzen be az Azure előfizetés(ek) kéri. 

3. Most már kell válassza a részletek az erőforráscsoport, amelyet a logika alkalmazás telepítéséhez. 
    ![Erőforráscsoport telepítése](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Mindenképpen jelölje ki a megfelelő sablont, és a paraméterek (például ha munkakörnyezetben telepít bizonyára tudni szeretné, hogy válassza a termelési paraméterek fájl) az erőforráscsoport fájlokat. 
4. A központi telepítés gombbal
 
    
6. A telepítés állapotának megjelenik a **kimeneti** ablakban (Előfordulhat, válassza az **Azure kiépítési**. 
    ![Kimenet](./media/app-service-logic-deploy-from-vs/output.png)

A jövőben módosítása a verziókövetés logika alkalmazását, és új verzió telepítése a Visual Studio segítségével. 

> [AZURE.NOTE] Ha közvetlenül a definíció az Azure-portálon módosítja, a következő alkalommal, amikor rendszerbe Visual Studio ezekhez a változtatásokhoz írható felül.

## <a name="next-steps"></a>Következő lépések

- Első lépések logika alkalmazások, kövesse az [összefüggés-alkalmazás létrehozása](app-service-logic-create-a-logic-app.md) oktatóprogram.  
- [Nézet hétköznapi példát és -esetek](app-service-logic-examples-and-scenarios.md)
- [Üzleti folyamatok logika alkalmazások automatizálható](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Megtudhatja, hogy miként rendszerek integrálása összefüggés-alkalmazások](http://channel9.msdn.com/Events/Build/2016/P462)