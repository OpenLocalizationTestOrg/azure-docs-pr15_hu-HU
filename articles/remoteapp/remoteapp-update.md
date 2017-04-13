<properties
   pageTitle="Az Azure RemoteApp webhelycsoport frissítése |} Microsoft Azure"
   description="Az Azure RemoteApp webhelycsoport frissítése"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Az Azure RemoteApp egy webhelycsoport frissítése

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Egy esetben amennyi Ha módosítania kell a alkalmazások vagy a kép az Azure RemoteApp gyűjteményben fog érkezni. Alkalmazás használatakor az Azure RemoteApp előfizetését, egy felhőalapú vagy a hibrid gyűjteményben részét képező képek közül minden frissítések kezeli Azure RemoteApp magát, így könnyen viszi.

Jó helyen jár Ha egyéni kép (beépített létrehozása vagy módosítása, a képek által létrehozott) használ, ajánljuk, akik a kép és az alkalmazások fenntartása. Ha módosítania kell a kép vagy az alkalmazásokban belül a kijelöléséhez, kell, és hozzon létre egy új, frissített verziójának a képet, majd a gyűjteményben a meglévő kép cseréje a új frissített képre.

Igen hogyan el a webhelycsoport frissítésével kapcsolatban? Érdemes meglehetősen egyszerű:

1. Frissítse a képet, akkor az a webhelycsoport használt. Bármely javítások vagy szükséges frissítéseket alkalmazni, és mentse azt egy új néven.
2. [Fel](remoteapp-uploadimage.md) - vagy [importálása](remoteapp-image-on-azurevm.md) , amely a képre az RemoteApp.
3. Ezután a webhelycsoportok lapon kattintson a **frissítés**gombra.
4. Válassza ki az új kép a **Sablon képe** listából.
4. Az alábbiakban a körlevelekben rész – el kell döntenie a gyűjteményben alkalmazás jelenleg használó felhasználók kezelése. Ön a következő lehetőségek közül:
    - **A frissítés után 60 perc megadása a felhasználóknak**. Amint a frissítés befejeződik, az Azure RemoteApp mentse a munkáját, és jelentkezzen ki, és jelentkezzen be újra az aktív jelzi azokat felhasználói megjelenít egy üzenetet. 60 perc múlva bármely az aktív felhasználók, akik nem kijelentkezett van fog automatikusan kilépteti. Felhasználók azonnal bejelentkezhet vissza.
    - **Azonnal jelentkezzen ki a felhasználókat**. Amint a frissítés befejezése jelentkezzen ki az összes felhasználó automatikusan figyelmeztetés nélkül. Ha ezt a beállítást választja, a felhasználói adatok elveszhetnek. Azonban hogy csatlakozhat az alkalmazás azonnal.

1. Kattintson a frissítés indításához.
