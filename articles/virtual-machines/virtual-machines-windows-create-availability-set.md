<properties
    pageTitle="Hozzon létre egy virtuális elérhetősége |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy beállítása a virtuális gépeken futó Azure portálja vagy az erőforrás-kezelő telepítési modell PowerShell használata az elérhetőség."
    keywords="elérhetőség beállítása"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Hozzon létre egy elérhetősége 

A portálon használja, ha azt szeretné, hogy a virtuális az elérhetőség részének megadása után az elérhetőség, először állítsa létrehozásához szükséges.

Létrehozásával és elérhetőségének beállítása használatával kapcsolatos további tudnivalókért olvassa el a [virtuális gépeken futó elérhetőségének kezelése](virtual-machines-windows-manage-availability.md)című témakört.


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>Hozzon létre egy elérhetőségét a VMs létrehozása előtt állítsa be a portál használatával

1. A központi menüben kattintson a **Tallózás gombra** , és válassza a **elérhetősége állítja be**.

2. Az **Elérhetőség állítja be a lap**kattintson a **Hozzáadás**gombra.

    ![Képernyőkép, melyen a Hozzáadás gombra, hozhat létre egy új elérhetőségének beállítása.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. A **Létrehozás elérhetősége megadása** lap töltse ki az adatokat, a számára.

    ![Képernyőkép, melyen az adatokat meg kell adni a létrehozása az elérhetőségét.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **Név** - neve legyen az 1-80 karakterek, számok, betűket, időszakok, aláhúzás és szaggatott vonal tevődik össze. Az első karakter egy betűt vagy számot kell lennie. Az utolsó karaktere betű, szám vagy aláhúzás kell lennie.
    - **Hibafa-tartományok** – hibafa tartományok definiálása virtuális gépeken futó által megosztott közös power forrás- és kapcsoló csoportja. Alapértelmezés szerint a VMs választják el egymástól legfeljebb három hibafa tartományok és 1 és 3 között módosítható.
    - **Frissítse a tartomány** - tartományok oszthatók ki az alapértelmezés szerint a és a öt frissítés beállíthatók 1 és 20 között. Frissítés tartományok azt jelzik, hogy virtuális gépeken futó és az alapul szolgáló fizikai hardver, hogy újra kell indítani egy időben. Például ha azt határozza meg, hogy az öt frissítése a tartományok lapon, ha a legfeljebb öt virtuális gépeken futó beállítása egyetlen elérhetősége belül megtörténik, a hatodik virtuális gép kerülnek be az első virtuális gép, a hetedik az azonos UD a második virtuális gép, mint a frissítés tartományra és így tovább. A újraindítása sorrendjének nem lehet valamilyen sorrendben követik egymást, de csak egy frissítés tartomány lesz újra kell indítani, egy időben.
    - **Előfizetés** - válassza azt az előfizetést, ha egynél több használni.
    - **Erőforráscsoport** – jelölje ki a meglévő erőforrás csoport a nyílra kattintva, majd a legördülő listáról erőforráscsoport kijelölése lefelé. Új erőforráscsoport is hozhat létre, írjon be egy nevet. A név tartalmazhat a következő karakterek közül: betűk, számok, időszakok, szaggatott vonal, aláhúzás és nyitó vagy záró zárójelet. A név nem végződhet ponttal. Az összes az elérhetőség csoportban a VMs létre kívánja hozni az Erőforrás elérhetősége megadása azonos csoportba.
    - **Hely** - válasszon egy helyet az a legördülő listában.

4. Amikor befejezte a kitöltéséhez kattintson a **Létrehozás**gombra. Az elérhetőség csoport létrehozását követően tekintheti meg a listában a portálon frissítés után.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>Hozzon létre egy virtuális gép és az elérhetőség egyszerre beállítása a portál használatával

Ha egy új virtuális a portálon hoz létre, hozzon létre egy új elérhetőségének beállítása a első virtuális hoz létre a virtuális közbeni.

![Képernyőkép, melyen a folyamat hozhat létre egy új elérhetőségének hoz létre a virtuális közbeni.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Egy új virtuális hozzáadása egy meglévő elérhetőségének beállítása

Minden egyes további virtuális létrehozott megadása kell tartoznak győződjön meg arról, hogy az azonos **erőforráscsoport** létrehozni a dokumentumot, és válassza a meglévő elérhetőségének beállítása a 3. 

![Képernyőkép: Jelölje ki a virtuális állítsa egy meglévő elérhetőségét.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>PowerShell használatá hozzon létre egy elérhetősége

Ebben a példában az elérhetőség **Nyugati USA -beli** hely beállítása a **RMResGroup** erőforráscsoport hoz létre. Meg kell végezni az első virtuális, amely tekinthetők meg a készlet létrehozása előtt.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
További tudnivalókért olvassa el a [New-AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx)című témakört.


## <a name="troubleshooting"></a>Hibaelhárítás

- Egy virtuális, létrehozásakor, ha a kívánt elérhetőségének beállítása a legördülő listában a portálon nem lehet létrehozott azt egy másik erőforráscsoport. Ha nem tudja, hogy az erőforrás-elérhetőségi csoport beállítása, nyissa meg a központi menüt, és kattintson a Tallózás > elérhetősége állítja be az elérhetőség beállítása és listájának megtekintése tartozó erőforráscsoport.


## <a name="next-steps"></a>Következő lépések

Tárhely hozzáadása a virtuális egy további [adatok lemez](virtual-machines-windows-attach-disk-portal.md)hozzáadásával.
