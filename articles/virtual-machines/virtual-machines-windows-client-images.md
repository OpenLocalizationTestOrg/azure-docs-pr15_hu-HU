<properties
   pageTitle="Windows-ügyfél képek használatáról a fejlesztők/vizsgálatot felhasználási területei |} Microsoft Azure"
   description="Visual Studio előfizetés előnyei használata telepítése a Windows 7/8/10 Azure-ban fejlesztők/vizsgálatot felhasználási területei"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="iainfou"/>

# <a name="using-windows-client-in-azure-for-devtest-scenarios"></a>Windows-ügyfél használata Azure fejlesztők/vizsgálatot felhasználási területei

Használhatja a Windows 7, Windows 8 rendszerben, vagy a Windows 10, a fejlesztők/vizsgálatot felhasználási területei Azure feltéve, hogy megfelelő Visual Studio (korábbi nevén MSDN) szóló előfizetés. Ez a cikk ismerteti az futó Windows ügyfél Azure és Azure gyűjtemény képek használatának követelményei.


## <a name="subscription-eligibility"></a>Előfizetés jogosultság
Aktív Visual Studio előfizetőinek (akik vásárolt egy Visual Studio előfizetői licenccel) használhatja a Windows-ügyfél fejlesztés és tesztelés céljából. Windows-ügyfél saját hardver- és Azure virtuális gépeken futó operációs rendszert futtató Azure előfizetés bármilyen típusú is használható. Windows-ügyfél előfordulhat, hogy nem kell rendszerbe állított használt Azure normál gyártási használatra, vagy azok, akik nem aktív a Visual Studio előfizetők használt.

Kényelme azt bizonyos Windows 10-es képek elérhetővé tett belül [jogosult fejlesztők/próba kínál](#eligible-offers)az Azure gyűjteményből. Visual Studio előfizetők belül ajánlat bármilyen típusú is [megfelelően előkészítése, és hozzon létre](virtual-machines-windows-prepare-for-upload-vhd-image.md) egy 64 bites Windows 7, Windows 8 rendszerben, vagy képe a Windows 10-es és [Azure feltöltése](virtual-machines-windows-upload-image.md). A használata korlátozott fejlesztők/tesztelése aktív Visual Studio előfizetők által marad.


## <a name="eligible-offers"></a>Jogosult ajánlatok
Az alábbi táblázat adatai az ajánlat jogosult telepítése a Windows 10-es keresztül az Azure-gyűjtemény a azonosítókat. A Windows 10-es képek az alábbi ajánlatok csak láthatók. Visual Studio előfizetők számára, akik Windows-ügyfél ajánlat különböző típusú futtatásához szükséges csak a [megfelelő készít](virtual-machines-windows-prepare-for-upload-vhd-image.md) , és hozzon létre egy 64 bites Windows 7, Windows 8 vagy Windows 10-es képpel és [, majd töltse fel a Azure](virtual-machines-windows-upload-image.md).

| Ajánlat neve | Ajánlat száma | Rendelkezésre álló ügyfél képek |
|:-----------|:------------:|:-----------------------:|
| [Kirovó fejlesztők és tesztelése](https://azure.microsoft.com/offers/ms-azr-0023p/)                          | 0023P | Windows 10-ben |
| [Visual Studio vállalati (MPN) előfizetők számára](https://azure.microsoft.com/offers/ms-azr-0029p/)      | 0029P | Windows 10-ben |
| [Visual Studio Professional előfizetők számára](https://azure.microsoft.com/offers/ms-azr-0059p/)          | 0059P | Windows 10-ben |
| [Visual Studio próba Professional előfizetők számára](https://azure.microsoft.com/offers/ms-azr-0060p/)     | 0060P | Windows 10-ben |
| [Visual Studio prémium az MSDN (ellátás)](https://azure.microsoft.com/offers/ms-azr-0061p/)       | 0061P | Windows 10-ben |
| [Visual Studio Enterprise előfizetők számára](https://azure.microsoft.com/offers/ms-azr-0063p/)            | 0063P | Windows 10-ben |
| [Visual Studio Enterprise (BizSpark) előfizetők számára](https://azure.microsoft.com/offers/ms-azr-0064p/) | 0064P | Windows 10-ben |
| [Vállalati fejlesztők és tesztelése](https://azure.microsoft.com/ofers/ms-azr-0148p/)                              | 0148P | Windows 10-ben |


## <a name="check-your-azure-subscription"></a>Jelölje be az Azure előfizetés
Ha nem tudja, hogy az ajánlat azonosító, szerezhet be azt az Azure portálon vagy a fiók portálon keresztül.

A "Előfizetések" lap belül az Azure-portálra a kell jegyezni, az ajánlat előfizetés azonosítója:

![Az Azure portálról ajánlat részletei](./media/virtual-machines-windows-client-images/offer_id_azure_portal.png) 

Az ajánlat azonosító ["Előfizetések" lap](http://account.windowsazure.com/Subscriptions) az Azure-fiókjába portálon is tekintheti meg:

![Az Azure-fiókjába portálról ajánlat részletei](./media/virtual-machines-windows-client-images/offer_id_azure_account_portal.png) 


## <a name="next-steps"></a>Következő lépések
A [PowerShell](virtual-machines-windows-ps-create.md), [erőforrás-kezelő sablonok](virtual-machines-windows-ps-template.md)vagy [Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)VMs most telepítheti.
