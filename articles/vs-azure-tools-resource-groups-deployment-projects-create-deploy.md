<properties
   pageTitle="Azure erőforrás csoportjának Visual Studio projektek |} Microsoft Azure"
   description="Visual Studio segítségével Azure erőforrás projektek létrehozása és telepítése az erőforrások Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Hozhat létre és üzembe helyezése a Visual Studio segítségével Azure az erőforrás-csoportok

A Visual Studio és az [Azure SDK](https://azure.microsoft.com/downloads/)létrehozhat egy projekt üzembe helyezése a infrastruktúra és kódot az Azure. Például a webes host, a webhely és az adatbázis megadása az alkalmazás, és telepítsen, hogy a kóddal együtt. Vagy egy virtuális gép virtuális és hálózati tároló fiók megadása, és együtt virtuális gépen végrehajtott parancsfájlt, hogy infrastruktúrát üzembe. Az **Azure erőforráscsoport** telepítés projekthez lehetővé teszi a szükséges erőforrások egyetlen, megismételhető művelet telepítése. Telepítésével és az erőforrások kezelésével kapcsolatos további tudnivalókért olvassa el a [Azure erőforrás-kezelő – áttekintés](azure-resource-manager/resource-group-overview.md)című témakört.

Azure erőforráscsoport projektek Azure erőforrás-kezelő JSON sablonok, amely meghatározza az erőforrásokat, Azure rendszerbe tartalmazzák. Az erőforrás-kezelő sablon elemeivel kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md). Visual Studio lehetővé teszi, hogy ezek a sablonok szerkesztése és eszközeivel egyszerűbb sablonok használata.

Ez a témakör az üzembe a web app és az SQL-adatbázishoz. Jó helyen jár a lépések megegyeznek szinte bármilyen típusú erőforrás. Egyszerűen telepítheti egy virtuális gép és a kapcsolódó erőforrásokat. Visual Studio számos különböző starter sablon nyújt a gyakori alkalmazási területek telepítésével.

