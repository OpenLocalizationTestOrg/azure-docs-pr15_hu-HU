<properties
   pageTitle="Windows virtuális telepítési-erőforrás-kezelő elhárítása |} Microsoft Azure"
   description="Erőforrás-kezelő telepítési problémák elhárítása, Azure-ban egy új Windows virtuális gép létrehozásakor"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Erőforrás-kezelő telepítési problémáinak megoldását egy új Windows virtuális gép létrehozása az Azure-ban

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Gyűjt naplók

Hibaelhárítás indításához gyűjt a naplókat, a hiba társított a probléma azonosításához. Az alábbi hivatkozások követése a folyamat részletes információkat tartalmaz.

[Erőforrás csoport telepítések Azure Portal segítségével – problémamegoldás](../resource-manager-troubleshoot-deployments-portal.md)

[Erőforrás-kezelő műveleteinek naplózása](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Ha az operációs rendszer általános Windows, és feltölteni, illetve az általános beállítással rögzített, majd ott nem esetleges hibákat. Hasonlóképpen az operációs rendszer esetén speciális Windows, töltöttem fel, illetve a speciális beállítás rögzített, majd a hibák nem lesz.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha az operációs rendszer általános Windows, és mint feltöltésük speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet, a virtuális problémákat tapasztal a OOBE képernyőn.

**N<sup>2</sup>:** Windows speciális és töltenek fel, mint az általános az operációs rendszer esetén a kiépítési hiba kapja meg a a virtuális problémákat tapasztal a OOBE képernyőn, mert a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával.

**Megoldás**

Ezt a beállítást, amely szerint az operációs rendszer (általános/speciális) az [Add-AzureRmVhd töltse fel az eredeti virtuális](https://msdn.microsoft.com/library/mt603554.aspx), érhető el a helyszíni használata mindkét hibák elhárításához. Töltse fel, mint általános, ne felejtse el először futtassa a sysprep.

**Rögzítés hibák:**

**N<sup>3</sup>:** Ha az operációs rendszer általános Windows, és mint rögzítés speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet mivel az eredeti virtuális nem használható, van megjelölve általános.

**N<sup>4</sup>:** Windows speciális és, általános rögzíthető az operációs rendszer esetén a kiépítési hiba kapja, mert a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával. Emellett az eredeti virtuális függvény nem használható mivel van megjelölve, speciális.

**Megoldás**

Mindkét e hibák megoldásáról, törölje az aktuális kép a portálon, és [vegye fel ismét a vágólapra az aktuális VHD](virtual-machines-windows-vhd-copy.md) ugyanazokat a beállításokat, mint az az operációs rendszer (általános/speciális).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probléma: Egyéni és tár/piactér képe; memóriafoglalási hiba
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
Ha problémák egy leállítva Windows virtuális indításakor vagy egy meglévő Windows Azure-ban virtuális gép átméretezés, tanulmányozza az [erőforrás-kezelő – problémamegoldás telepítési problémáinak újraindítását, és a méret egy meglévő Windows virtuális gép Azure-ban](virtual-machines-windows-restart-resize-error-troubleshooting.md).
