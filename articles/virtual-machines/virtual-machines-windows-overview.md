<properties
    pageTitle="Windows virtuális gépeken futó áttekintése |} Microsoft Azure"
    description="Tudjon meg többet létrehozása és kezelése a Windows virtuális gépeken futó Azure-ban."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>A Windows virtuális gépeken futó Azure-ban – áttekintés

Azure virtuális gépeken futó (virtuális) a különböző típusú [igény szerinti, a méretezhető számítógépes erőforrások](../app-service-web/choose-web-site-cloud-service-vm.md) , amely felajánlja Azure egyike. A szokásos választhat egy virtuális további beállítási lehetőségekre a számítógépes környezet, mint a választási lehetőségek felajánlása szükség esetén. Ez a cikk információt, mire érdemes lehetővé teszi a virtuális, hogyan létrehozása és kezelése terén létrehozása előtt.

Az Azure virtuális a rugalmasságot biztosít a virtualization anélkül, hogy vásárol, és karbantartása a fizikai hardver futtató azt. Azonban van szüksége a virtuális megőrzéséhez feladatokat, például konfigurálása, javítása és futtat, a Szoftvertelepítés hajt végre.

Azure virtuális gépeken futó többféle módon is használható. Néhány példa a következők:

- **Fejlesztés és tesztelése** – Azure VMs ajánlat gyorsan és egyszerűen módon létrehozásához számítógépen meghatározott konfigurációjának a szükséges kódot, és tesztelje az alkalmazás.
- **A felhőben alkalmazások** – az alkalmazás igény szerinti ingadozhat, mert értelme lehet economic Azure-ban egy virtuális futtatásához. Ha szüksége van rájuk, és a Leállítás őket, ha Ön nem kifizetéséhez további VMs.
- **Kiterjesztett adatközponthoz** – az Azure virtuális hálózat virtuális gépeken futó könnyen kapcsolhat össze a szervezet hálózatán.

Az alkalmazás által használt VMs számát is méretezhető be és ki függetlenül is tagnak kell lenni felel meg az igényeinek.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Mire van szükségem egy virtuális létrehozása előtt megfontolni?

Nincsenek mindig [tervezési szempontjait](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) rengeteg generál meg az alkalmazás-infrastruktúrát Azure-ban. Fontos megfontolni megkezdése előtt ezek olyan virtuális részei a következők:

- Az alkalmazás erőforrásnevek
- Az erőforrások tárolási helye
- A virtuális méretének
- Létrehozható VMs maximális száma
- Az operációs rendszer, amely a virtuális fut
- A virtuális után induljon konfigurációja
- A kapcsolódó források, amely a virtuális van szüksége

### <a name="naming"></a>Elnevezése

Virtuális gép van rendelve [nevét](virtual-machines-windows-infrastructure-naming-guidelines.md) , és van beállítva az operációs rendszer részét képező számítógép nevét. A virtuális neve legfeljebb 15 karakter hosszú lehet.

Azure használatával az operációs rendszer lemezen létrehozása, ha a számítógép neve és a virtuális számítógépnév megegyeznek. Ha [tölthet fel, és a saját kép használata](virtual-machines-windows-upload-image.md) , amely tartalmazza a korábban beállított operációs rendszert, és hozhat létre virtuális gép használni, akkor a nevek eltérő lehet. Javasoljuk, hogy a saját kép fájl feltöltésekor, hogy a számítógép nevét az operációs rendszer és a virtuális gép neve megegyezik.

### <a name="locations"></a>Helyek

