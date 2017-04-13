<properties
    pageTitle="Az alkalmazás-V-alkalmazások használata a Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogy miként alkalmazás-V-alkalmazások használata az Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Az alkalmazás-V-alkalmazások használata Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A hibrid tartomány illesztés igényel Azure RemoteApp gyűjteményben alkalmazások alkalmazás-V szolgáltatást úgy is használhatja.

Mielőtt hozzákezdene, győződjön meg róla, hogy az alkalmazás-V 5.1 ügyfél telepítse a legújabb frissítésekkel együtt. Szüksége lesz az alkalmazás-V ügyfél tartalmazó [egyéni képe](remoteapp-create-custom-image.md) .  

Sokkal egyszerűbb a meglévő alkalmazás – V infrastruktúra használata Azure RemoteApp. Mivel a hibrid gyűjtemény telepítve van az Azure VNET hozzáféréssel a tartományvezérlőnek be, és a VMs tartományhoz csatlakoztatott, a meglévő alkalmazás – v infrastruktúra és telepítési módszerek az alkalmazás-V gazdaalkalmazás easyily az Azure RemoteApp is élvezheti. Íme néhány dolgot kell szem előtt az alkalmazás-V telepítés verziójától függően:

| Beállítási lehetőségek |                       | Pozitív                                                               | Negatív                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Kézbesítési módszer       | A folyamatos átvitelű (igény szerinti) | Alkalmazás pedig a mindig a legújabb friss                                     | Első alkalommal időtartama                                                                                    |
|                       | Csatlakoztatott               | Ennek leggyorsabb; alkalmazás már szerepel a virtuális a                              | Közelítő - veszi fel képet helyet (127 GB-os korlát)                                                           |
| Alkalmazás helyet tároló  | A megosztott tartalom        | Azure RemoteApp példányának memória alkalmazás fut                         | Az alkalmazás helye memóriát, valamint a folyamatos átvitelű (fájl) kiszolgáló jó kapcsolat eats                      |
|                       | A lemez (gyorsítótárazott)         | A végrehajtás gyors. Az alkalmazás nem függő tartalomforrás elérhetősége | Közelítő - veszi fel képet helyet (127 GB-os korlát)                                                           |
| Célba juttatása             | Felhasználói                  | Teljes önálló alkalmazás – V infrastruktúra igényel                          |                                                                                                       |
|                       | Globális (számítógép)      |  Előre közzététele vagy Megcélozhat használata közzétételi kiszolgáló                         |  Módosítania kell a Azure kép Ha azt szeretné, az alkalmazás (nagyon nagy) frissítéséhez. A kép néhány terület elfoglalja. |

 Létrehozása után a saját kép és a hibrid webhelycsoport közzétenni az alkalmazást, adhatnak a felhasználóknak és fogják túlságosan nagyra értékelni a meglévő alkalmazás – V alkalmazásainak az Azure RemoteApp kézbesítve tetszőleges bármilyen eszközről.
