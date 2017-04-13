<properties
   pageTitle="Erőforrás-kezelő és klasszikus telepítési |} Microsoft Azure"
   description="Az erőforrás-kezelő telepítési modell és klasszikus közötti különbségek ismerteti (vagy Szolgáltatáskezelés) telepítési modell."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure erőforrás-kezelő klasszikus telepítési összehasonlítása: az erőforrások állapotát és üzembe modellek megértése

Ebben a témakörben tudnivalók: Azure erőforrás-kezelő és klasszikus telepítési modellek, az állapot erőforrásait, és hogy miért az erőforrások voltak üzembe helyezéséhez az egyiket. Az erőforrás-kezelő és klasszikus telepítési modellek jelenítik meg az Azure megoldások kezelése és telepítése két különböző módokon. Különböző API kétféle módon dolgozik velük, és a telepített erőforrások fontos eltérések is tartalmazhat. A két modellek, amelyek nem teljesen kompatibilisek egymással. Ez a témakör ismerteti azokat a különbségeket.

A telepítési és erőforrások kezelésének egyszerűsítése érdekében, a Microsoft összes új erőforrás erőforrás-kezelő használatát javasolja. Ha lehetséges azt javasoljuk, hogy meg újratelepítése meglévő erőforrásokat erőforrás-kezelő.

Ha új erőforrás-kezelő, érdemes először tekintse át a referenciacikk olyan kifejezéseket definiált az [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md).

## <a name="history-of-the-deployment-models"></a>A telepítési modellek előzmények

Azure eredetileg megadott csak a klasszikus telepítési modell. Ebben a modellben minden erőforrás létezik közé tartoznak független; nincs mód a csoportba a kapcsolódó források volt. Ehelyett kellett manuálisan nyomon követésére épülnek fel, hogy a megoldás vagy alkalmazás erőforrásokat, és ne feledje, hogy összehangolt kezelheti őket. Megoldást üzembe helyezéséhez kellett létrehozása az egyes erőforrásokhoz egyenként a klasszikus portálon keresztül, vagy hozzon létre egy parancsfájlt, hogy a megfelelő sorrendben összes erőforrás rendszerbe. Megoldás törlése egyenként törölje az egyes erőforrásokhoz volt. Sikerült egyszerűen nem alkalmazása és a kapcsolódó források hozzáférési szabályok frissítése. Végül, hogy nem sikerült alkalmazása a címkék erőforrások lássa el, amelyek segítségével adatokkal figyelemmel az erőforrások és a Számlázás kezelése.

2014-es, az Azure erőforrás-kezelő, amelyeket hozzá, amely a erőforráscsoport jelent meg. Erőforráscsoport erőforrás által megosztott egy közös életciklus tároló. Az erőforrás-kezelő telepítési modell több előnyöket nyújtja:

- Telepíthető, kezelése és a megoldás a szolgáltatások figyelheti a csoportként, hanem egyenként szolgáltatások kezelése ezek.
- Többször az életciklus során a megoldást üzembe helyez, és az erőforrások konzisztens állapotban van telepítve van.
- Alkalmazhat hozzáférés-vezérlés összes erőforrás az erőforrás csoportban, és ezek a házirendek automatikusan alkalmazza új erőforrások erőforráscsoport történő hozzáadásakor.
- Erőforrások logikailag rendszerezheti az előfizetése összes erőforrás címkéket alkalmazhat.
- A JavaScript objektum jelölés (JSON) a infrastruktúra definiálni a megoldás is használhatja. A JSON-fájlt nevezzük erőforrás-kezelő sablon.
- Megadhatja, hogy a függőségeket, így azokat a megfelelő sorrendben telepítik erőforrások között.

Erőforrás-kezelő hozzáadásakor összes erőforrás visszamenőleges hozzáadták alapértelmezett erőforrás csoportokat. Ha létrehozott egy erőforrás most klasszikus telepítési keresztül, az erőforrás automatikusan létrejön, hogy a szolgáltatás alapértelmezett erőforrás csoporton belül akkor is, ha nem adja meg a telepítési erőforráscsoport. Jó helyen jár csak a meglévő erőforrás csoporton belül nem jelenti, hogy az erőforrás az erőforrás-kezelő modell alakított. Hogyan kezeli az egyes szolgáltatásokhoz a a két környezetben modellek, a következő szakaszban áttekintjük. 

