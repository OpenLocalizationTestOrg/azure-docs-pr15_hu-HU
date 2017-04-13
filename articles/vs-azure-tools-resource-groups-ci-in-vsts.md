<properties
    pageTitle="Folytonos-integrációt az Azure erőforráscsoport projektek VIEWBEN Team Services |} Microsoft Azure"
    description="Állítsa be a Visual Studio Team Services folyamatos integration Azure erőforráscsoport telepítési projektek használatával a Visual Studióban ismerteti."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>A Visual Studio Team Services Azure erőforráscsoport telepítési projektek folyamatos integrációja

Azure-sablon üzembe helyezéséhez, feladatokat kell elvégeznie különböző fázisaiban folyamatát: Azure másolatot fejlesztése, vizsgálat (más néven "Átmeneti"), és üzembe sablon.  Kétféleképpen különböző művelet Visual Studio Team Services (VIEWBEN csapat szolgáltatások). Mindkét módszer ugyanazt az eredményt adja, tehát azt, amely a leginkább megfelel a munkafolyamatot.

-   Egy lépésben hozzáadása az PowerShell-parancsprogramot, amely szerepel az Azure erőforráscsoport telepítési project (Deploy-AzureResourceGroup.ps1) futtató build definíciójának. A parancsfájl eltérések másolja, és ezután üzembe helyezése a sablont.
-   Adja hozzá a több VIEWBEN Team Services összeállítása lépések egyenként szakasz feladat elvégzéséhez.

Ez a cikk bemutatja, hogyan használhatja az első lehetőséget (build meghatározása a PowerShell-parancsprogramot futtatásához kell használni). Egy beállítás előnye, hogy a parancsfájl, Visual Studio fejlesztők használják a azonos parancsfájl VIEWBEN Team Services által használt. Ez az eljárás feltételezi, hogy már rendelkezik beadja VIEWBEN Team Services a Visual Studio környezetben projekt.

## <a name="copy-artifacts-to-azure"></a>Azure eltérések másolása 

Az alkalmazási példát, függetlenül minden sablon telepítéshez szükséges eltérések esetén szüksége lesz Azure erőforrás-kezelő hozzáférést adhat. Ezeket az eltéréseket például is elhelyezhet fájlokat:

-   Egymásba ágyazott sablonok
-   Konfigurációs és DSC parancsfájlok
-   Alkalmazás bináris

