<properties
   pageTitle="Windows virtuális telepítési klasszikus elhárítása |} Microsoft Azure"
   description="Azure-ban egy új Windows virtuális gép létrehozásakor, a klasszikus telepítési problémák elhárítása"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Az új Windows virtuális gép létrehozása az Azure klasszikus telepítési problémák elhárítása

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Gyűjt naplók

Hibaelhárítás indításához gyűjt a naplókat, a hiba társított a probléma azonosításához.

Az Azure-portálon kattintson a **Tallózás** > **virtuális gépeken futó** > *a Windows virtuális gép* > **Beállítások** > **naplókat**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Ha az operációs rendszer általános Windows, és feltölteni, illetve az általános beállítással rögzített, majd ott nem esetleges hibákat. Hasonlóképpen az operációs rendszer esetén speciális Windows, töltöttem fel, illetve a speciális beállítás rögzített, majd a hibák nem lesz.

**Töltse fel a hibák:**

**N<sup>1</sup>:** Ha az operációs rendszer általános Windows, és mint feltöltésük speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet, a virtuális problémákat tapasztal a OOBE képernyőn.

**N<sup>2</sup>:** Windows speciális és töltenek fel, mint az általános az operációs rendszer esetén a kiépítési hiba kapja meg a a virtuális problémákat tapasztal a OOBE képernyőn, mert a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával.

**Megoldás:**

Mindkét hibák megoldásához töltse fel az eredeti virtuális elérhető helyszíni, ugyanazokat a beállításokat, mint az az operációs rendszer (általános/speciális). Töltse fel, mint általános, ne felejtse el először futtassa a sysprep. [Létrehozás és a feltöltés a Windows Server virtuális az Azure](virtual-machines-windows-classic-createupload-vhd.md) talál további információt.

**Rögzítés hibák:**

**N<sup>3</sup>:** Ha az operációs rendszer általános Windows, és mint rögzítés speciális, fog kapni a kiépítési időtúllépésről szóló hibaüzenetet mivel az eredeti virtuális nem használható, van megjelölve általános.

**N<sup>4</sup>:** Windows speciális és, általános rögzíthető az operációs rendszer esetén a kiépítési hiba kapja, mert a új virtuális fut, az eredeti számítógépnév, tartozó felhasználónevével és jelszavával. Emellett az eredeti virtuális függvény nem használható mivel van megjelölve, speciális.

**Megoldás:**

Mindkét e hibák megoldásáról, törölje az aktuális kép a portálon, és [vegye fel ismét a vágólapra az aktuális VHD](virtual-machines-windows-classic-capture-image.md) ugyanazokat a beállításokat, mint az az operációs rendszer (általános/speciális).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probléma: Egyéni és tár / piactér kép; memóriafoglalási hiba
Amikor az új virtuális kérelem érkezik, amely nincs a kérelem beleférjenek szabad területet, vagy nem támogatja a kért virtuális mérete fürtre esetek, amikor ezt a hibát merül fel. Még nem lehet az azonos felhőszolgáltatásában VMs különböző sorozata. Úgy szeretne hozzon létre egy új virtuális, mint a felhőalapú szolgáltatás alkalmas különböző méretű, ha a számítási kérelem sikertelen lesz.

Attól függően, hogy a felhőalapú szolgáltatás hozhat létre az új virtuális kényszerek előforduló két helyzetben okozta hibát.

**Okozó 1:** A felhőbeli szolgáltatástól kitűzi adott fürt, vagy azt egy affinitás csoporthoz csatolva, és így rögzített adott fürt kialakításával. Így új számítási erőforrás kéri, hogy affinitás csoport vannak próbálkozott a hol vannak tárolva a meglévő források azonos fürt. Azonban a azonos fürt előfordulhat, hogy nem támogatja a kért virtuális méretének vagy terhelés hibát eredményez elég szabad hely van. Az IGAZ, hogy az új erőforrások jönnek létre egy új, felhőalapú szolgáltatásba vagy egy meglévő felhőalapú szolgáltatást keresztül.

**Megoldás: 1:**

- Hozzon létre egy új, felhőalapú szolgáltatásba, és a társítani terület vagy a régió-alapú virtuális hálózati.
- Az új felhőalapú szolgáltatás hozzon létre egy új virtuális.
  Ha hibaüzenetet, amikor próbál létrehozni egy új, felhőalapú szolgáltatásba, próbálja meg később vagy az a felhő szolgáltatás régió módosítása.

> [AZURE.IMPORTANT] Ha egy meglévő felhőalapú szolgáltatás hozzon létre egy új virtuális próbált, de nem sikerült, és hozzon létre egy új felhőalapú szolgáltatásba, az új virtuális kellett, megadhatja a ugyanazt a felhőalapú szolgáltatást az összes VMs összesítése. Ehhez a VMs a meglévő felhőszolgáltatásában törlése, és vegye fel őket az új felhőszolgáltatásában a lemezről újra. Azonban fontos, hogy az új felhőszolgáltatásba lesz egy új nevet, és a virtuális, hogy meg kell frissíteni a függőségeket, amely jelenleg a meglévő felhőalapú szolgáltatást használja ezt az információt az Emlékezzen.

**2 okozó:** A felhőbeli szolgáltatástól rendelve, amely egy affinitás csoportjában van csatolva, hogy egy adott fürthöz tervező által rögzített virtuális hálózat. Az összes új számítási erőforrás kéri, hogy affinitás csoport a rendszer ezért vizsgálja meg a hol vannak tárolva a a meglévő erőforrások azonos fürt. Azonban a azonos fürt előfordulhat, hogy nem támogatja a kért virtuális méretének vagy terhelés hibát eredményez elég szabad hely van. Az IGAZ, hogy az új erőforrások jönnek létre egy új, felhőalapú szolgáltatásba vagy egy meglévő felhőalapú szolgáltatást keresztül.

**Megoldás: 2:**

- Hozzon létre egy új területi virtuális hálózathoz.
- Hozzon létre az új virtuális az új virtuális hálózat.
- [Csatlakozás a meglévő virtuális hálózat](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) , az új virtuális hálózathoz. Lásd: További tudnivalók a [területi virtuális hálózatok](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Azt is megteheti, akkor [a affinitás csoport alapú virtuális-hálózat áttelepítésének területi virtuális hálózathoz](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), majd hozza létre az új virtuális.

## <a name="next-steps"></a>Következő lépések
Ha problémák a Windows leállítva virtuális indításakor vagy egy meglévő Windows Azure-ban virtuális gép átméretezés, tanulmányozza a [fellépő klasszikus telepítési problémák újraindítása és Azure egy meglévő Windows virtuális gép átméretezés](windows/classic/virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).
