<properties
    pageTitle="A Azure Papírhalom Linux vendégek |} Microsoft Azure"
    description="Megtudhatja, hogyan létrehozása Linux-alapú virtuális gépeken futó Azure Papírhalom."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>A Azure Papírhalom Linux virtuális gépeken futó terjesztése

Az Azure Papírhalom Ez a Linux virtuális gépeken futó telepítheti a által hozzáadott Linux-alapú képként a Papírhalom Azure piactérről. Több Linux szállítók módon biztosítva van a képek, manuálisan is hozzáadhatók az Azure Papírhalom ez be, vagy saját maga is készíthet.

## <a name="download-an-image"></a>Képek letöltése

 1. Töltse le és Azure Papírhalom-kompatibilis kép kinyerése az alábbi hivatkozások, vagy saját előkészítése:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 LTS](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 LTS](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Bontsa ki a kép virtuális, ha szükséges, és [adja hozzá a képet a piactér](azure-stack-add-vm-image.md). Győződjön meg arról, hogy a `OSType` paraméter értéke `Linux`.
 
 3. Miután felvette a képet a Piactérhez, létrejön egy piactéren elérhető elemre, és egy Linux virtuális számítógépre telepítheti.
  
## <a name="prepare-your-own-image"></a>A saját kép előkészítése

1. Felkészülés a saját Linux kép közül az alábbi lépéseket:
 - [Felosztás centOS-alapú](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Az Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Piros kalap vállalati Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Töltse le és telepítse az [Azure Linux Agent](https://github.com/Azure/WALinuxAgent/)

    Az Azure Linux ügynök verzió 2.1.3 vagy újabb szükséges a Papírhalom Azure virtuális Linux gép hozhatók létre. A fent felsorolt már terjesztését számos verziójában a agent vagy magasabb csomag tartalmazza a tárházakban található (neve általában `WALinuxAgent` vagy `walinuxagent`). Azonban az Azure agent csomag verziójának esetén kisebb, mint 2.1.3 (azaz 2.0.18 vagy alsóbb szint), akkor telepítenie kell a agent manuálisan. A telepített határozható meg, a csomag nevére vagy futtatásával `/usr/sbin/waagent -version` a virtuális meg.

    Az alábbi utasításokat követve telepítse az Azure Linux ügynök manuálisan-

 - Először töltse le a legújabb Azure Linux agent [Github](https://github.com/Azure/WALinuxAgent/releases), például:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Az Azure ügynök kicsomagolása:

            # tar -vzxf v2.2.0.tar.gz

 - Python-setuptools telepítése

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Telepítse az Azure ügynök:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Rendszerek Python és 2.x és Python telepített 3.x egymás mellé előfordulhat, hogy kell futtassa a következő parancsot:

        # sudo python3 setup.py install --register-service

    További tudnivalókért lásd: az Azure Linux ügynök [– Fontos fájl](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Hozzáadás a képet a piactér](azure-stack-add-vm-image.md). Győződjön meg arról, hogy a `OSType` paraméter értéke `Linux`.

4. Miután felvette a képet a Piactérhez, létrejön egy piactéren elérhető elemre, és egy Linux virtuális számítógépre telepítheti.

## <a name="next-steps"></a>Következő lépések

[Gyakori kérdések az Azure Papírhalom](azure-stack-faq.md)