## <a name="understand-support-for-the-models"></a>Az adatmodellek támogatása ismertetése 

Milyen telepítési modellt az erőforrások kiválasztásakor három forgatókönyv tudatában kell lennie:

1. A szolgáltatás támogatja az erőforrás-kezelő, és csak egyetlen típust biztosít.
2. A szolgáltatás támogatja az erőforrás-kezelő, de két típusa - tartalmaz egy erőforrás-kezelő és klasszikus. Ebben az esetben csak a virtuális gépeken futó, tárterület-fiókokban és virtuális hálózatok vonatkozik.
3. A szolgáltatás nem támogatja az erőforrás-kezelő.

Megtudhatja, hogy támogatja-e erőforrás-kezelő szolgáltatást, olvassa el a [erőforrás-kezelő támogat a szolgáltató](resource-manager-supported-services.md)című témakört.

Ha a használni kívánt szolgáltatás nem támogatja az erőforrás-kezelő, továbbra is Önnek kell klasszikus telepítési használatával.

Ha a szolgáltatás lehetővé teszi az erőforrás-kezelő és a **nem** virtuális gép, a tárterület-fiók vagy a virtuális hálózaton, erőforrás-kezelő bármely komplikációk nélkül is használhatja.

A virtuális gépeken futó, tároló fiókok és virtuális hálózatok Ha az erőforrás készült klasszikus példányban keresztül továbbra is Önnek kell klasszikus műveletek révén működnek. Ha a virtuális gép, tárterület-fiók vagy egy virtuális hálózaton keresztül erőforrás-kezelő telepítési jött létre, továbbra is Önnek kell erőforrás-kezelő műveletek használatával. Ha előfizetése tartalmazza az erőforrás-kezelő és klasszikus telepítési keresztül létrehozott erőforrások kombinálja a különbséget elérheti áttekinthetőbb legyen. Az erőforrások kombinációval nem várt eredmények hozhat létre, mert az erőforrások nem támogatják a ugyanazokat a műveleteket.

Egyes esetekben az erőforrás-kezelő parancs meghallgathatja klasszikus telepítési keresztül létrehozott erőforrások adatainak, vagy például klasszikus erőforrás áthelyezése egy másik erőforrás csoport-rendszergazdai feladatot végre tud hajtani. De nem ezekben az esetekben meg kell adnia a benyomást, hogy a típus támogatja-e erőforrás-kezelő műveletek. Tegyük fel például, amely tartalmazza a klasszikus telepítési létrehozott virtuális gép erőforráscsoport. Ha az alábbi erőforrás-kezelő PowerShell-parancsot futtatja:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Vissza a virtuális gépen:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Az erőforrás-kezelő parancsmag **Get-AzureRmVM** azonban csak virtuális gépeken futó erőforrás-kezelő rendszerbe adja vissza. A következő parancsot a klasszikus telepítési keresztül létrehozott virtuális gépen nem ad vissza.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Csak erőforrások létrehozott erőforrás-kezelő támogatási címkék között. Klasszikus erőforrások címkék nem alkalmazható.

## <a name="resource-manager-characteristics"></a>Erőforrás-kezelő jellemzők

Segít megérteni a két modellek, tanulmányozzuk az erőforrás-kezelő típusok jellemzői:

