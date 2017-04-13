<properties
    pageTitle="A virtuális gép skála készletek-alkalmazások terjesztése |} Microsoft Azure"
    description="A virtuális gép skála készletek-alkalmazások terjesztése"
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>A virtuális gép skála beállítja az alkalmazások terjesztése

-Alkalmazást egy virtuális skála megadása a szokásos telepítik három módszerek egyikét:

- Új szoftver telepítése platform képként telepítési időben. Ebben a környezetben platform képként operációs rendszer képet az Azure piactéren elérhető, például a Ubuntu 16.04, Windows Server 2012 R2 stb.

A [Virtuális bővítmény](../virtual-machines/virtual-machines-windows-extensions-features.md)platform kép új szoftver telepíthető. A virtuális kiterjesztés szoftver fut, amikor egy virtuális telepítve van. Bármilyen kódot, amely tetszik, egyéni parancsfájl kiterjesztéssel telepítési időben futtathatja. [Az alábbi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) példában Azure erőforrás-kezelő sablon két virtuális kiterjesztésű: Linux egyéni parancsfájl bővítmény telepítése Apache és PHP és diagnosztikai bővítmény Azure Automatikus méretezéssel által használt teljesítményadatokat elhelyezése.

Az eljárás előnye elkülönítésének szintű rendelkeznek az alkalmazás kódját, és az operációs rendszer között, és külön-külön karbantarthatja az alkalmazás. Természetesen, azt jelenti, hogy még több mozgó, és virtuális telepítési idő hosszabb lehet, ha van sokkal töltse le és állítsa be a parancsfájl.

**Ha az egyéni parancsfájl bővítmény parancs (például a jelszó) a bizalmas információkat sikeres, ne felejtse el adja meg a `commandToExecute` a a `protectedSettings` attribútum helyett egyéni parancsfájl-bővítmény a `settings` attribútum.**

- Hozzon létre egy egyéni virtuális képet, amelynek szövegében szerepel egy egyetlen virtuális az operációs rendszer és az alkalmazás is. Itt skála megadása egy sor olyan megőrzéséhez, amelyeknek Ön által létrehozott képből másolt VMs áll. Ezt a megközelítést nem szükséges külön beállítás virtuális telepítési időben. Azonban a `2016-03-30` verzió virtuális skála beállítása (és korábbi verziók), az operációs rendszer lemezen skála megadása a VMs korlátozódik, egyetlen tárterület-fiókjába. Így skála meg van-e legfeljebb 40 VMs, nem pedig a méretezés-/ 100 virtuális platform képekkel korlát beállítása. További információt a [Skála megadása tervezés áttekintése](./virtual-machine-scale-sets-design-overview.md) című témakörben találhat.

- Egy platform vagy egy egyéni képe, amely tulajdonképpen egy tároló host telepítése, és telepítse az alkalmazást, mint egy vagy több tárolók, hogy Ön kezeli-orchestrator vagy konfigurációs kezelése eszközzel. A jó megoldás az ezzel a módszerrel kapcsolatos, az alkalmazási réteg át a felhőalapú infrastruktúrájának van kivett, és a karbantarthatja őket külön-külön.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Mi történik, amikor egy virtuális skála megadása méretezze át a meg?

Amikor felvesz egy vagy több VMs beállítása a kapacitás növelésével skálával – akár kézi és Automatikus méretezéssel – az alkalmazás automatikusan telepítve van. A példa a skála megadása van az definiált bővítményeket, a Futtatás meg egy új virtuális minden alkalommal, amikor létrehozása. Skála megadása egyéni kép alapul, minden új virtuális esetén a forráskép egyéni másolatát. Ha a méretezés VMs tároló hosts, majd lehet, hogy a tárolók egy egyéni parancsfájl bővítmény betöltése indítási kódot, vagy kiterjesztése előfordulhat, hogy telepíteni, amely egy fürt orchestrator (például Azure tároló szolgáltatás) regisztrálja ügynökszoftvert.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Hogyan kezelheti alkalmazás frissítések virtuális skála készletben?

Virtuális skála készletben alkalmazás frissítéseinek három fő az alábbi módszerek hajtsa végre a három előző alkalmazás telepítési módszer áll rendelkezésére:

* Virtuális kiterjesztésű frissíteni. A virtuális skála megadása hajtja végre a rendszer minden alkalommal, amikor egy új virtuális telepíti, egy meglévő virtuális definiált virtuális kiterjesztést reimaged van, vagy egy virtuális bővítmény frissül. Ha módosítania kell az alkalmazást, közvetlenül a életképes megközelítés a bővítmények – az alkalmazás frissítésére – egyszerűen frissítse a bővítmény definícióját. Egy egyszerű ehhez módja a fileUris, mutasson az új szoftver módosításával.

* A saját kép megváltoztatható megközelítés. Amikor az alkalmazás (vagy alkalmazás összetevők) egy virtuális képbe süteményvásár összpontosíthat egy megbízható folyamat fejlesztése, tesztelése és telepítési képre automatizálásához fejlesztésére. A szakaszos skála megadása éles gyors lecserélése megkönnyítése érdekében a architektúra tervezése Ezt a megközelítést jó példa akkor [működik Azure Spinnaker illesztőprogram](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Csomagoló és Terraform Azure erőforrás-kezelő támogatási is, a képek meghatározásával "kódok", és Azure-ban elkészíteni őket, majd használja a virtuális a skála megadása. Jó helyen jár ezzel így válna piactér képek, ahol bővítmények/egyéni parancsfájlok válnak fontosabb óta nem közvetlenül kezelni a piactérről bittel problémákat okozhatnak.

* Frissítse a tárolók. Absztrakt felett a felhőalapú infrastruktúrájának szintre alkalmazás életciklus-kezelése például encapsulating alkalmazások és az alkalmazás összetevők tárolók be, és ezek tárolók tároló orchestrators és a konfigurációs menedzserek például tölthetők le/Puppet kezelése.

A méretezés VMs beállítása, majd a tárolók stabil szubsztrát válnak, és csak a alkalmi biztonsági és OS kapcsolatos frissítések kér. Említett, az Azure tároló szolgáltatás példája egy jó ezt a megközelítést véve és foglalhatja keretbe szolgáltatás kiépítése.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Hogyan tegye meg bevezetésére OS frissítés frissítés tartományok?

Tegyük fel, frissítse az operációs rendszer képe a virtuális skála megadása haladási miközben szeretné. Egy erre módja a virtuális frissítése képek egyszerre csak egy virtuális. A PowerShell vagy Azure CLI megteheti. Nincsenek frissítése a virtuális skála modelljének beállítása (hogyan konfigurációját van megadva), és hiba "Kézi frissítés" hívások, az egyes VMs külön parancsot.

[Az alábbi](https://github.com/gbowerman/vmsstools) példa Python parancsfájlt, amely egy virtuális skála megadása egy frissítés tartomány frissítése egyszerre az előfizetőknek. (Bővítve: további fogalom, mint a megerősített gyártási kész megoldást igazolása – érdemes lehet hozzáadni az egyes hiba-ellenőrzési stb.).
