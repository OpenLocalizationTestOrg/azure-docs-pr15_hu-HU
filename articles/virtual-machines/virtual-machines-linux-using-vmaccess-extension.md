<properties
    pageTitle="Azure Linux VMs VMAccess bővítményében elérésének visszaállítása |} Microsoft Azure"
    description="Azure Linux VMs VMAccess bővítményében elérésének visszaállítása."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Kezelheti a felhasználókat, SSH és jelölőnégyzetet, vagy a Azure Linux VMs VMAccess bővítményében lemez javítása

Ez a cikk bemutatja, hogyan ellenőrizze vagy javítsa ki a lemezen, alaphelyzetbe felhasználói hozzáférés, felhasználói fiókokat vagy Linux SSHD konfigurációjának visszaállítása az Azure VMAcesss bővítmény használatával. A cikk van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).

- bejelentkezett az [Azure CLI](../xplat-cli-install.md) `azure login`.

- az Azure CLI _kell lennie az_ erőforrás-kezelő Azure mód `azure config mode arm`.

## <a name="quick-commands"></a>Gyors parancsok

Kétféleképpen a Linux VMs VMAccess használatáról:

- Az Azure CLI és a szükséges paramétereket használ.
- Nyers VMAccess dolgozza fel, és ezután működésbe lépnek JSON-fájlok használatával.

A gyors parancs szakasz fogjuk használni az Azure CLI `azure vm reset-access` módot. Az alábbi példák parancs cserélje ki az értékeket tartalmazó saját környezetből származó értékekkel, "példa".

## <a name="create-a-resource-group-and-linux-vm"></a>Erőforráscsoport és Linux virtuális létrehozása

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Hozzon létre egy Debian virtuális

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Legfelső szintű jelszó alaphelyzetbe állítása

A legfelső szintű jelszó visszaállítása:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH kulcs visszaállítása

A felhasználó nem legfelső szintű SSH kulcsa visszaállítása:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Felhasználó létrehozása

Felhasználó létrehozása:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>A felhasználó eltávolítása

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>SSHD alaphelyzetbe állítása

A SSHD konfiguráció visszaállítása:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Részletes útmutató

### <a name="vmaccess-defined"></a>Definiált VMAccess:

A lemez a Linux virtuális megjelenítő hibákat. A Linux virtuális valami legfelső szintű jelszavának alaphelyzetbe állítása vagy a SSH titkos kulcs véletlenül törölt. Hogy történt, a adatközponthoz napokban vissza, ha meg kellene meghajtó van, és nyissa meg a kiszolgáló konzol biztosító a KVM. Tekintsen úgy az Azure VMAccess bővítmény, mint az adott KVM kapcsoló, amely lehetővé teszi, hogy az access visszaállítása Linux vagy lemez szintű karbantartásához konzol érhető el.

A részletes ismertetését megtalálja fogjuk VMAccess, amely használja nyers JSON-fájlok a hosszú képernyőn.  Ezek a fájlok VMAccess JSON is Azure sablonok hívható meg.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Ellenőrizze vagy javítsa ki a lemezt a Linux virtuális VMAccess használatával

VMAccess használatával végezheti el a fsck futtassa a Linux virtuális a lemezen.  Megteheti azt is, a lemez ellenőrzése és a lemez javítás egy VMAccess használatával.

Jelölje be, és javítsa a lemez használhatja a VMAccess parancsfájl:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Felhasználói hozzáférés visszaállítása Linux VMAccess használatával

Ha elvesztette a hozzáférést az Elhelyezés a Linux virtuális a, a legfelső szintű jelszó alaphelyzetbe állítása VMAccess parancsfájl elindíthatja.

A legfelső szintű jelszót használja a VMAccess parancsfájl:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

A VMAccess parancsfájl használatával a felhasználó nem legfelső szintű SSH kulcsa alaphelyzetbe állításához:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Felhasználói fiókokat Linux VMAccess használatával

VMAccess egy Python parancsfájlt, amely a Linux virtuális a felhasználók kezelése: bejelentkezés és sudo vagy a legfelső szintű fiók használata nélkül is használható.

A VMAccess parancsfájl használatával felhasználó létrehozása:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Felhasználó törlése a VMAccess parancsfájl használata:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>A SSHD konfiguráció visszaállítása VMAccess használatával

Ha végezze el a módosításokat a Linux VMs SSHD beállításait, és zárja be a SSH-kapcsolatot, a módosítások ellenőrzéséhez előtt, akkor előfordulhat, hogy kell akadályozni SSH'ing újra.  A SSHD konfiguráció visszaállítása a helyes konfiguráció való nélkül SSH fölé naplózott VMAccess használható.

Hozzon létre egy új a SSHD konfiguráció használata a VMAccess parancsfájl:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

VMAccess parancsfájlt végrehajtása:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Következő lépések

Linux frissítése Azure VMAccess bővítmények használata egy módszert a futó Linux virtuális a változtatásokat.  Eszközök, például a felhőben nicializálás és Azure sablonok használatával módosíthatja a Linux virtuális indításakor.

[Tudnivalók a virtuális gép extensions és szolgáltatások](virtual-machines-linux-extensions-features.md)

[Erőforrás-kezelő Azure sablonok Linux virtuális kiterjesztésű létrehozása](virtual-machines-linux-extensions-authoring-templates.md)

[Testre szabhatja a Linux virtuális létrehozása során a felhő nicializálás használatával](virtual-machines-linux-using-cloud-init.md)
