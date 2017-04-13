<properties 
    pageTitle="Azure portál használatával Azure erőforrások |} Microsoft Azure" 
    description="Az erőforrások Azure-portál és -Azure erőforrás kezeléséhez használja." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Erőforrás-kezelő sablonok és Azure portál erőforrások terjesztése

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [REST API-VAL](resource-group-template-deploy-rest.md)

Ez a témakör bemutatja, hogyan [Azure erőforrás-kezelő](azure-resource-manager/resource-group-overview.md) az Azure erőforrások terjesztése az [Azure portál](https://portal.azure.com) használata. Az erőforrások kezelésével kapcsolatos további tudnivalókért lásd: [kezelése Azure erőforrások portálon keresztül](./azure-portal/resource-group-portal.md).

Jelenleg nem minden szolgáltatás lehetővé teszi, a portálra vagy az erőforrás-kezelő. A szolgáltatások akkor a [Klasszikus portálon](https://manage.windowsazure.com)használja. Egyes szolgáltatások állapotát a [Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/)című témakör tartalmaz.

## <a name="create-resource-group"></a>Erőforrás-csoport létrehozása

1. Hozzon létre egy üres erőforráscsoport, válassza az **Új** > **Management** > **Erőforráscsoport**.

    ![üres erőforrás csoport létrehozása](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Adjon meg nevet és helyet, és ha szükséges, jelöljön ki egy előfizetést. Meg kell adnia az erőforráscsoport helyét, mivel az erőforráscsoport tárolja a metaadatokat az erőforrásokról. Megfelelőségi okok miatt előfordulhat, hogy szeretne megadni, hogy a metaadatok tároló. Általánosságban elmondható azt javasoljuk, hogy meg egy helyet, akkor a legtöbb az erőforrások tároló. Használata ugyanazon a helyen egyszerűbbé teszi a sablont.

    ![csoport értékeivel](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Erőforrások piactérről terjesztése

Miután létrehozott egy erőforráscsoport, a piactérről erőforrások telepítheti azt. A piactér előre definiált megoldások nyújt a gyakori alkalmazási területek.

1. A telepítés elindításához jelölje be az **Új** , és telepíteni szeretné az erőforrás típusa. Ezután keresse meg a telepíteni kívánt erőforrás adott verziójában.

    ![erőforrás terjesztése](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Ha nem látható az adott megoldást üzembe szeretne, kereshet a piactér azt.

    ![Keresés piactér](./media/resource-group-template-deploy-portal/search-resource.png)

3. A kijelölt erőforrás típusától függően van megfelelő tulajdonságainak beállítása a telepítés előtt gyűjteménye. Ezeket a beállításokat nem láthatók: Ebben az esetben, változik erőforrástípus alapján Az összes ki kell választania a cél erőforráscsoport. Az alábbi képen látható, hogyan hozhat létre webalkalmazást, és a létrehozott erőforráscsoport beállítaná őket.

    ![erőforrás-csoport létrehozása](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Azt is megteheti eldöntheti, az erőforrás csoport létrehozásához, az erőforrások telepítésekor. Jelölje be az **Új létrehozása** , és adjon nevet az erőforrás-csoportnak.

    ![Új erőforráscsoport létrehozása](./media/resource-group-template-deploy-portal/select-new-group.png)

4. A telepítési kezdődik. A telepítési néhány perc is eltelhet. A telepítésének befejeződése után megjelenik egy értesítés.

    ![értesítés megtekintése](./media/resource-group-template-deploy-portal/view-notification.png)

5. Miután az erőforrások üzembe, vehet további források az erőforráscsoport parancsával a **Hozzáadás** az erőforrás csoport lap.

    ![erőforrás hozzáadása](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Egyéni sablonból erőforrások terjesztése

Ha azt szeretné, hajtsa végre a telepítést, de nem használja a sablonok a piactér, létrehozhat egy egyéni sablont, amely definiálja az infrastruktúra a megoldás. Sablonok létrehozásával kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md).

1. Szeretné üzembe helyezni egy testre szabott sablont a portálon keresztül, és válassza az **új, indítsa el a keresést **Sablon** telepítéshez, amíg a lehetőségek közül választhat.**

    ![Keresés a sablon telepítésének](./media/resource-group-template-deploy-portal/search-template.png)

2. Jelölje ki **a sablon telepítésének** a rendelkezésre álló erőforrásokat.

    ![Jelölje be a sablon telepítésének](./media/resource-group-template-deploy-portal/select-template.png)

3. Után a sablon példányban indítása, nyissa meg az üres sablon testreszabásához elérhető.

    ![sablon létrehozása](./media/resource-group-template-deploy-portal/show-custom-template.png)

    Szerkesztőben adja hozzá a JSON szintaxist, amely definiálja az erőforrások szeretné telepíteni. Válassza a **Mentés** , amikor a Kész gombra. A JSON szintaxis írási című témakörben olvashat [erőforrás-kezelő sablon forgatókönyv](resource-manager-template-walkthrough.md).

    ![sablon szerkesztése](./media/resource-group-template-deploy-portal/edit-template.png)

4. Vagy egy már meglévő sablon az [Azure quickstart útmutató sablonok](https://azure.microsoft.com/documentation/templates/)közül választhat. Ezek a sablonok a Közösség közzé. Számos, gyakori alkalmazási területek terjed ki, és valaki hozzáadta egy sablont, amely hasonlít az, hogy mi próbálja telepíteni. A sablonok szeretne megtalálni valamit, amely megfelel az igényektől is kereshet.

    ![quickstart útmutató sablon kiválasztása](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    A kiválasztott sablont megtekintheti a szerkesztőben.

5. Minden más érték beírása, után válassza a **Létrehozás** bevezetését tervezi a sablon lehetőséget. 

    ![sablon terjesztése](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Források a fiókjába mentett sablonból terjesztése

A portál lehetővé teszi, hogy mentse a sablont az Azure-fiókjába, és telepítsen újra később. Ezen mentett sablonok, az [első lépések az Azure-portálra a személyes sablonok](./marketplace-consumer/mytemplates-getstarted.md)használatáról további információt.

1. A mentett sablonok kereséséhez válassza a **Tallózás** > **sablonokat**.

    ![Tallózás a sablonok között](./media/resource-group-template-deploy-portal/browse-templates.png)

2. A fiókjába mentett sablonok a listából jelölje ki a dolgozni szeretne.

    ![mentett sablonok](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Jelölje ki a **központi telepítés** újratelepítenie a mentett sablont.

    ![mentett sablont terjesztése](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Következő lépések

- Megtekintheti a naplókat, olvassa el [az erőforrás-kezelő műveletek naplózási](resource-group-audit.md).
- Telepítési hibáinak elhárítása, olvassa el a [Hibaelhárítás erőforrás csoport telepítések Azure Portal segítségével](resource-manager-troubleshoot-deployments-portal.md)című témakört.
- Sablon visszakeresése a telepítő vagy erőforráscsoport, olvassa el [a meglévő erőforrásoktól exportálása Azure erőforrás-kezelő sablont](resource-manager-export-template.md).





