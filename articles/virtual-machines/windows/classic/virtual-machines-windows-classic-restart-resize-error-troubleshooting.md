<properties
   pageTitle="Virtuális újraindítása és a problémák méret |} Microsoft Azure"
   description="Újraindítása és Azure egy meglévő Windows virtuális gép átméretezés klasszikus telepítési problémák elhárítása"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-windows"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Újraindítása és Azure egy meglévő Windows virtuális gép átméretezés klasszikus telepítési problémák elhárítása

> [AZURE.SELECTOR]
- [Klasszikus](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
- [Erőforrás-kezelő](../../virtual-machines-windows-restart-resize-error-troubleshooting.md)

Amikor megpróbálja elindítani leállítva Azure virtuális gép (virtuális) vagy egy meglévő Azure virtuális átméretezése, a közös tapasztal, a program terhelés hiba történt. Ez a hiba esetén a fürt vagy régiójában nem rendelkezik a rendelkezésre álló erőforrásokat vagy nem támogatja a kért virtuális méret eredményhez.
> [AZURE.IMPORTANT] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../../../resource-manager-deployment-model.md).  Ez a cikk bemutatja, hogy a klasszikus telepítési modellt használja. Azt javasoljuk, hogy a legtöbb új telepítések az erőforrás-kezelő modell használata.

[AZURE.INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Gyűjt naplók

Hibaelhárítás indításához gyűjt a naplókat, a hiba társított a probléma azonosításához.

Az Azure-portálon kattintson a **Tallózás** > **virtuális gépeken futó** > _a Windows virtuális gép_ > **Beállítások** > **naplókat**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probléma: Hiba egy leállítva virtuális indításakor

Indítsa el a leállítva virtuális, de a get-memóriafoglalási hiba próbálja meg.

### <a name="cause"></a>OK

Az eredeti fürt, amelyen a felhőalapú szolgáltatást a kísérli rendelkezik a leállítva virtuális indítása az értekezlet-összehívást. A fürt azonban nem rendelkezik a szabad terület kérés.

### <a name="resolution"></a>Megoldás

* Hozzon létre egy új, felhőalapú szolgáltatásba, és társítása vagy terület vagy egy virtuális régió alapú hálózat, de nem egy affinitás csoport.

* A leállítva virtuális törölhető.

* Az új felhőszolgáltatásában a virtuális ismételt a lemezt használ.

* Indítsa el a újra létre virtuális.

Ha hibaüzenetet, amikor próbál létrehozni egy új, felhőalapú szolgáltatásba, próbálja meg később vagy az a felhő szolgáltatás régió módosítása.

> [AZURE.IMPORTANT] Az új felhőszolgáltatásba lesz egy új nevet, és a virtuális, hogy meg kell, hogy a függőségeket ezeket az információkat a meglévő felhőalapú szolgáltatást használó adatainak módosítása.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probléma: Régióhoz egy meglévő virtuális átméretezése

Méretezze át egy meglévő virtuális, de a get-memóriafoglalási hiba próbálja meg.

### <a name="cause"></a>OK

Az értekezlet-összehívást méretezze át a virtuális fel kell az eredeti fürt, amelyen a felhőalapú szolgáltatást a megpróbálkozik vele a rendszer. A fürt azonban nem támogatja a kért virtuális méretét.

### <a name="resolution"></a>Megoldás

A kért virtuális méretének csökkentése érdekében, és ismételje meg az átméretezés kérelmet.

* Kattintson a **Tallózás gombra az összes** > **virtuális gépeken futó (klasszikus)** > _a virtuális gép_ > **Beállítások** > **méretét**. A lépések részletes leírását [a virtuális gép átméretezés](https://msdn.microsoft.com/library/dn168976.aspx)látható.

Ha még nem lehetséges a virtuális méretének csökkentése érdekében, kövesse az alábbi lépéseket:

  * Hozzon létre egy új felhőszolgáltatásba, nem kapcsolódik egy affinitás csoportot, és nem társított affinitás alkalmazás csatolt virtuális hálózat biztosítása.

  * Hozzon létre egy új, nagyobb méretű virtuális azt.

Az azonos felhőszolgáltatásában összes a VMs is összevonhatja. Ha a meglévő felhőszolgáltatásba társítva virtuális régió-alapú hálózat, az új felhőszolgáltatásba csatlakozhat a meglévő virtuális hálózat.

Ha a meglévő felhőalapú szolgáltatást nem társított virtuális régió-alapú hálózat, majd van a VMs a meglévő felhőszolgáltatásában törlése, és hozza újra létre őket az új felhőszolgáltatásában a lemezről. Azonban fontos, hogy az új felhőszolgáltatásba lesz egy új nevet, és a virtuális, hogy meg kell frissíteni a függőségeket, amely jelenleg a meglévő felhőalapú szolgáltatást használja ezt az információt az Emlékezzen.

## <a name="next-steps"></a>Következő lépések

Ha problémák Azure-ban egy új Windows virtuális létrehozásakor, tanulmányozza a [diagramok létrehozásának egy új Windows virtuális gép Azure-ban fellépő telepítési problémák](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md).
