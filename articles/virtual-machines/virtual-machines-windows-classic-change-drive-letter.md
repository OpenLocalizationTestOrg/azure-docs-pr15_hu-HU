<properties
    pageTitle="D: a meghajtó, egy virtuális gép adatok lemezen |} Microsoft Azure"
    description="Ismerteti, hogyan módosíthatja egy Windows virtuális meghajtó betűket, hogy az adatok meghajtó is használhatja a d meghajtó."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>A d meghajtó használni egy Windows virtuális adatok meghajtó 

Ha az alkalmazás használata a D meghajtó adatok tárolására, kövesse az alábbi utasításokat az ideiglenes lemez használni egy másik meghajtóra betűt. Soha ne használva a ideiglenes lemez adatokat szeretne rögzíteni.

Ha átméretezése vagy **leállítása (Deallocate)** virtuális géphez, ez indíthat a virtuális gépen egy új hipervizor helyzetének. A tervezett vagy nem tervezett karbantartás esemény is indíthat a helyét. Ebben az esetben a ideiglenes lemez lesz hozzárendelve az első betűt. Ha egy alkalmazás, amely a d meghajtó kifejezetten igényel, tegye a következőket ideiglenes áthelyezése a pagefile.sys, az új adatok lemez csatolása és rendelje hozzá a D betű és majd helyezhet át a pagefile.sys az ideiglenes meghajtó szeretne. Miután elkészült, Azure nem veszi vissza az D:, ha a virtuális áthelyezése egy másik hipervizor.

További információt a hogyan Azure használja-e a ideiglenes lemez [ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) című cikk nyújt az ideiglenes meghajtó a Microsoft Azure virtuális gépeken futó

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Az adatok lemez csatolása

Először is szüksége lesz az adatok lemez csatolása a virtuális gép. 

- A portál használatához olvassa el a [hogyan csatolhat adatok lemezen az Azure-portálon](virtual-machines-windows-attach-disk-portal.md)
- A klasszikus portál használatához olvassa el a [hogyan csatolhat Windows virtuális géphez adatok lemezen](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Ideiglenes a C meghajtó pagefile.sys áthelyezése

1. Csatlakozás a virtuális gépen. 

2. Kattintson a jobb gombbal a **Start** menü, és válassza a **rendszer**.

3. A bal oldali menüben válassza a **Speciális rendszerbeállítások**.

4. A **Teljesítmény** csoportban válassza a **Beállítások**elemre.

5. Kattintson a **Speciális** fülre.

5. A **virtuális memória** csoportban válassza a **Módosítás**.

6. Jelölje ki a **C** meghajtó **rendszer kezeli a méretet** kattintson rá, és kattintson a **beállítás**.

7. Jelölje ki a **D** meghajtó, és válassza a **nincs lapozási fájl** , és kattintson a **beállítása**gombra.

8. Kattintson az Alkalmaz gombra. Hogy a számítógép újra kell indítani, a módosítások érvénybe figyelmeztető üzenetet kap.

9. Indítsa újra a virtuális gépen.




## <a name="change-the-drive-letters"></a>A meghajtó betűk módosítása 

1. A virtuális újraindítása után újra bejelentkezik a virtuális.

2. Kattintson a **Start** menüre, és írja be a **diskmgmt.msc** , és le az ENTER billentyűt. Elindul a lemez kezelése.

3. Kattintson a jobb gombbal a, **D**, ideiglenes meghajtót, és válassza a **Módosítás betűjele és elérési út**.

4. A betűjele jelölje ki a meghajtó **G** , és kattintson **az OK**gombra. 

5. Kattintson a jobb gombbal az adatok lemezen, és válassza a **Módosítás meghajtó betű és a görbék**.

6. Betűjele, csoportban jelölje be a **D** meghajtó, és kattintson **az OK**gombra. 

7. Kattintson a jobb gombbal a **G**, ideiglenes meghajtót, és válassza a **Módosítás betűjele és elérési út**.

8. A betűjele jelölje ki a meghajtó **E** , és kattintson **az OK**gombra. 

> [AZURE.NOTE] Ha a virtuális más lemezen vagy meghajtót, rendelhetem újra a meghajtó betűjét lemez és meghajtók ugyanazzal a módszerrel használatával. Azt szeretné, hogy a lemez konfigurációját kell:  
>- C:-OS lemez  
>- D: adatok lemez  
>- E: az ideiglenes lemezre



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys visszahelyezése az ideiglenes tárterület-meghajtóra 

1. Kattintson a jobb gombbal a **Start** menü, és válassza a **rendszer**

2. A bal oldali menüben válassza a **Speciális rendszerbeállítások**.

3. A **Teljesítmény** csoportban válassza a **Beállítások**elemre.

4. Kattintson a **Speciális** fülre.

5. A **virtuális memória** csoportban válassza a **Módosítás**.

6. Jelölje be a rendszer a **C** meghajtó, és kattintson a **nincs lapozási fájl** , és kattintson a **beállítása**gombra.

7. Jelölje ki az ideiglenes tároló meghajtót **az E** , és válassza a **rendszer felügyelt méretét** , és kattintson a **beállítása**gombra.

8. Kattintson a **alkalmazása**gombra. Hogy a számítógép újra kell indítani, a módosítások érvénybe figyelmeztető üzenetet kap.

9. Indítsa újra a virtuális gépen.




## <a name="next-steps"></a>Következő lépések
- Növelheti a rendelkezésre álló tárhely a virtuális géphez [lemezen további adatok](virtual-machines-windows-attach-disk-portal.md)csatolásával.



