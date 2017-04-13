<properties
   pageTitle="Virtuális újraindítása és a problémák méret |} Microsoft Azure"
   description="Erőforrás-kezelő telepítési problémáinak megoldását újraindítását, vagy egy meglévő Linux virtuális gép Azure-ban átméretezéssel"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Erőforrás-kezelő telepítési problémáinak megoldását újraindítását, és a méret egy meglévő Linux virtuális gép Azure-ban

Amikor megpróbálja elindítani leállítva Azure virtuális gép (virtuális) vagy egy meglévő Azure virtuális átméretezése, a közös tapasztal, a program terhelés hiba történt. Ez a hiba esetén a fürt vagy régiójában nem rendelkezik a rendelkezésre álló erőforrásokat vagy nem támogatja a kért virtuális méret eredményhez.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Gyűjt naplók

Hibaelhárítás indításához gyűjt a naplókat, a hiba társított a probléma azonosításához. Az alábbi hivatkozásokat tartalmaz a folyamat részletes információkat:

[Hibaelhárítási erőforrás csoport telepítések Azure Portal segítségével](../resource-manager-troubleshoot-deployments-portal.md)

[Erőforrás-kezelő műveleteinek naplózása](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probléma: Hiba egy leállítva virtuális indításakor

Indítsa el a leállítva virtuális, de a get-memóriafoglalási hiba próbálja meg.

### <a name="cause"></a>OK

Az eredeti fürt, amelyen a felhőalapú szolgáltatást a kísérli rendelkezik a leállítva virtuális indítása az értekezlet-összehívást. A fürt azonban nem rendelkezik a szabad terület kérés.

### <a name="resolution"></a>Megoldás

*   Elérhetőség megadása minden VMs le, és indítsa újra az egyes virtuális.

  1. Kattintson **az erőforrás csoportok** > _az erőforráscsoport_ > **erőforrások** > _elérhetőségi beállítása_ > **virtuális gépeken futó** > _a virtuális gép_ > **leállítása**.

  2. Miután az összes VMs leállításához jelölje ki a leállítva VMs, és kattintson a Start gombra.

*   Ismételje meg a restart-kérelmet Várjon egy kis időt.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probléma: Régióhoz egy meglévő virtuális átméretezése

Méretezze át egy meglévő virtuális, de a get-memóriafoglalási hiba próbálja meg.

### <a name="cause"></a>OK

Az értekezlet-összehívást méretezze át a virtuális fel kell az eredeti fürt, amelyen a felhőalapú szolgáltatást a megpróbálkozik vele a rendszer. A fürt azonban nem támogatja a kért virtuális méretét.

### <a name="resolution"></a>Megoldás

* Ismételje meg a kérelmet, virtuális kisebb méretű használatával.

* Ha a kért virtuális méretének nem módosíthatók:

  1. Elérhetőség megadása minden VMs leállítása.

    * Kattintson **az erőforrás csoportok** > _az erőforráscsoport_ > **erőforrások** > _elérhetőségi beállítása_ > **virtuális gépeken futó** > _a virtuális gép_ > **leállítása**.

  2. Után minden VMs leállításához méretezze át a kívánt virtuális egy nagyobb értékre.
  3. Jelölje ki a átméretezett virtuális, és kattintson a **Start**gombra, és indítsa el az egyes a leállítva VMs.

## <a name="next-steps"></a>Következő lépések

Ha problémák Azure-ban egy új Linux virtuális létrehozásakor, tanulmányozza a [diagramok létrehozásának egy új Linux virtuális gép Azure-ban fellépő telepítési problémák](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
