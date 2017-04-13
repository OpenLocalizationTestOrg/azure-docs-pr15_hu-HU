<properties
   pageTitle="A CLI segítségével hozzon létre egy Linux virtuális Azure |} Microsoft Azure"
   description="Létrehozhat egy Linux virtuális Azure a CLI használatával."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Létrehozhat egy Linux virtuális Azure a CLI használatával

Ez a cikk bemutatja a telepítéséről gyorsan Linux virtuális gép (virtuális) Azure a használatával a `azure vm quick-create` az Azure parancssori kezelőfelületről parancsot. A `quick-create` parancs üzembe helyezése a virtuális belül egy egyszerű, biztonságos infrastruktúrát, hogy prototípusának használatával, vagy egy fogalmat gyors tesztelése. A cikk van szükség:

- az Azure-fiók (az[első ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)).

- bejelentkezett az [Azure CLI](../xplat-cli-install.md) `azure login`.

- az Azure CLI _kell lennie az_ erőforrás-kezelő Azure mód `azure config mode arm`.

Egy Linux virtuális az [Azure portal](virtual-machines-linux-quick-create-portal.md)segítségével gyorsan is telepítheti.

## <a name="quick-commands"></a>Gyors parancsok

A következő példa bemutatja egy CoreOS virtuális telepíthető, és csatolja a biztonságos rendszerhéj (SSH) használatával (az argumentumokat eltérő lehet a) módját:

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Részletes útmutató

A következő forgatókönyv egy UbuntuLTS virtuális telepített, lépésenkénti, a magyarázatokat milyen módon minden egyes lépés folyamatban van.

## <a name="vm-quick-create-aliases"></a>Virtuális gyors-aliasok létrehozása

Válasszon egy terjesztési gyorsan az Azure CLI aliasokat, a leggyakoribb OS terjesztését megfeleltetve környezetbe. Az alábbi táblázat a aliases (kezdve az Azure CLI verzió 0,10). Az összes telepítések használó `quick-create` VMs félvezető meghajtó (SSD) tároló, amely gyorsabban kiépítési és nagy teljesítményű lemez access készül, az alapértelmezett. (Ezek aliasok jelenítik meg a rendelkezésre álló terjesztését a Azure egy apró része. További képek keresése a Microsoft Azure piactéren található [PowerShell-kép keresése](virtual-machines-linux-cli-ps-findimage.md), [a weben](https://azure.microsoft.com/marketplace/virtual-machines/)vagy [saját egyéni kép feltöltése](virtual-machines-linux-create-upload-generic.md).)

| Alias     | A Publisher | Ajánlat        | RAKTÁRI SZÁM         | Verzió |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2.         | legújabb  |
| CoreOS    | CoreOS    | CoreOS       | Állandó      | legújabb  |
| Debian    | credativ  | Debian       | 8           | legújabb  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | legújabb  |
| RHEL      | Piros kalap    | RHEL         | 7.2.         | legújabb  |
| UbuntuLTS | Kanonikus | Ubuntu kiszolgáló | 14.04.4-LTS | legújabb  |

Az alábbi szakaszok használata a `UbuntuLTS` **ImageURN** beállítással alias (`-Q`) egy Ubuntu 14.04.4 LTS Server telepítése.

Az előző `quick-create` példa csak beállításokkal a `-M` jelző letiltása a SSH jelszavak, így a program kéri az alábbi argumentumokat közben töltse fel a SSH nyilvános kulcs azonosítása:

- (tetszőleges karakterlánc a az első Azure erőforráscsoport általában finom) erőforrás csoportnevet.
- Virtuális neve
- hely (`westus` vagy `westeurope` jó alapértelmezett van)
- Linux (Ha engedélyezni szeretné, hogy mely operációs rendszer kívánt Azure)
- felhasználónév

A következő példa megadja az összes értéket, hogy nincs további Rákérdezés szükség. Mindaddig, amíg van egy `~/.ssh/id_rsa.pub` fájlként ssh-rsa formátum nyilvános kulcs, mint működik:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

A kimenet így néz a következő kimeneti tiltása:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Jelentkezzen be az új virtuális

Jelentkezzen be a virtuális használata a nyilvános IP-cím szerepel az eredményben. A teljes tartománynevét (FQDN), amely szerepel is használhatja:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

A bejelentkezési folyamat hasonlóan kell kinéznie a következő kimeneti tiltása:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Következő lépések

A `azure vm quick-create` telepítendő gyorsan egy virtuális, jelentkezzen be egy bash rendszerhéj és a munka megkezdése módja parancsot. Azonban használatával `vm quick-create` nem ad teljes körű vezérlő sem jelent az, lehetővé teszi azok hozzon létre egy összetettebb környezetben.  Egy, a infrastruktúra testre szabott Linux virtuális üzembe helyezéséhez követheti, ezek a cikkek egyikét:

- [Hozzon létre egy speciális telepítési egy erőforrás-kezelő Azure sablonnal](virtual-machines-linux-cli-deploy-templates.md)
- [Egy Linux virtuális Azure CLI parancsaival közvetlenül a saját egyéni környezet létrehozása](virtual-machines-linux-create-cli-complete.md)
- [Hozzon létre egy SSH védett Linux virtuális Azure sablonok használata](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Is [használja a `docker-machine` különböző paranccsal gyorsan létrehozhat egy Linux virtuális docker fogadó Azure illesztőprogram](virtual-machines-linux-docker-machine.md).
