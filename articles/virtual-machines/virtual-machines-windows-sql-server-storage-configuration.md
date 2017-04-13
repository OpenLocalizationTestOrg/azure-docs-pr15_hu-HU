<properties
    pageTitle="Az SQL Server VMs tárolási konfigurációt |} Microsoft Azure"
    description="Ez a témakör azt ismerteti, hogyan Azure beállítja a tárhelyet az SQL Server VMs során telepítési (erőforrás-kezelő telepítési modellje). Azt is bemutatja, hogyan konfigurálható tárhely a meglévő SQL Server VMs."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Az SQL Server VMs tároló konfigurálása

Amikor egy SQL Server virtuális gép kép konfigurálja az Azure-ban, a portálon segítségével automatizálhatja a tárolási konfigurációt. Ide tartoznak a tárhely csatolása a virtuális, tárolási elérhető SQL Server készít, és konfigurálja úgy optimalizálhatja az adott teljesítmény igényeinek megfelelően.

Ebből a témakörből megtudhatja, hogyan Azure beállítja a tárhelyet az SQL Server VMs az kiépítési során, és a meglévő VMs. Ebben a konfigurációban a [Gyakorlati tanácsok a teljesítmény](virtual-machines-windows-sql-performance.md) alapján az SQL Servert futtató Azure VMs.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

## <a name="prerequisites"></a>Előfeltételek
Az automatikus tárolását konfigurációs beállítások használatához a virtuális gép szükség a következő tulajdonságokat:

- Az [SQL Server gyűjteménye](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing)kiépítve.
- Az [erőforrás-kezelő telepítési modell](../resource-manager-deployment-model.md)használ.
- [Prémium tárterületet](../storage/storage-premium-storage.md)használ.

## <a name="new-vms"></a>Új VMs
Az alábbi szakaszok ismertetik, hogyan lehet új SQL Server virtuális gépeken futó tároló konfigurálásához.

### <a name="azure-portal"></a>Azure portál
Kiépítésekor az Azure virtuális használata egy SQL Server gyűjteménye, megadhatja az új virtuális automatikusan konfigurálja a tárhely. Megadhatja, hogy a tárhely, korlátozások a teljesítmény és terhelést típusát. Az alábbi képernyőképen látható kiépítési a tárhely konfigurációs lap SQL virtuális során.

