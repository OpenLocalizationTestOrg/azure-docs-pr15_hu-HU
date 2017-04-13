<properties
    pageTitle="Hozzon létre több virtuális gépeken futó |} Microsoft Azure"
    description="Több virtuális gépeken futó létrehozása a Windows beállításai"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="guybo"/>

# <a name="create-multiple-azure-virtual-machines"></a>Több Azure virtuális gépeken futó létrehozása

Vannak olyan sok olyan esetek, ahol sok hasonló virtuális gépeken futó (VMs) létrehozásához szükséges. Többek között számítógépes nagy teljesítményű (HPC), nagyméretű adatelemzés, méretezhető és gyakran állapot nélküli középszintű vagy kódmentes servers (például Webkiszolgalok) és elosztott adatbázisok.

Ez a cikk azt ismerteti, hogy a rendelkezésre álló beállítások több VMs létrehozása az Azure-ban. A beállításokkal lépjen túl az egyszerű esetek manuálisan kezdeményezik VMs sorozata. Sok VMs létrehozásához a folyamatok általában használó nem méretezze át is, ha több mint VMs néhány létrehozásához szükséges.

Sok hasonló VMs létrehozása egy úgy, hogy az erőforrás-kezelő Azure szerkezetet, az _erőforrás hurkok_használja.

## <a name="resource-loops"></a>Erőforrás hurkok

Erőforrás hurkok Azure erőforrás-kezelő sablonok szintaktikai kijelölésére is. Erőforrás hurkok ismétlődve hasonló módon beállított erőforráskészletek hozhat létre. Több tárhely ügyfelek, a hálózati kapcsolatok vagy a virtuális gépeken futó létrehozása erőforrás hurkok is használhatja. Erőforrás hurkok kapcsolatos további tudnivalókért tekintse át a [VMs létrehozása az elérhetőségét állítja be, hogy az erőforrás hurkok használatával](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>Méretezés problémáit

Bár az erőforrás hurkok könnyebb egy felhőalapú infrastruktúrájának a méretezés kiépítésekor, és tömörebb sablonok kiszámítására, bizonyos kihívásokkal maradnak. Például ha egy erőforrás hurok 100 virtuális gépeken futó létrehozása, szüksége összehangolására a hálózati kapcsolat vezérlők (NIC) megfelelő VMs és tároló fiókok. VMs számát valószínűleg eltér a tárterület-fiókok számát, mert kell kezelni az eltérő erőforrás hurok méretek, túl. Ezek solvable problémákat, de összetettségétől jelentősen növeli skála.

Egy másik beavatkozás igazolására szolgáló eljárás akkor fordul elő, ha szüksége van egy infrastruktúra elastically méretezze át. Például érdemes lehet automatikusan nő vagy kevesebb hozzáadható VMs terhelést válaszul Automatikus méretezéssel infrastruktúra. VMs nem adja meg az integrált mechanizmusa számot (skála, és a skála) változhat. VMs eltávolításával méretezése esetén nehéz zökkenőmentes elérhetősége magas legyen, hogy VMs kiegyensúlyozását-e frissítési és hibafa tartományok.

Végül egy erőforrás hurok használatakor erőforrások létrehozása több hívást nyissa meg a mögöttes háló. Hasonló erőforrások több hívások létrehozásakor az Azure implicit lehetőséget a tervezés, javítása és a telepítés megbízhatóságának és teljesítményének optimalizálhatja tartalmaz. Ez a _virtuális gép skála állítja be_ honnan származnak.

## <a name="virtual-machine-scale-sets"></a>Virtuális gép skála beállítása

Virtuális gép skála beállítja az Azure kiszámítania erőforrás üzembe helyezéséhez és kezeléséhez azonos VMs halmazának. Az összes VMs konfigurálva a virtuális beosztása azonos, készletek is egyszerűen méretezése és méretezése. Egyszerűen módosítása VMs számának megadása. Virtuális skála beállítása automatikus méretezéssel a terhelést a igényeivel alapján is beállítható.

Van szükség, ha át kívánja méretezni, és a számítási erőforrások alkalmazások a méretarány műveletek vannak implicit módon meghatározni hiba és frissítés tartományok.

Helyett használatával történik a több erőforrások, például a hálózati kártyák és VMs, egy virtuális skála megadása a hálózati, tárterület, virtuális gép és bővítmény tulajdonságai központi konfigurálható tulajdonsággal tartalmaz.

Virtuális skála készletek bemutatása tekintse át a [virtuális gép skála termék lap állítja be](https://azure.microsoft.com/services/virtual-machine-scale-sets/). További információt az Ugrás a [virtuális gépeken futó skála dokumentáció állítja be](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).
