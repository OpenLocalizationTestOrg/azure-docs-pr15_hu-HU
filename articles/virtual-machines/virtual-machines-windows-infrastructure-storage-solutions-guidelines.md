<properties
    pageTitle="Tárterület-megoldások irányelvek |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve az Azure infrastruktúrájának szolgáltatásai tároló megoldások telepítése."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Tárterület infrastruktúra útmutató

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál tároló igényeinek és tervezési szempontjait az optimális virtuális gép (virtuális) teljesítmény elérése érdekében.


## <a name="implementation-guidelines-for-storage"></a>Tárterület végrehajtása szabályai

Határozatok:

- Szükség van a terhelést normál vagy prémium tároló használni?
- Szükség van lemezcsíkozás lemezt 1023 GB-nál nagyobb létrehozása?
- Szükség van a terhelést az optimális I/O teljesítmény elérése érdekében lemezcsíkozás?
- Milyen beállítása a tárterület-fiókok van szüksége az informatikai terhelést vagy infrastruktúra szeretné üzemeltetni?

Feladatok:

- Tekintse át az alkalmazások telepít, és tervezése a megfelelő számot és a tárterület-fiókok típusa I/O igényeivel.
- Hozzon létre a tárhely fiókot az elnevezésük is egységes. Azure PowerShell és a portálon is használhatja.


## <a name="storage"></a>Tárhely

Azure tároló része a fő telepítésével és virtuális gépeken futó (VMs) és az alkalmazások kezelése. Azure tárhely a fájlban lévő adatok, strukturálatlan adatokat, és az üzenetek tárolásához szolgáltatásokat nyújt, és a VMs támogató infrastruktúra részét.

Létezik kétféle tároló fiókok VMs támogatása:

- Szabványos tároló fiókok joga blob-tárolóhoz (a Azure virtuális lemezt tárolására szolgál), táblatároló, várólista tárterület és tárhely.
- [Prémium tároló](../storage/storage-premium-storage.md) fiókok előadása I/O intenzív munkaterhelésekből, például az SQL Server-AlwaysOn fürt nagy teljesítményű, alacsony-késés lemezt támogatása. Prémium tárolási jelenleg csak az Azure virtuális lemezre támogatja.

Azure VMs az operációs rendszer lemez, ideiglenes lemezen és nulla vagy több választható adatok lemezt hoz létre. Az operációs rendszer lemez és adatok lemezt lap Azure BLOB, míg a ideiglenes lemez helyben tárolt a csomóponton, ahol a gép él. Az alkalmazások csak az használja ideiglenes lemez állandó adatok a virtuális előfordulhat, hogy telepíthető át a hosts során a karbantartás esemény közötti tervezésekor gondoskodik. Az ideiglenes lemezen tárolt adatokat elveszhetnek.

A mögöttes Azure tároló környezet annak érdekében, hogy az adatok marad a tervezett karbantartás vagy hardveres hibák ellen védett által biztosított tartósság és magas elérhetőségét. A tároló Azure környezetben tervezésekor, való replikáció virtuális tároló választhat:

- helyi meghajtóra belül egy adott Azure adatközponthoz
- egy adott területen belül Azure adatközpontokkal keresztül
- Azure adatközpontokkal keresztül különböző területei között

Erről [További információt a magas elérhetőség replikációs beállításai közül](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operációs rendszer lemezen és adatokat tartalmazó lemezen van maximális mérete 1023 gigabájt (GB). Blob maximális mérete 1024 GB, és a metaadatokat (élőláb) a virtuális fájl (egy GB a 1024<sup>3</sup> bájt), amely állhat. A Windows Server 2012 tárhelyek művelet túllépje ezt a korlátot összeadása során adatok lemezt a virtuális logikai kötet 1023GB-nál nagyobb mutathatja által is használhatja.

Néhány korlátozás van érvényben méretezhetőség a Azure tároló telepítések tervezésekor – lásd: [Microsoft Azure-előfizetést és a szolgáltatás korlátozások, kvótákat, és korlátozások](azure-subscription-service-limits.md#storage-limits) további információt. [Azure tároló méretezhetőség és a teljesítmény célok](../storage/storage-scalability-targets.md)is megjelenik.

Az alkalmazás tárolója tárolhatja objektum strukturálatlan adatokat, például a dokumentumok, képek, biztonsági másolatokat, konfigurációs adatok, naplók stb. blob-tárolóhoz használatával. Az alkalmazás a virtuális csatolni virtuális lemezre írása, nem pedig annak a az alkalmazást közvetlenül az Azure blob-tárolóhoz írhat. BLOB-tárolóhoz [hideg- és melegvíz tároló rétegek](../storage/storage-blob-storage-tiers.md) attól függően, hogy az elérhetőség igények és az költségkényszerek lehetőséget is biztosít.


## <a name="striped-disks"></a>Sávos lemez
Amellett próbál létrehozásához 1023 GB-nál nagyobb sok esetben csíkozást verzióval adatok lemezt növeli a teljesítményt azáltal, hogy több BLOB tárolt adatait egyetlen kötet biztonsági. Csíkozást, és az írási, és olvassa el az adatok egyetlen logikai lemezről szükséges I/O párhuzamosan folytatódik.

Azure ró vonatkozó korlátok adatok hozza létre és sávszélességet érhető el, virtuális méretétől függően. Részletekért olvassa el [a virtuális gépeken futó méretét](virtual-machines-windows-sizes.md).

Alkalmazás használatakor lemezcsíkozás az Azure adatok lemezre, vegye figyelembe az alábbi útmutatásokat:

- Adatok lemezt mindig kell lennie a maximális méret (1023 GB).
- Csatolni virtuális méretének megengedett maximális adatok lemezt.
- Tárhelyek használja.
- Kerülje a gyorsítótár-beállítások Azure adatok lemez (gyorsítótár-házirend = nincs).

További tudnivalókért lásd: [tárhelyek – tervezése a teljesítmény elérése érdekében](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).


## <a name="multiple-storage-accounts"></a>Több tárhely fiók

A tároló Azure környezetben tervezésekor több tárterületet fiók használható VMs számaként nő rendszerbe. Ezzel az elemzéssel elosztása az I/O, az alapul szolgáló Azure tároló infrastruktúra megőrzéséhez a VMs és alkalmazások optimális teljesítmény. Tervezésekor telepít az alkalmazásokat, fontolja meg az egyes virtuális van I/O követelmények és mérleg meg ezeket a VMs Azure tárolási fiókok között. Próbálja meg a VMs igénylő csak egy vagy két tárolási fiókokhoz magas I/O elkerülje.

További információt a különböző Azure-tárolási lehetőségek a I/O funkcióinak és néhány méretkorlát javasoljuk című témakörben [Azure tároló méretezhetőség és a teljesítmény célokat](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 