### <a name="nested-templates-and-configuration-scripts"></a>Egymásba ágyazott sablonok és konfigurációs parancsfájlok
A Visual Studio sablonjainak használata esetén (vagy Visual Studio kódrészletek épülő), a PowerShell-parancsprogramot, nem csak a eltérések új szakaszokat, azt is parameterizes a különböző környezetekben az erőforrások URI. A parancsfájl, majd másolja át az eltérések a biztonságos tárolót Azure-ban, létrehoz egy társítások token tároló, és átadja ezeket az információkat a sablon példányban alkalmazásba című témakör tartalmaz. Megtudhatja, ha többet szeretne megtudni az egymásba ágyazott sablonok [létrehozása a sablon telepítés](https://msdn.microsoft.com/library/azure/dn790564.aspx) lehetőséget.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Folytonos bevezetésének és csapat-szolgáltatások beállítása

Ha fel szeretne hívni a PowerShell-parancsprogramot VIEWBEN Team Services, frissítenie kell build definíciójának. Röviden a lépések a következők: 

1.  Az összeállítás definíciójának szerkesztése.
1.  Állítsa be a VIEWBEN Team Services Azure engedélyezési.
1.  Adjon hozzá egy Azure PowerShell build lépést az PowerShell parancsprogramot az Azure erőforráscsoport telepítési projekt hivatkozó.
1.  A beépített VIEWBEN Team Services projekt dolgozhat *– ArtifactsStagingDirectory* paraméter értékének beállítása

### <a name="detailed-walkthrough"></a>Részletes útmutató

Az alábbi lépésekkel végigvezeti a VIEWBEN Team Services folyamatos telepítésének konfigurálásához szükséges lépések 

1.  VIEWBEN Team Services build definíciójának szerkesztése, és adjon hozzá egy Azure PowerShell build lépést. Válassza az összeállítás definition **definíciók összeállítása** kategóriában, és válassza a hivatkozás **szerkesztése** .

    ![][0]

1.  Új **Azure PowerShell** build lépés hozzáadása az összeállítás definition, és válassza a **Hozzáadás összeállítása lépés...** gombra.

    ![][1]

1.  Válassza ki a **központi telepítés tevékenység** kategóriára, jelölje ki az **Azure PowerShell** tevékenységet, és válassza a **Hozzáadás** gombra.

    ![][2]

1.  Válassza az **Azure PowerShell** build lépést, majd válassza ki az értékeit.

    1.  Ha már van egy Azure szolgáltatás végpontjának VIEWBEN Team Services hozzáadni, válassza az előfizetés a **Azure előfizetés** legördülő listában, és folytassa a következő szakaszban az. 

        Ha nem az Azure szolgáltatás végpontjának VIEWBEN Team Services, kell lehetőséget. Ezen alszakasz végigvezeti a folyamaton. Ha az Azure-fiók egy Microsoft-fiókkal (például Hotmail-) használ, kell egy egyszerű hitelesítés megszerezni a következő lépésekkel.

    1.  Válassza az **Azure-előfizetés** mellett a **kezelés** hivatkozásra legördülő listában.

        ![][3]

    1. Az **Új szolgáltatás végpontjának** legördülő listában válassza az **Azure** .

        ![][4]

    1.  **Azure-előfizetés hozzáadása** párbeszédpanelen jelölje be a **Szolgáltatás egyszerű** lehetőséget.

        ![][5]

    1.  Az Azure előfizetési adatokat adhat az **Azure-előfizetés hozzáadása** párbeszédpanel. Adja meg az alábbiakat kell:
        -   Előfizetés azonosítója
        -   Előfizetés neve
        -   Egyszerű azonosító
        -   Egyszerű billentyűt
        -   Bérlői azonosító

    1.  Egy tetszés szerinti nevet adhat az **előfizetés** neve mezőbe. Ez az érték később **Azure előfizetés** legördülő listában a VIEWBEN Team Services fog megjelenni. 

    1.  Ha nem tudja, hogy az Azure előfizetés azonosítója, is használhatja az alábbi parancsok egyikére el.
        
        A PowerShell-parancsfájlokat használhatja:

        `Get-AzureRmSubscription`

        Azure CLI használja:

        `azure account show`
    

    1.  A szolgáltatás egyszerű ID azonosító beszerzése, szolgáltatás egyszerű billentyűt, és a bérlői ID, kövesse az [létrehozása az Active Directory-alkalmazás és a szolgáltatás egyszerű portál használatával](resource-group-create-service-principal-portal.md) , vagy [a szolgáltatás egyszerű Azure erőforrás-kezelővel hitelesítése](resource-group-authenticate-service-principal.md).

    1.  Az **Azure-előfizetés hozzáadása** párbeszédpanelen adja meg a szolgáltatás egyszerű Azonosítót, kulcs szolgáltatás egyszerű és bérlői azonosító értéket, és válassza az **OK** gombra.

        Most már rendelkezik egy érvényes szolgáltatás egyszerű az Azure PowerShell parancsprogramot futtatásához.

1.  Szerkesztés definíciójának szerkesztése, és válassza az **Azure PowerShell** build lépést. Jelölje ki az előfizetés a **Azure előfizetés** legördülő listában. (Ha az előfizetés nem jelenik meg, válassza a **frissítés** gombra, ezután az **kezelése** hivatkozás.) 

    ![][8]

1.  Adja meg a központi telepítés-AzureResourceGroup.ps1 PowerShell-parancsprogramot elérési útját. Ehhez a **Parancsfájl elérési útja** mező melletti három pontra (…) gombra, nyissa meg azt a központi telepítés-AzureResourceGroup.ps1 PowerShell parancsprogramot **parancsfájlok** mappájában található a projekt, jelölje ki azt, és válassza az **OK** gombra. 

    ![][9]

1. Miután kijelölte a parancsfájlt, frissítse az elérési utat a parancsfájl, hogy a Build.StagingDirectory (az azonos könyvtárban *ArtifactsLocation* beállított) a futtatni. Ehhez hozzáadásával "$(Build.StagingDirectory)/"áthelyezése a parancsfájl elérési útját elejére.

    ![][10]

1.  **Parancsfájl argumentumokat** mezőbe írja be a következő paraméterek (egy sorban). A Visual Studióban futtatásakor a parancsfájlt, megtekintheti, hogyan VIEWBEN paraméterei a **kimeneti** ablakban. Használhatja a kiindulási pontként paraméterértékeket beállításához az összeállítás lépésben.

  	| Paraméter | Leírás|
  	|---|---|
  	| -ResourceGroupLocation           | Ahol az erőforráscsoport található, például **eastus** vagy a **"Kelet US"**geo helyfüggő értéke. (Hozzáadás aposztrófok Ha a névben szóközt.) [Azure régiók](https://azure.microsoft.com/en-us/regions/) talál további információt.|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Az erőforráscsoport a telepítéshez használni neve.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Ez a paraméter bemutató, itt adhatja meg, hogy az eltéréseket kell a helyi rendszerből Azure feltöltendő. Csak kell beállítania a kapcsoló, ha a sablon példányban van szüksége, használja a PowerShell parancsprogramot (például konfigurációs parancsfájlok vagy egymásba ágyazott sablonok) szakasz kívánt extra eltérések.                                                                                                                                                                 |
  	| -StorageAccountName              | A telepítéshez szakasz eltéréseket használt tárterület fiók neve. Ez a paraméter megadása kötelező, csak akkor, ha éppen eltérések másolása Azure. Ez a tárterület-fiók nem automatikusan létrejön a telepítés, már léteznie kell.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Az a tárterület-fiókhoz tartozó erőforráscsoport neve. Ez a paraméter megadása kötelező, csak akkor, ha éppen eltérések másolása Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | A fájl elérési útját a sablon az Azure erőforráscsoport telepítési projektben. Nagyobb rugalmasság érdekében használjon ehhez a paraméterhez, amely a PowerShell-parancsprogramot, abszolút elérési út helyett helyének viszonyított elérési utat.|
  	| -TemplateParametersFile          | A fájl elérési útját a paraméterek az Azure erőforráscsoport telepítési projektben. Nagyobb rugalmasság érdekében használjon ehhez a paraméterhez, amely a PowerShell-parancsprogramot, abszolút elérési út helyett helyének viszonyított elérési utat.|
  	| -ArtifactStagingDirectory        | Ez a paraméter lehetővé teszi, hogy a PowerShell-parancsprogramot, hogy a mappa amelyből bináris projektfájlok másolva. Ez az érték felülbírálja az alapértelmezett érték a PowerShell-parancsprogramot által használt. Az VIEWBEN Team Services használja, adja meg az értéket: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Íme egy parancsprogramot argumentumokat példa (a vonal törött olvashatóság érdekében):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Ha végzett, a **Parancsprogram argumentumokat** mezőben kell a következőhöz hasonló:

    ![][11]

1.  Miután felvette a az Azure PowerShell összes szükséges elemet össze lépés, válassza a a **várólista** összeállítása gombra kattintva hozza létre a projekt. A **Szerkesztés** képen látható, a PowerShell-parancsprogramot kimenetét.

## <a name="next-steps"></a>Következő lépések

Olvassa el az [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md) , ha többet szeretne tudni az Azure erőforrás-kezelő és Azure erőforrás csoportok.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
