<properties
    pageTitle="Virtuális gép skála állítja be az Áttekintés |} Microsoft Azure"
    description="További tudnivalók a virtuális gép skála beállítása"
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
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Virtuális gép skála készletek – áttekintés

Virtuális gép skála készletei olyan az Azure kiszámítania erőforrás üzembe helyezéséhez és kezeléséhez azonos VMs halmazának is használhatja. A beállított összes VMs azonos, készletek készült támogatási igaz Automatikus méretezéssel – nincsenek előre kiépítését VMs virtuális skála szükség – és olyan megkönnyíti nagy számítási, nagy adatokkal és indexelése munkaterhelésekből kiválasztásával nagyméretű szolgáltatások összeállítása.

Alkalmazások, amely kell méretezni, és a műveleteket a rendszer implicit módon kiegyensúlyozott skála számítási erőforrások hiba és frissítés tartományok. Virtuális méretarányra bevezető beállítja az [Azure blog bejelentése](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/)vonatkoznak.

Nézze meg ezeket a videókat kapcsolatos további tudnivalók a virtuális skála beállítása:

 - [Megjelölés Russinovich folytatott beszélgetést Azure skála beállítása](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuális gép skála állítja be a változásairól Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Hozhat létre és kezelhet virtuális skála állítja be.

Létrehozhat egy virtuális skála megadása a [Azure portál](https://portal.azure.com) kijelölésével _Új_ , és a Keresés mezőbe beírja "skála". Ekkor megjelenik a "virtuális gép skála megadása" a találatok között. Innen töltheti testreszabása és üzembe a skála megadása a szükséges mezőket. 

Virtuális skála készletek is meg lehet adni, és a JSON-sablonok és [REST API -khoz](https://msdn.microsoft.com/library/mt589023.aspx) csak rendszerbe, például egyéni Azure erőforrás-kezelő VMs. Ennélfogva minden olyan szabványos Azure erőforrás-kezelő telepítési módszerek használható. Sablonok kapcsolatos további tudnivalókért olvassa el a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)című témakört.

A virtuális skála eredménye a példa sablonok készletét megtalálható az Azure quickstart útmutató sablonok GitHub tárházba [itt.](https://github.com/Azure/azure-quickstart-templates) (keresse meg a sablonokat _vmss_ címe)

Az alábbi sablonok a projektinformációs lapok megjelenik egy gomb, amely a portál telepítés szolgáltatással. Virtuális skála megadása üzembe helyezéséhez kattintson a gombra, és töltse ki a paramétereket, a portálon van szükség. Ha nem biztos, hogy egy erőforrás támogatja a felső vagy vegyes eset biztonságosabb kisbetű paraméterértékeket mindig használata. Virtuális méretarányra beállított sablon az alábbi hasznos videó boncolásán is van:

[Virtuális skála sablon boncolást beállítása](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Ki- és a virtuális skálával méretezési beállítása

Növelheti vagy csökkentheti a virtuális skála megadása a virtuális gépeken futó számát, egyszerűen módosítsa a _kapacitás_ tulajdonság, és telepítsen újra a sablont. Ez az egyszerűség egyszerűen írja be a saját egyéni méretezési réteg meg szeretné-e az Azure Automatikus méretezéssel által nem támogatott egyéni méretarány események megadása.

Sablon módosítása a kapacitás vannak újbóli, ha olyan sok kisebb sablont, amely csak tartalmazza a Termékváltozat és a frissített kapacitás adható meg. Ez a példa [itt.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Lépéseinek elvégzése, hozzon létre egy skála megadása, hogy a program automatikusan átméretezi, lásd: az [Automatikus méretezés gépek virtuális gép skála megadása](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>A virtuális skála figyelése beállítása

Az [Azure portál](https://portal.azure.com) listák skála beállítása és egyszerű tulajdonságainak megjelenítése, valamint nevére a partnerlistában VMs megadása. Részletesebben az [Erőforrás-Explorer Azure](https://resources.azure.com) virtuális skála készletek megtekintéséhez is használhatja. Virtuális skála készletek Microsoft.Compute, az erőforrás, erről a helyről megtekintheti őket az alábbi hivatkozások kibontásával:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Virtuális skála megadása felhasználási területei

Ez a szakasz néhány jellemző virtuális skála megadása lehetőséget. Egyes magasabb szintű Azure szolgáltatások (például a köteget, szolgáltatás háló, Azure tároló szolgáltatás) forgatókönyvekben fogja használni.

 - **RDP / SSH virtuális méretarányra beállítása példányok** - A virtuális skála megadása a VNET belül hoz létre, és egyéni VMs skála megadása nem osztott nyilvános IP-címeket. Ennek oka egy jó már nem általában szeretné, hogy a költség és kezelés terhelést felosztásának a számítási rács állapot nélküli összes erőforrás különálló nyilvános IP-címek és könnyedén csatlakozhat ezek VMs más forrásokból a VNET, köztük például terheléselosztókkal vagy különálló virtuális gépeken futó nyilvános IP-címek rendelkező a.

 - **Csatlakozás a hálózati Címfordítást szabályok használatával VMs** - hozzon létre egy nyilvános IP-címet, hozzárendelheti egy terheléselosztó és az IP-cím port hozzárendelése egy virtuális skála megadása a virtuális portjához bejövő hálózati Címfordítást szabályokhoz határozza meg. Példa:
 
    Forrás | Forrásport | Cél | Célport
    --- | --- | --- | ---
    Nyilvános IP | A port 50000 | vmss\_0 | A port 22-es hibakód
    Nyilvános IP | A port 50001 | vmss\_1 | A port 22-es hibakód
    Nyilvános IP | A port 50002 | vmss\_2 | A port 22-es hibakód

    Íme egy példa hálózati Címfordítást szabályok ahhoz, hogy minden méretaránnyal egy egyetlen nyilvános IP-protokollal beállítása a virtuális SSH kapcsolatot használó virtuális skála készlet létrehozása: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Íme egy példa tegye ugyanezt a RDP és a Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **"Jumpbox" használatával VMs csatlakozás** - hoz létre virtuális skálával, ha adja meg, és az azonos VNET, az önálló virtuális és VMs csatlakozhat egymásnak azok belső IP-segítségével virtuális skála megadása a virtuális önálló szünteti meg a VNET alhálózat szerint meghatározott formátumban. Ha egy nyilvános IP-cím létrehozása, és rendelje hozzá az önálló virtuális is RDP vagy SSH az önálló virtuális, és csatlakoztassa a számítógépről a virtuális méretarányra beállított példányaiban. Észreveheti ezen a ponton, hogy egy egyszerű virtuális-skála megadása, annál eleve biztonságosabb egyszerű önálló virtuális egy nyilvános IP-címet az alapértelmezett konfigurációját.

    [Példa ezt a módszert ezzel a sablonnal hoz létre egy egyszerű Mesos fürt fő virtuális, amelyek VMs virtuális méretarányra beállított alapú fürt kezeli önálló álló.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Példányok terheléselosztás virtuális méretarányra beállítása** - VMs a "ciklikus" megközelítés használata a számítási fürthöz végzi a munkát, beállíthatja az Azure terheléselosztó terheléselosztási szabályok lehetőséget. Ellenőrizze, hogy az alkalmazás futása pingelés szondákat adhatja meg a megadott protokollt, intervallum és kérelem elérési útjával portokat. Az Azure [Alkalmazás átjáró](https://azure.microsoft.com/services/application-gateway/) támogatja a méretezés készletek bonyolultabb terheléselosztási esetek együtt is.

    [Íme egy példa, amely hoz létre virtuális skála az IIS web Servert futtató VMs, és egy terheléselosztó használ, amely az egyes virtuális kap terhelését. Egy adott URL-címet a minden virtuális pingelése HTTP protokollt is használ.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (tekintse meg a Microsoft.Network/loadBalancers erőforrástípus és networkProfile és a virtualMachineScaleSet a extensionProfile)

 - **Virtuális skálával értéke lehet egy PaaS felülete a számítási fürthöz üzembe helyezés** - készletek néha megkötésekről a következő generációs dolgozó szerep virtuális skála. Érvényes leírását, de azt is fut a kockázat áttekinthetőbb legyen skála funkciók beállítása PaaS v1 dolgozó szerepkör funkcióival. Virtuális skála értelemben készletek adja meg a true "dolgozó szerepkör" vagy a dolgozó erőforrások, annak, hogy biztosítanak platform/futtatókörnyezet független, amely a számítási általános erőforrás testre szabható és integrálása az Azure erőforrás-kezelő IaaS.

    Egy PaaS v1 dolgozó szerepkört, miközben korlátozott értelmez platform/futtatókörnyezet támogatási (csak a Windows platformon képek) is szolgáltatásokból, például a virtuális felcserélése, konfigurálható beállítások frissítése, futtatókörnyezet/alkalmazás telepítési bizonyos beállítások amelyek vagy nem _még_ megtalálható virtuális készletek méretezheti vagy más magasabb szintű PaaS szolgáltatásai például szolgáltatás háló kerülnek. Ezzel a feledje, tekinthetik meg a virtuális skála beállítása egy kialakításának, amely támogatja az PaaS. Tehát PaaS megoldások szolgáltatás háló például, vagy fürt menedzserek Mesos fölött virtuális nyújthat, mint beállítása átméretezheti, mint méretezhető számítási réteg.

    [Példa ezt a módszert ezzel a sablonnal hoz létre egy egyszerű Mesos fürt fő virtuális, amelyek VMs virtuális méretarányra beállított alapú fürt kezeli önálló álló.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) Az [Azure tároló szolgáltatás](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) jövőbeli verzióiban ebben az esetben virtuális skála készletek alapján további komplex/ellenállóvá verziója fog üzembe.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Virtuális skála megadása a teljesítmény és a méretezés útmutató

- Ne hozzon létre több mint 500 VMs több virtuális skála készletek egyszerre.
- Legfeljebb 20 VMs per tároló fiók megtervezése (kivéve, ha a tulajdonság akkor _overprovision_ "hamis"értékre, amely esetben látogathat legfeljebb 40).
- Az első betűjét tároló fióknevét szerint széthúzása.  A példa VMSS sablonok az [Azure quickstart útmutató sablonok](https://github.com/Azure/azure-quickstart-templates/) ennek módjáról példákon mutatják.
- Ha az egyéni VMs, megtervezése a legfeljebb 40 VMs egy virtuális skála megadása egyetlen tárterület-fiókjában.  A virtuális méretarányra beállított telepítés megkezdése előtt a tárterület-fiókba előre másolt kép szüksége lesz. Lásd: további információt a gyakori kérdések.
- Legfeljebb 4096 VMs per VNET megtervezése.
- Létrehozhat VMs számát, amelyben telepít régióban core kvóta korlátozza. Előfordulhat, hogy ügyfélszolgálatának növelheti a számítási kvótakorlátja nagyobb, akkor is, ha a felhőszolgáltatások és a IaaS v1 programmal használható fúrásmintáit magas legfeljebb ma van. A kvóta lekérdezni futtatását is lehetővé teszi a következő Azure CLI parancsot: `azure vm list-usage`, és az alábbi PowerShell-parancsot: `Get-AzureRmVMUsage` (ha alatti 1,0 használata az PowerShell verziójával `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>Virtuális skála megadása a gyakran ismételt kérdések

**KÉRDÉSEK.** Hány VMs is van egy virtuális skála megadása?

**VÁLASZOK PARANCSRA.** 100 platform képek, amely legyen elosztva több tárterületet fiók használatakor. Ha egyéni képek legfeljebb 40 (Ha a _overprovision_ tulajdonsága "hamis" 20 alapértelmezés szerint), mivel egyéni képek korlátozódik jelenleg egyetlen tárterület-fiókjába.

**Kérdések** milyen más erőforrás vonatkozó korlátozások léteznek virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** Korlátozódik legfeljebb 500 VMs 10 perc időszakára / régió több skála készletek létrehozása. A meglévő [Azure feliratkozással korlátai /](../azure-subscription-service-limits.md) alkalmazni.

**KÉRDÉSEK.** Azok az adatok lemezre támogatott belül virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** Nem az első kiadásban. Az adatok tárolására szolgáló beállítások a következők:

- Azure-fájlok (kis-és Középvállalatok megosztott meghajtók)

- Operációs rendszer meghajtó

- TEMP meghajtó (helyi, nem készített biztonsági Azure tároló)

- Azure adatszolgáltatásként (például az Azure táblázatok, az Azure BLOB)

- Külső adatok szolgáltatás (például a távoli adatbázis)

**KÉRDÉSEK.** Azokat a területeket Azure virtuális skála készletek támogatja?

**VÁLASZOK PARANCSRA.** Bármely régió, amely támogatja az erőforrás-kezelő Azure virtuális skála készletek támogatja.

**KÉRDÉSEK.** Hogyan hozhat létre virtuális méretaránnyal egy egyéni kép felhasználásával beállítása?

**VÁLASZOK PARANCSRA.** A vhdContainers tulajdonság értékét hagyja üresen, például:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**KÉRDÉSEK.** Ha e csökkentheti a virtuális skála kapacitás 20 15, amelyek VMs törlődnek beállítása?

**VÁLASZOK PARANCSRA.** Virtuális gépeken futó törlődjenek a skála megadása egyenletesen frissítési tartományok és hibafa tartományok maximális elérhetőség. A legnagyobb azonosítóval VMs először törlődnek.

**KÉRDÉSEK.** Hogyan kell tudni azt, ha a 18 15 kapacitás majd növelése?

**VÁLASZOK PARANCSRA.** Ha kapacitása ahhoz, hogy a 18 növeléséhez 3 új VMs hozható létre. Minden alkalommal, amikor a virtuális példány azonosítója lesz növeli az előző legnagyobb érték (például 20 21, 22). VMs FDs és UDs vannak meghatározni.

**KÉRDÉSEK.** Amikor több bővítmény egy virtuális skála megadása, is-végrehajtási sorrend követelni?

**VÁLASZOK PARANCSRA.** Nem közvetlenül, de a customScript bővítmény a parancsprogram sikerült megvárja, amíg egy másik bővítmény ([például úgy, hogy a bővítmény napló figyelése](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)) befejezéséhez. Program bővítmény sorrendjét további útmutatást a blogbejegyzésben: [Azure virtuális skála készletben bővítmény sorrendjét](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**KÉRDÉSEK.** Azure elérhetősége értékkészletet működnek virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** igen. A virtuális skála be kapcsolva az implicit elérhetősége 5 FDs és 5 UDs beállítása. Nem kell semmit a virtualMachineProfile konfigurálása. Későbbi kiadásokban virtuális skála készletek valószínűleg több bérlők időtartamát, de most egy skála megadása esetén egyetlen elérhetősége meg.
