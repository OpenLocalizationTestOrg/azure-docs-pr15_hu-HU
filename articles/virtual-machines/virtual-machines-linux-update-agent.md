<properties
    pageTitle="Frissítse az Azure Linux ügynök a GitHub |} Microsoft Azure"
    description="Megtudhatja, hogyan kell a frissítés Azure Linux ügynök a Linux virtuális Github a lateset verziójára Azure-ban"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Az Azure Linux ügynök egy virtuális frissítése a legújabb verzióra való GitHub

Ha frissíteni szeretné az [Azure Linux ügynök](https://github.com/Azure/WALinuxAgent) egy Linux virtuális Azure-ban, akkor kell már van:

1. A futó Linux virtuális Azure-ban.
2. A kapcsolat, hogy Linux virtuális SSH használatával.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Ha Ön fogja kell a feladat végrehajtásához egy Windows rendszerű számítógépen, gitt SSH való Linux számítógépre is használhatja. További információért megtudhatja, [hogy miként jelentkezzen be a virtuális gépen futó Linux rendszerhez](virtual-machines-linux-mac-create-ssh-keys.md).

Azure záradékkal Linux distros az Azure Linux ügynök csomag illesztette azok tárházakban található, így ellenőrizze és telepítse a legújabb, hogy Distro tárházba először Ha lehetséges.  

Ubuntu egyszerűen írja be:

    #sudo apt-get install walinuxagent

És CentOS, írja be:

    #sudo yum install waagent


Az Oracle Linux, győződjön meg arról, hogy a `Addons` tárházba engedélyezve van. Válassza a fájl szerkesztése `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a sor `enabled=0` való `enabled=1` **[ol6_addons]** és **[ol7_addons]** a fájlban.

Az Azure Linux ügynök legújabb verziójának telepítéséhez, írja be:


    #sudo yum install WALinuxAgent

Ha nem találja a bővítmény tárházba egyszerűen felveheti ezek azok a sorok végén található a .repo fájl megfelelően az Oracle Linux megjelenés:

Az Oracle Linux 6 virtuális gépeken futó:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Az Oracle Linux 7 virtuális gépeken futó:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Írja be:

    #sudo yum update WALinuxAgent

Ez általában az összes szükséges, de ha valamilyen oknál fogva kell telepítheti a https://github.com közvetlenül, kövesse az alábbi lépéseket.


## <a name="install-wget"></a>Wget telepítése

Jelentkezzen be a virtuális SSH használatával.

Telepítse a wget (létezik néhány distros, amely nem telepítheti úgy, hogy alapértelmezett például Redhat CentOS és Oracle Linux verziók 6.4 és 6.5) beírásával `#sudo yum install wget` a parancssorban.


## <a name="download-the-latest-version"></a>Töltse le a legújabb verzióra

Nyissa meg a [megjelenési időpontjához Azure Linux ügynök GitHub az](https://github.com/Azure/WALinuxAgent/releases) weblapon, és ismerje meg a legújabb verzió számot. (Beírásával is keresse meg a korábbit `#waagent --version`.)

### <a name="for-version-20x-type"></a>Verziójához 2.0.x programból, írja be:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   A következő sort 2.0.14 verzióját használja, példaként:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Verziójához 2.1.x vagy újabb, írja be:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   A következő sort 2.1.0 verzióját használja, példaként:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Telepítse az Azure Linux ügynök

### <a name="for-version-20x-use"></a>Verziójához 2.0.x programból, használata:

 Waagent végrehajtható folytathat:

    #chmod +x waagent

 Másolja az új a végrehajtható fájl/usr/sbin /.

  A legtöbb Linux használja:

    #sudo cp waagent /usr/sbin

  CoreOS használja:

    #sudo cp waagent /usr/share/oem/bin/

  Futtassa az Azure Linux ügynök új telepítés esetén:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Verziójához 2.1.x, használata:

Előfordulhat, hogy a csomag telepítéséhez `setuptools` először – lásd: [Itt](https://pypi.python.org/pypi/setuptools). Futtassa:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Indítsa újra a waagent szolgáltatást

A legtöbb linux Distros:

    #sudo service waagent restart

Ubuntu használja:

    #sudo service walinuxagent restart

CoreOS használja:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Erősítse meg az Azure Linux ügynök verzió

    #waagent -version

A CoreOS a fenti parancs nem működnek.

Láthatja, hogy az Azure Linux ügynök verzióra frissült-e az új verzió.

Az Azure Linux ügynök kapcsolatos további tudnivalókért lásd: [Azure Linux ügynök – fontos fájl](https://github.com/Azure/WALinuxAgent).