- A létrehozott az [Azure portálon](https://portal.azure.com/)keresztül.

     ![Azure portál](./media/resource-manager-deployment-model/portal.png)

     Számítási, tárolására és erőforrások hálózat Ha az erőforrás-menedzserek vagy a hagyományos telepítési használata választógombot. Jelölje ki az **Erőforrás-kezelő**.

     ![Erőforrás-kezelő telepítési](./media/resource-manager-deployment-model/select-resource-manager.png)

- Azure PowerShell-parancsmagok az erőforrás-kezelő verziójával létrehozott. Ezeket a parancsokat a formátum *Igei-AzureRmNoun*van.

        New-AzureRmResourceGroupDeployment

- A létrehozott keresztül az [Azure erőforrás-kezelő REST API -t](https://msdn.microsoft.com/library/azure/dn790568.aspx) a többi műveletekhez.

- A létrehozott keresztül Azure CLI parancsok a **arm** módra.

        azure config mode arm
        azure group deployment create 

- Az erőforrás típusa nem tartalmaz **(klasszikus)** nevét. Az alábbi képen látható a típus, **tárterület**-fiókként.

    ![Web App alkalmazásban](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Klasszikus telepítési jellemzők

A klasszikus telepítési modell is előfordulhat, hogy tudja, mint a Szolgáltatáskezelés modell.

A klasszikus telepítési modell létrehozott erőforrások megosztása a következő tulajdonságokat:

- A [Klasszikus portal](https://manage.windowsazure.com) segítségével létrehozott

     ![Klasszikus portál](./media/resource-manager-deployment-model/classic-portal.png)

     Másik lehetőségként az Azure-portálra, és adja meg a **Klasszikus** telepítésének (számítási, a tárhely és a hálózat).

     ![Klasszikus telepítési](./media/resource-manager-deployment-model/select-classic.png)

- Azure PowerShell-parancsmagok Szolgáltatáskezelés verziójában létrehozott. Ezek a parancs neve formátuma az *Igei-AzureNoun*.

        New-AzureVM 

- A [Szolgáltatás felügyeleti REST API -t](https://msdn.microsoft.com/library/azure/ee460799.aspx) a többi műveletek keresztül létrehozva.
- A létrehozott keresztül Azure CLI parancsok **asm** módra.

        azure config mode asm
        azure vm create 

- Az erőforrás típusa **(klasszikus)** szerepel a név. Az alábbi képen látható a típus, **tároló**fiókként (klasszikus).

    ![klasszikus típusa](./media/resource-manager-deployment-model/classic-type.png)

Az Azure-portálra klasszikus telepítési keresztül létrehozott erőforrások kezelésére használható.

## <a name="changes-for-compute-network-and-storage"></a>Számítási, hálózati és tárterület módosítása

Az alábbi ábra a számítási, hálózati és tárolását forrásokkal erőforrás-kezelő jeleníti meg.

![Erőforrás-kezelő architektúra](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Megjegyzés: az erőforrások között a következő kapcsolatok:

- Erőforrások erőforráscsoport tartalmaz.
- A virtuális gép attól függ, hogy egy adott tároló fiókot a lemezt tárolását blob-tárolóhoz (kötelező) a tárolási erőforrás szolgáltatóval definiálva.
- A virtuális gép definiált a hálózati erőforrás-szolgáltató (kötelező), és egy elérhetőségének beállítása a számítási erőforrás szolgáltatóban definiált (nem kötelező) egy adott hálózati hivatkozik.
- A hálózati kártya hivatkozik, a virtuális gép kiosztott IP-cím (kötelező), a alhálózat virtuális hálózat a virtuális gép (kötelező), és a hálózati biztonsági csoport (nem kötelező).
- A virtuális hálózaton belül alhálózat hivatkozások hálózati biztonsági csoport (nem kötelező).
- A betöltés terheléselosztó példány hivatkozik, amely egy virtuális gép (nem kötelező) a hálózati tartalmazza, és a betöltés terheléselosztó nyilvános vagy magánjellegű IP-cím (nem kötelező) hivatkozik IP-címek kódmentes készlete.

Az alábbiakban az összetevők és a viszony klasszikus telepítéshez:

![klasszikus architektúra](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

A klasszikus megoldás virtuális géphez üzemeltető tartalmazza:

- Szükséges felhőalapú szolgáltatás, amely végpontként virtuális gépeken futó (számítási) elhelyezésére tároló. Virtuális gépeken futó automatikusan ellátni a hálózati kapcsolat kártya és Azure által megadott IP-címet. Emellett a felhőbeli szolgáltatástól egy külső betöltés terheléselosztó példány, egy nyilvános IP-cím és alapértelmezett végpontok engedélyezése a távoli asztali és a távoli PowerShell a Windows-alapú virtuális gépeken futó és a biztonságos rendszerhéj (SSH) forgalmat a Linux-alapú virtuális gépeken futó tartalmaz.
- Egy szükséges tárterület-fiókot, amely a virtuális géphez, beleértve az operációs rendszer, ideiglenes és további adatokat lemez (tároló) VHD tárolja.
- Egy választható virtuális hálózat, amely végpontként további tároló, amelyben alhálózati szerkezetbe és kijelöli a alhálózat, amelyen a virtuális gép található (hálózat).

Az alábbi táblázat ismerteti, hogyan kezeljék a számítási, hálózati és tárolását erőforrás szolgáltatók változásai:

 Elem | Klasszikus | Erőforrás-kezelő
 ---|---|---
| Virtuális gépeken futó felhőalapú szolgáltatáshoz |  Felhőbeli szolgáltatástól az elérhetőség szükséges a platform-és terheléselosztás virtuális gépeken futó tartó tároló volt. | Felhőalapú szolgáltatás már nem szükséges hozhat létre az új minta segítségével virtuális gép objektum. |
| Virtuális hálózatok | A virtuális hálózati nem kötelező, a virtuális gépen. Ha szerepel, nem a virtuális hálózat üzembe az erőforrás-kezelő. | Virtuális gép virtuális hálózatot, hogy az erőforrás-kezelő lett telepítve van szükség. |
| Tárterület-fiókok | A virtuális gép az operációs rendszer, ideiglenes és további adatokat lemezt a VHD tároló tárterület-fiók szükséges. | A virtuális gép tárolni a lemezt blob-tárolóhoz tárterület-fiók szükséges. |
| Elérhetőség beállítása | Elérhetőség az platformra azonos "AvailabilitySetName" konfigurálása a virtuális gépeken volt jelzi. Hibafa-tartományok maximális száma 2 volt. | Elérhetőség be kapcsolva Microsoft.Compute szolgáltató által elérhetővé tett erőforrás. Az elérhetőség csoportosító virtuális gépeken futó magas elérhetősége igénylő szerepelnie kell. Hibafa-tartományok maximális számát ettől 3. |
| Affinitás csoportok | Affinitás csoportok volt szükség hozhat létre virtuális hálózatok. Azonban területi virtuális hálózatok bevezetésével, amely nem volt szükség eltűnt. |Azure erőforrás-kezelővel elérhetővé tett API-khoz leegyszerűsítése affinitás csoportok fogalma nem létezik. |
| Terheléselosztás    | A egy felhőalapú szolgáltatásba létrehozása egy implicit terheléselosztó biztosít a virtuális gépeken futó rendszerbe. | A terheléselosztó Microsoft.Network szolgáltató által elérhetővé tett erőforrás. A virtuális gépeken futó osztható el kell, hogy az elsődleges hálózati felhasználói felületén a terheléselosztó kell lennie hivatkozik. Belső és külső, a terheléselosztókkal is lehet. A betöltés terheléselosztó példányt az IP-címek, amely egy virtuális gép (nem kötelező) a hálózati tartalmazza, és a betöltés terheléselosztó nyilvános vagy magánjellegű IP-cím (nem kötelező) hivatkozik kódmentes készlete hivatkozik. [Tudjon meg többet.](../articles/resource-groups-networking.md) |
|Virtuális IP-cím | Cloud Services egy alapértelmezett virtuális (ezek olyan virtuális IP-cím) jelenik meg, ha egy virtuális megjelenik egy felhőalapú szolgáltatásba. A virtuális IP-címet a cím, a implicit terheléselosztó társított.    | Nyilvános IP-címet a Microsoft.Network szolgáltató által elérhetővé tett erőforrás. Statikus (foglalt) és a dinamikus, a nyilvános IP-címet is lehet. Dinamikus nyilvános IP-címei egy terheléselosztó tevékenységekhez hozzárendelhető. Nyilvános IP-címei biztonsági csoportok védhetők. |
|Fenntartott IP-cím|   Lefoglalása Azure-ban IP-címet, és társítása egy felhőalapú szolgáltatásba, annak érdekében, hogy az IP-cím öntapadós.   | Nyilvános IP-cím "Statikus" üzemmódban hozhatja létre, és azt lehetőséget nyújt a ugyanazt a "fenntartott IP-cím". Statikus nyilvános IP-címei csak rendelhetők egy terheléselosztó pillanatban. |
|Nyilvános IP-cím (PIP) / virtuális | Nyilvános IP-címeket is rendelve egy virtuális közvetlenül. | Nyilvános IP-címet a Microsoft.Network szolgáltató által elérhetővé tett erőforrás. Statikus (foglalt) és a dinamikus, a nyilvános IP-címet is lehet. Jó helyen jár csak dinamikus nyilvános IP-címei rendelhetők a hálózati kapcsolat egy nyilvános IP egy virtuális pillanatban eléréséhez. |
|Végpontok| Beviteli végpontok szükség egy virtuális gépen nyitva adatkapcsolat egyes portok be kell beállítania. Kapcsolódás virtuális gépeken futó beviteli végpontok beállítását végzi a közös módok közül. | Hálózati Címfordítást a bejövő szabályok terheléselosztókkal a azonos lehetőséget, ha engedélyezi a VMs csatlakozhat adott portok végpontjait eléréséhez lehet beállítani. |
|Tartománynév| Egy felhőalapú szolgáltatásba volna egy implicit globálisan egyedi DNS-nevét. Példa: `mycoffeeshop.cloudapp.net`. | DNS-neveket is választható paraméterek egy nyilvános IP-cím erőforráshoz megadott. A teljesen minősített tartománynév van, a következő formában – `<domainlabel>.<region>.cloudapp.azure.com`. |
|Hálózati kapcsolatok | Elsődleges és másodlagos hálózat felülete és annak tulajdonságait virtuális gép a hálózati konfigurálásról lettek megadva. | Hálózati kapcsolat Microsoft.Network szolgáltató által elérhetővé tett erőforrás. A hálózati kapcsolat életciklusáról nem tartozik, virtuális géphez. A virtuális gép kiosztott IP-cím (kötelező), a virtuális hálózat a virtuális gép (kötelező), és a hálózati biztonsági csoport (nem kötelező) a alhálózat hivatkozik. |

Kapcsolódás virtuális hálózatokat különböző telepítési modellekből kapcsolatos további tudnivalókért lásd: [Kapcsolódás virtuális hálózatokat különböző telepítési modellekből a portálon](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Erőforrás-kezelő klasszikus áttelepítése

Ha készen áll az erőforrások áttelepítése klasszikus telepítési erőforrás-kezelő telepítési, olvassa el:

1. [Műszaki mély merülési a platform támogatott áttelepítési a klasszikus az Azure erőforrás-kezelő](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Támogatott platform áttelepítési IaaS forrásai a klasszikus az Azure erőforrás-kezelő](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Áttelepítése IaaS erőforrások klasszikus az Azure erőforrás-kezelő Azure PowerShell használatával](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Áttelepítése IaaS erőforrások klasszikus az Azure erőforrás-kezelő Azure CLI használatával](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Gyakori kérdések

**Létrehozhatok-e egy virtuális gép-kezelővel Azure virtuális hálózati vagy klasszikus telepítési használatával létrehozott tároló fiók bevezetését tervezi?**

Nem támogatott. Erőforrás-kezelő Azure virtuális gép üzembe klasszikus telepítési jött létre virtuális hálózatba nem használható.

**Létrehozhatok-e az Azure kiszolgálói API-ja használatával létrehozott felhasználói képből Azure erőforrás-kezelő használatával virtuális gép?**

Nem támogatott. Azonban a virtuális fájlok másolása a szolgáltatás kiszolgálói API-ja használatával létrehozott tárterület-fiókból, és felveheti azokat egy új fiók létrehozása az Azure erőforrás-kezelővel.

**Mi az, hogy az előfizetés kvótája hatással?**

A virtuális gépeken futó, a virtuális hálózatokat, és a tárterület-fiókok létrehozása az Azure erőforrás-kezelővel vonatkozó más kvóták elkülönülnek. Egyes előfizetések kapja meg az új API-k használata szükséges erőforrások létrehozása kvóták. Erről további információ a további kvótákat [Itt](../articles/azure-subscription-service-limits.md).

**E továbbra is használhatja az automatikus parancsfájlok kialakítási virtuális gépeken futó virtuális hálózatok és az erőforrás-kezelő API-k tárterület-fiókokat?**

Az automatizálási és a már készített korábban parancsprogramok továbbra is a meglévő virtuális gépeken futó, az Azure Szolgáltatáskezelés mód alapján létre virtuális hálózatok működik. A parancsfájlok azonban kell frissíteni kell használni az új séma ugyanazokat az erőforrásokat, az erőforrás-kezelő módban hozhat létre.

**Hol találom meg a példák az erőforrás-kezelő Azure-sablonok?**

A sablonok starter teljes készletével [Azure erőforrás-kezelő quickstart útmutató sablonok](https://azure.microsoft.com/documentation/templates/)megtalálható.

## <a name="next-steps"></a>Következő lépések

- Sablon, amely definiálja egy virtuális gép, a tárterület-fiók és a virtuális hálózati kibocsátása végigvezetik, olvassa el az [erőforrás-kezelő sablon forgatókönyv](resource-manager-template-walkthrough.md).
- A parancsok üzembe helyezése a sablon talál további [Deploy egy alkalmazást, az erőforrás-kezelő Azure sablonnal](resource-group-template-deploy.md).
