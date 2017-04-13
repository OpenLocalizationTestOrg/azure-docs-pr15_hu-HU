<properties
   pageTitle="Engedélyezés vagy letiltás Azure virtuális figyelése"
   description="Megtudhatja, hogy miként engedélyezheti vagy letilthatja az Azure virtuális figyelése"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Engedélyezheti vagy letilthatja az Azure virtuális figyelése

Ez a szakasz ismerteti, hogyan engedélyezéséhez vagy letiltásához futó Azure virtuális gépeken figyelése. Alapértelmezés szerint figyelése engedélyezve van Azure virtuális gépeken futó Ha telepíti az [Azure-portál](https://portal.azure.com) és felügyeleti diagramokat alapértelmezés 1 perces pontra által nyújtott. Engedélyezheti vagy letilthatja a figyelése a portálra vagy az Azure parancssori felület használata Mac, Linux és Windows (Azure CLI). 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Engedélyezése és letiltása az Azure portálon keresztül figyelése
 
Engedélyezheti a Azure virtuális, amely magában foglalja a példány adatainak 1 perces időszakokban ellenőrzésére. (a tárhely módosítások vonatkoznak). Diagnosztikai részletes adatokat a virtuális a portál grafikonok vagy az API-k majd érhető el. Alapértelmezés szerint Azure portal lehetővé teszi, hogy figyelése, de kikapcsolhatja ezt az alábbiaknak. Engedélyezheti a virtuális fut, vagy az állapot leállt közben figyelése.

- Nyissa meg az Azure **a portál [https://portal.azure.com](https://portal.azure.com)**

- A bal oldali navigációs sávon kattintson a virtuális gépeken futó.

- A lista virtuális gépeken futó válassza ki az egyik állapotát példányát. Virtuális gép blad nyílik meg.

- Kattintson a "Minden beállítások kifejezésre".

- Kattintson a "Diagnosztika".

- Állapot módosítása a be vagy ki állapotba. Is választhat az Ez a lap kívánt engedélyezése a virtuális gép részletek figyelése szintjét.

[Azure.Note] A diagnosztikai a Váltás az alapértelmezett érték, amikor létrehoz egy új virtuális számítógépre

![Engedélyezése és letiltása az Azure portálon keresztül figyelése.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Engedélyezése és letiltása az Azure CLI figyelése
 
Ahhoz, hogy az Azure virtuális figyelése.

- Hozzon létre egy PrivateConfig.json például a következő tartalommal nevű fájlt.
        {"storageAccountName": "a tárhely fiók adatok fogadásához", "storageAccountKey": "a fiók billentyűt"}
- A következő Azure CLI parancsot.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Ha vannak újabb 2.0-s verzióval módosíthatja. 

Beállításával kapcsolatban további részleteket felügyeleti mértékek és a minták, látogasson el a dokumentum - **[Használatával Linux diagnosztikai bővítmény Monitor Linux virtuális teljesítményének és diagnosztikai adatok](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

