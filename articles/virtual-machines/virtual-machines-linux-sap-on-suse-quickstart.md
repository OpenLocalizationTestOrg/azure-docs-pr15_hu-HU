<properties
   pageTitle="A Microsoft Azure SUSE Linux VMs SAP NetWeaver tesztelése |} Microsoft Azure"
   description="A Microsoft Azure SUSE Linux VMs SAP NetWeaver tesztelése"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>A Microsoft Azure SUSE Linux VMs futó SAP NetWeaver


Ez a cikk ismerteti a különböző szempontok amikor SAP NetWeaver futtatja a Microsoft Azure SUSE Linux virtuális gépeken futó (VMs). Május 19 2016 kezdve a SAP NetWeaver hivatalos SUSE Linux VMs Azure a támogatott. SAP Megjegyzés 1928533 megtalálható minden részlet vonatkozó Linux verziói, SAP kernelverziókra és így tovább "SAP-alkalmazások a Azure: termékek támogatott és Azure virtuális típusok".
További dokumentációjában az SAP Linux VMs megtalálható itt: [Segítségével SAP Linux (VMs) virtuális gépeken](virtual-machines-linux-sap-get-started.md).


Érdemes elolvasnia az alábbi információk bizonyos esetleges csapdák elkerülése.

## <a name="suse-images-on-azure-for-running-sap"></a>A futó SAP SUSE Azure képek

