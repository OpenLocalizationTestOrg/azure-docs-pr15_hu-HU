<properties
    pageTitle="Azure szószedet - Azure szótár |} Microsoft Azure"
    description="Az Azure szószedet segítségével az Azure platform a felhőben terminológia megértése. Ezt a rövid Azure szótárt definíciókat nyújt az Azure a gyakran használt felhő kifejezések."
    keywords="Azure szótár, felhőalapú terminológia, Azure szószedet, terminológia definíciókat, felhőalapú feltételek"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure szószedet: egy szótár felhő terminológia az Azure platform

A Microsoft Azure szószedet az Azure platform felhő terminológia rövid szótár.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Keresse meg a szolgáltatás definíciókat, és egyéb felhő kifejezések

* Azure szolgáltatások és azok AWS definíciókat mint [Microsoft Azure és Amazon webszolgáltatásokhoz](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)témakörben talál.

* Általános ipari felhő kifejezéseket című témakör tartalmaz [felhő számítások kifejezéseket](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Az Azure szószedet a fenti két hivatkozást tartalmazó Azure és a felhő iparágban-végpontok közötti osztályozási biztosít.  

## <a name="azure-glossary-list"></a>Azure szószedet lista


### <a name="account"></a>fiók  
Munkahelyi vagy iskolai vagy eléréséhez, és az Azure előfizetés kezeléséhez használja személyes Microsoft-fiókjával.  
Lásd még:, [hogyan Azure előfizetések társítva Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>elérhetőség beállítása  
Virtuális gépeken futó alkalmazás redundancia és megbízhatóságára megadására közös kezelt gyűjteménye. Az elérhetőség beállítása e biztosítja, hogy bármelyik a tervezett és a tervezett karbantartás esemény során legalább egy virtuális gép érhető el.  
Lásd még: [kezelése a Windows virtuális gépeken futó elérhetőségét](./virtual-machines/virtual-machines-windows-manage-availability.md) vagy [Linux virtuális gépeken futó elérhetőségének kezelése](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Azure klasszikus telepítési modell  
Két egyik [telepítési modellek](resource-manager-deployment-model.md) telepíthető erőforrások (az új minta az Azure erőforrás-kezelő) Azure-ban. Azure források telepítheti egy adatmodellt, vagy más, míg mások mindkét modellekben telepíthető. Erőforrás model(s), amely az egyes Azure erőforrások részletes útmutatást együtt telepíthető.


### <a name="cli"></a>Azure parancssori kezelőfelületről  
A [parancssor](xplat-cli-install.md) OSX, Linux PC-re és a Windows Azure szolgáltatások kezelésére használható.


### <a name="powershell"></a>Azure PowerShell  
A [parancssor](powershell-install-configure.md) a parancssorból Azure szolgáltatás kezelése a Windows PC-re. Egyes szolgáltatások vagy a szolgáltatások csak a PowerShell vagy a CLI keresztül kell kezelni. Minden egyes Azure erőforrás részleteket, amelyek egy erőforrás model(s) útmutatást együtt telepíthető.   
Lásd még: [miként telepítheti és Azure PowerShell konfigurálása](powershell-install-configure.md)


### <a name="arm-model"></a>Azure erőforrás-kezelő telepítési modell  
A Microsoft Azure (a másik pedig a klasszikus telepítési modell) az erőforrások üzembe használt két [telepítési modellek](resource-manager-deployment-model.md) közül. Azure források telepítheti egy modell vagy más, míg mások mindkét modellekben telepíthető. Erőforrás model(s), amely az egyes Azure erőforrások részletes útmutatást a együtt telepíthető.


### <a name="fault-domain"></a>Hibafa-tartomány  
Az elérhetőség beállított esetleg történhet egyszerre virtuális gépeken futó gyűjteménye. Példa egy csoportja közös power forrás- és kapcsoló osztozó állvány gépek. Az Azure az elérhetőség beállítása a virtuális gépeken futó automatikusan több hibafa tartományok neve.  
Lásd még: [kezelése a Windows virtuális gépeken futó elérhetőségét](./virtual-machines/virtual-machines-windows-manage-availability.md) vagy [Linux virtuális gépeken futó elérhetőségének kezelése](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>GEO  
Két vagy több régiók általában tartalmazó adatok illetőségére definiált határa. A szegélyek lehet belül vagy nemzeti határokon kívül, és adó rendelkezések hatással vannak. Minden geo legalább egy régió tartalmaz. Geos példák Ázsia Csendes-óceáni és a japán. *Földrajzi hely*néven is ismert.  
Lásd még: [Azure régiók](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>a replikáció GEO  
A folyamat azon automatikusan replikált például BLOB, táblázatok és sorok területi két belül.  
Lásd még: [Azure SQL-adatbázis aktív Geo replikáció](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>kép  
Egy fájl, amely tartalmazza az operációs rendszer és Alkalmazásbeállítások hozhat létre virtuális gépeken futó tetszőleges számú használható. Azure-ban: képek két típusa van: virtuális OS képe és a képet. A virtuális képe az operációs rendszer és az összes lemez csatolni virtuális géphez, a kép létrehozásakor tartalmazza. Operációs rendszer kép csak egy általános operációs rendszer nincs adatok lemez konfigurációjának tartalmazza.  
Lásd még: [Navigálás, és válassza a Windows PowerShell vagy a CLI Azure-ban virtuális gép képek](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>korlátai  
Információforrások, amelyek hozhatók létre és a teljesítmény viszonyítási alap, amely lehet elérni száma. Korlátozások társulnak általában előfizetések, szolgáltatások és szeretne rendelni.  
Lásd még: [Azure előfizetés és szolgáltatás korlátozások, kvótákat, és korlátozások](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>terheléselosztó  
Ez az erőforrás elosztja a bejövő forgalmat a hálózati számítógépek között. Azure-ban egy terheléselosztó elosztja a virtuális gépeken futó-terheléselosztó argumentumlista definiált forgalmat. Internetes [terheléselosztó](./load-balancer/load-balancer-overview.md) is lehet, vagy belső lehet.  


### <a name="offer"></a>ajánlat  
Az ár, jóváírások és egymáshoz kapcsolódó kifejezések alkalmazandó Azure-előfizetésbe.  
Lásd: az [Azure kínálnak Részletek lap](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>portál  
A biztonságos webes portál üzembe helyezéséhez és kezeléséhez Azure szolgáltatások segítségével.  Vannak olyan két portálokról: az [Azure-portálra](http://portal.azure.com/) , és a [Klasszikus portálon](http://manage.windowsazure.com/). Egyes szolgáltatások érhetők el a mindkét portálokról, míg más alkalmazásokat csak az egyiket a. Az [Azure portál elérhetőségi diagram](https://azure.microsoft.com/features/azure-portal/availability/) listákat, mely szolgáltatások legyenek elérhetők, amelyek portálon.  


### <a name="region"></a>régió  
Egy geo, amely nem közötti nemzeti szegélyek, és egy vagy több adatközpontokkal tartalmaz olyan területen. Árak, regionális szolgáltatások és ajánlat használata a régió szintjén elérhetővé tett. Egy tartomány van általában megfeleltetni egy másik területére, nem vagyok a gépnél, a több száz mérföld felfelé területi két kialakításához lehet. Területi párban vészhelyreállítás és magas elérhetősége esetek egy mechanizmusként használható. Más néven általában *helyét*.  
Lásd még: [Azure területek](best-practices-availability-paired-regions.md)


### <a name="resource"></a>erőforrás  
Elem, amely az Azure megoldást része. Minden egyes Azure szolgáltatás lehetővé teszi, hogy a különböző típusú erőforrások, mint az adatbázisok és virtuális gépeken futó telepítése.   
Lásd még: [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>Erőforráscsoport  
Az erőforrás-kezelő az alkalmazáshoz kapcsolódó erőforrásokat tartalmazó tároló. Az erőforráscsoport is vegye fel az összes alkalmazás az erőforrások, vagy csak a közös logikailag csoportosított erőforrásokat. Eldöntheti, hogy hogyan szeretné erőforrások hozzárendelése az erőforrás-csoportok a szervezet kiválasztania Miből alapján.  
Lásd még: [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Erőforrás-kezelő sablon  
Egy vagy több Azure erőforrások deklaratív meghatározó és, amely meghatározza, hogy a telepített erőforrások közötti függőségek JSON fájl. A sablon egységes és többször az erőforrások telepítendő használható.  
Lásd még: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>erőforrás-szolgáltató  
Az erőforrások tárolandó szolgáltatás telepítheti és erőforrás-kezelő kezelése. Minden erőforrás-szolgáltató kínál az erőforrásokat van telepítve, a műveletek. Erőforrás-szolgáltatók az Azure-portálra, Azure PowerShell és több programozási SDK keresztül is elérhető.  
Lásd még: [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>szerepkör  
Azt jelenti, hogy a felhasználók, csoportok és szolgáltatások rendelhetők hozzáférés szabályozása a. Szerepkörök képesek ilyen létrehozásakor, kezelése és Azure erőforrások további műveletek elvégzéséhez.  
Lásd még: [RBAC: beépített szerepkörök](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>Szolgáltatásiszint-szerződés (SLA)  
A szerződés a Microsoft kötelezettségek leíró üzemidőt és a kapcsolatok esetén. Minden egyes Azure szolgáltatás egy adott SLA rendelkezik.  
Lásd még: a [szolgáltatás szintű rendelkezést](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>tárterület-fiók  
Egy tárterület-fiókot, amellyel az Azure Blob, a várakozási sorban található, a táblázat és a fájl szolgáltatások Azure-tárolóban lévő eléréséhez. Tárterület-fiókját az egyedi névtér biztosít az Azure tároló adatobjektumok.  
Lásd még: [Azure kapcsolatos tárolás fiókok](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>előfizetés  
Egy ügyfél szerződés lehetővé tevő Azure szolgáltatások beszerzése Microsoftnál. Az előfizetés árak és egymáshoz kapcsolódó kifejezések az ajánlatot az előfizetés választott szabályozza. Lásd: a [Microsoft Online előfizetési szerződés](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Lásd még:, [hogyan Azure előfizetések társítva Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>címke  
Az indexelési kifejezés, amely lehetővé teszi, hogy a kategorizálás erőforrások vagy a Számlázás kezelése az igényeinek megfelelően. Címkék is használhatja, amikor az erőforrás-csoportok és erőforrások összetett gyűjteménye, és azokat az eszközöket, hogy a legtöbb értelemszerű ábrázolása kell. Ha például sikerült nyomon követése információforrások, amelyek egy hasonló szerepkör szolgálnak a szervezet, illetve az ugyanazon részleg tartoznak.  
Lásd még: [az Azure erőforrások rendszerezése címkék használata](resource-group-using-tags.md)


### <a name="update-domain"></a>tartomány frissítése  
A virtuális gépeken futó egy elérhetőségének beállítása, hogy egy időben frissülnek gyűjteménye. Virtuális gépeken futó ugyanabban a frissítés tartományban közös újraindítja tervezett karbantartás során. Azure egynél több frissítése tartomány soha nem újraindítása egyszerre. Más néven egy frissítési tartományt.  
Lásd még: [kezelése a Windows virtuális gépeken futó elérhetőségét](./virtual-machines/virtual-machines-windows-manage-availability.md) vagy [Linux virtuális gépeken futó elérhetőségének kezelése](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuális gépen  
A fizikai operációs rendszert futtató számítógép szoftver végrehajtása. Több virtuális gépeken futó ugyanazt a hardver egyszerre futtatható. Az Azure-virtuális gépeken futó érhetők el a különféle méretű.  
Lásd még: [virtuális gépeken futó dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>virtuális gép bővítmény  
Ez az erőforrás viselkedése és szolgáltatásokat, amelyek közül segítségével más működik, és küldje el a mód használata számítógépen futó programok hajt végre. A virtuális Access bővítmény segítségével például alaphelyzetbe állítása vagy módosítsa az Azure virtuális gépen távelérési értékeket.  
Lásd még: [virtuális gép extensions és a szolgáltatásokról (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) vagy [virtuális gép extensions és a szolgáltatásokról (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>virtuális hálózati  
A hálózat, ahol az Azure erőforrások, amelyek elkülönítik az Azure bérlők közötti kapcsolatot. Csatlakoztathatók más Azure virtuális hálózatokon az [Azure virtuális Magánhálózati átjáró](./vpn-gateway/vpn-gateway-about-vpngateways.md) keresztül, és a helyszíni hálózaton [több](./vpn-gateway/vpn-gateway-cross-premises-options.md)-beállításokkal. Teljesen megadhatja, hogy az IP-címterületet, a DNS-beállítások, a biztonsági házirendek és a útválasztási táblára a hálózaton belül.  
Lásd még: [virtuális hálózati – áttekintés](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Lásd még:**  
- [Azure – első lépések](https://azure.microsoft.com/get-started/)
- [Felhőalapú Erőforrásközpont](https://azure.microsoft.com/resources/)  
- [Az üzleti alkalmazás Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [A adatközpontban Azure](https://azure.microsoft.com/overview/business-apps-on-azure/) 





