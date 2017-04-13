<properties
    pageTitle="Előfizetés és a fiókok útmutató |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve előfizetések és Azure fiókok."
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

# <a name="subscription-and-accounts-guidelines"></a>Előfizetés és fiókok útmutató

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál hogyan megközelíthető előfizetést és a fiókok kezelése, mint a környezet ismertetése, és a felhasználó alap megnő.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Előfizetések és -fiókok végrehajtása szabályai

Határozatok:

- Milyen beállítása előfizetések és fiókok történjen az informatikai terhelést vagy infrastruktúra szeretné üzemeltetni van szüksége?
- Hogyan lehet a szervezet igazítása a hierarchia lebontva?

Feladatok:

- A logikai szervezeti hierarchia kezelése a előfizetés szint tetszés megadása
- A logikai hierarchiája megfelelően, adja meg a szükséges fiókok és az egyes előfizetések.
- Előfizetések és az elnevezésük is egységes fiókot létrehozni.


## <a name="subscriptions-and-accounts"></a>Előfizetések és -fiókok

Azure dolgozni, szüksége van egy vagy több Azure előfizetést. Erőforrások, mint virtuális gépeken futó (VMs), vagy e-előfizetések létezik virtuális hálózatok.

- Vállalati felhasználóknak általában egy vállalati regisztrációs, amely a hierarchiában a legfelső erőforrás, és egy vagy több fiókokhoz tartozó rendelkeznek.
- A felhasználók és az Enterprise tagság nélkül ügyfelek a legfelső erőforrás a fiókhoz.
- Az előfizetések fiókokhoz tartozó, és egy fiókot egy vagy több előfizetések is lehet. Azure rekordok számlaadatok előfizetés szintre.

A fiókja vagy előfizetési kapcsolat kétszintű hierarchia határértékén miatt fontos, hogy az elnevezésük is egységes fiókok és előfizetés számlázási igényeihez igazíthatja. Például a globális vállalati Azure használja, ha választanak előfordulhat, hogy van egy fiókom / régió, és van előfizetések kezelése csak a körzet szintet:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Ha például az alábbi szerkezet használhatja:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Ha egy tartomány úgy dönt, hogy a munkatársak adott csoportjához társított egynél több előfizetéssel kell rendelkeznie, az elnevezésük is egységes kell tartalmazniuk, kódolása a felesleges adatok a fiók vagy az előfizetés neve lehetőséget. Ez a szervezet lehetővé teszi, hogy az új szinteket a hierarchia létrehozása során számlázási jelentések számlázási adatok massaging:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

A szervezet ábrája a táblagéptől a következőket:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Elvégezheti a szükséges részletes számlázás letölthető fájl egy adott fiókhoz, illetve az összes fiókhoz keresztül egy vállalati megállapodás.


## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 