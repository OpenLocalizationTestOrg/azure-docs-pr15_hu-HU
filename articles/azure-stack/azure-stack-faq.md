<properties
    pageTitle="Gyakori kérdések az Azure Papírhalom |} Microsoft Azure"
    description="Azure Papírhalom gyakori kérdések."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Gyakori kérdések az Azure Papírhalom

## <a name="deployment"></a>Telepítési

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Szükség van az adatok lemez formázása kezdve, vagy indítsa újra a telepítés előtt?

Lemezen nyers formátumban kell lennie. Ha az operációs rendszer újratelepítését, előfordulhat, jelölje be, ha a régi tárolókészlethez továbbra is megtalálható, és törölje az alábbi lépésekkel:

1. Nyissa meg a Kiszolgálókezelő.
2. Jelölje ki a tárterület-készletek.
3. Lásd: Ha egy tárolókészlethez szerepel.
4. Ha szerepel a listában és engedélyezése az olvasási / írási, kattintson a **tárhely kvótáját** gombbal.
5. Kattintson a jobb gombbal a **virtuális merevlemez** (bal alsó sarokban) és a Törlés elemet.
6. Kattintson a jobb gombbal a **Tárhely kvótáját** , és kattintson a Törlés gombra.
7. Indítsa el újra a Azure Papírhalom parancsfájl, és ellenőrizze, hogy a lemez ellenőrzés továbbítja.

Ha szükséges a következő parancsfájl használható:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Van lehetőség az összes SSD lemez a tárhely kvótáját a ez telepítés?

Ez a beállítás nem támogatott ebben a verzióban.  További tudnivalókért olvassa el az [követelmények útmutató](azure-stack-deploy.md) további információt.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Van lehetőség NVMe adatok lemezt a Microsoft Azure Papírhalom Ez?

Miközben tároló szóközöket közvetlen támogatja a NVMe lemezt, Azure Papírhalom csak támogatja lehetséges meghajtó típusai és lehetséges kombinációja csak egy részhalmazát tároló szóközöket közvetlen. 

### <a name="how-can-i-reinstall-azure-stack"></a>Hogyan tudom újratelepíteni Azure Papírhalom?
A lépésekkel a [újratelepítés útmutató](azure-stack-redeploy.md).  

## <a name="tenant"></a>Bérlői webhelyen

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Telepíthetem-e a saját kép, a bérlői webhelyet?

Igen, hasonlóan az Azure-bérlői webhelyre is képek feltöltése Azure egymást fedő, a szolgáltatás-rendszergazda képeinek mellett. Című témakörben [egy virtuális kép](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Tesztelés

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Egymásba ágyazott virtualizációs segítségével tesztelje a Microsoft Azure Papírhalom Ez?

Beágyazott virtualizációs nem támogatott vagy vizsgálni Azure Papírhalom technikai előzetes verzió 2.

## <a name="virtual-machines"></a>Virtuális gépeken futó

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Azure Papírhalom támogatja virtuális gépeken futó dinamikus lemezen?

Azure Papírhalom nem támogatja a dinamikus lemezen.

## <a name="next-steps"></a>Következő lépések

[Hibaelhárítás](azure-stack-troubleshooting.md)
