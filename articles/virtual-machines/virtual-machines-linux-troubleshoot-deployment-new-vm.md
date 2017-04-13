<properties
   pageTitle="Telepítési-erőforrás-kezelő Linux virtuális elhárítása |} Microsoft Azure"
   description="Erőforrás-kezelő telepítési problémák elhárítása, Azure-ban egy új Linux virtuális gép létrehozásakor"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Erőforrás-kezelő telepítési problémáinak megoldását egy új Linux virtuális gép létrehozása az Azure-ban

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Gyűjt naplók

Hibaelhárítás indításához gyűjt a naplókat, a hiba társított a probléma azonosításához. Az alábbi hivatkozások követése a folyamat részletes információkat tartalmaz.

[Erőforrás csoport telepítések Azure Portal segítségével – problémamegoldás](../resource-manager-troubleshoot-deployments-portal.md)

[Erőforrás-kezelő műveleteinek naplózása](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Ha az operációs rendszer általános Linux, és feltölteni, illetve az általános beállítással rögzített, majd ott nem esetleges hibákat. Hasonlóképpen az operációs rendszer esetén Linux speciális, feltölteni, illetve a speciális beállítás rögzített, majd a hibák nem lesz.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha az operációs rendszer általános Linux, és mint feltöltésük speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet, mert a virtuális marad, a kiépítési szakaszban.

**N<sup>2</sup>:** Esetén az operációs rendszer Linux speciális és töltenek fel, mint az általános, kapja a kiépítési hiba, mert a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával.

**Megoldás:**

Mindkét hibák megoldásához töltse fel az eredeti virtuális elérhető helyszíni, ugyanazokat a beállításokat, mint az az operációs rendszer (általános/speciális). Tölthet fel, mint az általános, ne feledje, hogy futtatni – első deprovision.

**Rögzítés hibák:**

**N<sup>3</sup>:** Ha az operációs rendszer általános Linux, és mint rögzítés speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet mivel az eredeti virtuális nem használható, van megjelölve általános.

**N<sup>4</sup>:** Speciális Linux és, általános rögzíthető az operációs rendszer esetén a kiépítési hiba kapja, mivel a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával. Emellett az eredeti virtuális függvény nem használható mivel van megjelölve, speciális.

**Megoldás:**

Mindkét e hibák megoldásáról, törölje az aktuális kép a portálon, és [vegye fel ismét a vágólapra az aktuális VHD](virtual-machines-linux-capture-image.md) ugyanazokat a beállításokat, mint az az operációs rendszer (általános/speciális).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probléma: Egyéni és tár / piactér kép; memóriafoglalási hiba
Ez a hiba helyzetekben merül fel, amikor az új virtuális kérelem rögzítve van, amely nem támogatja a kért virtuális méretét, vagy nem tartalmaz a kérelem beleférjenek szabad területet fürtre.

**Okozó 1:** A fürt nem támogatja a kért virtuális méretét.

**Megoldás: 1:**

- Ismételje meg a kérelmet, virtuális kisebb méretű használatával.
- Ha a kért virtuális méretének nem módosíthatók:
  - Elérhetőség megadása minden VMs leállítása.
  Kattintson **az erőforrás csoportok** > *az erőforráscsoport* > **erőforrások** > *elérhetőségi beállítása* > **virtuális gépeken futó** > *a virtuális gép* > **leállítása**.
  - Miután az összes VMs leállításához létrehozása a új virtuális a kívánt méretet.
  - Első lépésként indítása az új virtuális, majd jelölje ki a leállítva VMs, és kattintson a **Start**gombra.

**2 okozó:** A fürt nincsenek ingyenes források.

**Megoldás: 2:**

- Ismételje meg a kérelmet, várjon egy kis időt.
- Ha az új virtuális lehet egy másik elérhetősége része
  - Hozzon létre egy új virtuális egy másik elérhetőségét (a régióban azonos) beállítása.
  - Adja hozzá az új virtuális virtuális hálózatához.

## <a name="next-steps"></a>Következő lépések
Ha problémák egy leállítva Linux virtuális indításakor és méretezze át egy meglévő Linux virtuális Azure-ban, tanulmányozza az [erőforrás-kezelő – problémamegoldás telepítési problémáinak újraindítását vagy egy meglévő Azure-ban Linux virtuális gép átméretezés](virtual-machines-linux-restart-resize-error-troubleshooting.md).
