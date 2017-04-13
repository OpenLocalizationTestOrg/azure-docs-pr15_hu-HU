 <properties
   pageTitle="Azure és Linux |} Microsoft Azure"
   description="Azure számítja ki, a tárhely és a hálózat-szolgáltatások Linux virtuális gépeken futó ismerteti."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure és Linux

Microsoft Azure gyűjteménye növekvő integrált nyilvános szolgáltatások analytics, virtuális gépeken futó, adatbázisok mobil hálózatot, tárterület, beleértve a felhő és webes – a megoldások szolgáltatója ideális.  Microsoft Azure, amely lehetővé teszi, hogy csak akkor használjon kifizetéséhez méretezhető számítógépes platformot biztosít, ha azt szeretné – anélkül, hogy fektetni helyszíni hardver.  Azure készen áll, amikor Ön, ha át kívánja méretezni, a megoldások és ki bármilyen méretarányra van szüksége az ügyfelet az igényeinek.

Ha ismeri a Amazon a különböző szolgáltatásai AWS, ellenőrizheti az Azure viewben AWS [definition hozzárendelést dokumentumot](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Területek
Microsoft Azure erőforrások vannak a világ több földrajzi régiók elosztva.  Egy "tartomány" egyetlen földrajzi területen több adatközpontokban jelöli.  Január 1, 2016, kezdve ez tartalmazza: 8-Amerikában, 2 Európában 6 hüvelyk Ázsia Csendes-óceáni, kontinens Kínában 2 és 3 indiai.  Ha azt szeretné, hogy a teljes listáját a rekordok, amelyekben Azure terület, azt karbantartása: a meglévő és újonnan listáját bejelentve a régiók.

- [Azure területek](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Elérhetőség
Ahhoz, hogy a üzembe ahhoz, hogy a 99.95 virtuális szolgáltatási szint szerződés kell üzembe két vagy több VMs fut, a rendszer az elérhetőség terhelést beállított. Ezzel biztosíthatja, a VMs vannak több hibafa tartományok a saját adatközpontokban elosztva, valamint az alakzatot a hosts különböző karbantartási a Windows rendszerbe. A teljes [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) egészének az Azure garantált elérhető ismerteti.

## <a name="azure-virtual-machines--instances"></a>Azure virtuális gépeken futó és a példányok
Microsoft Azure támogatja a számos népszerű Linux terjesztését és a partnerek számos által fenntartott futtatása.  Felosztás például piros kalap vállalati, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD és a Microsoft Azure piactéren található további találja. Azt a különböző Linux Közösségek még több változatok hozzáadása az [Azure záradékkal Linux Distros](virtual-machines-linux-endorsed-distros.md) listához aktívan dolgozhat.

Ha a használni kívánt Linux distro megválasztott nem jelenleg található a gyűjteményben, "áttelepítheti a saját Linux" virtuális [létrehozásával](virtual-machines-linux-create-upload-generic.md)és feltöltése egy Linux virtuális Azure-ban.

Azure virtuális gépeken futó lehetővé teszi a számítások megoldások Agilis módon széles köre üzembe. Gyakorlatilag bármilyen terhelést, és szinte bármilyen operációs rendszeren – a Windows Linux, bármely nyelven telepítheti vagy egyéni hozott létre a csoportlistában a partnereknek a növekvő listáját. Még nem látható mi keres?  Ne aggódjon, – a helyszíni is áttelepítheti a saját kép.

## <a name="vm-sizes"></a>Virtuális méretek
Azure-ban egy virtuális telepítésekor fogja válassza ki a virtuális belül a sorozat, amely a terhelést a megfelelő méretű közül. A méret is érinti a virtuális gép feldolgozás power, memóriahasználat és tárolását kapacitása. A virtuális fut, és a hozzárendelt erőforrások használata más idő mennyiségét alapján vannak számlát kapni. Teljes listáját a [virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

Íme néhány alapvető útmutató hivatkozás kiválasztása a virtuális méretet a sorozat (A, D, DS, G és GS).

* A-sorozat VMs az értéket, az egyszerűsített munkaterhelésének és a fejlesztők/Tesztelési forgatókönyvek kezdő VMs árú. Ezeket széles körben elérhető összes régiókban és is csatlakozás és virtuális gépeken futó elérhető szabványos források.
* A-sorozat méretű (A8 - A11) különleges számítási intenzív konfigurációk alkalmas nagy teljesítményű számítási fürt alkalmazások.
* D-sorozat VMs, amely nagyobb számítási és ideiglenes lemez teljesítmény igény alkalmazások futtatásához készültek. D-sorozat VMs a gyorsabb processzor, magasabb memória-core arány és félvezető meghajtó (SSD) az ideiglenes lemez szükséges.
* Dv2-sorozat, a D sorozat legújabb verziója, egy nagyobb teljesítményű Processzor szolgáltatásait. A Dv2-sorozat Processzor körülbelül 35 %-kal gyorsabb, mint a D-sorozat Processzor. A legújabb generációs alapul 2.4 GHz-es Intel Xeon® E5-2673 v3 processzor (Haskell), és az Intel Turbo erősítése technológia 2.0-s, válassza a legfeljebb 3,2 GHz-es. A Dv2 sorozat az azonos memóriát, valamint a lemez konfigurációk a D adatsorként tartalmaz.
* G-sorozat VMs ajánlja fel a legtöbb memóriát, és futtassa a hosts Intel Xeon E5 V3 családi processzorokkal.

Megjegyzés: DS-sorozat és GS-sorozat VMs hozzáférése prémium tároló – a SSD biztonsági I/O intenzív munkaterhelésekből nagy teljesítményű, alacsony-késés tárhelyet. Prémium tároló bizonyos régióban érhető el. A részletekért lásd:

- [Prémium tárterület: Azure virtuális gép munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatizálási
Egy megfelelő DevOps kulturális eléréséhez összes infrastruktúra kódot kell lennie.  Amikor összes infrastruktúra abban a kód is egyszerűen lehet az ismételt megadásukra (Phoenix-kiszolgálók).  Azure működik-e a fő automatizálási például Ansible, tölthetők le, SaltStack és Puppet szerszámok.  Azure is rendelkezik saját szerszámok az automatizálási:

- [Azure sablonok](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure megvalósításához [felhő nicializálás](http://cloud-init.io/) támogatása a legtöbb Linux Distros azt támogató keresztül.  Jelenleg Canonical's Ubuntu VMs felhő init alapértelmezés szerint engedélyezve telepítik.  RedHats RHEL, CentOS és Fedora támogatja a felhőben nicializálás, azonban a az Azure képek RedHat által fenntartott nincs telepítve felhő init.  Egy RedHat tartozó OS felhő nicializálás használ, egyéni kép kell létrehoznia felhő init telepítve.

- [Azure Linux VMs felhő nicializálás használata](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kvóták
Minden egyes Azure előfizetés munkalapjai alapértelmezett kvóta hatással lehetnek a sok VMs beállítása a projekthez központi helyen. Az aktuális használati előfizetés alapon korlát 20 VMs / régió.  Kvótákat is emelt egy korlát növelése kérő támogatási jegy tárolásához.  További információt a kvóta korlátozások:

- [Azure előfizetési szolgáltatás korlátok](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partnerek

A Microsoft szorosan működik-e a rendelkezésre álló képek frissülnek, és az Azure futtatókörnyezet optimalizálva biztosítása érdekében a partnerek.  További információt a partnerek jelölje be a piactér az alábbi lapokon.

- [A felosztott Azure záradékkal Linux](virtual-machines-linux-endorsed-distros.md)

Redhat - [Azure piactér - RedHat vállalati Linux 7.2.](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanonikus - [Azure piactér - Ubuntu kiszolgáló 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure piactér - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure piactér - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS – [Azure piactér - CoreOS (stabil)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure piactér - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami – [Az Azure Bitnami tárban](https://azure.bitnami.com/)

Mesosphere - [Azure piactér - Adatközpont Mesosphere OS Azure a](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure piactér - tároló Azure szolgáltatás Docker méhrajt](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure piactér - CloudBees Jenkins Platform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>A telepítő első Azure a
Azure szüksége van az Azure-fiók, az Azure CLI telepítve és két SSH nyilvános és titkos kulcsok használatának megkezdéséhez.

## <a name="sign-up-for-an-account"></a>A fiókjához regisztráció
Az első lépés a Azure felhő használatával regisztrálni szeretne az Azure-fiók.  Ismerkedés az [Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/) lap megnyitásához.

## <a name="install-the-cli"></a>A CLI telepítése
Az új Azure fiókhoz használatának megkezdéséhez azonnal használata a webes felügyeleti panelen az Azure portálon.  Használatával a parancssorban az Azure felhő kezeléséhez, telepítse a `azure-cli`.  Telepítse az [Azure CLI ](../xplat-cli-install.md)a Mac vagy Linux számítógépen.

## <a name="create-an-ssh-key-pair"></a>Hozzon létre egy SSH kulcs pár
Most már van egy Azure-fiók, az Azure webes portál és az Azure CLI.  A következő lépésként hozzon létre egy SSH kulcs pár használt SSH Linux be egy jelszót használata nélkül.  [Hozzon létre SSH billentyűi Linux és Mac](virtual-machines-linux-mac-create-ssh-keys.md) jelszó nélküli bejelentkezések és a biztonság erősítése engedélyezése.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>A Microsoft Azure Linux – első lépések
Az Azure-fiók beállítása, az Azure CLI telepítve van, és a létrehozott SSH kulcsok Ön most már készen áll a kezdésre létrehozása az Azure felhőben infrastruktúrát meg.  Az első tevékenység létrehozása VMs néhány.

## <a name="create-a-vm-using-the-cli"></a>Hozzon létre egy virtuális a CLI használatával
A használatával a CLI Linux virtuális létrehozása módszerrel gyorsan egy virtuális üzembe a terminált elhagyása nélkül dolgozik.  A webes portál adhatja meg a szolgáltatás érhető el parancssori jelölő vagy kapcsoló keresztül.  

- [Hozzon létre egy Linux virtuális a CLI használatával](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Hozzon létre egy virtuális a portálon
Egy Linux virtuális létrehozása az Azure web-portálon módja a könnyen mutat, majd kattintson a különböző végig a eléréséhez kattintson a telepítés gombra.  Vagy parancssori kapcsolók helyett tudunk szép webes elrendezés különböző beállításokat és beállítások megtekintése.  Mindent a parancssor kapható is lehetőség a portálon.

- [Hozzon létre egy Linux virtuális a portálon](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Jelentkezzen be SSH használt jelszó nélkül
A virtuális Azure most már fut, és készen áll a jelentkezzen be.  SSH keresztül jelentkezzen be a jelszavak használatával pedig nem biztonságos sok időt vesz igénybe.  SSH billentyűk használata a legbiztonságosabb módon, és jelentkezzen be a leggyorsabban.  Létrehozásakor, Linux virtuális a portálon, illetve a CLI, ha két hitelesítési módszert.  A SSH olyan jelszót válasszon, az Azure beállítja a jelszavak keresztül bejelentkezések engedélyezése a virtuális.  Ha azt választotta, hogy egy nyilvános SSH billentyűvel, a Azure állítja be a csak a via SSH kulcsok bejelentkezések engedélyezni a virtuális, és a jelszó bejelentkezések letiltja. Biztonságos a Linux virtuális azáltal, hogy csak SSH bejelentkezések használja a SSH nyilvános kulcs lehetőséget a portálon vagy CLI virtuális létrehozása során.

- [Tiltsa le a Linux virtuális SSH jelszavak SSHD konfigurálásával](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Kapcsolódó Azure-összetevők

## <a name="storage"></a>Tárhely

- [Bevezetés a Microsoft Azure-tárolóhoz](../storage/storage-introduction.md)

- [Lemezen hozzáadása egy Linux virtuális az azure cli használatával](virtual-machines-linux-add-disk.md)

- [Hogyan csatolhat adatok lemezen egy Linux virtuális az Azure-portálon](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Hálózati

- [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)

- [IP-címek Azure-ban](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Az Azure-ban egy Linux virtuális portok megnyitása](virtual-machines-linux-nsg-quickstart.md)

- [Teljesen minősített tartománynév létrehozása az Azure-portálon](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Tárolók

- [Virtuális gépeken futó és tárolók Azure-ban](virtual-machines-linux-containers.md)

- [Azure tároló szolgáltatás – bevezetés](../container-service/container-service-intro.md)

- [Az Azure-tároló szolgáltatás fürt terjesztése](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Következő lépések

Most már a Azure Linux áttekintése.  A következő lépés kördiagramjaira, és hozza létre a néhány VMs.

- [Hozzon létre egy Linux virtuális Azure a portálon](virtual-machines-linux-quick-create-portal.md)

- [Létrehozhat egy Linux virtuális Azure a CLI használatával](virtual-machines-linux-quick-create-cli.md)
