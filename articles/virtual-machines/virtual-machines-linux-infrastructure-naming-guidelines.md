<properties
    pageTitle="Útmutatások elnevezési infrastruktúra |} Microsoft Azure"
    description="Tudjon meg többet a fő tervezéséhez és kivitelezéséhez elnevezési szabályai az Azure infrastruktúrájának szolgáltatásai."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Infrastruktúra elnevezési útmutató

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál hogyan kell a különböző Azure erőforrások logikai és könnyen azonosíthatónak erőforráskészletek össze a környezet közötti elnevezési szabályai megközelíthető.

## <a name="implementation-guidelines-for-naming-conventions"></a>Elnevezési szabályai végrehajtása szabályai

Határozatok:

- Mik azok az Azure erőforrások elnevezési szabályai?

Feladatok:

- Adja meg a elő-/ utótagok konzisztencia megőrzéséhez végig az erőforrások használni.
- Meg szeretné globálisan egyedi kötelező megadni tároló fiók nevét.
- A dokumentum használatos, valamint telepítések közötti összhangot érintett feleknek terjesztése az elnevezésük is egységes.

## <a name="naming-conventions"></a>Elnevezési szabályai

Rendelkeznie kell elnevezni jó helyen semmit az Azure-ban létrehozása előtt. Elnevezési gondoskodhat arról, hogy az összes erőforrás kiszámíthatóan nevét, ezzel megkönnyíti alsó társított ezek az erőforrások kezelése a rendszergazdai terhet.

Előfordulhat, hogy az egész szervezet vagy egy adott Azure előfizetés vagy a fiókban definiált elnevezési szabályai meghatározott kövesse választhatja. Bár egyszerűen könnyűvé szervezetekben implicit szabályok létrehozásához, az Azure erőforrások használatakor, engedélyezni szeretné a csoportoknak a közös munka Azure osztályozási szüksége.

Egy sor olyan látható elnevezési szabályai azzal. Dolgot kell néhány elnevezési szabályokat, amelyek különböző kivágása állítja vonatkozó szabályok.

## <a name="affixes"></a>Kell

Elnevezési definiálása tekinti meg, mint egy döntés, hogy utótag-e a:

- A név (előtag) elejére
- A név (utótag) végére

Íme például egy erőforráscsoport a két lehetőség nevet a `rg` elhelyezi:

- Rg-Webappban (előtag)
- Webalkalmazás-Rg (utótag)

Különböző tényezők, amelyek bemutatják, az adott erőforrás elő-/ utótagok is hivatkozhat. Az alábbi táblázatban néhány példát mutat jellemző felhasználási módjuk látható.

| Méretarány                               | Példák                                                               | Jegyzetek                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Környezet                          | a fejlesztői stg, gyártási rendelés                                                         | Attól függően, hogy a cél és az egyes környezetekben nevét.                                                     |
| Hely                             | usw (nyugati Egyesült Államokbeli), (2 kelet-amerikai) használata                                         | Attól függően, hogy az az Adatközpont vagy a régió, a szervezet terület.                               |
| Azure összetevő, a szolgáltatás vagy a termékkulcs | Az erőforráscsoport virtuális hálózat VNet rg                        | Attól függően, hogy a terméket, amelyhez az erőforrás nyújt támogatást.                                          |
| Szerepkör                                 | DB, az alkalmazás, a webes                                                           | A virtuális gép szerepkörű.                                                              |
| Példány                             | 01, 02, 03, stb.                                                       | Az erőforrások, amelyek egynél több példányban van. Ha például terheléselosztás webkiszolgálón a egy felhőalapú szolgáltatásba. |


A elnevezési szabályai létrehozásakor győződjön meg arról, hogy azok jól megye mely elő-/ utótagok használni az egyes: az erőforrás, és milyen pozícióban (előtag és utótag).

## <a name="dates"></a>Dátumok

Gyakran fontos, hogy az erőforrás a neve létrehozásának dátumát. Azt javasoljuk, hogy a ÉÉÉÉHHNN dátumformátumot. Ez a formátum biztosítja, hogy nem csak a teljes dátum rögzítését, de az is, hogy mappanevek csak azon a napon különböznek egymástól két erőforrások vannak rendezve, betűrend szerint és időrendi sorrendben.

## <a name="naming-resources"></a>Erőforrások elnevezése

Minden egyes típusának megadásához az elnevezésük is egységes erőforrás, amely nevek hozzárendelése az egyes erőforrásokhoz létrehozott meghatározó szabályok kell rendelkeznie. Ezeket a szabályokat kell alkalmazása az összes típusú erőforrások, például:

- Az előfizetések
- Fiókok
- Tárterület-fiókok
- Virtuális hálózatok
- Alhálózat
- Elérhetőség beállítása
- Erőforrás-csoportok
- Virtuális gépeken futó
- Végpontok
- Hálózati biztonsági csoportok
- Szerepkörök

Ahhoz, hogy a név nyújt elegendő információt, megállapíthatja, hogy melyik erőforrásnézethez, amire hivatkozik, leíró nevét kell használni.

## <a name="computer-names"></a>Számítógép neve

Amikor létrehoz egy virtuális gép (virtuális), az Azure igényel egy virtuális legfeljebb 64 karakter hosszú nevet, amellyel az erőforrás neve. Azure használja ugyanazt a nevet a virtuális operációs rendszereken. Azonban ezeket a neveket esetleg nem mindig ugyanazok.

Ha egy virtuális jön létre, amely már tartalmaz egy operációs rendszer .vhd kép fájlból, az Azure virtuális nevét eltérhetnek a virtuális operációs rendszer számítógép nevét. Ebben az esetben is adhat megnehezítheti fokú virtuális-kezelés tehát nem javasoljuk. Rendelje hozzá az Azure virtuális erőforrás neve megegyezik az operációs rendszer adott virtuális gép hozzárendeli számítógépnevét.

Azt javasoljuk, hogy az Azure virtuális neve megegyezik az alapul szolgáló operációs rendszer számítógép nevét.

## <a name="storage-account-names"></a>Tárterület fióknevét

Tárterület-fiókok van speciális szabályokat a nevüket. Csak használhatja a kisbetűket és számokat. [Tárterület-fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) talál további információt. Emellett a tárhely fióknév core.windows.net, a globálisan érvényes és egyedi DNS nevét kell lennie. Például a tárterület-fiók neve mystorageaccount, ha a következő eredményül kapott DNS nevek egyedinek kell lennie:

- mystorageaccount.BLOB.Core.Windows.NET
- mystorageaccount.TABLE.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 