Azure-ban létrehozott összes erőforrás több [földrajzi régiók](https://azure.microsoft.com/regions/) a világ különböző van meghatározva. Általában a régió neve **hely** egy virtuális létrehozásakor. A virtuális a hely határozza meg, a virtuális merevlemez tárolási helyére.

Az alábbi táblázat néhány módszert elérheti az elérhető helyek listáját jeleníti meg.

| A módszer | Leírás |
| ------ | ----------- |
| Azure portál | Válasszon egy helyet a listából, amikor létrehoz egy virtuális. |
| Azure PowerShell | A [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) paranccsal. |
| REST API-VAL | Használja a [helyek listájában](https://msdn.microsoft.com/library/dn790540.aspx) a műveletet. |

### <a name="vm-size"></a>Virtuális mérete

A virtuális használt [méretét](virtual-machines-windows-sizes.md) a terhelést a futtatni kívánt határozza meg. A méretét, majd válassza ki például power, memória és tárolókapacitású feldolgozása tényezők határozza meg. Azure felajánlja a különféle méretű a támogatási számos különböző típusú használ.

Azure költségek az [óránkénti ár](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) a virtuális méretét és az operációs rendszer alapján. Részleges órát, használt percig Azure díjakat. Tárterület árú és külön-külön fel.

### <a name="vm-limits"></a>Virtuális korlátai

Az előfizetés alapértelmezett [kvótákat](../azure-subscription-service-limits.md) tartalmaz, amely hatással lehetnek a sok VMs beállítása a projekthez központi helyen. Az aktuális használati előfizetés alapon korlát 20 VMs / régió. Korlátozások is emelt növekedését kérő támogatási jegy tárolásához.

### <a name="operating-system-disks-and-images"></a>Operációs rendszer lemezre és háttérképek

Virtuális gépeken futó használva [virtuális merevlemez (VHD)](virtual-machines-windows-about-disks-vhds.md) azok operációs rendszeren és az adatok. A képek-OS telepítéséhez választhat VHD is használható. 

Azure sok [piactér képek](https://azure.microsoft.com/marketplace/virtual-machines/) használata különböző verzióiban és a Windows kiszolgálói operációs rendszerek típusú biztosít. Kép a publisher, ajánlat, termékváltozat és verzióját (általában verziója van megadva legújabb) piactérről képek azonosítja. 

Ebben a táblázatban láthatók a bemutatják, hogyan megtalálhatja az információkat a képhez.

| A módszer | Leírás |
| ------ | ----------- |
| Azure portál | Amikor kijelöl egy képet szeretne használni az értékek automatikusan adható meg. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -hely "hely"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -hely "hely"-"publisherName" Publisher<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -hely "hely"-"publisherName" Publisher-ajánlat "offerName" |
| REST API-hoz | [A közzétevőknek lista képe](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Kínál lista képe](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Lista képe SKU](https://msdn.microsoft.com/library/mt743701.aspx) |

Kiválaszthatja, hogy [tölthet fel](virtual-machines-windows-upload-image.md) , és a saját kép használata, és ekkor a közzétevő nevét, ajánlat és termékváltozat nem használható.

### <a name="extensions"></a>Bővítmények

Virtuális [bővítmények](virtual-machines-windows-extensions-features.md) adja meg a bejegyzés konfigurációs és az automatizált feladatok virtuális további lehetőségeket.

A gyakori feladatok extensions használata végezhető el:

- **Egyéni parancsfájlok futtatása** – az [Egyéni parancsfájl-bővítmény](virtual-machines-windows-extensions-customscript.md) segít a virtuális munkaterhelésekből konfigurálja úgy, hogy a parancsprogram futtatása, amikor a virtuális már kiépítve.
- **Deploy és kezelheti a konfigurációk** – beállításokat és a környezetek kezelése a egy virtuális DSC beállítása a [PowerShell kívánt állam konfigurációs (DSC) bővítmény](virtual-machines-windows-extensions-dsc-overview.md) segítségével.
- **Diagnosztikai adatok összegyűjtése** – az [Azure diagnosztika bővítmény](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) segít beállítani a Lync-állapota az alkalmazás használható diagnosztika adatokat szeretne gyűjteni virtuális.

### <a name="related-resources"></a>Kapcsolódó források

Az erőforrások az alábbi táblázatban a virtuális által használt, és létezik, vagy a virtuális létrehozásakor hozható létre kell.

| Erőforrás | Szükséges | Leírás |
| -------- | -------- | ----------- |
| [Erőforráscsoport](../azure-resource-manager/resource-group-overview.md) | igen | A virtuális szerepelnie kell egy erőforrás csoportban. |
| [Tárterület-fiók](../storage/storage-create-storage-account.md) | igen | A virtuális kell a tároló fiókot a virtuális merevlemezeken tárolásához. |
| [Virtuális hálózati](../virtual-network/virtual-networks-overview.md) | igen | A virtuális virtuális hálózat tagjának kell lennie. |
| [Nyilvános IP-cím](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | nem | A virtuális beállíthatja, hogy egy nyilvános IP-cím rendelt távoli eléréséhez. |
| [Hálózati kapcsolat](../virtual-network/virtual-network-network-interface-overview.md) | igen | A virtuális van szüksége a hálózati kapcsolat kapcsolatba lépni a hálózaton. |
| [Adatok lemez](virtual-machines-windows-attach-disk-portal.md) | nem | A virtuális adatok lemezre kattintva bontsa ki a tárolási lehetőségeket is tartalmazhat. |

## <a name="how-do-i-create-my-first-vm"></a>Hogyan hozhatok létre saját első virtuális?

Ha a virtuális készítéséhez számos beállítást. A választási lehetőségek, akkor a környezet, az Ön függ. 

Ez a táblázat információk segítenek elsajátíthatja a virtuális.

| A módszer | A cikk |
| ------ | ------- |
| Azure portál | [A portálon Windows operációs rendszert futtató virtuális gép létrehozása](virtual-machines-windows-hero-tutorial.md) |
| Sablonok | [Az erőforrás-kezelő sablonnal Windows virtuális gép létrehozása](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Hozzon létre egy Windows virtuális PowerShell használatával](virtual-machines-windows-ps-create.md) |
| Ügyfél SDK | [Azure erőforrásoknak C# terjesztése](virtual-machines-windows-csharp.md) |
| REST API-hoz | [Hozzon létre, vagy egy virtuális módosítása](https://msdn.microsoft.com/library/mt163591.aspx) |

Meg kívánom soha nem történik, de alkalmanként valamit, amit mentésük. Ez a helyzet akkor történik, ha tekintse meg az információkat az [erőforrás-kezelő – problémamegoldás telepítési problémák a Windows Azure virtuális gép létrehozásával](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Hogyan kezelhetem a virtuális létrehozott?

A böngészőalapú portál parancssori eszközökkel futtatása, vagy közvetlenül API-khoz támogatásával VMs kell kezelni. Néhány tipikus adatkezelési feladatok, előfordulhat, hogy végrehajtott vannak és mappák adatainak megjelenítése a virtuális, egy virtuális elérhetősége kezeli, és a biztonsági másolatok készítése történő bejelentkezés.

### <a name="get-information-about-a-vm"></a>A virtuális adatainak megtekintése

Az alábbi táblázat mutatja meg néhány módszert, amely egy virtuális információkat kaphat.

| A módszer | Leírás |
| ------ | ----------- |
| Azure portál | A központi menüben kattintson a **virtuális gépeken futó** , és a listából válassza ki a virtuális. A virtuális lap, a van hozzáférése áttekintő adatainak, állítsa be értékeket, és a figyeléshez mértékek. |
| Azure PowerShell | VMs kezelése a PowerShell használatával kapcsolatos információkért olvassa el a [kezelése Azure virtuális gépeken futó erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-manage.md)című témakört. |
| REST API-VAL | Az [első virtuális információk](https://msdn.microsoft.com/library/mt163682.aspx) művelet segítségével egy virtuális adatainak. |
| Ügyfél SDK | C# VMs kezelése való használatával kapcsolatos további tudnivalókért lásd [kezelése Azure virtuális gépeken futó Azure erőforrás-kezelő a és C# használatával](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Jelentkezzen be a virtuális rendszerbe

Az Azure-portálon [egy távoli asztali RDP-munkamenet](virtual-machines-windows-connect-logon.md)indítása használhatja a Csatlakozás gombra. Mi hol néha elromolhatnak a távoli kapcsolat használata közben. Ha ezt a helyzetet akkor történik, olvassa el [az Azure virtuális géphez Windows operációs rendszert futtató távoli asztali kapcsolatos hibák elhárítása](virtual-machines-windows-troubleshoot-rdp-connection.md)kapcsolatokat a súgóinformációt.

### <a name="manage-availability"></a>Elérhetőség kezelése

Fontos átláthatja a [magas rendelkezésre állás érdekében](virtual-machines-windows-manage-availability.md) az alkalmazás hogyan. Ebben a konfigurációban magában foglalja a több VMs annak érdekében, hogy legalább egy fut létrehozása.

Ahhoz, hogy a 99.95 virtuális szolgáltatási szint szerződés a telepítéshez sorrendben kell fut a terhelést belüli [elérhetősége beállítása](virtual-machines-windows-infrastructure-availability-sets-guidelines.md)egy két vagy több VMs telepítése. Ebben a konfigurációban biztosítja a VMs vannak elosztva több hibafa tartományt, és van telepítve, a különböző karbantartási windows hosts alakzatot. A teljes [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) egészének az Azure garantált elérhető ismerteti.
 
### <a name="back-up-the-vm"></a>Készítsen biztonsági másolatot a virtuális
 
Adatok és az Azure biztonsági mentése és a webhely-helyreállítás Azure-szolgáltatásokban eszközök védelme [helyreállítási szolgáltatások tárolóból elemre](../backup/backup-introduction-to-azure-backup.md) szolgál. A helyreállítási szolgáltatások tárolóból elemre az [üzembe helyezéséhez és kezeléséhez biztonsági mentést, az erőforrás-kezelő rendszerbe VMs PowerShell használatával](../backup/backup-azure-vms-automation.md)is használhatja. 

## <a name="next-steps"></a>Következő lépések

- A leképezés Linux VMs használata esetén tekintse meg [Azure és Linux rendszerhez](virtual-machines-linux-azure-overview.md).
- További tudnivalók az útmutatásokat, állítsa be a következő [Példa Azure infrastruktúra forgatókönyv](virtual-machines-windows-infrastructure-example.md)a infrastruktúra körül.
- Győződjön meg arról, hajtsa végre a [Gyakorlati tanácsok a Windows Azure virtuális gép fut](virtual-machines-windows-guidance-compute-single-vm.md)-e.