SAP – NetWeaver forgalmi Azure, használata csak SUSE Linux vállalati kiszolgáló SLES 12 (SPx) – Lásd még: SAP Megjegyzés 1928533. A speciális SUSE képe az Azure piactéren elérhető ("SLES 11 SP3 SAP CAL"), de ez nem készült általános használatát. Ez a kép ne használjon, mert az [SAP felhő készülék tár](https://cal.sap.com/) megoldás van fenntartva.  

Erőforrás-kezelő Azure az összes új vizsgálat és Azure telepítést kell használni. Keres SUSE SLES képek és a verziókkal az Azure PowerShell vagy az Azure parancssori kezelőfelületről, használja az alábbi parancsokat. A kimenet, például, majd használja a OS kép határozhat meg egy új SUSE Linux virtuális találnak JSON sablon.
A PowerShell-parancsokkal, hogy érvényes 1.0.1-es verziójú Azure PowerShell-verzióhoz és újabb.

* Keresse meg a meglévő közzétevők, beleértve a SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Keresse meg a SUSE meglévő ajánlataiban:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Keresse meg a SUSE SLES ajánlataiban:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Keresse meg a SLES Termékváltozatot egy adott verziójához:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Egy SUSE virtuális WALinuxAgent telepítése

A WALinuxAgent nevű agent az Azure piactéren elérhető SLES képek része. Kézzel (például a helyszíni SLES OS virtuális merevlemez (virtuális) feltöltésekor) telepítéséről további tudnivalókért lásd:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (ezek olyan virtuális-gépek-linux-záradékkal-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP – "enhanced ellenőrzés"

SAP – "fokozott figyelése" rendszer kötelező Azure SAP futtatásához. Ellenőrizze a részleteket az SAP Megjegyzés 2191498 "SAP a Linux az Azure: fokozott figyelése".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure adatok lemezt csatol egy Linux Azure virtuális

Tegye a következőt: Soha ne egy Linux Azure virtuális csatlakoztatási Azure adatok lemezt használatával a eszközazonosítót. Használja a univerzális egyedi azonosító (UUID). Ügyeljen arra, hogy grafikus eszközök csatlakoztatási Azure adatok lemezre, például használatakor. Győződjön meg róla /etc/fstab elemeit.

Az Eszközazonosítót problémája, akkor lehet, hogy módosítani, és ezután lehet kifüggeszteni a Azure virtuális betöltési folyamat. A probléma csökkentésében, hozzáadhatja a nofail paraméter /etc/fstab a. De ügyeljen arra, hogy nofail, mert alkalmazások használhat a csatlakozási pont, mielőtt, és előfordulhat, hogy a legfelső szintű fájl rendszerbe írni, abban az esetben, ha egy külső Azure adatok lemezt a betöltés során nem lett csatlakoztatott.

Via UUID csatlakoztatása alól kivételt van csatolása-OS lemez hibaelhárítási célból, a következő szakaszban leírt módon.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>Egy SUSE virtuális, amely nem érhető el többé – hibaelhárítás

Előfordulhat, hogy azokról a helyzetekről, amikor egy Azure virtuális SUSE gép lefagy a indítási folyamat (például a lemez csatlakoztatása kapcsolatos hiba). Ez a probléma ellenőrizheti funkcióval indítási diagnosztika Azure virtuális gépeken futó v2 az Azure-portálon. További tudnivalókért lásd: [indítási diagnosztika] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

A probléma megoldására egy úgy, hogy a sérült virtuális az operációs rendszer lemez csatolása Azure virtuális egy másik SUSE gép. Végezze el a szükséges módosításokat, például /etc/fstab szerkesztése vagy eltávolítása a hálózati udev szabályok csoportban, a következő szakaszban leírt módon.

Van egy fontos tudnivaló a is célszerű figyelembe venni. Az azonos Azure piactéren elérhető kép (például SLES 11 SP4) több SUSE VMs üzembe helyezése hatására az operációs rendszer lemezre mindig ugyanazt a UUID csatlakoztatni. Emiatt a UUID használatával-OS lemez csatolni egy másik virtuális ugyanazt a Microsoft Azure piactéren képet használatával telepített elveszik két azonos begyjegyzésbe. Ez problémákat okoz és jelentheti, hogy a virtuális hibaelhárítási szólnak valójában fog elindulni a lemezről csatolt és sérült OS az eredetihez helyett.

Ennek elkerülése két módja van:

* Microsoft Azure piactéren másik képre használata a hibaelhárítási virtuális (például SLES 11 SPx helyett SLES 12).
* Nem csatolni a sérült OS lemez egy másik virtuális UUID – használata használatával valami mást.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>Az Azure a helyszíni SUSE virtuális feltöltése

A lépéseket követve töltse fel az Azure a helyszíni SUSE virtuális leírását témakörben [Azure SLES vagy openSUSE virtuális gép] (virtual-machines-linux-suse-create-upload-vhd.md).

Ha szeretne egy virtuális nélkül a deprovision lépés végén feltöltése (például, hogy egy meglévő SAP-telepítés megőrzése, valamint az állomásnév), ellenőrizze az alábbiakat:

* Győződjön meg arról, hogy a rendszer lemez csatlakoztatva van-e UUID és nem a eszközazonosítót. Csak a /etc/fstab UUID módosítása nem elég a OS lemez. Is ne felejtse el alkalmazkodás az indítási betöltő keresztül YaST vagy /boot/grub/menu.lst szerkesztésével.
* Ha a VHDX formátum használata a SUSE-OS lemez és átalakíthatja virtuális Azure feltölthető, igen valószínű, hogy a hálózati eszköz változik eth0 eth1. Problémák elkerülése indításakor, hogy a Azure később állíthatja vissza eth0 [klónozott SLES 11 VMware a problémák orvoslására: eth0] leírtak szerint (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Nemcsak a Mi a cikkben ismertetett azt javasoljuk, hogy távolítsa el a:

   /lib/udev/rules.d/75-persistent-NET-Generator.Rules

Az Azure Linux ügynök (waagent) segítségével elkerülheti a lehetséges problémák, mindaddig, amíg nem áll több is telepítheti.

## <a name="deploying-a-suse-vm-on-azure"></a>Egy Azure virtuális SUSE gép üzembe helyezése

Az új Azure erőforrás-kezelő modell JSON sablonfájlok használatával új SUSE VMs kell létrehoznia. A JSON sablonfájl létrehozását követően a virtuális telepítheti a következő CLI parancs helyett a PowerShell használatával:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
JSON sablonfájlok kapcsolatos részletekért lásd: az [szerzői Azure erőforrás-kezelő sablonok] (erőforrás-csoport-szerzői-templates.md) és [Azure quickstart útmutató sablonok] (https://azure.microsoft.com/documentation/templates/).

Ha többet szeretne tudni a CLI és Azure erőforrás-kezelő, lásd: [használata az Azure CLI for Mac, a Linux és a Windows Azure erőforrás-kezelővel] (xplat-cli-azure-erőforrás-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-licenc- és billentyűt

A hivatalos SAP-Azure minősítéssel egy új mechanizmusa számítja ki az SAP hardver billentyűt, amellyel az SAP-licenc jelent meg. Az SAP kernel volna, hogy módosítani kell az adott használja. Linux volt SAP kernelverziókra nem vette fel a kód módosítása. Bizonyos esetekben ezért (például az Azure virtuális átméretezése), az SAP hardver kulcs megváltozott, és az SAP-licenc volt már nem érvényes. Ez a legújabb SAP Linux mag a megoldása. További információ ellenőrizze SAP Megjegyzés 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf csomagot / beállítva adm

SUSE "sapconf" SAP-beállítások csoportja kezelő nevű csomagot tartalmaz. Mit jelent az ebben a csomagban, és telepíteni és használni, hogyan olvashat bővebben lásd a [sapconf használatával az SAP rendszereihez futtatásához SUSE Linux vállalati kiszolgáló előkészítése] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) és [mi, sapconf vagy SUSE Linux vállalati kiszolgáló előkészítése az SAP rendszert futtató?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Új eszköz, amely felváltotta sapconf - beállítva adm. nincs időközben Egy található ez az eszköz az alábbi hivatkozásokat követve olvashat bővebben.

SLES dokumentációjában az sap-hana adm beállítva profil megtalálható [Itt](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

SAP-Munkaterhelésekből beállítva adm - az eszközbeállítási rendszerek megtalálható [Itt](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) fejezet 6.2.


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS elosztott SAP telepített megosztása

Az elosztott telepítés – például ha, amelyre telepíteni szeretné az adatbázist, és az SAP alkalmazáskiszolgálók külön VMs – a megoszthatja a /sapmnt könyvtár keresztül hálózati fájl (NFS). Ha problémát tapasztal a telepítési útmutatást NFS megosztása /sapmnt a létrehozása után, ellenőrizze, hogy ha a "no_root_squash" a megosztás van-e beállítva.

## <a name="logical-volumes"></a>Logikai kötet

Múltbeli nyelvazonosítót egy logikai nagy (például az SAP-adatbázis), több Azure adatok lemezre lett ajánlott lvm nem lett teljes mértékben érvényesített még a Azure mdadm használni kívánt. Megtudhatja, hogy miként állíthatja be a Azure Linux RAID, mdadm használatával, olvassa el a [konfigurálása szoftveres RAID Linux](virtual-machines-linux-configure-raid.md)című témakört. Május 2016 elejére kezdve időközben is lvm Azure teljesen támogatott és mdadm alternatívájaként használható. Azure lvm kapcsolatos további információkat lásd: [LVM konfigurálása a egy Linux virtuális Azure-ban](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE tárházba

A szokásos Azure SUSE tárházba esetén az Access-szel problémát egy egyszerű paranccsal hozzon létre egy új. Ez akkor fordulhat elő, ha létrehozása a személyes OS képe Azure területen, és ezután másolja a vágólapra a kép, amelyre a magánjellegű OS kép alapuló új VMs terjesztése más területére. Csak a következő parancsot a virtuális belül:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome asztali

Ha azt szeretné, telepítheti az Gnome asztali belül egy egyetlen virtuális – például egy SAP grafikus teljes körű SAP bemutató rendszert böngészőt, és az SAP management console – segítségével a tipp telepítse az Azure SLES képek:

   A SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   A SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>SAP-támogatás az Oracle Linux a felhőben

A támogatás korlátozás Linux Oracle virtualizált környezetben van. Bár ez nem az Azure-specifikus témakör, fontos megértéséhez. SAP – nem támogatja az Oracle SUSE vagy egy nyilvános felhőben, például Azure piros kalap. Ez a témakör megvitatása, közvetlenül kapcsolatba lépni Oracle.
