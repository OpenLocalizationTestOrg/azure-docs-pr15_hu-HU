<properties 
   pageTitle="Inaktiváljon StorSimple eszköz |} Microsoft Azure"
   description="Megtudhatja, hogy miként StorSimple eszközt eltávolít a szolgáltatás által először inaktiválása, és kattintson a törlés."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Inaktiváljon StorSimple eszköz

## <a name="overview"></a>– Áttekintés

Előfordulhat, hogy ki szeretne venni a szolgáltatás ki StorSimple eszköz (például akkor, ha cseréje vagy frissítése az eszközön, vagy ha már nem használ StorSimple). Ez a helyzet, ha szüksége lesz az eszköz inaktiválásához, mielőtt ki is törölheti azt. Inaktiválásával folyamat az eszköz és a megfelelő StorSimple kezelő szolgáltatás között. Ebből az oktatóanyagból megtudhatja, hogyan szolgáltatás első inaktiválásával, és kattintson a törlés StorSimple eszköz eltávolítása. 

Ha inaktiválta egy eszközt, az eszközön helyben tárolt adatokról már nem lesznek elérhetők. Csak az adatokat, az eszköz a felhőben tárolt társított állíthatók helyre.  

>[AZURE.WARNING] Az inaktiválás végleges művelet, és nem vonható vissza. Inaktív eszköz nem lehet regisztrálni a StorSimple Manager szolgáltatással, kivéve, ha először a gyár alaphelyzetbe. 
>
>A gyár folyamat törli az eszközén helyben tárolt adatok alaphelyzetbe állítása. Ezért fontos, hogy Ön előtt végezze el a felhőben projektjeibe, és minden adat inaktiválta egy eszközt. Ez lehetővé teszi, hogy az összes adatot helyreállítása későbbi szakaszában.

Ebből az oktatóanyagból megtudhatja, hogy miként:

- Inaktiválta egy eszközt, és az adatok törlése
- Egy eszközt inaktiválja, majd az adatok megőrzése

Azt is bemutatja, hogyan inaktiválására és törlés működik a virtuális StorSimple eszközön.

>[AZURE.NOTE] Csak inaktiválta egy StorSimple fizikai vagy virtuális eszközt, hogy leállítása vagy törlése az ügyfelek és a hosts függnek, hogy az eszköz.

## <a name="deactivate-and-delete-data"></a>Inaktiválja, és az adatok törlése

Ha kíváncsi az eszköz teljesen törlése, és nem szeretné megőrizni az adatokat az eszközre, majd töltse ki az alábbi lépéseket.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Az eszköz inaktiválása és az adatok törlése  

1. Előtt inaktiválása egy eszközt, az összes mennyiségi törölnie kell az eszköz társított tárolók (és a kötet). A kapcsolódó biztonsági másolatok törlése után csak mennyiségi tárolók törölheti.

2. Az eszköz inaktiválta az az alábbi képlettel történik:

    1. StorSimple kezelő szolgáltatás **eszközök** lapon jelölje ki az eszközt, amelyet ki szeretne inaktiválása, és a lap alján kattintson az **Inaktiválás**gombra.

    2. A megerősítést kérő üzenet jelenik meg. Kattintson az **Igen gombra** a folytatáshoz. Az inaktiválás folyamat nyílik meg, és néhány percet igénybe vehet.

3. Az inaktiválás, miután teljesen törölheti az eszközön. Eszköz törlése eltávolítja a szolgáltatás a csatlakoztatott eszközök listája. A szolgáltatás már nem kezelhetők a törölt eszközt. Az eszköz törléséhez kövesse az alábbi lépéseket:

    1. StorSimple kezelő szolgáltatás **eszközök** lapon jelölje ki a törölni kívánt inaktiválva eszközt.

    2. A lap alján kattintson a **Törlés**gombra.

    3. Megerősítését kéri. Kattintson az **Igen gombra** a folytatáshoz.

    Eltarthat néhány percig, amíg törlődik az eszközt.

## <a name="deactivate-and-retain-data"></a>Inaktiválja, majd megőrzi az adatok

Ha érdekli a Törlés az eszközön, de szeretné tartani az adatokat, majd töltse ki az alábbi lépéseket.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Történő inaktiválta egy eszközt, és az adatok 

1. Kapcsolja ki az eszközt. A hangerő-tárolók és az eszköz a pillanatképek marad.

    1. StorSimple kezelő szolgáltatás **eszközök** lapon jelölje ki az eszközt, amelyet ki szeretne inaktiválása, és a lap alján kattintson az **Inaktiválás**gombra.

    2. A megerősítést kérő üzenet jelenik meg. Kattintson az **Igen gombra** a folytatáshoz. Az inaktiválás folyamat nyílik meg, és néhány percet igénybe vehet.

2. A mennyiségi tárolók és a kapcsolódó pillanatképek letöltése sikertelen. Eljárások lépjen a [feladatátvétel és katasztrófa helyreállítási StorSimple mobileszközére](storsimple-device-failover-disaster-recovery.md).

3. Miután az Inaktiválás és feladatátvételi törölheti az eszköz teljesen. Eszköz törlése eltávolítja a szolgáltatás a csatlakoztatott eszközök listája. A szolgáltatás már nem kezelhetők a törölt eszközt. Hajtsa végre az alábbi lépéseket az eszköz törlése:
 
    1. StorSimple kezelő szolgáltatás **eszközök** lapon jelölje ki a törölni kívánt inaktiválva eszközt.

    2. A lap alján kattintson a **Törlés**gombra.

    3. Megerősítését kéri. Kattintson az **Igen gombra** a folytatáshoz.

    Eltarthat néhány percig, amíg törlődik az eszközt.

## <a name="deactivate-and-delete-a-virtual-device"></a>A virtuális eszköz inaktiváljon

Az inaktiválás StorSimple virtuális eszköz felszabadítja a virtuális gépen. A virtuális gép és az erőforrások jön létre, ha azt kiépített ezután törölheti. Miután a virtuális eszköz nyitásához azt nem lehet visszaállítani az eredeti állapotába. 

Az inaktiválás eredménye a következő műveleteket:

- A StorSimple virtuális eszköz el van távolítva.

- A OSDisk és az adatok lemezt a StorSimple virtuális eszköz által létrehozott törlődnek.

- A szolgáltatott szolgáltatás és a kiépítési során készített virtuális hálózati megőrződnek. Ha nem használ a szervezetek, törölni kell őket kézzel.

- A virtuális StorSimple eszköz által létrehozott felhőalapú pillanatképek megőrződnek.

## <a name="next-steps"></a>Következő lépések
- Az inaktív eszköz gyári állítani, nyissa meg [az eszköz gyári alapértelmezett beállításainak visszaállítása](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- A technikai támogatási, [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md).

- A StorSimple kezelő szolgáltatás használatával kapcsolatos további információért keresse fel [a StorSimple Manager szolgáltatással való StorSimple eszközére](storsimple-manager-service-administration.md). 
