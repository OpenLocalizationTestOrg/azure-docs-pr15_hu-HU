

Alkalmazások, amely kell méretezni, és a műveleteket a rendszer implicit módon kiegyensúlyozott skála számítási erőforrások hiba és frissítés tartományok. Egy virtuális méretarányra bevezető készletek tekintse át a legutóbbi [Azure blog bejelentése](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Nézze meg ezeket a videókat kapcsolatos további tudnivalók a virtuális skála beállítása:

 - [Megjelölés Russinovich folytatott beszélgetést Azure skála beállítása](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuális gép skála állítja be a változásairól Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Hozhat létre és kezelhet virtuális skála állítja be.

Virtuális skála készletek definiálható és rendszerbe JSON-sablonok és a [REST API -khoz](https://msdn.microsoft.com/library/mt589023.aspx) csak egyes Azure erőforrás-kezelő VMs például. Ennélfogva minden olyan szabványos Azure erőforrás-kezelő telepítési módszerek használható. Sablonok kapcsolatos további tudnivalókért olvassa el a [szerzői Azure erőforrás-kezelő sablonok](../articles/resource-group-authoring-templates.md)című témakört.

A virtuális skála eredménye a példa sablonok készletét az Azure quickstart útmutató sablonok GitHub adattárban itt találhatók:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) - keresse meg a _vmss_ a címben sablonokat.

Az alábbi sablonok a projektinformációs lapok megjelenik egy gomb, amely a portál telepítés szolgáltatással. Virtuális skála megadása üzembe helyezéséhez kattintson a gombra, és töltse ki a paramétereket, a portálon van szükség. Ha nem biztos, hogy egy erőforrás támogatja a felső vagy vegyes eset biztonságosabb kisbetű paraméterértékeket mindig használata. Virtuális méretarányra beállított sablon az alábbi hasznos videó boncolásán is van:

[Virtuális skála sablon boncolást beállítása](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Ki- és a virtuális skálával méretezési beállítása

Növelheti vagy csökkentheti a virtuális skála megadása a virtuális gépeken futó számát, egyszerűen módosítsa a _kapacitás_ tulajdonság, és telepítsen újra a sablont. Ez az egyszerűség egyszerűen írja be a saját egyéni méretezési réteg meg szeretné-e az Azure Automatikus méretezéssel által nem támogatott egyéni méretarány események megadása.

Sablon módosítása a kapacitás vannak újbóli, ha olyan sok kisebb sablont, amely csak tartalmazza a Termékváltozat és a frissített kapacitás adható meg. Például az alábbiak szerint néz ki: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Lépéseinek elvégzése, hozzon létre egy skála megadása, hogy a program automatikusan átméretezi, lásd: az [Automatikus méretezés gépek virtuális gép skála megadása](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>A virtuális skála figyelése beállítása

Az [Azure erőforrás Explorer](https://resources.azure.com) használata a virtuális skála készletek megtekintéséhez jelenleg ajánlott. Virtuális skála készletek Microsoft.Compute, az erőforrás, erről a helyről megtekintheti őket az alábbi hivatkozások kibontásával:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>Virtuális skála megadása felhasználási területei

Ez a szakasz néhány jellemző virtuális skála megadása lehetőséget. Egyes magasabb szintű Azure szolgáltatások (például a köteget, szolgáltatás háló, Azure tároló szolgáltatás) forgatókönyvekben fogja használni.

 - **RDP / SSH virtuális méretarányra beállítása példányok** - A virtuális skála megadása a VNET belül hoz létre, és az egyes VMs nem nyilvános IP-címek van rendelve. Ennek oka egy jó már nem általában szeretné, hogy a költség és kezelés terhelést felosztásának a számítási rácsban állapot nélküli összes erőforrás különálló IP-címek és könnyedén csatlakozhat ezek VMs más forrásokból a VNET, köztük például terheléselosztókkal vagy különálló virtuális gépeken futó nyilvános IP-címek rendelkező a.

 - **Csatlakozás a hálózati Címfordítást szabályok használatával VMs** - hozzon létre egy nyilvános IP-címet, hozzárendelheti egy terheléselosztó és az IP-cím port hozzárendelése egy virtuális skála megadása a virtuális portjához bejövő hálózati Címfordítást szabályokhoz határozza meg. Pl.

    Nyilvános IP-Port 50000 > vmss ->\_0 -> Port 22 nyilvános IP - > Port 50001 -> vmss\_1 -> Port 22 nyilvános IP - > Port 50002 -> vmss\_2 -> Port 22-es hibakód

    Íme egy példa hálózati Címfordítást szabályok ahhoz, hogy minden méretaránnyal egy egyetlen nyilvános IP-protokollal beállítása a virtuális SSH kapcsolatot használó virtuális skála készlet létrehozása: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Íme egy példa tegye ugyanezt a RDP és a Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **"Jumpbox" használatával VMs csatlakozás** - hoz létre virtuális skálával, ha adja meg, és az azonos VNET, az önálló virtuális és VMs csatlakozhat egymásnak azok belső IP-segítségével virtuális skála megadása a virtuális önálló szünteti meg a VNET alhálózat szerint meghatározott formátumban. Ha egy nyilvános IP-cím létrehozása, és rendelje hozzá az önálló virtuális is RDP vagy SSH az önálló virtuális, és csatlakoztassa a számítógépről a virtuális méretarányra beállított példányaiban. Észreveheti ezen a ponton, hogy egy egyszerű virtuális-skála megadása, annál eleve biztonságosabb egyszerű önálló virtuális egy nyilvános IP-címet az alapértelmezett konfigurációját.

    Példa ezt a módszert, ezzel a sablonnal hoz létre egy egyszerű Mesos fürt fő virtuális, amelyek VMs virtuális méretarányra beállított alapú fürt kezeli önálló álló: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Ciklikus terheléselosztás virtuális méretarányra beállítása példányok** - végzi a munkát, használja a "ciklikus" megközelítés VMs számítási fürthöz, beállíthat egy Azure terheléselosztó terheléselosztási szabályok lehetőséget. Azt is megadhatja, ellenőrizze, hogy az alkalmazás futása pingelés szondákat megadott protokollt, intervallum és kérelem útvonallal portokat.

    Íme egy példa, amely hoz létre virtuális skála az IIS web Servert futtató VMs, és egy terheléselosztó használ, amely az egyes virtuális kap terhelését. Azt is HTTP protokollt használ egy adott URL-címet a minden virtuális pingelése: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - Microsoft.Network/loadBalancers erőforrástípus és a networkProfile és a virtualMachineScaleSet a extensionProfile.

 - **Virtuális skálával értéke lehet egy PaaS felülete a számítási fürthöz üzembe helyezés** - készletek néha megkötésekről a következő generációs dolgozó szerep virtuális skála. Érvényes leírását, de azt is fut a kockázat áttekinthetőbb legyen skála funkciók beállítása PaaS v1 dolgozó szerepkör funkcióival. Virtuális skála értelemben készletek adja meg a true "dolgozó szerepkör" vagy a dolgozó erőforrások, annak, hogy biztosítanak platform/futtatókörnyezet független, amely a számítási általános erőforrás testre szabható és integrálása az Azure erőforrás-kezelő IaaS.

    Egy PaaS v1 dolgozó szerepkört, miközben korlátozott értelmez platform/futtatókörnyezet támogatási (csak a Windows platformon képek) is szolgáltatásokból, például a virtuális felcserélése, konfigurálható beállítások frissítése, futtatókörnyezet/alkalmazás telepítési bizonyos beállítások amelyek vagy nem _még_ megtalálható virtuális készletek méretezheti vagy más magasabb szintű PaaS szolgáltatásai például szolgáltatás háló kerülnek. Ezzel a feledje, tekinthetik meg a virtuális skála beállítása egy kialakításának, amely támogatja az PaaS. Tehát PaaS megoldások szolgáltatás háló például, vagy fürt menedzserek Mesos fölött virtuális nyújthat, mint beállítása átméretezheti, mint méretezhető számítási réteg.

    Íme egy példa a Mesos fürtre, amely egy virtuális skála megadása a virtuális önálló az azonos VNET telepít. Az önálló virtuális Mesos mesteralakzat, és a virtuális skála megadása a kisegítő csomópontok csoportját képviseli: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). Az [Azure tároló szolgáltatás](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) jövőbeli verzióiban ebben az esetben virtuális skála készletek alapján további komplex/ellenállóvá verziója fog üzembe.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Virtuális skála megadása a teljesítmény és a méretezés útmutató

- A nyilvános villámnézetben létrehozása több mint 500 VMs több virtuális skála készletben egyszerre.
- Tárterület-fiók egy legfeljebb 40 VMs megtervezése.
- Az első betűjét tároló fióknevét szerint széthúzása.  A példa VMSS sablonok az [Azure quickstart útmutató sablonok](https://github.com/Azure/azure-quickstart-templates/) ennek módjáról példákon mutatják.
- Ha az egyéni VMs, megtervezése a legfeljebb 40 VMs egy virtuális skála megadása egyetlen tárterület-fiókjában.  A virtuális méretarányra beállított telepítés megkezdése előtt a tárterület-fiókba előre másolt kép szüksége lesz. Lásd: további információt a gyakori kérdések.
- Legfeljebb 2048 VMs per VNET megtervezése.  Ezt a korlátot nő a jövőben.
- A számítási core kvóta bármelyik tartományban lévő korlátozza hozhat létre VMs számát. Előfordulhat, hogy ügyfélszolgálatának növelheti a számítási kvótakorlátja nagyobb, akkor is, ha a felhőszolgáltatások és a IaaS v1 programmal használható fúrásmintáit magas legfeljebb ma van. A kvóta lekérdezni futtatását is lehetővé teszi a következő Azure CLI parancsot: _azure virtuális lista-használatát_, és az alábbi PowerShell-parancsot: _Get-AzureRmVMUsage_ (Ha a _Get-AzureVMUsage_1.0 alatt az PowerShell verziójával használata).

## <a name="vm-scale-set-frequently-asked-questions"></a>Virtuális skála megadása a gyakran ismételt kérdések

**KÉRDÉSEK.** Hány VMs is van egy virtuális skála megadása?

**VÁLASZOK PARANCSRA.** 100 platform képek, amely legyen elosztva több tárterületet fiók használatakor. Ha egyéni képek legfeljebb 40, mivel az egyéni képek korlátozódik egyetlen tárolási fiókhoz előzetes verzióban.

**Kérdések** milyen más erőforrás vonatkozó korlátozások léteznek virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** Legfeljebb 500 VMs létrehozása az előnézeti időszakban / régió több skála készletben, korlátozódik. A meglévő [Azure feliratkozással korlátai /](../articles/azure-subscription-service-limits.md) alkalmazni.

**KÉRDÉSEK.** Azok az adatok lemezre támogatott belül virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** Nem az első kiadásban. Az adatok tárolására szolgáló beállítások a következők:

- Azure-fájlok (kis-és Középvállalatok megosztott meghajtók)

- Operációs rendszer meghajtó

- TEMP meghajtó (helyi, nem készített biztonsági Azure tároló)

- Külső adatok (például az távoli DB, az Azure táblázatokat, a Azure BLOB) szolgáltatás

**KÉRDÉSEK.** Azokat a területeket Azure virtuális skála készletek támogatása?

**VÁLASZOK PARANCSRA.** Bármely régió, amely támogatja az erőforrás-kezelő Azure virtuális skála készletek támogatja.

**KÉRDÉSEK.** Hogyan hozhat létre virtuális méretaránnyal egy egyéni kép felhasználásával beállítása?

**VÁLASZOK PARANCSRA.** A vhdContainers tulajdonság értékét hagyja üresen, például:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**KÉRDÉSEK.** Ha e csökkentheti a virtuális skála kapacitás 20 15, amelyek VMs törlődnek beállítása?

**VÁLASZOK PARANCSRA.** Virtuális gépeken futó törlődjenek a skála megadása egyenletesen frissítési tartományok és hibafa tartományok maximális elérhetőség.

**KÉRDÉSEK.** Hogyan kell tudni azt, ha a 18 15 kapacitás majd növelése?

**VÁLASZOK PARANCSRA.** Ha növeli a 18, az index 15, 16 VMs 17 létrejön. Mindkét esetben a VMs FDs és UDs vannak meghatározni.

**KÉRDÉSEK.** Amikor több bővítmény egy virtuális skála megadása, is-végrehajtási sorrend követelni?

**VÁLASZOK PARANCSRA.** Nem közvetlenül, de a customScript bővítmény a parancsprogram sikerült megvárja, amíg egy másik bővítmény befejezéséhez (például úgy, hogy a bővítmény figyelése jelentkezzen – lásd [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**KÉRDÉSEK.** Azure elérhetősége értékkészletet működnek virtuális skála beállítása?

**VÁLASZOK PARANCSRA.** igen. A virtuális skála be kapcsolva az implicit elérhetősége 3 FDs és 5 UDs beállítása. Nem kell semmit a virtualMachineProfile konfigurálása. Későbbi kiadásokban virtuális skála készletek valószínűleg több bérlők időtartamát, de most egy skála megadása esetén egyetlen elérhetősége meg.
