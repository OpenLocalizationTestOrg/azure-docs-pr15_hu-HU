<properties
    pageTitle="Virtuális gép skála tervezése skála beállítása |} Microsoft Azure"
    description="Tudnivalók a virtuális gép skála beállítja a skála tervezése"
    keywords="Linux virtuális gép, virtuális gép skála állítja be." 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Méretezés beállítja a virtuális skála tervezése

Ez a témakör azt ismerteti, hogy virtuális gép skála eredménye a tervezési szempontjait. Mik azok a virtuális gép skála készletek kapcsolatos információkért olvassa el az [Virtuális gép skála készletek áttekintése](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Tárhely

A skála megadása a VMs az operációs rendszer lemezt tárolására megadása tároló fiókok használja. Azt javasoljuk, hogy a tárterület-fiók vagy annál kisebb 20 VMs arányát. Azt is javasoljuk, hogy, húzza szét az ábécé keresztül az elején karakterét a tárhely fiókneveket. Így segít húzza szét a betöltése, különböző belső rendszerek különböző módon. Például a következő sablonban azt az erőforrás-kezelő sablon függvény uniqueString létrehozásához használt előtag tiltva, amely a tárhely fióknevét vannak $a: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

A "2016-03-30" API-verzió kezdve virtuális skála beállítása lesz az alapértelmezett "overprovisioning" VMs. Overprovisioning be van kapcsolva, a skála megadása a ténylegesen fordulat felfelé VMs több, mint a rendszer megkérdezi, majd törli a felesleges VMs, amelyek a legutóbbi be is. Overprovisioning javítja a kiépítési sikerességéről. Akkor vannak nem számlázható-e fölös VMs, és azok nem számítanak a kvótákat felé.

Miközben overprovisioning javításához kiépítési sikerességéről okozhat a olyan alkalmazás, amely nem kezelésére szolgál ne a jelentett tűnhessenek VMs áttekinthetőbb legyen viselkedését. Kapcsolja ki a overprovisioning, győződjön meg arról, a sablon a következő karakterlánc van: "overprovision": "false értéket". További részletek találhatók [virtuális skála megadása REST API-dokumentáció](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Ha kikapcsolja a overprovisioning, nem vagyok a gépnél kaphat VMs tároló fiók egy nagyobb arányát, de folyamatos feletti 40 nem javasoljuk.


## <a name="limits"></a>Korlátai
Egy egyéni kép (egy Ön által beépített) épül skála megadása összes OS lemez VHD belül egy tárterület-fiókot kell létrehoznia. Emiatt a maximális VMs száma egy egyéni kép épül skála megadása ajánlott 20. Ha kikapcsolja overprovisioning legfeljebb 40 láthatja el.

Platform képként épül skála meg jelenleg korlátozódik 100 VMs (javasolt a méretezés 5 tároló fiókokat).

A további VMs engedélyezni, hogy ezek a korlátok, mint szeretne-e több skála készletek üzembe, [ezzel a sablonnal](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)látható módon.