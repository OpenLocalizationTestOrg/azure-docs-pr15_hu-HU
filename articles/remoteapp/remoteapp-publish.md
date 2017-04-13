<properties
    pageTitle="Az alkalmazás közzététele az Azure RemoteApp |} Microsoft Azure"
    description="További információ az Azure RemoteApp alkalmazások és erőforrások közzététel."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Az alkalmazás közzététele a RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Miután létrehozta a RemoteApp webhelycsoport, közzé kell az alkalmazások vagy információforrások, amelyek a felhasználók számára elérhetővé szeretné. A webhelysablonok képét ellátni, az előfizetés csak néhány alkalmazások alapértelmezett - megosztása más alkalmazások által közzétett, meg kell közzéteheti őket.

> [AZURE.NOTE] Szükség van az alkalmazás frissítésére? Kell [frissíteni a kép](remoteapp-update.md) először.

A **közzétételi** lap a portálon kattintson a **Közzététel**gombra. Alkalmazás hozzáadása a sablon képe **Start** menüből, vagy adja meg az elérési útját, amelyben az alkalmazás telepítve van a sablonként képe. Ha úgy dönt, hogy a **Start** menüből hozzáadása, válassza az alkalmazás közzétenni a listából. Ha úgy dönt, hogy adja meg az App elérési útját, írja be egy nevet az alkalmazás és az elérési út az alkalmazást. Az elérési út – például "rendszermeghajtón" változók használata helyett "c:\".

> [AZURE.NOTE] Az alkalmazás hozzáadása a **Start** menüből szeretné, ha van szükség *, hogy az alkalmazás hozzáadása a * *indítása* * menü a sablon képe.* Egyéb esetben RemoteApp csak fogják látni, mely *akkor* kattintson a **Start** menü, és összezavarodik lesz. 

>Győződjön meg arról, hogy az alkalmazás a **Start** menü, helyezni a %systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs parancsot mappa - **.lnk** - helyi fájl.

> Ha elfelejtette, az alkalmazás hozzáadása a **Start** menü, a sablon létrehozásakor, válassza az elérési út hozzáadása az alkalmazáshoz. (Vagy hozza létre a sablon képe, de, amely igazán további munka.)


 