Ez a cikk a .NET 2.9 Visual Studio 2015 frissítés 2 és a Microsoft Azure SDK jeleníti meg. Visual Studio 2013 az Azure SDK 2.9 használja, ha a felhasználói felület nagyjából ugyanúgy történik. Használhatja az Azure SDK 2.6 vagy újabb; verziója azonban a felhasználói felület, a felhasználói felület eltérhetnek a jelen cikkben felsorolt kezelőfelülete. Javasoljuk, hogy a lépések megkezdése előtt telepítse az [Azure SDK](https://azure.microsoft.com/downloads/) legújabb verzióját. 

## <a name="create-azure-resource-group-project"></a>Projekt létrehozása Azure erőforráscsoport

Ezt az eljárást hozza létre az Azure erőforráscsoport project **Web app + SQL** sablonnal.

1. A Visual Studióban, válassza a **fájl**, az **Új projekt**, válassza a **C#** vagy a **Visual Basic**. Válassza a **felhőben**, és válassza az **Azure erőforráscsoport** projekt.

    ![Felhőalapú telepítés projekthez](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Válassza a az Azure erőforrás-kezelő telepítéshez használni kívánt sablont. Értesítés ott a telepítéshez használni kívánt project függően sok más lehetőséget. Ez a témakör a **Web app + SQL** -sablon lehetőség közül.

    ![Sablon kiválasztása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    A sablont, akkor válassza ki az imént a kiindulási pont; felvehet, és távolítsa el az igényektől teljesítéséhez erőforrást.

    >[AZURE.NOTE] Visual Studio olvassa be az online elérhető sablonok listáját. A lista változhat.

    Visual Studio a web App alkalmazásban és az SQL-adatbázis erőforrás telepítési projektek hoz létre.

1. Milyen létrehozott megtekintéséhez bontsa ki a telepítési projekt csomópontját.

    ![csomópontok megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Mivel választottunk a Web app + ebben a példában az SQL-sablont, a következő fájlok jelenik meg: 

  	|Fájlnév|Leírás|
  	|---|---|
  	|Üzembe AzureResourceGroup.ps1|Egy PowerShell-parancsprogramot, amely elindítja az Azure erőforrás-kezelő üzembe PowerShell-parancsait.<br />**Megjegyzés:** Visual Studio a PowerShell-parancsprogramot használja a sablon telepítése. Ez a parancsfájl végzett módosítások befolyásolja a Visual Studio környezetben, így ügyeljen arra, hogy.|
  	|WebSiteSQLDatabase.json|Az erőforrás-kezelő sablont, amely definiálja az infrastruktúra szeretné üzembe Azure, és a paraméterek biztosíthat a telepítés során. Is az erőforrásokat, az erőforrás-kezelő üzembe helyezése a megfelelő sorrendben az erőforrások közötti függőségek határozza meg.|
  	|WebSiteSQLDatabase.parameters.json|Szükség szerint a sablon értékeket tartalmazó paraméterek fájlt. Adja meg az egyes telepítési testreszabása paraméterértékeket.|

    Összes erőforrás csoport telepítési projekt tartalmaznak, ezek a fájlok egyszerű. Más projektekben egyéb funkciók támogatása további fájlokat tartalmazhatnak.

## <a name="customize-the-resource-manager-template"></a>Az erőforrás-kezelő sablon testreszabása

Telepítési projekt, amely jellemzi a telepíteni kívánt erőforrások a JSON-sablonok módosításával is testre szabhatja. JSON JavaScript Objektumjelrendszer rövidítése, és könnyen használata szerializált adatformátum. A JSON-fájlokat a minden fájl tetején hivatkozó séma használata Ha azt szeretné, a séma megértéséhez, letöltheti és elemzés. A séma határozza meg, hogy milyen elemeket is érvényes, a fájltípusok és formátumok mezők, a lehetséges értékek sorszámozott értékeket, és így tovább. Az erőforrás-kezelő sablon elemeivel kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md).

A sablon dolgozik, nyissa meg a **WebSiteSQLDatabase.json**.

A Visual Studio-szerkesztő nyújt segítséget nyújtanak az erőforrás-kezelő sablon szerkesztése a eszközöket. A **JSON** Vázlatablak megkönnyíti a sablonba definiált részeket láthassák.

![Vázlat JSON megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Az elemek közül a tagolás megnyitja azt a részét a sablon emeli ki a megfelelő JSON.

![Nyissa meg a JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Az erőforrások hozzáadása vagy az **Erőforrás hozzáadása** gombra kattintva a JSON körvonala ablak tetején, vagy kattintson a jobb gombbal az **erőforrásokat** , és válassza az **Új erőforrás hozzáadása**.

![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Ebben az oktatóanyagban jelölje ki a **Tárterület-fiókot** , és adja meg egy nevet. Adjon egy nevet, 11-nél több karakterből áll, amely csak a számokat és betűket kisbetűk tartalmaz.

![Tárhely hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Figyelje meg, hogy nem csak a az erőforrás hozzá lett, hanem a típus tároló fiók paraméter, és a tárterület-fiók nevének változó.

![Vázlat megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

A **storageType** paraméter előre definiált engedélyezett típusok és alapértelmezett típusát. Hagyja ezeket az értékeket, és szerkesztheti az Ön esetében. Ha nem szeretné, hogy mindenki számára egy **Premium_LRS** tárterület-fiókot – ezzel a sablonnal üzembe, eltávolítása a megengedett típusú. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio is tartalmaz az intellisense útmutatóból megtudhatja, hogy milyen tulajdonságok érhetők el a sablon szerkesztése során. Például a alkalmazás díjcsomagjától tulajdonságainak szerkesztéséhez nyissa meg azt a **HostingPlan** erőforrás, és adjon hozzá egy értéket az a **tulajdonságokat**. Figyelje meg, hogy az intellisense elérhető értékek láthatók, és ismerteti, ezt az értéket.

![az intellisense megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Beállíthatja, hogy **numberOfWorkers** 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Telepítse az erőforráscsoport projektet az Azure

Most már készen áll a projekt telepítéséhez. Azure erőforráscsoport projektben telepítésekor beállítaná őket az Azure erőforráscsoport. Az erőforráscsoport információforrások, amelyek egy közös életciklus megosztása logikai csoportja.

1. Válassza a helyi menü a telepítés projekt csomópontjának **Deploy** > **Új telepítési**.

    ![Üzembe helyezéséhez új telepítési menüpont](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Az **erőforrás csoporthoz Deploy** párbeszédpanel jelenik meg.

    ![Erőforráscsoport párbeszédpanel terjesztése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Az **erőforrás csoport** legördülő listában válassza a meglévő erőforráscsoport, vagy hozzon létre egy újat. Hozzon létre egy erőforrás csoportot, nyissa meg az **Erőforrás csoport** legördülő listában, és válassza az **Új létrehozása**.

    ![Erőforráscsoport párbeszédpanel terjesztése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Megjelenik az **Erőforráscsoport létrehozása** párbeszédpanel. Adjon meg a csoport nevét és helyét, és kattintson a **Létrehozás** gombra.

    ![Erőforráscsoport párbeszédpanel létrehozása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. A paraméterek a telepítéshez szerkesztése a **Szerkesztése paramétereinek** gombra kattintva.

    ![Szerkesztése paramétereinek gomb](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Adja meg az értékeket az üres paraméterek, és válassza a **Mentés** gombra. Az üres paramétereket **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**és **adatbázisnév**.

    **hostingPlanName** adja meg egy nevet az [alkalmazás szolgáltatáscsomagja](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) hozhat létre. 
    
    **administratorLogin** megadja a felhasználó nevét, az SQL Server-rendszergazdák számára. Ne használjon közös felügyeleti nevét, például a **rendszergazdai** vagy a **rendszergazda**. 
    
    A **administratorLoginPassword** megadja az SQL Server-rendszergazdai jelszót. A **jelszavak egyszerű szövegként a paraméterek fájl mentése** beállítás, nem biztonságos; Emiatt nem jelöli be ezt a beállítást. Egyszerű szövegként nem mentette a jelszót, mert szüksége lesz adja meg a telepítés során újra a jelszót. 
    
    **adatbázisnév** létrehozása az adatbázis nevét adja meg. 

    ![Szerkesztése paramétereinek párbeszédpanel](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. A **központi telepítés** gombra kattintva telepítse az projektet az Azure. Egy PowerShell-konzol kívül Visual Studio példány nyílik meg. A PowerShell konzolban, amikor a rendszer kéri, adja meg az SQL Server-rendszergazdai jelszavát. **A PowerShell konzol előfordulhat, hogy más elemek eltakarja, vagy a tálcán a kis méretre.** Keresse meg a konzol, és jelölje ki azt a adja meg a jelszót.

    >[AZURE.NOTE] Visual Studio Azure PowerShell-parancsmagok telepítését kérheti. Az Azure PowerShell-parancsmagok az erőforrás csoportok sikeres telepítéséhez szükséges. Ha a rendszer arra kéri, telepítse őket.
    
1. A telepítési néhány percig tart. A **kimeneti** ablakban a telepítés állapotának megtekintése. A telepítésének befejeződése után az utolsó üzenetek jelzik, hogy a sikeres környezet, amelyben az alábbihoz hasonló:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. A böngészőben nyissa meg az [Azure-portálra](https://portal.azure.com/) , és jelentkezzen be a fiókjába. Az erőforráscsoport megtekintéséhez kattintson az **erőforrás-csoportokat** , és az erőforráscsoport, hogy telepítette.

    ![Jelölje be a csoport](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Az összes telepített forrásokban talál. Figyelje meg, hogy a tárterület-fiók neve nem pontosan mi megadott fel az adott erőforrás. A tároló fiók egyedinek kell lennie. A sablon automatikusan hozzáadja a nevét, adjon meg egy egyedi nevet a megadott karakterlánc. 

    ![erőforrások megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Ha a módosításokat, és telepítsen újra a projekt, a meglévő erőforráscsoport válassza a helyi menüből Azure erőforrás csoport projekt szeretné. A helyi menüben válassza a **központi telepítés**, és válassza az erőforráscsoport telepítette.

    ![Azure erőforráscsoport rendszerbe](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Telepíteni az infrastruktúra kódot.

Ezen a ponton a infrastruktúra telepítette az alkalmazás, de nincs telepítve a projekt tényleges kód. Ez a témakör bemutatja a web app és az SQL-adatbázistáblák telepítéséről a telepítés során. Ha helyett egy webalkalmazás virtuális gép telepít, használni kívánt kód a számítógépen a telepítés részeként. A folyamat egy webalkalmazás vagy egy virtuális gép beállítása programkódot telepít szinte ugyanúgy történik.

1. Projekt hozzáadása a Visual Studio megoldáshoz. Kattintson a jobb gombbal a megoldást, és válassza a **Hozzáadás** > **Új projektet**.

    ![Adja hozzá a projekthez](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. **ASP.NET webalkalmazások**hozzáadása. 

    ![Adja hozzá a web App alkalmazásban](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Jelölje ki a **MVC** , és törölje a jelet a mező szolgáltatója **a felhőben** , mert az csoport projektet ezt a feladatot hajt végre.

    ![Jelölje ki a MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Miután Visual Studio hoz létre a web App alkalmazásban, látni mindkét projekteket a megoldást.

    ![projektek megjelenítéséhez](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Most akkor győződjön meg arról, hogy az erőforrás projektek tudatában az új projektet. Térjen vissza az erőforrás projektek (AzureResourceGroup1). Kattintson a jobb gombbal a **hivatkozást** , és válassza a **Hivatkozás hozzáadása**.

    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Jelölje ki a web app a project létrehozott.

    ![hivatkozás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    A hivatkozások hozzáadásával a web app a project az erőforrás csoport projekthez csatolni, és automatikusan a három fő tulajdonságainak beállítása. A **Tulajdonságok** ablak a hivatkozás tulajdonságokból látni.

      ![Lásd: a hivatkozás](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Az tulajdonságokat a következők:

    - A **További tulajdonságokat** a webes telepítőcsomag átmeneti tárolására, amely az Azure-tárolóhoz tolódik helyét tartalmazza. Megjegyzés: a mappa (ExampleApp) és a fájl (package.zip). Ezek az értékek nyújtanak paraméterként, az alkalmazás telepítésekor. 
    - **Olyan fájl elérési útja** tartalmaz, az elérési utat, ahol a csomag jön létre. A **Cél tartalmazza** a parancs végrehajtása telepítésének tartalmazza. 
    - Az alapértelmezett érték **összeállítása; Csomag** lehetővé teszi, hogy a telepítés létre, és hozzon létre egy webes telepítőcsomag (package.zip).  
    
    Ahogy a példányban kapja a szükséges információkat az a tulajdonságokat a csomag létrehozása szükségtelen közzétételi profil.
      
1. Erőforrás felvétele a sablont.

    ![erőforrás hozzáadása](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Ez esetben válassza a **Webhely Web Apps alkalmazások terjesztése**. 

    ![webhely hozzáadása terjesztése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Telepítsen újra az erőforrás projektek az erőforrás-csoporthoz. Ez esetben létezik néhány új paramétert. Nem kell a szükséges értékeket **_artifactsLocation** vagy **_artifactsLocationSasToken** , mert a Visual Studio automatikusan létrehozza a ezeket az értékeket. Jó helyen jár hogy meg a mappát, és a fájlnevet az elérési útját, hogy a telepítőcsomag ( **ExampleAppPackageFolder** és **ExampleAppPackageFileName** az alábbi képen látható) tartalma. Adja meg az értékeket a hivatkozás tulajdonságok (**ExampleApp** és **package.zip**) azt bemutattuk.

    ![webhely hozzáadása terjesztése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    A **eltérés tárterület-fiókot**jelölje ki a az erőforráscsoport rendszerbe.
    
1. A telepítés után, jelölje be a web App alkalmazásban a portálon. Jelölje be az URL-címet, tallózással keresse meg a webhelyet.

    ![Tallózással keresse meg a webhelyen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Figyelje meg, hogy telepítette az alapértelmezett ASP.NET alkalmazást.

    ![a telepített alkalmazás megjelenítése](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Következő lépések

- A portál és az erőforrások kezelése című témakörben talál [az Azure portálon kezelheti az Azure erőforrások](./azure-portal/resource-group-portal.md).
- Sablonok kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő szerzői sablonok](resource-group-authoring-templates.md).
