<properties
    pageTitle="Virtuális gép skála megadása a Visual Studio segítségével telepítése |} Microsoft Azure"
    description="Visual Studio és erőforrás-kezelő sablon segítségével virtuális gép skála készletek terjesztése"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="guybo"/>

# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Virtuális gép skála megadása a Visual Studio segítségével terjesztése

Ez a cikk bemutatja, hogyan üzembe egy Azure virtuális gép skála megadása a Visual Studio erőforrás csoport telepítésének használata.


[Azure virtuális gép skála beállítása](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) egy Azure kiszámítania erőforrás üzembe helyezését és hasonló virtuális gépeken futó automatikus méretezése egyszerűen integrált beállításokkal kezelésében és terheléselosztás. Építse, és üzembe [Azure erőforrás Manager (ARM) sablonok](https://github.com/Azure/azure-quickstart-templates)használata a virtuális skála beállítása. ARM-sablonok Azure CLI, PowerShell, többi telepíthető, továbbá közvetlenül a Visual Studio. Visual Studio biztosít példa sablonokat, amelyek Azure erőforrás csoport telepítési projektben részeként telepíthető.

Azure erőforráscsoport telepítések csoportba, és tegye közzé a kapcsolódó Azure erőforrások halmazának egyetlen telepítési művelet hatékonyan. Itt talál további információt a őket: [létrehozása és üzembe helyezése a Visual Studio segítségével Azure erőforrás-csoportokat](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Előzetes követelmények

Első lépések a virtuális skála beállítja a Visual Studióban üzembe helyezése a következőkre lesz szüksége:

- Visual Studio 2013 vagy a Skype 2015
- Azure SDK 2.7, a 2,8 vagy 2.9

Megjegyzés: Ezeket az utasításokat tegyük fel, a Visual Studio 2015 [Azure SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)alkalmazást.

## <a name="creating-a-project"></a>Projekt létrehozása

1. Új projekt létrehozása a Visual Studio Skype 2015 vállalati **fájl kiválasztása |} Új |} Projekt**.

    ![Fájl menü Új parancsa][file_new]

2. A **Visual C# |} Felhőalapú**, válassza az **Erőforrás-kezelő Azure** -ARM sablon üzembe helyezése a projekt létrehozása.

    ![Projekt létrehozása][create_project]

3.  A sablonok listából válassza ki a Windows virtuális gép skála megadása sablon vagy Linux rendszerhez.

    ![Sablon kiválasztása][select_Template]

4. A projekt létrehozását követően megjelenik az PowerShell telepítésének parancsfájlok, az erőforrás-kezelő Azure sablon és paraméter fájl virtuális gép skála megadása.

    ![Megoldás Explorer][solution_explorer]

## <a name="customize-your-project"></a>A projekt testreszabása

Most már az alkalmazás igényeknek, például a virtuális bővítmény tulajdonságai hozzáadása és szerkesztése a terheléselosztás szabályok testre szabhatja a sablon szerkesztheti. Alapértelmezés szerint a virtuális skála megadása sablonok bevezetését tervezi a AzureDiagnostics bővítmény, ami megkönnyíti az Automatikus méretezéssel szabályok felvétele vannak beállítva. Még egy nyilvános IP-címet, konfigurált bejövő hálózati Címfordítást szabályok mely SSH (Linux) vagy RDP (Windows) – a virtuális példányok való csatlakozás legyen az előtér-porttartományt kezdődik 50000, ami azt jelenti, amíg Linux, ha egy terheléselosztó üzembe helyezése, SSH porthoz 50000 nyilvános IP-cím (vagy a tartománynevet), átirányítja a méretezés halmazban első virtuális 22 portja. Kapcsolódás port 50001 átirányítja a második virtuális 22 portja és így tovább.

 A Visual Studio és a sablonok szerkesztése legegyszerűbben a JSON tagolás használata rendszerezheti a paramétereket, a változók és az erőforrások. A séma megértéséhez a Visual Studio is felhívja a sablon hibák mielőtt beállítaná őket.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>A projekt telepítése

6. ARM sablon üzembe Azure a virtuális skála megadása erőforrás létrehozásához. Kattintson a projekt csomópontját a jobb gombbal, válassza a központi telepítés **|} Új telepítési**.

    ![Sablon terjesztése][5deploy_Template]

7. Jelölje ki az előfizetés a "Üzembe az erőforráscsoport" párbeszédpanel.

    ![Sablon terjesztése][6deploy_Template]

8. További lehetőségek is létrehozhat Azure új erőforráscsoport bevezetését tervezi a sablont.

    ![Új erőforráscsoport][new_resource]

9. Ezután jelölje ki a **Szerkesztése paramétereinek** gombra kattintva adja meg a paramétereket, amely a sablont, az egyes értékek, például a felhasználónév átkerülnek, és az operációs rendszer jelszavának van szükség, a telepítő létrehozásához. Ha nem PowerShell Tools for Visual Studio telepítve, jelölje be a "Mentés jelszavak" javasolt annak érdekében, hogy egy rejtett PowerShell parancssori kérdés elkerülése érdekében, vagy használhatja a [keyvault támogatja](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).

    ![Szerkesztése paramétereinek][edit_parameters]

10. Kattintson a **központi telepítés**gombra. A **kimeneti** ablakban jelennek meg a telepítés előrehaladását. Megjegyzendő, hogy a művelet végrehajtja a **Központi telepítés-AzureResourceGroup.ps1** parancsfájl.

    ![Kimeneti ablakban][output_window]

## <a name="exploring-your-vm-scale-set"></a>A virtuális méretarányra beállított felfedezése

Ha a telepítés befejeződött, megtekintheti az új virtuális skála megadása a Visual Studio **Felhő Explorer** (a lista frissítése). Felhőalapú Explorer lehetővé teszi az erőforrások Azure a Visual Studióban fejlesztett alkalmazások közben. Megtekintheti a virtuális skála megadása a [Azure-portál](https://portal.azure.com) és [Azure erőforrás Explorer](https://resources.azure.com/).

![Felhőalapú Explorer][cloud_explorer]

 A portál legegyszerűbben úgy vizuális kezelése az Azure infrastruktúra – a böngészőben, miközben Azure erőforrás Explorer egyszerűvé Explorer és Azure erőforrások, egy ablak tart a nézetbe"példány", és az erőforrások készíteni PowerShell-parancsait is megjelenítő hibakeresési. Habár virtuális skála beállítása csak előzetes verzióban, az erőforrás Explorer a virtuális skála eredménye a jelennek meg a legtöbb részleteket.

## <a name="next-steps"></a>Következő lépések

Ha korábban sikeresen telepítette virtuális skála beállítja a Visual Studio segítségével testre szabhatja az igényeinek, az alkalmazás igényeknek megfelelően alakíthatja a projekt további. Ha például beállítását Automatikus méretezéssel egy háttérismeretek erőforrást vesz fel, infrastruktúra hozzáadása a sablont, például a különálló VMs vagy telepíti az alkalmazásokat az egyéni parancsfájl bővítmény használatával. Példa sablonok jó forrást található az [Azure quickstart útmutató sablonok](https://github.com/Azure/azure-quickstart-templates) GitHub adattárban (Keresés a "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
