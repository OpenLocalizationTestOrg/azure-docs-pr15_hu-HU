<properties
    pageTitle="Felhőalapú nicializálás használatával szabhatja testre a Linux virtuális létrehozása során |} Microsoft Azure"
    description="Felhőalapú nicializálás segítségével testre szabhatja a Linux virtuális létrehozása során."
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
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Testre szabhatja a Linux virtuális létrehozása során a felhő nicializálás használatával

Ebből a cikkből megtudhatja, hogyan készíthet egy felhőalapú nicializálás parancsfájlt, állítsa a hostname (állomásnév), a frissítés telepített csomagok és felhasználói fiókokat.  A felhő nicializálás parancsfájlok nevezzük a virtuális létrehozása az Azure CLI során.  A cikk van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).

- bejelentkezett az [Azure CLI](../xplat-cli-install.md) `azure login`.

- az Azure CLI _kell lennie az_ erőforrás-kezelő Azure mód `azure config mode arm`.

## <a name="quick-commands"></a>Gyors parancsok

Hozzon létre egy felhőalapú-init.txt parancsfájlt, amely állítja be az állomásnév, minden csomagon frissíti vagy Linux sudo felhasználó felvétele.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Hozzon létre egy erőforráscsoport VMs történő elindítására.

```bash
azure group create myResourceGroup westus
```

Hozzon létre egy Linux virtuális rendszerindításkor állította be, a felhőben nicializálás használatával.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Részletes útmutató

### <a name="introduction"></a>– Bevezetés

Egy új Linux virtuális indításakor egyre semmi sem a szabványos Linux virtuális, testre szabott vagy készen áll a igényeinek megfelelően. [Felhőalapú nicializálás](https://cloudinit.readthedocs.org) szabványos módja a parancsfájlt vagy konfigurációs beállítások be, hogy Linux virtuális beillesztendő, ahogy azt tagjának először indítja.

Azure, a háromféleképpen egy másik alakzatot egy Linux virtuális változtatásokat, miközben folyamatban van telepítve, és elindítja.

- Helyezhet el a felhőben nicializálás parancsfájlok.
- Helyezhet el a parancsfájlok az Azure [VMAccess bővítmény](virtual-machines-linux-using-vmaccess-extension.md)használatával.
- Az Azure-sablon használatával felhő nicializálás.
- Az Azure-sablon használatával [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

A beillesztendő parancsfájlok indítási után bármikor:

- Parancsok közvetlenül futtatásához SSH
- Az Azure [VMAccess bővítmény](virtual-machines-linux-using-vmaccess-extension.md)használatával, imperatively vagy az Azure-sablon parancsfájlok behelyezése
- Beállítási felügyeleti eszközök, például Ansible, só, tölthetők le és Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Azure virtuális a felhőben nicializálás elérhetősége gyors létrehozása kép aliasok:

| Alias     | A Publisher | Ajánlat        | RAKTÁRI SZÁM         | Verzió | felhőalapú nicializálás |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2.         | legújabb  | nem         |
| CoreOS    | CoreOS    | CoreOS       | Állandó      | legújabb  | igen        |
| Debian    | credativ  | Debian       | 8           | legújabb  | nem         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | legújabb  | nem         |
| RHEL      | Redhat    | RHEL         | 7.2.         | legújabb  | nem         |
| UbuntuLTS | Kanonikus | UbuntuServer | 14.04.4-LTS | legújabb  | igen        |

A Microsoft cloud nicializálás szerepelnek, és a képeket, hogy biztosítanak-e Azure dolgozik megszerezni a partnerekkel működik.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>A virtuális létrehozása az Azure CLI a felhő nicializálás parancsfájl hozzáadása

Indítsa el a felhőben nicializálás parancsfájl, egy virtuális létrehozása az Azure-ban, adja meg a felhőben nicializálás-fájlt az Azure CLI a `--custom-data` válthat.

Hozzon létre egy erőforráscsoport VMs történő elindítására.

```bash
azure group create myResourceGroup westus
```

Hozzon létre egy Linux virtuális rendszerindításkor állította be, a felhőben nicializálás használatával.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>A felhőalapú nicializálás parancsfájl beállítása egy Linux virtuális gép hostname (állomásnév) létrehozása

A hostname (állomásnév) akkor bármely Linux virtuális a legegyszerűbb és a legfontosabb beállítások közül. Azt egyszerűen beállíthatja a felhőben nicializálás használata a parancsfájl.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Példa felhő nicializálás parancsfájl nevű `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

A virtuális első indításakor a felhőben nicializálás parancsfájl állítja be az állomásnév `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Bejelentkezés, és ellenőrizze a új virtuális gép hostname (állomásnév).

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Linux frissítése a felhőbeli nicializálás parancsfájl létrehozásáról

A biztonság érdekében azt szeretné, a Ubuntu virtuális frissítése a első indításakor.  Felhőalapú init azt teheti a követés forgatókönyvvel attól függően, hogy a Linux eloszlás esetén használja.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Példa felhő nicializálás parancsfájl `cloud_config_apt_upgrade.txt` a Debian család számára

```bash
#cloud-config
apt_upgrade: true
```

Miután Linux indult, a telepített csomagok segítségével frissített `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Bejelentkezés, és ellenőrizze, hogy minden csomagon frissülnek.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Felhasználó felvétele Linux felhő nicializálás parancsfájl létrehozása

Az első tevékenységek minden új Linux virtuális egyik egy felhasználó felvétele a saját maga számára, vagy használatának kerülése `root`. SSH kulcsok a legjobb biztonsági és használhatóság, és hozzáadja őket a `~/.ssh/authorized_keys` fájlt a felhőbe nicializálás forgatókönyvvel.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Példa felhő nicializálás parancsfájl `cloud_config_add_users.txt` Debian család számára

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Miután Linux indult, a felsorolt felhasználók létrehozása és az sudo csoport.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Bejelentkezés, és ellenőrizze az újonnan létrehozott felhasználó.

```bash
ssh myVM
cat /etc/group
```

Kimenet

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Következő lépések

Felhőalapú nicializálás egyre szabványos lehet módosítani a Linux virtuális indításakor. Azure szintén virtuális bővítmények, amelyek lehetővé teszik a LinuxVM indítási vagy futása közben is módosíthatja. Ha például hozzon létre egy új SSH vagy a felhasználó adatai a virtuális futása közben az Azure VMAccessExtension is használhatja. Felhőalapú-jelzők közül lenne szüksége kell indítani a számítógépet a jelszó alaphelyzetbe állítása.

[Tudnivalók a virtuális gép extensions és szolgáltatások](virtual-machines-linux-extensions-features.md)

[Kezelheti a felhasználókat, SSH és jelölőnégyzetet, vagy a Azure Linux VMs VMAccess bővítményében lemez javítása](virtual-machines-linux-using-vmaccess-extension.md)