![SQL Server virtuális tároló Configuration kiépítési során](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Választási lehetőségek alapján, Azure hajtja végre a következő tárolási konfigurációs feladatokat a virtuális létrehozása után:

- Hoz létre, és csatolja a prémium tároló adatok lemezt a virtuális géphez.
- Az adatok lemezt hozzáférhető legyen az SQL Server állít be.
- Az adatok lemezt be egy, a megadott mérete és teljesítménye (IOPS és az átvitel) igényeknek megfelelően alakítható tárolókészlethez állít be.
- A tárhely kvótáját társít a virtuális gépen új meghajtót.
- A megadott terhelés típusa (adatok raktározás, tranzakció alapú feldolgozása vagy általános) alapján új meghajtó optimalizálja.

Hogyan állítja be a Azure a tárhely beállításai a további részletekért olvassa el a [tárhely konfigurálása szakaszban](#storage-configuration). Egy teljes áttekintése a következő egy SQL Server virtuális létrehozása az Azure-portálon olvassa el a [a kiépítési oktatóprogram](virtual-machines-windows-portal-sql-server-provision.md)című témakört.

### <a name="resource-manage-templates"></a>Erőforrás-kezelés sablonok
Az alábbi erőforrás-kezelő sablonok használata esetén két prémium adatok lemez csatolt nincs tároló készlet beállításokkal alapértelmezés szerint. Ezek a sablonok módosítani szeretné a számát prémium adatok lemezt a virtuális gép csatolt is testreszabhatja.

- [Az automatikus biztonsági másolat létrehozása a virtuális](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [Az automatikus javítási virtuális létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [Hozzon létre virtuális AKV-integrációval](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Meglévő VMs
Az SQL Server meglévő VMs módosíthatja az egyes tároló beállításokat, az Azure-portálon. Jelölje ki a virtuális, és nyissa meg a beállítások területen válassza az SQL Server Configuration. Az SQL Server-beállítások lap az aktuális tároló használatát a virtuális jeleníti meg. Ez a diagram a virtuális megtalálható összes meghajtó jelenik meg. A rendelkezésre álló tárterület méretének minden meghajtó, négy részből jeleníti meg:

- SQL-adatok
- SQL-naplója
- Egyéb (az SQL tároló)
- Érhető el

![A meglévő SQL Server virtuális tároló konfigurálása](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

A tárhely hozzáadása egy új meghajtót, vagy egy meglévő meghajtót kiterjesztése beállításához kattintson a Szerkesztés hivatkozásra a diagram felett.

A beállítási lehetőségek láthatja, hogy függően változik ezt a szolgáltatást, mielőtt használt. Az első alkalommal történő használatakor megadhatja a tárhelyre egy új meghajtót. Ha korábban használt Ez a szolgáltatás hozzon létre egy meghajtót, megadhatja, ha ki szeretné terjeszteni a meghajtó tároló.

### <a name="use-for-the-first-time"></a>Használata első alkalommal
Ha első alkalommal ezzel a szolgáltatással, megadhatja a mérete és teljesítménye tárhelykorlátainak egy új meghajtót. A tapasztalt hasonlít a kiépítési idő szeretné látni. A fő különbség, hogy nincs engedélye terhelést típusának megadásához. Ezt a korlátozást megakadályozza, hogy a virtuális gépen az SQL Server meglévő beállításaitól megszakítása.

![Az SQL Server tároló csúszkák konfigurálása](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure létrehoz egy új meghajtót az alkalmazásra vonatkozó technikai adatok alapján. Ebben az esetben Azure hajtja végre a következő tárolási konfigurációs feladatokat:

- Hoz létre, és csatolja a prémium tároló adatok lemezt a virtuális géphez.
- Az adatok lemezt hozzáférhető legyen az SQL Server állít be.
- Az adatok lemezt be egy, a megadott mérete és teljesítménye (IOPS és átviteli) igényeknek megfelelően alakítható tárolókészlethez állít be.
- A tárhely kvótáját társít a virtuális gépen új meghajtót.

Hogyan állítja be a Azure a tárhely beállításai a további részletekért olvassa el a [tárhely konfigurálása szakaszban](#storage-configuration).

### <a name="add-a-new-drive"></a>Új meghajtó hozzáadása
Ha már beállította az SQL Server virtuális tárhely, tárhely bővítésével ezt a lehetőséget választva új két lehetőség közül választhat. Az első, hogy hozzáadása egy új meghajtót, amelyek növelhetik a virtuális teljesítmény szintjét.

![Új meghajtó hozzáadása egy SQL virtuális](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Jó helyen jár Miután megadta a meghajtóba, kell végrehajtania a teljesítmény növelése érdekében néhány további manuális konfiguráció.

### <a name="extend-the-drive"></a>A meghajtó bővítése
A többi tárhely bővítésével kapcsolatos, hogy a meglévő meghajtó bővítése. Ez a beállítás növeli a meghajtó a rendelkezésre álló tárhely, de azt nem teljesítmény növelése érdekében. A tárterület-készletek nem változtathatja meg a hasábok számát a tárhely kvótáját létrehozását követően. A hasábok számát határozza meg, hogy a párhuzamos írások, amelyben adatok lemezt is lehet. Ennélfogva minden felvett adatok lemezt nem teljesítmény növelése érdekében. Több tárhely csak az éppen írt adatok tudjanak nyújtani. Ezt a korlátozást is jelenti, hogy a meghajtó bővítésekor a hasábok számát határozza meg, hogy a lehető legkevesebb adatok lemezt felvehető. Így ha négy adatok lemezre egy tárolókészlethez hoz létre, a hasábok számát is négy. Bármikor kibővíti tárolására, fel kell vennie legalább négy adatok lemezre.

![Kijelölés méretének növelése a meghajtó egy SQL virtuális](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Tárterület konfigurálása
Ez a témakör egy hivatkozást, amely automatikusan végrehajtja Azure SQL-virtuális kiépítési vagy az Azure-portálon konfigurációs során tároló konfigurációs megváltozott.

- A virtuális tárolási kevesebb mint két TBs be van jelölve, ha Azure nem hoz létre egy tárolókészlethez.
- A virtuális tárolási legalább két TBs be van jelölve, Azure egy tárolókészlethez adja meg. Ez a témakör következő rész a tárhely készlet konfiguráció részletes adatait.
- Automatikus tároló konfigurációs mindig [prémium tároló](../storage/storage-premium-storage.md) P30 adatok lemezt használja. Emiatt nem között a kijelölt terabájt és a számát a virtuális kapcsolt adatok lemez 1:1 megfeleltetés.

Árinformációkat, kapcsolatban a [tárhely árak](https://azure.microsoft.com/pricing/details/storage) lapon a **Lemezterülettel** lapon.

### <a name="creation-of-the-storage-pool"></a>A tárhely kvótáját létrehozása
Azure SQL Server VMs a tárhely kvótáját létrehozása a következő beállításokat használja.

| A beállítás | Érték |
|-----|-----|
| Csíkok mérete  | 256 KB (adatok raktározás); 64 KB (tranzakció alapú) |
| Lemezen méretek | 1 TB |
| Gyorsítótár | Olvasás |
| Felosztás mérete | 64 KB NTFS elosztási egység mérete |
| Azonnali fájl inicializálni | Engedélyezve van |
| A memóriában zárolása lapok | Engedélyezve van |
| Helyreállítási | Egyszerű helyreállítás (nincs tűrőképessége) |
| Az oszlopok száma | Adatok lemez<sup>1</sup> száma |
| TempDB helye | Tárolt adatok lemez<sup>2</sup> |

<sup>1</sup> a tárhely kvótáját létrehozását követően nem változtathatja meg a tárhely kvótáját az oszlopok számát.

<sup>2</sup> a beállítás csak az első meghajtó hoz létre, a tárhely szolgáltatással vonatkozik.

## <a name="workload-optimization-settings"></a>Terhelést optimalizálási beállítások:
Az alábbi táblázat ismerteti a három terhelést Szövegbeállítások és a megfelelő optimalizálásokat:

| Terhelés típusa | Leírás | Optimalizálásokat |
|-----|-----|-----|
| **Általános** | A legtöbb munkaterhelésekből támogató alapértelmezett beállítás | Nincs lehetőség |
| **Tranzakció alapú feldolgozása** | Optimalizálja a hagyományos adatbázis OLTP munkaterhelésekből tárterület | Nyomkövetés-jelölő 1117<br/>Nyomkövetés-jelölő 1118 |
| **Adattárolási** | Optimalizálja a tárterület elemzési és jelentéskészítési munkaterhelésekből | Nyomkövetés-jelölő 610<br/>Nyomkövetés-jelölő 1117 |

>[AZURE.NOTE] A terhelési típus csak adja meg, ha a, hozhatók létre SQL virtuális gép szeretne, jelölje ki a tárhely konfigurációs lépésre.

## <a name="next-steps"></a>Következő lépések
Futó SQL Server Azure VMs kapcsolódó témakörök [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)talál.
